# PLAYER DASHBOARD

# 1. NAVIGATION — P1

Dashboard, Games/Tournaments, Wallet, Results & History, Notifications, Tutorials, Support, Account/Settings.

# 2. DASHBOARD — P1

Show available USD balance, reserved balance where relevant, active/upcoming registrations, recent results, relevant notifications and shortcuts to competitions/deposit/withdrawal. Never expose admin controls.

# 3. GAMES/TOURNAMENTS — P1

Provide browse/search/filter capabilities. Competition cards/details must make game, status, schedule, entry fee USD, capacity/availability, prize information and registration requirements clear.

# 4. WALLET — P0

Show USD balance, reserved amount, transactions, deposit and withdrawal actions. Deposit history should show source currency/amount and resulting USD credit where appropriate.

# 5. RESULTS & HISTORY — P1

Show registrations, participation, placements, winnings and relevant financial references without exposing another player's private data.

# 6. NOTIFICATIONS — P1

Show unread count, notification list and read/unread state.

# 7. TUTORIALS — P2

Explain registration, deposits, competitions, wallet, withdrawals and platform rules in lightweight language.

# 8. SUPPORT — P1

Players can create/reply to tickets and see status.

# 9. ACCOUNT/SETTINGS — P1

Profile/contact data, authentication/security settings, account status and logout.

# 10. UI STATES — P0

Every page needs loading, empty, success and error states. Money must identify USD. Money-affecting/destructive actions require confirmation.

# 11. AI IMPLEMENTATION DIRECTIVES

## P0

The dashboard is a presentation layer. It must never perform authoritative wallet, registration or settlement mutations directly.

## P1

Every displayed financial value must come from authorized server/database data, not locally invented calculations.
