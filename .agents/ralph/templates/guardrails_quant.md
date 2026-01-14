# Guardrails (Signs) - Quant Research

> Lessons learned from failures. Read before acting.

## Core Signs

### Sign: Read Before Writing
- **Trigger**: Before modifying any file
- **Instruction**: Read the file first
- **Added after**: Core principle

### Sign: Test Before Commit
- **Trigger**: Before committing changes
- **Instruction**: Run required tests and verify outputs
- **Added after**: Core principle

---

## Data Handling Signs

### Sign: Validate Input Data
- **Trigger**: Before processing any data file
- **Instruction**: Run schema validation; check for nulls, NaN, out-of-range values
- **Added after**: Silent data corruption incidents

### Sign: Never Modify Source Data
- **Trigger**: When working with input files
- **Instruction**: Create derived copies; keep originals immutable
- **Added after**: Accidental data loss

### Sign: Check Data Leakage
- **Trigger**: When splitting data for train/test
- **Instruction**: Verify no information flows from test to train
- **Added after**: Overfitting due to leakage

### Sign: Document Data Provenance
- **Trigger**: When loading or transforming data
- **Instruction**: Log source path, row count, date range, and any filters applied
- **Added after**: Inability to trace results back to source data

---

## Numerical Computation Signs

### Sign: Check for NaN/Inf
- **Trigger**: After every numerical computation
- **Instruction**: Assert no NaN or Inf in results before proceeding
- **Added after**: Silent propagation of invalid values

### Sign: Use Vectorized Operations
- **Trigger**: When processing arrays/dataframes
- **Instruction**: Prefer numpy/pandas vectorized ops over Python loops
- **Added after**: Performance issues with large datasets

### Sign: Set Random Seeds
- **Trigger**: At start of any experiment with randomness
- **Instruction**: Set and log seeds for numpy, random, torch, etc.
- **Added after**: Irreproducible results

### Sign: Handle Division by Zero
- **Trigger**: When computing ratios or percentages
- **Instruction**: Check for zero denominators; use safe division with fallback
- **Added after**: Runtime errors in edge cases

### Sign: Normalize Weights
- **Trigger**: When working with portfolio weights
- **Instruction**: Verify weights sum to 1.0 (or expected total); normalize if needed
- **Added after**: Incorrect aggregate calculations

---

## Reproducibility Signs

### Sign: Pin Dependencies
- **Trigger**: When adding new packages
- **Instruction**: Pin exact versions in requirements.txt
- **Added after**: Environment drift broke builds

### Sign: Verify Determinism
- **Trigger**: Before marking story complete
- **Instruction**: Run twice, compare outputs byte-for-byte (or within tolerance)
- **Added after**: Non-deterministic failures in CI

### Sign: Log All Parameters
- **Trigger**: When running experiments
- **Instruction**: Log all hyperparameters, thresholds, settings used
- **Added after**: Inability to reproduce past results

### Sign: Use Fixed Timestamps
- **Trigger**: When generating output with timestamps
- **Instruction**: Use deterministic timestamp (e.g., max of input dates) not current time
- **Added after**: Non-deterministic outputs due to timestamp drift

---

## Statistical Analysis Signs

### Sign: Report Confidence Intervals
- **Trigger**: When reporting statistical results
- **Instruction**: Include CI, not just point estimates
- **Added after**: Overconfident conclusions from noisy data

### Sign: Correct for Multiple Comparisons
- **Trigger**: When testing multiple hypotheses
- **Instruction**: Apply Bonferroni/FDR correction as appropriate
- **Added after**: False positives from p-hacking

### Sign: Validate Statistical Assumptions
- **Trigger**: Before applying statistical tests
- **Instruction**: Check normality, independence, homoscedasticity as required
- **Added after**: Invalid conclusions from violated assumptions

### Sign: Use Effect Sizes
- **Trigger**: When reporting significant results
- **Instruction**: Report effect size alongside p-value
- **Added after**: Statistically significant but practically meaningless findings

---

## Pydantic/Schema Signs

### Sign: Use Strict Mode
- **Trigger**: When defining Pydantic models
- **Instruction**: Use `model_config = ConfigDict(strict=True, extra="forbid")`
- **Added after**: Silent type coercion caused bugs

### Sign: Validate at Boundaries
- **Trigger**: When receiving external data
- **Instruction**: Parse through Pydantic model immediately; fail fast on invalid data
- **Added after**: Invalid data propagated deep into system

---

## Learned Signs

(Add project-specific lessons here as you encounter failures)
