# Docket — UI style prompt

Paste this whole file into Claude Code (or any coding agent) and then describe the screen you want. Every value here comes from a measured Figma reference. Where something wasn't in that reference, it was derived from the same rules and is marked *(derived)*.

---

## Your role

You are building UI in the **Docket** style using **React + Tailwind CSS v4**. Docket is a quiet, precise, record-keeping aesthetic: near-monochrome surfaces, one blue accent, hairline edges made of shadow, regular-weight type only, and monospace reserved for machine data. It should feel like a well-kept clinical file, not a marketing page.

Follow the tokens and recipes below exactly. Don't invent new colors, weights, radii, or shadows. If a screen needs something not covered here, build it from the closest recipe and the rules in "Principles", and say what you derived.

---

## Principles

1. **Monochrome first.** Grays carry all the hierarchy. **Blue (`accent`) marks only the chosen thing**: the selected option, the primary action, focus. Red appears only as the required asterisk and in error text.
2. **One weight.** Everything is weight 400. Hierarchy comes from size (24 / 18 / 11) and gray value (ink → muted). Never bold, never semibold.
3. **Mono means machine.** Geist Mono is only for system and record data: IDs, dates, codes, eyebrow headings on a shell, and attribution labels. Human-facing text (labels, options, buttons, descriptions) is always Geist.
4. **Edges are shadows, not borders.** Surfaces get a 0.5px shadow ring plus a tiny soft drop shadow. Use `border` only for controls' internal dividers and radio and checkbox outlines.
5. **Tight lines, generous gaps.** Single-line UI text uses `line-height: 1`. Space comes from gaps (12px between a label and its description) and tall rows (100px option rows), not from leading.
6. **Folder layering.** A record or decision sits in a **shell**: a slightly tinted outer surface with a hatched header strip (context) and a footer (attribution). The white **card** sits inside it, flush to its sides.
7. **Solid separates zones, dashed separates alternatives.** A solid line divides a card's header from its body. Dashed lines divide sibling choices or rows.
8. **Tactile metadata.** Machine values sit in a **keycap chip**: a tiny embossed key with an inset white highlight. This is the style's signature detail. Use it sparingly, at most a few per view.
9. **Two left edges.** Titles, pills and eyebrows align at 20px. Controls (radios, avatars) align at 27.5px, and their text follows 8px after the control.

---

## Setup

### Fonts
- Next.js: `npm i geist`, then `import { GeistSans } from "geist/font/sans"` and `import { GeistMono } from "geist/font/mono"`, and apply their `.variable` classes to `<html>`.
- Anything else: `https://fonts.googleapis.com/css2?family=Geist:wght@400&family=Geist+Mono:wght@400&display=swap`
- Load weight 400 only.

### `globals.css` (Tailwind v4)

