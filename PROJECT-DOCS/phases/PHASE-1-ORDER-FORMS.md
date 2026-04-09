# PHASE 1: ORDER FORMS SPECIFICATION
**Critical Feature:** Customer order request forms (NO payment)
**Last Updated:** 2026-04-08

---

## 🎯 CRITICAL REQUIREMENTS

### **No Payment System** (At Launch)
- This is an ORDER REQUEST system, not a payment system
- Customers request orders, owner confirms manually
- Creates personal touch and manages kitchen flow
- Payment integration is Phase 5 (future)

### **Pricing IS Displayed** ✅ CORRECTED
- **PRICING SHOWN:** Menu and order form display prices
- **QUANTITY SHOWN:** Shows available order count (e.g., "10 available")
- **Creates Urgency:** "Brisket - $18 (Only 3 left!)"
- **Real-Time Updates:** Quantity decreases as orders confirmed

### **Mobile-Friendly FIRST**
- Primary use case: customer on phone
- Then make responsive for tablet/desktop
- Large tap targets, easy to use while hungry

### **Two Separate Forms**
1. **Weekend Specials** - Quick pickup, same-day or next-day
2. **Catering Request** - Advance orders, larger quantities

---

## 📱 WEEKEND ORDER FORM

### **Purpose**
Quick orders for weekend BBQ pickup (Friday-Sunday)

### **Form Design (Mobile-First)**

```
┌─────────────────────────────────────┐
│  🔥 ORDER WEEKEND SPECIALS 🔥       │
├─────────────────────────────────────┤
│                                     │
│  THIS WEEKEND'S MENU:               │
│                                     │
│  ○ Smokehouse Rib Dinner - $17     │
│    ⚡ 8 orders available             │
│    Quantity: [_]                    │
│                                     │
│  ○ Chicken Wingettes - $16          │
│    ⚡ 12 orders available            │
│    Quantity: [_]                    │
│                                     │
│  ⊗ Brisket Sandwich - $17.50       │
│    🚫 SOLD OUT                      │
│    (grayed out, disabled)           │
│                                     │
│  ○ Brisket (1/2 Slab) - $30         │
│    ⚡ 5 orders available             │
│    Quantity: [_]                    │
│                                     │
│  ○ Hotlinks - $8.00                 │
│    ⚡ Only 2 left!                   │
│    Quantity: [_]                    │
│                                     │
│  SIDES (optional):                  │
│  ☐ Mac & Cheese                     │
│  ☐ Coleslaw                         │
│  ☐ Baked Beans                      │
│                                     │
│  ─────────────────────────────────  │
│                                     │
│  YOUR INFORMATION:                  │
│  Name:  _____________________       │
│  Email: _____________________       │
│  Phone: _____________________       │
│                                     │
│  PICKUP DAY:                        │
│  ○ Friday  ○ Saturday  ○ Sunday     │
│                                     │
│  PICKUP TIME:                       │
│  ○ 11am-1pm  ○ 1pm-3pm  ○ 3pm-5pm  │
│                                     │
│  SPECIAL REQUESTS:                  │
│  ┌─────────────────────────────┐   │
│  │ [Textarea for notes]        │   │
│  └─────────────────────────────┘   │
│                                     │
│  [Submit Order Request]             │
│  (Large button, 44px+ height)       │
│                                     │
│  Note: You'll receive confirmation  │
│  via email once your order is       │
│  confirmed. Please allow 2-4 hours. │
└─────────────────────────────────────┘
```

### **Form Fields**

#### **Menu Items** (Dynamic from Supabase)
- **Type:** Radio buttons OR checkboxes (user can select multiple)
- **Display:**
  - Available: Normal text, enabled, show price and quantity
  - Sold Out: Grayed text, disabled, show "SOLD OUT"
  - Low Stock: Highlight urgency "Only X left!"
- **Quantity:** Number input next to each item
- **PRICING:** Show price per unit (e.g., "$17.00 each", "$30.00 per 1/2 slab")
- **Availability:** Show quantity available (e.g., "8 orders available", "Only 2 left!")

#### **Customer Info** (Required)
- Name: Text input, required
- Email: Email input, required (for confirmation)
- Phone: Tel input, required (for pickup coordination)

