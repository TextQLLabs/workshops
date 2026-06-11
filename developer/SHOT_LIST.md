# Screenshot shot list

Drop PNGs into `assets/` with these exact filenames and they appear in the guide automatically
(no HTML edits needed — placeholders show "Screenshot coming soon" until the file exists).

Capture at ~1280-1440px browser width, light mode, demo data only — no customer data.

| # | File | Module | What to capture |
|---|---|---|---|
| 1 | `assets/dev-m0-apikey.png` | M0 | API key creation (Settings → Configuration → API Keys) |
| 2 | `assets/dev-m1-chat.png` | M1 | a terminal with a Chat API request and streamed response |
| 3 | `assets/dev-m3-sandbox.png` | M3 | sandbox API results (warehouse data into a DataFrame) |
| 4 | `assets/dev-m4-mcp.png` | M4 | Ana answering inside Claude/Cursor via MCP |
| 5 | `assets/dev-m5-tools.png` | M5 | MCP tools configuration (external tools Ana can call) |

Adding more: copy a `<figure class="shot">` block in `index.html`, point it at a new
`assets/*.png`, and set `data-todo` to the capture description.
