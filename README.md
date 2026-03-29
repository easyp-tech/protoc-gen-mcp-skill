# protoc-gen-mcp-skill

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

An intelligent agent skill to help AI agents like Cursor, Copilot, and Claude Code write, generate, and manage
Model Context Protocol (MCP) servers using `protoc-gen-mcp` and `easyp`. 

This skill teaches agents to use `.proto` files as the single source of truth for generating type-safe Go MCP tools, ensuring strict schemas.

## Installation

You can install this skill globally for your agent (e.g., Cursor, Claude Code) using the Vercel `skills` CLI.

First, ensure you have the `skills` CLI available via `npx`:

```bash
npx skills add easyp-tech/protoc-gen-mcp-skill -g
```

> **Note:** Replace `easyp-tech` with the correct GitHub username/organization if using a fork.

### Local Project Installation

To install this skill specifically for a single project (rather than globally):

```bash
npx skills add easyp-tech/protoc-gen-mcp-skill
```

## What This Skill Does

By installing this skill, your AI agent will gain expert-level understanding of:
- **Defining MCP Tools in Protobuf:** Annotating RPC methods, types, and fields using `mcp.options.v1`.
- **Generating Go Bindings:** Configuring `easyp.yaml` to generate `*.pb.go` and `*.mcp.go`.
- **Implementing Handlers:** Writing the generated Go server interfaces (`<Service>ToolHandler`).
- **MCP Constraints & Mappings:** Knowing precisely how Protobuf maps to JSON Schema validation for MCP clients.

## Development & Usage

The core instructions are located in `skills/protoc-gen-mcp/SKILL.md`. Agents will automatically parse these instructions and apply them when you ask tasks like:

* "Generate an MCP tool from my exist proto file."
* "What is the correct way to add a required field constraint to an MCP tool?"
* "Setup easyp to generate protoc-gen-mcp bindings."

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
