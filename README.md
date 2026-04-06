# Obsidian RAG

[한국어](README.ko.md) | English

No vector database. No embeddings. No code.  
Just Obsidian + Claude Code = your personal knowledge base with AI librarian.

> Inspired by [Andrej Karpathy's approach](https://x.com/karpathy) — use structured markdown and an LLM as your librarian instead of traditional RAG infrastructure.

---

## How It Works

```
You clip articles → raw/ folder
        ↓
Claude Code reads & organizes → wiki/ folder  
        ↓
You ask questions → Claude navigates indexes → answers
```

Traditional RAG: `Documents → Embeddings → Vector DB → Similarity Search → LLM`  
Obsidian RAG: `Documents → Markdown → Claude Code reads files directly`

### The Navigation Trick (No Vector DB Needed)

When you ask a question, Claude Code does 3-4 targeted file reads:

1. Reads `_master-index.md` — "What topics exist?"
2. Picks the right topic → reads that topic's `_index.md`
3. Reads 1-3 relevant articles
4. Synthesizes the answer

This scales to hundreds or thousands of documents.

---

## Quick Start

### Prerequisites

- [Obsidian](https://obsidian.md) (free)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (`npm install -g @anthropic-ai/claude-code`)

### Step 1 — Clone This Repo

Clone into a location where Obsidian can access it:

```bash
# Example: clone into your Documents folder
git clone https://github.com/YOUR_USERNAME/obsidian-RAG.git

# Or if you use iCloud-synced Obsidian:
cd ~/Library/Mobile\ Documents/iCloud~md~obsidian/Documents/
git clone https://github.com/YOUR_USERNAME/obsidian-RAG.git
```

### Step 2 — Open as Obsidian Vault

1. Open Obsidian
2. Click **"Open folder as vault"**
3. Select the cloned `obsidian-RAG` folder
4. Done — you'll see the folder structure in the sidebar

### Step 3 — Start Claude Code

```bash
cd /path/to/obsidian-RAG
claude
```

Claude Code automatically reads `CLAUDE.md` and knows it's your knowledge base librarian.

### Step 4 — Try It Out

There are example files in `raw/`. Tell Claude Code:

```
compile
```

Watch it process the raw files into structured wiki articles with indexes and cross-links.

---

## The Four Verbs

| Command | What Happens |
|---------|-------------|
| **Clip** | Save articles to `raw/` (via Web Clipper or manually) |
| **Compile** | Tell Claude Code `compile` — processes `raw/` into `wiki/` |
| **Query** | Ask questions — Claude navigates the wiki to answer |
| **Audit** | Tell Claude Code `audit` — reviews wiki for gaps and issues |

### Compile

```
compile
```

Claude Code will:
1. Read each file in `raw/`
2. Create/find the right topic folder in `wiki/`
3. Write a structured article with key takeaways and [[wiki links]]
4. Update all indexes

### Query

```
What are the key differences between traditional RAG and Obsidian RAG?
```

```
How does [concept A] relate to [concept B] based on the wiki?
```

```
Based on everything in the wiki, summarize the main approaches to [topic].
Save your answer as a new wiki article.
```

That last one is the magic — the answer becomes part of the knowledge base.

### Audit

```
audit
```

Claude Code reviews the entire wiki and reports:
- Inconsistent or contradictory information
- Missing cross-links
- Coverage gaps
- Suggested new articles

---

## Folder Structure

```
obsidian-RAG/
├── CLAUDE.md              # Rules for Claude Code (reads every session)
├── README.md              # This file
├── raw/                   # Your inbox — dump source material here
│   ├── example-*.md       # Sample files to try "compile"
│   └── (your clipped articles)
├── wiki/                  # Claude Code's domain — auto-maintained
│   ├── _master-index.md   # Entry point — lists all topics
│   └── {topic}/           # Topic subfolders (created during compile)
│       ├── _index.md      # Lists all articles in this topic
│       └── *.md           # Individual articles
└── output/                # Query results and reports
```

---

## Optional: Obsidian Web Clipper

For one-click article saving to `raw/`:

1. Install **Obsidian Web Clipper** browser extension
2. Settings → General → Add your vault
3. Templates → Edit Default template:
   - **Vault**: select your vault
   - **Note location**: `raw`
   - **Note name**: `{{date|date:"YYYY-MM-DD"}}-{{title|safe_name}}`

Now any web article → one click → markdown file in `raw/`.

### Optional: Local Images Plus

To auto-download images from clipped articles:

1. Obsidian → Settings → Community Plugins → Browse
2. Search **"Local Images Plus"** → Install → Enable

---

## Obsidian RAG vs Traditional RAG

| | Obsidian RAG | Traditional RAG |
|---|---|---|
| **Infrastructure** | None (just files) | Vector DB + Embedding model + API |
| **Setup time** | 5 minutes | Hours to days |
| **Human readable** | Yes — browse your own wiki | No — vector store is a black box |
| **Self-improving** | Audit finds gaps, queries add knowledge | Static until re-indexed |
| **Best for** | Personal use, research, small teams | Production apps, 5000+ docs, multi-user |
| **Cost** | Claude Code subscription only | DB hosting + embedding API + LLM API |

### When to upgrade to real RAG
- 5,000+ documents where index navigation breaks down
- Need semantic search fallback
- Multi-user or production deployment
- Bulk ingestion of hundreds of documents at once

---

## Tips

- **Daily workflow**: Clip → Compile → Query → Audit
- **Cross-linking**: The more [[wiki links]] between articles, the better Claude navigates
- **Compound knowledge**: Ask synthesis questions and save answers back to wiki — every question makes the system smarter
- **Periodic audit**: Run `audit` weekly to keep quality high

---

## License

Apache-2.0
