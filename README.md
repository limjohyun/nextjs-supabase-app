<h1 align="center">nextjs-supabase-app</h1>

<p align="center">
 Next.js (App Router) + Supabase Auth 스타터 킷
</p>

<p align="center">
  <a href="#features"><strong>Features</strong></a> ·
  <a href="#clone-and-run-locally"><strong>Clone and run locally</strong></a> ·
  <a href="#project-docs"><strong>Project docs</strong></a> ·
  <a href="#deploy-to-vercel"><strong>Deploy to Vercel</strong></a>
</p>
<br/>

## Features

- Works across the entire [Next.js](https://nextjs.org) stack (App Router, Client/Server Components, `proxy.ts` for session refresh)
- [`@supabase/ssr`](https://supabase.com/docs/guides/auth/server-side/nextjs) to configure Supabase Auth to use cookies, with separate client factories for browser/server/proxy (see `lib/supabase/`)
- Password-based authentication pages under `app/auth/*`, installed via the [Supabase UI Library](https://supabase.com/ui/docs/nextjs/password-based-auth)
- `app/instruments/page.tsx` — an example Server Component page that fetches directly from a Supabase table
- Styling with [Tailwind CSS v3](https://tailwindcss.com) + [shadcn/ui](https://ui.shadcn.com/) (`new-york` style)
- Optional deployment with [Supabase Vercel Integration and Vercel deploy](#deploy-to-vercel)
  - Environment variables automatically assigned to Vercel project

## Clone and run locally

1. You'll need a Supabase project, which can be made [via the Supabase dashboard](https://database.new)

2. Clone this repository and install dependencies:

   ```bash
   git clone https://github.com/limjohyun/nextjs-supabase-app.git
   cd nextjs-supabase-app
   npm install
   ```

3. Create a `.env.local` file in the project root with:

   ```env
   NEXT_PUBLIC_SUPABASE_URL=[INSERT SUPABASE PROJECT URL]
   NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=[INSERT SUPABASE PROJECT API PUBLISHABLE OR ANON KEY]
   ```

   > [!NOTE]
   > This example uses `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`, which refers to Supabase's new **publishable** key format.
   > Both legacy **anon** keys and new **publishable** keys can be used with this variable name during the transition period. Supabase's dashboard may show `NEXT_PUBLIC_SUPABASE_ANON_KEY`; its value can be used in this example.
   > See the [full announcement](https://github.com/orgs/supabase/discussions/29260) for more information.

   Both values can be found in [your Supabase project's API settings](https://supabase.com/dashboard/project/_?showConnect=true).

4. Run the dev server:

   ```bash
   npm run dev
   ```

   The app should now be running on [localhost:3000](http://localhost:3000/).

5. This project comes with the default shadcn/ui style (`new-york`) initialized. If you want a different style, delete `components.json` and [re-install shadcn/ui](https://ui.shadcn.com/docs/installation/next).

> Check out [the docs for Local Development](https://supabase.com/docs/guides/getting-started/local-development) to also run Supabase locally.

## Project docs

- [`CLAUDE.md`](./CLAUDE.md) — architecture and conventions notes for Claude Code / AI coding agents working in this repo.
- [`docs/guides/`](./docs/guides) — supplementary dev guides: component patterns, styling conventions, and Next.js App Router notes.

## Deploy to Vercel

1. Import this repository into [Vercel](https://vercel.com/new) (Framework Preset `Next.js` is auto-detected; keep the default build command).
2. Add the same two environment variables from step 3 above under **Project Settings → Environment Variables**.
3. Deploy.

## Feedback and issues

File feedback and issues on [this repository](https://github.com/limjohyun/nextjs-supabase-app/issues). For issues with the underlying Supabase Auth starter template itself, see the [Supabase GitHub org](https://github.com/supabase/supabase/issues/new/choose).

## More Supabase examples

- [Next.js Subscription Payments Starter](https://github.com/vercel/nextjs-subscription-payments)
- [Cookie-based Auth and the Next.js App Router (free course)](https://youtube.com/playlist?list=PL5S4mPUpp4OtMhpnp93EFSo42iQ40XjbF)
- [Supabase Auth and the Next.js App Router](https://github.com/supabase/supabase/tree/master/examples/auth/nextjs)
