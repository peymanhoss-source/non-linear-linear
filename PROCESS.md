# Process log

How this prototype came together. It was built in one Claude Code session on Oct 13, 2026.

Each step below has:
- **Prompt:** what I asked, quoted word for word (typos included).
- **Proposed:** what Claude suggested back.
- **Decision:** what I picked or redirected.
- **Result:** what changed in `index.html`.

One rule shaped the whole session: Claude had to **ask before committing to any exploration**. So most steps are a proposal followed by my call, not straight output.

---

## 1. Ground rules → `CLAUDE.md`

> Ok working on a design challenge. The main prompt is about having a monitoring mechanism. Basically a ticket could be assiogned to an AI agent and there needs to be a view of how it's doing and whether it needs any input to be unblocked.
>
> Before diving in let's setup a baseic Claude.md file with some rules for this prototyping exercise.
>
> * one single file (could be rail), run it locally, no actual build
> * UI is inspired by linear, let's go light theme, inter body font, 13px, 1px borders, more of a dense row/enterprisy look, let's also keep an accent color for maybe things like status or priotity for now. Everything else could be pretty muted
> * fake all the data and the AI outputs, just make it seem relevant to a typical project management type of data set
> * When I ask for explorations, instead of new pages, just keep everything on the same page with a demo bar that would let me toggle between different options.
> * For the sake of this exercise always aks me first before committing to any explorations. Once the md file is down let's get to exploring

**Result:** `CLAUDE.md` with rules for:
- the build (one HTML file, no build step)
- visual style (Linear-like, light, Inter 13px, hairline borders, one accent color for signal only)
- data (all fake but realistic, including agent output)
- explorations (live on one page, toggled from a demo bar)
- process (ask first)

**Proposed:** four exploration areas:
1. Where monitoring lives: inline in the issue list, a dedicated agents view, or an inbox.
2. How an agent run looks inside an issue.
3. The "needs input" interaction.
4. Health signals.

## 2. Reframing around the TPM

> What I had in mind is desining a view for a TPM or project managerment type of role. This person needs to monitor the activities of both agents and devs on their team during a sprint (2 week cycle). At a high level there could be a list view of all the issues within this sprint/projects. Let's focus on having a summary card that highlights what the agents are executing. And within that let's prioritize this based on potentially tasks/tickets that the agents need input on. The inputs could potentially be:
>
> * An approval (yes / no)
> * Accessing or integration with other services
> * Input from another dev resource on the team (the TPM could then give the agent the go ahead to go ahead and directly contact that dev person directly - but always acts as a middle person before the agent can just feely bombard a dev).
>
> Let's think about a few views. Potentailly clicking on one of the items in this card would then open up a side-panel to view more details on the task.
>
> Forgout but in the MD.file for now let's not worry about having a nav. Just focus on the main container. Everythinf could be on desktop for now 1440px width

**Result:** `CLAUDE.md` updated with:
- the TPM persona
- the three input types (Approval, Access, Person)
- the TPM-as-gatekeeper idea
- no nav, desktop at 1440px

**Proposed:** three layouts for the summary card:
- **A · Triage queue:** one list ranked by priority, then by time waiting, with quick actions in each row.
- **B · By input type:** columns for Approvals, Access and People.
- **C · Agent roster:** one row per agent, with blocked agents first.

Plus one side-panel design, with a different body for each input type.

## 3. Build all three

> Let's actually look at all three options and use the demo bar to toggle between them. for the side panel just go with one view for now

**Result:** the first full build:
- a Cycle 24 header, the summary card and the issue list
- 34 fake tickets on a Payments team (6 devs, 5 agents)
- a demo bar to switch the card between **A / B / C**
- the side panel, with decisions that actually change the data: tickets leave the queue, toasts appear, and a "Reset data" button puts everything back

Two choices Claude made without asking, and flagged:
- Quick Approve/Deny buttons in a row only appear for **low-risk** approvals. The production migration requires opening the panel.
- Access requests always go through the panel, so the scopes are visible before granting.

## 4. Priority tiers and a trust rule (sent mid-build)

