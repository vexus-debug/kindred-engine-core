# Marketing Section for the Clinic Dashboard (UI only)

Add a new "Marketing" area to the clinic dashboard with its own sidebar group and sub-pages. This phase is **interface only** — screens, layouts, tables, dialogs and charts built with realistic mock data. No database tables, no email/SMS providers, no AI calls yet. Everything is structured so real sending and data can be wired in later without redesigning.

## Sidebar and pages

New "Marketing" group in the clinic sidebar (owner/admin, plus receptionist on reviews/referrals), with these pages under `/clinic/:slug/marketing/...`:

1. **Marketing Overview** — hub with KPI cards (campaigns sent, open/click rate, new patients from marketing, review score, promo redemptions), upcoming scheduled items, quick actions.
2. **Email Blasts** — campaign list with status (draft/scheduled/sent), campaign builder (name, audience segment, subject, template picker, body editor, send now vs schedule), preview pane, and a sent-campaign report (opens, clicks, unsubscribes).
3. **SMS Blasts** — same flow tuned for text: 160-character counter, recipient count, cost estimate, opt-out footer, delivery summary.
4. **Social Content Planner** — month/week calendar of posts, per-platform tabs (Instagram, Facebook, TikTok, X, Google Business), post composer with image slot, caption and hashtags, a "generate ideas" button (placeholder for now), draft → needs approval → scheduled → published states, and a library of dental post ideas.
5. **Reviews & Referrals** — review request builder (which patients, channel, timing), review inbox with ratings and reply box, referral program screen (referral codes, who referred whom, reward status), testimonial picks for the public clinic site.
6. **Promotions & Offers** — promo/discount codes, seasonal offer cards (whitening, checkup packages, kids' month), treatment bundles, expiry and usage limits.
7. **Patient Recall & Reactivation** — the segments dental clinics care about: due for six-month recall, no-show follow-up, lapsed 12+ months, treatment plans accepted but unscheduled, birthdays — each with a "start campaign" button that routes into the email or SMS builder.
8. **Marketing Analytics** — funnel (reached → booked → treated), channel comparison, campaign ROI table, source of new patients, best-performing posts.

## Additional dental marketing features, surfaced inside those pages

- Audience segment builder (age, last visit, treatment type, insurance, outstanding balance)
- Automated lifecycle journeys: welcome, post-treatment care, recall reminders, birthday, win-back
- Offer landing pages tied to the existing public clinic site
- Google Business profile posts and review syncing
- Before/after gallery with a consent flag
- Waitlist-fill blast ("we have an opening tomorrow at 2pm")
- QR codes and print assets for in-clinic promotion
- Loyalty / membership plan promotion
- Unsubscribe and consent centre, so compliance is visible from day one

These appear as sections, tabs, or clearly labelled "coming soon" cards so the shape of the product is visible without backend work.

## Technical notes

- New pages under `src/pages/dashboard/marketing/`, shared pieces under `src/components/dashboard/marketing/`.
- Routes registered as nested children of the existing `/clinic/:slug` layout in `src/App.tsx`; sidebar entries added to `dentalNav` in `src/config/clinicTypeConfig.ts`; paths added to `PAGE_ROLE_ACCESS` in `src/config/roleAccess.ts`.
- Mock data in one file per page under `src/data/marketing/`, typed with interfaces that mirror the eventual database tables, so swapping in Supabase queries later is a drop-in change.
- Reuses the existing shadcn UI, `PageHeader`, `EmptyState`, `TableSkeleton`, recharts and current design tokens — no new styling system.
- No migrations, no edge functions, no secrets in this phase.