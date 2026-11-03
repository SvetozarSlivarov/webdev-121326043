# Agent log

One entry per agent session, newest at the bottom. Be honest: what went wrong is the useful part.

## 2026-11-03 · Implement Exercise 1
- Goal: Build the MCP server and make the exercise self-check pass.
- Delegated: Asked the agent to explain the initialize handshake, notification semantics, and the difference between tool failures and protocol errors.
- Checkpoints: `node mcp-server/check.mjs -- node mcp-server/server.js` and `node mcp-server/check.mjs -- npm run --silent mcp`.
- Went wrong: The first pass needed the protocol to be aligned with the real MCP messages, especially the `tools/list` schema and the distinction between `isError: true` and JSON-RPC errors.
- Changed by hand: Wrote the stdio JSON-RPC loop, the handlers for initialize/notifications/initialized/tools/list/tools/call, and the error responses for unknown tool and unknown method; then verified the start command and the protocol output.
