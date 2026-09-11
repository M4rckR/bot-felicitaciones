# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single self-contained HTML/CSS/JS snippet implementing **"Tarjetín"**, a deterministic decision-tree
chatbot. It runs today as experiment **TC0080** on the BCP home `https://www.mitarjetabcp.viabcp.com/`,
delivered as an **Adobe Target HTML offer**. There is no build step, package manager, test suite, or git
repo — the file is pasted whole into the experimentation tool and ships as one blob.

**The active goal of this repo is to port the bot to `/felicitaciones` with different content.** The
existing bot is the working reference; the `referencia/paginas/` snapshots describe the target page. See
*Porting to /felicitaciones* below.

## Files

| File | Role |
| --- | --- |
| `adobe-target/piloto/bot.html` | **The file being built — this is what you edit.** The new `/felicitaciones` bot. Started as a byte-copy of `referencia/bot-actual.html`'s engine with the namespace swapped (a `tcxxxx` placeholder until 2026-09-11, `tc0091` since) and the home node tree replaced. See *State of the offer* below. |
| `adobe-target/control/control.html` | **The control variant of the experiment.** Not a bot: a standalone `<script>` that pushes the single `- C` event and nothing else. The control group gets a page with no bot, so none of the pilot's seven events can fire there. See *Analytics*. |
| `netlify.toml` | Deploy config for the preview. Builds a `publicado/` allow-list containing **only** `preview/` and the pilot offer. **`referencia/paginas/` is deliberately excluded: those snapshots carry real client names and credit lines.** |
| `README.md` | Human-facing entry point: how to run the preview locally and on Netlify, what the directory holds, pilot vs control. |
| `referencia/bot-actual.html` | **Read-only reference.** The live home snippet as authored: `<style>` (1–991), markup (993–1056), `<script>` IIFE (1058–4141). Do not edit — it is the working original we ported from. |
| `referencia/bot-insertado.html` | Read-only snapshot of the **live home page in production with the bot inserted**. Shows the real injection context. Its `<style>` (5331–6321) and `<script>` (6397–9480) are byte-identical to `referencia/bot-actual.html` — only indentation and live-DOM attributes differ. Do not edit; it is evidence, not a build output. |
| `preview/` | **Editable.** Dev-only, never shipped: `preview/index.html` (the harness) and `fonts/` (Flexo webfonts). The harness must stay a faithful mirror of the offer — that is its whole job. |
| `contenido/wordings/` | **Dev-only mirror of all the bot's copy, split one file per flow.** Read this instead of the HTML when the task is wording. Never loaded by the snippet — see *Working on copy* below. |
| `referencia/pasos/` | Screen-by-screen mockups Marco supplies as the source of truth for content, e.g. `primera-pantalla.png`. |
| `contenido/afinidad-tarjetas.json` | Marco's "Relevancia - Afinidad" table: 17 cards scored 1–5 against the four `q1` criteria. Drives which cards the bot recommends. Its `_meta.pendiente_confirmar` lists what is still undecided — read it before building recommendation logic. **Read-only in spirit**: it is the record of what Marco sent, so new fields go in `contenido/tarjetas-catalogo.json` instead. |
| `contenido/tarjetas-catalogo.json` | **The card crosswalk.** 17 cards × DOM code, name per source, affinity, membership, exoneration, miles, Priority Pass — plus the DOM selectors that reveal which cards a user is a lead for, the "3 + Visa Oro" rule, and the per-criterion casuistry filled in as Marco delivers each perfilador screen. Dev-only: the snippet never reads it. See *Lead detection* below. |
| `referencia/paginas/*.html` | Read-only DOM snapshots of the **target page** `/felicitaciones`. `certi` = staging Adobe Launch (`launch-b540b12ff8c9-staging`), otherwise production (`launch-e838ddbd0060`). `con-exp`/`sin-exp` differ **only in tracking-tag ordering and beacon ids** — the visible text is byte-identical, so don't spend time diffing them. |

All four snapshots are 0.5–1.2 MB. **Grep them; never read them whole.**

**Directory layout** (reorganised 2026-09-11, Marco's request, so pilot and control read as two separate
deliverables):

```
adobe-target/      what gets pasted into Target — the only thing that ships
  piloto/bot.html
  control/control.html
preview/           the harness. The only thing Netlify publishes
contenido/         working material the snippet NEVER reads: wordings/ + the two card JSONs
docs/              correcciones-wording.md, the UI-team deliverable
referencia/        read-only evidence: bot-actual, bot-insertado, paginas/, pasos/
```

> ⚠️ **`referencia/paginas/` must never be deployed.** The four snapshots carry real client data —
> `data-client-name` (`YESY MARIBEL GARCIA`, `OSCAR CARLOS IBANEZ`) and `data-credit-line`. `netlify.toml`
> publishes an allow-list, not the repo root, precisely so nobody has to remember to exclude them. Do not
> add them to that copy for any reason. This is also why the harness fabricates its own
> `<xt21-card-option>` markup instead of loading a snapshot.

**What you may edit.** Only the references are off limits: `referencia/paginas/*.html`, `referencia/bot-actual.html` and
`referencia/bot-insertado.html`. They are evidence, and altering them destroys what they exist to preserve — read and
grep only. The offer (`adobe-target/piloto/bot.html`), the preview (`preview/index.html`), `contenido/wordings/`,
`contenido/tarjetas-catalogo.json` and this file are the working set and are edited normally.

**Marco dictates every change.** Do not proceed, extend the scope, or fill in pending screens unless he
says so directly. Deliver exactly what was asked and stop there.

## Commands

There is no build toolchain and no package manager. Visual validation runs through the harness:

```bash
python3 -m http.server 8000 --bind 127.0.0.1   # desde la raiz del proyecto
# abrir http://localhost:8000/preview/
```

The harness is also deployed to Netlify so it can be opened from a phone — `netlify.toml` has no framework
and no install step, just a copy into an allow-listed `publicado/`. To check what a deploy would expose,
run that same copy locally:

```bash
rm -rf publicado && mkdir -p publicado/adobe-target/piloto &&
  cp -R preview publicado/preview &&
  cp adobe-target/piloto/bot.html publicado/adobe-target/piloto/bot.html &&
  grep -rl 'data-client-name' publicado/ || echo "sin datos de cliente"
```

`preview/index.html` is the dev harness for `adobe-target/piloto/bot.html`. It **fetches the snippet at runtime** — it
never copies its content, so the two cannot drift. It gives: live re-injection when the file changes
(1 s poll), Desktop/Móvil-390px toggle via an iframe (so the bot's `matchMedia("(max-width: 768px)")`
reacts to real width), a `digitalData` panel showing every analytics event the bot pushes, and a console
panel where `validateGraph` errors surface. A `file://` open is detected and refused with instructions,
because the fetch would be CORS-blocked.

**The header takes no height** (Marco, 2026-09-11). The harness exists to see the bot as it will really
look, and a permanently visible bar steals screen — most of all on mobile, where the bot is full-screen. So
the header is `position: fixed`, `translateY(-100%)` by default, and `main` is a full `100dvh`. It comes
down on any of four triggers, which deliberately coexist: hovering the top 12px (`#hotzone`, which must
stay the header's *previous sibling* — the CSS joins them with `+`), hovering the header itself,
`:focus-within` so keyboard use works, and the `.fijo` class set by the `≡` handle or the **H** key.

Two collisions were found and fixed while building it; both come back if the pieces move:

- The handle sits at `z-index: 31`, above the header's 30, so it covered the first button. The header
  carries `padding-left: 52px` to leave it room.
- `#hotzone` is **disabled under 900px**. There is no hover to serve there, and a 12px strip across the top
  would eat taps on the bot's own close button, since in that view the bot fills the screen.

The `≡` handle is the only chrome always on screen, at 55% opacity, **top-left** because the bot's launcher
lives bottom-right. Its dot flashes on re-injection, so a reload is visible without opening the header to
read `#status`.

**The side panel folds on desktop too**, through the same `☰ panel` button (`main.sin-panel`), because
otherwise the bot never gets the full width. Under 900px that same button slides it over the content
instead. Crossing the breakpoint resets both states — without that the backdrop stays stuck over the
content, or the panel vanishes with no visible way back.

**The harness is responsive and usable on a phone** (Marco, 2026-09-11). Three things make it work, and
each is load-bearing:

- **`<meta name="viewport">` in the harness `<head>`.** Without it a real phone renders the preview at
  980px and scales it down, so the media queries never fire. There is a second viewport tag further down —
  that one belongs to the *iframe* document and is not the same thing.
- **The breakpoint is 900px**, the width below which the 380px side panel stops fitting next to an iframe
  that still needs the bot's 390px. Under it: one column, the width toggle hides (on a real phone the
  width is already real), and the panel becomes off-canvas — `transform: translateX(100%)`, opened by the
  `☰ panel` button, closed by the backdrop or Escape. Leaving that breakpoint on a wide screen resets the
  panel, otherwise the backdrop stays stuck over the content.
- **`height: 100dvh`, not `100vh`**, on `body` and `main`: the retracting address bar cuts the layout
  with `100vh`.

**The iframe stays, the frame around it is gone** (Marco, 2026-09-11). The `#stage` wrapper — checkerboard
background, drop shadow, rounded corners, 16px padding — was removed and the iframe is now a direct grid
child of `<main>`, flush. **Do not remove the iframe itself**: it is what gives the bot a width of its own,
so `matchMedia("(max-width: 768px)")` reacts to the iframe rather than the window, and it is what keeps the
harness CSS from reaching the bot. Centring in 390px mode now comes from `justify-self: center` on the
iframe, which used to come from the wrapper's `place-items: center`.

**There is no chat/slides toggle any more** (Marco, 2026-09-10). The harness used to expose one, wired to
`window.tc*SetMode`. The bot ships in chat mode and slides is not a state any real user can reach, so as a
preview lever it only invited confusion. The `SetMode` hook is gone from the snippet too. Do not re-add the button.

It deliberately renders on a **neutral background with only the design tokens `/felicitaciones` actually
defines** — the real reference pages are never loaded, since that would fire live BCP/Adobe/Meta tracking
beacons.