#### **Pickup Details**
- Day: Radio buttons (Friday, Saturday, Sunday)
- Time: Radio buttons (11am-1pm, 1pm-3pm, 3pm-5pm)

#### **Special Requests**
- Textarea, optional
- Placeholder: "Any special requests or dietary needs?"

### **Form Validation**
- At least one menu item selected
- Quantity > 0 for selected items
- Name, email, phone required
- Valid email format
- Valid phone format (XXX-XXX-XXXX)
- Pickup day and time required

### **Submit Behavior**
1. Validate all fields
2. Show loading spinner
3. Save to Supabase `orders` table
4. Show success message: "Order request received! We'll confirm via email within 2-4 hours."
5. Clear form
6. Optionally: redirect to confirmation page

---

## 🍽️ CATERING REQUEST FORM (SIMPLIFIED)

### **Purpose**
Simple catering inquiry form - NO menu, just contact info + message

### **Form Design (Mobile-First)**

```
┌─────────────────────────────────────┐
│  🎉 CATERING REQUEST 🎉             │
├─────────────────────────────────────┤
│                                     │
│  Let us make your event special!    │
│  We cater parties, corporate events,│
│  weddings, and more.                │
│                                     │
│  YOUR INFORMATION:                  │
│  Name:  _____________________       │
│            (required)               │
│                                     │
│  Email: _____________________       │
│            (required)               │
│                                     │
│  Phone: _____________________       │
│            (required)               │
│                                     │
│  ─────────────────────────────────  │
│                                     │
│  EVENT DETAILS:                     │
│  ┌─────────────────────────────┐   │
│  │ Tell us about your event:   │   │
│  │                             │   │
│  │ Please include:             │   │
│  │ - Event date and time       │   │
│  │ - Number of guests          │   │
│  │ - Menu preferences          │   │
│  │ - Delivery or pickup        │   │
│  │ - Budget (optional)         │   │
│  │                             │   │
│  │ [Large textarea]            │   │
│  └─────────────────────────────┘   │
│            (required)               │
│                                     │
│  [Submit Catering Request]          │
│  (Large button, 44px+ height)       │
│                                     │
│  We'll contact you within 24 hours  │
│  with a custom quote!               │
└─────────────────────────────────────┘
```

### **Form Fields** (SIMPLIFIED)

#### **Customer Info** (All Required)
- Name: Text input, required
- Email: Email input, required
- Phone: Tel input, required

#### **Event Details** (Required)
- Message: Large textarea, required
- Placeholder text:
  ```
  Tell us about your event!

  Please include:
  - Event date and time
  - Number of guests
  - What you'd like to serve (ribs, brisket, sides, etc.)
  - Delivery address or pickup
  - Your budget (optional but helpful)
  - Any special requests
  ```

### **Form Validation**
- Name, email, phone required
- Email format valid
- Phone format valid
- Message not empty (min 20 characters)
- That's it! Much simpler.

### **Submit Behavior**
1. Validate all fields
2. Show loading spinner
3. Save to Supabase `catering_requests` table
4. Show success: "Catering request received! We'll send you a custom quote within 24 hours."
5. Send notification to owner (email alert)
6. Clear form

---

## 🗄️ DATABASE SCHEMA ADDITIONS

