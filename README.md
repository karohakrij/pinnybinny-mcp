<p align="center">
  <img src="logo.png" width="128" alt="PinnyBinny">
</p>

# PinnyBinny MCP Server

Create classroom activities from a single prompt.

> "Create a vocabulary quiz about planets for 5th grade."  
> → Preview → Approve → **Board URL**

PinnyBinny is a privacy-first classroom activity platform. This MCP server lets you create flashcard sets, quizzes, memory games, and sorting boards directly from your AI client — with a mandatory preview step before anything is saved.

<!-- Add screenshot here: teacher prompt → AI preview → board URL -->

---

## How it works

```
Teacher prompt
      │
      ▼
AI Client (Claude / Cursor / ChatGPT)
      │
      ▼  tools/call  preview: true
PinnyBinny MCP Server
      │
      ▼
Preview — teacher sees the content

Teacher approves
      │
      ▼  tools/call
PinnyBinny MCP Server
      │
      ▼
Board created ──▶  pinnybinny.com/board/…
```

**Teacher is always in control.** AI never saves anything without explicit approval.

---

## Available tools

| Tool | Creates | Limit |
|------|---------|-------|
| `create_flashcards` | Flashcard sets (question / answer) | 1–100 cards |
| `create_quiz` | Multiple-choice quizzes | 1–50 questions |
| `create_pexeso` | Memory matching games | 1–50 pairs |
| `create_columns` | Sorting / categorization boards | 1–10 columns |

All tools support `"preview": true` — validates and returns a formatted preview without saving anything to the database.

---

## Setup

### 1. Get a token

MCP tokens are available for paid PinnyBinny plans.

1. Log in to [pinnybinny.com](https://pinnybinny.com)
2. Go to **Settings → AI / MCP integrations**
3. Click **Generate new token**

### 2. Add to your AI client

**Claude Desktop** — add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "pinnybinny": {
      "url": "https://b.pinnybinny.com/api/mcp/v1",
      "headers": {
        "Authorization": "Bearer pb_your_token_here"
      }
    }
  }
}
```

See [`examples/claude_desktop_config.json`](examples/claude_desktop_config.json) for the full example.

**Cursor** — Settings → MCP / Tools → add remote server:
- URL: `https://b.pinnybinny.com/api/mcp/v1`
- Header: `Authorization: Bearer pb_your_token_here`

**Claude.ai (web)** — Settings → Integrations → add remote MCP server *(availability depends on region)*

**ChatGPT** — Settings → Connected apps → add server

---

## Availability

MCP access requires an active paid PinnyBinny plan.

Free accounts can still use AI Import through the web interface by copying and pasting generated JSON.

See [pricing plans](https://pinnybinny.com/#pricing) for current plan details.

---

## Example prompts

```
Create a flashcard set about Spanish food vocabulary for beginners. 15 cards.
```

```
Create a multiple-choice quiz about photosynthesis for 7th grade, 10 questions.
```

```
Create a memory matching game with chemical formulas and their names. 12 pairs.
```

```
Create a sorting board: advantages and disadvantages of social media. 2 columns.
```

---

## Why preview-first?

Most AI tools create content automatically. PinnyBinny doesn't.

Every tool call goes through a two-step flow:

1. **`preview: true`** — AI proposes content. Nothing is saved. Teacher sees exactly what will be created.
2. **Approve** — Teacher confirms. Board is created. URL is returned.

This matters in education:

- AI sometimes generates inaccurate or off-topic content
- Teachers are responsible for what students see
- "AI-assisted" should mean AI helps, not AI decides

**PinnyBinny is not an autonomous AI agent.** It's a tool that makes teacher workflows faster — with the teacher always in the loop.

---

## Privacy

**No student data is processed by AI.**

PinnyBinny AI workflows are teacher-side only:

- AI helps teachers *create* activities
- Students interact with boards through PinnyBinny — not through any AI
- No student responses are sent to AI models
- No student profiling, grading, or analysis by AI
- No surveillance features

The MCP server only creates boards. It does not read student activity, analyze responses, or generate reports.

---

## Schemas

The data format for each activity type is defined in [`schemas/`](schemas/):

| File | Used for |
|------|----------|
| [`flashcards.schema.json`](schemas/flashcards.schema.json) | Flashcard sets |
| [`quiz.schema.json`](schemas/quiz.schema.json) | Multiple-choice quizzes |
| [`pexeso.schema.json`](schemas/pexeso.schema.json) | Memory matching games |
| [`columns.schema.json`](schemas/columns.schema.json) | Sorting boards |

JSON Schema draft-07. These schemas are the single source of truth for MCP validation, AI import, and prompting.

Sample data files: [`examples/`](examples/)

---

## Protocol

PinnyBinny implements **MCP over Streamable HTTP** (JSON-RPC 2.0).  
One POST request → one response. No WebSockets, no SSE, no persistent connections.

**Server URL:** `https://b.pinnybinny.com/api/mcp/v1`  
**Auth:** `Authorization: Bearer pb_<64 hex chars>`  
**Rate limit:** 60 requests / minute per token

---

## Quick test (curl)

```bash
# List available tools
curl -X POST https://b.pinnybinny.com/api/mcp/v1 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer pb_your_token_here" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'

# Preview a flashcard set (nothing saved to DB)
curl -X POST https://b.pinnybinny.com/api/mcp/v1 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer pb_your_token_here" \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"create_flashcards","arguments":{"title":"Test","cards":[{"question":"What is H2O?","answer":"Water"}],"preview":true}}}'
```

---

## Security

To report a security vulnerability, see [SECURITY.md](SECURITY.md).  
Please do not use public GitHub issues for security reports.

---

## License

[MIT](LICENSE)
