# design-director

An agent skill that combines three popular frontend design skills into one entry point:

- [**frontend-design**](https://github.com/anthropics/skills) (Anthropic): picks a deliberate
  aesthetic direction.
- [**design-taste-frontend**](https://github.com/Leonxlnx/taste-skill) (tasteskill): rules and a
  browser finish gate for company and marketing sites.
- [**ui-ux-pro-max**](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill): searchable data on
  styles, palettes, font pairings, charts, UX rules and stack guidelines.

Installed side by side, the three overlap and contradict each other. One suggests glassmorphism,
another bans that look, and a third tells the agent to change fonts on every build. Design
Director decides which skill handles which task and which one wins a conflict. It routes
marketing sites through tasteskill, app UI and dashboards through ui-ux-pro-max, and puts your
project's own `DESIGN.md` above all three.

It doesn't copy their content. It loads only the parts of each skill that the task needs, so you
still get updates from the original authors.

## Install

Design Director needs the three source skills. Install all four:

```bash
npx skills add https://github.com/neelsatyavolu/design-director --skill design-director
npx skills add https://github.com/anthropics/skills --skill frontend-design
npx skills add https://github.com/Leonxlnx/taste-skill --skill design-taste-frontend
npx skills add https://github.com/nextlevelbuilder/ui-ux-pro-max-skill --skill ui-ux-pro-max
```

Or copy `skills/design-director/` into `~/.claude/skills/` (Claude Code) or your agent's skills
folder.

Requirements come from the source skills: Python 3 for ui-ux-pro-max, and Node 22+ plus Chrome,
Edge or Chromium for the tasteskill gate.

## Use

In Claude Code, type `/design-director` followed by your request:

```
/design-director build a landing page for a small coffee roaster
/design-director redesign this settings screen
/design-director review the dashboard in src/app/dashboard
```

The source skills stay installed and can still trigger on their own. Calling `/design-director`
makes sure the combined routing and conflict rules apply.

## How it decides

1. Your request in the conversation.
2. The project's `DESIGN.md`. On a new project without one, it asks whether to use your personal
   `~/DESIGN.md` (if you have one) or to create a system for this product.
3. tasteskill's house rules, on marketing sites only.
4. frontend-design's direction step.
5. ui-ux-pro-max's data, treated as options rather than rules.

See [`skills/design-director/SKILL.md`](skills/design-director/SKILL.md) for the full routing.

## License

MIT for this skill. Each source skill keeps its own license.
