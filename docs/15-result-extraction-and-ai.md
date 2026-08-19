# RESULT EXTRACTION AND AI

## Purpose

AI may assist administrators by extracting structured competition results from spectator recordings, screenshots, documents or other evidence. AI is not trusted to make financial decisions.

## Pipeline

record evidence -> extraction model -> structured JSON -> schema validation -> business validation -> admin preview -> settlement engine.

## Required structured output

A result payload should identify:

- competition ID/reference;
- extraction timestamp/source;
- participants/registration references;
- normalized metrics;
- confidence/uncertainty where available;
- source evidence reference where available.

## Security

Treat model output as untrusted. Validate every field and reject malformed/ambiguous output. Never execute arbitrary code from model output.

## Human review

The administrator must be able to inspect extracted results before settlement. Any uncertain mapping must require explicit correction or confirmation.

## Game-specific extraction

Extraction schemas may differ by game. The generic settlement pipeline should consume normalized results after game-specific validation.

## Financial boundary

AI cannot directly call wallet credit operations. Only the authorized settlement service can produce ledger transactions after validation and explicit confirmation.
