# PPSU ERP — Vercel Ready

This version keeps the **same login and student dashboard output/UI** as the supplied PHP project, but moves the backend to **Next.js + Turso/libSQL**, which is a better fit for Vercel.

## Recommended production stack
- Frontend/backend: Next.js
- Hosting: Vercel
- Database: Turso (libSQL / SQLite-compatible)
- Authentication: secure HTTP-only signed session cookie + bcrypt password hashes
- Images: existing assets in `public/assets` (for user-uploaded images, add Vercel Blob or Supabase Storage later)

## Local run
1. Install Node.js 20+.
2. Copy `.env.example` to `.env.local`.
3. Keep `TURSO_DATABASE_URL=file:./data/ppsu.db` for local development.
4. Run `npm install`.
5. Run `npm run dev`.
6. Open `http://localhost:3000`.
7. Create the first admin with the bootstrap endpoint using the `ADMIN_SETUP_KEY` from `.env.local`:
   `curl -X POST http://localhost:3000/api/admin/bootstrap -H "Content-Type: application/json" -d '{"key":"change-this-setup-key","username":"admin","password":"YOUR_STRONG_PASSWORD"}'`
8. Open `/admin/login`.

## Vercel + Turso deployment
1. Create a Turso database and obtain its database URL and auth token.
2. Push this folder to GitHub.
3. Import the GitHub repository into Vercel.
4. In Vercel Project Settings → Environment Variables, add:
   - `TURSO_DATABASE_URL`
   - `TURSO_AUTH_TOKEN`
   - `SESSION_SECRET` (long random secret)
   - `ADMIN_SETUP_KEY` (one-time bootstrap secret)
5. Redeploy.
6. Run the bootstrap request once against your deployed domain, then delete/rotate `ADMIN_SETUP_KEY` if desired.
7. Open `/admin/login` and create student accounts.

## Important
- The visual output is intentionally preserved: login slideshow, fields, buttons, warning, store badges, dashboard cards, fee table, mobile breakpoint, fonts and Font Awesome icon choices.
- Do not use a local SQLite file on Vercel production. Use Turso for persistent data.
- Do not store production passwords in source code.
