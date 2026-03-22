# Changelog

## [Unreleased]

### Added
- Claude Code plugin with slash commands and admin agent (Phase 4.5)
- 4 Claude Code skills: CMDB, table explorer, update set reviewer, incident triage (Phase 4)

## [0.3.1] - 2026-02-26

### Fixed
- CLI TypeError on stdio transport (`unexpected keyword argument 'host'`)
- server.json description length for MCP Registry (max 100 chars)

### Added
- MCP Registry schema and automated publish workflow
- `.mcp.json.example` and configuration examples in README

### Changed
- Renamed package to `mcp-server-servicenow`

## [0.3.0] - 2026-02-23

### Added
- OAuth 2.1 + PKCE proxy for per-user ServiceNow auth (Phase 3)
- FastMCP 3.0 migration with dual transport (Phase 2)
- Streamable HTTP transport and Cloud Run deployment
- Security model docs and native vs community comparison
- CI workflow, LICENSE, issue templates, integration tests

## [0.1.0] - 2026-02-07

### Added
- 18-tool ServiceNow MCP server (Phase 1)
- Table API, CMDB, System, Update Sets tool modules
- Basic auth, OAuth password grant, API key authentication
- Pydantic configuration, unit tests
