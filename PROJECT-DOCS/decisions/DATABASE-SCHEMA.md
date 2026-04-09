# DATABASE SCHEMA - WET SMOKEHOUSE
**Last Updated:** 2026-04-08
**Database:** Supabase (PostgreSQL)
**Purpose:** Define all tables and relationships

---

## 📅 RECENT UPDATES

### 2026-04-08: Added Quantity Tracking & Pricing
- menu_items.quantity_available (tracks inventory)
- menu_items.price (owner sets, persists week-to-week)
- Auto sold-out when quantity = 0
- Quantity decrements when order confirmed

---

## 🗄️ TABLES

### **1. menu_items**
**Purpose:** Store menu items with pricing, availability, and quantity tracking

```sql
CREATE TABLE menu_items (
  -- Primary key
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

  -- Item details
  name TEXT NOT NULL,
  description TEXT,
  category TEXT NOT NULL, -- 'dinner', 'additional_meat', 'drink', 'side'

  -- Pricing & inventory
  price DECIMAL(10,2) NOT NULL, -- e.g., 17.00 for Rib Dinner
  unit TEXT, -- e.g., 'each', '1/2 slab', 'per drink'
  quantity_available INTEGER DEFAULT 0, -- How many orders available

  -- Availability
  available BOOLEAN DEFAULT false, -- Owner toggle (manual override)
  -- Note: Item is sold out if available=false OR quantity_available=0

  -- Display
  image_url TEXT,
  sort_order INTEGER DEFAULT 0, -- Display order on menu
  featured BOOLEAN DEFAULT false, -- Show prominently

  -- Metadata
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_menu_items_available ON menu_items(available);
CREATE INDEX idx_menu_items_category ON menu_items(category);

-- Example data
INSERT INTO menu_items (name, description, category, price, unit, quantity_available, available, sort_order) VALUES
('Smokehouse Rib Dinner', 'Includes 2 sides and a soft drink', 'dinner', 17.00, 'each', 10, true, 1),
('Grilled Chicken Wingettes', 'Includes 2 sides and a soft drink', 'dinner', 16.00, 'each', 12, true, 2),
('Smoked Brisket Sandwich', 'Includes 2 sides and a soft drink', 'dinner', 17.50, 'each', 0, false, 3),
('Brisket', '', 'additional_meat', 30.00, '1/2 slab', 5, true, 10),
('Ribs', '', 'additional_meat', 35.00, '1/2 slab', 8, true, 11),
('Hotlinks', '', 'additional_meat', 8.00, 'each', 15, true, 12),
('Chicken Winglets', '', 'additional_meat', 6.00, 'each', 20, true, 13);
```

---

### **2. orders** (Weekend Orders)
**Purpose:** Store customer weekend orders

```sql
CREATE TABLE orders (
  -- Primary key
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  order_number TEXT UNIQUE NOT NULL, -- e.g., 'WK-1001'

  -- Order items (JSONB array)
  items JSONB NOT NULL,
  -- Example: [
  --   {
  --     menu_item_id: 'uuid',
  --     name: 'Smokehouse Rib Dinner',
  --     quantity: 2,
  --     price_at_time: 17.00,
  --     unit: 'each',
  --     subtotal: 34.00
  --   },
  --   {
  --     menu_item_id: 'uuid',
  --     name: 'Brisket',
  --     quantity: 1,
  --     price_at_time: 30.00,
  --     unit: '1/2 slab',
  --     subtotal: 30.00
  --   }
  -- ]

  -- Customer info
  customer_name TEXT NOT NULL,
  customer_email TEXT NOT NULL,
  customer_phone TEXT NOT NULL,

  -- Pickup details
  pickup_day TEXT NOT NULL, -- 'Friday', 'Saturday', 'Sunday'
  pickup_time TEXT NOT NULL, -- '11am-1pm', '1pm-3pm', etc.
  special_requests TEXT,

  -- Order totals
  total_amount DECIMAL(10,2) NOT NULL, -- Sum of all item subtotals

  -- Status tracking
  status TEXT DEFAULT 'pending', -- 'pending', 'confirmed', 'completed', 'cancelled'
  confirmed_at TIMESTAMP,
  confirmed_by TEXT DEFAULT 'owner', -- Who confirmed (from Google Sheets)
  completed_at TIMESTAMP,

  -- Google Sheets tracking
  sheets_row INTEGER, -- Row number in Google Sheets

  -- Payment (Phase 5 - future)
  payment_status TEXT, -- null until payments enabled
  payment_intent_id TEXT, -- Square/Stripe reference

  -- Metadata
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_pickup_day ON orders(pickup_day);
CREATE INDEX idx_orders_customer_email ON orders(customer_email);
CREATE INDEX idx_orders_created_at ON orders(created_at DESC);

-- Auto-generate order number
CREATE OR REPLACE FUNCTION generate_order_number()
RETURNS TRIGGER AS $$
BEGIN
  NEW.order_number := 'WK-' || LPAD(nextval('order_number_seq')::TEXT, 4, '0');
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE SEQUENCE order_number_seq START 1001;

CREATE TRIGGER set_order_number
BEFORE INSERT ON orders
FOR EACH ROW
EXECUTE FUNCTION generate_order_number();
```

