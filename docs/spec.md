---
tags: [flagship, finance, parser, rust, mcp, local-first, india]
aliases: [Paisa, Finance Tracker, PSF, Paisa Standard Format]
created: 2026-06-26
status: solid
---

# Paisa — Personal Finance Parser + Dashboard

> **Status:** ✅ Solid spec. Ready to build.
> **Platform:** CLI + Web dashboard
> **Stack:** Rust parser + Next.js dashboard
> **Effort:** ~88 hours full, ~30 hours MVP

---

## The Story

You don't know how money works. You don't know the difference between stocks and shares. You want financial literacy but every tool assumes you already know.

**Paisa is different.** It starts with a parser — feed it bank statements, mutual fund PDFs, SIP emails — and it builds your financial picture automatically.

**The moat:** PSF (Paisa Standard Format) — a universal format for financial data. Once parsed, any app can consume it.

**Inspired by:**
- paisa.fyi (Indian finance tracker)
- beancount / ledger CLI (double-entry accounting)
- Plaid / Setu (Indian financial data pipes)

---

## The Problem

**Indian finance is messy:**
- Banks export in different formats (CSV, PDF, XLSX)
- Mutual funds send emails, not structured data
- No standard format for financial data
- Existing tools (Walnut, ET Money) are SaaS — your data lives on their servers

**Current solutions fail because:**
- Manual entry is tedious
- SaaS means you don't own your data
- No Indian-specific categories (SIP, ELSS, PPF, NPS)
- No parser — you categorize everything yourself

---

## The Solution

**Paisa = Parser-first + Local-first + Indian-specific**

1. **Parse anything** — Bank statements, MF emails, SIP confirmations
2. **PSF format** — Universal format for financial data
3. **Local-first** — Your data stays on your machine
4. **Indian-specific** — SIP, ELSS, PPF, NPS, ₹ formatting
5. **Dashboard** — Visualize spending, investments, net worth

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    PAISA ECOSYSTEM                           │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │  INPUT SOURCES                                         │ │
│  │  - Bank CSV/PDF (SBI, HDFC, ICICI, etc.)              │ │
│  │  - MF emails (CAMS, Kfintech)                         │ │
│  │  - SIP confirmations                                  │ │
│  │  - Manual entry                                       │ │
│  └──────────────────────┬─────────────────────────────────┘ │
│                         │                                    │
│  ┌──────────────────────┴─────────────────────────────────┐ │
│  │  RUST PARSER                                           │ │
│  │  - CSV parser (csv crate)                             │ │
│  │  - PDF parser (pdf-extract)                           │ │
│  │  - XLSX parser (calamine)                             │ │
│  │  - Email parser (tesseract-rs for scanned PDFs)       │ │
│  │  - Categorizer (rule-based + ML)                      │ │
│  └──────────────────────┬─────────────────────────────────┘ │
│                         │                                    │
│  ┌──────────────────────┴─────────────────────────────────┐ │
│  │  PSF (Paisa Standard Format)                           │ │
│  │  - transactions.psf                                   │ │
│  │  - accounts.psf                                       │ │
│  │  - investments.psf                                    │ │
│  │  - Used by: Paper, Momentum, any future app            │ │
│  └──────────────────────┬─────────────────────────────────┘ │
│                         │                                    │
│  ┌──────────────────────┴─────────────────────────────────┐ │
│  │  DASHBOARD (Next.js)                                   │ │
│  │  - Spending breakdown                                  │ │
│  │  - Investment tracker                                  │ │
│  │  - Net worth over time                                 │ │
│  │  - Tax estimates                                      │ │
│  └──────────────────────────────────────────────────────  │ │
└─────────────────────────────────────────────────────────────┘
```

---

## Features

### P0 — MVP (~30h)

1. **CSV Parser** — SBI, HDFC, ICICI bank statements
2. **PSF Format** — Define and implement standard format
3. **Basic Dashboard** — Spending breakdown, category pie chart
4. **Indian Categories** — SIP, ELSS, PPF, NPS, UPI, etc.
5. **Local-First** — SQLite, no server required

### P1 — Parser Pro (~30h)

6. **PDF Parser** — Scanned bank statements (OCR)
7. **MF Email Parser** — CAMS/Kfintech email parsing
8. **Auto-Categorization** — Rule-based + simple ML
9. **Investment Tracker** — MF, stocks, FD, RD

### P2 — Intelligence (~28h)

10. **Net Worth** — Assets vs liabilities over time
11. **Tax Estimates** — Indian tax regime calculations
12. **Budgeting** — Monthly budgets with alerts
13. **MCP Server** — Agents can query financial data

---

## PSF Format (The Moat)

```json
{
  "version": "1.0",
  "transactions": [
    {
      "id": "txn_001",
      "date": "2026-06-27",
      "amount": -500.00,
      "currency": "INR",
      "description": "Swiggy Order",
      "category": "food",
      "subcategory": "delivery",
      "account": "hdfc_savings",
      "type": "debit"
    }
  ],
  "accounts": [
    {
      "id": "hdfc_savings",
      "name": "HDFC Savings",
      "type": "bank",
      "balance": 45000.00,
      "currency": "INR"
    }
  ]
}
```

**PSF is consumed by:** Paper (trading), Momentum (tasks), any future finance app.

---

## Monetization

| Tier | Price | What |
|------|-------|------|
| **Free** | $0 | Parser + PSF + basic dashboard |
| **Pro** | ₹99/mo | Cloud sync, MF parsing, tax estimates |
| **Family** | ₹299/mo | Multiple accounts, shared budgets |

---

## Worth Assessment

| Dimension | Score | Notes |
|-----------|-------|-------|
| Portfolio value | 9/10 | Rust + parser + Indian market |
| Learning value | 9/10 | Rust, PDF parsing, financial domain |
| Market/user value | 8/10 | Indian finance is underserved |
| Personal value | 8/10 | You'll learn financial literacy |

---

## Dependencies

- **Paper** — Uses PSF for trading data
- **Momentum** — Uses PSF for task-related expenses

---

## Quality Markers

- [ ] Parses SBI/HDFC/ICICI CSV
- [ ] PSF format is well-defined
- [ ] Dashboard shows spending breakdown
- [ ] Indian categories work
- [ ] Local-first with optional cloud sync
- [ ] MCP server for agent access

---

**Build second.** Paper depends on PSF format. Compounding wins.
