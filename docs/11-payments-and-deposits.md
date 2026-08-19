# PAYMENTS AND DEPOSITS

## Supported launch methods

Cameroon launch:

- MTN Mobile Money;
- Orange Money;
- cryptocurrency through the configured provider.

Provider integrations must be adapter-based.

## Payment states

A deposit should use explicit states such as CREATED, PENDING, CONFIRMED, PROCESSING, CREDITED, FAILED, EXPIRED, CANCELLED or REVERSED as required by the provider flow.

## Critical rule

A browser redirect or client callback is not sufficient proof of payment. The server must rely on a trusted provider response/webhook or controlled administrative confirmation according to the provider integration.

## Provider event handling

Validate signature/authentication where provided. Extract provider payment ID and event ID. Check whether the event has already been processed. Persist the event/payment state before performing financial mutation where appropriate. Then perform one atomic USD wallet credit.

## Cameroon mobile money

The UI must make it clear that MTN Mobile Money and Orange Money are Cameroon-specific payment options. The architecture must allow country/payment-method availability to be configuration-driven.

## Crypto

The crypto adapter must preserve asset, network where relevant, expected amount, actual amount, provider invoice/payment ID, transaction/hash reference where available, confirmation status and final USD conversion information.

Do not credit based only on an address being displayed. Use the provider's confirmed payment state.

## Fees

Provider fees, platform fees and network fees must be represented explicitly if charged. Never hide fees by silently changing the credited amount.

## Deposit records

Every deposit must retain enough data to reconstruct why and how much USD was credited, including original amount, asset/currency, rate, rate source, timestamps, provider reference and resulting wallet transaction.

## Reconciliation

Provider records and BG Arena deposit records must be reconcilable by provider payment ID/event ID and internal deposit ID.
