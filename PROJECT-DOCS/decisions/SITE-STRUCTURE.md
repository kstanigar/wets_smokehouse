# SITE STRUCTURE - WET SMOKEHOUSE
**Last Updated:** 2026-04-08
**Purpose:** Define page structure and navigation

---

## 📅 RECENT UPDATES

### 2026-04-08: Site Structure Defined
- 4 main pages: Home, About Us, Menu, Catering
- Menu = Home (shows current week's offerings with pricing/quantities)
- Two action buttons: "Order" and "Catering Request"
- Reference menu image: wet_menu.jpg in assets/

---

## 🗺️ SITE STRUCTURE

### **Navigation**
```
┌─────────────────────────────────────┐
│  🔥 WET SMOKEHOUSE                  │
│  [Home] [About Us] [Menu] [Catering]│
└─────────────────────────────────────┘
```

---

## 📄 PAGES

### **1. HOME (Menu Display)**
**URL:** `/` or `/home`
**Purpose:** Show this weekend's menu with pricing and available quantities

**Content:**
- Hero section: "This Weekend's Smokehouse Special"
- Hours: (from wet_menu.jpg)
- Current menu items with:
  - Item name
  - Price (per lb, per slab, etc.)
  - **Quantity available** (e.g., "10 available")
  - Description
  - Photo
- Two prominent CTAs:
  - **"Order Now"** button → Weekend order form
  - **"Request Catering"** button → Catering form
- VIP signup form (footer)

**Example Display:**
```
┌─────────────────────────────────────┐
│  🔥 THIS WEEKEND'S MENU 🔥          │
├─────────────────────────────────────┤
│                                     │
│  SMOKEHOUSE RIB DINNER              │
│  $17.00                             │
│  ⚡ 8 orders available               │
│  Includes 2 sides and a soft drink  │
│  [Photo]                            │
│                                     │
│  GRILLED CHICKEN WINGETTES          │
│  $16.00                             │
│  ⚡ 12 orders available              │
│  Includes 2 sides and a soft drink  │
│  [Photo]                            │
│                                     │
│  SMOKED BRISKET SANDWICH            │
│  $17.50                             │
│  🚫 SOLD OUT                        │
│  Includes 2 sides and a soft drink  │
│  [Photo - grayed out]               │
│                                     │
│  [Order Now] [Request Catering]     │
└─────────────────────────────────────┘
```

**Dynamic Content:**
- Fetches from Supabase `menu_items` where `available = true`
- Shows real-time quantity
- Updates when orders confirmed (quantity decrements)
- "Sold Out" badge when `quantity_available = 0` or `available = false`

---

### **2. ABOUT US**
**URL:** `/about`
**Purpose:** Tell the story of WET SMOKEHOUSE

**Content:**
- Owner's story
- BBQ philosophy
- Photos of smoker/kitchen
- Awards/recognition (if any)
- "Why weekend-only" explanation
- Contact info

**Keep Simple:**
- 2-3 paragraphs max
- 2-3 photos
- Mobile-friendly
- Link to order form at bottom

---

### **3. MENU (Detailed View)**
**URL:** `/menu`
**Purpose:** Full menu with all items, pricing, descriptions

**This might be same as HOME page** - or could be:
- More detailed descriptions
- Full menu (even sold-out items shown)
- Nutritional info (if available)
- Allergen info
- Sides options detail

**Note:** User said "Home (menu)" so Home and Menu might be the SAME page

---

### **4. ORDER (Weekend Order Form)**
**URL:** `/order`
**Purpose:** Weekend order submission form

**Content:**
- Weekend order form (see PHASE-1-ORDER-FORMS.md)
- Shows menu items with pricing and quantities
- Customer selects items and quantities
- Customer info fields
- Pickup day/time selection
- Submit button

**Access:**
- Linked from "Order Now" button on Home/Menu
- Direct navigation link in header

---

### **5. CATERING (Simple Request Form)**
**URL:** `/catering`
**Purpose:** Catering inquiry form

**Content:**
- **NO MENU** - just a simple form
- Brief intro text: "Let us cater your next event!"
- Form fields:
  - Name (required)
  - Email (required)
  - Phone (required)
  - Message/Details (required textarea)
    - Placeholder: "Tell us about your event (date, guest count, menu preferences, etc.)"
- Submit button: "Submit Catering Request"

**Much Simpler Than Planned:**
- No event date picker
- No menu checkboxes
- No delivery address fields
- Just: Name, Email, Phone, Message
- Owner will call/email to discuss details

**Example:**
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
│  Email: _____________________       │
│  Phone: _____________________       │
│                                     │
│  EVENT DETAILS:                     │
│  ┌─────────────────────────────┐   │
│  │ Tell us about your event:   │   │
│  │ (Date, guest count, menu    │   │
│  │  preferences, budget, etc.) │   │
│  │                             │   │
│  │ [Textarea]                  │   │
│  └─────────────────────────────┘   │
│                                     │
│  [Submit Catering Request]          │
│                                     │
│  We'll contact you within 24 hours  │
│  with a custom quote!               │
└─────────────────────────────────────┘
```

---

### **6. ADMIN (Backend)**
**URL:** `/admin`
**Purpose:** Owner management interface

**Protected:** Requires authentication (Supabase Auth)

**Content:**
- Menu management (see updated spec below)
- VIP email blast
- Link to Google Sheets

---

## 🍖 MENU ITEMS (From wet_menu.jpg)

Based on the menu image, here are the items to include:

### **Main Items (Dinners/Sandwiches)**
1. **Smokehouse Rib Dinner** - $17.00
   - Includes 2 sides and a soft drink

2. **Grilled Chicken Wingettes** - $16.00
   - Includes 2 sides and a soft drink

3. **Grilled Harlinks Dinner** - $16.00
   - Includes 2 sides and a soft drink

4. **Smoked Brisket Sandwich** - $17.50
   - Includes 2 sides and a soft drink

### **Additional Meat Options**
5. **Brisket (1/2 Slab)** - $30.00

6. **Ribs (1/2 Slab)** - $35.00

7. **Hotlinks** - $8.00

8. **Chicken Winglets** - $6.00

### **Drinks**
9. **Slick Ribbon $4.00** (need clarification on what this is)

10. **Beef Babe Plaka Cake** - $5.50 (need clarification)

11. **Chicken Cake** - $6.00 (need clarification)

12. **Cuidwirks Cake** - $4.50 (need clarification)

**Note:** Some items on menu image are hard to read - will need owner to clarify exact names and descriptions.

---

## 🎨 NAVIGATION BEHAVIOR

### **Mobile Navigation (Hamburger)**
- Keep existing Munchos hamburger menu
- Update links:
  - Home → `/`
  - About Us → `/about`
  - Menu → `/menu` (or same as Home)
  - Order → `/order`
  - Catering → `/catering`

### **Desktop Navigation (Horizontal)**
- Full nav bar at top
- Logo on left
- Links on right
- "Order Now" as CTA button (highlighted)

### **Footer (All Pages)**
- VIP email signup form
- Social media links
- Contact info
- Hours
- Address (if physical location)
- Copyright

---

## 🔄 USER FLOWS

### **Flow 1: Weekend Order**
```
Home page
  ↓ (sees menu with prices and quantities)
Click "Order Now" button
  ↓
Order form page (/order)
  ↓
Fill out form (select items, pickup time, etc.)
  ↓
Submit
  ↓
Success message: "We'll confirm via email within 2-4 hours"
  ↓
(Backend) Order → Supabase → Google Sheets
  ↓
Owner confirms via checkbox
  ↓
Customer receives confirmation email
```

### **Flow 2: Catering Request**
```
Home page OR Catering nav link
  ↓
Click "Request Catering" button
  ↓
Catering form page (/catering)
  ↓
Fill out simple form (name, email, phone, message)
  ↓
Submit
  ↓
Success message: "We'll contact you within 24 hours"
  ↓
(Backend) Request saved to Supabase
  ↓
Owner receives email notification
  ↓
Owner calls/emails customer to discuss
```

### **Flow 3: VIP Signup**
```
Any page (footer)
  ↓
Fill out VIP form (name, email)
  ↓
Submit
  ↓
Success message: "You're on the list!"
  ↓
Saved to Supabase vip_customers
  ↓
Appears in Google Sheets "VIP List" tab
  ↓
Receives VIP email blasts
```

---

## 📱 RESPONSIVE BEHAVIOR

### **Mobile (375px - 767px)**
- Hamburger menu
- Stacked layout
- Full-width menu cards
- Large tap targets for buttons

### **Tablet (768px - 1023px)**
- May keep hamburger or show horizontal nav
- 2-column menu grid
- Larger images

### **Desktop (1024px+)**
- Horizontal navigation
- 3-column menu grid
- Max-width container (1200px)
- Centered content

---

## 🔄 CHANGE LOG

### 2026-04-08
- Defined 4 main pages
- Home = Menu display with pricing and quantities
- Simplified catering to just a message form
- Identified menu items from wet_menu.jpg
- Defined user flows

---

*Next Update: When site structure changes*
*Reference: wet_menu.jpg for menu items and pricing*