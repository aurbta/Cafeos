# CafeOS Deployment Notes

Last audited: 2026-10-22

## Authoritative project targets

- GitHub repository: https://github.com/aurbta/Cafeos
- Default branch: `main`
- Supabase organization: `Cafeos`
- Supabase project ref: `qyozmsrgsvfrhcfkutki`
- Supabase region: `ap-south-1`
- Supabase API URL: `https://qyozmsrgsvfrhcfkutki.supabase.co`
- Vercel team: `yashgoyal98370-3979's projects`
- Vercel project: `cafeos` (`prj_aqO5Zg8kFdSGbq6gawjbsgyQ1ubP`)
- Vercel framework: Next.js; configured Node runtime: `24.x`
- Vercel domains reported by the project API:
  - https://cafeos-yashgoyal98370-3979s-projects.vercel.app
  - https://cafeos-git-main-yashgoyal98370-3979s-projects.vercel.app

## Current deployment state

The latest deployment record observed is ERROR for commit `a2854c4e84a709a421b7ee0110cc20c57fd32fc3`, commit message `Import CafeOS Phase 00–03 application foundation`. A prior deployment for commit `1233bf8a679866341cf02e896d162b4b4cc6ba23` is READY. The Vercel project reports `live: false`; therefore the app must not be described as successfully launched. The error must be inspected and corrected in a later implementation/deployment phase. Do not assume the READY deployment contains the current repository head.

## Environment variable inventory

Browser-safe names in `.env.example`:
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
- `NEXT_PUBLIC_SITE_URL`
- `NEXT_PUBLIC_APP_URL`

Server-only names in `.env.example`:
- `CAFEOS_PLATFORM_ADMIN_TOKEN`
- `CAFEOS_RATE_LIMIT_SECRET`
- `SUPABASE_SECRET_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- Optional integration placeholders: `AI_PROVIDER`, `AI_API_KEY`, `WHATSAPP_PROVIDER`, `WHATSAPP_ACCESS_TOKEN`, `WHATSAPP_PHONE_NUMBER_ID`

The example file contains blank values for these variables. The connected Vercel tools have not verified that required variables are present and correctly scoped. Secret values must not be pasted into chat, logged, or committed. Never expose privileged Supabase keys under a `NEXT_PUBLIC_*` name.

## Supabase state

Migration history contains Phase 02 authentication/tenant security migrations and Phase 03 branch/table/floor-plan/QR migrations. The project is ACTIVE_HEALTHY. The inspected public table listing showed RLS enabled on core operational tables and zero rows for the listed tables. This does not replace a complete schema, RLS, and authenticated end-to-end audit.

## Phase 00 decisions and guardrails

- Use only `aurbta/Cafeos`, Supabase organization/project `Cafeos`, and Vercel project `cafeos` for subsequent work.
- Temporary Vercel URL first; custom domain later.
- No production release until the build error, required environment variables, security settings, and authenticated workflows are verified.
- Do not create test restaurant/customer records in a live database without an explicit need and a controlled cleanup plan.
- Never commit secrets or change production configuration as a workaround for a failing build.

## Next verification steps

1. Inspect the failed Vercel deployment's build/runtime logs and correct the root cause in a dedicated GitHub branch.
2. Run CI against the current repository head: locked install, lint, type-check, unit tests, and production build.
3. Confirm Vercel's Git integration is connected to `aurbta/Cafeos` and deploying the intended `main` commit.
4. Verify required environment variable presence and scope without exposing values.
5. Run safe browser smoke tests for the CafeOS landing page, Restaurant ID login, platform-admin gate, protected tenant routes, branch/table editor, and QR flow.
6. Keep production claims and customer-facing operational enablement blocked until those checks pass.
