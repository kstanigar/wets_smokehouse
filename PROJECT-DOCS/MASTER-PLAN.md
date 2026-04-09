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
- [ ] Initialize Git repo
- [ ] Begin Phase 1

**Changed:**
- Directory: wet_smokehouse (not wet_bbq)
- Launch: Email only (SMS planned for later)
- Payments: Square primary (when approved), Stripe secondary
- Architecture: Build interfaces now, implement features later

**Next:** Initialize Git, begin Phase 1 (Frontend)

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
1. Menu management from phone
2. VIP customer email blasts (with photos)
3. Customer email capture (VIP list building)
4. Simple management for non-tech owner
5. Zero monthly costs at launch
6. Easy to add SMS/payments later (no refactoring)

### Solution Architecture
```
Customer Site (Next.js)
    ↓
Supabase (Database + Auth + Storage)
    ↓
Google Sheets (Owner's order view)

VIP Blasts:
Backend Admin → MessageService → SendGrid (email)
                            └→ [Twilio later] (SMS)

Payments (Future):
Customer → PaymentProcessor → Square
                          └→ [Stripe alternative]
```

### Owner Workflow
1. **Thursday Evening:**
   - Log into admin on phone (30 sec)
   - Update weekend menu checkboxes (30 sec)
   - Send VIP email blast with photo (1 min)

2. **Friday-Sunday:**
   - Customers visit site, see menu
   - Customers can sign up for VIP emails
   - (Later: customers can order via Square)

3. **Order Management (When Payments Added):**
   - Check orders in Google Sheets
   - Tap checkbox to confirm

**Total Time:** <5 minutes/week

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
- [ ] Menu displays correctly
- [ ] Admin panel accessible on phone
- [ ] Can update menu items (checkboxes work)
- [ ] Customer email signup form works

### Week 2 (Phase 2: Backend)
- [ ] VIP email list stored in Supabase
- [ ] Owner can compose email message
- [ ] Owner can upload photo for email
- [ ] Test email blast to 5 test addresses
- [ ] Email includes photo and message

### Week 3 (Phase 3: Integration)
- [ ] Menu updates sync to Supabase
- [ ] Menu changes appear on frontend instantly
- [ ] VIP emails sent via SendGrid
- [ ] Google Sheets shows VIP signups
- [ ] All features work on mobile

### Week 4 (Phase 4: Deployment)
- [ ] Site deployed to Vercel
- [ ] Custom domain connected (if applicable)
- [ ] Owner trained (15-minute call)
- [ ] Documentation complete
- [ ] Ready for weekend operations

### Post-Launch (Phase 5: Payments - When Approved)
- [ ] Square integration complete
- [ ] Customers can add to cart
- [ ] Checkout redirects to Square
- [ ] Orders saved to Supabase
- [ ] Orders appear in Google Sheets
- [ ] Owner can confirm orders via checkbox

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

**Why Google Sheets for Orders?**
- Owner already knows how to use it
- Works on phone
- Familiar checkbox interface
- Zero learning curve
- Can export data if needed

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