# برومبت Claude Design — سيستم إدارة مطعم

> انسخ كل اللي تحت الخط ده والصقه في Claude Design.

---

Design a complete, modern and elegant **restaurant management system (POS)** web app. The whole UI is in **Arabic, fully RTL**. It will be built later with React + Firebase (Firestore realtime), so design a clean, component-based system (design tokens, reusable components) that is easy to implement.

## 1. Brand & visual style
- Mood: modern, premium, calm, very clear — **not cluttered**. A cashier must find anything in 1–2 taps during rush hour.
- Primary color: **modern purple / violet**. Suggested palette:
  - Primary `#6D28D9`, Primary hover `#5B21B6`, Deep `#2E1065` (sidebar / headers)
  - Soft lavender surfaces `#F5F3FF` / `#EDE9FE`
  - Accent (sparingly, for totals/highlights): warm gold `#F5B841`
  - Status: success `#16A34A`, warning `#F59E0B`, danger `#DC2626`, info `#0EA5E9`
- Typography: **IBM Plex Sans Arabic** (or Cairo / Tajawal). Big, bold numbers for prices, totals and order numbers. Use Western digits (0-9) for numbers.
- Rounded corners (12–16px), soft shadows, generous spacing, large touch targets (min 48px) — it will be used on touch screens and tablets.
- Provide **light mode** (default) and a **dark mode** (deep purple-black) variant, especially for the kitchen screen.
- Every order type has its own color + icon used consistently everywhere (cards, badges, receipts, filters):
  - صالة (Dine-in) — purple, table icon
  - تيك أواي (Takeaway) — orange, bag icon
  - دليفري (Delivery) — blue, scooter icon
  - استلام من الفرع (Phone order / Branch pickup) — teal, phone + store icon
  - طلبات (Talabat partner) — Talabat-style orange-red label "طلبات" (generic, no logo)

## 2. Order numbering (show it clearly in the designs)
- Each order type has its own prefix and its own daily sequence that resets with the daily closing:
  `S-001` صالة · `T-001` تيك أواي · `D-001` دليفري · `P-001` استلام فرع · `TL-001` طلبات
- The order number is always the **biggest element** on order cards, KDS tickets and receipts.

## 3. Roles & app shell
Roles: **مدير (Admin)**, **كاشير**, **ويتر**, **مطبخ**, **مندوب دليفري**. Each role sees only its screens.
- Right-side collapsible sidebar (RTL) with icons + labels, deep purple.
- Top bar: branch name, current shift badge (**صباحي** ☀ / **مسائي** 🌙), logged-in employee name, realtime sync indicator (green dot "متصل" — two cashiers can work at the same time), notifications bell (new online / QR orders).

## 4. Screens to design

### 4.1 Login & shift
- Employee login with PIN pad (big numeric keypad) + employee avatar list.
- "فتح وردية" modal: choose shift (صباحي / مسائي), enter opening cash balance (رصيد بداية الشيفت).

### 4.2 Cashier POS (the most important screen)
Layout (RTL): 
- **Right**: menu categories as a vertical list or large tabs (مشويات، ساندوتشات، وجبات، سلطات، مشروبات، حلويات...). Categories keep the menu organized and never crowded.
- **Center**: item grid of the selected category — card with name, price, optional image; items that are out of stock show greyed out with "خلص" badge. A search box on top.
- **Left**: the open order panel:
  - Order type segmented control at the top (صالة / تيك أواي / دليفري / استلام فرع / طلبات) with their colors.
  - For **دليفري / استلام فرع**: phone number field first → if the customer exists, auto-fill name + saved addresses (choose one or add new). If new → quick inline form (name, phone, area, address, landmark). Show "عميل مسجّل · 7 طلبات سابقة" chip.
  - Delivery fee row with a toggle "إضافة رسوم توصيل" + amount (default per area, editable).
  - Assign driver (المندوب) dropdown for delivery.
  - For **صالة**: choose table.
  - Order lines: item, quantity stepper (− / +), modifiers/add-ons (e.g. «جبنة زيادة +10»), line note.
  - **Quick note chips** when tapping an item: «بدون بصل»، «بدون طماطم»، «سبايسي»، «صوص زيادة»، plus free-text note.
  - Summary: subtotal, add-ons, delivery fee, discount (needs manager approval — show a lock icon), **total in big bold**.
  - Bottom actions: **"إرسال للمطبخ"**, **"دفع وطباعة"** (one tap — prints instantly, no print dialog), "تعليق الطلب".
- Payment modal: كاش / كارت / آجل, cash received → change due (الباقي) big. Payment status is independent from preparation status.

### 4.3 Orders board (لوحة الأوردرات)
- Kanban or filterable list: جديد → في التحضير → جاهز → خرج مع المندوب → تم التسليم.
- Filters by order type (colored chips), payment status (مدفوع / غير مدفوع / آجل), shift.
- Each card: big order number, type badge, customer name/table, time since order, total, payment badge.

### 4.4 Branch pickup / phone orders (استلام من الفرع)
- List of held orders waiting for the customer: order number, customer name, phone, pickup time, total, status "في الانتظار".
- When the customer arrives: one tap **"تحصيل وطباعة"** → payment → instant receipt print.
- Support **scheduled pre-orders** (طلب مسبق بميعاد) for large orders: date/time picker + countdown badge.

### 4.5 Delivery tracking & collection (الدليفري)
- Per-driver view: each driver card shows the orders currently with him and the **total amount he owes** (آجل على المندوب).
- Each delivery order shows a very clear state: **"لم يتم التحصيل"** (red/amber badge) until the driver returns with the money, then one big button **"تم التحصيل ✓"** → turns green. This must be extremely easy and obvious so no order is counted as collected by mistake.
- Timeline per order: خرج → وصل → تم التحصيل.