### **orders table** (weekend orders)
```sql
CREATE TABLE orders (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

  -- Order items (JSONB array)
  items JSONB NOT NULL,
  -- Example: [
  --   {menu_item_id: 'uuid', name: 'Brisket', quantity: 2, unit: 'lb'},
  --   {menu_item_id: 'uuid', name: 'Ribs', quantity: 1, unit: 'slab'}
  -- ]

  -- Customer info
  customer_name TEXT NOT NULL,
  customer_email TEXT NOT NULL,
  customer_phone TEXT NOT NULL,

  -- Pickup details
  pickup_day TEXT NOT NULL, -- 'Friday', 'Saturday', 'Sunday'
  pickup_time TEXT NOT NULL, -- '11am-1pm', '1pm-3pm', '3pm-5pm'
  special_requests TEXT,

  -- Status tracking
  status TEXT DEFAULT 'pending', -- 'pending', 'confirmed', 'completed', 'cancelled'
  confirmed_at TIMESTAMP,
  confirmed_by TEXT, -- 'owner' (from Google Sheets)

  -- Pricing (for Phase 5 - payments)
  total_amount DECIMAL(10,2), -- null until pricing enabled
  payment_status TEXT, -- null until payments enabled

  -- Metadata
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### **catering_requests table**
```sql
CREATE TABLE catering_requests (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

  -- Event details
  event_date DATE NOT NULL,
  event_time TIME NOT NULL,
  guest_count INTEGER NOT NULL,
  event_type TEXT NOT NULL,
  event_type_other TEXT, -- if event_type = 'Other'

  -- Menu selection (JSONB)
  menu_items JSONB NOT NULL,
  -- Example: {
  --   meats: [{name: 'Brisket', quantity: 10, unit: 'lb'}, ...],
  --   sides: [{name: 'Mac & Cheese', servings: 20}, ...]
  -- }

  -- Delivery/Pickup
  delivery_method TEXT NOT NULL, -- 'pickup' or 'delivery'
  delivery_address TEXT,
  delivery_city TEXT,
  delivery_zip TEXT,

  -- Customer info
  customer_name TEXT NOT NULL,
  customer_email TEXT NOT NULL,
  customer_phone TEXT NOT NULL,
  company_name TEXT,

  -- Budget and notes
  budget_estimate DECIMAL(10,2),
  notes TEXT,

  -- Status tracking
  status TEXT DEFAULT 'pending', -- 'pending', 'quoted', 'confirmed', 'completed', 'cancelled'
  quoted_at TIMESTAMP,
  quote_amount DECIMAL(10,2),
  confirmed_at TIMESTAMP,

  -- Metadata
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

---

## 🎨 UI/UX REQUIREMENTS

### **Mobile-Friendly First**
- Design for 375px width minimum (iPhone SE)
- Large tap targets (44px × 44px minimum)
- Easy to scroll with thumb
- Form fields stack vertically
- No horizontal scrolling

### **Responsive Breakpoints**
- Mobile: 375px - 767px (stacked, full-width)
- Tablet: 768px - 1023px (slight padding, centered)
- Desktop: 1024px+ (max-width container, centered)

### **Form Styling**
- Clear labels above each field
- Placeholder text for guidance
- Validation errors inline (red text below field)
- Success messages prominent (green, top of form)
- Loading states (disable submit button, show spinner)

### **"Sold Out" Display**
```css
/* Available item */
.menu-item-available {
  opacity: 1;
  cursor: pointer;
}

/* Sold out item */
.menu-item-sold-out {
  opacity: 0.5;
  cursor: not-allowed;
  text-decoration: line-through;
}

.menu-item-sold-out::after {
  content: " - SOLD OUT";
  color: red;
  font-weight: bold;
}
```

### **Accessibility**
- Form labels for screen readers
- Proper input types (email, tel, number, date, time)
- Focus states visible
- Error messages announced
- Keyboard navigation works

---

## 🔄 WORKFLOW

### **Weekend Order Flow**
```
Customer fills weekend form
    ↓
Form validation (client-side)
    ↓
Submit → Supabase.orders.insert()
    ↓
Success message shown
    ↓
(Phase 3) Order appears in Google Sheets
    ↓
(Phase 3) Owner checks box in Sheets
    ↓
(Phase 3) Webhook → Update Supabase status
    ↓
(Phase 3) Confirmation email sent
```

### **Catering Request Flow**
```
Customer fills catering form
    ↓
Form validation (client-side + event date check)
    ↓
Submit → Supabase.catering_requests.insert()
    ↓
Success message shown
    ↓
Email alert sent to owner (Phase 2)
    ↓
Owner reviews request
    ↓
Owner sends custom quote (manual email for now)
    ↓
(Future) Customer accepts quote → becomes order
```

---

## 📋 PHASE 1 DELIVERABLES

- [ ] Weekend order form (mobile-friendly, no pricing)
- [ ] Catering request form (mobile-friendly, no pricing)
- [ ] "Sold Out" display works correctly
- [ ] Forms save to Supabase
- [ ] Client-side validation works
- [ ] Success messages display
- [ ] Error handling graceful
- [ ] Tested on mobile devices (iOS + Android)
- [ ] Responsive design works on tablet/desktop

---

*Last Updated: 2026-04-08*
*Related: PHASE-1-frontend.md, PHASE-3-integration.md*