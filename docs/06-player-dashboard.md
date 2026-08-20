# BG ARENA — PLAYER DASHBOARD SPECIFICATION

# 1. PURPOSE — P1
The player application is the safe, simple interface through which authenticated players manage competitions, wallet, results, notifications and support.

# 2. NAVIGATION — P1
Dashboard; Games/Tournaments; Wallet; Results & History; Notifications; Tutorials; Support; Account/Settings.

# 3. DASHBOARD HOME — P1
Show greeting, avatar initial, USD available balance, reserved balance when relevant, active/upcoming competitions, recent transactions, notifications and useful calls to action. Do not expose raw provider/internal security data.

# 4. WALLET DISPLAY — P0
Wallet balance comes from authoritative server/ledger-derived data. Never calculate or mutate authoritative balance in client state. Clearly display `$`/USD.

# 5. COMPETITION CARDS — P1
Show title, game, format, schedule, entry fee USD, prize information, capacity/status and registration action. Avoid showing stale status after mutations; refresh authoritative state.

# 6. ASYNC STATES — P1
Every page defines loading skeleton, empty state, success state, recoverable error and unavailable state. Financial operations also show pending/review states.

# 7. DEPOSIT UX — P1
Player selects target USD amount and payment method. Crypto selector and mobile-money network selector are provider-driven. Estimates are clearly labeled. Final credit is only shown after confirmed server processing.

# 8. HISTORY — P1
Transaction history shows USD financial effect and, for deposits, original payment amount/currency/provider when appropriate. Historical records are read-only to players.

# 9. SETTINGS — P1
Allow profile/contact settings, notification preferences and account actions. Do not expose secrets or admin controls.

# 10. ACCESSIBILITY/RESPONSIVENESS — P2
Controls have labels, keyboard/focus behavior, useful error text and responsive layouts suitable for mobile-first usage.

# 11. SECURITY — P0
Never embed service-role keys, provider credentials or trusted financial mutations in client code. Never accept a client-computed balance as authoritative.
