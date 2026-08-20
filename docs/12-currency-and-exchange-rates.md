# BG ARENA — CURRENCY AND EXCHANGE-RATE SERVICE

# 1. PURPOSE — P0
Centralize external-currency-to-USD conversion so financial code never embeds provider-specific exchange-rate calls.

# 2. INTERFACE — P1
`getRate(fromCurrency,toCurrency)`, `convertToUSD(amount,sourceCurrency)`, `getSupportedCurrencies()`, provider health/status. Use a provider abstraction behind `ExchangeRateService`.

# 3. FINANCIAL RATE — P0
Display estimates and settlement rates are different concepts. A UI estimate may be cached. The authoritative rate used to credit a completed deposit must follow the defined processing policy and be stored as an immutable snapshot.

# 4. RATE SNAPSHOT — P0
Store base currency, quote currency USD, rate, provider/source, provider reference if available, retrieved timestamp, usage/deposit reference and creation timestamp. Never rewrite the snapshot after it is used for a completed deposit.

# 5. FIAT — P0
XAF/EUR/etc. are external currencies only. If unsupported, fail safely. Do not infer a rate from currency symbols or hardcoded assumptions.

# 6. CRYPTO — P0
For NOWPayments, prefer confirmed provider payment data where it establishes the applicable payment value, while still recording BG Arena's USD accounting rate/source and conversion evidence. Distinguish asset from network, e.g. USDT/TRON vs USDT/Ethereum.

# 7. RATE FAILURE — P0
If a payment is confirmed but no reliable rate exists, set CONVERSION_PENDING/CONVERSION_FAILED according to policy. Do not guess or use a stale display estimate as an authoritative rate unless the policy explicitly allows it and records that fact.

# 8. ROUNDING — P0
Define decimal precision and rounding before production. Rounding must be deterministic and applied once at the accounting boundary. Preserve original source amount separately.

# 9. PROVIDER FALLBACK — P2
A fallback provider may be configured. When used, record that source explicitly. Never silently switch providers without preserving evidence.

# 10. CACHE — P2
Cache may reduce display/API load but must have TTL and must not accidentally become financial authority.

# 11. TESTS — P0
Test normal conversion, unsupported currency, provider timeout, malformed rate, zero/negative rate, rounding, fallback selection and historical rate immutability.
