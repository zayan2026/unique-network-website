# UNIQUE NETWORK PROVIDING LIMITED — Deployment Guide (বাংলা)

## Website pages
- `index.html` — public professional landing page and package cards
- `/login` — customer login (Pages routing: deploy login.html as login/index.html or add redirects)
- `/signup` — customer registration
- `/admin/login` — separate admin login
- `/dashboard` and `/admin/dashboard` — customer/admin portal

## Cloudflare Pages + D1 setup
1. Upload this project to a Git repository and connect it to Cloudflare Pages. Pages Functions need Git integration or Wrangler; drag-and-drop static deployment alone will not run Functions.
2. Create a Cloudflare D1 database. Set its database ID in `wrangler.toml`.
3. Apply schema: `wrangler d1 execute unique-network-db --remote --file=schema.sql`
4. Set secret: `wrangler secret put ADMIN_BOOTSTRAP_SECRET` and enter a long random secret.
5. Deploy with Wrangler: `wrangler pages deploy .` (or configure Pages build output directory as project root).
6. Initialize administrator once, using POST `/api/bootstrap-admin` with header `X-Bootstrap-Secret` and JSON `{ "name":"Network Admin", "phone":"01819601269", "password":"Use-a-unique-12+-character-password" }`. Do not share the secret. Admin password is PBKDF2 hashed.

## URL routing note
For Cloudflare Pages clean paths, add redirects in `_redirects`:
```
/login /login.html 200
/signup /signup.html 200
/dashboard /dashboard.html 200
/admin/login /admin-login.html 200
/admin/dashboard /admin-dashboard.html 200
```

## Contact and package information
Address: Uthli, Katrasin, Shibaloy, Manikganj. Support: 01819601269. Office: 01626243110. bKash/Nagad/Rocket: 01819601269.
Packages: NO Complain 10 Mbps ৳400; NO Complain 20 Mbps ৳450; NO Complain 2 6 Mbps ৳400; Bojro 30 Mbps ৳500; Gorjon 35 Mbps ৳525; Durbar 40 Mbps ৳600; Proloy 50 Mbps ৳650; Tornado 60 Mbps ৳700; Ghurni 80 Mbps ৳800; Jhor 100 Mbps ৳1000; Typhoon 120 Mbps ৳1200; Thunder 150 Mbps ৳1500.

**Production note:** API includes registration, login/session, admin bootstrap, order status and manual payment status endpoints. The admin dashboard UI is a shell; operational order listing controls need connecting to `/api/admin/orders`. Test on a staging database before launch, configure Cloudflare security/backup and rotate secrets.
