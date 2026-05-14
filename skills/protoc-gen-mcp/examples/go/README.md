# Go MCP Server Example

## Proto Definition

```proto
syntax = "proto3";

package myapi.v1;

option go_package = "github.com/you/myproject/myapi/v1;myapiv1";

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
    - PACKAGE_VERSION_SUFFIX
    - RPC_NO_CLIENT_STREAMING
    - RPC_NO_SERVER_STREAMING

generate:
  inputs:
    - directory:
        path: proto
        root: "."
  plugins:
    - name: go
      out: .
      opts:
        paths: source_relative
    - command: ["go", "run", "github.com/easyp-tech/protoc-gen-mcp/cmd/protoc-gen-mcp@v0.5.0"]
      out: .
      opts:
        paths: source_relative
```

## Generated Files

| File | Description |
|---|---|
| `myapi.pb.go` | Standard protobuf Go types |
| `myapi.mcp.go` | MCP handler interface + registration helper |

## Handler Implementation

```go
package main

import (
	"context"
	"log"

	myapiv1 "github.com/you/myproject/myapi/v1"
	"github.com/modelcontextprotocol/go-sdk/mcp"
	emptypb "google.golang.org/protobuf/types/known/emptypb"
)

type handler struct{}

func (handler) CreateItem(
	_ context.Context,
	req *myapiv1.CreateItemRequest,
) (*myapiv1.CreateItemResponse, error) {
	return &myapiv1.CreateItemResponse{Id: "item-1"}, nil
}

func (handler) Health(
	_ context.Context,
	_ *emptypb.Empty,
) (*myapiv1.HealthResponse, error) {
	return &myapiv1.HealthResponse{Status: "ok"}, nil
}

func main() {
	server := mcp.NewServer(&mcp.Implementation{
		Name:    "myapi-mcp",
		Version: "v0.1.0",
	}, nil)

	if err := myapiv1.RegisterMyServiceAPITools(server, handler{}); err != nil {
		log.Fatal(err)
	}

	if err := server.Run(context.Background(), &mcp.StdioTransport{}); err != nil {
		log.Fatal(err)
	}
}
```

## Namespace Override at Registration

```go
myapiv1.RegisterMyServiceAPITools(server, handler{},
	mcpruntime.WithNamespace("custom_prefix"),
)
```

## Run

```bash
go run ./cmd/myserver
```

Generated tool names: `myapi_CreateItem`, `myapi_Health`.
