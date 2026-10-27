---
name: "diagram-craft"
description: "Layout, labeling, color and RTL rules for network, architecture, workflow, sequence, data-flow and state diagrams and one-page explainers. Use whenever drawing any diagram."
---

# Diagram craft

Rules for diagrams that read on the first look. Distilled from the authoring and layout references of Archify (tt-a1i/archify, Apache-2.0), rewritten as plain rules with no tooling, plus Arabic/RTL rules of our own. Use together with whatever renders the diagram (inline SVG, an artifact page, a whiteboard, Mermaid); these rules decide what goes where, not how it is drawn.

## 1. Pick the type from the question

| The question | Type | Examples here |
|---|---|---|
| What is it made of, and what connects to what | Architecture | network topology, VLANs behind the firewall, servers and their services |
| What happens, step by step, with branches and approvals | Workflow | sample intake to result, purchase approval, incident runbook |
| Who talks to whom, in what order | Sequence | login against AD, scan-to-email via SMTP relay |
| Where data or documents go and what transforms them | Data flow | report pipeline, backup path |
| What state is it in and how does it move | Lifecycle / state | sample status, ticket status, device provisioning |
| What contains what (single parent) | Tree | folder structure, org chart, module breakdown |
| Tables and their keys | ERD | LIMS or admin-panel schema |
| When things happened | Timeline | outage review, project history |
| Where the time went | Waterfall | slow page load, slow login |

One diagram answers one question. If the request mixes two (topology and a timeline), draw two.

## 2. Composition: meaning before boxes

- **Lead with the main path**: the reader's main journey through the system. Everything else is a branch of it.
- **Preserve every fact the user gave**: each responsibility, relationship direction, protocol, port, and condition. A condition that changes behavior (approval, auth, a firewall rule) sits on the affected node or edge, not only in a side note.
- **Boundaries mean something real**: isolation, ownership, runtime, or persistence (a VLAN, a firewall zone, a building, a cloud tenant). Never draw a boundary around one node just to repeat its name. Keep outside actors outside the boundary's padded box.
- **Group** cooperating parts into an honestly named subsystem only when grouping does not hide who controls what, a trust boundary, or where data is stored.
- **Cards and notes** answer extra questions; they never replace a required node or edge.
- **No quotas**: no target count of nodes, edges or boxes.
- **Title**: one concise title. Name a deliberately narrow scope in it ("Camera network, ground floor"). No subtitle unless asked.
- **Never invent facts**: an unknown IP, port or count is marked as a placeholder, not guessed.

## 3. Placement: decide positions from connections

Classify every relationship before placing anything:

- **Main path**: neighbors adjacent in reading order. A longer path steps down through meaningful rows instead of one thin strip.
- **Branch or store** (a database, NAS, log store): directly above or below the node that owns, reads or writes it, centered on it, so its edge is one straight segment. Keep all branches of one row on the same side.
- **Return** (back to an earlier step): put its source on the side of the main path that has no branches, so it runs through an empty corridor.
- **Second entrance** into a node that already has an incoming edge: bring it from a different side.
- **Fan-out**: a side carrying k edges needs at least `32 + 14 × (k − 1)` px. Spread a hub's edges over two or three sides, or make the hub bigger. Center a parent over its children; align an only child with its parent.
- **Feedback cycles**: lay them out as an open rectangle in their real edge order (top row forward, bottom row back).
- Start the main actor near the top-left of the reading direction, not in the middle of empty canvas. Use rows for rhythm, not blank space.
- Before finalizing, trace each non-main edge: its straight or one-bend corridor must not pass another node or cross another edge. If it does, move the endpoint that is off the main path.

## 4. Routing

- **An edge never crosses an unrelated node.** This one is never acceptable.
- Orthogonal routes; the first and last segment leave and enter the node perpendicular to its side.
- No segment shorter than 8 px; interior segments at least 16 px (no tiny doglegs).
- Two unrelated edges never share the same line for 8 px or more. Related edges may share a corridor on purpose, as a bus.
- An edge does not run along a container border for a long stretch.
- A detour longer than about 2.5× the shortest legal route means the placement is wrong: move the nodes, do not add bends.
- Do not trade readability for fewer crossings (no huge blank canvas, no tiny text).

## 5. Labels are data

