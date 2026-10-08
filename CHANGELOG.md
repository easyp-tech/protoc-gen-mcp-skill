# Changelog

All notable changes to this skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.0] - 2026-10-08

### Changed

- Moved every example to EasyP v1:
  - `protobuf.mod` holds `require github.com/easyp-tech/protoc-gen-mcp v0.7.1`;
  - `easyp.yaml` holds the lint policy;
  - `easyp.gen.yaml` holds the plugins.
- Workflow commands: `easyp mod tidy`, then `easyp lint` / `easyp generate` without the removed `-p/-r` flags. Added the `--frozen` CI form.
- Prerequisites install an explicit v1 nightly, and point v0 projects to `easyp migrate` and the protobuf-expert-skill migration guide.
- Pinned `protoc-gen-mcp` to `v0.7.1` (was `v0.5.0`).

 - 2026-03-30

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
