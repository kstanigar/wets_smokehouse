# TECH STACK - WET SMOKEHOUSE BBQ
**Last Updated:** 2026-04-08
**Purpose:** Document technology choices and rationale

---

## 🎯 TL;DR

**Launch Stack:** Next.js + TypeScript + Supabase + Vercel + SendGrid
**Monthly Cost:** $0 (all free tiers)
**Post-Launch:** Add Twilio (SMS) + Square (payments) when approved

---

## 📅 RECENT UPDATES (Newest First)

### 2026-04-08: Launch Stack Finalized
- Email-only VIP blasts (SendGrid)
- SMS planned for Phase 6 (Twilio)
- Payments planned for Phase 5 (Square primary, Stripe secondary)
- All free tiers at launch ($0/month)

---

## 🛠️ LAUNCH STACK (Phases 1-4)

### Frontend: Next.js 14 + TypeScript + Tailwind CSS

**Why Next.js?**
- Server-side rendering (SEO benefits)
- Built-in API routes (no separate backend needed)
- Vercel optimized (easy deployment)
- React Server Components (better performance)

**Why TypeScript?**
- Type safety (catch bugs before runtime)
- Better IDE support (autocomplete, refactoring)
- Easier to maintain as project grows
- Required for Supabase client types

**Why Tailwind CSS?**
- Already in Munchos template
- Fast to style
- Mobile-first approach built-in
- Small bundle size (only used classes)

**Alternatives Considered:**
- ❌ Plain React: Need SSR for menu SEO
- ❌ Gatsby: Overkill for small site
- ❌ Vue/Svelte: Owner may hire React devs later

---

### Database: Supabase (PostgreSQL)

**Why Supabase?**
- Free tier: 500MB database, 1GB storage (plenty for this project)
- Real PostgreSQL (not NoSQL like Firebase)
- Built-in auth (magic links)
- Row-Level Security (RLS) for data protection
- Real-time subscriptions (menu updates)
- Built-in storage (menu photos)
- Excellent TypeScript support

**Free Tier Limits:**
- 500MB database space (thousands of orders)
- 1GB file storage (hundreds of menu photos)
- 2GB bandwidth/month (enough for weekend traffic)
- 50K monthly active users (way more than needed)

**Upgrade Trigger:**
- Pro tier ($25/month) when going full-time or >500MB data

**Alternatives Considered:**
- ❌ Firebase: NoSQL harder to query, worse free tier
- ❌ MongoDB Atlas: Free tier too limited (512MB)
- ❌ Self-hosted PostgreSQL: Too complex for owner to manage

**Database Schema:** See [ARCHITECTURE.md](./ARCHITECTURE.md)

---

### Hosting: Vercel (Free Tier)

**Why Vercel?**
- Free tier: Unlimited deployments, 100GB bandwidth
- Best Next.js support (same company)
- Automatic HTTPS
- GitHub integration (auto-deploy on push)
- Edge network (fast globally)
- Serverless functions (no server to manage)

**Free Tier Limits:**
- 100GB bandwidth/month (plenty)
- 6,000 build minutes/month
- Unlimited serverless function invocations

**Upgrade Trigger:**
- Pro tier ($20/month) when traffic >100GB or need better analytics

**Alternatives Considered:**
- ❌ Netlify: Good, but Vercel better for Next.js
- ❌ AWS Amplify: Too complex
- ❌ DigitalOcean: Need to manage servers

---

### Email: SendGrid (Free Tier)

**Why SendGrid?**
- Free tier: 100 emails/day (perfect for weekend business)
- Industry standard (high deliverability)
- Good documentation
- Email templates
- Analytics (open rates, click rates)

**Free Tier Limits:**
- 100 emails/day (enough for VIP blasts)
- 2,000 contacts
- 14-day email activity history

**Upgrade Trigger:**
- Essentials plan ($15/month) if VIP list >100 and sending daily

**Alternatives Considered:**
- ❌ Mailgun: Free tier only 5,000/month (need daily limits)
- ❌ AWS SES: Harder to set up, requires credit card
- ❌ Resend: Newer, less proven

---

### Version Control: GitHub (Free)

