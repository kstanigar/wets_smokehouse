# PHASE 3: Google Sheets Integration
**Duration:** Week 3 (7 days)
**Status:** ⚪ Waiting (Phases 1-2 must complete first)
**Budget:** $0 (Google Sheets API is free)
**Goal:** **CRITICAL** - Orders flow to Google Sheets, owner confirms via checkbox

---

## 🎯 TL;DR

**CRITICAL FEATURE:** Google Sheets is the primary order confirmation workflow
Orders appear in Sheets in real-time
Owner taps checkbox in Sheets to confirm order
Webhook triggers confirmation email to customer
This is NOT read-only - owner actively uses Sheets

---

## 📅 RECENT UPDATES (Newest First)

### 2026-04-08: Phase Defined with Critical Google Sheets Workflow
- Google Sheets is CRITICAL to project success
- Owner confirms ALL orders via checkbox in Sheets
- NOT read-only access - owner writes to Sheets
- Checkbox triggers webhook → customer confirmation
- Must work on mobile (owner's phone)

---

## ✅ GOALS

1. **Orders flow to Google Sheets in real-time** (weekend + catering)
2. **Owner confirms orders via checkbox** (primary workflow)
3. **Checkbox triggers webhook** → updates Supabase
4. **Customer gets confirmation email** automatically
5. **Menu updates sync to Sheets** (optional view for owner)
6. **VIP signups appear in Sheets** (owner can see list)
7. **Works perfectly on mobile** (Google Sheets app)

**Success Criteria:**
- Order appears in Sheets within 5 seconds of submission
- Owner can tap checkbox on phone
- Checkbox triggers confirmation email to customer
- Customer receives email within 30 seconds of checkbox
- "Sold Out" status syncs to frontend when owner unchecks menu item
- Zero manual data entry for owner

---

## 🚧 CRITICAL IMPORTANCE

### **Why Google Sheets is Critical:**

1. **Owner Already Knows It**
   - Zero learning curve
   - Works on phone (Google Sheets app)
   - Familiar checkbox interface

2. **Personal Touch**
   - Owner sees every order
   - Manual confirmation creates connection
   - Manages kitchen flow (can pause if overwhelmed)

3. **Simple Mobile Workflow**
   - Open Sheets app on phone
   - See new order (highlighted row)
   - Tap checkbox
   - Done (customer gets confirmation)

4. **Data Export**
   - Owner can export to Excel anytime
   - Keep records for taxes
   - Analyze order patterns

5. **Backup System**
   - If website goes down, orders still in Sheets
   - Owner can manually contact customers
   - No lost orders

---

## 📋 TASKS

### Task 1: Google Sheets API Setup
**Status:** ⚪ Not Started
**Estimate:** 2 hours
**Approval Required:** Yes (show Sheets structure first)

**Requirements:**
- Create Google Sheet: "WET SMOKEHOUSE Orders"
- Three tabs: "Weekend Orders", "Catering Requests", "VIP List"
- Set up Google Service Account
- Enable Google Sheets API
- Grant service account write access to sheet

**Implementation:**
- [ ] Create Google Cloud project
- [ ] Enable Google Sheets API
- [ ] Create service account
- [ ] Download credentials JSON (add to .env, DO NOT commit)
- [ ] Create Google Sheet
- [ ] Share sheet with service account email
- [ ] Install: `npm install googleapis`
- [ ] Test read/write access

**Approval Point:** Show Sheets structure before creating

---

### Task 2: Weekend Orders Sheet Structure
**Status:** ⚪ Not Started
**Estimate:** 1 hour
**Approval Required:** No (structure defined below)

**Tab: "Weekend Orders"**

| Column | Header | Type | Notes |
|--------|--------|------|-------|
| A | ✓ | Checkbox | **Owner taps here to confirm** |
| B | Order # | Text | Auto-generated ID |
| C | Date/Time | Timestamp | When order placed |
| D | Customer Name | Text | From form |
| E | Phone | Text | From form (for pickup coordination) |
| F | Email | Text | From form |
| G | Items | Text | "2lb Brisket, 1 Ribs, Mac & Cheese" |
| H | Pickup Day | Text | Friday/Saturday/Sunday |
| I | Pickup Time | Text | 11am-1pm, etc. |
| J | Special Requests | Text | Customer notes |
| K | Status | Formula | ="Confirmed" if A checked, else "Pending" |
| L | Confirmed At | Timestamp | When checkbox tapped |

**Formatting:**
- Header row: Bold, frozen
- New orders: Highlighted yellow background (removed after 24h)
- Confirmed orders: Green background
- Cancelled orders: Red background, strikethrough

**Mobile View:**
- Most important columns first (Checkbox, Name, Items, Pickup)
- Less important can scroll right (Email, Special Requests)

---

### Task 3: Catering Requests Sheet Structure
**Status:** ⚪ Not Started
**Estimate:** 1 hour
**Approval Required:** No (structure defined below)

**Tab: "Catering Requests"**

| Column | Header | Type | Notes |
|--------|--------|------|-------|
| A | ✓ | Checkbox | **Owner reviewed** |
| B | Request # | Text | Auto-generated ID |
| C | Submitted | Timestamp | When request placed |
| D | Event Date | Date | When event is |
| E | Guest Count | Number | Number of people |
| F | Customer Name | Text | From form |
| G | Phone | Text | For quote discussion |
| H | Email | Text | Send quote via email |
| I | Menu Items | Text | "10lb Brisket, Mac for 20, etc." |
| J | Delivery? | Text | "Pickup" or delivery address |
| K | Budget | Currency | Customer's budget estimate |
| L | Notes | Text | Customer's event details |
| M | Status | Dropdown | "New", "Quoted", "Confirmed", "Completed" |
| N | Quote Amount | Currency | Owner enters quote |
| O | Quoted At | Timestamp | When quote sent |

**Workflow:**
- Checkbox = "I've reviewed this"
- Status dropdown: Owner updates manually
- Quote Amount: Owner types in quote
- Follow up happens via phone/email (manual for now)

---

### Task 4: VIP List Sheet Structure
**Status:** ⚪ Not Started
**Estimate:** 30 minutes
**Approval Required:** No

**Tab: "VIP List"**

| Column | Header | Type | Notes |
|--------|--------|------|-------|
| A | Name | Text | From signup form |
| B | Email | Text | For email blasts |
| C | Phone | Text | For future SMS (Phase 6) |
| D | Signed Up | Timestamp | When they joined |
| E | Email Opt-In | Checkbox | Unchecked = unsubscribed |
| F | SMS Opt-In | Checkbox | For Phase 6 |
| G | Last Blast Sent | Timestamp | Track engagement |

**Read-Only for Owner:**
- This tab is just for viewing
- Owner doesn't edit this (managed by system)
- Can export for use in other tools

---

### Task 5: Append Orders to Sheets (Real-Time)
**Status:** ⚪ Not Started
**Estimate:** 3 hours
**Approval Required:** No (technical implementation)

**Requirements:**
- When weekend order submitted → append row to "Weekend Orders"
- When catering request submitted → append row to "Catering Requests"
- Happens instantly (<5 seconds)
- Highlight new rows (yellow background for 24h)

**Implementation:**
- [ ] Create `lib/google-sheets/client.ts` (Google Sheets API wrapper)
- [ ] Create function: `appendWeekendOrder(order)`
- [ ] Create function: `appendCateringRequest(request)`
- [ ] Format data for Sheets (convert JSONB items to readable text)
- [ ] Add timestamp in correct timezone
- [ ] Apply yellow highlight to new rows
- [ ] Test with real order submissions
- [ ] Handle API errors gracefully (retry 3x, then log error)

**Error Handling:**
- If Sheets API fails, still save to Supabase
- Log error to Sentry (or console for now)
- Owner can manually check Supabase if Sheets doesn't update
- Retry logic: 3 attempts with exponential backoff

---

### Task 6: Checkbox Webhook (CRITICAL)
**Status:** ⚪ Not Started
**Estimate:** 4 hours
**Approval Required:** Yes (show flow before implementing)

**Requirements:**
- When owner checks checkbox in Sheets → trigger webhook
- Webhook updates Supabase order status to "confirmed"
- Webhook sends confirmation email to customer
- Works on mobile (Google Sheets app)

**Implementation Options:**

**Option A: Google Apps Script (Recommended)**
```javascript
// In Google Sheet: Extensions > Apps Script
function onEdit(e) {
  const sheet = e.source.getActiveSheet();
  const sheetName = sheet.getName();
  const row = e.range.getRow();
  const col = e.range.getColumn();

  // Check if edit was to checkbox column (A) in Weekend Orders
  if (sheetName === 'Weekend Orders' && col === 1 && row > 1) {
    const isChecked = e.range.getValue();

    if (isChecked === true) {
      // Get order ID from column B
      const orderId = sheet.getRange(row, 2).getValue();

      // Send webhook to our API
      const webhookUrl = 'https://wet-smokehouse.vercel.app/api/webhooks/order-confirmed';
      const payload = {
        orderId: orderId,
        confirmedAt: new Date().toISOString(),
        sheetRow: row
      };

      UrlFetchApp.fetch(webhookUrl, {
        method: 'post',
        contentType: 'application/json',
        payload: JSON.stringify(payload)
      });

      // Update "Confirmed At" column (L)
      sheet.getRange(row, 12).setValue(new Date());

      // Change background to green
      sheet.getRange(row, 1, 1, 12).setBackground('#d4edda');
    }
  }
}
```

**Our Webhook Endpoint:**
- [ ] Create `/api/webhooks/order-confirmed`
- [ ] Verify webhook authenticity (shared secret)
- [ ] Update Supabase: `orders.status = 'confirmed'`
- [ ] Update Supabase: `orders.confirmed_at = NOW()`
- [ ] Send confirmation email to customer
- [ ] Return success response

**Option B: Google Sheets API Polling (Backup)**
- Poll Sheets every 60 seconds for new checkmarks
- Less real-time but simpler
- Use if Apps Script doesn't work on mobile

**Approval Point:** Show webhook flow diagram before implementing

---

### Task 7: Confirmation Email Template
**Status:** ⚪ Not Started
**Estimate:** 1 hour
**Approval Required:** Yes (show template first)

**Requirements:**
- Sent when order confirmed (checkbox checked)
- Professional but friendly tone
- Includes all order details
- Pickup instructions

**Email Template:**
```
Subject: Your WET SMOKEHOUSE Order is Confirmed! 🔥

Hi [Customer Name],

Great news! Your order is confirmed and we're firing up the smoker.

ORDER DETAILS:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Items list]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PICKUP:
Day: [Pickup Day]
Time: [Pickup Time]
Location: [Address - from .env]

SPECIAL REQUESTS:
[Special requests or "None"]

WHAT TO BRING:
- Cooler (recommended for transport)
- Your appetite!

QUESTIONS?
Call or text us at: [Phone - from .env]

We can't wait to serve you this weekend!

- WET SMOKEHOUSE Team

P.S. Want to be first to know about weekend specials?
[Link to VIP signup if not already signed up]
```

**Implementation:**
- [ ] Create email template file
- [ ] Use SendGrid (from Phase 2)
- [ ] Populate with order data
- [ ] Include unsubscribe link (legal requirement)
- [ ] Test with real email address

**Approval Point:** Show email mockup before sending

---

### Task 8: Menu Sync to Sheets (Optional)
**Status:** ⚪ Not Started
**Estimate:** 1 hour
**Approval Required:** No

**Requirements:**
- When owner updates menu in admin → sync to "Menu" tab in Sheets
- Gives owner quick view of current menu status
- Read-only tab (just for reference)

**Tab: "Current Menu"**

| Column | Header | Type |
|--------|--------|------|
| A | Item | Text |
| B | Available | Checkbox |
| C | Price | Currency |
| D | Updated At | Timestamp |

**Implementation:**
- [ ] Create "Current Menu" tab
- [ ] After menu update in admin → update Sheets
- [ ] Owner can glance at Sheets to see what's available
- [ ] Useful for phone calls ("What do you have today?")

---

## 🚧 BLOCKERS

- Phase 1 must complete (order forms exist)
- Phase 2 must complete (email service works)
- Google Cloud account needed (user may need to create)
- Google Sheet must be created and shared with service account

---

## 📊 PROGRESS TRACKER

**Overall:** 0% Complete

| Task | Status | Progress | Time Spent |
|------|--------|----------|------------|
| Google API Setup | ⚪ Not Started | 0% | 0h |
| Sheet Structures | ⚪ Not Started | 0% | 0h |
| Append to Sheets | ⚪ Not Started | 0% | 0h |
| Checkbox Webhook | ⚪ Not Started | 0% | 0h |
| Confirmation Email | ⚪ Not Started | 0% | 0h |
| Menu Sync | ⚪ Not Started | 0% | 0h |

**Estimated Total:** 12.5 hours

---

## 🔗 DEPENDENCIES

**Required Before Starting:**
- Phase 1 complete (order forms working)
- Phase 2 complete (email service ready)
- Supabase orders table populated (test data)

**Required During Phase:**
- Google Cloud account
- Google Sheet created and shared
- Service account credentials (.env)

---

## 📝 NOTES

### **Mobile Experience (Critical)**
- Google Sheets app works great on iPhone/Android
- Checkboxes are tappable (good touch target)
- Owner can see all columns by scrolling right
- Notifications possible (Google Sheets can notify on changes)

### **Alternative: Google Forms for Orders**
User chose custom order forms (better UX)
Google Forms could be backup if custom forms too complex

### **Scaling Considerations**
- Google Sheets has 10 million cell limit
- Each order = ~12 cells
- Can handle 800K+ orders before hitting limit
- Archive old orders to new sheet annually

### **Security**
- Service account has write-only access to this specific sheet
- Owner's Google account owns the sheet
- Webhook endpoint protected with shared secret
- No customer payment info in Sheets (just order details)

---

## 🔄 CHANGE LOG

### 2026-04-08
- Created Phase 3 plan
- Defined Google Sheets as CRITICAL workflow
- Designed sheet structures (Weekend, Catering, VIP)
- Planned checkbox webhook system
- Created confirmation email template

---

*Next Update: When Phase 3 starts*
*Related: PHASE-1-ORDER-FORMS.md, PHASE-2-backend.md*