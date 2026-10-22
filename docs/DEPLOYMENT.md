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

The latest deployment record observed is ERROR for commit `a2854c4e84a709a421b7ee0110cc20c57fd32fc3`, commit message `Import CafeOS Phase 00–03 application foundation`. A prior deployment for commit `1233bf8a679866341cf02e896d162b4b4cc6ba23` is READY. The exact failure is `STATIC_BUILD_NO_OUT_DIR`: Vercel expects an output directory named `public`, despite the build producing Next.js server-rendered routes and middleware. This strongly indicates a static-build/output-directory configuration mismatch. For the Next.js project, remove the custom `public` output-directory setting and use the Next.js framework preset/default output handling, then redeploy and verify. No Vercel setting was changed during Phase 00. The Vercel project reports `live: false`; therefore the app must not be described as successfully launched. The error must be inspected and corrected in a later implementation/deployment phase. Do not assume the READY deployment contains the current repository head.

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


## Phase 00 environment audit refresh — 2026-10-22

This section supersedes the older target inventory above where repository, Supabase, or Vercel identifiers differ.

- **Canonical GitHub repository:** https://github.com/aurbta/Cafeos — default branch `main`. Vercel confirms the current project is linked to this repository and the deployment source commit is `a2854c4e84a709a421b7ee0110cc20c57fd32fc3` (message: “Import CafeOS Phase 00–03 application foundation”).
- **Canonical Supabase organization/project:** organization `Cafeos` (`sxecsiglqoqkamwsqldr`); project `Cafeos` (`qyozmsrgsvfrhcfkutki`), region `ap-south-1`, status `ACTIVE_HEALTHY`. Remote migration history contains the Phase 02 authentication/RLS and Phase 03 branch/table/floor-plan/QR migrations through `phase_03_parent_status_invariants`. Public tables exist with RLS enabled; no business/customer records were found in the inspected core tables.
- **Canonical Vercel project:** `cafeos` (`prj_aqO5Zg8kFdSGbq6gawjbsgyQ1ubP`) in the connected Hobby team. Project domains include `https://cafeos-yashgoyal98370-3979s-projects.vercel.app` and `https://cafeos-git-main-yashgoyal98370-3979s-projects.vercel.app`.
- **Deployment root cause found and corrected:** the previous deployment failed with `STATIC_BUILD_NO_OUT_DIR` because Vercel expected a static `public` output directory despite this being a Next.js App Router application. Project framework remains Next.js; the forced output directory was reset to automatic detection, build/install commands reset to automatic detection, root directory set to repository root, and Node.js set to `22.x`.
- **Verification in progress:** a fresh deployment was started from `aurbta/Cafeos` branch `main`, commit `a2854c4`, using the corrected settings. Its URL is https://cafeos-57vbhewcw-yashgoyal98370-3979s-projects.vercel.app. Follow-up verification confirms Vercel reports `READY` and the build completed successfully. The deployment is tied to source commit `a2854c4e84a709a421b7ee0110cc20c57fd32fc3`. Browser smoke tests and authenticated end-to-end acceptance have not yet been completed, so READY means the deployment build succeeded, not that CafeOS is production-accepted. The previous failed deployment URL must not be presented as live.
- **Environment variables:** Vercel has the browser-safe app URL, Supabase URL, and publishable key configured for Production/Preview/Development. Secret values are not copied into documentation. Presence of `SUPABASE_SECRET_KEY` or `SUPABASE_SERVICE_ROLE_KEY` and `CAFEOS_PLATFORM_ADMIN_TOKEN` has not been verified; administrator provisioning may remain unavailable until they are set server-side.
- **Security follow-up:** Supabase advisor reports informational no-policy notices on intentionally private service-only tables and warnings for callable SECURITY DEFINER RPCs, including the public QR/session RPCs. Review each function's grants, search path, input validation, and returned data before production. Performance advisor notices unused indexes on the currently low/empty workload; do not remove indexes without workload evidence.
- **Release boundary:** Phase 00 audit and environment inventory are complete. This is not production acceptance. No merge, production promotion, or change to customer data was performed as part of this audit. Phase 02/03 still require their documented acceptance gates and real authenticated/browser tests.
