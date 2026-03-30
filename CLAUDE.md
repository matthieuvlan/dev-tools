# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A collection of five standalone developer utility tools, each a self-contained HTML file with embedded CSS and JavaScript. No build system, no dependencies, no server — open directly in a browser.

**Tools:**
- `crud-client.html` — HTTP client (GET/POST/PUT/PATCH/DELETE with headers, body, auth)
- `token-tools.html` — JWT decode/encode/verify + SAML decode/encode/format
- `json-tools.html` — JSON formatter with live syntax highlighting + JSON diff
- `regex-tester.html` — Interactive regex tester with flags, match highlighting, and replace
- `encoders.html` — Base64 (text + file) and URL encoder/decoder with URL breakdown

## Development

No installation or build step required. Open any HTML file directly in a browser.

For testing changes: refresh the browser. All state is in-memory only (no localStorage).

## Architecture

All five files share the same pattern:

- **CSS custom properties** at the top of each `<style>` block define the design system (`--bg`, `--surface`, `--accent`, etc.) and JetBrains Mono / DM Sans fonts via Google Fonts CDN.
- **Navigation bar** at the top links all five tools together.
- **Single `$()` helper** is used as a `document.querySelector` shorthand throughout.
- **Event wiring** uses inline `onclick`/`oninput` HTML attributes rather than `addEventListener`.
- **Rendering** uses direct `innerHTML` assignment with an `esc()` helper for HTML-escaping user input to prevent XSS.
- **No async/await except** in `crud-client.html` (Fetch API) and `token-tools.html` (Web Crypto API for JWT sign/verify).

### Browser APIs in use
- `fetch` — HTTP requests (crud-client)
- `crypto.subtle` — HMAC/RSA signing and verification (token-tools)
- `FileReader` — File-to-Base64 encoding (encoders)
- `URL` constructor — URL parsing (encoders)
- `navigator.clipboard` — Copy to clipboard (all tools)

## Conventions

- Each tool is fully self-contained; there is no shared JS or CSS file.
- Toast notifications (`showToast()`) are the standard way to give user feedback.
- Multi-panel tools use CSS Grid with `grid-template-columns`.
- Tab-based tools toggle a `.active` class on both the tab button and its content panel.
- Base64url encoding (used in token-tools) strips `=` padding and replaces `+`→`-`, `/`→`_`.
