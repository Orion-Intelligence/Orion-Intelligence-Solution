(deepintel)=

# DeepIntel

DeepIntel covers the AI features of Orion: the AI Workspace with Nexus chat and the Kill Chain view, report chat and AI summaries, and the conversation behaviors that apply to both. AI features are available only when the deployment enables the AI endpoint and the account's role and licenses allow it.

## AI Workspace

AI Workspace is opened from `Profile > AI` when the deployment enables the AI endpoint. It is used for support-style and investigation-assistant conversations inside the profile area, and it also hosts the **Kill Chain** attack-chain planner.

The workspace can support:

- asking investigation or support questions
- reviewing previous messages in the chat rail
- sharing a chat transcript through a tokenized shared link where enabled
- opening shared chat transcripts outside the dashboard shell
- building and running an authorized-target attack chain in the **Kill Chain** view

The workspace has two top-level views, chosen from the view switch in the top bar: `Chat view` and `Kill Chain view`. The current view is kept in the URL as a `?view=` query parameter (`chat`, `directory`, `split`, or `killchain`), so a view can be linked to or reloaded directly. While `Chat view` is active, a secondary layout switch offers `Chat view`, `Directory view`, and `Split view`.

```{admonition} Who can use the AI Workspace
:class: note
Access to the AI workspace and its AI calls depends on role, license, and a per-tenant setting. The AI routes accept the administrator, crawler, member, and analyst roles. Nexus chat and the Kill Chain engine additionally require the AI endpoint to be enabled for the tenant and a scanning license. The `Kill Chain view` is not gated separately inside the workspace, but running a chain or using Deep Intel goes through the same Nexus endpoints, so the same license and endpoint requirements apply.
```

### Nexus Conversation Controls

AI Workspace uses the Nexus assistant surface when the AI endpoint and user license allow access. The workspace opens as a full chat view and can also receive a query context from the surrounding dashboard route.

The main controls are:

- `New Chat` clears the current visible conversation and starts a fresh Nexus session when no response is actively streaming.
- `Share` creates a tokenized shared-chat link for the current visible user and Nexus messages.
- quick prompt buttons prefill the composer with common investigation-assistant prompts.
- the composer sends with `Enter` and inserts a new line with `Shift + Enter`.
- the send button changes to a stop control while Nexus is generating a response.

The composer and edited user messages use a 300-token limit. When a draft is over the limit, the workspace shows how many tokens must be removed before the message can be sent or saved.

### Message Actions

User messages can be copied or edited when Nexus is not currently sending. Editing a user message removes that message and the later conversation turns, places the edited text back into the composer flow, and resends it as a new request. This keeps the visible conversation aligned with the revised prompt instead of leaving stale assistant answers after an edited question.

Nexus responses render markdown when returned by the assistant. Completed Nexus messages expose bot-message actions, while actively streaming messages show an in-progress indicator instead of the completed-message action row.

### History, Streaming, And Recovery

AI Workspace loads saved chat history when the page opens. History preserves user and Nexus messages for the session.

When a response is running, the workspace shows streaming status and step text when available. If the user presses the stop control, the current Nexus response stops streaming and the send control returns to its normal state.

If the page reloads or the user returns while the last saved item is a user message with no matching Nexus response, AI Workspace attempts to resume the active Nexus stream. If recovery fails, the conversation remains visible and the user can retry from the error state where a retry payload is available.

Shared chat links are separate from the editable workspace. A shared transcript opens outside the dashboard shell and shows only the messages included in the generated share payload.

### Kill Chain

The **Kill Chain** view is an interactive graph for planning and running a penetration-testing attack chain against a single authorized target. Each box on the canvas is a node, and nodes are wired together so that the output of one step feeds the next. A node runs a pentest tool, an intelligence/data API, or lets the AI choose the best source for its task. Diamond shapes on the graph represent AI hypotheses and link to the nodes that test them.

The graph is drawn around the six kill-chain phases, shown as node colours and labels: `Reconnaissance`, `Scanning & Enumeration`, `Access & Vuln ID`, `Exploitation`, `Actions on Objectives`, and `Reporting`.

```{admonition} Authorized testing only
:class: warning
Pressing `Run chain` executes real reconnaissance, scanning, and exploitation tooling against the host you enter as the target. Only set a target that you are explicitly authorized to test. Steps that resolve to a host outside the target scope are stopped and marked `blocked — out of scope`.
```

