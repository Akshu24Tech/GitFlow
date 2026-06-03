<div align="center">

# ⬡ GitFlow

### Understand any GitHub repository in seconds

**Paste a repo URL. Pick your LLM. Get an interactive flowchart.**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Zero Backend](https://img.shields.io/badge/backend-none-brightgreen)](.)
[![Single File](https://img.shields.io/badge/size-single%20HTML-blue)](.)

[**Try it Now →**](./gitflow.html) · [Report Bug](../../issues) · [Request Feature](../../issues)

</div>

---

![Screenshot](screenshot%20(2).png)

## What is GitFlow?

GitFlow turns any GitHub repository into an **interactive execution flowchart** — showing you how the project actually works, step by step, not just what files it contains.

```
Paste GitHub URL  →  GitFlow reads key files  →  LLM analyzes execution flow  →  Interactive Mermaid flowchart
```

Works for any language: Python, JavaScript, TypeScript, Go, Java, Rust, and more.

---

## Features

- **Execution flowchart** — not a file tree. Shows entry points, processing steps, decision points, data flow
- **Multi-provider LLM support** — use whatever API key you already have (see below)
- **3-tab output** — Flowchart · Summary · Raw Mermaid code
- **Smart file prioritization** — automatically finds entry points, pipelines, agents, main files
- **Export** — download the diagram as SVG or copy Mermaid code
- **Zero backend** — runs 100% in your browser, no server, no data collection
- **Private repos** — add a GitHub token and analyze anything

---

## Supported LLM Providers

| Provider | Free Tier | Key Format | Best Model |
|---|---|---|---|
| **Groq** ⚡ | ✅ Yes | `gsk_...` | Llama 3.3 70B |
| **OpenAI** | ❌ Paid | `sk-...` | GPT-4o |
| **Gemini** | ✅ Yes (generous) | `AIza...` | Gemini 2.5 Pro |
| **Anthropic** | ❌ Paid | `sk-ant-...` | Claude Sonnet |
| **OpenRouter** | ✅ Some free models | `sk-or-...` | Any model |
| **Ollama** 🏠 | ✅ Free (local) | No key needed | Llama 3.3, Qwen 3 |

> **Recommended for free usage:** Groq (fastest) or Gemini (most generous free tier)

---

## Quick Start

### Option 1: Just open the file
```bash
git clone https://github.com/yourusername/gitflow
open gitflow.html   # or double-click it
```

No build step. No npm install. No server. Just open in any browser.

### Option 2: Use GitHub Pages / Vercel
Drop `gitflow.html` into any static host and share the URL with your team.

---

## How to Get API Keys

### Groq (Recommended — free, fast)
1. Go to [console.groq.com](https://console.groq.com)
2. Sign up (free, no credit card)
3. API Keys → Create API Key
4. Paste in GitFlow as `gsk_...`

### Gemini (Free generous quota)
1. Go to [aistudio.google.com](https://aistudio.google.com)
2. Get API Key
3. Paste in GitFlow as `AIza...`

### OpenAI
1. Go to [platform.openai.com](https://platform.openai.com/api-keys)
2. Create new secret key
3. Paste as `sk-...`

### Anthropic
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. API Keys → Create Key
3. Paste as `sk-ant-...`

### OpenRouter (access 100+ models)
1. Go to [openrouter.ai](https://openrouter.ai/keys)
2. Create API key
3. Paste as `sk-or-...`
4. Some models are free (check the model list)

### Ollama (fully local, no key)
1. Install [Ollama](https://ollama.ai)
2. Pull a model: `ollama pull llama3.3`
3. In GitFlow, select **Ollama**, set base URL to `http://localhost:11434`
4. No API key needed

---

## Usage

### Public Repositories
```
user/repo
https://github.com/user/repo
```

### Private Repositories
1. Create a [GitHub Personal Access Token](https://github.com/settings/tokens) with `repo` scope
2. Paste it in the **GitHub Token** field (optional)

### Reading the Output

**Flowchart tab** — Interactive Mermaid diagram. Rectangles = processes, Diamonds = decisions, Subgraphs = logical groups.

**Summary tab** — Plain-English project description, numbered execution steps, detected tech stack.

**Mermaid Code tab** — Raw diagram code. Copy it into [mermaid.live](https://mermaid.live) for further editing.

---

## How It Works

```
1. Parse GitHub URL  →  extract owner/repo
2. Fetch repo tree   →  GitHub API (no token = 60 req/hr, with token = 5000 req/hr)
3. Score files       →  prioritize entry points, pipelines, agents, main files (up to 18 files)
4. Fetch contents    →  read top files (3000 chars each)
5. Send to LLM       →  structured prompt forces valid Mermaid output
6. Sanitize output   →  fix common LLM Mermaid mistakes before rendering
7. Render            →  Mermaid.js with 3-tier fallback rendering
8. Summary call      →  second LLM call returns JSON: description + steps + tech stack
```

---

## Architecture

```
gitflow.html   (single file, ~1400 lines)
│
├── Provider Layer       callLLM() → routes to correct provider
│   ├── callOpenAICompat()    Groq / OpenAI / OpenRouter
│   ├── callGemini()          Google Gemini
│   ├── callAnthropic()       Anthropic Claude
│   └── callOllama()          Local Ollama
│
├── GitHub Layer
│   ├── getTree()             fetch recursive file tree
│   ├── prioritizeFiles()     score & rank key files
│   └── fetchFileContent()    base64 decode file contents
│
├── Mermaid Layer
│   ├── sanitizeMermaid()     clean LLM output before render
│   ├── renderMermaid()       mermaid.render() with 3-tier fallback
│   └── extractMermaid()      parse code block from LLM response
│
└── UI
    ├── Provider pills        switch provider dynamically
    ├── Terminal log          real-time progress
    └── 3-tab result          Flowchart · Summary · Raw
```

**Zero build dependencies.** CDN-loaded:
- [Mermaid.js 10](https://mermaid.js.org) — diagram rendering
- [Syne + Space Mono](https://fonts.google.com) — typography

---

## Tips

**For best results:**
- Use **Llama 3.3 70B on Groq** — fastest + highest quality for free
- Add a **GitHub token** to avoid rate limits on large repos
- For complex repos (LangGraph, multi-agent), **Gemini 2.5 Pro** or **GPT-4o** gives better flow understanding

**Supported languages:** Python, JavaScript, TypeScript, Go, Java, Rust, Ruby, PHP, C/C++, C#, Swift, Kotlin, and more.

---

## Limitations

- Analysis is based on static code reading — dynamic imports and runtime behavior may not be captured
- Very large repos (1000+ files) will only analyze the top ~18 most relevant files
- Mermaid diagrams have a max complexity — ultra-complex architectures get simplified
- GitHub API rate limit: 60 requests/hour without token, 5000/hour with token

---

## Contributing

1. Fork the repo
2. All code is in `gitflow.html` — edit and test by opening in browser
3. Submit a PR

**Ideas welcome:**
- [ ] Add more provider support (Cohere, Mistral direct, etc.)
- [ ] Support GitLab / Bitbucket URLs
- [ ] Export as PNG
- [ ] Save/load previous analyses
- [ ] Custom prompt templates per project type


---

<div align="center">

Built for developers who hate reading codebases cold.

*Stop guessing. Start seeing.*

</div>
