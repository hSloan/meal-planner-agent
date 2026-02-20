# Instacart Browser Automation Reference

## Navigation

- Base URL: `https://www.instacart.com`
- Search: Use the search bar at top of page
- Cart: Accessible via cart icon (top right)

## Common Selectors & Flow

Instacart's UI changes frequently. Always use `browser snapshot` to inspect current state
rather than relying on hardcoded selectors. Use aria refs for stability.

### Search Flow

1. Navigate to instacart.com
2. Snapshot to find the search input
3. Type the item name and press Enter
4. Snapshot results — look for product cards with name, price, unit
5. Click "Add to cart" on the best match
6. If quantity > 1, use the quantity adjuster (usually +/- buttons)

### Cart Review Flow

1. Click the cart icon
2. Snapshot the cart page
3. Report items, quantities, and prices to the human
4. Total should be visible at bottom

### Checkout Flow

1. From cart, click "Go to checkout" or equivalent
2. Snapshot checkout page
3. Report delivery window options and fees
4. **STOP** — let the human confirm before proceeding
5. If confirmed, select delivery window
6. **STOP at payment** — never enter or submit payment details

## Tips

- If not logged in, ask the human to log in first (snapshot the page to show them)
- Prefer "Best match" or store-brand items for budget-conscious plans
- Some items have quantity limits — check and report if hit
- If an item shows "Out of stock," search for alternatives and ask the human