#### Opening Kill Chain

1. Open the AI Workspace from `Profile > AI`.
2. In the top view switch, select `Kill Chain view`. The URL changes to `?view=killchain`.
3. On first open, the view restores any saved chain for the active chat and shows `Loading your kill chain…` while it loads.

#### Setting A Target

1. In the target field at the top left (placeholder `domain, IP, URL or CIDR — press Enter`), type a single domain, IPv4 address, URL, or CIDR range.
2. Press `Enter` to apply it.
3. Until a valid target is set, the canvas shows `Set a target to start building your chain`, and `Add node` and `Run chain` stay disabled.

The target must be a single host, IP, URL, or CIDR. Values with spaces, or anything that is not a recognized domain / IP / URL / CIDR, are rejected.

#### Building The Chain

You can assemble the chain in several ways:

- `Add node` adds an AI node, which you then configure.
- `Scenario library` (the list layout, labelled `Scenario Library` / `Prebuilt pentest chains`) loads a prebuilt pentest chain onto the canvas.
- Deep Intel can design or extend the chain for you from a plain-language objective (see Deep Intel below).
- `Import chain (.kc)` loads a chain you previously exported.

Select a node to open its inspector panel on the right. In the inspector you can edit the `Title`, choose the `Source` (`Pentest tool`, `Data API`, or `AI chooses`), pick the specific tool or API, write a `Prompt / task`, set the `Primary input`, and manage `Inputs from` other nodes using `Add input`. Router nodes expose branch rules (`Branches (first match wins)`, `Add branch`, `Else branch`); merge nodes expose `Merge strategy` and `Wait for`. To connect nodes manually, use `Add input` (or the on-canvas connect prompt `Click a node to feed its output into the selected node.`) and select the source node; `Cancel` aborts the connection.

The canvas layout switch in the top-right offers `Chain view` (the graph), `Scenario library` (prebuilt chains), `Split view` (graph plus library side by side), and `Exports` (generated reports).

#### Hypotheses And Suggestions

The AI proposes attack hypotheses in the `AI hypotheses` panel at the bottom left. A `Suggesting…` indicator appears while new hypotheses are being generated. Each card shows a phase/goal tag, a confidence percentage, the hypothesis statement, an optional rationale, and the tests it would run.

Hypotheses must be accepted explicitly before anything runs:

- `Test it` (shown as `+ Test it`) accepts a proposed hypothesis and stages its test nodes on the canvas. Derived proposals show `+ Extend it` instead.
- `Try` (shown as `+ Try`) stages an open hypothesis that has no nodes yet.
- `Dismiss` removes a proposal or open hypothesis from the panel.
- A staged hypothesis shows `Staged — press Run chain to test it.` — staging alone does not execute it.

Click any card to focus it on the graph and open its detail popover. The popover shows the status, statement, rationale, the verdict (`Why it succeeded`, `Why it failed`, `Inconclusive — re-run to retry`, or `Verdict`), and the `Confidence` percentage, with a `Delete` action.

When a hypothesis is ruled out, its card shows `Closed — ruled out by the AI; results kept for the report.` When the objective is met, the panel shows `Objective met — no further steps required.` With no hypotheses and no target yet, the panel shows `No hypotheses yet — set a target and run the chain, or open Deep Intel to form some.`

Hypotheses move through these statuses:

| Status | Meaning |
| --- | --- |
| `Open` | Proposed or staged; not yet tested. |
| `Testing` | Its test nodes are running. |
| `Confirmed` | Its tests supported the hypothesis. |
| `Refuted` | Its tests ruled it out (kept for the report). |
| `Inconclusive` | No verdict reached; re-run to retry. |

#### Running The Chain

1. Set a valid target and stage at least the nodes you want to test.
2. Press `Run chain`. The engine executes any steps that are not already complete; completed steps are kept and are not re-run.
3. While a chain is running, the `Run chain` button is replaced by `Stop`, and `Reset chain` is disabled. A spinner and the current step label appear on the canvas; the running node pulses.
4. Press `Stop` to halt the run. Running nodes return to the `ready` state and the paused chain is saved.

