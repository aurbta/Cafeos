# CafeOS

India-first, multi-tenant café management SaaS. Development is phased; see docs/BUILD_STATUS.md.

## Engineering baseline
- Next.js App Router, React, TypeScript, Tailwind CSS
- Supabase PostgreSQL, Auth, Realtime, and Storage
- INR-first commerce with cash and merchant UPI workflows
- Tenant- and branch-scoped authorization, RLS, and audit trails
- Offline-aware PWA plus a separately deployed local-network service (planned; not implemented)
- Provider adapters for AI, WhatsApp, accounting, maps, delivery, and hardware

## Security rules
- Never commit secrets, access tokens, service-role keys, production data, or real employee/customer records.
- Never put privileged keys in NEXT_PUBLIC_* variables.
- All tenant-owned data must be authorized server-side and protected with RLS.
- Payment claims, screenshots, or client events do not constitute trusted settlement confirmation.
- AI output cannot independently perform sensitive financial or inventory actions.

## Local development
- Node.js 20.19+ and npm
- Copy .env.example to .env.local and fill only values for services configured in your environment.
- Run npm install, npm run dev, npm run lint, npm run typecheck, npm run test, and npm run build as applicable.
- Database migrations belong in supabase/migrations/ and must be committed.
