# SubTrack — Subscription Manager

A responsive subscription tracker with a light theme, purple accents, CSV statement scanning, Supabase login/sign-up, a profile page and persistent login sessions.

## What's included

- Login and sign-up page (`html/login.html`).
- Supabase email/password authentication.
- Persistent login sessions: Supabase keeps the session when the browser is refreshed or reopened. Users can log out from their profile.
- Protected dashboard (`html/index.html`) and profile page (`html/profile.html`).
- Profile displays the user's name, email and account creation date.
- Subscription data is stored in browser localStorage under a separate key for each signed-in user.
- Dashboard, subscriptions, upcoming payments, analytics and CSV statement scanner.

## Setup: connect Supabase (required for login)

1. Create a free project at [https://supabase.com](https://supabase.com).
2. In your Supabase project, open **Project Settings → API** (or **Data API / API Keys**, depending on the dashboard layout).
3. Copy the **Project URL** and the **anon / publishable key**. Do not use or expose the `service_role` secret key.
4. Open `javascript/supabase-config.js` and replace the two placeholders:

```js
window.SUBTRACK_SUPABASE_CONFIG = {
    url: "https://YOUR-PROJECT.supabase.co",
    anonKey: "YOUR_SUPABASE_ANON_OR_PUBLISHABLE_KEY"
};
```

5. In Supabase, open **Authentication → URL Configuration**. Add your local development URL to the allowed redirect URLs. For example, if Live Server uses port 5500, add:
   - `http://127.0.0.1:5500/html/login.html`
   - `http://localhost:5500/html/login.html`
   If your Live Server uses another port, use that exact port instead.
6. In **Authentication → Providers**, ensure Email is enabled. For a simple test, you can turn off email confirmation in the Email provider settings; otherwise, new users must confirm their email before logging in.

## Run locally

1. Extract the ZIP.
2. Open the extracted folder in VS Code.
3. Install the **Live Server** extension if you do not have it.
4. Right-click `html/login.html` and choose **Open with Live Server**.
5. Create an account, then log in. Once logged in, the dashboard opens. The profile icon/name in the top-right opens the profile page.

Do not open the HTML files by double-clicking them (`file://`). Use Live Server or another local web server. Internet access is needed for Supabase, Google Fonts, Lucide, Chart.js and PapaParse CDNs.

## CSV format

The scanner looks for columns equivalent to `date`, `description` (or `merchant`) and `amount`:

```csv
date,description,amount
2026-08-05,Netflix subscription,199
2026-09-05,Netflix subscription,199
2026-10-05,Netflix subscription,199
2026-08-12,Spotify Premium,119
2026-09-12,Spotify Premium,119
2026-10-12,Spotify Premium,119
```

Column names can also be `transaction date`, `merchant`, `narration`, `details`, or `debit` where appropriate.

## Important current limitations

- Authentication is handled by Supabase, but subscription records are still stored locally in the current browser. They are separated by signed-in user on that browser, but are **not yet synced to Supabase or across devices**.
- CSV files are scanned in the browser; this demo does not upload bank statements to a server or connect to a bank.
- CSV detection is heuristic. It can miss recurring payments or suggest false matches. Review matches before saving.
- The spending chart uses illustrative trend variation based on the current monthly total; it is not historical transaction analysis.
- This is a learning-project MVP, not a financial service.

## Next backend step

The next stage is to create a `subscriptions` table in Supabase with a `user_id` column and Row Level Security policies, then replace localStorage operations with Supabase database calls. That will enable secure cloud sync across devices.
