# SETTLEMENT ENGINE

## Purpose

Convert validated competition results into deterministic financial outcomes.

## Workflow

1. identify competition;
2. import result data;
3. validate schema;
4. resolve registrations/players;
5. validate game-specific metrics;
6. run settlement rules;
7. produce preview;
8. admin reviews preview;
9. admin explicitly confirms;
10. execute atomic settlement;
11. create winning ledger credits;
12. mark settlement complete;
13. notify affected players.

## Settlement input

Input should identify competition, result source, participants and game-specific metrics. Example fields may include rank, kills, finishes, score, team, outcome or other metrics depending on the competition.

## Validation

Reject unknown competition IDs, nonexistent registrations, duplicate participants, invalid numeric values, impossible ranks, unauthorized result changes and settlement totals exceeding configured limits.

## Determinism

The same validated settlement input and configuration must produce the same financial output. Preserve a snapshot of the configuration and calculation used.

## PUBG-style result rules

Where a competition uses PUBG-specific rules, those rules belong to its settlement adapter/configuration. Examples may include distinguishing knockdowns from confirmed finishes, handling environmental deaths, revives and final credited kills. These rules must not become assumptions for other games.

## Atomicity

Settlement confirmation must prevent partial completion. If one winner credit fails, the settlement must not leave a subset credited while the settlement is marked complete.

## Duplicate protection

A completed settlement for a competition cannot be executed again. Retry requests must return the existing settlement outcome.

## Corrections

A confirmed settlement must not be silently edited. Corrections require a controlled reversal/adjustment process, reason, authorization and audit trail.
