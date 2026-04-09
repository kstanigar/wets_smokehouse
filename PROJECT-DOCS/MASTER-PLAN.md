# WET SMOKEHOUSE BBQ - MASTER PLAN
**Last Updated:** 2026-04-08
**Current Phase:** Documentation Complete → Ready for Phase 1
**Budget Status:** $0/month (all free tiers)
**Completion:** 5% (Documentation established)

---

## 🎯 TL;DR (AI Quick Context)

**What:** Weekend-only BBQ website with VIP email blasts, menu management
**Owner:** Non-technical, phone-only management
**Budget:** $0/month at launch (all free tiers)
**Stack:** Supabase + Vercel + SendGrid (email only)
**Future:** SMS (Twilio), Square payments, Stripe alternative
**Timeline:** 4 weeks to launch
**Status:** Documentation phase complete → Phase 1 approved to start

---

## 📅 RECENT UPDATES (Newest First)

### 2026-04-08: INVENTORY MANAGEMENT & PRICING CLARIFIED
**Decision:** Real-time inventory system with quantity tracking and pricing display
**Status:** ✅ User clarified CRITICAL inventory workflow
**Action Items:**
- [x] Update all .md files with inventory/pricing requirements
- [ ] Add quantity tracking to database schema
- [ ] Add price management to admin interface
- [ ] Implement quantity decrement on order confirmation

**CRITICAL CHANGES:**
- **PRICING IS SHOWN:** Menu and order form display prices (I was WRONG before!)
- **Quantity Tracking:** Each item has available order count (e.g., "10 available")
- **Creates Urgency:** "Brisket - $18 (Only 3 left!)" drives sales
- **Real-Time Decrement:** Quantity decreases as orders confirmed in Google Sheets
- **Auto Sold Out:** When quantity = 0, item automatically becomes "Sold Out"

**Backend Admin Changes:**
- Each menu item has 3 fields:
  1. **Available:** Radio button (yes/no)
  2. **Quantity:** Number of orders available (e.g., 10)
  3. **Price:** Price per unit (persists week-to-week unless changed)
- Owner updates quantity before each weekend
- Prices stay same unless owner changes them

**Sold Out Logic:**
- **Manual:** Owner unchecks "Available" → Sold Out
- **Automatic:** Quantity reaches 0 → Sold Out

---

### 2026-04-08: CRITICAL - Order Form & Google Sheets Workflow Clarified
**Decision:** Orders flow through Google Sheets (NOT payment system at launch)
**Status:** ✅ User clarified core workflow
**Action Items:**
- [x] Update all .md files with order form requirements
- [x] Add Google Sheets write access to architecture
- [x] Design weekend vs catering order forms
- [x] Update Phase 1 to include order forms

**CRITICAL CHANGES:**
- **Order Forms (NO Payment):** Customers fill out form to REQUEST orders
- **Pricing DISPLAYED:** Menu items show price AND quantity available (CORRECTED)
- **Radio Buttons/Checkboxes:** Customer selects items to order
- **"Sold Out" Display:** Unavailable items shown as "Sold Out" (disabled)
- **Google Sheets = Primary Workflow:** Orders go to Sheets, owner confirms via checkbox
- **NOT Read-Only:** Owner WRITES to Sheets (checkboxes to confirm orders)
- **Mobile-Friendly First:** Then responsive for desktop

**Two Order Forms:**
1. **Weekend Specials:** Full menu with pricing and quantities
2. **Catering Request:** Simple form (name, email, phone, message only - NO menu)

**Workflow:**
```
Customer fills order form → Supabase → Google Sheets (instant)
                                            ↓
Owner opens Sheets on phone → Checks box to confirm
                                            ↓
Webhook updates Supabase → Customer gets confirmation email
```

**Next:** Update all documentation with this critical workflow

---

