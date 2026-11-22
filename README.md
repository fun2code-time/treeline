# Arcadia

Arcadia is an original player-created worlds platform starter. It is not affiliated with Roblox, Mojang, or another game platform.

## Current milestone

Implemented in this repository:
- Responsive Arcadia discovery homepage and original visual identity.
- Playable Three.js Crystal Crossing sample with keyboard movement, jumping, camera follow, and collectible crystals.
- Persistent SQLite accounts with bcrypt password hashing and signed HTTP-only session cookies.
- Owner-only server-checked controls for payment availability and maintenance mode.
- Payment availability is persisted in SQLite and recorded in an audit log.
- Stripe Checkout scaffold with a signature-verified webhook and duplicate-credit guard.
- Server-side daily free-coin reward with a currency ledger.

Not implemented yet:
- Production multiplayer synchronization and authoritative game servers.
- User-generated game publishing and Creator Studio.
- Avatar editor, inventory, creator marketplace, friends, messaging, and parties.
- Full moderation, reports, appeals, admin role/permission editor, and admin 2FA.
- Email verification, password reset, subscription lifecycle, refund tooling, and full revenue reports.
- Automated test suite, production deployment, backups, and observability.

The featured game cards are sample content, not live published games. The 3D sample is a local single-player prototype, not network multiplayer.

## Requirements and local setup

- Node.js 20+
- npm
- Optional Stripe account for test-mode checkout

1. Clone this repository and enter its folder.
2. Copy `.env.example` to `.env`.
3. Set `JWT_SECRET` to a random secret at least 32 characters long.
4. Set `OWNER_EMAIL` to the email that should receive the initial owner role.
5. Install dependencies and start the server:

```bash
npm install
npm run dev
```

Open http://localhost:3000.

The account registered using the configured `OWNER_EMAIL` is assigned the OWNER role. Configure this before creating accounts. For a public deployment, replace this bootstrap approach with a one-time, operator-controlled owner provisioning command and protect the owner account with MFA.

## Stripe test mode

Set `STRIPE_SECRET_KEY` to a Stripe test secret and `APP_URL` to the app's public URL. Configure a webhook endpoint at `/api/stripe/webhook` and set `STRIPE_WEBHOOK_SECRET`. Subscribe to `checkout.session.completed`.

For local webhook forwarding, install and use the Stripe CLI:

```bash
stripe listen --forward-to localhost:3000/api/stripe/webhook
```

Payments are disabled by default. Sign in as the configured owner, click the account circle, enter a reason, and enable payments from the owner console. The backend checks the payment setting before creating a Checkout session. Use Stripe test cards only during development.

## Before public launch

This is an early development foundation, not a production-ready platform. Add CSRF protection and origin validation, email verification, password reset, login throttling/account lockout, administrator MFA, secure owner provisioning, persistent payment-order records, strict webhook amount/currency/product validation, refund reconciliation, a production database with backups, automated tests, and server-authoritative multiplayer simulation. Use HTTPS and secure cookie settings in production. Never commit `.env` or payment secrets.
