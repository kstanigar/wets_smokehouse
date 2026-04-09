# PHASE 1: Frontend Transformation
**Duration:** Week 1 (7 days)
**Status:** 🟢 Approved to Start
**Budget:** $0 (using free tiers only)
**Goal:** Transform Munchos → WET SMOKEHOUSE, build admin interface

---

## 🎯 TL;DR

Transform Munchos template to WET SMOKEHOUSE branding
Build simple 3-button admin interface (mobile-first)
Add customer email signup form
Deploy preview to Vercel
Owner can view and update menu from phone

---

## 📅 RECENT UPDATES (Newest First)

### 2026-04-08: Phase Approved
- User approved Phase 1 to begin
- Documentation complete
- Git repo pending initialization
- No blockers

---

## ✅ GOALS

1. Rebrand Munchos template to WET SMOKEHOUSE
2. Build admin interface (3 sections: menu, VIP blast, orders)
3. Add customer email capture form on frontend
4. Connect to Supabase (menu read/write)
5. Deploy preview to Vercel
6. Test on mobile device

**Success Criteria:**
- Owner can update menu from phone
- Customers can sign up for VIP emails
- Changes appear on customer site instantly
- Admin loads in <2 seconds on 4G
- Email signup form saves to Supabase

---

## 📋 TASKS

### Task 1: Git Repository Setup
**Status:** ⚪ Not Started
**Estimate:** 30 minutes
**Approval Required:** No

- [ ] Initialize Git repo in wet_smokehouse/
- [ ] Create .gitignore (node_modules, .env, .DS_Store, etc.)
- [ ] Create initial commit with PROJECT-DOCS/
- [ ] Create feature branch: `feature/phase-1-frontend`
- [ ] Set up remote on GitHub (private repo)
- [ ] Push documentation to remote

---

### Task 2: Munchos → WET SMOKEHOUSE Rebrand
**Status:** ⚪ Not Started
**Estimate:** 3 hours
**Approval Required:** Yes (show before/after screenshots)

**Changes Needed:**
- [ ] Update color scheme:
  - Primary: Gold #d3ab55 → BBQ theme (rustic reds/browns)
  - Get color palette from wet_menu.jpg
- [ ] Replace logo:
  - Current: Pork cuts icon
  - New: BBQ/smokehouse themed
- [ ] Update all text content:
  - "Munchos Smoke & Grill" → "WET SMOKEHOUSE"
  - Update tagline
  - Update About Us section (remove Chef Joe story)
- [ ] Update menu items in gallery:
  - Remove: Filet, Lamb, Rump, etc.
  - Add: Brisket, Ribs, Pulled Pork, Sausage (from wet_menu.jpg)
  - Update prices
- [ ] Replace gallery images:
  - Use images from /wet_bbq/images/ or source new BBQ photos
- [ ] Update fonts if needed (keep simple, readable on mobile)
- [ ] Update favicon
- [ ] Test responsive design on mobile (375px, 768px, 1024px)

**Approval Point:** Show before/after screenshots before proceeding

---

### Task 3: Customer Email Signup Form
**Status:** ⚪ Not Started
**Estimate:** 2 hours
**Approval Required:** Yes (show mockup first)

**Requirements:**
- Location: Footer section (below gallery, above social media)
- Fields:
  - Name (required)
  - Email (required, validated)
  - Phone (optional - for future SMS)
- Design: Simple, mobile-friendly
- Submit button: "Join VIP List"
- Success message: "Thanks! We'll send you our weekend specials."
- Error handling: Show validation errors inline

**Mockup:**
```
┌─────────────────────────────────────┐
│  🔥 GET VIP ACCESS 🔥               │
│                                     │
│  Be the first to know when the     │
│  smoker fires up each weekend!     │
│                                     │
│  Name:  _____________________       │
│  Email: _____________________       │
│  Phone: _____________________ (opt) │
│                                     │
│  [Join VIP List]                    │
└─────────────────────────────────────┘
```

**Implementation:**
- [ ] Create signup form component
- [ ] Add email validation (regex)
- [ ] Add phone validation (optional, format: XXX-XXX-XXXX)
- [ ] Style for mobile-first
- [ ] Add to footer section
- [ ] Connect to Supabase (save to vip_customers table)
- [ ] Show success/error messages
- [ ] Test on mobile