```css
@import "tailwindcss";

@custom-variant dark (&:where(.dark, .dark *));

/* ---------- Raw values: light ---------- */
:root {
  --canvas: #F9F9F9;           /* page background */
  --shell-top: #FFFFFF;        /* shell gradient, top */
  --shell-bottom: #FCFCFC;     /* shell gradient, bottom 22.9% */
  --surface: #FFFFFF;          /* cards, inputs, selects */
  --fill: #EDEDED;             /* pills, soft buttons, icon buttons */
  --fill-hover: #E5E5E5;       /* (derived) */
  --well: #F9F9F9;             /* select chevron well, table header */
  --keycap: #F1F0F0;

  --ink: #1E1E1E;              /* primary text */
  --ink-strong: #131313;       /* primary icons */
  --ink-muted: #646464;        /* descriptions, secondary text, select value */
  --ink-tag: #5D5D5D;          /* text on fill (pills) */
  --ink-chip: #6D6D6D;         /* keycap text */
  --ink-icon: #606060;         /* secondary icons */
  --ink-faint: #8A8A8A;        /* (derived) placeholders only */

  --line: #E7E7E7;             /* solid zone divider */
  --line-dash: #DFDFDF;        /* dashed divider, radio/checkbox outline */
  --line-strong: #D6D6D6;      /* divider inside a control */
  --keycap-ring: #D3D3D3;

  --accent: #006199;
  --accent-hover: #00527F;     /* (derived) */
  --on-accent: #FFFFFF;
  --required: #FF2525;
  --danger: #C4161C;           /* (derived) error text, 6.0:1 on white */

  --hatch: rgb(161 161 161 / 0.06);
  --hover: rgb(0 0 0 / 0.02);  /* (derived) row/ghost hover wash */
  --press: rgb(0 0 0 / 0.04);  /* (derived) */

  --elev-raised: 0 0 0 0.5px rgb(0 0 0 / .14), 0 0.5px 1px -0.5px rgb(0 0 0 / .10), 0 1px 2px 0 rgb(0 0 0 / .18);
  --elev-card:   0 0 0 0.5px rgb(0 0 0 / .12), 0 0.5px 1px -0.5px rgb(0 0 0 / .10), 0 1px 2px 0 rgb(0 0 0 / .06);
  --elev-keycap: inset 0 -0.5px 0.5px #FFFFFF, inset 0 1.25px 1px #FFFFFF, 0 0.5px 1.5px rgb(0 0 0 / .18), 0 0 0 0.5px var(--keycap-ring);
  --elev-overlay: 0 0 0 0.5px rgb(0 0 0 / .12), 0 4px 12px -2px rgb(0 0 0 / .12), 0 12px 32px -8px rgb(0 0 0 / .12); /* (derived) popovers, toasts */
  --elev-primary: inset 0 1px 0 rgb(255 255 255 / .12), 0 0 0 0.5px rgb(0 0 0 / .20), 0 1px 2px rgb(0 0 0 / .18); /* (derived) */
}

/* ---------- Raw values: dark (derived) ---------- */
.dark {
  --canvas: #0E0E0E;
  --shell-top: #161616;
  --shell-bottom: #131313;
  --surface: #1A1A1A;
  --fill: #2A2A2A;
  --fill-hover: #323232;
  --well: #202020;
  --keycap: #242424;

  --ink: #EDEDED;
  --ink-strong: #F5F5F5;
  --ink-muted: #A0A0A0;
  --ink-tag: #ABABAB;
  --ink-chip: #A3A3A3;
  --ink-icon: #A3A3A3;
  --ink-faint: #7A7A7A;

  --line: #2A2A2A;
  --line-dash: #333333;
  --line-strong: #3A3A3A;
  --keycap-ring: #3A3A3A;

  --accent: #5AAEE0;
  --accent-hover: #7FC0E8;
  --on-accent: #0B1A24;
  --required: #FF5A5A;
  --danger: #FF7A70;

  --hatch: rgb(255 255 255 / 0.025); /* same ~6-level contrast as the light hatch */
  --hover: rgb(255 255 255 / 0.03);
  --press: rgb(255 255 255 / 0.05);

  --elev-raised: 0 0 0 0.5px rgb(255 255 255 / .12), 0 0.5px 1px -0.5px rgb(0 0 0 / .4), 0 1px 2px 0 rgb(0 0 0 / .5);
  --elev-card:   0 0 0 0.5px rgb(255 255 255 / .09), 0 0.5px 1px -0.5px rgb(0 0 0 / .4), 0 1px 2px 0 rgb(0 0 0 / .35);
  --elev-keycap: inset 0 -0.5px 0.5px rgb(255 255 255 / .04), inset 0 1.25px 1px rgb(255 255 255 / .06), 0 0.5px 1.5px rgb(0 0 0 / .5), 0 0 0 0.5px var(--keycap-ring);
  --elev-overlay: 0 0 0 0.5px rgb(255 255 255 / .10), 0 4px 12px -2px rgb(0 0 0 / .5), 0 12px 32px -8px rgb(0 0 0 / .6);
  --elev-primary: inset 0 1px 0 rgb(255 255 255 / .18), 0 0 0 0.5px rgb(0 0 0 / .4), 0 1px 2px rgb(0 0 0 / .4);
}

/* ---------- Tailwind tokens: static ---------- */
@theme {
  /* With next/font (the `geist` package) the variables below are defined; elsewhere the "Geist" fallback is used. */
  --font-sans: var(--font-geist-sans, "Geist"), ui-sans-serif, system-ui, sans-serif;
  --font-mono: var(--font-geist-mono, "Geist Mono"), ui-monospace, monospace;

  --text-title: 24px;  --text-title--line-height: 1;
  --text-body: 18px;   --text-body--line-height: 1;
  --text-dense: 15px;  --text-dense--line-height: 1;   /* (derived) tables, hints */
  --text-meta: 11px;   --text-meta--line-height: 1;    /* mono only */

  --radius-card: 12px;
  --radius-control: 9px;
  --radius-pill: 6.5px;
  --radius-keycap: 1.5px;

  --ease-out-soft: cubic-bezier(0.23, 1, 0.32, 1);
}

/* ---------- Tailwind tokens: themed (swap with .dark) ---------- */
@theme inline {
  --color-canvas: var(--canvas);
  --color-surface: var(--surface);
  --color-fill: var(--fill);
  --color-fill-hover: var(--fill-hover);
  --color-well: var(--well);
  --color-keycap: var(--keycap);
  --color-ink: var(--ink);
  --color-ink-strong: var(--ink-strong);
  --color-ink-muted: var(--ink-muted);
  --color-ink-tag: var(--ink-tag);
  --color-ink-chip: var(--ink-chip);
  --color-ink-icon: var(--ink-icon);
  --color-ink-faint: var(--ink-faint);
  --color-line: var(--line);
  --color-line-dash: var(--line-dash);
  --color-line-strong: var(--line-strong);
  --color-accent: var(--accent);
  --color-accent-hover: var(--accent-hover);
  --color-on-accent: var(--on-accent);
  --color-required: var(--required);
  --color-danger: var(--danger);
  --color-hover: var(--hover);
  --color-press: var(--press);

  --shadow-raised: var(--elev-raised);
  --shadow-card: var(--elev-card);
  --shadow-keycap: var(--elev-keycap);
  --shadow-overlay: var(--elev-overlay);
  --shadow-primary: var(--elev-primary);
}

@utility bg-shell { background: linear-gradient(to top, var(--shell-bottom) 22.883%, var(--shell-top) 100%); }
@utility bg-hatch { background-image: repeating-linear-gradient(128deg, var(--hatch) 0 6.661px, transparent 6.661px 9.403px); }
@utility divider-solid { height: 0.75px; background: var(--line); }
@utility divider-dashed {
  height: 0.75px;
  background-image: repeating-linear-gradient(to right, var(--line-dash) 0 6px, transparent 6px 12px);
  background-position: -3px 0; /* 3px half-dash at the start, like the reference */
}

body {
  background: var(--canvas);
  color: var(--ink);
  font-family: var(--font-sans);
  font-weight: 400;
  -webkit-font-smoothing: antialiased;
}
```

