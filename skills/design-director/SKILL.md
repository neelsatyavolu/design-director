---
name: design-director
description: >-
  One entry point for all frontend design work. Combines frontend-design (aesthetic direction),
  design-taste-frontend / tasteskill (company and marketing sites, with a browser finish gate) and
  ui-ux-pro-max (searchable styles, palettes, font pairings, charts, UX and stack rules), with a
  fixed order for which one wins when they disagree. Use when building, designing, redesigning,
  restyling, polishing or reviewing any web UI: landing pages, company or product sites,
  dashboards, admin panels, app screens, components, forms, charts, artifacts or mobile UI.
---

# Design Director

This skill picks which of three design skills to use for a task and settles conflicts between
them. It does not repeat their content. Read only the source files the task needs.

| Source | Install name | Use it for |
|---|---|---|
| Direction | `frontend-design` | Choosing an aesthetic direction (short; read first on new work) |
| Taste | `design-taste-frontend` | Company, marketing, product, shop, hospitality, SaaS and game **sites** |
| Lookup | `ui-ux-pro-max` | App UI, dashboards, admin, charts, forms, mobile; lookup data for any task |

## 0. Find the sources

Find each source skill's folder once per session:

```bash
for n in frontend-design design-taste-frontend ui-ux-pro-max taste-skill; do
  find ~/.claude/skills ~/.agents/skills ~/.claude/plugins .claude/skills .agents/skills \
    -maxdepth 7 -type d -name "$n" 2>/dev/null | head -1
done
```

Below, `<direction>`, `<taste>` and `<lookup>` mean those folders. The Taste plugin install names
its folder `taste-skill`. If a source is missing, say which one and give its install command from
this skill's README. Then carry on with the sources you have.

## 1. Precedence (highest wins)

1. The user's explicit request in this conversation.
2. The design system. Use the project's own `DESIGN.md` if it has one. On a new product with no
   `DESIGN.md`: if the user keeps a personal one (for example `~/DESIGN.md`), ask whether to use
   it or create a system that fits this product, and write a new one to the project's
   `DESIGN.md`. For one-off output that isn't a new product, use the personal one if it exists.
   Read the chosen file before designing. Its colours, fonts, radii and rules override every
   source skill below.
3. Taste house rules and "Never" list, **on sites only**.
4. Direction thinking.
5. Lookup data. Treat it as a menu of options, never as a mandate.

Known conflicts, already settled:
- Lookup suggests glassmorphism, claymorphism, neumorphism, purple/violet gradients, and so on.
  Reject any suggestion that breaks rule 2 or 3. On sites, violet and lavender are banned.
- Direction says to vary themes and fonts on every build. That applies only when neither rule 1
  nor rule 2 sets them.
- Direction pushes maximal effects. On sites, Taste rule 8 limits shaders and scenes to technical
  trades. On app UI, restraint and legibility come first.
- Taste's one-section-per-screen and dealt-moment rules apply to marketing pages, never to app
  screens or dashboards.

## 2. Route by task

**A. Marketing or company site** (landing page, company, product, shop, cafe, hotel, studio,
SaaS, AI, developer tool, game):
1. Settle the design system (precedence rule 2).
2. Read `<direction>/SKILL.md` and fix the direction in two or three sentences.
3. Read `<taste>/SKILL.md` and follow its run, checklist and finish gate. Its gate runs as a Stop
   hook only when Taste itself is loaded, so run `node <taste>/scripts/gate.mjs` yourself before
   calling the work done.
4. Use Lookup only for a specific gap, such as a font pairing or a chart type.

**B. App UI, dashboard, admin panel, form, component or mobile screen:**
1. Settle the design system.
2. Read Direction for the overall look (skip it when the design system already decides).
3. Read the "Quick Reference" in `<lookup>/SKILL.md` (accessibility, touch, performance, layout)
   and treat its CRITICAL and HIGH rules as required.
4. Query its data:
   ```bash
   S=<lookup>/scripts/search.py
   python3 $S "<product> <industry> <keywords>" --design-system -p "<Name>"
   python3 $S "<keyword>" --domain ux|style|typography|chart|landing [-n 5]
   python3 $S "<keyword>" --stack html-tailwind|react|nextjs|shadcn|...
   ```
   Lookup's own SKILL.md uses a relative `skills/ui-ux-pro-max/...` path; use the folder found in
   step 0 instead. Filter the results through the precedence rules.
5. For charts, also load a data-visualisation skill if one is installed.

**C. Review, critique or fix of an existing UI:** check it against the design system, then
against Taste's checklist (sites) or Lookup's Quick Reference (app UI). Report findings in
priority order before changing anything, unless the user asked for fixes.

**D. Claude Artifact page:** if an `artifact-design` skill is loaded, it sets the page contract.
Use this skill for the visual direction inside that contract.

## 3. Before calling it done

- Every colour, font and radius traces back to the design system or to a stated direction.
- Check the layout at 390, 768 and 1440 px wide, and check both themes if there are two.
- Contrast is at least 4.5:1 for body text, focus states are visible, and touch targets are at
  least 44px.
- Sites: the Taste gate passes, or each failure is reported with the reason.