- An edge label carries the action, protocol, port, direction, or sync/async meaning. Omit it only when both endpoints already make it obvious and it carries none of those.
- **Budget space**: the clear gap between two boxes must exceed the label width + 8 px. Latin label width ≈ `6.5 px × characters + 13 px`. For Arabic, measure the rendered text (Arabic runs wider and taller than this formula; reserve about 1.3× and a taller line).
- Spacing means clear gap, not center distance: two 165 px boxes 200 px apart leave only 35 px.
- When a label collides, repair in this order: (1) move it along its own route, (2) widen the gap or reroute, (3) shorten the wording while keeping its meaning. Deleting a meaningful label is never a fix. A label stays beside its own edge, never parked in distant empty space.
- A node sublabel holds the role or protocol in a few words; longer facts go to a note or card.

## 6. Color and legend

- **Every saturated color means something** (a node kind, a zone, a flow type). Never add a color to make it lively. Keep one fixed mapping across all diagrams of a project.
- Neutrals carry structure: borders, containers, text. Brand colors may tint containers or the title, but must not collide with the semantic mapping.
- Never rely on color alone: async = dashed, security/policy = a distinct stroke plus an icon or label.
- The legend lists only the kinds actually present. It never stands in for a missing node or edge.
- Flat at rest: borders and tone, no decorative shadows or glow; emphasis only for focus or the main path.
- Design both light and dark so the same color still means the same thing.

## 7. Readability

- Text a reader must parse stays at body size. Tiny labels are only metadata (an ID, a port).
- At normal desktop width, the smallest diagram text should render around 7.5 px or more in viewBox terms after scaling; below 6 px is a failure.
- Keep the whole diagram readable without horizontal scrolling on a laptop; grow downward in meaningful rows instead.
- Keep one term per thing across the title, labels, legend and any text beside the diagram.

## 8. Arabic and right-to-left

- For an Arabic page, the main path reads right to left: start at the top right and flow leftward, then down. Mirror branch/return conventions accordingly.
- Technical identifiers stay left-to-right inside Arabic text: IPs, VLAN IDs, ports, hostnames, model numbers, protocols. Wrap each in its own isolated LTR span (`dir="ltr"` with `unicode-bidi: isolate`, or `<bdi>`), so `192.0.2.14`, `VLAN 100` and `HTTPS 443` never flip.
- In SVG, set `direction` and `text-anchor` per label: Arabic labels anchor `end` at the right edge of their box; LTR identifiers keep `start`.
- Never start an Arabic label with a Latin word if avoidable; put the Arabic noun first ("خادم `AD`").
- Use an Arabic-capable font (for example IBM Plex Sans Arabic) with a real fallback stack, and measure text after the font loads.
- Arrowheads follow the real direction of the relationship, not the reading direction.

## 9. One-page explainer (when the answer is a page)

- The first line is the conclusion. Then at most four short reasons.
- Each panel answers one question; each sentence makes one claim. Do not restate what a table or diagram already shows; add its reason or exception.
- Choose the form by the shape of the information: messages in order → sequence; process with branches → workflow; parts and connections → architecture; hierarchy → tree; events over time → timeline; options against criteria → comparison table with good/caution/bad cells; ordered actions → numbered steps; a warning → callout; a number against a limit → one metric.
- Steps that walk through a diagram sit beside it, each titled with the label of the node it points at.
- Give numbers with units and their source; recompute every derived number and show the arithmetic once; mark estimates as estimates.
- Phrase actions as imperatives with the trigger first ("If the switch reports a loop, disable port 12 first").
- When the reader must act, end with a checklist, one concrete check per line.
- A small answer gets a small page: conclusion plus one or two panels.

## 10. Check before handing over

Render it and look once, then repair in this order:

1. Nodes overlapping or outside the frame.
2. Edges through nodes; wrong entry/exit direction.
3. Crossings, ambiguous shared corridors, long border runs, detours, tiny doglegs.
4. Label vs node, label vs label, label vs edge clearance.
5. Labels leaving the frame.

Trace the main path, then each branch, edge by edge. If several routes are tangled, re-place the connected group in one edit instead of nudging one edge at a time. Stop after two repair rounds; if a defect remains, say exactly what it is rather than claiming the diagram is clean.