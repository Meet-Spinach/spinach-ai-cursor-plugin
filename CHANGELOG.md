# Changelog

## 1.0.0

- Declares `"type": "http"` and `"placement": "server"` on the MCP server so Grok Bot dials it through the hosted MCP path. Without `placement` Grok Bot never shows the Spinach connect / sign-in card.
- Sets `minClientVersions.cursor` to `3.13.0`, `category` to `integrations`, and fixes the `repository` URL.
- Logo: replaces the padded 1024px macOS app icon SVG with a 192×192 `assets/logo.png` tile.
- README: marketplace install steps, current tool list (blockers, video, screenshots), and corrected compliance claims.

## 0.1.0

- Initial release.
- Wires up the Spinach remote MCP server (`https://mcp.spinach.ai/mcp`).
- Skills: `find-meeting-context`, `get-action-items`, `summarize-recent-meetings`, `follow-live-meeting`.
- Rule: `consult-meeting-context`.