---

### **3. catering_requests**
**Purpose:** Store catering inquiries (simple form)

```sql
CREATE TABLE catering_requests (
  -- Primary key
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  request_number TEXT UNIQUE NOT NULL, -- e.g., 'CAT-1001'

  -- Customer info
  customer_name TEXT NOT NULL,
  customer_email TEXT NOT NULL,
  customer_phone TEXT NOT NULL,

  -- Event details (from message field)
  message TEXT NOT NULL, -- Customer describes their event

  -- Status tracking
  status TEXT DEFAULT 'new', -- 'new', 'contacted', 'quoted', 'confirmed', 'declined'
  contacted_at TIMESTAMP,
  quote_sent_at TIMESTAMP,
  quote_amount DECIMAL(10,2),
  confirmed_at TIMESTAMP,

  -- Google Sheets tracking
  sheets_row INTEGER,

  -- Notes (owner's internal notes)
  internal_notes TEXT,

  -- Metadata
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_catering_status ON catering_requests(status);
CREATE INDEX idx_catering_created_at ON catering_requests(created_at DESC);

-- Auto-generate request number
CREATE OR REPLACE FUNCTION generate_catering_number()
RETURNS TRIGGER AS $$
BEGIN
  NEW.request_number := 'CAT-' || LPAD(nextval('catering_number_seq')::TEXT, 4, '0');
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE SEQUENCE catering_number_seq START 1001;

CREATE TRIGGER set_catering_number
BEFORE INSERT ON catering_requests
FOR EACH ROW
EXECUTE FUNCTION generate_catering_number();
```

---

### **4. vip_customers**
**Purpose:** Store VIP customer list for email/SMS blasts

```sql
CREATE TABLE vip_customers (
  -- Primary key
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

  -- Customer info
  name TEXT NOT NULL,
  email TEXT NOT NULL UNIQUE,
  phone TEXT, -- For future SMS (Phase 6)

  -- Opt-in tracking
  email_opted_in BOOLEAN DEFAULT true,
  sms_opted_in BOOLEAN DEFAULT false, -- For Phase 6

  -- Engagement tracking
  signed_up_at TIMESTAMP DEFAULT NOW(),
  last_blast_received TIMESTAMP,
  unsubscribed_at TIMESTAMP,

  -- Metadata
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_vip_email ON vip_customers(email);
CREATE INDEX idx_vip_opted_in ON vip_customers(email_opted_in);
```

---

### **5. vip_blasts**
**Purpose:** Track VIP email/SMS blast history

```sql
CREATE TABLE vip_blasts (
  -- Primary key
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

  -- Blast content
  subject TEXT NOT NULL,
  message TEXT NOT NULL,
  photo_url TEXT, -- URL to menu photo in Supabase Storage

  -- Blast details
  blast_type TEXT DEFAULT 'email', -- 'email', 'sms', 'both' (Phase 6)
  recipient_count INTEGER,
  sent_at TIMESTAMP DEFAULT NOW(),

  -- Engagement (if tracking)
  opens INTEGER DEFAULT 0,
  clicks INTEGER DEFAULT 0,

  -- Metadata
  created_at TIMESTAMP DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_vip_blasts_sent_at ON vip_blasts(sent_at DESC);
```

---

## 🔄 KEY WORKFLOWS

### **Workflow 1: Menu Update (Owner)**
```sql
-- Owner sets menu for the weekend
UPDATE menu_items
SET
  available = true,
  quantity_available = 10,
  price = 17.00, -- Can update or leave same
  updated_at = NOW()
WHERE name = 'Smokehouse Rib Dinner';
```

### **Workflow 2: Customer Places Order**
```sql
-- Insert order
INSERT INTO orders (
  items,
  customer_name,
  customer_email,
  customer_phone,
  pickup_day,
  pickup_time,
  total_amount
) VALUES (
  '[{"menu_item_id": "uuid", "name": "Rib Dinner", "quantity": 2, "price_at_time": 17.00, "subtotal": 34.00}]',
  'John Doe',
  'john@example.com',
  '555-1234',
  'Saturday',
  '11am-1pm',
  34.00
);

-- Note: Quantity NOT decremented yet (happens on confirmation)
```