---

## Scale and rhythm

| Thing | Value |
|---|---|
| Type | title 24 · body 18 · dense 15 *(derived)* · meta 11 (mono) |
| Line height | 1 for single-line UI text; **1.4** for anything that can wrap *(derived)* |
| Label → description gap | 12 |
| Card padding (x) | 20 |
| Card header | 32 top · 28 title row · 20 gap · 26 pills · 34 bottom (140 total) |
| Option row | 100 tall; content starts 27.5 from the top |
| Control height | 39.5 (select, input, button) · 28 icon button · 26 pill · 15 keycap |
| Gaps between pills | 4 |
| Radii | card 12 · control 9 · pill 6.5 · keycap 1.5 · round full |
| Hairlines | rings 0.5px · strokes 0.75px · icon strokes 2.25px |
| Card width | 600 in the reference; cards can be any width, but keep the 20px gutter |

Half-pixel values are intentional. They render as crisp single device pixels on 2× screens. Keep them, but **only inside a component, never in the page flow**:

- **Snap stacked content to whole pixels.** A 39.5px control in normal flow pushes everything below it onto half-pixel boundaries. On 1× screens, 0.75px dividers there blur into nothing; dashed ones disappear entirely. Whenever content stacks below a 39.5px control, put the control in a 40px slot: `<div className="flex h-10 items-center">…</div>`. Inside a row that already has a fixed height (like the 100px option row), no slot is needed.
- **Dividers belong on whole-pixel edges.** Place them at the top or bottom of a block whose position is a whole number. Don't stack them under fractional heights.
- **Check at 1×.** Before you finish, render at device scale factor 1 and zoom into a dashed divider below the fold. If it's missing or smeared, something above it has a fractional height.

---

## Core components (from the reference)

### Icons
Thick, rounded, quiet. A chevron is 10×5.23 with a 2.25px stroke, round caps and round joins. Draw icons yourself at this weight, or use Lucide at `strokeWidth={2.25}` sized 10–14px.

