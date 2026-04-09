# PHASE 2: Backend Logic & VIP Email System
**Duration:** Week 2 (7 days)
**Status:** ⚪ Waiting (Phase 1 must complete first)
**Budget:** $0 (using free tiers only)
**Goal:** Build VIP email blast functionality, admin authentication

---

## 🎯 TL;DR

Implement VIP email blast feature using SendGrid
Add admin authentication (Supabase Auth)
Build MessageService interface (ready for SMS later)
Test email delivery with photos

---

## 📅 RECENT UPDATES (Newest First)

### 2026-04-08: Phase Defined
- Created phase plan
- Awaiting Phase 1 completion
- Will build email-only implementation with SMS hooks

---

## ✅ GOALS

1. Implement admin authentication (Supabase Auth)
2. Build MessageService interface (abstraction for email/SMS)
3. Implement EmailService using SendGrid
4. VIP blast sends emails with photos
5. Track blast history in database
6. Test with real email addresses

**Success Criteria:**
- Owner can log into admin securely
- Owner can send VIP blast from phone
- Email includes custom message + photo
- Email looks good on mobile devices
- 100 VIPs can receive email in <30 seconds

---

## 📋 TASKS

### Task 1: Admin Authentication
**Status:** ⚪ Not Started
**Estimate:** 3 hours
**Approval Required:** No (standard implementation)

**Requirements:**
- Use Supabase Auth (free tier)
- Magic link login (no password to remember)
- Session persists for 7 days
- Logout functionality

**Implementation:**
- [ ] Set up Supabase Auth in project
- [ ] Create `/login` page (magic link)
- [ ] Protect `/admin` route with middleware
- [ ] Add logout button to admin page
- [ ] Test login flow on mobile
- [ ] Handle errors (invalid email, network issues)

---

### Task 2: MessageService Interface
**Status:** ⚪ Not Started
**Estimate:** 1 hour
**Approval Required:** Yes (architectural decision)

**Purpose:** Abstract messaging so we can swap email → SMS → both without refactoring

**Interface Design:**
```typescript
// lib/messaging/MessageService.ts
export interface MessageService {
  sendVIPBlast(blast: VIPBlast): Promise<BlastResult>
}

export interface VIPBlast {
  subject: string
  message: string
  photoUrl?: string
  recipients: VIPCustomer[]
}

export interface BlastResult {
  success: boolean
  sentCount: number
  failedCount: number
  errors?: string[]
}

export interface VIPCustomer {
  id: string
  name: string
  email?: string
  phone?: string
  emailOptedIn: boolean
  smsOptedIn: boolean
}
```

**Implementation:**
- [ ] Create `lib/messaging/` directory
- [ ] Define TypeScript interfaces
- [ ] Create abstract MessageService class
- [ ] Document for future SMS implementation

**Approval Point:** Show interface design before implementing

---

### Task 3: EmailService Implementation
**Status:** ⚪ Not Started
**Estimate:** 4 hours
**Approval Required:** No (technical implementation)

**Requirements:**
- Implements MessageService interface
- Uses SendGrid API (free tier: 100 emails/day)
- Sends HTML emails with embedded photo
- Includes unsubscribe link (CAN-SPAM compliance)
- Handles errors gracefully

**Implementation:**
- [ ] Sign up for SendGrid (free tier)
- [ ] Get API key, add to .env (SENDGRID_API_KEY)
- [ ] Install: `npm install @sendgrid/mail`
- [ ] Create `lib/messaging/EmailService.ts`
- [ ] Implement sendVIPBlast method:
  - Fetch opted-in VIP customers from Supabase
  - Build HTML email template
  - Embed photo (or link to hosted photo)
  - Add unsubscribe link
  - Send via SendGrid
  - Log to vip_blasts table
- [ ] Handle SendGrid errors (rate limits, invalid emails)
- [ ] Test with 3 real email addresses

**Email Template:**
```html
<div style="font-family: Arial, sans-serif; max-width: 600px;">
  <h1 style="color: #8B4513;">WET SMOKEHOUSE</h1>
  <img src="{{photoUrl}}" style="width: 100%; max-width: 600px;" />
  <h2>{{subject}}</h2>
  <p>{{message}}</p>
  <hr />
  <p style="font-size: 12px; color: #666;">
    You're receiving this because you signed up for VIP updates.
    <a href="{{unsubscribeUrl}}">Unsubscribe</a>
  </p>
</div>
```

---

### Task 4: VIP Blast UI Completion
**Status:** ⚪ Not Started
**Estimate:** 2 hours
**Approval Required:** No (UI already approved in Phase 1)

