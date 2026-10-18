# T15 Weplay Auction House — Custom Marketplace Starter

A responsive, static frontend for a community marketplace, backed by Supabase for real authentication, PostgreSQL persistence, and image uploads.

## Included

- Responsive marketplace homepage and listing cards
- Email/password sign-up and login using Supabase Auth
- Persistent listings stored in PostgreSQL
- Create, edit, and delete listings (database row-level security enforces ownership)
- Image uploads to a public Supabase Storage bucket (JPG, PNG, WebP; 5 MB max)
- Search, category filtering, and price/date sorting
- Discord seller contact (copies the seller's Discord username and opens Discord)
- Direct buyer/seller payment arrangement; no payment processing
- GitHub Pages-friendly static files

## Important status note

This package is the website implementation, not a deployed live service. Live database and authentication features remain unavailable until you create and configure a Supabase project and deploy the files. Do not advertise it as production-ready until you test the deployed site with separate buyer and seller accounts.

## 1. Create the Supabase backend

1. Create a project at <https://supabase.com/>.
2. Open **SQL Editor** in the Supabase dashboard.
3. Open `supabase-setup.sql`, copy the complete contents, paste them into SQL Editor, and run it.
4. In **Project Settings → API** (or the project's Connect/API settings), copy the **Project URL** and the **anon/public key**. Depending on the dashboard version, this may be labelled the publishable key.
5. Open `config.js` and replace `YOUR_SUPABASE_PROJECT_URL` and `YOUR_SUPABASE_ANON_PUBLIC_KEY` with those values.
6. Never use the `service_role` key or any secret key in `config.js` or other frontend files. The public anon/publishable key is designed to be used in a browser when Row Level Security policies are correctly configured.
7. In Supabase **Authentication → URL Configuration**, set the Site URL to your eventual GitHub Pages URL, for example `https://YOUR-USERNAME.github.io/t15-weplay-auction-house/`. Add the same URL to the Redirect URLs list. If using a different repository name, use its exact Pages URL.
8. Choose whether to require email confirmation in Supabase Auth settings. If confirmation is enabled, test that your email provider and confirmation redirect work. For a production project, configure a reliable SMTP provider rather than relying on development email limits.

## 2. Test locally

The frontend uses a CDN build of Supabase JS. You can open `index.html` directly for a visual preview, but a local web server is more representative. From this folder, run:

```bash
python -m http.server 8000
```

Then visit <http://localhost:8000>. Configure Supabase first to test live features. If email confirmation is on, confirm the email before signing in.

## 3. Publish free with GitHub Pages

1. Create a GitHub repository, for example `t15-weplay-auction-house`.
2. Upload `index.html`, `styles.css`, `app.js`, `config.js`, `supabase-setup.sql`, and `README.md` to the repository root. Keep the files in the same folder.
3. Open **Settings → Pages** for the repository.
4. Choose **Deploy from a branch**, select your main branch and `/ (root)`, then save.
5. Wait for GitHub Pages to publish. Open the provided URL and test sign-up, email confirmation, login, creating a listing, uploading an image, editing/deleting a listing, search, and Discord contact.
6. Update Supabase Auth Site URL and Redirect URLs to the exact published URL.

GitHub Pages can host the static frontend free for a public repository, subject to GitHub's current plan terms. Supabase has a free tier with usage limits; check current limits and terms before launch.

## 4. Security and limitations

- Database row-level security is enabled. Public visitors can read active listings; authenticated users can create listings and modify/delete only their own rows.
- Storage policies restrict uploads and mutations to each user's own UID folder. The image bucket is public, so uploaded images are viewable by anyone with their URL. Do not upload private documents or sensitive images.
- Client-side form checks improve usability but are not a substitute for backend policies. The SQL schema also applies basic constraints.
- Search and filters operate on the latest 200 active listings fetched by the client. A larger marketplace should move search, pagination, and filtering to server-side queries.
- The site stores a Discord username, not a Discord user ID. The contact action copies the username and opens Discord; the user may need to search/add the seller manually. Discord usernames generally cannot be converted into a guaranteed direct profile URL.
- Payments, escrow, order management, dispute handling, seller reviews, moderation dashboards, automatic image cleanup, rate limiting, and anti-fraud tools are not included.
- Before public launch, test RLS policies with multiple accounts, add reporting/moderation, set community rules, and review prohibited goods and platform terms.

## Troubleshooting

- **Setup required banner:** `config.js` still contains placeholders or a value is malformed.
- **Listings fail to load:** check Supabase URL/key, run the SQL file, and inspect browser console/network errors.
- **Sign-up works but login does not:** check email confirmation and Auth URL settings.
- **Image upload fails:** verify the `listing-images` bucket and storage policies were created by the SQL script; make sure the image is JPG/PNG/WebP and 5 MB or smaller.
- **GitHub Pages shows 404:** confirm Pages is enabled and files are in the configured source folder. For project sites, the URL includes the repository name.
