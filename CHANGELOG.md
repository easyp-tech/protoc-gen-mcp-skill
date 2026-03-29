# Changelog

All notable changes to this skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.0] - 2026-03-30

### Fixed

- Fixed `deps` syntax in `easyp.yaml` example: changed from `repository:` map format to plain string format expected by easyp.
- Updated pinned version references from `@v0.1.0` to `@v0.3.1` to match the latest `protoc-gen-mcp` release.

## [0.2.1] - 2026-03-29

### Fixed

- Added required `deps` block for `easyp.yaml` in the instructions.
- Added `easyp mod download` command to ensure `mcp/options/v1` dependencies are resolved.

## [0.2.0] - 2026-03-29

### Fixed

- Removed `.agents/` folder from git tracking properly.

## [0.1.0] - 2026-03-29

### Added

- Initial release of the `protoc-gen-mcp` skill.
- Added comprehensive documentation on `.proto` definitions, `mcp.options.v1` usage.
- Added step-by-step workflow for configuring `easyp.yaml`.
- Added generation scripts for `*.pb.go` and `*.mcp.go`.
- Added reference guides for options and schema mapping.