> One you're done one key aspect is around priotiziation. Obviously any tasks is assigned some level of prioritiy. 4 tiers: no priotity,low, medium,high
> And let's by default organize the content in that list based on high to no priority.
>
> One other interesting angle is also thinking about the co-dependency between potentially the agents and humans trying to do work. In some cases potentially a human could ping the agent for input or like mentioned above the agent could be in need of input/access to other services (integreations) by the humans.
>
> Let's create a rule for establishing better trust. Humans can ping agents whenever they want. Agents need permission from the TPM to ping any dev or other technical resource on the team.

**Result:**
- Urgent was removed, leaving High, Medium, Low and No priority. Everything sorts high → none.
- The issue list now groups by priority by default, with a Priority / Status toggle.
- The **trust rule** went into `CLAUDE.md`: humans can ping agents whenever they want, but agents need the TPM's go-ahead before contacting any dev. The UI shows that go-ahead as an explicit button.

## 5. Side panel: push the content over instead of overlaying it

> let's make the side panel not be a flyout. It should just push the content in the main container to the left. so you'd be able to see stuff side-by-side

**Result:** the panel became a docked 520px column that pushes the content over. While it's open, the card and list tighten up (fewer columns, smaller gaps) so all three card options still fit next to it.

## 6. Humans in the mix

> Ok this is interesting. Let's focus on the human/agent interaction and pings between them. I didn't realize in that top agent activity card every row was by an agent. It's more interesting to have a mix of agents and actualy humans in there that both need some input. THere could be a filter to filter off of just agents or just humans. Also a mechanism to filter off of priority.

**Proposed:**
- Treat every item as a request with a sender and a recipient: agent → you, agent → dev (gated by you), human → you, and human → agent (information only, since no approval is needed).
- Add filters for **From** (All · Agents · Humans) and **Priority**.
- Explore three layouts: a unified queue, two lanes ("Waiting on you" and "Between teammates"), or conversation rows that show chains like `Leo → Forge → You`.

**Status:** still open. The next prompts went back to the side panel first (see [Open threads](#open-threads)).

## 7. Put the request inside the plan

> ok the side panel is interesting. but what would be more meaningful is potentially having the "what do to next" be incorporated in to the plan section. The idea being you'd be able to see what's the exact step the agent is stuck on and needs input on. As opposed to it being a separate section in the side panel right now. Also the agent activity log could potentially be something that is burried a layer deeper. For now let's just focus on having it be either a link or pop-up to another panel but not something that needs to be surfaced at a high level

**Result:**
- The plan became a vertical timeline of the agent's steps.
- The blocked step is tagged **Needs your input**, and the request (approve, grant, answer) sits **right under that step**.
- After a decision, a note stays on the step, e.g. "Approved by you · 16:21".
- The activity log moved behind an **Activity log ›** link that opens a second view with a back button.

## 8. Simplify the "question for you" input

> let's focus on simplying the input when the agent needs questions. But instead of the three options with the radio buttons. What if there's a free text input to give it an answer. And then there's an alternative to just ping Priya. The current options seems really bloated/cluttered.

**Result:** the three radio options were replaced with:
- The agent's actual question, plus one line of context.
- A **text box with Send answer** (⌘↵ also sends).
- One secondary line: **"Or let Atlas ask Priya"**, with an **Ask Priya** button and an optional preview of the message the agent would send. That button is the TPM go-ahead the trust rule requires.

## 9. Less ticket detail, link to the issue instead

> instead of a lot o more detail about the ticket in that card let's also have a link to that issue in there.

**Result:** the properties block in the side panel was replaced by one line (status, priority, agent) and an **Open issue ↗** link. The title also links to the issue.

---

## Open threads

Proposed but not built yet, from step 6:
- Mixing **human** requests into the summary card alongside agent requests.
- **From** and **Priority** filters on the card, and possibly shared with the issue list.
- Which layout to use for the mix: unified queue, two lanes, or conversation chains.
- Whether to keep the agent-only card options (B · By input type and C · Agent roster) once humans are added.
