# QUICKSTART - WET SMOKEHOUSE BBQ
**For AI Agents joining this project**
**Last Updated:** 2026-04-08

## 🚀 Read This First (60 Seconds)

**Project:** WET SMOKEHOUSE - Weekend BBQ website with email VIP blasts
**Owner:** Non-technical, uses phone only
**Budget:** $0/month at launch
**Timeline:** 4 weeks to launch (email), then add SMS/payments post-launch
**Current Status:** [Check MASTER-PLAN.md for current phase]

---

## 📚 Required Reading (In Order)

1. **[RULES.md](./RULES.md)** - READ FIRST (5 min)
   - Approval workflow (Rule #2)
   - No hallucinations (Rule #3)
   - Feature branch workflow (Rule #5)
   - Documentation requirements (Rule #4)

2. **[MASTER-PLAN.md](./MASTER-PLAN.md)** - Current state (3 min)
   - Check "RECENT UPDATES" section at top
   - Check "PHASE STATUS TRACKER"
   - Check "NEXT ACTIONS"

3. **Current Phase File** - Today's tasks (2 min)
   - Check `phases/PHASE-[X]-[name].md`
   - Read "Current Status" at top
   - See what's blocked/in-progress

---

## 🎯 Quick Context

**Launch Features (Phases 1-4):**
- **Order request forms** (weekend + catering, NO payment)
- **Google Sheets workflow** (orders appear, owner confirms via checkbox)
- Menu management (owner updates availability from phone)
- "Sold Out" display (disabled items on order form)
- VIP email blasts (SendGrid free tier)
- Customer email capture (build VIP list)
- Mobile-friendly first, then responsive

**Post-Launch Features (Phases 5-6, When Approved):**
- Square payments (owner has account)
- Twilio SMS (add to VIP blasts)
- Stripe alternative (if needed)

**What's Been Built:**
- [Check SESSION-LOG.md for latest]

**What's Next:**
- [Check MASTER-PLAN.md → NEXT ACTIONS]

**Blockers:**
- [Check MASTER-PLAN.md → Blockers section]

---

## ⚡ Common Commands

### Check Project Status
```bash
# See what phase we're in
cat PROJECT-DOCS/MASTER-PLAN.md | grep "Current Phase"

# See latest session
cat PROJECT-DOCS/sessions/SESSION-LOG.md | head -50

# See what's blocked
cat PROJECT-DOCS/MASTER-PLAN.md | grep "Blockers" -A 5
```

### Before Making Changes
1. Read RULES.md Rule #2 (get approval first)
2. Check current feature branch
3. Show plan to user
4. Wait for approval
5. Implement on feature branch

### After Making Changes
1. Update SESSION-LOG.md (newest at top)
2. Update relevant PHASE file
3. Update MASTER-PLAN.md if phase status changes
4. Commit to feature branch
5. Push to remote

---

## 🚫 Don't Do This

- ❌ Code without approval (Rule #2)
- ❌ Push to main/master (Rule #5)
- ❌ Implement SMS now (planned for Phase 6)
- ❌ Implement payments now (planned for Phase 5)
- ❌ Hard-code email service (use MessageService interface)
- ❌ Hard-code payment provider (use PaymentProcessor interface)
- ❌ Complex UI (Owner is non-technical)
- ❌ Skip documentation (Rule #4)

---

## ✅ Do This

- ✅ Read RULES.md first
- ✅ Check MASTER-PLAN.md for current state
- ✅ Build interfaces for future features (SMS, payments)
- ✅ Use abstraction layers (easy to swap providers)
- ✅ Ask user if uncertain
- ✅ Document everything
- ✅ Keep it simple (mobile-first, 3-button interface)
- ✅ Test on mobile

---

## 🏗️ Architecture Key Concepts

### MessageService Interface
```typescript
// Build this NOW (Phase 2)
interface MessageService {
  sendVIPBlast(message: string, photo?: string): Promise<void>
}

// Launch: EmailService implements MessageService (SendGrid)
// Phase 6: SMSService implements MessageService (Twilio)
// Future: ComboService sends both email + SMS
```

### PaymentProcessor Interface
```typescript
// Build this in Phase 5
interface PaymentProcessor {
  createCheckout(items: CartItem[]): Promise<CheckoutSession>
  handleWebhook(payload: any): Promise<Order>
}

// Phase 5: SquareService implements PaymentProcessor
// Later: StripeService implements PaymentProcessor (if needed)
```

**Key Principle:** Build interfaces now, swap implementations later (no refactoring)

---

## 📞 When to Ask User

- Uncertain about priority
- Error after 2 attempts
- Architecture decision needed
- About to implement SMS feature (wait for Phase 6 approval)
- About to implement payment feature (wait for Phase 5 approval)
- UX/UI feedback needed

---

## 📋 Launch vs. Post-Launch

### Launch Scope (Phases 1-4)
✅ Menu management
✅ VIP email blasts (SendGrid)
✅ Customer email capture
✅ Mobile admin interface
✅ Google Sheets integration (ready for orders)

### Post-Launch (Owner Approval Required)
⏳ Square payments (Phase 5)
⏳ Twilio SMS (Phase 6)
⏳ Stripe alternative (if Square doesn't work)

---

*Created: 2026-04-08*
*Purpose: Onboard new AI agents in <10 minutes*
*Updated: When architecture or scope changes*