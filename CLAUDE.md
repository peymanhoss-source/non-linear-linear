# Agent Monitoring — Design Prototype

Design challenge: tickets can be assigned to an AI agent. We need a way to see how the agent is doing on each ticket and whether it needs human input to get unblocked.

**Persona:** a TPM / project manager overseeing a team of human devs and AI agents through a sprint (2-week cycle). They watch progress across the cycle, and act as the gatekeeper between agents and the rest of the team: agents ask the TPM first; the TPM decides whether an agent may contact a dev directly.

**Agent input types** (what an agent can be blocked on):
- **Approval** — a yes/no decision.
- **Access** — permission to access or integrate with another service.
- **Person** — needs input from another dev. The TPM brokers this: answer it themselves, or approve the agent contacting that dev directly. Agents never ping devs without the TPM's go-ahead.

**Trust rule (human ↔ agent communication):**
- Humans can ping agents whenever they want. No gate.
- Agents need the TPM's permission before they ping any dev or other technical resource on the team. The UI must never show an agent contacting a human without a visible TPM go-ahead.

**Priority:** 4 tiers: High, Medium, Low, No priority (no "Urgent"). Lists, queues and cards sort **High → No priority** by default. Within a tier, time waiting breaks ties.

This is a throwaway prototyping exercise. Optimize for speed of iteration and visual fidelity, not production code quality.

## Layout

- **No app nav/sidebar** for now. Just the main container.
- **Desktop only**, designed at **1440px** width. No responsive work needed.

## Build & run

- Everything lives in **one file**: `index.html` (inline CSS + JS). No framework, no bundler, no npm, no build step.
- Run it by opening the file directly in a browser (`open index.html`). If a server is ever needed, `python3 -m http.server` is the ceiling.
- Plain vanilla JS is the default. A CDN script is OK only if it clearly earns its place — ask first.

## Visual style (Linear-inspired)

- **Light theme only.**
- **Font:** Inter (Google Fonts), 13px body. Small type scale: 11px meta / 12px secondary / 13px body / 14–15px headings. Use `font-variant-numeric: tabular-nums` for IDs, counts, timestamps.
- **Borders:** 1px hairlines everywhere (`#e6e6e9`-ish). Avoid heavy shadows; at most a soft shadow on floating surfaces (menus, popovers).
- **Density:** dense rows (~32–36px), tight padding, enterprise/list-heavy feel. Prefer tables, lists, and side panels over cards.
- **Color:** mostly muted grays. One **accent color** reserved for meaningful signal — status, priority, "needs input", active agent state. Don't spend the accent on decoration.
- Define all colors/spacing as CSS custom properties on `:root` so palettes are easy to tweak.
- Small radii (4–6px), restrained iconography (inline SVG, 14–16px, stroke-based).

## Data

- **All data is fake** and hardcoded in a JS object at the top of the script.
- Make it feel like a real project-management dataset: team names, ticket IDs (e.g. `ENG-1423`), realistic titles, priorities, labels, cycles, assignees (humans and agents), timestamps.
- **AI agent output is faked too**: step logs, plans, tool calls, diffs/summaries, questions to the human, confidence, elapsed time. Make it read as plausible and specific to the ticket, not lorem ipsum.

## Explorations

- When asked for explorations, **do not create new pages or files**. Keep everything in `index.html`.
- A fixed **demo bar** (small, unobtrusive, visually distinct from the product UI — e.g. a dark strip at the bottom) lets me toggle between options for each exploration.
- Each exploration gets a labeled group in the demo bar (e.g. `Inbox: A · B · C`). Switching options swaps the relevant UI in place; the rest of the page stays put.
- Persist the current demo-bar selection in `localStorage` (wrapped in try/catch) so reloads keep the state.
- Keep previous options around unless I say to remove them — they're there for comparison.

## Process

- **Always ask before committing to an exploration.** Propose the directions (briefly: what each option is and what question it answers), wait for my go-ahead, then build.
- Small fixes and tweaks to an already-agreed direction don't need a check-in.
- After building, tell me what changed and which demo-bar toggle to flip to see it.
