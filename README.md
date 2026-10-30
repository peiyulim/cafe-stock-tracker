# Café Stock Tracker

A single-page stock tracker for three café outlets (Jurong, Orchard, Tampines), backed by Supabase.

## Features

- A tab per outlet, with items grouped by category (Coffee, Dairy, Powders & syrups, Bakery, Packaging)
- −/+ controls for quantity on hand and reorder level, with a Save button per item
- Items at or below their reorder level are marked **Low**
- An "Order from suppliers" tab that totals the shortfall across all outlets, with a copy button

## Running it

Open `stock-tracker.html` in any browser. No build step or sign-in is needed.

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

The page uses the project's publishable key, and the table lets anyone with that key change
`qty_on_hand` and `reorder_level`. No other columns can be changed, and rows can't be added or deleted.
Because this repo is public, anyone can find the key here.