**Requirements:**
- Connect "Send to VIPs" button to EmailService
- Show sending progress (loading state)
- Show success message with count sent
- Show errors if any fail
- Save blast to vip_blasts table

**Implementation:**
- [ ] Create API route: `/api/vip-blast`
- [ ] Validate admin session (Supabase Auth)
- [ ] Call EmailService.sendVIPBlast()
- [ ] Return success/error to frontend
- [ ] Frontend shows progress spinner
- [ ] Frontend shows: "Sent to 47 VIP customers!"
- [ ] Handle errors: "Failed to send to 3 customers"

---

### Task 5: Unsubscribe Functionality
**Status:** ⚪ Not Started
**Estimate:** 2 hours
**Approval Required:** No (legal requirement)

**Requirements:**
- Legal compliance (CAN-SPAM Act requires unsubscribe)
- Simple one-click unsubscribe
- Update vip_customers.unsubscribed_at timestamp

**Implementation:**
- [ ] Create page: `/unsubscribe/[customerId]`
- [ ] On load, update Supabase:
  - Set vip_customers.unsubscribed_at = NOW()
  - Set email_opted_in = false
- [ ] Show message: "You've been unsubscribed. Sorry to see you go!"
- [ ] Add link to re-subscribe if they change their mind
- [ ] Test unsubscribe flow

---

### Task 6: SMS Hooks (Preparation for Phase 6)
**Status:** ⚪ Not Started
**Estimate:** 1 hour
**Approval Required:** No (planning only)

**Purpose:** Set up code structure so SMS can be added later without refactoring

**Implementation:**
- [ ] Create `lib/messaging/SMSService.ts` (empty stub)
- [ ] Add TODO comments for Twilio implementation
- [ ] Add sms_opted_in to VIP customer filter logic
- [ ] Document in PHASE-6-sms.md what needs to be done
- [ ] Ensure VIP signup form has phone field (optional)

**Example Stub:**
```typescript
// lib/messaging/SMSService.ts
import { MessageService, VIPBlast, BlastResult } from './MessageService'

export class SMSService implements MessageService {
  async sendVIPBlast(blast: VIPBlast): Promise<BlastResult> {
    // TODO: Implement in Phase 6
    // 1. Install @twilio/sdk
    // 2. Get Twilio credentials
    // 3. Filter recipients by sms_opted_in
    // 4. Send SMS with message + photo URL
    // 5. Handle Twilio errors
    throw new Error('SMS not implemented yet')
  }
}
```

---

## 🚧 BLOCKERS

- Phase 1 must complete first
- Need SendGrid account (user creates during Phase 2)

---

## 📊 PROGRESS TRACKER

**Overall:** 0% Complete

| Task | Status | Progress | Time Spent |
|------|--------|----------|------------|
| Authentication | ⚪ Not Started | 0% | 0h |
| MessageService | ⚪ Not Started | 0% | 0h |
| EmailService | ⚪ Not Started | 0% | 0h |
| VIP Blast UI | ⚪ Not Started | 0% | 0h |
| Unsubscribe | ⚪ Not Started | 0% | 0h |
| SMS Hooks | ⚪ Not Started | 0% | 0h |

**Estimated Total:** 13 hours

---

## 🔗 DEPENDENCIES

**Required Before Starting:**
- Phase 1 complete (admin UI exists)
- Supabase project set up
- VIP customers table populated

**Required During Phase:**
- SendGrid account (free tier)
- Test email addresses (3-5)

---

## 📝 NOTES

### SendGrid Free Tier Limits
- 100 emails/day (enough for weekend business)
- 2,000 contacts
- 14-day email activity history

### CAN-SPAM Compliance Checklist
- ✅ Include physical address (footer)
- ✅ Include unsubscribe link (one-click)
- ✅ Process unsubscribes within 10 days
- ✅ Don't use misleading subject lines
- ✅ Identify message as advertisement (if selling)

### Email Best Practices
- Subject line <50 characters
- Personalize with customer name
- Include clear call-to-action
- Mobile-responsive design
- Test on multiple email clients (Gmail, Outlook, Apple Mail)

### Future SMS Preparation
- Phone field optional at signup (not required yet)
- sms_opted_in defaults to false (explicit opt-in needed)
- When Phase 6 starts, we just implement SMSService
- No refactoring needed

---

## 🔄 CHANGE LOG

### 2026-04-08
- Created Phase 2 plan
- Defined MessageService interface for future SMS
- Planned SendGrid email implementation
- Added unsubscribe for legal compliance
- Added SMS preparation tasks

---

*Next Update: When Phase 2 starts*