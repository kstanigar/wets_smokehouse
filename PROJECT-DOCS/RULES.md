# PROJECT RULES - WET SMOKEHOUSE BBQ
**Last Updated:** 2026-04-08
**Status:** Active

> These rules govern ALL AI agents working on this project.
> Read this file FIRST before making any changes.

---

## 🚨 CRITICAL RULES (Never Break These)

### Rule #1: Reverse Chronological Updates
- **ALL .md files**: Most recent information at TOP
- Older information moves to BOTTOM
- Each update must include date and summary

### Rule #2: Approval-First Development
- ✅ Get user approval BEFORE implementing code changes
- ✅ Show plan/diff/summary first
- ✅ Wait for explicit "approved" or "proceed"
- ❌ NEVER auto-implement without approval

### Rule #3: No Hallucinations or Error Loops
- ✅ If uncertain, check official documentation online
- ✅ If error persists after 2 attempts, ask user for guidance
- ✅ Suggest alternative solutions
- ❌ NEVER guess at API syntax or make up features
- ❌ NEVER retry same failing command >3 times

### Rule #4: Session Documentation
- ✅ Document changes after EVERY session
- ✅ Update MASTER-PLAN.md with progress
- ✅ Log to SESSION-LOG.md with date/summary
- ✅ Update relevant PHASE file with status

### Rule #5: Feature Branch Workflow
- ✅ ALWAYS push to feature branch (never main/master)
- ✅ Branch naming: `feature/description` or `fix/issue`
- ✅ User merges to main after review
- ❌ NEVER force push
- ❌ NEVER commit directly to main

---

## 📋 Project Context (Quick Reference)

**Project Name:** WET SMOKEHOUSE BBQ
**Project Type:** Weekend-only BBQ + Catering business
**Owner Tech Level:** Non-technical (focus on simplicity)
**Budget:** $0/month at launch (all free tiers)
**Timeline:** 4 weeks to launch

**Key Constraint:** Owner must be able to manage from phone while cooking

---

## 🎯 Development Workflow

1. **Read** MASTER-PLAN.md (understand current state)
2. **Check** relevant PHASE file (know what's next)
3. **Plan** changes (show user before coding)
4. **Get Approval** (wait for user confirmation)
5. **Implement** on feature branch
6. **Document** in SESSION-LOG.md
7. **Update** MASTER-PLAN.md status
8. **Push** to feature branch
9. **Request** user to review/merge

---

## 🛠️ Tech Stack Constraints

### Launch Stack (Phase 1-4)
**Allowed:**
- Supabase (free tier)
- Vercel (free tier)
- SendGrid (free tier - email only)
- Google Sheets API (free)
- Next.js / React / TypeScript
- Tailwind CSS

**Planned for Later (Build Hooks Now):**
- Twilio SMS (when owner approves - build interface ready)
- Square Payments (when owner approves - priority payment provider)
- Stripe Payments (alternative to Square - secondary option)

**Not Allowed (Cost):**
- Paid tiers until user approves
- Heavy libraries (MUI, Radix, etc.)
- Additional paid services

**Not Allowed (Complexity):**
- Over-engineered solutions
- Unnecessary abstractions
- Features owner won't use

---

## 🏗️ Architecture Principles

### Future-Proof Design (No Refactoring Later)

**Messaging Interface:**
```typescript
// Build abstraction layer NOW
interface MessageService {
  sendVIPBlast(message: string, photo?: string): Promise<void>
  // Can swap email → SMS → both without changing caller
}

// Launch: EmailService implements MessageService
// Later: SMSService implements MessageService
// Future: ComboService implements MessageService
```

**Payment Interface:**
```typescript
// Build abstraction layer NOW
interface PaymentProcessor {
  createCheckout(items: CartItem[]): Promise<CheckoutSession>
  handleWebhook(payload: any): Promise<Order>
  // Can swap Square → Stripe without changing caller
}

// Launch: MockPaymentService (no-op, returns success)
// Phase 5: SquareService implements PaymentProcessor
// Later: StripeService implements PaymentProcessor
```

**Key Principle:** Build interfaces/hooks NOW, implement features LATER

---

## 📱 Owner Interface Requirements

**Must Be:**
- Mobile-friendly (owner uses phone)
- <3 clicks to do anything
- Zero technical jargon
- Works offline (graceful degradation)

**Three Core Actions:**
1. Update menu availability (checkboxes)
2. Send VIP email blast (text + photo + send button)
3. View orders (Google Sheets with checkbox)

---

## 🔐 Security Requirements

- Supabase Row-Level Security (RLS) enabled
- Admin routes protected with auth
- No secrets in code (use .env)
- HTTPS only (Vercel default)
- Email opt-in tracking (legal compliance - CAN-SPAM Act)

---

## 📊 Success Metrics

**Week 1:** Owner can update menu from phone
**Week 2:** Customer emails captured for VIP list
**Week 3:** VIP email blasts work with photos
**Week 4:** Owner can manage everything from phone in <5 min/day

**Launch:** Owner operates entire business from phone with email VIP blasts

**Post-Launch (When Approved):**
- Add SMS functionality (Twilio)
- Add Square payments
- Optionally add Stripe as alternative

---

## ❌ Common Pitfalls to Avoid

1. **Over-engineering** - This is a weekend BBQ, not Amazon
2. **Ignoring mobile** - Owner works at the smoker, not a desk
3. **Complex UI** - Owner isn't tech-savvy
4. **Hard-coded services** - Use interfaces for easy swapping
5. **Skipping documentation** - Future AI agents need context
6. **Implementing payments early** - Owner hasn't approved yet

---

## 📞 When to Ask User

- Uncertain about feature priority
- Architecture decision with tradeoffs
- Error persists after 2 attempts
- User feedback needed on UX/UI
- Before implementing payment features
- Before implementing SMS features

---

## 🔄 Update History

### 2026-04-08: Rules Established & Refined
- Created initial RULES.md
- Defined approval workflow
- Set budget constraints ($0/month at launch)
- Established feature branch workflow
- **Updated:** Email only at launch (SMS later)
- **Updated:** Square primary, Stripe secondary (both post-launch)
- **Updated:** Build interfaces now, implement features when approved
- **Updated:** Directory is wet_smokehouse

---

*Next Update: [Date] - [Summary]*