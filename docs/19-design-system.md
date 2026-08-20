# BG ARENA — DESIGN SYSTEM AND UI GOVERNANCE

# 1. PURPOSE — P1
The design system keeps BG Arena visually consistent while allowing feature-specific components.

# 2. GLOBAL TOKENS — P0
Colors, typography, spacing, radii, shadows, breakpoints and motion are centralized. Feature code must not redefine global tokens to solve local styling problems.

# 3. SHARED COMPONENTS — P1
Buttons, inputs, selects, cards, dialogs, badges, tabs, tables, alerts, avatars, skeletons, empty states, toasts and navigation belong in shared/design-system when reused.

# 4. FEATURE COMPONENTS — P1
PaymentMethodCard, DepositStatusCard, MobileMoneyNetworkSelector, CryptoCurrencySelector and similar components remain feature-owned unless proven broadly reusable.

# 5. FINANCIAL UI — P0
Clearly label USD. Distinguish estimated payment from confirmed credit. Show original deposit currency separately. Never display a client-computed balance as authoritative.

# 6. DANGEROUS ACTIONS — P0
Admin financial actions use explicit confirmation, consequence text and permission checks. Buttons must not imply completion before the server confirms it.

# 7. STATES — P1
Every asynchronous component defines loading, empty, success, error and pending/review states. Payment states map to documented domain states.

# 8. ACCESSIBILITY — P2
Keyboard navigation, labels, focus management, sufficient contrast, semantic controls and useful validation messages are required quality targets.

# 9. RESPONSIVENESS — P1
Player UI is mobile-first. Admin tables remain usable on small screens through responsive layouts, horizontal scroll or purpose-built mobile views.

# 10. DESIGN CHANGE RULE — P0
A local feature change must not silently redesign unrelated screens or global tokens. Architecture/specification scope and visual scope remain separate.
