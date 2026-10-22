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
- Vercel framework: Next.js; configured Node runtime: `22.x`
- Vercel domains reported by the project API:
  - https://cafeos-yashgoyal98370-3979s-projects.vercel.app
  - https://cafeos-git-main-yashgoyal98370-3979s-projects.vercel.app

## Current deployment state

The original failed deployment returned `STATIC_BUILD_NO_OUT_DIR` because Vercel was configured to expect a static output directory named `public`, despite the application using Next.js server-rendered routes and middleware. This has been corrected in the Vercel project settings: framework remains Next.js, the custom output directory is cleared for automatic detection, and build/install commands are reset to automatic detection.

A fresh deployment from `aurbta/Cafeos` branch `main`, commit `18925fdde3c5e37c5ac9444e175e8db4e36937e2` (`docs: record CafeOS deployment configuration fix`) reached Vercel state **READY**. Build logs confirm successful Next.js compilation, type/lint validation, 17 generated static pages, serverless function creation, and completed deployment output. Verified URLs:
- Landing page: https://cafeos-peach.vercel.app — HTTP 200
- Login route: https://cafeos-peach.vercel.app/login — HTTP 200
- Admin route: https://cafeos-peach.vercel.app/admin — HTTP 404; no route exists at this path.

This resolves the build/deployment configuration error and verifies that the app shell responds. It does not establish full production readiness or acceptance of Phases 02/03. Required server-only secrets have not been verified; authenticated workflows, invitation/provisioning, and end-to-end browser tests remain separate gates. Do not claim that a complete café operations product is live.
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
- **Verification complete for the build fix:** fresh main-branch deployment `dpl_4purG4wZ4gH1eaUuviGUkGsHRuFo` reached READY on commit `18925fdde3c5e37c5ac9444e175e8db4e36937e2`. The landing page and `/login` returned HTTP 200; `/admin` returned HTTP 404 because no such route exists. Build success does not imply authenticated end-to-end acceptance.
- **Environment variables:** Vercel has the browser-safe app URL, Supabase URL, and publishable key configured for Production/Preview/Development. Secret values are not copied into documentation. Presence of `SUPABASE_SECRET_KEY` or `SUPABASE_SERVICE_ROLE_KEY` and `CAFEOS_PLATFORM_ADMIN_TOKEN` has not been verified; administrator provisioning may remain unavailable until they are set server-side.
- **Security follow-up:** Supabase advisor reports informational no-policy notices on intentionally private service-only tables and warnings for callable SECURITY DEFINER RPCs, including the public QR/session RPCs. Review each function's grants, search path, input validation, and returned data before production. Performance advisor notices unused indexes on the currently low/empty workload; do not remove indexes without workload evidence.
- **Release boundary:** Phase 00 audit and environment inventory are complete. This is not production acceptance. No merge, production promotion, or change to customer data was performed as part of this audit. Phase 02/03 still require their documented acceptance gates and real authenticated/browser tests.