**Approval Point:** Show mockup before coding

---

### Task 4: Admin Interface (3-Button Layout)
**Status:** ⚪ Not Started
**Estimate:** 4 hours
**Approval Required:** Yes (show mockup first)

**Requirements:**
- Route: `/admin` (will add auth in Phase 2)
- Mobile-first design (owner uses phone)
- 3 clear sections with big, tappable buttons

**Wireframe:**
```
┌─────────────────────────────────────┐
│  WET SMOKEHOUSE - Admin            │
├─────────────────────────────────────┤
│                                     │
│  📋 WEEKEND MENU                    │
│  ─────────────────────────────────  │
│  ☑ Brisket - $18/lb                 │
│  ☑ Ribs (half slab) - $30           │
│  ☐ Pulled Pork - SOLD OUT           │
│  ☑ Sausage - $6.00                  │
│  ☑ Hotlinks - $8.00                 │
│                                     │
│  [Update Menu] ← Big button         │
│                                     │
│  ─────────────────────────────────  │
│                                     │
│  📧 VIP EMAIL BLAST                 │
│  ─────────────────────────────────  │
│  Subject: This Weekend's Special    │
│                                     │
│  Message:                           │
│  ┌─────────────────────────────┐   │
│  │ [Text area for message]     │   │
│  │                             │   │
│  └─────────────────────────────┘   │
│                                     │
│  📸 Upload Menu Photo:              │
│  [Drag & drop or tap to select]    │
│                                     │
│  [Send to VIPs] ← Big button        │
│                                     │
│  ─────────────────────────────────  │
│                                     │
│  📊 VIEW ORDERS                     │
│  ─────────────────────────────────  │
│  [Open Google Sheet] ← Big button   │
│  (Opens in new tab)                 │
│                                     │
└─────────────────────────────────────┘
```

**Implementation:**
- [ ] Create `/admin` route in Next.js
- [ ] Build menu management section:
  - Fetch menu items from Supabase
  - Render checkboxes (checked = available, unchecked = sold out)
  - Update button saves to Supabase
- [ ] Build VIP blast section (UI only, email send in Phase 2):
  - Subject input
  - Message textarea
  - Photo upload (save to Supabase Storage)
  - Send button (placeholder for Phase 2)
- [ ] Build orders section:
  - Link to Google Sheet (hardcoded for now)
  - Opens in new tab
- [ ] Style with large tap targets (min 44px × 44px)
- [ ] Test on iPhone/Android screen sizes
- [ ] Add loading states for buttons

**Approval Point:** Show mockup before implementation

---

### Task 5: Supabase Setup
**Status:** ⚪ Not Started
**Estimate:** 2 hours
**Approval Required:** Yes (show schema before creating)

**Requirements:**
- Create Supabase project (free tier)
- Define database schema
- Set up Row-Level Security (RLS)
- Seed with test data

