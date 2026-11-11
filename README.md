# GCCF Feedback Centre

A mobile-responsive Next.js front end and Supabase/Postgres database foundation for Global Christian Corporate Fellowship — “Where Faith Meets Leadership”.

## Included
- Public feedback form; no account required.
- Anonymous and identified submissions.
- Feedback categories, rating and message.
- Four staff roles: Chairman, Administrator/Secretary, Mobilizer and Finance.
- PostgreSQL schema, row-level security policies and restricted public submission RPC.
- Responsive burgundy, gold and cream design.

## Important deployment note
This is a deployable application starter, not a live hosted service. A Supabase project and a hosting project must be created and configured before real submissions can be accepted. Do not advertise the system as live until the deployment checklist is complete.

## 1. Create the database
1. Create a Supabase project in the organisation's own account.
2. Open SQL Editor and run `supabase/migrations/001_initial_schema.sql`.
3. In Project Settings → API, copy the Project URL and anon/public key.
4. Put those values in `.env.local` using `.env.example` as the template.
5. Never place the service-role key in a `NEXT_PUBLIC_` variable or browser code.

## 2. Run locally
- Install Node.js 20 or later.
- Run `npm install`.
- Run `npm run dev`.
- Open `http://localhost:3000`.

## 3. Create staff accounts safely
- Create each staff user in Supabase Authentication using an invitation flow.
- Add their profile using the SQL Editor or a trusted server-side admin process. Example (replace values):
  `insert into public.staff_profiles(user_id,full_name,role) values ('AUTH_USER_UUID','Full Name','chairman');`
- Allowed role values: `chairman`, `administrator_secretary`, `mobilizer`, `finance`.
- Do not expose a public role-registration form.
- Do not share admin passwords in group chats.

## 4. Role and privacy design
- Chairman and Administrator/Secretary can review general feedback.
- Mobilizer access is limited to mobilisation, event and fellowship categories.
- Finance access is limited to Finance category.
- Anonymous submissions store no submitter name/contact in the database.
- Public clients have no table-level read/update/delete access. Intake is via a constrained database function.

## 5. Production hardening required before launch
This repository is a foundation and requires the following before production use:
- Add a real staff login and protected admin pages, with server-side role checks.
- Add rate limiting and bot protection (for example Cloudflare Turnstile plus server-side verification).
- Configure Supabase Auth email/phone policies, MFA for staff, password reset and account recovery.
- Add audit logging for staff actions and restrict access to internal notes.
- Review and test all RLS policies with each role and anonymous sessions.
- Configure backups, point-in-time recovery where available, monitoring and incident response.
- Set data retention, privacy notice, consent wording and contact for privacy requests.
- Use HTTPS-only hosting and managed secrets; rotate keys if exposed.
- Add notification delivery only through trusted server-side code.

## 6. Deploy
Deploy the Next.js project to a hosting provider that supports Next.js (e.g. Vercel). Add the two environment variables from `.env.local` in the hosting dashboard, connect the repository, and deploy. Use a custom domain owned by GCCF and enable HTTPS.

## Data protection
Collect the minimum information needed. Do not ask for sensitive pastoral, medical or financial details in open text unless the organisation has a defined, protected handling process. Anonymous feedback is anonymous by design; do not attempt to identify submitters.
