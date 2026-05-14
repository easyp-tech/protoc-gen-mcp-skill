# Python MCP Server — Dataclass Handler Mode

Default handler mode (`python_handler=dataclass`). Implement against generated dataclasses from `*_mcp.py`.

## Proto Definition

```proto
syntax = "proto3";

package myapi.v1;

import "mcp/options/v1/options.proto";
import "google/protobuf/empty.proto";

service MyServiceAPI {
  option (mcp.options.v1.service) = {
    namespace: "myapi"
    description: "My tools exposed as MCP tools."
  };

  rpc CreateItem(CreateItemRequest) returns (CreateItemResponse) {
    option (mcp.options.v1.method) = {
      title: "Create item"
      description: "Create a new item with validation."
      annotations: { read_only_hint: false }
    };
  }

  rpc Health(google.protobuf.Empty) returns (HealthResponse) {
    option (mcp.options.v1.method) = {
      title: "Health check"
      description: "Verify the server is alive."
      annotations: { read_only_hint: true }
    };
  }
}

message CreateItemRequest {
  string name = 1 [(mcp.options.v1.field) = {
    description: "Item name."
    examples: [{ string_value: "Widget" }]
    min_length: 1
    max_length: 200
  }];

  int32 count = 2 [(mcp.options.v1.field) = {
    default_value: { integer_value: 1 }
    minimum: 1
    maximum: 1000
  }];

  repeated string tags = 3 [(mcp.options.v1.field) = {
    max_items: 20
    unique_items: true
  }];

  optional string note = 4;
}

message CreateItemResponse {
  string id = 1;
}

message HealthResponse {
  string status = 1;
}
```

## easyp.yaml

```yaml
deps:
  - github.com/easyp-tech/protoc-gen-mcp@v0.5.0

lint:
  use:
    - PACKAGE_DEFINED
    - PACKAGE_LOWER_SNAKE_CASE
    - PACKAGE_VERSION_SUFFIX
    - RPC_NO_CLIENT_STREAMING
    - RPC_NO_SERVER_STREAMING

generate:
  inputs:
    - directory:
        path: proto
        root: "."
  plugins:
    - name: python
      with_imports: true
      out: .
    - command: ["go", "run", "github.com/easyp-tech/protoc-gen-mcp/cmd/protoc-gen-mcp@v0.5.0"]
      out: .
      opts:
        paths: source_relative
        lang: python
        python_runtime: google.protobuf
        # python_handler: dataclass  (default — can be omitted)
```

## Generated Files

| File | Description |
|---|---|
| `myapi_pb2.py` | Standard protobuf Python types |
| `myapi_mcp.py` | Dataclass sidecar — `CreateItemRequest`, `CreateItemResponse`, `register_my_service_api_tools()` |
| `mcp/__init__.py` | Bridge so `mcp.options.*` coexists with the official `mcp` SDK package |

## Handler Implementation

```python
from __future__ import annotations

import anyio
import mcp.server.lowlevel
import mcp.server.stdio

from proto import myapi_mcp


class MyServiceAPI:
    def create_item(
        self,
        _ctx: myapi_mcp.ToolRequestContext,
        req: myapi_mcp.CreateItemRequest,
    ) -> myapi_mcp.CreateItemResponse:
        return myapi_mcp.CreateItemResponse(id="item-1")

    def health(
        self,
        _ctx: myapi_mcp.ToolRequestContext,
        _req: myapi_mcp.HealthRequest,
    ) -> myapi_mcp.HealthResponse:
        return myapi_mcp.HealthResponse(status="ok")


def new_server() -> mcp.server.lowlevel.Server:
    server = mcp.server.lowlevel.Server("myapi-mcp", version="0.1.0")
    myapi_mcp.register_my_service_api_tools(server, MyServiceAPI())
    return server


async def run_stdio_server() -> None:
    server = new_server()
    async with mcp.server.stdio.stdio_server() as (read_stream, write_stream):
        await server.run(
            read_stream,
            write_stream,
            server.create_initialization_options(),
        )


def main() -> None:
    anyio.run(run_stdio_server)


if __name__ == "__main__":
    main()
```

## Key Notes

- For optional fields and `oneof` groups, the absence sentinel is `UNSET`, not `None`
- Generated dataclasses provide explicit `oneof` wrapper types (e.g., `MyRequestLocationCityVariant`)
- Runtime maps MCP JSON → ProtoJSON → `pb2` → dataclasses before calling your handler
- Response dataclass is mapped back through protobuf serialization and output-schema validation
- Enable `with_imports: true` on the Python plugin so proto imports resolve at runtime

## Run

```bash
python server.py
```

Generated tool names: `myapi_CreateItem`, `myapi_Health`.