`preview/fonts/` holds the real Flexo webfonts and is wired **only into the harness** — never into the
snippet, which must inherit its typeface from the host page. The bot uses one family
(`Flexo-Regular`) at weights 600 and 700, so the harness maps Demi→600 and Bold→700 to avoid synthetic
bolds. Verify they load by checking for `200` on the `.woff2` requests in the server log; a silent fallback
to sans-serif looks plausible but is not faithful.

A single snippet can also be opened directly (`open referencia/bot-actual.html`) — all icons are inline SVG and there
are no external assets — but that gives no host-page tokens and no analytics panel.

Runtime hooks (`tc0080` in `referencia/bot-actual.html`, `tc0091` in `adobe-target/piloto/bot.html`):
- `window.tc0080SetMode("slides" | "chat")` — switches render mode and restarts the flow. **`referencia/bot-actual.html`
  only.** It was inherited by the port, never built for it, and on 2026-09-10 the preview's toggle, the hook
  and the whole `slides` render path were removed from `adobe-target/piloto/bot.html` — see *Dead code removed*. Do not
  re-add either the hook or the preview button.
- `window.tc0091Catalogo()` — **`adobe-target/piloto/bot.html` only.** Returns the perfilador criteria, every card the
  bot declares (`{codigo, nombre, criterio, caso}`) and the codes currently detected in the page. Feeds the
  preview's casuistics panel; the bot never calls it.
- Graph problems are logged at init as `console.error("Destino inexistente:", ...)`.
- Runtime errors go through `console.debug("[tc0080][error:TYPE]", payload)` (see `debugError`).

**There is no Chrome on this machine — only Brave and Edge** (Marco, 2026-09-10), so the `claude-in-chrome`
browser tools cannot drive this preview. Do not trust them here: in the 2026-09-10 session they served a
page that looked exactly like the harness but was a **stale copy of `adobe-target/piloto/bot.html` from the previous
day**, with none of the edits just made, while the file on disk and the local server were both correct.
The tell was that the local server logged **zero** requests from that browser. Cost about fifteen tool
calls chasing a phantom cache bug. To see a change rendered, start the server and open it in Brave —
`Start-Process` on `C:\Program Files\BraveSoftware\Brave-Browser\Application\brave.exe` with the URL — and
then **ask Marco what he sees, or verify statically and say plainly that the visual check was not
possible**. Never report a rendered observation taken from those tools.

The harness also reinjects the whole iframe document on every file change, so a screenshot taken
mid-reinjection can show unstyled markup — a capture race, not a broken snippet. And note the harness
auto-reloads only `adobe-target/piloto/bot.html`: after editing `preview/index.html` itself, the page needs a manual F5.

## Working on copy

`contenido/wordings/` mirrors every user-visible string in `adobe-target/piloto/bot.html`, one JSON per flow, plus `ui-fija.json`
for the copy that lives in the markup and in hardcoded engine strings rather than in `config.nodes`.

**The snippet never reads these files** — not in production, not in the local preview. Do not add a
`fetch`, an `import`, or any other way of loading them: `adobe-target/piloto/bot.html` must stay self-contained because
it ships as one blob pasted into Adobe Target. They exist purely so a wording task doesn't have to drag
four 17-row tables through context to change one word.

Workflow for a copy change — **all three steps, every time**:

1. Read `contenido/wordings/_index.json` to find which file owns the screen. Open only that file.
2. Edit the JSON, **then apply the same change to `adobe-target/piloto/bot.html`** — `contenido/wordings/README.md` documents how
   each field maps back (tables → `| a | b |` rows, blank string = spacer, emoji → HTML entity).
3. **Open the change in `preview/index.html`, walk to that screen, and look at it.** Report what was
   actually on screen, not what the diff says should be there.

**A change that only exists in `contenido/wordings/*.json` is not a change.** The JSON is inert — nothing loads it,
so editing it alone leaves the bot byte-identical. Never stop at step 2, and never report a wording task as
done without having seen it rendered. This is the one failure mode the mirror introduces; the rest of the
repo has no way to catch it, since there is no build, no test suite and no sync script.

If the copy touched is in `ui-fija.json`, step 2 means editing markup or engine strings, not
`config.nodes` — and some of it only renders in a specific state (the error bubble needs a pending
transition in `sessionStorage`; `"Pensando"` never renders at all while `config.thinking.showText` is
`false`). Say so plainly instead of claiming a visual check that wasn't possible.

`adobe-target/piloto/bot.html` is the source of truth; the JSON is a mirror and there is no script keeping them in sync.
If you edit the HTML directly, update the JSON in the same pass. The `lineas` fields are jump hints that go
stale as soon as nodes are added — confirm with `grep -n 'id: "q22"' adobe-target/piloto/bot.html` before editing.

## Architecture of the snippet

The sections below describe the shared engine. Every selector, class, and id is namespaced and scoped
under `#<ns>-scope` — `tc0080` in `referencia/bot-actual.html`, `tc0091` in `adobe-target/piloto/bot.html`.

### Bootstrap (bottom of the script)

`ensureScope()` creates `#tc0080-scope` and moves the mask, welcome bubble, launcher, and panel into it.
`waitForBotAndInit()` polls every 150 ms for up to 8 s until those four elements exist, then constructs one
`Bot`. A `MutationObserver` on `document.body` re-runs the bootstrap if the scope disappears — the host is
an Angular SPA that can wipe injected DOM. Re-init calls `destroy()` on the previous instance, which
unlocks page scroll and clones-and-replaces bound elements to drop listeners.

### The `Bot` object

ES5 constructor + `Bot.prototype.*`, `var` declarations (a few arrow functions and `Array.find` appear in
`validateGraph`/`getNode`). `this.config` holds the whole conversation graph inline; `this.state` holds the
runtime. Match this style when editing.

**Conversation graph** — `config.nodes` is a flat array keyed by `id`. Ids encode tree depth:
`q0` (root menu) → `q1` → `q11` → `q111`. In **`referencia/bot-actual.html`** it holds ~44 nodes, where `mixedNN` are
the "what now?" menus after a leaf answer and `rating` / `rating_number` / `rating_thanks_auto` /
`mixed100` are the closing sequence. **`adobe-target/piloto/bot.html` has 15 nodes and none of those**: its closing chain
was deleted on 2026-09-09 and the machinery behind it on 2026-09-10.

Node fields: `id`, `type`, `title`, `text`, `className` (extra bubble classes), `next`, `options`,
`actions`, `feedbackText`, `richText`, `usageMenuOption`.

Node types:
- `question` — renders `options` as buttons; waits for the user.
- `auto` — renders, then auto-advances to `next` after the thinking delay (`maybeAdvanceAutoNode`).
- `mixed` — leaf menu mixing navigation options, external CTA links, and a close button.
  **`referencia/bot-actual.html` only** — removed from `adobe-target/piloto/bot.html` on 2026-09-10.
- `rating` — renders the 1–5 star widget instead of options.
  **`referencia/bot-actual.html` only** — removed from `adobe-target/piloto/bot.html` on 2026-09-10.

In `adobe-target/piloto/bot.html` every one of the 15 nodes is `type: "question"`, and the engine only understands
`question` plus `auto`. See *Dead code removed* below.

Option fields: `label`, plus `next`, or `href`+`target`+`isCta: true` for an external CTA, or
`isClose: true` for the terminal close button. `actions` (thumbs up/down via `iconSlot: "up"|"down"`)
render the `feedbackText` row on response nodes. `usageMenuOption` emits a *second* bubble linked to the
question bubble, so both stay clickable at once.

**Render model** — `render()` rebuilds `.tc0080-body` with `innerHTML` on every transition, then
`attachHandlers()` re-binds everything. All interaction flows through `data-*` attributes on the generated
markup (`data-next`, `data-idx`, `data-rating`, `data-response-idx` + `data-response-message-id`,
`data-cta-discover`, `data-close`, `data-back`, `data-restart`, `data-main-restart`, `data-retry-error`,
`data-runtime-error-action`). A new interactive control means emitting a `data-` attribute and handling it
in `attachHandlers`. In `adobe-target/piloto/bot.html` `data-rating`, `data-back` and `data-restart` are gone, and
`data-carousel*` was added.

**Two modes** (`config.mode`, default `"chat"`) — **`referencia/bot-actual.html` only**. `adobe-target/piloto/bot.html` has one
render path and no `config.mode` at all; see *Dead code removed* below.
- `chat` — append-only transcript. `state.chatLog` accumulates `{role: "user"|"bot", ...}`; each bot entry
  is a *snapshot* of the node taken in `appendBotMessageForCurrent`, so later config reads never
  retroactively rewrite history. `renderChat` walks the whole log and keeps only the last bot message
  (plus any linked usage bubble) interactive. `back()` restores from `state.chatSnapshots`.
- `slides` — `renderSlides` shows only the current node; `back()` pops `state.history`.

The `chatLog`-as-snapshot behaviour **does** still hold in `adobe-target/piloto/bot.html` and matters more than ever —
it is what freezes the perfilador's resolved cards into history. What is gone there is the *undo*
machinery (`back()`, `state.chatSnapshots`), not the per-message snapshot.

**Transitions** — `goTo(id, label)` → optional `startWaitingTransition` (1200 ms "thinking" bubble, see
`config.thinking`) → `applyTransition(id)`. Transitions are ignored while waiting unless
`allowWhileWaiting` is passed.

**Rich text** — `richText: true` runs `text` through `formatRichText`: `**bold**`, `- ` bullets, blank
lines as spacers, and — in `adobe-target/piloto/bot.html` only — `| pipe | tables |`. Long copy is written as a string
array joined with `"\n"`. Emojis are HTML entities
(`&#128179;`) and `escapeHtml` deliberately preserves existing entities while escaping everything else — so
node text and labels are trusted content. Do not route user-influenced strings through it.
`getUserBubbleLabel` strips leading emoji from an option label before echoing it as the user bubble.

### Analytics

Events push onto `window.digitalData` (created as an array if absent) via `pushPromotionEvent`, with
`event: "trackPromotionView" | "trackPromotionClick"` and `promotion: {name, creative, position}`.

**In `referencia/bot-actual.html`** names follow `Home - Cards - Bot - <section> - <experimentCode> - P` with
`experimentCode: "TC0080"`. Tracked: bot open, final-response view, feedback click
(`position: "<nodeId> - <1|0>"`), discover-card CTA view/click, rating view/click, close view/click.