```tsx
export function Chevron({ solid = false, className = "" }: { solid?: boolean; className?: string }) {
  return (
    <svg viewBox="0 0 19.954 10.452" width={9.977} height={5.226} aria-hidden
         className={`overflow-visible ${className}`}>
      <path d="M0 0 L9.977 10.452 L19.954 0" stroke="currentColor" strokeWidth={4.5}
            strokeLinecap="round" strokeLinejoin="round" fill={solid ? "currentColor" : "none"} />
    </svg>
  );
}

export function RequiredMark() {
  return (
    <svg viewBox="0 0 19 19" width={9.5} height={9.5} aria-hidden className="translate-y-[1.75px] text-required">
      <path fill="currentColor" d="M5.81176 19L2.90588 17.0774L6.65 11.3095H0V7.69048H6.65L2.90588 1.92262L5.81176 0L9.5 6.0506L13.2441 0L16.15 1.92262L12.35 7.69048H19V11.3095H12.35L16.15 17.0774L13.2441 19L9.5 13.006L5.81176 19Z"/>
    </svg>
  );
}
```
- The **outline** chevron is for disclosure (collapse or expand).
- The **solid** chevron (filled and stroked, a rounded triangle) is for select triggers.

### Shell and card (the folder)
```tsx
export function Shell({ eyebrow, required, footer, children }: {
  eyebrow: string; required?: boolean; footer?: React.ReactNode; children: React.ReactNode;
}) {
  return (
    <section className="bg-shell rounded-card shadow-raised overflow-hidden">
      <header className="bg-hatch h-[50px] px-5 flex items-center gap-[3px]">
        <h2 className="font-mono text-body text-ink">{eyebrow}</h2>
        {required && <><RequiredMark /><span className="sr-only">(required)</span></>}
      </header>
      <div className="bg-surface rounded-card shadow-card">{children}</div>
      {footer}
    </section>
  );
}
```
Use the shell for anything that is *a record being decided on or reviewed*. A plain view can use the card on its own (`bg-surface rounded-card shadow-card`).

### Card header
```tsx
<div className="relative px-5 pt-8 pb-[34px]">
  <div className="flex h-7 items-center justify-between">
    <h3 className="text-title text-ink">Staff decision</h3>
    <IconButton label="Collapse" />
  </div>
  <div className="mt-5 flex gap-1">{/* Pills */}</div>
  <span aria-hidden className="divider-solid absolute inset-x-0 bottom-0 translate-y-1/2" />
</div>
```

### Pill and keycap
```tsx
export function Pill({ children, value }: { children: React.ReactNode; value?: string }) {
  return (
    <span className={`inline-flex h-[26px] items-center gap-[4.5px] rounded-pill bg-fill pl-2 ${value ? "pr-[5px]" : "pr-2"} text-body text-ink-tag`}>
      {children}
      {value && <Keycap>{value}</Keycap>}
    </span>
  );
}

export function Keycap({ children }: { children: React.ReactNode }) {
  return (
    <span className="inline-flex items-center rounded-keycap bg-keycap px-1 py-0.5 font-mono text-meta text-ink-chip shadow-keycap tabular-nums">
      {children}
    </span>
  );
}
```
- **Pill:** a human-readable attribute of the record ("Record 0417", "Diagnosis date").
- **Keycap:** the machine value ("02/14/2026") or a system label ("Possible reviewer").

### Icon button
```tsx
export function IconButton({ label, expanded = true, onClick }: { label: string; expanded?: boolean; onClick?: () => void }) {
  return (
    <button type="button" aria-label={label} aria-expanded={expanded} onClick={onClick}
      className="grid size-7 place-items-center rounded-full bg-fill text-ink-strong
                 transition-[background-color,scale] duration-150 ease-out-soft
                 hover:bg-fill-hover active:scale-[.94]
                 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-accent">
      <Chevron className={`transition-transform duration-200 ease-out-soft ${expanded ? "" : "-rotate-90"}`} />
    </button>
  );
}
```

