# Project Roadmap

> AI Data Cleaner is built part by part, with explicit approval gates between each phase. This document shows the high-level progression from MVP to full product.

---

## Completed — v0.11 (Parts 1–16)

### Foundation (Parts 1–5)
- [x] Project structure and module architecture
- [x] Security layer — authentication, session management, `.env` configuration
- [x] Login interface (session-based, credential-controlled)
- [x] Dataset safety enforcement — immutable original, working copy isolation
- [x] Azure OpenAI integration (Responses API)

### Core Analysis (Parts 6–9)
- [x] Data Quality Report generation — structured, exportable
- [x] Error taxonomy implementation — Type 1, Type 2, Type 3 classification
- [x] AI-powered dataset analysis with structured output
- [x] Analysis report export (`.txt`)

### Cleaning Pipeline (Parts 9–11)
- [x] Deterministic cleaning methods catalog
- [x] Cleaning execution with scope selection by error type
- [x] Cleaning report export (`.txt`)
- [x] Cleaned dataset export (`.csv`)
- [x] AI Data Assistant (conversational, dataset-aware)

### Advanced Features (Parts 12–16)
- [x] Cell-level confidence scoring
- [x] Before/after confidence heatmaps with threshold visualization
- [x] Quality score KPIs with delta (before/after cleaning)
- [x] Calibration mode — ground-truth comparison, quantitative metrics
- [x] Calibration history persistence (local log)
- [x] Guided workflow interface (6 sequential steps)
- [x] Default mode / Advanced mode UX split
- [x] UI stabilization — sober, professional, enterprise-ready

---

## In Progress — v0.12 (Part 17)

### Multi-Source Import & Export
- [ ] Excel support (multi-sheet)
- [ ] Google Sheets connector
- [ ] Export to multiple formats
- [ ] Source metadata tracking in reports

---

## Planned — v0.13+

### Extended Detection
- [ ] Broader Type 3 error coverage
- [ ] Cross-column consistency checks
- [ ] Custom error type definitions

### Calibration & Reporting
- [ ] Enhanced calibration tooling
- [ ] Custom report templates
- [ ] Comparative session reporting (run A vs run B)

### Platform
- [ ] Multi-dataset session support
- [ ] Role-based access levels
- [ ] API mode for pipeline integration

---

## Methodology Note

Each part is scoped, approved, implemented, tested, and logged before the next begins. The roadmap reflects a deliberate pace — stability and correctness over speed of delivery. No part is shipped without a green Streamlit run and bug log update.
