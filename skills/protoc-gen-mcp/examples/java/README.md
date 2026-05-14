# Java MCP Server Example

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
    - name: java
      with_imports: true
      out: src/generated/main/java
    - command: ["go", "run", "github.com/easyp-tech/protoc-gen-mcp/cmd/protoc-gen-mcp@v0.5.0"]
      out: src/generated/main/java
      opts:
        paths: source_relative
        lang: java
```

## Generated Files

| File | Description |
|---|---|
| `src/generated/main/java/.../*.java` | Protobuf Java types |
| `src/generated/main/java/.../MyapiMcp.java` | MCP sidecar with nested `<Service>ToolHandler` interface |

## build.gradle.kts

```kotlin
plugins {
    id("java")
    application
}

repositories {
    mavenCentral()
}

dependencies {
    implementation("io.modelcontextprotocol.sdk:mcp:0.10.0")
    implementation("com.google.protobuf:protobuf-java:4.31.1")
}

application {
    mainClass = "com.example.MyServiceServer"
}
```

## Handler Implementation

```java
package com.example;

import io.modelcontextprotocol.sdk.McpServerTransportProvider;
import io.modelcontextprotocol.sdk.McpServers;
import myapi.v1.MyapiMcp;
import myapi.v1.MyapiMcp.MyServiceAPIToolHandler;
import myapi.v1.Myapi.CreateItemRequest;
import myapi.v1.Myapi.CreateItemResponse;
import myapi.v1.Myapi.HealthRequest;
import myapi.v1.Myapi.HealthResponse;
import myapi.v1.Myapi.ToolRequestContext;

public class MyServiceServer implements MyServiceAPIToolHandler {

  @Override
  public CreateItemResponse createItem(ToolRequestContext ctx, CreateItemRequest req) {
    return CreateItemResponse.newBuilder().setId("item-1").build();
  }

  @Override
  public HealthResponse health(ToolRequestContext ctx, HealthRequest req) {
    return HealthResponse.newBuilder().setStatus("ok").build();
  }

  public static void main(String[] args) throws Exception {
    var transportProvider = McpServers.transportProviderBuilder()
        .stdio()
        .build();
    MyapiMcp.registerMyServiceAPITools(transportProvider, new MyServiceServer(), "myapi");
    transportProvider.start().get();
  }
}
```

## Key Notes

- Generated handlers implement nested `<Service>ToolHandler` interfaces inside a `<ProtoFile>Mcp` sidecar
- Registered through `register<Service>Tools(McpServerTransportProvider transportProvider, impl, namespace)`
- User-authored protos do not need a Go `go_package` option — the generator synthesizes internal protogen metadata

## Build and Run

```bash
gradle installDist
./build/install/myserver/bin/myserver
```

Generated tool names: `myapi_CreateItem`, `myapi_Health`.
