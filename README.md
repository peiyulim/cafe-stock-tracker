# Café Stock Tracker

A single-page stock tracker for three café outlets (Jurong, Orchard, Tampines), backed by Supabase.

## Features

- A tab per outlet, with items grouped by category (Coffee, Dairy, Powders & syrups, Bakery, Packaging)
- −/+ controls for quantity on hand and reorder level, with a Save button per item
- Items at or below their reorder level are marked **Low**
- An "Order from suppliers" tab that totals the shortfall across all outlets, with a copy button

## Running it

Open `stock-tracker.html` in any browser (or the deployed site) and sign in with your username and password.

## Users and sign-in

- Accounts live in Supabase Auth (`auth.users`). Each user signs in as `<username>`, which the app maps to `<username>@example.com`.
- `public.profiles` holds each user's username, display name and role (`admin` or `member`). A trigger fills it in when a user is created.
- **Members** can view stock and change quantities and reorder levels.
- **Admins** also get a **Users** tab to add users (with an initial password and role) and reset passwords, via the `manage-users` Edge Function.
- Anyone signed in can change their own password from the top bar.

## Data

The page reads and writes the `public.stock_counts` table in Supabase.

| Column | Meaning |
|---|---|
| `outlet`, `sku`, `item_name`, `unit`, `category` | Item details |
| `qty_on_hand` | Current stock |
| `reorder_level` | Item is Low at or below this number |
| `counted_at` | Date of the stock count (one row per outlet + SKU + date) |

**Shortfall** is `reorder_level − qty_on_hand` (never below 0).

## Security note

The publishable key in the page is safe to share: without signing in it can't read or change stock.
Signed-in users can only change `qty_on_hand` and `reorder_level`; rows can't be added, deleted or renamed from the app.