### 4.6 Talabat section (قسم طلبات)
- Orders received through Talabat, marked as **deferred payment (آجل)**.
- Running balance owed by Talabat, list of orders per period, and a "تسوية" (settlement) action to mark a batch as paid, with settlement history.

### 4.7 Deferred accounts (الحسابات الآجلة)
- Customers / companies that pay later: balance, order history, record a payment, print statement.

### 4.8 Tables map (الصالة)
- Visual floor map with tables (round / square), colors: فاضية (green outline), مشغولة (purple filled with running total + elapsed time), محجوزة (amber), في انتظار الحساب (red).
- Tap a table → open its tab, add items multiple times until payment. Move / merge tables.
- Every table has a QR code (show "طباعة QR" action).

### 4.9 Kitchen Display (KDS) — dark mode
- Columns per station (الجريل، المقلي، المشروبات، الحلويات) or a station selector — each station sees only its items.
- Ticket card: **huge order number**, type badge, table/customer, items with quantities, notes highlighted in amber (e.g. «بدون بصل»).
- Items added later to an already-sent order appear as a **new ticket labeled "إضافة"** with only the new items.
- Timer on every ticket vs expected prep time: normal → amber when near → **red pulsing "متأخر"** when late.
- Tap item to mark done, tap ticket to mark ready.

### 4.10 Reservations & waitlist (الحجوزات وقائمة الانتظار)
- Reservation form: name, phone, date/time, number of guests, table.
- Waitlist queue with party size and wait time, "استدعاء" and "إجلاس" actions.

### 4.11 Shift close & daily closing (تقفيل الوردية / التقفيل اليومي)
- Shift close: enter actual cash counted → system shows **expected cash vs actual and the difference** (green if 0, red if short, blue if over).
- Breakdown: sales by payment method (كاش، كارت، آجل، طلبات)، by order type، discounts، returns، cancellations، delivery fees، uncollected delivery orders (warning if any still not collected).
- Daily closing (Z report) combining صباحي + مسائي, with "طباعة التقرير" and "تقفيل اليوم" (resets order numbers).

### 4.12 Admin dashboard (لوحة المدير)
- KPI cards: مبيعات اليوم، عدد الأوردرات، متوسط الفاتورة، آجل مستحق (طلبات + مندوبين + عملاء).
- Charts: sales by hour (peak time), sales by order type, top items, average kitchen prep time at peak.
- Approvals inbox: discount / return / cancellation requests — each logged with **employee name + reason**, approve / reject.
- Activity log (سجل العمليات).

### 4.13 Menu management (إدارة المنيو) — admin
- Categories list (drag to reorder, icon, color), items inside each category.
- Item editor: name, price, image, kitchen station, expected prep time (minutes), add-ons groups (e.g. «إضافات»: جبنة زيادة +10، صوص +5), sizes, availability toggle **"متاح / خلص"** (one tap to stop an item).

### 4.14 Staff & roles (الموظفين والصلاحيات)
- Employees table: name, role, PIN, shift, active toggle.
- Permissions matrix (role × action checkboxes).

### 4.15 Customers (العملاء)
- Searchable by phone. Customer profile: name, phones, multiple saved addresses, order history, total spent, notes.

### 4.16 Customer-facing (mobile) screens
- **QR table ordering**: mobile menu opened from the table QR — restaurant header, category tabs, items, cart, "إرسال الطلب" → order arrives linked to the table.
- **Online ordering link** (restaurant-name link) for takeaway & delivery: menu, cart, choose delivery/pickup, address, optional scheduled time, order confirmation + status page.

## 5. Receipts (very important — make them very elegant)
Design **80mm thermal receipts** in Arabic. Thermal printers print in black only, so design a monochrome-safe version, and also a **purple preview version** for on-screen display.
- Header: restaurant logo/name, branch, phone.
- **Order type in a big, bold, inverted block** (white text on black) — «دليفري» / «تيك أواي» / «صالة» / «استلام من الفرع» / «طلبات» — readable from a distance.
- **Order number very large** (e.g. `D-014`), date, time, cashier, shift.
- For **دليفري**: customer name, phone number, full address + landmark in a clear boxed section, driver name.
- For **صالة**: table number & waiter. For **استلام فرع**: customer name & phone, pickup time.
- Items table: الصنف / الكمية / السعر, add-ons indented under the item, notes in italic (« بدون بصل »).
- Totals: subtotal, delivery fee, discount, **الإجمالي** large, payment method, paid / change, or a clear **"آجل — لم يتم التحصيل"** stamp.
- Footer: thank-you message, social handles, optional QR to the online ordering link.
- Also design the **kitchen ticket** (no prices, huge order number, big item names, notes highlighted, "إضافة" label for added items).
- Show receipt variants: دليفري، تيك أواي، صالة، استلام فرع، طلبات.

## 6. Components & states to include
Buttons (primary/secondary/danger/ghost, large POS size), order-type badges, payment badges, status chips, quantity stepper, note chips, PIN pad, modals, toasts ("تم الطباعة ✓"), empty states, loading skeletons, offline / reconnecting banner, confirmation dialogs for cancel/return (reason field required).

## 7. Deliverables
Desktop POS (1440×900 and 1280×800), tablet (1024×768) for waiter & KDS, mobile (390×844) for QR / online ordering and the driver view, receipts at 80mm width, plus a small design-system page (colors, typography, spacing, components).