**Why GitHub?**
- Free private repos
- Vercel integration (auto-deploy)
- Industry standard
- Good for future collaboration

**Alternatives Considered:**
- ❌ GitLab: Good but less common
- ❌ Bitbucket: Less integrated with Vercel

---

### Operations View: Google Sheets API (Free)

**Why Google Sheets?**
- Owner already knows how to use it
- Works on phone
- Free forever
- Easy checkbox confirmations
- Can export data if needed

**Use Cases:**
- View VIP signups (read-only)
- View orders (when payments added)
- Confirm orders (checkbox → webhook → Supabase)

**Alternatives Considered:**
- ❌ Airtable: Better features but costs money
- ❌ Custom admin dashboard: Too complex for owner

---

## 🚀 POST-LAUNCH STACK (Phases 5-6)

### SMS: Twilio (Phase 6 - When Approved)

**Why Twilio?**
- Industry standard for SMS
- Pay-per-message ($0.0079/SMS)
- No monthly fee
- Excellent documentation
- Supports MMS (photos)

**Cost:**
- $0.0079 per SMS
- 50 VIPs weekly = ~$1.58/month
- 100 VIPs weekly = ~$3.16/month

**Alternatives Considered:**
- ❌ AWS SNS: Cheaper but harder to use
- ❌ Vonage: Similar price, less popular

**Implementation:** Phase 6 when owner approves

---

### Payments: Square (Phase 5 - Primary)

**Why Square First?**
- Owner already has Square account
- No monthly fee
- 2.9% + $0.30 per transaction (same as Stripe)
- Good API documentation
- Supports in-person payments (future catering events)

**Cost:**
- $0/month base fee
- 2.9% + $0.30 per transaction

**Alternatives:**
- Stripe (secondary fallback if Square doesn't work)

**Implementation:** Phase 5 when owner approves payments

---

### Payments: Stripe (Phase 5 - Secondary)

**Why Stripe as Backup?**
- More developer-friendly API
- Better documentation
- More features (subscriptions, etc.)
- Same pricing as Square

**Cost:**
- $0/month base fee
- 2.9% + $0.30 per transaction

**When to Use:**
- If Square integration has issues
- If owner wants subscription features
- If need more advanced payment features

**Implementation:** PaymentProcessor interface allows easy swap

---

## 🔮 FUTURE CONSIDERATIONS (Post-Full-Time)

### Analytics: Vercel Analytics (Free) or Plausible ($9/month)

**When to Add:**
- Going full-time
- Want to understand customer behavior
- Track menu item popularity

---

### Error Tracking: Sentry (Free Tier)

**When to Add:**
- After launch if bugs occur
- Free tier: 5K errors/month

---

### Queue System: Inngest (Free Tier)

**When to Add:**
- VIP list >500 people
- Need to avoid email rate limits
- Want to schedule blasts

---

### Image Optimization: Cloudinary (Free Tier)

**When to Add:**
- Uploading lots of photos
- Need automatic resizing
- Free tier: 25GB/month

---

## 💰 COST SUMMARY

### Launch (Phases 1-4)
```
Supabase Free:  $0/month
Vercel Free:    $0/month
SendGrid Free:  $0/month
GitHub Free:    $0/month
Google Sheets:  $0/month
─────────────────────────
TOTAL:          $0/month
```

### Post-Launch (Phase 5-6)
```
Base Stack:     $0/month
Twilio SMS:     $2-5/month (pay-per-use)
Square:         $0/month (2.9% per transaction)
─────────────────────────
TOTAL:          $2-5/month + transaction fees
```

### Full-Time Operation (Future)
```
Supabase Pro:   $25/month
Vercel Pro:     $20/month
SendGrid/Twilio: $5-15/month
Square/Stripe:  Transaction fees only
─────────────────────────
TOTAL:          $50-60/month + transaction fees
```

---

## 🔄 CHANGE LOG

### 2026-04-08
- Finalized launch stack (all free tiers)
- Added SendGrid for email VIP blasts
- Planned Twilio for Phase 6 (SMS)
- Planned Square for Phase 5 (payments)
- Documented cost breakdown

---

*Next Update: When tech stack changes or new tools added*