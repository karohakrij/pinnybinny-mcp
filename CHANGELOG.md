# Changelog

All notable changes to the PinnyBinny MCP server are documented here.

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