### **Workflow 3: Owner Confirms Order (Google Sheets Checkbox)**
```sql
-- Webhook receives confirmation from Google Sheets
UPDATE orders
SET
  status = 'confirmed',
  confirmed_at = NOW()
WHERE order_number = 'WK-1001';

-- CRITICAL: Decrement menu item quantities
-- For each item in the order, decrement quantity_available

UPDATE menu_items
SET
  quantity_available = quantity_available - 2, -- Customer ordered 2
  available = CASE
    WHEN (quantity_available - 2) <= 0 THEN false -- Auto sold-out
    ELSE available
  END,
  updated_at = NOW()
WHERE id = 'uuid-of-rib-dinner';

-- Now frontend shows: "Rib Dinner (8 available)" instead of 10
```

### **Workflow 4: Item Sold Out**
```sql
-- Automatic sold out (quantity = 0)
UPDATE menu_items
SET available = false
WHERE quantity_available = 0;

-- Manual sold out (owner unchecks in admin)
UPDATE menu_items
SET available = false
WHERE id = 'uuid';
```

---

## 🔐 ROW-LEVEL SECURITY (RLS)

### **Public Access (No Auth Required)**
```sql
-- Anyone can read available menu items
CREATE POLICY "Public can read available menu"
ON menu_items FOR SELECT
USING (available = true AND quantity_available > 0);

-- Anyone can insert orders (customer submission)
CREATE POLICY "Public can create orders"
ON orders FOR INSERT
WITH CHECK (true);

-- Anyone can insert catering requests
CREATE POLICY "Public can create catering requests"
ON catering_requests FOR INSERT
WITH CHECK (true);

-- Anyone can insert VIP signups
CREATE POLICY "Public can signup for VIP"
ON vip_customers FOR INSERT
WITH CHECK (true);
```

### **Admin Access (Authenticated Only)**
```sql
-- Admin can read/write everything
CREATE POLICY "Admin full access menu"
ON menu_items FOR ALL
USING (auth.role() = 'authenticated');

CREATE POLICY "Admin full access orders"
ON orders FOR ALL
USING (auth.role() = 'authenticated');

CREATE POLICY "Admin full access catering"
ON catering_requests FOR ALL
USING (auth.role() = 'authenticated');

CREATE POLICY "Admin full access VIP"
ON vip_customers FOR ALL
USING (auth.role() = 'authenticated');

CREATE POLICY "Admin full access blasts"
ON vip_blasts FOR ALL
USING (auth.role() = 'authenticated');
```

---

## 📊 USEFUL QUERIES

### **Get Current Menu (Customer View)**
```sql
SELECT
  id,
  name,
  description,
  price,
  unit,
  quantity_available,
  image_url,
  category
FROM menu_items
WHERE available = true
  AND quantity_available > 0
ORDER BY sort_order;
```

### **Get Pending Orders (Owner View)**
```sql
SELECT
  order_number,
  customer_name,
  customer_phone,
  items,
  pickup_day,
  pickup_time,
  total_amount,
  created_at
FROM orders
WHERE status = 'pending'
ORDER BY created_at ASC;
```

### **Decrement Quantity on Confirmation**
```sql
-- When order WK-1001 confirmed, decrement each item
WITH order_items AS (
  SELECT
    jsonb_array_elements(items) AS item
  FROM orders
  WHERE order_number = 'WK-1001'
)
UPDATE menu_items
SET
  quantity_available = quantity_available - (
    SELECT (item->>'quantity')::INTEGER
    FROM order_items
    WHERE (item->>'menu_item_id')::UUID = menu_items.id
  ),
  available = CASE
    WHEN quantity_available - (SELECT (item->>'quantity')::INTEGER FROM order_items WHERE (item->>'menu_item_id')::UUID = menu_items.id) <= 0
    THEN false
    ELSE available
  END
WHERE id IN (
  SELECT (item->>'menu_item_id')::UUID
  FROM order_items
);
```

---

## 🔄 CHANGE LOG

### 2026-04-08
- Created database schema
- Added quantity_available to menu_items
- Added price to menu_items (persists week-to-week)
- Auto sold-out logic when quantity = 0
- Simplified catering_requests (just name, email, phone, message)
- Added order_number auto-generation
- Added RLS policies

---

*Next Update: When schema changes*
*Related: PHASE-1-frontend.md, PHASE-3-integration.md*