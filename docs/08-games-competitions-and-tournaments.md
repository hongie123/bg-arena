# GAMES, COMPETITIONS AND TOURNAMENTS

## Game-agnostic model

A game is a catalog entity. A competition references a game but owns its own operational configuration.

Never add columns such as `pubg_id`, `codm_uid`, or similar to the core profile table.

## Competition configuration

A competition can define:

- game;
- format;
- title/description;
- rules;
- registration window;
- event schedule;
- capacity;
- USD entry fee;
- USD prize structure;
- registration fields;
- settlement configuration;
- status.

## Formats

The data model should permit tournaments, single matches, brackets, leagues or other formats without changing wallet architecture.

## Status lifecycle

DRAFT -> PUBLISHED -> REGISTRATION_OPEN -> REGISTRATION_CLOSED -> IN_PROGRESS -> RESULT_PENDING -> SETTLEMENT_PENDING -> SETTLED -> COMPLETED.

Cancellation paths must be explicit and must define entry-fee/refund consequences.

## Prize configuration

Prize amounts are stored/calculated in USD. A competition must not settle more than its configured distributable prize amount. Any platform fee must be explicitly configured and represented in the financial model.

## Registration eligibility

Validate status, timing, capacity, duplicate registration, required fields, player account status and entry-fee requirements.

## Competition-specific rules

Rules and game-specific result/registration requirements are attached to the competition or a reusable configuration. Core services consume normalized outputs.