### 2026-04-08: Documentation Structure Approved & Created
**Decision:** Create all PROJECT-DOCS files, initialize Git repo
**Status:** ✅ Approved by user
**Action Items:**
- [x] Create PROJECT-DOCS directory structure
- [x] Create RULES.md
- [x] Create MASTER-PLAN.md
- [x] Create QUICKSTART.md
- [x] Create PHASE files (1-4)
- [x] Create SESSION-LOG.md
- [x] Create APPROVALS.md
- [x] Initialize Git repo
- [ ] Update with order form workflow
- [ ] Begin Phase 1

**Changed:**
- Directory: wet_smokehouse (not wet_bbq)
- Launch: Email only (SMS planned for later)
- Payments: Square primary (when approved), Stripe secondary
- Architecture: Build interfaces now, implement features later

**Next:** Update documentation with order form workflow, begin Phase 1 (Frontend)

---

### 2026-04-08: Launch Scope Refined
**User Requirements Updated:**
- **Email at Launch:**
  - Frontend: Customer email capture (VIP signup)
  - Backend: Owner sends VIP email blasts
  - Tool: SendGrid free tier (100 emails/day)
- **SMS Later:**
  - Owner manually sends SMS for now
  - Build Twilio interface/hooks in code
  - Implement when owner approves
- **Payments Later:**
  - Square is primary choice (owner has account)
  - Stripe as secondary/alternative
  - NOT live at launch
  - Build payment interface/hooks in code

**Architectural Decision:**
Build abstraction layers NOW to avoid refactoring later:
- MessageService interface (email now, SMS later)
- PaymentProcessor interface (Square/Stripe later)

---

### 2026-04-08: Initial Requirements Gathering
**User Requirements:**
- Weekend-only operation (not daily)
- Owner is non-tech-savvy
- Must work from phone
- Email VIP blasts critical
- Budget-conscious ($0/month at launch)
- Simple 3-button interface

**Decisions Made:**
- Use all free tiers (Supabase, Vercel, SendGrid)
- Keep Google Sheets for owner's order view
- Mobile-first design
- Plan for SMS/payments without implementing yet

---

## 🏗️ PROJECT OVERVIEW

### Problem Statement
Weekend BBQ business needs:
1. **Order Request System** - Customers fill form to request orders (NO payment at launch)
2. **Google Sheets Workflow** - Orders appear in Sheets, owner confirms via checkbox
3. **Menu Management** - Owner updates availability from phone
4. **VIP Email Blasts** - Send weekend specials with photos
5. **Mobile-Friendly** - Customers order from phones
6. **Simple for Owner** - Non-tech owner manages from phone
7. **Zero Monthly Costs** - All free tiers at launch
8. **Future-Proof** - Easy to add SMS/payments later (no refactoring)

### Solution Architecture
```
ORDER WORKFLOW (Primary - Launch Feature):
Customer fills order form (mobile-friendly)
    ↓
Order saved to Supabase
    ↓
Order appears in Google Sheets (instant)
    ↓
Owner checks checkbox in Sheets (confirms order)
    ↓
Webhook updates Supabase order status
    ↓
Customer gets confirmation email

MENU MANAGEMENT:
Backend Admin → Supabase → Frontend (real-time update)
    ↓
"Sold Out" items disabled on order form

VIP BLASTS:
Backend Admin → MessageService → SendGrid (email)
                            └→ [Twilio later] (SMS)

PAYMENTS (Phase 5 - Future):
Add payment step before order confirmation
Customer → PaymentProcessor → Square → Order confirmed
                          └→ [Stripe alternative]
```

### Owner Workflow
1. **Thursday Evening (Menu Setup):**
   - Log into admin on phone (30 sec)
   - For each menu item:
     - Check "Available" radio button
     - Enter quantity available (e.g., "Brisket: 10 orders")
     - Update price if needed (or leave same as last week)
   - Save menu (1 min)
   - Send VIP email blast with photo (1 min)

2. **Friday-Sunday (Orders Come In):**
   - Customers see menu: "Brisket - $18/lb (10 available)"
   - Customers fill order form and submit
   - Orders appear in Google Sheets INSTANTLY
   - Owner opens Sheets on phone
   - Taps checkbox next to order to confirm
   - **Quantity auto-decrements:** "Brisket (9 available)" on frontend
   - Customer automatically gets confirmation email

