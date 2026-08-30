+++
title = "mcp-snapotter"
date = "2025-08-29"
description = "MCP server for SnapOtter's 243 file-processing tools — images, video, audio, PDFs, and more."
+++

[mcp-snapotter](https://github.com/highb/mcp-snapotter) is an
[MCP](https://modelcontextprotocol.io) server for
[SnapOtter](https://github.com/snapotter-hq/SnapOtter), a self-hosted file-processing
platform. It exposes SnapOtter's 243 tools to any MCP-compatible client (Claude Code,
Claude Desktop, etc.) through a clean, discoverable interface.

Instead of flooding the model's context with 243 near-identical tools, it exposes eight
generic ones and indexes the OpenAPI spec for discovery:

| Tool | Purpose |
|---|---|
| `snapotter_list_tools` | Browse/search the catalog by section or keyword |
| `snapotter_describe_tool` | Show a tool's documented settings fields |
| `snapotter_run_tool` | Run one tool on a file; awaits async jobs |
| `snapotter_run_pipeline` | Chain steps server-side in one pass |
| `snapotter_batch` | One tool over many files |
| `snapotter_upload_file` | Add a local file to the library |
| `snapotter_list_files` | List the SnapOtter library |
| `snapotter_get_file` | One file's metadata and version history |
| `snapotter_download` | Fetch a downloadUrl to disk |
| `snapotter_health` | Connectivity check |

Settings are discovered at call time from SnapOtter's validation errors, so the
server doesn't need to hand-model 243 settings schemas. Async tools (54 of them)
are handled transparently with SSE-based progress polling.

Written in Python with strict mypy and ruff, MIT licensed.