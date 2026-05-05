# What is DESIGN.md?

---

The DESIGN.md concept was introduced by Google Stitch. Stitch uses a plain markdown file to describe design patterns, colors, typography, spacing, components, and hands it to an AI agent so it can generate consistent UI. No Figma plugin, no JSON schema, just a markdown file the agent reads before it writes code.

We built the [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) repository on top of that idea. getdesign.md is the web app version of that repo. You can browse, preview, and copy any DESIGN.md file without cloning anything.

---

## The problem: AI builds "nice" but not "yours"

Tell any AI agent to "build me a landing page" and you already know what you'll get. Rounded cards. A purple-blue gradient. A centered hero. A "Get Started" button. It works. It also looks like everything else.

The reason is simple. The agent's idea of "good design" is an average of averages. It has no clue why Vercel uses border instead of shadow, why Linear keeps its letter-spacing so tight, or why Stripe goes easy on gradients. Even if it did know, cramming all of that into a prompt is borderline impossible.

So you end up with two bad options:

1. Write 40 lines of prompt every time ("use #0070f3 for links, -0.02em letter spacing on headings, 8px border radius, no shadows just 1px borders...") and still get half of it wrong.
2. Screenshot a site, paste it, say "make it look like this." The agent copies pixels but misses the system behind them.

Neither scales.

---

## The fix: DESIGN.md

A DESIGN.md file describes a design system semantically. It is not a token list. Not a Figma export. Not a component library. Picture a document where an experienced designer explains a brand's visual language to a developer who's seeing it for the first time. That's what it reads like.

Here's what goes inside:

- **Visual theme and atmosphere** tells the agent what the brand looks like and, more importantly, *why*. The philosophy behind the aesthetic. Sentences like "Minimalism as engineering principle." The agent gets intent, not just instructions.

- **Color palette and roles** gives every color a hex value *and* a job.
  "`#ff5b4f`, ship red, used for the production deploy flow because shipping should feel urgent."
  The name tells you what the color does.

- **Typography rules** cover font, size, weight, line-height, letter-spacing. But the real value is context: which style goes where, and why.
  "Display sizes get -2.4px tracking because headlines should feel like minified code."

- **Spacing, shadows, motion, components** fill in the rest of the system. Every rule, wherever possible, with a reason attached.

DESIGN.md keeps token, rule, and rationale in the same file. A token tells you what to use but not where. A rule tells you where but not when to bend it. The rationale is what lets an agent make the right call when it hits a situation the file never covers.

---

## Why markdown?

Because it is the language AI agents speak best. They can read JSON tokens but can't interpret them. They can't see Figma files. They'll imitate a screenshot but won't systematize it. Markdown sits in the middle: readable by humans, parseable by machines, easy to version and diff, and you can drop it in a repo root.

Drop a DESIGN.md file in your project root and tell your agent "use DESIGN.md as reference before you write any UI." From that point on, whether you're working with Claude Code, Cursor, or Windsurf, the agent knows which font, which color, which spacing to reach for. You don't have to repeat yourself in every prompt.

---

## Why a collection?

Most teams don't write their own DESIGN.md from scratch. Most teams say "make it look like Linear," "give it that Stripe feel," or "keep it close to Apple." These references are real. They come up constantly.

getdesign.md collects those starting points. Inspiration files based on Vercel, Stripe, Linear, Apple, Tesla, Notion, Figma, Supabase, and dozens more, all in the same format, all comparable. Pick one, drop it into your project, tell your agent "use this file as reference." Building on top of that language with your own content and components is up to you.

The goal is not "copy Vercel." It is to give the agent a starting language. Enough context to escape the generic average and land on a specific aesthetic. From there you drift, you make it yours, you evolve it.

---

## What DESIGN.md is not

The name can be misleading, so this matters:

You can't drop it in and call the theme done. It is a dictionary. The implementation still needs writing. There is no code inside, just rules. It describes what a button looks like; you or your agent still build the button.

It is not a brand guideline PDF either. Brand guidelines are written for humans and speak too loosely for agents to act on ("our brand feels approachable yet premium"). A DESIGN.md has to be specific enough for the agent to make its next decision.

It is not a Figma export. Figma token exports tell you "what" but skip "why." A DESIGN.md carries the rationale.

And it is not static. When the brand evolves, the file evolves. It gets versioned, PR'd, discussed. It behaves like code.

---

## Mental model

