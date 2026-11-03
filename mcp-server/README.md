# Exercise 1 · MCP server

This server speaks a minimal JSON-RPC 2.0 protocol over stdio and exposes two MCP tools: `find_lecture` and `list_lectures`.

## How to start it

```bash
npm run --silent mcp
```

## Session transcript

```json
{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-11-25","capabilities":{},"clientInfo":{"name":"me","version":"1.0.0"}}}
{"jsonrpc":"2.0","id":1,"result":{"protocolVersion":"2025-11-25","capabilities":{"tools":{}},"serverInfo":{"name":"lectures","version":"0.1.0"}}}
{"jsonrpc":"2.0","method":"notifications/initialized"}
{"jsonrpc":"2.0","id":2,"method":"tools/list"}
{"jsonrpc":"2.0","id":2,"result":{"tools":[{"name":"find_lecture","description":"Return the title and topics of one course lecture by its number, 1 to 12. Use it when asked what a lecture covers.","inputSchema":{"type":"object","properties":{"number":{"type":"integer","description":"Lecture number, 1 to 12"}},"required":["number"]}},{"name":"list_lectures","description":"List the number and title of every course lecture. Use it when asked what the course covers.","inputSchema":{"type":"object","properties":{}}}]}}
{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"find_lecture","arguments":{"number":5}}}
{"jsonrpc":"2.0","id":3,"result":{"content":[{"type":"text","text":"Lecture 5: What the agent built, part 1. Topics: HTTP, REST vs GraphQL, data modeling, ORM, layers"}],"isError":false}}
{"jsonrpc":"2.0","id":4,"method":"tools/call","params":{"name":"find_lecture","arguments":{"number":99}}}
{"jsonrpc":"2.0","id":4,"result":{"content":[{"type":"text","text":"There is no lecture 99. The course has lectures 1 to 12."}],"isError":true}}
```

The server logs each incoming JSON message to stderr and writes only protocol responses to stdout, so the host reads clean JSON lines from the standard output stream.

## The description experiment

I changed the `find_lecture` description from a vague summary to a concrete instruction that names the input, the output, and the situation where it should be used. The model should choose it more reliably because the wording tells it what the tool returns and when the tool is relevant. The key idea is that a tool description is effectively the model’s decision input, so a precise description improves selection even when the code is unchanged.
