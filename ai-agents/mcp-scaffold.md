---
name: mcp-scaffold
description: Scaffold an MCP server stub with tool definitions and handler structure for a described capability
---

You are scaffolding a Model Context Protocol (MCP) server. The user will describe what capability the server should expose. Generate a complete, runnable stub.

**MCP SERVER ANATOMY**

An MCP server exposes tools that Claude can call. The server:
1. Declares available tools (name, description, input schema)
2. Handles `tools/call` requests and returns results
3. Runs as a local process Claude connects to via stdio or HTTP

**GENERATE THESE FILES**

```
mcp-[server-name]/
├── server.py          # Main MCP server
├── tools/
│   ├── __init__.py
│   └── [tool_name].py  # One file per logical tool group
├── requirements.txt
└── README.md
```

**server.py TEMPLATE**

```python
#!/usr/bin/env python3
"""MCP server for [capability description]."""
import json
import sys
from typing import Any

def get_tools() -> list[dict]:
    """Declare all available tools."""
    return [
        {
            "name": "tool_name",
            "description": "What this tool does. When to use it. What it returns.",
            "inputSchema": {
                "type": "object",
                "properties": {
                    "param": {
                        "type": "string",
                        "description": "What this param is, valid values, example"
                    }
                },
                "required": ["param"]
            }
        },
        # Add more tools...
    ]

def handle_tool_call(name: str, arguments: dict[str, Any]) -> Any:
    """Route tool calls to handlers."""
    handlers = {
        "tool_name": handle_tool_name,
    }
    handler = handlers.get(name)
    if not handler:
        return {"error": f"Unknown tool: {name}"}
    return handler(**arguments)

def handle_tool_name(param: str) -> dict:
    """Handler for tool_name."""
    # TODO: implement
    return {"result": f"Processed: {param}"}

def main():
    """MCP server main loop — reads JSON-RPC from stdin, writes to stdout."""
    for line in sys.stdin:
        try:
            request = json.loads(line.strip())
            method = request.get("method")
            req_id = request.get("id")

            if method == "tools/list":
                response = {"id": req_id, "result": {"tools": get_tools()}}
            elif method == "tools/call":
                params = request.get("params", {})
                result = handle_tool_call(params["name"], params.get("arguments", {}))
                response = {"id": req_id, "result": {"content": [{"type": "text", "text": json.dumps(result)}]}}
            else:
                response = {"id": req_id, "error": {"message": f"Unknown method: {method}"}}

            print(json.dumps(response), flush=True)
        except Exception as e:
            print(json.dumps({"error": str(e)}), flush=True)

if __name__ == "__main__":
    main()
```

**CLAUDE DESKTOP CONFIG** (add to `claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "[server-name]": {
      "command": "python3",
      "args": ["/path/to/mcp-[server-name]/server.py"]
    }
  }
}
```

Generate the complete scaffold with real tool names and descriptions based on the user's capability description. Fill in the `handle_*` functions with working stub logic (not just `pass`). Include a `requirements.txt` and a 10-line `README.md`.
