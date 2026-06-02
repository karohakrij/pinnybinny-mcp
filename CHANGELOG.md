# Changelog

All notable changes to the PinnyBinny MCP server are documented here.

## [1.1.0] — 2026-06

### Added

- OAuth 2.0 support for Claude.ai web connector (Dynamic Client Registration, RFC 7591; PKCE, RFC 8414, RFC 9728)
- Preview cache system: `preview_id` returned on `preview: true` call, server stores and reuses approved content on `preview: false` — prevents AI from regenerating content between preview and creation
- `preview_id` field added to all tool schemas

### Changed

- Content length limits reduced for classroom readability:
  - Flashcard question: 1000 → 180 chars
  - Flashcard answer: 1000 → 80 chars
  - Quiz question: 1000 → 180 chars
  - Quiz answer option: 500 → 80 chars
  - Pexeso card: 500 → 90 chars
- `preview` default changed from `false` to `true` — preview is now opt-out, not opt-in

---

## [1.0.0] — 2026

### Added

- MCP server over Streamable HTTP (JSON-RPC 2.0)
- `create_flashcards` — create flashcard sets (question/answer, 1–100 cards)
- `create_quiz` — create multiple-choice quizzes (1–50 questions, 4 answers each)
- `create_pexeso` — create memory matching games (1–50 pairs)
- `create_columns` — create sorting/categorization boards (1–10 columns)
- `preview: true` support on all tools — validate and preview without saving
- Bearer token authentication (`pb_` prefix, SHA-256 hash stored)
- Rate limiting: 60 requests/minute per token
- Canonical JSON schemas for all activity types (JSON Schema draft-07)