To re-run work, open a finished or failed node and press `Re-run`, or use `More > Re-arm all nodes` to clear run state across the chain so the next `Run chain` executes everything again.

Nodes report these statuses on the graph:

| Status | Node label text |
| --- | --- |
| `idle` | (no status line) |
| `ready` | (no status line) |
| `running` | `running…` |
| `done` | `done` or `done · N runs` |
| `failed` | `failed · re-run` |
| `blocked` | `blocked — out of scope` |
| `skipped` | `skipped` |

#### Deep Intel

`Deep Intel` is the assistant side chat inside the Kill Chain view, opened with the `Deep Intel` button at the bottom right. From it you can drive the whole chain in plain language.

1. Click `Deep Intel` to open the panel. Use the expand/collapse control to switch between the compact and full-width panel.
2. Type into the field (placeholder `Ask about the chain, or tell me what to do…`). `Enter` sends (`Enter sends.`); `Send` also sends; `Stop` cancels a running response.
3. If no target is set yet, your first message is treated as an objective: Deep Intel locks the target it finds in your text and forms root hypotheses from the objective.

Deep Intel can answer questions about what the chain found or why a hypothesis was refuted, and it accepts control instructions such as forming more hypotheses, running the chain, stopping, re-arming, and writing the report. When an instruction would modify the chain, a confirmation prompt appears before it is applied.

The engagement objective you give Deep Intel is also used to steer the report node so the final report states directly whether the objective was achieved.

#### Reports And Exports

A `Reporting`-phase report node compiles a consolidated penetration-testing report when the chain runs. Open the report node to see its state (`Run the chain to compile the report.`, `Compiling the report…`, or a compiled report) and, when ready, use `Download report (PDF)`.

Generated reports are also listed under the `Exports` layout. The exports list is loaded per chat session; each entry can be opened or deleted. Exports are served from `/api/nexus/exports/kill-chain/<chat_id>`.

You can also export and re-import the chain structure itself:

- `More > Export chain (.kc)` downloads the current chain (scope, nodes, edges) as a `.kc` JSON file, named after the target host.
- `More > Import chain (.kc)` loads a `.kc` file back onto the canvas.

#### Persistence Across Reloads

Each kill chain is stored server-side in a container tied to the active chat session, so it survives reloads and is restored when you return to that chat. Persisted state includes the target scope, the goal and whether it is complete, all nodes and edges, the hypotheses (proposed, accepted, dismissed, and deleted), and panel preferences such as the Deep Intel chat history, whether Deep Intel was open or expanded, and the last target.

On reload:

- a chain that was running is detected and its progress is resumed by polling the server;
- an interrupted hypothesis build is resumed within a short window;
- a Deep Intel control instruction that was still running on the server is resumed.

Deleting the chat session deletes its kill-chain container. `Reset chain` clears everything after a confirmation prompt (`This clears the entire kill chain — target, nodes and results. Continue?`) and removes the stored chain from the server.

#### Limits

- One kill chain exists per chat session; switching chats loads that chat's own chain.
- `Add node`, `Run chain`, and the hypothesis actions are unavailable until a valid target is set.
- The target must be a single domain, IPv4, URL, or CIDR; multi-value or unrecognized input is rejected.
- The Deep Intel chat history kept with the chain is capped at the most recent messages.
- Reset and import are blocked while a chain is running.

## AI Chat and Summary

If AI is enabled, users may also see:

- AI summary generation
- chat over the report content

For chat-style and social-style records, report pages can also include:

- channel or source title
- source URL
- report sharable link
- sender details
- message identifiers
- views, likes, shares, comments, tags, or retweets
- expandable metadata blocks
- JSON inspection

This makes the report page suitable for both analyst review and downstream sharing.

The tested chatbot flow specifically confirms:

- opening the chat widget from a report
- entering a prompt
- sending a message
- rendering a visible message thread in the chat area

## Chatbot and Report Conversation Flow

The report workspace also includes a tested conversational path when the chat widget is enabled.

Covered user-visible behavior includes:

- opening the chat widget from a report
- typing a prompt into the report chat input
- sending the message
- seeing the chat thread render inside the report workspace

```{figure} ../../screenshots/report-chatbot-20260326.png
:alt: Report chatbot widget
:width: 100%

Report-level chatbot workflow used for conversational follow-up on an opened record.
```