3. **When Items Selling Fast:**
   - Frontend shows urgency: "Brisket - $18 (Only 2 left!)"
   - When quantity = 0 → Auto "Sold Out"
   - Owner can manually mark sold out if needed (uncheck "Available")

**Total Time:**
- Menu setup: 3-5 minutes (Thursday)
- Per order confirmation: <30 seconds (tap checkbox in Sheets)
- Menu updates during weekend: <1 minute if needed

---

## 📊 PHASE STATUS TRACKER

| Phase | Status | Start Date | End Date | Completion |
|-------|--------|------------|----------|------------|
| Planning | ✅ Complete | 2026-04-08 | 2026-04-08 | 100% |
| Phase 1: Frontend | 🟢 Approved | TBD | TBD | 0% |
| Phase 2: Backend | ⚪ Waiting | TBD | TBD | 0% |
| Phase 3: Integration | ⚪ Waiting | TBD | TBD | 0% |
| Phase 4: Deployment | ⚪ Waiting | TBD | TBD | 0% |
| Phase 5: Payments | ⚪ Future | TBD | TBD | 0% |
| Phase 6: SMS | ⚪ Future | TBD | TBD | 0% |

---

## 💰 BUDGET BREAKDOWN

### Launch Costs (Phases 1-4)
| Service | Tier | Cost | Status |
|---------|------|------|--------|
| Supabase | Free | $0 | ✅ Active |
| Vercel | Free | $0 | ✅ Active |
| SendGrid | Free | $0 | ✅ Active |
| GitHub | Free (private) | $0 | 🟡 Initializing |
| Google Sheets API | Free | $0 | 🟡 Pending |

**Launch Total:** $0/month

---

### Future Costs (Post-Launch, When Approved)

#### Phase 5: Square Payments
| Service | Cost | When |
|---------|------|------|
| Square | 2.9% + $0.30/transaction | When owner approves |
| No monthly fee | $0 | - |

#### Phase 6: SMS Addition
| Service | Cost | When |
|---------|------|------|
| Twilio SMS | ~$0.0079/message | When owner approves |
| 50 VIPs weekly | ~$1.58/month | Example cost |

#### Future Upgrades (If Needed)
| Service | Trigger | Cost |
|---------|---------|------|
| Supabase Pro | Database >500MB or full-time | $25/month |
| Vercel Pro | Traffic >10K visitors/month | $20/month |
| Stripe Alternative | If Square not working | 2.9% + $0.30/txn |

---

## 🎯 SUCCESS CRITERIA

### Week 1 (Phase 1: Frontend)
- [ ] Owner can view site on phone
- [ ] Menu displays correctly (mobile-friendly first, then responsive)
- [ ] Admin panel accessible on phone
- [ ] Can update menu items (checkboxes work)
- [ ] **Weekend order form works** (radio buttons, no pricing)
- [ ] **Catering order form works** (separate form)
- [ ] "Sold Out" items display correctly
- [ ] Customer VIP email signup form works

### Week 2 (Phase 2: Backend)
- [ ] **Orders save to Supabase**
- [ ] **Orders appear in Google Sheets instantly**
- [ ] VIP email list stored in Supabase
- [ ] Owner can compose email message
- [ ] Owner can upload photo for email
- [ ] Test email blast to 5 test addresses
- [ ] Email includes photo and message

### Week 3 (Phase 3: Integration)
- [ ] **Google Sheets checkbox triggers order confirmation**
- [ ] **Webhook updates Supabase when owner checks box**
- [ ] **Customer gets confirmation email automatically**
- [ ] Menu updates sync to Supabase
- [ ] Menu changes appear on frontend instantly (sold out items)
- [ ] VIP emails sent via SendGrid
- [ ] All features work on mobile

### Week 4 (Phase 4: Deployment & Testing)
- [ ] Site deployed to Vercel
- [ ] Custom domain connected (if applicable)
- [ ] **Test full order workflow** (form → Sheets → confirmation)
- [ ] Owner trained (30-minute call - includes Google Sheets)
- [ ] Documentation complete
- [ ] Ready for weekend operations

