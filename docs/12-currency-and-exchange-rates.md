# CURRENCY AND EXCHANGE RATES

## Internal rule

USD is the only internal wallet/accounting currency.

## Deposit conversion

When a deposit becomes eligible for credit:

1. identify source currency/asset;
2. obtain the current configured exchange rate to USD;
3. record the rate and source;
4. calculate exact USD value;
5. apply configured rounding once at the defined boundary;
6. credit the USD wallet;
7. store the conversion snapshot permanently with the deposit.

## Rate source

The exact production exchange-rate provider is a configurable integration. Do not hard-code an invented provider URL or rate.

## Crypto pricing

Crypto conversion must use the configured provider/rate source appropriate to the deposit processing time. Preserve asset, network, rate and timestamp.

## Rate failures

If the rate service is unavailable, returns invalid data, or the rate is outside configured sanity limits, do not credit the deposit automatically. Keep it pending and surface an operational error.

## Rate audit

A historical transaction must never be recalculated later merely because today's rate differs. The rate used at credit time is part of the financial record.

## Display

The UI can display estimated USD values before payment confirmation, but labels them as estimates until the authoritative payment and conversion process completes.
