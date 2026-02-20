---
name: meal-planner
description: >
  Weekly meal planning agent that creates personalized 7-day meal plans (2-3 meals/day),
  generates grocery lists, and orders groceries via Instacart browser automation.
  Use when a human asks to plan meals, create a meal plan, build a grocery list,
  order groceries, get recipe suggestions, or track meal preferences.
  Handles dietary restrictions, budget constraints, household size, and learns
  preferences over time from feedback.
---

# Meal Planner

## Overview

Guide a human through weekly meal planning: gather preferences, build a 7-day plan,
generate a consolidated grocery list, and optionally order via Instacart.

## Data Files

Store all user data in the workspace under `meal-planner-data/`:

- `profile.json` — dietary restrictions, allergies, household size, budget, cuisine preferences
- `preferences.json` — meal ratings, favorites, dislikes, history
- `current-plan.json` — active week's meal plan with recipes
- `grocery-list.json` — current week's consolidated grocery list

Create `meal-planner-data/` on first use if it doesn't exist.

## Workflow

### 1. Onboarding (first use or missing profile)

If `meal-planner-data/profile.json` doesn't exist, ask:

1. How many people are you cooking for?
2. Any dietary restrictions? (vegetarian, vegan, kosher, halal, gluten-free, etc.)
3. Any food allergies?
4. Weekly grocery budget?
5. Cuisine preferences? (e.g., Mediterranean, Asian, American, Mexican, mixed)
6. Cooking skill level? (beginner, intermediate, advanced)
7. How much time per meal? (quick <30min, moderate 30-60min, involved 60min+)
8. How many meals per day? (2 or 3)

Save answers to `profile.json`. Don't ask all at once — group into 2-3 natural messages.

### 2. Generate Meal Plan

Using the profile, generate a 7-day plan. For each meal include:

- Meal name
- Brief description
- Estimated prep/cook time
- Estimated cost per serving
- Key ingredients

**Guidelines:**
- Vary cuisines and proteins across the week
- Reuse overlapping ingredients to reduce waste and cost
- Stay within the stated budget
- Scale portions to household size
- Factor in preferences/ratings from `preferences.json` if it exists
- Suggest leftovers strategically (cook once, eat twice)

Present the plan day-by-day. Ask the human to review and swap out any meals they don't want.

### 3. Generate Grocery List

Once the plan is confirmed:

1. Parse all recipes and extract ingredients
2. Aggregate quantities (combine duplicates)
3. Subtract pantry staples if the human has indicated what they already have (ask once)
4. Organize by store section: Produce, Meat/Seafood, Dairy, Pantry, Frozen, Bakery, Other
5. Include estimated cost per item and total
6. Save to `grocery-list.json`

Present the list and ask if anything should be added or removed.

### 4. Instacart Ordering (Browser)

**Always ask permission before proceeding with Instacart.**

Steps:

1. Ask: "Would you like me to add these items to your Instacart cart?"
2. If yes, use the browser tool to:
   - Navigate to instacart.com
   - Search for each item on the grocery list
   - Select the best match (prefer store-brand for budget, match organic/specific preferences)
   - Add to cart with correct quantity
3. After adding all items, ask: "I've added everything to your cart. Would you like to review the cart before I proceed?"
4. Take a snapshot of the cart for the human to review
5. **Never place the order without explicit confirmation.** Ask: "Everything looks good. Shall I proceed to checkout?"
6. If confirmed, proceed to checkout but **stop before final payment confirmation** and let the human complete payment.

**Error handling:**
- If an item isn't found, suggest alternatives and ask the human
- If an item is significantly more expensive than estimated, flag it
- If the cart total exceeds budget, flag it and suggest substitutions

### 5. Feedback & Learning

After the week (or anytime), ask for feedback:

- Which meals did you enjoy? (rate 1-5)
- Which meals would you skip next time?
- Any new preferences or restrictions?

Update `preferences.json` with:
- Meal ratings (name, rating, date)
- Favorites list (meals rated 4-5)
- Avoid list (meals rated 1-2)
- Ingredient preferences (liked/disliked ingredients)

Use this data to improve future recommendations.

## Recipe Format

When the human asks for a recipe detail, provide:

```
## [Meal Name]
Serves: [N] | Prep: [X]min | Cook: [Y]min | Est. cost: $[Z]

### Ingredients
- [quantity] [ingredient]
...

### Instructions
1. [step]
...

### Notes
- [storage tips, variations, etc.]
```

## Budget Tracking

- Track estimated vs actual cost (from Instacart cart)
- If actual exceeds budget by >10%, proactively suggest swaps
- Show running total as items are added to cart

## Important Rules

- **Never place an order without explicit human confirmation**
- **Never share or expose payment information**
- **Always present the cart for review before checkout**
- Respect dietary restrictions absolutely — never suggest restricted foods
- When uncertain about an allergy or restriction, err on the side of caution and ask