**In `adobe-target/piloto/bot.html` the tagging plan is Marco's spec of 2026-09-11 and it is closed**: exactly
these **six** events, no more. Anything else that existed was deleted — do not re-add an event because the
home bot has it.

| Method | name (after `Felicitaciones - Cards - Bot - `) | creative | position | Fires when |
| --- | --- | --- | --- | --- |
| `trackLauncherView` | `Inicio - TC0091 - P` | Button | `Inicio Tarjetin` | `bind()` — launcher painted. Once per page via `Bot.launcherViewSent` |
| `trackOpenBotClick` | `Inicio - TC0091 - P` | Button | `Modal` | panel opened |
| `trackFinalResponseView` | `Arbol - TC0091 - P` | Modal | `1 - 1.1` … `3 - 3.3` | every **respuesta** node |
| `trackElegirTarjetaView` | `Arbol - TC - TC0091 - P` | Button | `Elegir Tarjeta` | once per message carrying `cards` or `recommendation.cta` |
| `trackCloseView` | `TC0091 - P` | Button | `Cerrar` | node has an `isClose` option |
| `trackCloseClick` | `TC0091 - P` | Button | `Cerrar` | `[data-close]` clicked |

**The bot does not tag the "Elegir tarjeta" click** (Marco, 2026-09-11). Pressing the page's own button
makes the page emit three events by itself, and those are the base — a seventh event of ours would count
the same click twice:

```json
{ "event": "trackAction", "action": { "category": "Opciones Tarjeta", "group": "Cards",
                                      "label": "Quiero esta Tarjeta", "name": "Click" } }
{ "event": "trackMetadataList", "metadataList": [ { "key": "TarjetaSeleccionada", "value": "…" },
                                                  { "key": "Flujo", "value": "Flujo Normal" } ] }
{ "event": "trackPopup", "popup": { "name": "Cards - Edita tu linea de credito" } }
```

The **View** stays, because only the bot knows it offered the card. What is lost is attribution on the
click: none of the three says it came from the bot — `Flujo` reports `"Flujo Normal"` either way. Pairing a
click with the bot means correlating it with the `Arbol - TC` View that precedes it. See *Still open*.

Three traps worth keeping:

- **The close name has no section segment.** It goes from `Bot` straight to the code — the only one of the
  four families without one. That is how Marco specified it; do not "fix" it by adding `Cierre`.
- **`isRespuestaNode` decides what counts as a respuesta**: `id.length > 2`, which excludes the four menus
  (`q0`–`q3`) and includes the eleven answer screens. The old gate was `node.feedbackText`, which no node
  has, so **the whole Arbol funnel was silently dead** until 2026-09-11.
- **`position` uses the tree's own numbering, not the internal id** (Marco, 2026-09-11):
  `getPositionArbol` turns `q11` into `"1 - 1.1"` and `q33` into `"3 - 3.3"` — the same numbering the
  screens are delivered with and that the mockups and `docs/correcciones-wording.md` use. The `qNN` id
  never leaves the snippet. It derives from the digits, so a third level would read `1.1.1` on its own.
- **`Elegir Tarjeta` is one View per screen, not per button.** A perfilador screen shows up to 4 CTAs and
  the `position` is a fixed literal, so four identical events would only inflate the count. It reads the
  *resolved* message, so a hidden recommendation emits no View either.

**Deleted on 2026-09-11, per Marco:** the `Arbol - Utilidad` feedback event (`trackFeedbackClick`,
`getFeedbackScore`), and `trackDiscoverCardView/Click` + `isDiscoverCardCtaOption`, which keyed off
`option.isCta` — a shape no node in this bot uses.

**And then the whole feedback/response machinery went with it** (Marco: "bórralo"), **15.3 KB**, taking
`adobe-target/piloto/bot.html` from 233.7 KB to 218.4 KB:

| Where | What |
| --- | --- |
| CSS | the 21 `.tc0091-response-*` rules, `.tc0091-chat-item--response`, `.tc0091-response-content` |
| Render | the `if (responseActions.length)` block in `renderBotItem` — label, divider, footer, both thumb SVGs in selected and unselected variants — and the `nodeType === "response"` branch |
| Handlers | the whole `[data-response-idx]` block |
| State | `responseActions` and `selectedResponseActionIdx` on both chatLog builders, `feedbackText` |
| Markup | `data-feedback-node`, `data-feedback-anchor` |
| Scroll | `scrollToLatestFeedbackStart` plus the `currentIsFeedback` / `followsFeedbackNode` branch in `adjustChatScroll`, which now always falls to `scrollBodyToBottom` |

Nothing was reachable: no node in this bot is `type: "response"` or carries `feedbackText`/`actions`. The
two post-removal checks were re-run and both come back clean — **zero orphan prototype methods** (out of
93) and **zero unused `.tc0091-*` classes**. If the thumbs ever come back, they come back from
`referencia/bot-actual.html`.

**`adobe-target/control/control.html` is the other half of the experiment.** The control group gets a page with no bot, so
none of the seven can fire there. That file is a standalone `<script>` that pushes the single `- C` event
(`Inicio - TC0091 - C`, Button, `Inicio Tarjetin`) and nothing else: no markup, no styles, no DOM writes,
guarded by `window.__tc0091ControlViewSent` against Target re-injection. It pushes immediately rather than
waiting for the page to create `digitalData` — a late control View unbalances the comparison against a
pilot that pushes at once.

This is the same `digitalData` queue the rest of the page uses — e.g. `referencia/bot-insertado.html` shows a sibling
experiment **TC0037** (floating "Pide tu Tarjeta de Crédito BCP" button, `.m-button-fixed`) pushing
`Home - Cards - TC0037 - P`.

### Error handling and recovery

- `validateGraph()` at init logs dangling `next` targets and `auto` nodes without `next`.
- A missing target at runtime (`handleMissingNextNodeError`) renders an inline retry component and counts
  attempts; past `config.errorHandling.missingNextNodeMaxRetries` (3) it restarts the flow.
- Closing the panel mid-thinking persists the pending transition to `sessionStorage` under
  `tc0080_pending_transition`; reopening (`checkPendingTransition`) shows an "Error en el envío /
  Reintentar" bubble, but only if the same node and user message are still in the log.

### Mobile behaviour

`@media (max-width: 768px)` plus JS: `lockPageScroll`/`unlockPageScroll` freeze `html`/`body` overflow while
the panel is open (with extra `unlockPageScroll` retries after mask-close, for mobile style-application
lag), `bindScrollIsolation` keeps touch scrolling inside the panel, and `updateLauncherMobileOffset` toggles
`tc0080-launcher--lifted` when the host page's `.boton--pulsante` scrolls off the top.

## How it reaches the page

Adobe Target (Alloy Web SDK) fetches decisions for a list of scopes and applies them with
`applyPropositions` + `actionType: "replaceHtml"` into placeholder divs
`div.mbox-container[data-mbox="<scope>"]`, which a Launch rule creates inside a parent container div.

On the **home** (`referencia/bot-insertado.html`): parent `.node-content-parent-otp`, scopes `cards-home`,
`cards-home-personalizacion`, `cards-home-experimento`, `cards-home-acciones`, `cards-home-pruebas`,
`cards-home-paso-paso`, `cards-home-personalizacion-2/-3`, `cards-home-utm-1..5` (plus
`.node-content-parent-no-cookies` / `cards-home-no-cookies` for the pre-consent variant).

In the snapshot the bot's `<style>`/markup/`<script>` sit inside `.node-content-parent-otp` as siblings
after the empty `cards-home` div. **Which specific mbox delivers TC0080 is not determinable from the
snapshot** — don't guess it; confirm with whoever configures Target.

## Porting to /felicitaciones (current objective)

Target page mboxes live under parent `.node-content-parent-felicitaciones`, with scopes
`cards-felicitaciones`, `-personalizacion`, `-experimento`, `-experimento-wording`, `-acciones`,
`-cintillo`, `-footer`, `-seguros`.

Verified differences between the current home and `/felicitaciones`:

- **Design tokens resolve differently.** The home defines almost none of the CSS custom properties the bot
  uses, so it currently renders on the hardcoded fallbacks. `/felicitaciones` *does* define
  `--background-100`, `--onsurface-050/-100`, `--primary-040/-100/-400`, `--secondary-700`. The values
  match the bot's fallbacks (only `--primary-100` differs: `#B2D6FF` fallback vs `#b3d6ff` token), so the
  port is visually safe. `--on-text`, `--on-white`, `--state-success` are defined on neither page and
  always fall back. `--bcp-font-family-primary-regular` (`"Flexo-Regular"`) exists on both and is the one
  var used **without** a fallback.
- **The mobile launcher lift will never trigger.** `getLauncherLiftTriggerNode()` looks for
  `.boton--pulsante` / `.boton-pulsante`. That element exists on the home (hero banner button) but appears
  **zero times** in every `/felicitaciones` snapshot, so `shouldLiftLauncherOnMobile()` always returns
  false. Pick a new trigger selector for the target page or drop the behaviour.
- **Stacking context.** The bot uses `z-index` 9998 (mask) / 9999 (launcher, panel). On `/felicitaciones`
  the cookie-policy banner `.bcp_politica_uso_cookieAdobe.mostrar` uses `9999999999` and an Angular
  `.app-section__header` uses `99999`. (`#tagbird-ui-root` at `2147483647` is a browser-extension overlay
  in the capture, outside `</body>` — not real page content.)

### What the page actually is

`/felicitaciones` is the **post-approval** step: the user is already approved and must pick one of their
approved cards. Header flow is `Oferta → Dónde recibirla → Confirmación`. The snapshot shows a credit line
(`hasta S/ 13,700`), filters `Todas (4) / American Express (1) / VISA (3)`, and four cards — American
Express Clásica LATAM Pass, Visa Clásica LATAM Pass, Visa Clásica Qore, Visa Light — each with membership
terms, TEA/TCEA, seguro de desgravamen, and disposición de efectivo. So the new bot answers "which of *my*
approved cards do I choose, and what do these conditions mean?", not the home bot's "which card should I
apply for?".

### "Elegir tarjeta" does not navigate (2026-09-11)

