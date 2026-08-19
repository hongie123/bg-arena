# NOTIFICATIONS

## Channels

Primary channels are in-app notifications and email where configured. SMS is not an authentication requirement and is not required for the core platform.

## Events

Notifications may be generated for registration, payment, deposit credit, competition changes, results, winnings, withdrawal requests/status changes, support responses and administrative announcements.

## Reliability

Financial events should not depend on notification delivery to complete. The financial transaction succeeds independently; notification delivery is a secondary process.

## Idempotency

The same domain event should not create unlimited duplicate notifications when retried.

## Preferences

Future user notification preferences should be stored separately from core financial state.
