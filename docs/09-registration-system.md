# REGISTRATION SYSTEM

## Flow

1. Authenticate player.
2. Load competition.
3. Verify competition is accepting registrations.
4. Verify account eligibility.
5. Load required dynamic fields.
6. Validate submitted field values.
7. Check capacity and duplicate-entry policy.
8. Determine entry fee.
9. Collect/debit entry fee using the wallet/payment workflow.
10. Create registration and associate the financial transaction atomically where possible.
11. Notify player.

## Dynamic fields

Each competition can define fields such as in-game username, player ID, team, region or other game-specific data. Field definitions contain type, label, required flag, order and validation configuration.

## Payment behavior

If the entry fee is paid from wallet funds, the wallet debit and successful registration must be consistent. A failed debit must not produce a paid registration.

If an external deposit is needed first, registration remains unpaid/pending until the required USD balance is actually credited.

## Cancellation/refund

If an admin cancels a competition, the platform follows the configured refund policy. Refunds are compensating wallet credits with references to the original entry transaction and cancellation reason.

## Multiple entries

Default behavior is one registration per player per competition. If a future format allows multiple entries, the competition must explicitly enable it and define how each entry is distinguished.