### Option rows (radio group)
```tsx
export function OptionRow({ name, value, label, description, defaultChecked, trailing, first }: {
  name: string; value: string; label: string; description: string;
  defaultChecked?: boolean; trailing?: React.ReactNode; first?: boolean;
}) {
  return (
    <label className="group relative flex h-[100px] cursor-pointer items-start pl-[27.5px] pr-5 pt-[27.5px]
                      transition-colors duration-150 hover:bg-hover active:bg-press">
      {!first && <span aria-hidden className="divider-dashed absolute inset-x-0 top-0 -translate-y-1/2" />}
      <input type="radio" name={name} value={value} defaultChecked={defaultChecked} className="sr-only" />
      <span className="flex h-[18px] items-center">
        <span className="size-4 rounded-full border-[0.75px] border-line-dash bg-surface
                         transition-[border-width,border-color] duration-150 ease-out-soft motion-reduce:transition-none
                         group-has-[:checked]:border-[3.5px] group-has-[:checked]:border-accent
                         group-has-[:focus-visible]:outline-2 group-has-[:focus-visible]:outline-offset-2 group-has-[:focus-visible]:outline-accent" />
      </span>
      <span className="ml-2 flex flex-1 items-center gap-4">
        <span className="flex flex-col gap-3 text-body">
          <span className="text-ink">{label}</span>
          <span className="text-ink-muted">{description}</span>
        </span>
        {trailing && <span className="ml-auto">{trailing}</span>}
      </span>
    </label>
  );
}
```
- The radio's center is the **surface** color, never hard-coded white, so it works in dark mode.
- The selected and focus styles key off the label (`group-has-[:checked]`), not `peer-*`. `peer-*` only reaches siblings of the input, and the circle is nested inside one.
- Wrap the rows in `<fieldset><legend className="sr-only">…</legend>…</fieldset>`.

### Split select
```tsx
export function SplitSelect({ value, onOpen }: { value: string; onOpen?: () => void }) {
  return (
    <button type="button" aria-haspopup="listbox" onClick={onOpen}
      className="flex h-[39.5px] items-stretch overflow-hidden rounded-control bg-surface shadow-raised
                 transition-[scale] duration-150 ease-out-soft active:scale-[.98]
                 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-accent">
      <span className="flex items-center pl-[10.5px] pr-[15px] text-body text-ink-muted">{value}</span>
      <span aria-hidden className="w-px bg-line-strong" />
      <span className="grid w-[39px] place-items-center bg-well text-ink-icon"><Chevron solid /></span>
    </button>
  );
}
```

### Attribution footer (on the shell)
```tsx
<footer className="flex gap-[13.5px] pl-[27.5px] pr-5 pt-[24.5px] pb-[24.5px]">
  <img src={avatarUrl} alt="" className="mt-1 size-11 rounded-full object-cover" />
  <div className="flex flex-col items-start gap-3">
    <Keycap>Possible reviewer</Keycap>
    <span className="text-body text-ink">Dr. Steven Minucchie</span>
    <span className="text-body text-ink-muted">Reviewed on --</span>
  </div>
</footer>
```
An empty value is written `--`, never "N/A" or "None".

---

## Derived components

These follow the same rules. Use them as written.

### Buttons
All buttons are 39.5px tall, radius `control`, `text-body`, weight 400, with `px-4` horizontal padding.

| Variant | Use | Classes |
|---|---|---|
| Primary | The one committing action per view ("Save decision") | `bg-accent text-on-accent shadow-primary hover:bg-accent-hover` |
| Secondary | Neutral actions next to primary | `bg-surface text-ink shadow-raised hover:bg-well` |
| Soft | Low-stakes or in-card actions | `bg-fill text-ink hover:bg-fill-hover` |
| Ghost | Tertiary and cancel | `text-ink-muted hover:bg-hover hover:text-ink` |

Shared classes:
`inline-flex h-[39.5px] items-center justify-center gap-2 rounded-control px-4 text-body transition-[background-color,color,scale] duration-150 ease-out-soft active:scale-[.97] focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-accent disabled:pointer-events-none disabled:opacity-45`

- **Loading:** set `aria-busy`. Keep the button's width, set the label to `opacity-0`, and center a 14px spinner over it: a ring with a 1.5px stroke in `currentColor` at 25% plus a 90° arc at 100%, rotating 700ms linear.
- **Destructive:** use secondary with `text-danger`. Never a red-filled button.