**The page's own button is not a link.** Each `<xt21-card-option>` closes with a
`<bcp-button type="button" name="QA_Congratulations_BtnSeleccionar_<CODE>">` wrapping a plain `<button>` —
an Angular component that runs the SPA's own logic. There is no `href` anywhere in it, so there was no
destination to copy into the bot. Marco's call: **do the same thing the page does.**

So the CTA clicks the real button. `getNativeCardButton(codigo)` walks `xt21-card-option`, matches on the
code through the existing `getCardCodeFromNode`, and returns the inner `<button>` (falling back to the
`[name^="QA_Congratulations_BtnSeleccionar_"]` host if Angular has not hydrated yet). `elegirTarjeta` then
tracks the click, closes the panel and calls `.click()` on it.

**Verified against certi on 2026-09-11** (Marco ran the click in the console): the programmatic `.click()`
**does** drive the page. It opens `<xt21-modify-credit-line-modal>` — "Ahora puedes editar tu línea de
crédito", with the card name, the range and a *Cerrar* / *Continuar* pair. So choosing a card is **not** a
straight navigation to step 2: there is a credit-line modal in between. Anything that assumes the bot's CTA
navigates away is wrong.

**That modal is out of scope** (Marco, 2026-09-11): it is the page's own base behaviour on selecting a card.
The bot's job ends at pressing the button. Do not style it, intercept it, or try to carry state into it.

**The click does fire the page's own event, and the bot must not add it.** Measured in certi on
2026-09-11 (Marco):

```json
{ "event": "trackPopup", "popup": { "name": "Cards - Edita tu linea de credito" } }
```

It comes from the `analytic="" tag="tagPopup"` on the modal, fires on its own when the button is pressed,
and **a synthetic click fires it just the same**. So the page's funnel stays intact and the bot must not
emit anything resembling it — pressing the real button is the whole job.

> An earlier reading here said the button emitted nothing, based on `digitalData.slice(n)` returning `[]`.
> That was wrong: the push is asynchronous and the check ran in the same tick. If you re-measure, wait.

**A second event carries the card**, fired in the same tick:

```json
{ "event": "trackMetadataList",
  "metadataList": [
    { "key": "TarjetaSeleccionada", "value": "American Express Platinum BCP LATAM Pass" },
    { "key": "Flujo", "value": "Flujo Normal" } ] }
```

So the page already records which card was chosen. **The bot must not duplicate it**: `position` stays the
fixed `"Elegir Tarjeta"` Marco specified. (An earlier note here proposed appending the code — dropped, the
data exists and is better sourced at the page.)

> ⚠️ **`Flujo` is the open question for the experiment.** The field exists, which means the page already
> contemplates more than one flow — but a selection made through the bot still reports **`"Flujo Normal"`**.
> As it stands, nothing on the page's side tells a bot-driven choice apart from a direct one. Whether that
> value can become something like `"Flujo Tarjetín"` is BCP's call, not ours. Failing that, attribution
> means time-correlating their `trackMetadataList` with the bot's `Arbol - TC` click. Raised 2026-09-11.

**A fourth spelling of the card names.** That event says "American Express Platinum **BCP** LATAM Pass",
where the DOM says "American Express Platinum LATAM Pass" and the bot now matches the DOM. One more reason
the crosswalk rule is *cross by code, never by name* — see `contenido/tarjetas-catalogo.json`.

**Choosing a card closes the bot for good** (Marco, 2026-09-11). `elegirTarjeta` calls `cerrarDefinitivo()`,
not `close()`: panel, mask, welcome bubble **and launcher** all go, with no way back — the flow continues on
the page. This is also what keeps the launcher from floating over the credit-line modal, whose backdrop is
`z-index: 7001` against the launcher's `9999`. Do not swap it back to `close()`, which deliberately
*restores* the launcher.

Consequences worth knowing:

- **Both CTAs are `<button type="button">` now, not `<a href>`.** `href`, `target` and `rel` are gone from
  `createRecommendationMarkup` and `createCardBoxMarkup`; what travels instead is `data-codigo`. The
  `cta` field is down to `{label}` — a `href` there would do nothing.
- **The four `q21`–`q24` recommendation blocks carry `codigo: "TCRORL"`**, which they needed to know which
  button to press. That makes the unfiltered-recommendation problem concrete rather than theoretical: for a
  user without the Visa Oro there is no native button to click, so **the CTA does nothing** and logs
  `ELEGIR_TARJETA_SIN_BOTON`. The panel is deliberately *not* closed in that case — closing it with nothing
  happening would read as the bot breaking. See *Still open*.
- **The harness fabricates the inner `<button>` too**, and logs the click to its console panel, so the whole
  path is testable locally. Without it there would be nothing to click in the preview.

### Lead detection (which cards a user actually has)

**The card list is per-user**, and the DOM says which cards the user is a lead for. Every approved card is
an `<xt21-card-option>` (inner `section.card-option`) and publishes its **code twice**: in the image
`src` (`.../assets/images/congratulations/img/AMXCLL.svg`) and in the button's analytics `name`
(`QA_Congratulations_BtnSeleccionar_AMXCLL`). **Code present in the DOM ⇒ the user is a lead for that
card.** Nothing else has to be inferred.

Both environments expose this identically, and `con-exp` / `sin-exp` don't differ here either. The one
difference is the CDN host — `acdneu2xt21p01ecdn01` (production) vs `acdneu2xt21c01ecdn01` (certi) — so
**match on `/congratulations/img/` and take the file name, never the full URL**. **Production is the
reference environment**; see the card-count bullet under *Facts worth not re-deriving* for why certi is
still needed as evidence.

The `#profitType` header carries `data-credit-line`, `data-client-name`, `data-lead-program` (`LAN` in
both), `data-profit-type` (`.` in prod, `GRUP20` in certi) and **`data-card-recomended`** — the page's own
favourite, which matches `.card-option.highlights-green` and the `LA FAVORITA` badge. Per-card costs live
in `xt21-cost-item[label="Membresía Anual" | "TEA / TCEA máx" | "Seguro de desgravamen" | "Disposición de
efectivo"]`, and the miles line in `.card-option__benefits` — so a dynamic version could read the amounts
from the page instead of hardcoding them.

**Marco's rule for the perfilador (2026-09-09):** show **at most 3** cards, and a **fourth that is always
Visa Oro LATAM Pass (`TCRORL`) in the green recommendation box — but only if the user has it**. Note
`TCRORL` is absent from the production snapshot, so that user would see no fourth card. The selection
order for the three is undecided; Marco defines it screen by screen.

`contenido/tarjetas-catalogo.json` is the crosswalk: 17 cards × code, DOM name, name as written in the bot, affinity
scores, membership, exoneration, miles and Priority Pass, plus the DOM selectors and the open questions.
It is dev-only — **the snippet never reads it**, same rule as `contenido/wordings/`. **Cross-reference by code, never
by name**: the DOM says "American Express Black LATAM Pass", the bot's tables say "American Express Black
LATAM" (no trailing "Pass") and `contenido/afinidad-tarjetas.json` says "Amex Black LATAM Pass".

**The bot never abbreviates the brand** (Marco, 2026-09-11): every user-visible string writes **"American
Express"** in full — "Amex" and "AMEX" appear nowhere in `adobe-target/piloto/bot.html`. With that plus the completed
"Pass" on `AMXGRE`, **`nombreEnElBot` now matches `nombreDom` character for character** on the six cards
that appear in a snapshot. That coincidence is recent and one copy change away from breaking, so it does
not license cross-referencing by name. This replaced the 2026-09-09
decision to unify on "Amex". `contenido/afinidad-tarjetas.json` still says "Amex …" and is left alone: it is the
record of what Marco sent, and `contenido/tarjetas-catalogo.json` keeps that spelling in `nombreEnAfinidad` so the
crosswalk stays honest. Its `nombreEnElBot` fields carry the full name.

### Decisions taken (2026-09-06)

- **Reuse the engine as-is** was the starting decision: same graph/chat/thinking/rating/error machinery,
  changing only `config.nodes`, the copy and the namespace. Marco has since directed specific engine
  changes (tables, animations, no `<br>`, avatar removal) — see *Engine changes* below. Still do not
  refactor on your own initiative; change the engine only when he asks for something that needs it.
- **Marco supplies all content**, screen by screen, including the tree and copy. Do not invent flows,
  wording, card data, or CTA destinations — implement what he sends.
- **Experiment code is `TC0091`** (Marco, 2026-09-11). The placeholder swap is **done**: `tc0091-` CSS
  namespace, `#tc0091-scope`, `tc0091_pending_transition`, `window.tc0091Catalogo` and
  `config.analytics.experimentCode: "TC0091"`. The analytics prefix `[[PAGINA]]` became `Felicitaciones`.
  586 replacements in `adobe-target/piloto/bot.html`, plus `contenido/wordings/` and `preview/index.html`. Nothing in the
  working set still carries a placeholder.

## State of the offer (`adobe-target/piloto/bot.html`)

### Where the project stands (handoff, end of 2026-09-09)

**Three of the four branches are finished and signed off by Marco screen by screen.** The bot is walkable
end to end in the preview, `validateGraph` is clean, every node is reachable from `q0`, and no `next`
dangles.

| Branch | State |
| --- | --- |
| `q0` root menu | done |
| `q2` "Quiero comparar mis tarjetas" + `q21`–`q24` | **done**, all four tables + recommendation cards |
| `q3` "Resolver dudas" + `q31`–`q33` | **done**, all three answers |
| `q1` "Ayúdame a elegir" + `q11`–`q14` | **done** (2026-09-09) — all four are dynamic, two casuistries each |

**There is no content left to build.** The perfilador landed on 2026-09-09: `q11`–`q14`, each with a case
A and a case B, all resolved at runtime from the user's approved cards. What remains is *decisions* and the
**one** missing card code — see *Still open*.

**What changed on 2026-09-09** (a long session; this is the short version):

- `q3` went from 5 questions to 3, and all three answers were written.
- `q21` and `q24` got their recommendation cards; `q22` and `q23` were reconfirmed verbatim, zero changes.
- All four comparison screens swapped `✅ Elegir una tarjeta` for `Cerrar`, which orphaned the whole
  satisfaction-survey chain — **and Marco chose to delete it** rather than re-route it.
