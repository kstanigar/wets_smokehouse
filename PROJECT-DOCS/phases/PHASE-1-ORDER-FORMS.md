# PHASE 1: ORDER FORMS SPECIFICATION
**Critical Feature:** Customer order request forms (NO payment)
**Last Updated:** 2026-04-08

---

## 🎯 CRITICAL REQUIREMENTS

### **No Payment System**
- This is an ORDER REQUEST system, not a payment system
- Customers request orders, owner confirms manually
- Creates personal touch and manages kitchen flow

### **No Pricing Displayed**
- Menu items shown WITHOUT prices on order form
- Pricing only shown in admin/menu display (if at all)
- Keeps focus on the food, not the cost

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
│  ○ Brisket (per pound)              │
│    Quantity: [_] lbs                │
│                                     │
│  ○ Ribs (half slab)                 │
│    Quantity: [_]                    │
│                                     │
│  ⊗ Pulled Pork - SOLD OUT          │
│    (grayed out, disabled)           │
│                                     │
│  ○ Sausage (per link)               │
│    Quantity: [_]                    │
│                                     │
│  ○ Hotlinks                         │
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
  - Available: Normal text, enabled
  - Sold Out: Grayed text, disabled, show "SOLD OUT"
- **Quantity:** Number input next to each item
- **NO PRICING:** Do not show prices

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

## 🍽️ CATERING ORDER FORM

### **Purpose**
Advance orders for catering events (parties, gatherings, corporate)

### **Form Design (Mobile-First)**

```
┌─────────────────────────────────────┐
│  🎉 REQUEST CATERING 🎉             │
├─────────────────────────────────────┤
│                                     │
│  EVENT INFORMATION:                 │
│  Event Date: [Date picker]          │
│  Event Time: [Time picker]          │
│  Guest Count: [___] people          │
│  Event Type:                        │
│    ○ Birthday  ○ Corporate          │
│    ○ Wedding   ○ Other: ______      │
│                                     │
│  ─────────────────────────────────  │
│                                     │
│  CATERING MENU:                     │
│                                     │
│  MEATS:                             │
│  ☐ Brisket (estimate lbs): [_]      │
│  ☐ Ribs (estimate slabs): [_]       │
│  ☐ Pulled Pork (lbs): [_]           │
│  ☐ Sausage (count): [_]             │
│  ☐ Chicken (pieces): [_]            │
│                                     │
│  SIDES:                             │
│  ☐ Mac & Cheese (serves): [_]       │
│  ☐ Coleslaw (serves): [_]           │
│  ☐ Baked Beans (serves): [_]        │
│  ☐ Cornbread (serves): [_]          │
│  ☐ Potato Salad (serves): [_]       │
│                                     │
│  ─────────────────────────────────  │
│                                     │
│  DELIVERY/PICKUP:                   │
│  ○ I'll pick up                     │
│  ○ Please deliver                   │
│                                     │
│  If delivery:                       │
│  Address: ____________________      │
│           ____________________      │
│  City/Zip: ___________________      │
│                                     │
│  ─────────────────────────────────  │
│                                     │
│  YOUR INFORMATION:                  │
│  Name:  _____________________       │
│  Email: _____________________       │
│  Phone: _____________________       │
│  Company (opt): ______________      │
│                                     │
│  BUDGET/NOTES:                      │
│  ┌─────────────────────────────┐   │
│  │ Budget: $_______            │   │
│  │                             │   │
│  │ Tell us about your event:   │   │
│  │ [Textarea]                  │   │
│  └─────────────────────────────┘   │
│                                     │
│  [Submit Catering Request]          │
│                                     │
│  Note: We'll contact you within     │
│  24 hours with a custom quote.      │
└─────────────────────────────────────┘
```

### **Form Fields**

#### **Event Info**
- Event Date: Date picker (min: 7 days from now)
- Event Time: Time picker
- Guest Count: Number input, required
- Event Type: Radio buttons + "Other" text input

#### **Menu Selection** (Checkboxes)
- Meats: Checkboxes with quantity inputs
- Sides: Checkboxes with serving count inputs
- NO PRICING: Prices provided in quote later

#### **Delivery/Pickup**
- Radio: Pickup or Delivery
- If delivery: Address fields (required)

#### **Customer Info**
- Name, Email, Phone (required)
- Company name (optional)

#### **Budget/Notes**
- Budget: Optional number input (helps owner quote)
- Notes: Textarea for event details

### **Form Validation**
- Event date at least 7 days out (configurable)
- Guest count > 0
- At least one meat or side selected
- If delivery selected, address required
- Name, email, phone required

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