# Meal Planner Agent

An OpenClaw skill that creates personalized weekly meal plans, generates grocery lists, and orders groceries via Instacart browser automation.

## What It Does

1. **Onboarding** — Learns your household size, dietary restrictions, allergies, budget, cuisine preferences, and cooking skill level
2. **Meal Planning** — Generates a 7-day plan (2-3 meals/day) with varied cuisines, smart leftover reuse, and cost estimates
3. **Grocery List** — Consolidates ingredients, removes pantry staples you already have, and organizes by store section
4. **Instacart Ordering** — Adds items to your Instacart cart via browser automation (always asks permission, never completes payment)
5. **Feedback & Learning** — Tracks meal ratings and preferences to improve future plans

## Requirements

- **OpenClaw** with browser, web search, and exec tools enabled
- **Browser access** for Instacart ordering (optional — the skill works without it for planning only)
- An **Instacart account** if you want automated grocery ordering

## Installation

Install as an OpenClaw skill:

```bash
openclaw skill install meal-planner-agent.skill
```

Or clone directly:

```bash
git clone https://github.com/hSloan/meal-planner-agent.git ~/.openclaw/workspace/skills/meal-planner
```

## Usage

Ask your agent naturally:

> "Plan my meals for the week"

> "I need a grocery list for a family of 4, $120 budget, no dairy"

> "Order this week's groceries on Instacart"

On first use the agent walks you through a quick onboarding to learn your preferences. After that, it remembers everything in `meal-planner-data/`.

## Running as a Dedicated Agent

The skill includes a reference config for running meal planner as a standalone OpenClaw agent with its own model and system prompt. See `references/agent-config.md`.

## Data Files

Stored in `meal-planner-data/` within the workspace:

| File | Purpose |
|------|---------|
| `profile.json` | Dietary restrictions, allergies, household size, budget, preferences |
| `preferences.json` | Meal ratings, favorites, dislikes — used to improve future plans |
| `current-plan.json` | Active week's meal plan with recipes |
| `grocery-list.json` | Current week's consolidated grocery list |

## Safety Rules

- **Never places an order without explicit human confirmation**
- **Never enters or submits payment information**
- **Always presents the cart for review before checkout**
- Respects dietary restrictions absolutely — never suggests restricted foods
- Flags budget overages proactively and suggests swaps

## License

MIT