### Text field
```tsx
<label className="flex flex-col gap-2">
  <span className="text-body text-ink">Date of death</span>
  <input className="h-[39.5px] rounded-control bg-surface px-[10.5px] text-body text-ink shadow-raised
                    placeholder:text-ink-faint outline-none
                    focus:shadow-[var(--elev-raised),0_0_0_2px_var(--accent)]
                    aria-[invalid=true]:shadow-[var(--elev-raised),0_0_0_1px_var(--danger)]"
         placeholder="MM/DD/YYYY" />
  <span className="text-dense text-ink-muted">Use the date on the certificate.</span>
</label>
```
- Error: replace the hint with `text-dense text-danger` text that says what to do ("Enter a date after 01/01/2020"). Validate on blur, not while the user is typing.
- Dates, IDs and codes typed into a field stay in Geist (sans). Use mono only when the value is shown back, read-only, in a keycap.

### Checkbox
A 16px square with radius 4. Unchecked: `border-[0.75px] border-line-dash bg-surface`. Checked: `bg-accent border-accent` with a white (`text-on-accent`) check, 2px stroke, round caps. Align it to the first line of its label exactly like the radio.

### Segmented tabs
- Track: `inline-flex h-[39.5px] gap-0.5 rounded-control bg-fill p-[3px]`
- Segment: `h-full px-3 rounded-pill text-body text-ink-tag transition-colors duration-150`
- Active segment: `bg-surface text-ink shadow-raised`
- Use `role="tablist"` and `role="tab"` with `aria-selected`.

### Table (inside a card)
- Header row: `h-9 px-5 bg-well font-mono text-meta text-ink-chip`, in sentence case, with a `divider-solid` under it.
- Body rows: `h-14 px-5 text-dense text-ink`, with `divider-dashed` between rows and `hover:bg-hover`.
- Secondary columns use `text-ink-muted`. Numbers use `tabular-nums` and are right-aligned. IDs go in a `Keycap`.
- No zebra stripes, no vertical lines, no outer border (the card is the border).

### Empty state
Put it inside the card body, left-aligned at the 20px edge, in a `bg-hatch rounded-control px-5 py-8` panel:
- a `Keycap` label ("No entries")
- one `text-body text-ink-muted` sentence saying what will appear here
- optionally one Soft button

No illustrations.

### Popover / listbox (opens from the split select)
- Container: `rounded-control bg-surface shadow-overlay p-1 min-w-full`
- Options: `h-9 px-[10px] rounded-pill text-body text-ink hover:bg-hover`
- The selected option shows an accent check on the right.
- Animation: enter from `scale-[.97] opacity-0` to `scale-100 opacity-100` over 150ms `ease-out-soft`, `origin-top`. Exit over 100ms.

### Toast
- Container: `fixed bottom-6 left-1/2 -translate-x-1/2 rounded-card bg-surface shadow-overlay px-5 py-4 flex items-center gap-4`
- Content: a `Keycap` eyebrow (e.g. "Saved"), then `text-body text-ink`, then an optional Ghost action.
- Animation: enter from `translate-y-2 opacity-0` over 200ms `ease-out-soft`. Exit over 150ms.
- Auto-dismiss after 5s, and pause while hovered.

---

## Interaction states and motion *(derived)*