- Six copy corrections were approved, plus that button unification. All are logged in
  `docs/correcciones-wording.md`, which is a **deliverable for the UI team**, not an internal note.
- Several engine rules changed: bubble width, bubble spacing, paragraph spacing, tight line breaks, option
  padding. Each is written up under *Engine changes* below.
- `contenido/wordings/` was created as a dev-only mirror of all the copy — read it instead of this file when the task
  is wording.

15 nodes. Namespace is `tc0091-` / `#tc0091-scope` / `tc0091_pending_transition` /
`window.tc0091Catalogo` / `experimentCode: "TC0091"`, and the analytics prefix is
`"Felicitaciones - Cards - Bot - ..."`. Assigned and swapped on 2026-09-11 — no placeholders left.

| Branch | Status |
| --- | --- |
| `q0` root menu | done — 3 options |
| `q1` "Ayúdame a elegir una tarjeta" | done — 4 criteria → `q11`–`q14` (Marco calls these **1.1–1.4**) |
| `q2` "Quiero comparar mis tarjetas" | done — 4 features → `q21`–`q24` (**2.1–2.4**) |
| `q3` "Resolver dudas para elegir" | **branch closed 2026-09-09** — menu of 3 questions → `q31`–`q33`, all three answered. It used to have 5; three were dropped and one added |
| `q31` "¿Puedo cambiar mi línea de crédito?" | done — two tight lines + focus paragraph, plain text |
| `q32` "¿Es seguro solicitarla por aquí?" | done — the only screen in the bot with bullets, so `richText: true` |
| `q33` "¿Por qué me ofrecieron esa línea?" | done — four factor lines tight, **no bullets**, plain text |
| `q21` "Acumulación de millas" | done — intro + 17-row table + recommendation card + menu. Marco sent the full screen on 2026-09-09 |
| `q22` "Membresía Anual" | done — reconfirmed verbatim against Marco's 2026-09-09 text, zero changes needed. All in **one bubble** |
| `q23` "Exoneración de membresía" | done — reconfirmed verbatim against Marco's 2026-09-09 text, zero changes needed |
| `q24` "Priority Pass" | done — final intro + recommendation card, both delivered 2026-09-09. The recommended card contradicts its own table, see *Still open* |
| `q11` "Viajar y acumular millas" | done 2026-09-09, and **the only dynamic screen**: it carries no text or cards of its own, just `perfilador: "viajar"`. `resolvePerfilador` reads the user's approved cards from the DOM and picks the case and the cards at runtime. All 17 cards live in `config.perfilador`. See *The perfilador is dynamic* below |
| `q12` "Ahorrar en costos" | done 2026-09-09 — dynamic, `perfilador: "ahorrar"`. Case A = the 11 cheaper cards, case B = the 6 premium ones + the Visa Oro |
| `q13` "Obtener más beneficios" | done 2026-09-09 — dynamic, `perfilador: "beneficios"`. Case A = 14 cards, case B = 4 |
| `q14` "Experiencias exclusivas" | done 2026-09-09 — dynamic, `perfilador: "experiencias"`. Case A = 10 premium, case B = 8 |

`q22` is the reference implementation of a finished comparison screen. All four now close with
`menuText: "¿Qué deseas hacer ahora?"` + `↩️ Volver a las alternativas` → `q2` + `Cerrar` (`isClose`).

**The closing chain was deleted** (Marco, 2026-09-09). `mixed1`, `rating`, `rating_number`,
`rating_thanks_auto` and `mixed100` are gone from `config.nodes`. They had become unreachable when the four
comparison screens swapped "✅ Elegir una tarjeta" for "Cerrar", and Marco chose to drop the satisfaction
survey rather than find it a new entry point. Every node is reachable from `q0` and no `next` dangles.

### Dead code removed (2026-09-10)

**The rating and `mixed` machinery is gone** (Marco asked for it on 2026-09-10). It had been dead since the
closing chain was deleted the day before: no node was `type: "rating"` or `type: "mixed"` any more. About
**9.7 KB** came out, and `adobe-target/piloto/bot.html` no longer contains the string "rating" anywhere.

What was removed, so nobody goes looking for it:

| Where | What |
| --- | --- |
| CSS | `.tc0091-rating`, `.tc0091-rating-btn` (+ `svg`, `:focus`, `:focus-visible`), `.tc0091-node-rating`, `.tc0091-node-rating-thanks` |
| Widget | `createRating`, `createRatingReadonly`, `getRatingStarSvg` (both star SVGs), `paintRatingStars` |
| Analytics | `trackRatingFinalView`, `trackRatingFinalClick`, and the `node.id === "rating_number"` hook in `appendBotMessageForCurrent` |
| State | `state.rating` (init, `restart()`, the snapshot, and the restore in `back()`) and the `selectedRating` field on both chatLog message builders |
| Handlers | the whole `[data-rating]` block in `attachHandlers` — click, hover, focus, `mousemove`/`mouseleave`/`focusout` on `.tc0091-rating` |
| Render | `if (node.type === "rating")` and `if (node.type === "mixed")` in `renderSlides`, plus `includeRatingInBubble` in `renderChat` |

**Then the `slides` mode went too**, in the same session and at Marco's request, once the preview's
chat/slides toggle was removed and nothing called it any more:

| Where | What |
| --- | --- |
| Render | `renderSlides`, plus `createHeader`, `createText`, `createOptions`, `createFooter` — none had another caller |
| Mode | `config.mode`, `setMode()`, `window.tc0091SetMode`, and `render()`'s ternary, which now calls `renderChat` directly |
| Dead branches | the slides early-return in `adjustChatScroll`, the `tc0091-thinking--slide` waiting branch in `render()`, and the three `if (config.mode === "chat")` guards in `goTo`, `applyTransition` and `back()` |
| State | `state.history`, which only the slides `back()` ever read |
| Handlers | the `[data-restart]` and `[data-back]` blocks — they existed only for the slides footer |
| CSS | `.tc0091-msg`, `.tc0091-options button`, `.tc0091-footer` (+ its nested `.tc0091-btn` and `[data-back]`), `.tc0091-thinking--slide` |

**And that orphaned the undo machinery, which Marco then had removed as well**: `back()`,
`pushChatSnapshot()` and `state.chatSnapshots`. The slides footer's "Volver" button had been `back()`'s
only caller — **the chat UI never had a back control**, because `↩️ Volver a las alternativas` is ordinary
`data-next` navigation, not `back()`. So nothing was lost in behaviour, and `goTo` no longer clones the
whole `chatLog` on every transition.

**Total for the session: 16.4 KB, 246.7 KB → 230.3 KB.** A scan of all 91 prototype methods afterwards
found no orphans, which is the check worth repeating after any removal of this kind.

**Two signatures changed**: `renderBotItem(item, isActiveQuestion, includeRating, claseEntrada)` lost its
third parameter and is now `renderBotItem(item, isActiveQuestion, claseEntrada)`, with a single call site
in `renderChat`; and `createThinkingMarkup(extraClass)` lost its parameter, since the only caller that
passed one was the slides branch.

**Deliberately kept:**
- `.tc0091-btn` — the panel's close button is `class="tc0091-btn tc0091-close"`, and `.tc0091-chat-actions`
  nests it. It was never part of the slides path.
- `createMainCta` — `renderChat` calls it when `state.finished` is true.
- The per-message snapshot inside `appendBotMessageForCurrent`. That is what freezes the perfilador's
  resolved cards into the chatLog; only the *undo* snapshots went.

`referencia/bot-actual.html` keeps every bit of this — it is the live home bot and is read-only. **This is now by far
the largest divergence between the two engines**: they no longer agree on node types, render paths, state
shape or several method signatures. Do not port anything between them assuming they match, and do not
"restore" something here because it exists there.

If the satisfaction survey, the slides mode or a back button ever come back, they come back from
`referencia/bot-actual.html` as the reference, not from this file's history.

**Four more dead CSS rules went too** (Marco, 2026-09-10), inherited from the home bot and unrelated to
slides: `.tc0091-chat-actions` and its nested `.tc0091-btn`, bare `.tc0091-error` — the
`.tc0091-error-message`, `-icon`, `-text` and `-retry` rules *are* used and stayed — and `.tc0091-hidden`,
which was never applied to anything.

**A sweep of every `.tc0091-*` class declared in the `<style>` block against the markup and the JS strings
now returns zero unused classes.** That sweep, plus the prototype-orphan scan, is the pair of checks worth
re-running after any removal here. `.tc0091-btn` survives on a single use: the panel's close button is
`class="tc0091-btn tc0091-close"`.

### Engine changes made in the offer

These are real deviations from `referencia/bot-actual.html`. Diffing the two engines will no longer return zero.

- **`richText` now works on every node type.** It was only honoured for `response`/`auto`; `question`
  nodes silently fell through to plain text, so bold, bullets and tables were ignored there. Fixed in
  `renderBotItem`.
- **Tables.** `formatRichText` parses markdown pipe rows (`| a | b |`, first row = header, dash row
  discarded, closed by a blank or non-table line) into `.tc0091-rich-table` wrapped in
  `.tc0091-rich-table-wrap`. The wrapper exists so `border-radius` clips the zebra striping and row
  borders; without it the bottom corners render square.
- **No `<br>` anywhere, and two levels of line separation** (the second added 2026-09-09).
  `formatPlainText()` splits on blank lines first, then on single newlines:

  | In the copy | Rendered as | Gap |
  | --- | --- | --- |
  | blank line | new `<p class="tc0091-text-line">` | 16px |
  | single newline | `<span class="tc0091-text-row">` (`display: block`) inside the same `<p>` | **0px** |

  The tight row exists because Marco's Figma chains two sentences with a hard break and no air, reserving
  the air to separate blocks of idea (`q31`, `q33`). It is a `display: block` span, not a `<br>` — the same
  trick the launcher greeting already used with `.tc0091-welcome-line`. **This changed the meaning of a
  single `\n`**: before, every line became its own 16px-separated paragraph. All pre-existing copy used
  `\n\n`, so nothing broke, but check the two-level rule when reading old node text.

  `formatRichText` (used when `richText: true`) is a separate path and does **not** support the tight row:
  its blank line emits a `.tc0091-rich-spacer` and `.tc0091-rich-text` has `gap: 8px`.
