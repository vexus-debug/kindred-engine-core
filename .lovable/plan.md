# Split domains: app dashboard vs public clinic sites

Goal: the SaaS dashboard lives at `app.clinexus.com.ng`, while clinic public sites are reached at `clinexus.com.ng/site/<clinic-slug>`.

Yes, this is doable. Both hostnames serve the same published app, and the app decides what to show based on which hostname it was opened on.

## How it works

```text
clinexus.com.ng/site/bright-dental     -> public clinic website (allowed)
clinexus.com.ng/clinic/.../dashboard   -> redirect to app.clinexus.com.ng/...
app.clinexus.com.ng/                   -> login / dashboard (allowed)
app.clinexus.com.ng/site/bright-dental -> redirect to clinexus.com.ng/site/...
```

## Domain setup (in Lovable, no code)

1. Publish the project.
2. In Project Settings > Domains, connect both `clinexus.com.ng` (root, plus `www`) and `app.clinexus.com.ng`.
3. Both domains point to the same deployment, so no duplicate project or second codebase is needed.

## Code changes

1. Add a host helper (`src/lib/domains.ts`) that knows the two hostnames, exposes `isAppHost()` / `isPublicHost()`, and builds absolute URLs: `publicSiteUrl(slug)` -> `https://clinexus.com.ng/site/<slug>`, `appUrl(path)` -> `https://app.clinexus.com.ng<path>`. On preview/localhost and any unknown host both checks return true, so nothing redirects during development.
2. Add a `HostRouter` guard rendered inside the router in `src/App.tsx`:
   - on the app host, any `/site/...` path redirects (full page redirect) to the same path on the root domain;
   - on the root/public host, any non-`/site/...` path (login, signup, `/clinic/*`, `/admin/*`, `/select-clinic`) redirects to the same path on `app.clinexus.com.ng`.
3. Replace the two `window.location.origin` share links with `publicSiteUrl(slug)` so copy/share/preview links in `WebsiteSettingsPage.tsx` and `SettingsPage.tsx` always hand out the root-domain URL.
4. Point the signup `emailRedirectTo` at `appUrl('/select-clinic')` so confirmation emails land on the dashboard host.

Internal `<Link>` navigation inside the public site pages stays relative, so it keeps working on whichever host serves it.

## Notes

- Today the root domain would show the login page at `clinexus.com.ng/`; with this change it redirects to the app host. A marketing landing page there instead is a separate piece of work.
- Per-clinic subdomains (`bright-dental.clinexus.com.ng`) would need wildcard DNS and a different setup; this plan implements the path-based version you asked for.