- **Hover:** a wash, not a color change. Use `hover:bg-hover` on rows and ghosts, or step to the `*-hover` token on filled controls. Only apply hover on devices that support it (Tailwind v4's `hover:` already does this).
- **Pressed:** `active:scale-[.97]` for buttons, `.94` for icon buttons, `.98` for wide controls. Rows get `active:bg-press` instead of scaling.
- **Focus:** always `focus-visible:outline-2 outline-offset-2 outline-accent`. Text inputs show a 2px accent ring on `:focus`. Never remove focus without a replacement.
- **Disabled:** `opacity-45`, no pointer events, shadows unchanged.
- **Selected:** accent appears. That's the only time blue appears outside of primary buttons and focus.
- **Motion:**
  - Durations are 100–200ms. Easing is `ease-out-soft` for entering; exits are ~30% faster.
  - Animate only `transform`, `opacity`, `background-color`, `color`, and the radio's `border-width`.
  - Nothing animates on page load.
  - Collapse and expand use `grid-template-rows: 0fr → 1fr` over 200ms, while the chevron rotates.
  - Add `motion-reduce:transition-none` to anything that moves.

---

## Dark mode *(derived)*

- Toggle the `dark` class on `<html>`. Every token swaps; components need no `dark:` classes.
- Elevation reverses: **the higher a surface, the lighter it is** (canvas `#0E0E0E` → shell `#161616` → card `#1A1A1A`).
- Rings become white at 9–12% opacity, and drop shadows get darker.
- The accent lightens to `#5AAEE0`, and primary buttons put dark text on it (`on-accent`).
- The keycap keeps its emboss with faint white insets.
- Check: every text token pairing stays above 4.5:1. The tokens above were checked: ink-muted 6.7:1, ink-chip 6.2:1, accent 7.1:1.

---

## Voice

- Sentence case everywhere, including mono labels. Never ALL CAPS.
- Options are short verbs or verb phrases ("Use visit note", "Modify", "Hold"). Descriptions say what will happen ("Ask the site", "Enter another date").
- Titles are nouns ("Staff decision", "Conflicting entries").
- No exclamation marks, no emoji, no "Oops". Use `--` for empty values. Write dates as `MM/DD/YYYY` in mono.

---

## Accessibility notes

- The unselected radio and checkbox outlines (`line-dash`, 1.33:1) are deliberately faint and stay faithful to the reference. The label text carries the meaning. If you need strict WCAG 1.4.11 conformance, swap that outline to `#949494` (3:1) and say so.
- The required asterisk is decorative (3.8:1). Always pair it with `sr-only` "(required)" and the `required` attribute on the field.
- The 11px mono keycaps are metadata. Never put the only copy of critical information in them.
- Inputs use 18px text, so iOS won't zoom on focus.

---

## Reference composition

This is the original screen at 1×. Use it to check your output.

```tsx
export function ConflictingEntries() {
  return (
    <div className="w-[600px]">
      <Shell eyebrow="Conflicting entries" required footer={
        <footer className="flex gap-[13.5px] pl-[27.5px] pr-5 pt-[24.5px] pb-[24.5px]">
          <img src="/avatar.png" alt="" className="mt-1 size-11 rounded-full object-cover" />
          <div className="flex flex-col items-start gap-3">
            <Keycap>Possible reviewer</Keycap>
            <span className="text-body text-ink">Dr. Steven Minucchie</span>
            <span className="text-body text-ink-muted">Reviewed on --</span>
          </div>
        </footer>
      }>
        <div className="relative px-5 pt-8 pb-[34px]">
          <div className="flex h-7 items-center justify-between">
            <h3 className="text-title text-ink">Staff decision</h3>
            <IconButton label="Collapse staff decision" />
          </div>
          <div className="mt-5 flex gap-1">
            <Pill>Record 0417</Pill>
            <Pill value="02/14/2026">Diagnosis date</Pill>
          </div>
          <span aria-hidden className="divider-solid absolute inset-x-0 bottom-0 translate-y-1/2" />
        </div>
        <fieldset>
          <legend className="sr-only">Staff decision</legend>
          <OptionRow first name="decision" value="visit-note" defaultChecked
                     label="Use visit note" description="Earliest date clinical entry"
                     trailing={<SplitSelect value="Rule D–04" />} />
          <OptionRow name="decision" value="modify" label="Modify" description="Enter another date" />
          <OptionRow name="decision" value="hold" label="Hold" description="Ask the site" />
        </fieldset>
      </Shell>
    </div>
  );
}
```

Measured geometry to check it against (1×, relative to the shell):

| Element | Position and size |
|---|---|
| Shell | 600×614 |
| Hatch header | 50 tall |
| Card | y50, 600×440 |
| Title | box y34 |
| Icon button | 28px at x552 y32 |
| Pills | y80, 26 tall; "Record 0417" 118.5 wide |
| Header divider | y140 |
| Rows | 100 each |
| Radios | 16px discs at x27.5, centered on each label's first line |
| Select | 153×39.5 at x427, vertically centered on the row's content |
| Footer avatar | 44px at x27.5 y518.5 |
| Keycap | y514.5 |

---

## Before you finish

- [ ] Only weight 400. Only Geist and Geist Mono. Mono only for machine data.
- [ ] No hex values in components. Everything comes from tokens.
- [ ] Blue only on selected state, primary action and focus. Red only on the required mark and error text.
- [ ] Surfaces use shadow rings, not borders. Half-pixel values kept inside components.
- [ ] Nothing in the page flow sits on a half pixel: 39.5px controls with content below them are in 40px slots, and dashed dividers are visible on a 1× render.
- [ ] Solid divider between zones, dashed between alternatives.
- [ ] Every interactive element has hover (where hover exists), pressed, focus-visible and disabled states.
- [ ] Opened in dark mode and checked.
- [ ] No load animations, and `prefers-reduced-motion` respected.