**Schema:**
```sql
-- Menu items
CREATE TABLE menu_items (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  description TEXT,
  price DECIMAL(10,2) NOT NULL,
  unit TEXT, -- e.g., "lb", "each", "half slab"
  category TEXT, -- e.g., "meat", "sides", "drinks"
  available BOOLEAN DEFAULT true,
  image_url TEXT,
  sort_order INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- VIP customers
CREATE TABLE vip_customers (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  email TEXT NOT NULL UNIQUE,
  phone TEXT,
  email_opted_in BOOLEAN DEFAULT true,
  sms_opted_in BOOLEAN DEFAULT false, -- for future SMS
  created_at TIMESTAMP DEFAULT NOW(),
  unsubscribed_at TIMESTAMP
);

-- VIP blast history (for tracking)
CREATE TABLE vip_blasts (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  subject TEXT NOT NULL,
  message TEXT NOT NULL,
  photo_url TEXT,
  sent_at TIMESTAMP DEFAULT NOW(),
  recipient_count INTEGER,
  blast_type TEXT DEFAULT 'email' -- 'email', 'sms', 'both'
);

-- Orders (for Phase 5 - payments)
CREATE TABLE orders (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  customer_name TEXT NOT NULL,
  customer_email TEXT,
  customer_phone TEXT,
  items JSONB NOT NULL, -- array of {menu_item_id, quantity, price}
  total_amount DECIMAL(10,2) NOT NULL,
  payment_status TEXT DEFAULT 'pending', -- 'pending', 'paid', 'refunded'
  payment_provider TEXT, -- 'square', 'stripe', null
  payment_intent_id TEXT, -- Square/Stripe reference
  confirmed_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Implementation:**
- [ ] Create Supabase account (user may already have)
- [ ] Create new project: "wet-smokehouse-bbq"
- [ ] Run schema SQL in Supabase SQL editor
- [ ] Set up RLS policies:
  - menu_items: public read, admin write
  - vip_customers: admin read/write only
  - orders: admin read/write only
- [ ] Seed test data:
  - 5 menu items (Brisket, Ribs, Pulled Pork, Sausage, Hotlinks)
  - 3 test VIP customers
- [ ] Get Supabase credentials (URL, anon key)
- [ ] Add to .env.local (DO NOT commit)

**Approval Point:** Show schema before creating tables

---

### Task 6: Connect Frontend to Supabase
**Status:** ⚪ Not Started
**Estimate:** 2 hours
**Approval Required:** No (technical implementation)

**Requirements:**
- Install Supabase client library
- Connect menu display to Supabase
- Connect admin menu updates to Supabase
- Connect VIP signup to Supabase

**Implementation:**
- [ ] Install: `npm install @supabase/supabase-js`
- [ ] Create `lib/supabase.ts` client
- [ ] Fetch menu items on homepage (replace hardcoded data)
- [ ] Admin: Fetch menu items for checkboxes
- [ ] Admin: Update menu_items.available on checkbox change
- [ ] VIP signup: Insert to vip_customers table
- [ ] Test all CRUD operations
- [ ] Handle errors gracefully

---

### Task 7: Vercel Deployment
**Status:** ⚪ Not Started
**Estimate:** 1 hour
**Approval Required:** No (just share preview link)

**Requirements:**
- Connect GitHub repo to Vercel
- Set environment variables
- Deploy preview
- Test on mobile device

**Implementation:**
- [ ] Create Vercel account (user may already have)
- [ ] Import wet_smokehouse repo
- [ ] Add environment variables:
  - NEXT_PUBLIC_SUPABASE_URL
  - NEXT_PUBLIC_SUPABASE_ANON_KEY
- [ ] Deploy to preview URL
- [ ] Test on iPhone/Android
- [ ] Share preview link with user

---

## 🚧 BLOCKERS

- None currently

---

## 📊 PROGRESS TRACKER

**Overall:** 0% Complete

| Task | Status | Progress | Time Spent |
|------|--------|----------|------------|
| Git Setup | ⚪ Not Started | 0% | 0h |
| Rebrand | ⚪ Not Started | 0% | 0h |
| Email Signup | ⚪ Not Started | 0% | 0h |
| Admin UI | ⚪ Not Started | 0% | 0h |
| Supabase | ⚪ Not Started | 0% | 0h |
| Integration | ⚪ Not Started | 0% | 0h |
| Deploy | ⚪ Not Started | 0% | 0h |

**Estimated Total:** 14.5 hours

---

## 🔗 DEPENDENCIES

**Required Before Starting:**
- PROJECT-DOCS created ✅
- User approval to proceed ✅
- Git repo initialized ⏳

**Required During Phase:**
- Supabase account (user creates)
- Vercel account (user creates)
- GitHub account (user has)

---

## 📝 NOTES

### Design Decisions
- Using Tailwind CSS (already in Munchos template)
- Keeping existing hamburger menu (works well on mobile)
- Mobile-first breakpoints: 375px, 768px, 1024px
- Big tap targets: 44px × 44px minimum

### Performance Targets
- Lighthouse score >90
- First Contentful Paint <1.5s
- Time to Interactive <3s on 4G

### Email Validation
- Use regex: `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`
- Show error: "Please enter a valid email"

### Phone Validation (Optional)
- Accept formats: 555-555-5555, (555) 555-5555, 5555555555
- Format on blur to: XXX-XXX-XXXX

---

## 🔄 CHANGE LOG

### 2026-04-08
- Created Phase 1 plan
- Defined tasks and estimates
- Added customer email signup requirement
- Created wireframes for admin UI
- Defined Supabase schema

---

*Next Update: When Phase 1 starts*