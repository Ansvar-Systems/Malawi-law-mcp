# Malawi Law MCP Server

**The Malawi Law Commission alternative for the AI age.**

[![npm version](https://badge.fury.io/js/@ansvar%2Fmalawi-law-mcp.svg)](https://www.npmjs.com/package/@ansvar/malawi-law-mcp)
[![MCP Registry](https://img.shields.io/badge/MCP-Registry-blue)](https://registry.modelcontextprotocol.io)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GitHub stars](https://img.shields.io/github/stars/Ansvar-Systems/Malawi-law-mcp?style=social)](https://github.com/Ansvar-Systems/Malawi-law-mcp)
[![CI](https://github.com/Ansvar-Systems/Malawi-law-mcp/actions/workflows/ci.yml/badge.svg)](https://github.com/Ansvar-Systems/Malawi-law-mcp/actions/workflows/ci.yml)
[![Daily Data Check](https://github.com/Ansvar-Systems/Malawi-law-mcp/actions/workflows/check-updates.yml/badge.svg)](https://github.com/Ansvar-Systems/Malawi-law-mcp/actions/workflows/check-updates.yml)
[![Database](https://img.shields.io/badge/database-pre--built-green)](https://github.com/Ansvar-Systems/Malawi-law-mcp)
[![Provisions](https://img.shields.io/badge/provisions-14%2C392-blue)](https://github.com/Ansvar-Systems/Malawi-law-mcp)

Query **545 Malawian Acts** -- from the Electronic Transactions and Cybersecurity Act and the Penal Code to the Employment Act, Companies Act, and more -- directly from Claude, Cursor, or any MCP-compatible client.

If you're building legal tech, compliance tools, or doing Malawian legal research, this is your verified reference database.

Built by [Ansvar Systems](https://ansvar.eu) -- Stockholm, Sweden

---

## Why This Exists

Malawian legal research means navigating malawilii.org, the Malawi Law Commission site (lawcom.mw), and parliament.gov.mw -- scattered across multiple sources with inconsistent formatting. Whether you're:

- A **lawyer** validating citations in a brief or contract
- A **compliance officer** checking obligations under the Electronic Transactions and Cybersecurity Act or the Competition and Fair Trading Act
- A **legal tech developer** building tools on Malawian law
- A **researcher** tracing legislative provisions across 545 Acts

...you shouldn't need dozens of browser tabs and manual cross-referencing. Ask Claude. Get the exact provision. With context.

This MCP server makes Malawian law **searchable, cross-referenceable, and AI-readable**.

---

## Quick Start

### Use Remotely (No Install Needed)

> Connect directly to the hosted version -- zero dependencies, nothing to install.

**Endpoint:** `https://malawi-law-mcp.vercel.app/mcp`

| Client | How to Connect |
|--------|---------------|
| **Claude.ai** | Settings > Connectors > Add Integration > paste URL |
| **Claude Code** | `claude mcp add malawi-law --transport http https://malawi-law-mcp.vercel.app/mcp` |
| **Claude Desktop** | Add to config (see below) |
| **GitHub Copilot** | Add to VS Code settings (see below) |

**Claude Desktop** -- add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "malawi-law": {
      "type": "url",
      "url": "https://malawi-law-mcp.vercel.app/mcp"
    }
  }
}
```

**GitHub Copilot** -- add to VS Code `settings.json`:

```json
{
  "github.copilot.chat.mcp.servers": {
    "malawi-law": {
      "type": "http",
      "url": "https://malawi-law-mcp.vercel.app/mcp"
    }
  }
}
```

### Use Locally (npm)

```bash
npx @ansvar/malawi-law-mcp
```

**Claude Desktop** -- add to `claude_desktop_config.json`:

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "malawi-law": {
      "command": "npx",
      "args": ["-y", "@ansvar/malawi-law-mcp"]
    }
  }
}
```

**Cursor / VS Code:**

```json
{
  "mcp.servers": {
    "malawi-law": {
      "command": "npx",
      "args": ["-y", "@ansvar/malawi-law-mcp"]
    }
  }
}
```

---

## Example Queries

Once connected, just ask naturally:

- *"What does the Electronic Transactions and Cybersecurity Act say about data protection?"*
- *"Find provisions in the Penal Code about fraud and forgery"*
- *"Search for labour rights under the Employment Act"*
- *"What does the Companies Act say about director duties?"*
- *"Is the Competition and Fair Trading Act still in force?"*
- *"Find provisions about consumer protection in Malawian law"*
- *"Build a legal stance on data breach notification obligations in Malawi"*
- *"Validate the citation 'Section 5 of the Electronic Transactions and Cybersecurity Act 2016'"*

---

## What's Included

| Category | Count | Details |
|----------|-------|---------|
| **Acts** | 545 statutes | Comprehensive Malawian legislation |
| **Provisions** | 14,392 sections | Full-text searchable with FTS5 |
| **Legal Definitions** | Included | Extracted from Act texts |
| **Database Size** | ~25 MB | Optimized SQLite, portable |
| **Freshness Checks** | Automated | Monitoring against malawilii.org |

**Verified data only** -- every citation is validated against official sources (malawilii.org, Malawi Law Commission). Zero LLM-generated content.

---

## Why This Works

**Verbatim Source Text (No LLM Processing):**
- All statute text is ingested from malawilii.org (Malawi Legal Information Institute) and the Malawi Law Commission
- Provisions are returned **unchanged** from SQLite FTS5 database rows
- Zero LLM summarization or paraphrasing -- the database contains statute text, not AI interpretations

**Smart Context Management:**
- Search returns ranked provisions with BM25 scoring (safe for context)
- Provision retrieval gives exact text by Act name and section number
- Cross-references help navigate without loading everything at once

**Technical Architecture:**
```
malawilii.org / lawcom.mw --> Parse --> SQLite --> FTS5 snippet() --> MCP response
                                ^                        ^
                         Provision parser         Verbatim database query
```

### Traditional Research vs. This MCP

| Traditional Approach | This MCP Server |
|---------------------|-----------------|
| Search malawilii.org by Act name | Search by plain language: *"data protection consent"* |
| Navigate multi-section Acts manually | Get the exact provision with context |
| Manual cross-referencing between Acts | `build_legal_stance` aggregates across sources |
| "Is this Act still in force?" --> check manually | `check_currency` tool --> answer in seconds |
| Find SADC/AU alignment --> dig through frameworks | `get_eu_basis` --> linked frameworks instantly |
| No API, no integration | MCP protocol --> AI-native |

**Traditional:** Search malawilii.org --> Navigate HTML --> Ctrl+F --> Cross-reference between Acts --> Check amendments manually --> Repeat

**This MCP:** *"What are the data protection obligations under the Electronic Transactions and Cybersecurity Act and how do they align with SADC frameworks?"* --> Done.

---

## Available Tools (13)

### Core Legal Research Tools (8)

| Tool | Description |
|------|-------------|
| `search_legislation` | FTS5 full-text search across 14,392 provisions with BM25 ranking. Supports quoted phrases, boolean operators, prefix wildcards |
| `get_provision` | Retrieve specific provision by Act name and section number |
| `check_currency` | Check if an Act is in force, amended, or repealed |
| `validate_citation` | Validate citation against database -- zero-hallucination check. Supports "Section 5 Electronic Transactions Act", "s. 22 Penal Code" |
| `build_legal_stance` | Aggregate citations from multiple Acts for a legal topic |
| `format_citation` | Format citations per Malawian conventions (full/short/pinpoint) |
| `list_sources` | List all available Acts with metadata, coverage scope, and data provenance |
| `about` | Server info, capabilities, dataset statistics, and coverage summary |

### International Law Integration Tools (5)

| Tool | Description |
|------|-------------|
| `get_eu_basis` | Get international frameworks (SADC, AU, Commonwealth) that a Malawian Act aligns with |
| `get_malawian_implementations` | Find Malawian laws implementing a specific international framework or convention |
| `search_eu_implementations` | Search international documents with Malawian alignment counts |
| `get_provision_eu_basis` | Get international law references for a specific provision |
| `validate_eu_compliance` | Check alignment status of Malawian laws against international standards |

---

## International Law Alignment

Malawi is not an EU member state. Malawian law develops through its own Westminster-derived constitutional and parliamentary framework, with international alignment through:

- **SADC** -- Southern African Development Community protocols on trade, finance, and governance
- **African Union (AU)** -- Malabo Convention on Cybersecurity and Personal Data Protection; AU frameworks on digital economy
- **Commonwealth** -- Commonwealth legal traditions and model laws
- **UN Conventions** -- UNCAC (anti-corruption), Convention on the Rights of the Child, and international human rights instruments

The international bridge tools allow you to explore these alignment relationships -- checking which Malawian provisions correspond to SADC or AU requirements, and vice versa.

> **Note:** International cross-references reflect alignment and framework relationships, not direct transposition. Malawi develops its own legislative approach, and the alignment tools help identify where Malawian and international law address similar domains.

---

## Data Sources & Freshness

All content is sourced from authoritative Malawian legal databases:

- **[malawilii.org](https://malawilii.org/)** -- Malawi Legal Information Institute (primary source)
- **[Malawi Law Commission](https://www.lawcom.mw/)** -- Official law reform and consolidation body
- **[Parliament of Malawi](https://www.parliament.gov.mw/)** -- Official Acts and Bills

### Data Provenance

| Field | Value |
|-------|-------|
| **Authority** | Republic of Malawi |
| **Primary source** | malawilii.org (Malawi Legal Information Institute) |
| **Languages** | English (official language) |
| **Coverage** | 545 consolidated Acts |
| **Last ingested** | 2026-02-25 |

### Automated Freshness Checks

A [GitHub Actions workflow](.github/workflows/check-updates.yml) monitors data sources for changes:

| Check | Method |
|-------|--------|
| **Act amendments** | Drift detection against known provision anchors |
| **New Acts** | Comparison against source index |
| **Repealed Acts** | Status change detection |

**Verified data only** -- every citation is validated against official sources. Zero LLM-generated content.

---

## Security

This project uses multiple layers of automated security scanning:

| Scanner | What It Does | Schedule |
|---------|-------------|----------|
| **CodeQL** | Static analysis for security vulnerabilities | Weekly + PRs |
| **Semgrep** | SAST scanning (OWASP top 10, secrets, TypeScript) | Every push |
| **Gitleaks** | Secret detection across git history | Every push |
| **Trivy** | CVE scanning on filesystem and npm dependencies | Daily |
| **Socket.dev** | Supply chain attack detection | PRs |
| **Dependabot** | Automated dependency updates | Weekly |

See [SECURITY.md](SECURITY.md) for the full policy and vulnerability reporting.

---

## Important Disclaimers

### Legal Advice

> **THIS TOOL IS NOT LEGAL ADVICE**
>
> Statute text is sourced from malawilii.org and the Malawi Law Commission. However:
> - This is a **research tool**, not a substitute for professional legal counsel
> - **Court case coverage is not included** -- do not rely solely on this for case law research
> - **Verify critical citations** against primary sources for court filings
> - **International cross-references** reflect alignment relationships, not direct transposition

**Before using professionally, read:** [DISCLAIMER.md](DISCLAIMER.md) | [SECURITY.md](SECURITY.md)

### Client Confidentiality

Queries go through the Claude API. For privileged or confidential matters, use on-premise deployment.

### Bar Association

For professional legal use in Malawi, consult guidance from the **Malawi Law Society** regarding professional obligations and confidentiality requirements.

---

## Development

### Setup

```bash
git clone https://github.com/Ansvar-Systems/Malawi-law-mcp
cd Malawi-law-mcp
npm install
npm run build
npm test
```

### Running Locally

```bash
npm run dev                                       # Start MCP server
npx @anthropic/mcp-inspector node dist/index.js   # Test with MCP Inspector
```

### Data Management

```bash
npm run ingest              # Ingest Acts from malawilii.org
npm run build:db            # Rebuild SQLite database
npm run drift:detect        # Run drift detection against anchors
npm run check-updates       # Check for amendments and new Acts
npm run census              # Generate coverage census
```

### Performance

- **Search Speed:** <100ms for most FTS5 queries
- **Database Size:** ~25 MB (efficient, portable)
- **Reliability:** 100% ingestion success rate across 545 Acts

---

## Related Projects: Complete Compliance Suite

This server is part of **Ansvar's Compliance Suite** -- MCP servers that work together for end-to-end compliance coverage:

### [@ansvar/eu-regulations-mcp](https://github.com/Ansvar-Systems/EU_compliance_MCP)
**Query 49 EU regulations directly from Claude** -- GDPR, AI Act, DORA, NIS2, MiFID II, eIDAS, and more. Full regulatory text with article-level search. `npx @ansvar/eu-regulations-mcp`

### [@ansvar/us-regulations-mcp](https://github.com/Ansvar-Systems/US_Compliance_MCP)
**Query US federal and state compliance laws** -- HIPAA, CCPA, SOX, GLBA, FERPA, and more. `npx @ansvar/us-regulations-mcp`

### [@ansvar/security-controls-mcp](https://github.com/Ansvar-Systems/security-controls-mcp)
**Query 261 security frameworks** -- ISO 27001, NIST CSF, SOC 2, CIS Controls, SCF, and more. `npx @ansvar/security-controls-mcp`

**70+ national law MCPs** covering Australia, Brazil, Canada, Denmark, Ethiopia, France, Germany, Ghana, India, Ireland, Japan, Kenya, Netherlands, Nigeria, Norway, Singapore, South Africa, Sweden, Switzerland, UAE, UK, and more.

---

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Priority areas:
- Court case law expansion (High Court, Supreme Court of Appeal)
- AU Malabo Convention cross-references
- SADC protocol alignment mapping
- Historical Act versions and amendment tracking
- Subordinate legislation coverage (statutory instruments)

---

## Roadmap

- [x] Core Act database with FTS5 search
- [x] Full corpus ingestion (545 Acts, 14,392 provisions)
- [x] International law alignment tools
- [x] Vercel Streamable HTTP deployment
- [x] npm package publication
- [ ] Court case law expansion (High Court, Supreme Court of Appeal)
- [ ] Historical Act versions (amendment tracking)
- [ ] Statutory instruments and subsidiary legislation
- [ ] SADC protocol cross-references

---

## Citation

If you use this MCP server in academic research:

```bibtex
@software{malawi_law_mcp_2026,
  author = {Ansvar Systems AB},
  title = {Malawi Law MCP Server: AI-Powered Legal Research Tool},
  year = {2026},
  url = {https://github.com/Ansvar-Systems/Malawi-law-mcp},
  note = {545 Malawian Acts with 14,392 provisions sourced from malawilii.org}
}
```

---

## License

Apache License 2.0. See [LICENSE](./LICENSE) for details.

### Data Licenses

- **Statutes & Legislation:** Republic of Malawi (public domain via malawilii.org open access)
- **International Framework Metadata:** SADC / AU / Commonwealth (public domain)

---

## About Ansvar Systems

We build AI-accelerated compliance and legal research tools for the global market. This MCP server started as our internal reference tool -- turns out everyone building for the Malawian or Southern African market has the same research frustrations.

So we're open-sourcing it. Navigating 545 Acts shouldn't require a law degree.

**[ansvar.eu](https://ansvar.eu)** -- Stockholm, Sweden

---

<p align="center">
  <sub>Built with care in Stockholm, Sweden</sub>
</p>
