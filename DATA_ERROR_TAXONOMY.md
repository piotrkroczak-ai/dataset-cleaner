# Data Error Taxonomy

> The classification system at the heart of AI Data Cleaner. Every detected issue is assigned a type that determines how it is reported, handled, and cleaned.

---

## Why a Taxonomy?

Data cleaning without a shared vocabulary produces inconsistent results. Calling "trailing whitespace" and "ambiguous partial duplicate" both just "errors" collapses a crucial distinction: one is safely auto-fixable, the other requires human judgment.

This taxonomy gives:
- A shared language between the AI analysis and the cleaning pipeline
- A structured basis for the calibration system (expected vs. detected per type)
- An explainable audit trail ("this row was modified because of a Type 1 / leading space")
- A safety gradient — more dangerous operations require more user intent

---

## The Three Types

### Type 1 — Simple Errors
**Profile:** High-confidence, structurally obvious, low risk of overcorrection.

These are errors where the correct fix is unambiguous — the kind a rule-based system could handle reliably. The AI confirms their presence; the cleaning pipeline handles them deterministically.

**Examples:**
- Missing values (empty cells)
- Full duplicate rows
- Leading or trailing whitespace
- Dirty column names (special characters, inconsistent casing)
- Decimal comma/period inconsistency
- Boolean value variants (`True/False`, `1/0`, `Yes/No`)

**Cleaning approach:** Typically auto-cleanable with a single deterministic function.

---

### Type 2 — Intermediate Errors
**Profile:** Context-dependent, correct interpretation requires configuration.

These errors are real but require the user to define the expected behavior before cleaning. The system detects them and flags the configuration needed.

**Examples:**
- Mixed date formats within a column
- Simple statistical outliers
- Negative values in logically non-negative fields
- Country name variants (`France`, `FR`, `FRA`)
- Invalid email formats
- Future dates in historical fields

**Cleaning approach:** User configures the target format or threshold; cleaning executes deterministically against that configuration.

---

### Type 3 — Advanced Errors
**Profile:** Ambiguous, requires business context or human judgment.

These are errors the system can detect but cannot safely clean automatically. They are flagged for human review, with the relevant rows identified.

**Examples:**
- Corrupted or garbled text values
- Partial duplicates (same entity, different records)
- Multi-column date inconsistencies
- Irrelevant or noise columns

**Cleaning approach:** Flagged and reported; no automatic transformation. The user decides.

---

## Counting Convention

To ensure reproducible, comparable metrics:
- The official unit of measurement is the **unique error event**: `(row_id, column, error_type)`
- Schema-level issues (column names, structure) use a reserved identifier rather than a row number
- The same error event is never counted twice
- Calibration metrics compare actual vs. detected vs. cleaned counts per type

This convention is what makes the calibration system meaningful — without a clear counting unit, "detection coverage" is ambiguous.

---

## How Types Flow Through the Application

```
Detection (AI + Python)
    │
    ├── Type 1 → Confidence scoring → Auto-clean eligible → Cleaning report
    │
    ├── Type 2 → Confidence scoring → Requires config → User configures → Clean → Report
    │
    └── Type 3 → Confidence scoring → Flagged for review → Reported, not cleaned
```

The quality score and heatmap reflect all three types. The cleaning delta (before vs. after) reflects only what was actually cleaned.
