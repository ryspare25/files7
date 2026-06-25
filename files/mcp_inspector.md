# MCP Inspector

## What It Is

A debugging UI for Model Context Protocol (MCP) servers. The MCP equivalent of Postman for REST APIs — introspect resources, call tools, test prompts, without writing any client code.

## Interface Panels

- **Left:** Connection config. Transport type (STDIO/SSE), command, arguments, environment variables. Green dot = server live.
- **Center tabs:** The three MCP primitives — Resources, Resource Templates, Tools — plus Prompts, Ping, Sampling, Roots.
- **Right:** Live wire-format response from the last clicked resource/tool.
- **Bottom left:** Request history (chronological). Bottom right: Server Notifications stream.

## Typical Request Lifecycle (from history)

1. `initialize` — handshake, capability negotiation
2. `resources/list` — enumerate available resources
3. `resources/templates/list` — enumerate URI templates
4. `resources/read` — fetch a specific resource by URI

## Example Response Structure

Clicking `docs://documents` returns:

```json
{
  "contents": [
    {
      "uri": "docs://documents",
      "mimeType": "application/json",
      "text": "[\"deposition.md\", \"report.pdf\", \"financials.docx\", \"outlook.pdf\", \"plan.md\", \"spec.txt\"]"
    }
  ]
}
```

## Use Case Pattern

The server in the screenshot is a document-access MCP:
- `docs://documents` resource → returns file listing
- `fetch_doc` template → retrieves individual files by name

This pattern (list + fetch) is the standard resource exposure idiom in MCP servers that wrap a filesystem or document store.

## Key Point

The inspector shows the **exact response structure** the MCP client (e.g. Claude) will receive at runtime — making it the authoritative tool for verifying server behavior before wiring it into an agent.
