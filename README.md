# Agent monitoring: design challenge prototype

**▶ To try it:** click the green **Code** button → **Download ZIP**, unzip it, and open `index.html` in a browser. Designed for desktop at 1440px.

**The prompt:** tickets can be assigned to an AI agent. Design a way to see how the agent is doing, and whether it needs input to get unblocked.

**My framing:** a **TPM** running a two-week cycle with a mixed team of devs and AI agents. The TPM needs to see at a glance which agents are blocked on them, then unblock them without leaving the page.

Agents can be blocked on three kinds of input:

| Type | Example |
|---|---|
| **Approval** | "Run the production backfill on `refunds` (2.1M rows)?" |
| **Access** | "Write access to AWS SQS (staging) + CloudWatch read" |
| **Person** | "Is `settled_at` stored in UTC or merchant time?" The TPM can answer it, or let the agent ask Priya |

**Trust rule:** humans can ping agents whenever they want. Agents need the TPM's go-ahead before contacting any dev.

## Run it

Open `index.html` in a browser. There's no build step or install.

- Designed for desktop at **1440px**.
- An internet connection loads the Inter font. Without it, the page falls back to the system font.
- All data and agent output are fake.

## What to try

- **Demo bar (bottom left):** switches the summary card between three layouts:
  - **A · Triage queue:** one list ranked by priority, then by time waiting.
  - **B · By input type:** columns for Approvals, Access and People.
  - **C · Agent roster:** one row per agent, with blocked agents first.
- **Click any agent ticket** in the card or the list to open the side panel next to the page. The agent's plan shows as a timeline, and the request sits **on the exact step it's blocking**.
- **Decide on something:**
  - approve the Safari test quarantine
  - grant AWS access
  - answer Atlas's question, or let Atlas ask Priya

  The queue, the list and the plan all update.
- **Activity log ›** in the panel opens the agent's raw activity, one level deeper.
- **Reset data** in the demo bar puts everything back.

## Files

| File | What it is |
|---|---|
| `index.html` | The prototype: one file, inline CSS and JS |
| `PROCESS.md` | Step-by-step log of how it was built: my prompts word for word, what was proposed, what I decided |
| `CLAUDE.md` | The ground rules I set for the AI pair (style, data, explorations, process) |
| `WithinAI-Challenge-Figjam.jam` | FigJam board. Opens in FigJam, or view it here: _[add share link]_ |

Built with Claude Code. See `PROCESS.md` for the full story.