### Post-Launch (Phase 5: Payments - When Approved)
- [ ] Square integration complete
- [ ] Add pricing to order form
- [ ] Payment step added before order submission
- [ ] Checkout redirects to Square
- [ ] Only paid orders go to Google Sheets
- [ ] Owner confirms fulfillment (not payment) via checkbox

### Post-Launch (Phase 6: SMS - When Approved)
- [ ] Twilio account set up
- [ ] SMS opt-in checkbox on signup
- [ ] VIP blast sends to both email + SMS
- [ ] Owner can choose email-only or email+SMS

---

## 🔗 RELATED DOCUMENTS

- [RULES.md](./RULES.md) - Project governance
- [QUICKSTART.md](./QUICKSTART.md) - New AI agent onboarding
- [PHASE-1-frontend.md](./phases/PHASE-1-frontend.md) - Week 1 plan
- [PHASE-2-backend.md](./phases/PHASE-2-backend.md) - Week 2 plan
- [PHASE-3-integration.md](./phases/PHASE-3-integration.md) - Week 3 plan
- [PHASE-4-deployment.md](./phases/PHASE-4-deployment.md) - Week 4 plan
- [TECH-STACK.md](./decisions/TECH-STACK.md) - Technology choices
- [ARCHITECTURE.md](./decisions/ARCHITECTURE.md) - System design
- [SESSION-LOG.md](./sessions/SESSION-LOG.md) - Daily progress

---

## 🚀 NEXT ACTIONS

### Immediate
1. [x] Create all .md documentation files
2. [ ] Initialize Git repo with feature branch workflow
3. [ ] Create initial commit with documentation
4. [ ] Begin Phase 1: Frontend development

### This Week (Phase 1)
- Complete Munchos → WET SMOKEHOUSE rebrand
- Build admin interface (3 buttons: menu, VIP blast, orders link)
- Add customer email signup form
- Deploy preview to Vercel

### Blockers
- None currently

---

## 📝 NOTES & DECISIONS

### Key Architectural Decisions

**Why Build Interfaces Before Features?**
- Avoid refactoring when adding SMS later
- Swap email → SMS → both without changing code
- Add Square → Stripe without breaking existing code
- Clean separation of concerns

**Why Email-Only at Launch?**
- $0/month cost (SendGrid free tier)
- Owner can manually send SMS for now
- Test VIP engagement before investing in Twilio
- Easy to add SMS later with existing interface

**Why Square Before Stripe?**
- Owner already has Square account
- Lower friction to enable payments
- Stripe as backup if Square doesn't meet needs
- Both use same PaymentProcessor interface

**Why Google Sheets for Orders?** ⚠️ CRITICAL TO SUCCESS
- Owner already knows how to use it
- Works on phone (owner at smoker, not desk)
- Familiar checkbox interface (tap to confirm)
- Zero learning curve
- Can export data if needed
- **PRIMARY confirmation method** (not just a view)
- Creates personal touch (manual confirmation)
- Owner sees all order details instantly
- **NOT read-only** - owner writes checkmarks

**Why Supabase over Firebase?**
- Better PostgreSQL (real database, not NoSQL)
- Free tier more generous
- Row-Level Security built-in
- Easier to migrate to self-hosted later
- Better TypeScript support

**Why Not Implement Payments at Launch?**
- Owner hasn't approved yet
- Adds complexity to training
- Start simple, add when revenue justifies
- Focus on building VIP list first

---

## 🔄 CHANGE LOG

### 2026-04-08
- Created MASTER-PLAN.md
- Defined 4-week launch timeline (email only)
- Added Phase 5 (Square) and Phase 6 (SMS) for post-launch
- Established $0/month launch budget
- Created interface-based architecture for easy feature addition
- Updated directory to wet_smokehouse

---

*Last reviewed by: Claude Sonnet 4.5 on 2026-04-08*
*Next review: After Phase 1 completion*