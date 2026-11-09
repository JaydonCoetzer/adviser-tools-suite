# Adviser Tools Suite — Concept Wealth Woodhill

A growing suite of client-facing financial planning tools for the Concept Wealth
Woodhill advice desk (Sanlam-authorised, Pretoria). Each tool is a **standalone,
client-side HTML file** — no build step, no bundler. Open any file in a browser and
it works.

## Tools

| Tool | File | Status |
|---|---|---|
| **Retirement Cashflow Projector** | `retirement-cashflow-projector.html` | ✅ Available |
| Tax tool | — | In development |
| Risk / cover tool | — | In development |
| Estate tool | — | In development |

`index.html` is the "Adviser Toolkit" landing page that links the tools.

## Retirement Cashflow Projector

Projects a client's retirement income from a living annuity: pre-retirement fund
growth, at-retirement allocation (lump sum / discretionary / living annuity),
drawdown, fees, SARS 2026/27 income tax, medical credits, inflation and real
purchasing power, plus ad-hoc events.

- **⤓ Download PDF** — one click generates a branded, multi-page client report
  (`Retirement_Cashflow_<ClientName>.pdf`) via jsPDF, styled to match the Concept
  *Investment & Retirement Summary* pack.
- **🖨 Print** — native print of the on-screen report.
- Append `?example=1` to the URL to auto-load a worked example.

## Design system

- Font: Plus Jakarta Sans (on-screen). Report uses a Segoe UI / Calibri serifless stack.
- Glass UI, navy + blue palette. See `DESIGN_BRIEF.md`.
- Charts: Chart.js 4.4.x (CDN). PDF: jsPDF + jspdf-autotable (CDN).

## Running

No server required — open the `.html` files directly in a browser. The tools load
Chart.js, jsPDF and Google Fonts from CDN, so an internet connection is needed for
charts, fonts and PDF export.

---

*For illustrative purposes only; not financial advice. Tax figures per SARS 2026/27.*
*Concept Wealth Woodhill · FSP authorised by Sanlam.*