You used to hire a designer and say "you know Linear, right? Give me that feel." It worked because the designer already carried the reference in their head. That shared context sat underneath every conversation you had with them.

An AI agent doesn't have that shared context. DESIGN.md writes it into a file and puts it in the agent's "head." The reference pool you built with a designer over years, you set up with an agent in two minutes.

getdesign.md is not an "asset site." It is closer to an experiment in how design languages get shared when the designer is a machine. The DESIGN.md files are the format. The site is a collection that makes that format concrete.

---

# The structure of a DESIGN.md file

A DESIGN.md file in the Google `alpha` spec has two halves: a **YAML front matter block** that holds the structured tokens, and a **prose body** of canonical sections that turns those tokens into language an agent can act on.

There are 9 standard pieces, walked through below in the order they appear in the file.

---

## 1. YAML front matter

The very top of the file is a YAML block fenced by `---`.

```yaml
---
version: alpha
name: Lumenpath
description: A bright, citrus-warmed product canvas where every primary
  moment runs in a single Tangerine accent. Display type stays large and
  confident at modest weights; cards float on a pale-cream surface
  separated by hairline borders rather than shadows.
---
```

The `version: alpha` line tells a linter which spec rules to apply. The `description` anchors the brand's atmosphere.

---

## 2. Color tokens

```yaml
colors:
  primary: "#F76B1C"
  ink: "#1B1A17"
  body: "#3D3A33"
  muted: "#7A7568"
  canvas: "#FFFAF1"
  surface-card: "#FFFFFF"
  hairline: "#E8E1D2"
  on-primary: "#FFFFFF"
```

Referenced later as `{colors.primary}`, `{colors.canvas}`, etc.

---

## 3. Typography tokens

```yaml
typography:
  display-lg:
    fontFamily: "'Söhne', Inter, sans-serif"
    fontSize: 56px
    fontWeight: 500
    lineHeight: 1.05
    letterSpacing: -1.8px
  body-md:
    fontFamily: "Inter, sans-serif"
    fontSize: 16px
    fontWeight: 400
    lineHeight: 1.55
    letterSpacing: 0
  button:
    fontFamily: "Inter, sans-serif"
    fontSize: 14px
    fontWeight: 500
    lineHeight: 1
    letterSpacing: 0
```

---

## 4. Shape and spacing tokens

```yaml
rounded:
  none: 0px
  sm: 6px
  md: 10px
  lg: 16px
  full: 9999px

spacing:
  xs: 4px
  sm: 8px
  md: 12px
  base: 16px
  lg: 24px
  xl: 32px
  section: 80px
```

---

## 5. Component tokens

```yaml
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.button}"
    rounded: "{rounded.md}"
    padding: 10px 18px
  card:
    backgroundColor: "{colors.surface-card}"
    textColor: "{colors.ink}"
    rounded: "{rounded.lg}"
    padding: 20px
```

---

## 6. Overview prose

```markdown
## Overview

Lumenpath reads like a product brand that wants to feel both warm and
deliberate...

**Key Characteristics:**
- Single accent color: {colors.primary}
- Modest display weights
- Hairline-only depth
```

---

## 7. Colors, Typography, Layout, Elevation prose

Example:

```markdown
## Colors

### Brand & Accent
- **Tangerine** ({colors.primary} — #F76B1C): The single brand color.
```

These sections expand on tokens with rationale.

---

## 8. Components prose

```markdown
## Components

**`button-primary`** — The signature primary CTA.
Background `{colors.primary}`, text `{colors.on-primary}`,
type `{typography.button}`, rounded `{rounded.md}`.
```

YAML components and prose components must match 1:1.

---

## 9. Responsive Behavior and Known Gaps

```markdown
## Responsive Behavior

| Name | Width | Key Changes |
|---|---|---|
| Mobile | < 720px | Hamburger nav; cards stack. |

### Touch Targets
- Primary CTAs ≥ 44 × 44px.

## Known Gaps
- Animation timings not defined.
- Dark mode not documented.
```

---

## Why these 9 pieces?

The structure mirrors how an agent makes UI decisions:

1–5 define exact values.
6 explains why it looks that way.
7 clarifies where tokens apply.
8 defines components.
9 defines behavior and limits.

Each section builds on the previous one.

---

## Disclaimer

These are not official design systems from the listed companies. They are curated starting points inspired by publicly visible design patterns.

All trademarks, brand names, and design elements belong to their respective owners. These DESIGN.md files document publicly observable design patterns for educational and development purposes.