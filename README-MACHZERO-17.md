# SymPy #30976 — Independent Approach to Incorrect Sign Evaluation

## 1. Problem Statement

SymPy sometimes produces incorrect negative results when evaluating absolute values and square roots of certain symbolic integer expressions.

This is a mathematical correctness issue because:

- Absolute values must always be nonnegative.
- The principal square root of a real square must be nonnegative.
- Symbolic assumptions must not produce contradictory mathematical results.

## 2. Objective

Develop an independent solution that ensures symbolic expressions retain mathematically valid sign information throughout simplification and substitution.

The objective is to improve the reliability of sign evaluation without changing the intended mathematical behavior of existing SymPy functions.

## 3. Proposed Investigation

Instead of immediately modifying `Abs` or `sqrt`, we will investigate the complete evaluation process.

Our approach:

1. Examine how symbolic expressions obtain their sign properties.
2. Investigate how integer and nonnegative assumptions influence expression evaluation.
3. Identify whether an incorrect sign conclusion is introduced during expression construction, simplification, or substitution.
4. Trace how that conclusion propagates into absolute-value and square-root evaluation.

## 4. Independent Solution Strategy

We propose investigating a validation-based approach to symbolic sign conclusions.

The approach will:

- Examine the assumptions available for each expression.
- Distinguish proven sign information from unknown sign information.
- Avoid relying on an unsupported sign conclusion.
- Preserve unevaluated expressions when their sign cannot be established safely.
- Ensure subsequent evaluation does not contradict established mathematical properties.

The precise implementation will be determined after identifying the source of the incorrect inference.

## 5. Proposed Code Organization

### A. Sign Analysis

Investigate the existing assumptions and symbolic property evaluation mechanisms.

Goal: Ensure sign information is mathematically justified before it is used in further evaluation.

### B. Expression Evaluation

Investigate how `Abs` and square-root expressions consume sign information.

Goal: Prevent incorrect sign conclusions from propagating into evaluated expressions.

### C. Regression Testing

Create independent tests covering:

- Integer symbols.
- Nonnegative integer symbols.
- Expressions with unknown sign.
- Positive and negative substitutions.
- Absolute values of symbolic expressions.
- Square roots of squared expressions.

## 6. Testing and Validation

The solution must satisfy these mathematical invariants:

- `Abs(x) >= 0` for real `x`.
- `sqrt(x**2) >= 0` for real `x`.
- Substitution must preserve mathematically correct results.
- Unknown symbolic signs must not be treated as proven positive or negative.
- Existing valid simplifications must continue to work.

## 7. Expected Impact

The proposed improvement aims to:

- Increase reliability of symbolic sign inference.
- Prevent incorrect negative outputs.
- Improve consistency between symbolic evaluation and numerical substitution.
- Reduce the possibility of similar regressions in other symbolic operations.

## 8. Implementation Scope

The solution will prioritize a minimal, independently developed correction.

Only the necessary existing SymPy components and regression tests will be modified.

No unnecessary changes to the architecture or mathematical definitions of SymPy are intended.

## 9. Success Criteria

The issue will be considered resolved when:

1. The original regression is reproducible before the fix.
2. The proposed correction eliminates the incorrect result.
3. All new regression tests pass.
4. Relevant existing SymPy tests continue to pass.
5. The fix does not introduce incorrect simplifications for expressions with unknown signs.