- **Bubble spacing depends on who spoke** (Marco, 2026-09-09). `.tc0091-chat` no longer uses `gap` — a gap
  is uniform and this distance is not. Spacing runs through `> * + *` margins: **24px when the sender
  changes, 12px when two bubbles from the same side are chained**, so a run of bot messages reads as one
  block. Verified on the closing chain, where four consecutive bot bubbles sit at 12px and the one that
  follows a user bubble stays at 24px. The selectors key off `.tc0091-chat-bot-row` and
  `.tc0091-chat-item--user` being direct children of `.tc0091-chat` — keep that flat structure if you touch
  `renderChat`.
- **Message entrance animation.** `.tc0091-msg-enter` slides bubbles in from their own side (bot from the
  left, user from the right) with `scale(0.8)`. Because `render()` rebuilds the whole thread with
  `innerHTML`, the class is applied only to chatLog entries past `this.animatedUpTo` — otherwise the
  entire history re-animates on every message. That counter lives **outside `state`** on purpose so it
  never travels in `pushChatSnapshot`; it resets in the constructor and `restart()`, and is clamped in
  `back()`. The initial opacity lives only inside the `@keyframes`, so `prefers-reduced-motion` (which
  kills animations with `!important`) leaves bubbles visible rather than invisible.
- **Panel entrance on desktop.** `shouldAnimateMobilePanel()` → `shouldAnimatePanel()`, now true at any
  width. The class/rAF/`transitionend` machinery was already there; only the mobile gate was removed.
- **Bot avatar removed from the thread.** The `.tc0091-chat-bot-icon` span, its `iconSvg` variable and its
  CSS are gone. The header avatar stays. `.tc0091-chat-bot-row` remains because it anchors
  `data-feedback-anchor`.
- **`.tc0091-body` is `overflow-x: hidden`.** The old `overflow: auto` shorthand let the entrance
  animation's X displacement raise a horizontal scrollbar.
- **Option buttons wrap.** `white-space: nowrap` removed, plus `min-width: 0` on
  `.tc0091-chat-item-option-label` — a flex item will not shrink below its content width without it, so
  removing nowrap alone does nothing.
- Online dot is `var(--state-success, #6AC90F)` — **green**, where `referencia/bot-actual.html:1035` and Marco's
  mockup both have `white`. Confirmed by Marco on 2026-09-09; do not "fix" it.
- Header title is **"Asistente Virtual"** with a capital V (Marco, 2026-09-09), where the home bot uses
  "Asistente virtual". The divergence between the two products is intentional.
- Option buttons are `padding: 7.5px 16px` (Marco, 2026-09-09; was `9px 16px`), which puts them at 34px
  tall. The orange recommendation CTA keeps its own `6px 16px` and the bubble keeps its `12px`.
- Spacing values were tuned (title `margin-bottom: 8px`, `.tc0091-chat-item` `padding: 12px`, options
  `gap: 8px`).
