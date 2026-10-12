---
name: design-with-nomoreda
description: Create, inspect or resume NOMOREDA electronic designs, from project creation and component selection through schematics, PCB placement, routing and native verification. Use the connected project's automatic or staged-review workflow.
---

Use NOMOREDA's connected MCP tools as the design authority. On app-wide/account
connections, call `list_projects` to identify the intended design and include its
`projectId` on every project tool call; there is no shared active project.
Project-only credentials and endpoints are retired. Reconnect through account
`/mcp` with explicit consent; never silently expand an existing restricted grant.
Read `get_agent_context` and `get_engineering_workflow` before design writes. Reuse
the existing connection and scope. For a new connection use the
host's NOMOREDA Connect flow at the app-wide `/mcp` endpoint; preserve existing
access scope and use normal sign-in/consent controls.

## Load only current guidance

Call `get_design_knowledge` without arguments through authenticated MCP. It needs
no project selection. Retain the returned revision and index hashes. Check its
`retrieval` field for supported task presets, sections and retrieval inputs;
use exact returned IDs. An installed version does not establish server support.
Before declaring a capability unavailable or handing research back to the user,
discover the relevant tools on the existing connection and read the matching task
guidance. Component research uses the component-selection guidance, including
when a missing part interrupts schematic or PCB work.

When retrieval is advertised, request the current task with
`{task: "pcb-routing", expectedRevision}` or selected sections with
`{topics: [returnedId], expectedRevision}`. Use the task matching the actual phase;
stackup, placement and routing have separate presets. Presets include their
required shared guidance and dependencies. For other procedures, select relevant
sections from the index and follow their applicable prerequisites. Do not load
whole parent topics merely because a link names that phase. A task selects
reading only; it does not perform operations or authorize work.

Follow the bundle's top-level `nextOffset` until null, retaining the same selector,
`expectedRevision`, `loadedTopics` and `limit` across pages. For every offset above
zero also pass `expectedSelection` with the preceding bundle's `selectionHash`;
changing the selection or loaded cache mid-read is refused. Read every returned
`pages[].text`; page-level offsets describe fragments within their topic. Only a
completed bundle establishes that its `requiredTopics` content has been read.
If retrieval is absent, use compatible single `topic` calls: read `entry`,
`overview`, `workflow`, `workflow-modes`, `mcp-execution` and the phase topics
needed now, passing `expectedRevision` and following each `nextOffset` to null.

## Retain content and refresh changes

Keep two separate records: index hashes for change detection, and fully read
content still present in context with its `{id, sha256}`. Only the latter goes
in `loadedTopics`; an index row, remembered hash or compact summary is not loaded
content. A matching `unchangedTopics` entry reuses that retained content. Never
reuse it when content was lost during resumption or compaction; omit its loaded
hash and read it again. Keep the selected IDs, revision, selection hash, unchanged
loaded-topic list and next offset for an interrupted read, but do not mark partial
topics loaded. If that state or retained content is lost, restart at offset zero.

Authenticated MCP tool replies carry a compact `nomoredaKnowledge.revision`
marker, including errors. On a change, refresh the index with the previous index
hashes as `knownTopics` to identify changed/new and removed topics. `knownTopics`
is index comparison only; it never proves content was read. Reload the current
task or affected sections, using `loadedTopics` only for fully retained content.
Read changed shared guidance and relevant dependencies before the next dependent
action; reassess pending plans. If a page reports a revision mismatch, refresh
the index and restart the selection at offset zero. Never mix revisions.

Preserve the project, user choices, saved evidence and exact retries after
uncertain writes. A guidance update does not authorize a new task, change a mode
or prove earlier work wrong. Updates appear on a subsequent tool response, not
during thinking or an operation already running. Check the index when resuming
or changing phases. Routine knowledge edits need no reinstall; tool/schema changes
need supported discovery. The 0.8.0 skill enables efficient retrieval; an older
server uses the single-topic fallback. Local stdio bridge users must upgrade the
plugin to 0.8.1 and restart the connection for the expanded tool catalog and
mandatory revision markers on near-limit replies. Older bridges can fail during
discovery; a skill refresh alone does not replace the bridge. Hosted HTTP
connections need no bridge update.

Knowledge is available only through authenticated MCP, never a downloaded
reference or public HTTP fallback. If `get_design_knowledge` is missing, refresh
through the host's supported discovery and report if it remains unavailable.
Do not invent a URL or assume a skill installation upgraded the server.

Honor explicit auto/no-intervention or review intent; otherwise reuse the saved
user choice and ask once if none exists. Inspection alone needs no mode choice.
Neither mode bypasses access, native preview/Apply, saved-version checks or
technical constraints. Preserve human changes and report concrete unsupported
operations. ERC/DRC and native export do not establish fabrication or operating
qualification. Delegation is optional: use host-supported subagents when
independent work would improve speed or reliability, not as a mandatory step.
Reuse the existing MCP connection and project scope; delegation requires no
additional connection setup. If the host cannot share it, keep MCP calls with
the coordinator and delegate analysis of supplied data.
Follow the authenticated Essentials guidance for coordination and final validation.
Keep a compact working record of mode, requirements, parts/sources, project
revision, saved version/job IDs, guidance state and blockers.