- **Type scale: 14px text is always `line-height: 20px`** (Marco, 2026-09-09). Applied to every rule that
  declares `font-size: 14px` — bubble text, titles, card descriptions, bullets, recommendation details,
  option buttons, CTAs, the launcher greeting and the error text. Ratios like `1.2`/`1.3` are gone from
  those rules; use the pixel value when adding new 14px text.

  The one exception is the `aviso` box, which Marco specified at 12/18 — see the `aviso` node field below.

  Two knock-ons worth knowing: **option buttons went from 34px to 37px tall** (the `padding: 7.5px 16px`
  Marco tuned earlier was aimed at 34px, and the taller line-height wins), and `.tc0091-card-name` **was
  15px and is now 14px** at Marco's request, so the perfilador card title is the same size as its body copy
  and only weight tells them apart.

  Not covered by the rule, and still on their own values: `.tc0091-rec-card` (15px — the card name inside
  `q21`–`q24`'s recommendation box, the sibling of `.tc0091-card-name` that did *not* change), the tables
  (13px), the badge and the status line (12px), and `.tc0091-error-retry` (13px).
- **Paragraph spacing is a flat 16px** (Marco, 2026-09-09). `.tc0091-text-line` is `margin: 0 0 16px`, so
  the gap between paragraphs and the gap between the last paragraph and the options below are the same.
  Previously paragraphs sat at 8px and only the last line got 16px; the `:last-child` override is gone,
  since both values now coincide. Affects every `"<intro>\n\nElige una opción:"` screen (`q0`–`q3`).
- **Bubble width** (Marco, 2026-09-09). `.tc0091-chat-item` is `max-width: 100%` with no fixed `width`, so
  a bubble sizes to its own max-content. On top of that, a bot bubble that **contains option buttons or a
  table** is forced to `width: 100%`:

  ```css
  .tc0091-chat-item--bot:has(.tc0091-chat-item-options),
  .tc0091-chat-item--bot:has(.tc0091-rich-table) { width: 100%; }
  ```

  Without that rule a menu sits at whatever its longest button measures — `q3` landed at 96% and looked
  misaligned next to the others. Text-only bubbles are deliberately left out and keep their natural size.
  The measurements were taken on 2026-09-09, when the closing chain still existed: 89% ("¡Gracias por
  utilizar nuestro asistente!"), 75% (the rating widget), 61% ("¡Gracias por tu calificación!"). **Those
  three bubbles no longer exist** — the chain was deleted and the rating widget removed — so they are kept
  only as the evidence behind the rule, not as screens to go looking for. User bubbles are never forced
  (40–62%).

  `:has()` is the only modern-CSS dependency in the file. Where it is unsupported the bubble just falls
  back to content width — the previous behaviour, not a broken layout.

  This replaced a fixed `width: 90%` on bot bubbles plus a `tc0091-node-ancho` opt-in class for the table
  screens. **That class no longer exists** — its CSS rule and all four `className` usages were removed, and
  `.tc0091-node-rating`'s `width: 90%` went with them. Do not reintroduce a fixed width to "make a bubble
  wide". The `mixed100` wrinkle recorded here before (a tiny "¿Qué deseas hacer ahora?" bubble forced to
  100%) **no longer applies**: that node was deleted with the closing chain.
- **Recommendation card (`recommendation` node field).** New optional node field rendered by
  `createRecommendationMarkup` between the text block and the options:
  `{title, badge, card, details: [], cta: {label, href, target}}`. It sits **inside the same bubble** as
  the table because the CTA belongs to the card, not to the navigation menu — the home bot instead puts
  its CTA in the `mixed` menu's option list, which would move "Elegir tarjeta" below
  "¿Qué deseas hacer ahora?". The CTA renders as an `<a>` while the message is the active question and as
  a disabled `<button>` afterwards, matching how option buttons decay.

  Layout comes from Marco's mockup (sent 2026-09-06) and is **not** freely stylable:
  - `title` ("Te recomendamos esta tarjeta") is plain thread text **outside** the box, above it.
  - the box has a green border and a near-white `#F9F9FB` fill, `border-radius: 24px`, `padding: 16px`.
  - `badge` ("La más usada") is a solid-green chip **inside** the box, top-left, on its own line, with an
    almost square `border-radius: 2px` — deliberately not a pill.
  - `details` render as a real `<ul>` with disc bullets, not paragraphs.
  - the CTA is right-aligned and **auto-width** (`.tc0091-rec-actions` is `justify-content: flex-end`),
    unlike `.tc0091-chat-item-option--cta` which is full-width.

  Greens are eyeballed from the mockup and unconfirmed: border `var(--state-success, #6AC90F)` (the token
  already used by the header's online dot), badge `#3D9B00` picked darker so white text stays legible.
- **`aviso` node field (2026-09-09).** A one-line notice rendered by `createAvisoMarkup` **between the
  cards and `menuText`**: a light-blue rounded box with a 20×20 inline SVG on the left. Used by `q11` for
  "Solo te mostraremos las tarjetas recomendadas", which used to be the first paragraph of `menuText`.

  It is **the only text in the thread that is not 14/20**: Marco specified 12px / `line-height: 18px` /
  `#002A8D`, and supplied the SVG (whose `fill` is hardcoded `#002A8D`, as delivered). He also gave
  `border-radius: 8px`, `margin-top: 24px`, and **32px between the box and `menuText`** — that last one via
  `.tc0091-aviso + .tc0091-menu-text`, an adjacent-sibling rule so `q21`–`q24` and `q31`–`q33` keep their
  16px. Only `background: var(--primary-040, #EBF4FF)`, `padding: 16px` and `gap: 16px` were matched to his
  mockup by eye and are still unconfirmed.
- **`menuText` node field.** Copy rendered after the recommendation card and before the options, so
  "¿Qué deseas hacer ahora?" can close a single-bubble screen. Both fields travel in the chatLog snapshot
  (`appendBotMessageForCurrent`) and are honoured in `renderSlides` too.
- **`cards` node field (2026-09-09), for the perfilador.** An array of card boxes rendered by
  `createCardsMarkup` between the text and `menuText`:
  `{badge?, name, description, bullets: [], cta: {label, href, target}}`. Visually it is the same box as
  the recommendation block, in two variants: neutral border by default, and **green border when the card
  has a `badge`** — the badge is what drives the highlight, there is no separate flag. Each card carries
  its own right-aligned CTA, which decays from `<a>` to disabled `<button>` exactly like the
  recommendation one.

  It is a **sibling of `createRecommendationMarkup`, not a refactor of it**: `q21`–`q24` are approved and
  were left untouched, so the `.tc0091-card*` CSS deliberately duplicates the `.tc0091-rec*` rules. If both
  ever need to change together, that duplication is the thing to fix — but only when asked.

  The one layout difference from the recommendation block is `.tc0091-card-desc`: a description line glued
  to the card name, with the air kept for separating it from the bullets (`.tc0091-card-bullets` has
  `margin-top: 12px`). `cards` travels in the chatLog snapshot and is honoured in `renderSlides`.

- **The `cards` block is a horizontal carousel, not a stack** (Marco, 2026-09-10). Stacked vertically, 3–4
  cards made the bubble enormous: the user scrolled the whole panel and lost the comparison between cards.
  One card is visible at a time at 82% of the track width, so ~18% of the next one peeks past the right
  edge, and below it sits a centred control row — gray circular prev (disabled on the first card), dots,
  orange circular next.

  **The card itself did not change by one pixel.** `createCardBoxMarkup` was split out of
  `createCardsMarkup` verbatim, and `createCardsMarkup` now only wraps it. The `.tc0091-card*` CSS is
  untouched; every carousel rule is a new `.tc0091-carousel*` class.

  Things worth knowing before touching it:

  - **The scrolling is native, not `transform`.** `overflow-x: auto` + `scroll-snap-type: x mandatory` on
    `.tc0091-carousel-track`, so swipe, inertia and snap are the browser's; the JS only sets `scrollLeft`
    for the arrows and dots. Do not rewrite it as a transform slider — that would mean reimplementing the
    touch gesture.
  - **`bindScrollIsolation` had to learn about it.** Its `wheel`/`touchmove` handlers call
    `preventDefault()` when the panel has no vertical scroll, which killed the horizontal swipe on exactly
    the short screens the carousel is for. Both now bail out early (keeping `stopPropagation`) when the
    event target is inside `.tc0091-carousel-track`. If swipe ever stops working, look there first.
  - **Equal heights come from `align-items: stretch`** on the flex track, and because of that the CTA of
    the shorter cards is pushed to the bottom with `margin-top: auto` (its 12px of air becomes
    `padding-top`). That override is scoped to `.tc0091-carousel-slide`, so a single-card block keeps the
    original spacing.
  - **A single card renders as before**, through `.tc0091-cards`, with no controls — a dots row for one
    card is noise.
  - **The `La más usada` badge needs `align-self: flex-start`** inside a slide. It is an `inline-block`,
    but turning the card into a flex column makes it a flex item, and the default stretch ran its green
    across the whole card width (spotted by Marco, 2026-09-10). Anything else `inline-block` added to
    `.tc0091-card` later will hit the same trap — the `<p>`s and the `<ul>` do not, they were full width
    already, and `.tc0091-card-actions` stretches on purpose to right-align its CTA.
  - **The active index lives in `this.carouselIndex`, keyed by carousel id, outside `state`** — same
    reasoning and same trap as `animatedUpTo`: `render()` rebuilds the thread with `innerHTML`, so without
    it every carousel in the history would snap back to the first card on each new message. It resets in
    the constructor and in `restart()`. The id is `"msg-" + messageId` in chat and `"slide-" + node.id` in
    slides mode.
  - The active index is derived by **nearest slide `offsetLeft`**, not by dividing `scrollLeft` by a fixed
    step: the last slide snaps against the end of the scroll, so a rounded division would never select it.
  - Dots reuse the thinking-dots convention — active `#0A47F0`, inactive `#99A1AD` at 45% opacity — and the
    next arrow reuses the CTA orange `#FF7800` / `#ff961f`. No new colours were invented.
  - **Carousel navigation stays enabled in old messages**, unlike the CTAs, which still decay to disabled
    `<button>`. Browsing back through cards is not a decision that advances the flow. Unconfirmed by
    Marco.

### The perfilador is dynamic (2026-09-09)

Marco's decision: **stop building the perfilador statically** — it works for real. `q11` is the first node
whose content is not in the node.

- The node carries **`perfilador: "viajar"`** and nothing else: no `text`, no `cards`.
- `resolvePerfilador(criterioId)` calls `getLeadCardCodes()`, which walks
  `document.querySelectorAll("xt21-card-option")` and extracts each code from the image `src` (falling back
  to the button's `name`). See *Lead detection* above.
- **Case A vs case B is derived, not configured**: if any of the user's cards is in the case A list, case A
  wins; otherwise case B. The two intros live in `config.perfilador.criterios.viajar.casoA/casoB`.
- **Cut rule** (`recortarConDestacada`): up to `maxSinDestacada` (3) cards from the order, plus
  `destacada` (`TCRORL`, Visa Oro LATAM Pass) appended last **only if the user has it** — so a user without
  the Oro sees 3, not 4. Case B has no destacada and simply cuts at `maxTarjetas` (4).
- The badge is what makes a card green: `createCardsMarkup` applies `--destacada` when `badge` is present,
  and `recortarConDestacada` is the only thing that sets it.
- **Resolution happens in `appendBotMessageForCurrent`, not at paint time.** The resolved intro and cards
  are frozen into the chatLog snapshot, so re-renders never rewrite history if the page DOM changes.

**Cards with `codigo: null` can never be detected and therefore never show** — today that silently drops
*Visa Clásica* (plain) and nothing else. **Marco decided on 2026-09-10 to leave it that way for now**: with
no code there is nothing to detect, so the card simply never renders, and the day a code is assigned it
starts appearing on its own for the users who are leads for it. No other change is needed.

`config.perfilador.sinDeteccion` decides what happens when the DOM yields no cards at all: it falls back to
the case A list, on the reasoning that the real page always has at least one approved card, so an empty
read means the selector failed. Logged as `console.debug("[tc0091][error:PERFILADOR_SIN_LEADS]", …)`.

**That fallback no longer appends the destacada** (2026-09-11). It used to run through
`recortarConDestacada`, which put the Visa Oro at the end — exactly what Marco's rule forbids, since with
no detection there is no way to assert the user holds it. It now cuts at `maxTarjetas` and stops. The cut
sizes moved into `getTopeTarjetas(cfg)`, shared by this path and case B.

`window.tc0091Catalogo()` is a dev hook (`tc0091SetMode` no longer exists — it went with the slides mode): it returns the criteria, every card the
bot declares (code, name, which case) and the codes currently detected. `preview/index.html` builds its
casuistics panel from it, so **the panel grows on its own as perfilador screens are added** — nothing to
keep in sync by hand.

### The comparison tables are dynamic too (2026-09-09)

Marco extended the lead rule to `q21`–`q24`: **the tables list only the cards the user actually has.**

- The four tables moved out of the nodes into `config.comparador` (`millas`, `membresia`, `exoneracion`,
  `priorityPass`), each `{intro, encabezado, filas: [{codigo, nombre, valor}]}`. The nodes now carry only
  `comparador: "<id>"`.
- `resolveComparador` filters `filas` by `getLeadCardCodes()` and **rebuilds the markdown table as a
  string**, which then goes through the existing `formatRichText` — the same path the hand-written tables
  used, so the rendering is unchanged. `richText` is forced on for any node with `comparador`.
- Each table keeps its own verbatim row order; filtering never reorders.
- Empty detection → all 17 rows, logged as `COMPARADOR_SIN_LEADS`. Same reasoning as the perfilador.

**The recommendation block obeys the lead rule too** (Marco, 2026-09-11). `resolveRecommendation(node)`
reads `recommendation.codigo` and returns `null` unless that code is in `getLeadCardCodes()`, so the whole
green box — title, badge, card, details and CTA — simply is not painted for a user who does not hold that
card. It resolves in `appendBotMessageForCurrent`, next to the perfilador and the comparador, so the
decision freezes into the chatLog like everything else.

Two details that follow from it: `tieneCtaElegirTarjeta` reads the *resolved* message, so the
`Arbol - TC` View is not emitted either when the box is hidden; and **no detection means no box**, since
being a lead cannot be asserted. Misses are logged as `RECOMENDACION_NO_ES_LEAD` /
`RECOMENDACION_SIN_CODIGO`.

Before this, the four screens recommended Visa Oro unconditionally. With the tables already filtered, a
user without it got a recommendation absent from their own table, whose button — once the CTA started
driving the page's native button — did nothing at all.

### Testing casuistics in the preview

The harness fabricates the page's own markup instead of patching the bot: one `<xt21-card-option>` per
selected card, with the code in both the image `src` and the button `name`, injected **before** the snippet
runs. So the real detection code path is what gets exercised. The fake `src` points at a local path that
404s on purpose — nothing is fetched from the BCP CDN.

The side panel has a preset dropdown (with/without the Visa Oro, one miles card only, case B, no detection
at all…) plus a checkbox per card for arbitrary combinations. Cards without a code render disabled and in
amber, which is the fastest way to see the consequence of a missing code.

**There is deliberately no "producción" or "certi" preset** (Marco, 2026-09-10). Those two snapshots were
delivered only to prove the DOM does not change between environments and that the detection holds up —
they are evidence, not test scenarios. Do not re-add them as presets; the six scenarios plus
"Personalizado" are the list.

### The perfilador, screen by screen

All four are built. Each criterion's two lists together cover **all 17 cards**, with no overlap beyond the
Visa Oro, which appears in both cases of `ahorrar`, `beneficios` and `experiencias` (but **not** in
`viajar`'s case B — Marco took it out there because that case says the user has no miles card and the Oro
is one).

Four things constrain the branch, and still do if a fifth criterion ever appears:

1. **They have to recommend more than one card.** `q1` now promises "te recomendaré **algunas opciones** que
   mejor se adapten a ti" (it used to say "solo la opción"). A single-card answer would contradict the
   menu that leads into it.
2. **`contenido/afinidad-tarjetas.json` feeds exactly these four branches** — 17 cards scored 1–5 against the four
   `q1` criteria. Read its `_meta.pendiente_confirmar` before building any logic. It does **not** order the
   `q2` comparison tables.
3. **The cards shown depend on the user being a lead for them** (Marco, 2026-09-09): at most **3** cards
   plus the Visa Oro LATAM Pass last, and **only if the user has it**. Implemented — see *The perfilador is
   dynamic* below. Marco delivers the priority order per screen; record it under `casuisticasPorCriterio`
   in `contenido/tarjetas-catalogo.json` in the same pass as the node.
4. **They are dynamic now.** `q11` is the pattern: a node with `perfilador: "<criterio>"` plus its two
   card lists in `config.perfilador.criterios`. Do not hardcode a card list into a node again.

Do not invent their copy, their card lists or their closing block. Marco delivers each screen as text plus
a Figma image — the design of the multi-card block included.

### Still open

Also tracked in `docs/correcciones-wording.md` (Parte 6), which is the version written for the UI team.

**Blocked on Marco**

- **The recommended card is hardcoded and Priority Pass contradicts itself.** All four comparison screens
  recommend *Visa Oro LATAM Pass*, but that card shows **"No"** in `q24`'s own Priority Pass table, so the
  screen recommends a card without the benefit it is comparing. Marco knows, asked to leave it **static as
  is**, and will supply *condiciones* later to pick the card per case (2026-09-09). **Do not change it on
  your own initiative.**
- **`q23`'s two Qore rows still look like membership, not exoneration, amounts**: `Visa Clásica Qore S/80`
  and `Visa Oro Qore S/170` are exactly their `q22` membership figures, while every other Qore row uses the
  exoneration scale. Marco re-sent the screen on 2026-09-09 with the same two values, so it is reproduced
  verbatim and still unanswered.
- Which `cards-felicitaciones-*` mbox delivers the offer.
- **`AMXGRE`: "Amex green" in Marco's list vs *American Express Black LATAM Pass* in the certi DOM.** Kept
  as Black on the DOM's evidence. See the card-code bullet under *Facts worth not re-deriving* for what
  breaks if that reading is wrong.
- **The code for Visa Clásica (plain).** The only card left at `"codigo": null`. Marco chose on 2026-09-10
  to leave it that way: it simply does not render, and it will start appearing on its own once a code
  exists. Nothing to build — just the code.
- **The mobile launcher lift has no trigger on this page.** `getLauncherLiftTriggerNode()` looks for
  `.boton--pulsante` / `.boton-pulsante`, which appears **zero times** in every `/felicitaciones` snapshot,
  so the behaviour is dead code today. Pick a new selector or drop it.

**Lower-stakes, unconfirmed**

- **`Flujo` always reports `"Flujo Normal"`, and that is now the only gap in the funnel.** Since the bot
  stopped tagging the "Elegir tarjeta" click, the page's three native events are the sole record of it, and
  none of them says the click came from the bot. Attribution rests on correlating with the `Arbol - TC`
  View. Whether `Flujo` can carry something like `"Flujo Tarjetín"` is BCP's call — ask them.
- Comparison tables are at `font-size: 13px`. Marco's mockup was rendered wider than the real 374px panel,
  where 14px would wrap long card names onto three lines. Also, his mockup's vertical column divider is
  inset from the row edges; ours is a full-height `border-left`.
- **`q24`'s intro and its table disagree slightly**: the intro says "Estas son tus tarjetas aprobadas **que
  incluyen** el beneficio Priority Pass", which reads like a filtered list, but the table shows all 17 with
  Sí/No. Marco's own copy; raised, not answered.
- `q22` writes Visa Light's membership as a bare `0`, not `S/0`. Verbatim.

**Resolved — do not re-raise**

- `q2`'s four options carry **no emoji**: confirmed intentional by Marco's 2026-09-09 mockup.
- The `"Hola!"` greeting stays without the opening `¡`; `"¡Sí!"` in `q31`/`q32` does carry it. Two separate
  decisions, both made deliberately.
- The online dot stays **green**, not white like the home bot and the mockup.
- The user bubble repeats the button label in full. Marco's mockups draw it shorter in three places
  ("Comparar tarjetas", "Resolver dudas", "…esa línea de crédito?"); adding a per-option bubble label was
  offered and **rejected**. The button wins.
- `q24`'s missing recommendation card: Marco asked for it to be added on 2026-09-09.
- `q24`'s intro: Marco delivered the final wording on 2026-09-09. **No copy in the file is written by
  development any more.**

### Facts worth not re-deriving

- **The affinity table does not order the comparison screens.** Marco pastes the card order per screen and
  it is to be used verbatim. `contenido/afinidad-tarjetas.json` feeds the *recommendation* branches (`q11`–`q14`)
  only. The 2.1 table happens to list the same 17 cards, which is a coincidence of catalogue, not a sort.
- **Card counts differ by environment**: the `certi` snapshots show **13** cards
  (`Todas (13) / VISA (9) / American Express (4)`); production shows **4** — American Express Clásica LATAM
  Pass, Visa Clásica LATAM Pass, Visa Clásica Qore, Visa Light. **They are not two catalogues, they are two
  users**: each snapshot renders only the cards that user is a lead for.

  **Neither is a "reference environment", and neither is a test scenario** (Marco, 2026-09-10). The two
  snapshots were delivered for exactly one purpose: to prove the DOM does not change between environments
  and that the detection code holds up against both. That is why the preview has no `producción` / `certi`
  preset. The code identifies the *card*, not the environment — verified on the three cards present in both
  (`AMXCLL`, `TCRCLL`, `TCRMIN`): same code, same name. Beyond that they are evidence of where each code
  was observed, recorded per card in `vistaEn`. Together they yield **14 codes out of 17** seen in a real
  DOM; `TCRLY5` (Visa Clásica Qore) was seen only in production, the other 10 only in certi.

  **Marco's list of 2026-09-10 brings 16 codes** and is the authority — it confirmed `TCRLY5` (which until
  then rested only on the production snapshot) and supplied **`TCRLY4` for Visa Platinum Qore**, a card
  that had been sitting at `"codigo": null` and therefore never rendered. With `TCRBA7` (delivered
  2026-09-09) that leaves **`TCRBA7` and `TCRLY4` as the only two codes not verifiable against any
  snapshot**, and **Visa Clásica (plain) as the only card still without a code**. **Absent ≠ nonexistent** —
  a card missing from both snapshots only means neither captured user was a lead for it.

  **One conflict is still unresolved.** Marco's list writes `AMXGRE` as "Amex green", but the certi DOM
  renders that same code as *American Express Black LATAM Pass* (verified in
  `referencia/paginas/referencia-certi-con-exp.html`). The catalogue keeps the DOM's reading — `AMXGRE` = American Express Black
  LATAM Pass — because it is direct evidence, and treats "green" as an internal offer label. If `AMXGRE` turned
  out to really be an Amex Green, then the American Express Black would have no code and would silently
  vanish from the three premium casuistries where it appears today (`viajar` case A, `beneficios` case A, `experiencias`
  case A). Do not resolve this by guessing.

  The page lists cards descending by tier (Infinite → Signature → Platinum → Oro → Clásica → Light), and the
  two environments agree on that order.
- **`AMXGRE` is now written the same everywhere** (Marco, 2026-09-11). Marco's 2.1 and 2.2 lists wrote the
  Black card without the trailing "Pass", so the four tables said "American Express Black LATAM" while the
  perfilador said "… Pass". He asked for the "Pass" to be completed, and all five now agree on **"American
  Express Black LATAM Pass"**. Do not re-raise it as a verbatim irregularity — it was, and it was fixed.
  The Qore cards, Visa Light and Visa Clásica correctly carry no "LATAM Pass": they are not in that program.
- **The comparison screens do not share one card order.** 2.1 and 2.3 put Visa Light and Visa Clásica at
  positions 7–8 and American Express Clásica at 9; 2.2 moves it up to 7. Reproduced verbatim per screen —
  do not normalise them to each other.
- 2.2 writes Visa Light's membership as bare `0`, not `S/0` like every other row. Reproduced verbatim.

## Editing conventions

- **Keep every file that describes the work up to date, always, in the same pass as the change**
  (Marco, 2026-09-10). This is not optional and it does not wait to be asked. Nothing in this repo is
  generated: there is no build, no test suite and no sync script, so a document that contradicts the code
  is not a stale comment — it is the only description of the system, and it is now wrong. The moment a
  change lands, whatever describes it changes with it:

  | If you touched… | Update in the same pass |
  | --- | --- |
  | user-visible copy in `adobe-target/piloto/bot.html` | the owning `contenido/wordings/*.json` **and** `docs/correcciones-wording.md` if it was a correction |
  | card codes, names, affinity, casuistry | `contenido/tarjetas-catalogo.json` (ficha + every `casuisticasPorCriterio` entry) and its `_meta.pendiente_confirmar` |
  | the node tree, engine rules, namespace | the *State of adobe-target/piloto/bot.html* and *Engine changes* sections here |
  | `preview/index.html` | the *Commands* and *Testing casuistics* sections here |
  | anything Marco decided or left open | *Still open* here **and** Parte 6 of `docs/correcciones-wording.md` |

  Two specific duties that get forgotten: **delete facts that stopped being true** instead of only adding
  new ones (a resolved item left in *Still open* costs the next session real time re-raising it), and
  **carry the decision's date and author** — "Marco, 2026-09-10" — so a later reader can tell a decision
  from a guess. If a change makes a sentence here half-true, rewrite the sentence; do not append a
  contradiction next to it.

- **Every correction to Marco's copy goes into `docs/correcciones-wording.md`, in the same pass as the code
  change** (Marco, 2026-09-09). That file is a deliverable: he presents it to the UI team, and it is written
  so an AI can read it cold and explain it. Log what it said, what it says now, and the rule applied.
  **If he approves a correction without mentioning the file, add it anyway and tell him you did** — he
  asked to be reminded rather than have it silently skipped.
- **No template literals in the offer** (Marco, 2026-09-09). Build every string with `+` concatenation;
  never a backtick string or `${}`. `adobe-target/piloto/bot.html` currently has **zero** of either — keep it that way.
  Related ES6 that *is* still present and was inherited from `referencia/bot-actual.html`: five arrow functions in
  `validateGraph`/`getNode`, one `Array.prototype.find`, three `Number.isFinite`, one `Array.from`. Marco
  has not asked to remove those.
- Copy is Spanish (Peru), informal "tú". Keep emoji as HTML entities, not literal characters — Marco sends
  them as literal emoji and you convert. `&#128516;` (😄) is the one used after "¡Claro!".
- Adding a branch: append the node to `config.nodes`, follow the `qNNN` depth naming, wire `next` from the
  parent's `options`, and give every new leaf a temporary `[PENDIENTE]` stub with an "Ir al menú principal"
  option so `validateGraph` stays clean and the tree stays walkable.
- Keep CSS, markup, and script in one file and the namespace intact — the snippet is injected into a page
  it does not own.
- **Marco's copy is the source of truth.** Reproduce it verbatim, including what look like typos, and
  raise them separately instead of silently fixing. He has approved corrections this way before
  (`una tarjetas` → `una tarjeta`, `Ayudame` → `Ayúdame`) and rejected others (the greeting stays `Hola!`
  without the opening `¡`, matching the mockup, even though the launcher bubble has it).
- Verify every change in the running preview and report what was actually observed. Several bugs here were
  invisible in the code and obvious on screen: pipes rendering literally, a horizontal scrollbar, an
  animation that scaled a full-width row instead of the bubble.
