<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/CasualOffice/.github/main/profile/casual-office-mark-dark.svg">
  <img alt="Casual Office" src="https://raw.githubusercontent.com/CasualOffice/.github/main/profile/casual-office-mark.svg" width="128" height="128">
</picture>

# Casual Office

**An open-source office suite you run on your own server — and the engines underneath it.**

Spreadsheets · Documents · Slides · PDF · Desktop — real-time co-editing,
`.xlsx` / `.docx` / `.pptx` round-trip, no vendor lock-in.

[casualoffice.org](https://casualoffice.org)

</div>

---

## The apps

These are the things you deploy and open. Every one of them treats the file as the
source of truth: open the real format, edit it, save it back.

| Product | What it is | Status |
| --- | --- | --- |
| **[sheets](https://github.com/CasualOffice/sheets)** | **Casual Sheets** — web spreadsheet with `.xlsx` fidelity and real-time co-editing. React + a Univer fork + TypeScript, with pivot tables, charts, and pluggable storage. | live at [sheet.casualoffice.org](https://sheet.casualoffice.org) |
| **[docs](https://github.com/CasualOffice/docs)** | **Casual Docs** — WYSIWYG `.docx` editor on a ProseMirror schema with an OOXML-preserving layout painter. Co-editing via Yjs behind a ~120-line Go gateway. | live at [docs.casualoffice.org](https://docs.casualoffice.org) |
| **[slides](https://github.com/CasualOffice/slides)** | **Casual Slides** — `.pptx` editor with deep OOXML fidelity, an Office-style ribbon, layouts and themes. Currently paused while Docs and PDF reach v1. | paused · no live demo |
| **[pdf](https://github.com/CasualOffice/pdf)** | **Casual PDF** — high-fidelity PDF viewer and editor: annotate, fill forms, edit text in place, e-sign (PKCS#7), and redact at the byte level. One PDFium engine across web and desktop. | v1 in progress · [pdf.casualoffice.org](https://pdf.casualoffice.org) |
| **[desktop](https://github.com/CasualOffice/desktop)** | **Casual Desktop** — Tauri binaries for macOS, Linux and Windows that reuse the web cores verbatim. Offline, single-user, with an on-device agentic AI assistant. | v0.0.6 |
| **[RASystem](https://github.com/CasualOffice/RASystem)** | **Casual RAS** — a white-label, embeddable remote-access platform. Vendors embed it for secure screen viewing, remote control, collaboration and approved support actions under their own brand. P2P over iroh/QUIC. | alpha · [ras.casualoffice.org](https://ras.casualoffice.org) |

## The engines

A second generation, written from the ground up in Rust: deterministic, embeddable, and
compiled to WebAssembly so the engine runs in a browser tab with no server behind it.

**These are successors, not renames.** Casual Sheets and Casual Docs still ship and still
run the demos above; the engines are where new work happens and are not yet drop-in
replacements for the apps.

| Project | What it is | Status |
| --- | --- | --- |
| **[opencalc](https://github.com/CasualOffice/opencalc)** | **OpenCalc** — an embeddable spreadsheet engine. Reads and writes `.xlsx` (plus CSV/TSV/PSV), calculates 347 of the 356 spec functions including `LET`, `LAMBDA` and dynamic arrays, and paints a WASM canvas grid. Server-mediated OT, so co-editing needs no sticky sessions. Ships as `@opencalc/sheet` on npm, as a Rust crate, and behind WOPI. Successor to `sheets`. | alpha · [calc.casualoffice.org](https://calc.casualoffice.org) · [live editor](https://calc.casualoffice.org/editor.html) |
| **[opendoc](https://github.com/CasualOffice/opendoc)** | **OpenDoc** — a deterministic Word-document engine. Reads and writes `.docx`, holds the document in a normalized editable model, and lays it out and renders it to pixels for native, WebAssembly and headless hosts — no browser, server or UI framework required. Successor to `docs`. | pre-release · [opendoc.casualoffice.org](https://opendoc.casualoffice.org) · [live editor](https://opendoc.casualoffice.org/editor.html?demo=1) |
| **[core](https://github.com/CasualOffice/core)** | A pure-Rust engine that reads, writes and converts office documents — DOCX, ODT, PDF, Markdown, plain text — with WebAssembly and C FFI bindings. | active |

Two principles run through both engines: **nothing is lost silently** (unmodelled parts of a
file are kept byte-for-byte, and what genuinely cannot be kept is reported rather than
dropped), and **determinism is a gated contract** (time and randomness are supplied to the
engine, never sampled by it, so a session is replayable and a render is regression-testable).

## The platform

| Project | What it is | Status |
| --- | --- | --- |
| **[enclave](https://github.com/CasualOffice/enclave)** | **Enclave** — a self-hostable enterprise content and collaboration platform, SharePoint-class in purpose. Content, identity, permissions, governance, search and MCP-native AI access behind **one policy boundary**: every entry point — web, sync, mobile, REST, MCP, agent, preview, download, search, webhook — runs the same authorization chain, implemented once. Rust + Axum + PostgreSQL; React + TypeScript. | in development · no hosted demo |

## Shared infrastructure

| Project | What it is | Status |
| --- | --- | --- |
| **[collab](https://github.com/CasualOffice/collab)** | Real-time collaboration server for the self-hosted editors — Hocuspocus + Yjs with auth, WOPI, pluggable storage, snapshots and versioning. Stores opaque OOXML. | active |
| **[TaskForge](https://github.com/CasualOffice/TaskForge)** | Self-hostable work tracker in Rust whose core stays small — explainable permissions, configurable workflows over five permanent states, and a closed plugin registry you never fork to extend. | in development |
| **[univer-revamp](https://github.com/CasualOffice/univer-revamp)** | Internal fork of the Univer Sheets engine, carrying the patches Casual Sheets depends on. | tracked upstream |

## What it replaces

If you have used Google Workspace or Microsoft 365 and want the same shape without the
data-handover, this is that, self-hosted:

- **Casual Sheets / OpenCalc** → Google Sheets · Excel Online
- **Casual Docs / OpenDoc** → Google Docs · Word Online
- **Casual Slides** → Google Slides · PowerPoint Online
- **Casual PDF** → Acrobat · PDF SaaS editors
- **Enclave** → SharePoint · enterprise content management

Every product opens the canonical Microsoft format directly — no proprietary container, no
one-way export.

## How it's built

- **Rust** — the engines (OpenCalc, OpenDoc, `core`), the Enclave backend (Axum, SQLx,
  PostgreSQL), Casual RAS, and TaskForge. Compiled to `wasm32-unknown-unknown` where the
  engine needs to run in a browser tab.
- **TypeScript / React** — the shipping web editors and the Enclave SPA. Sheets is on a
  [Univer fork](https://github.com/CasualOffice/univer-revamp); Docs is a ProseMirror schema
  with a custom layout painter that preserves OOXML.
- **Realtime** — Yjs + Hocuspocus / y-websocket for the first-generation editors; a
  server-mediated OT protocol for OpenCalc, where any node relays to the document's leader so
  no sticky sessions are needed.
- **Desktop** — Tauri, wrapping the same web cores.

## Self-hosting

Each product ships a Docker image and a compose file for the demo stack — check that
product's README for the current step-by-step. OpenCalc gets you co-editing over a link with
two containers, and a cluster with one more compose file.

## Contributing — and joining

**Contributors are genuinely welcome here, and the door to the organisation is open.**
The whole suite is Apache-2.0 and built in the open: every design document, tracker row and
decision lives in the repository, so there is no private context you would need to be handed
before you could start.

**Getting started.** Bug reports, feature ideas and pull requests are all welcome. Pick a repo
above, read its `CONTRIBUTING` guide, and open a PR. For anything larger than a fix, open an
issue first so the approach can be agreed before you spend the evening on it. If you are not
sure where to start, say so — pointing someone at the right first task is not an imposition.

**Joining as a member.** You do not need to be invited to start, and you do not need years of
history to be asked in. The path is short and deliberately unceremonious:

1. Land a couple of merged pull requests on any repo — size matters far less than care.
2. Say you would like to join, in an issue or by email.
3. You get an invitation to the organisation.

Members get triage rights, a say in the direction of the project they work on, and the ability
to review others' work. Nobody is asked to commit to a schedule; people contribute at whatever
pace their life allows, and stepping back for a while is normal rather than a resignation.

**Just want to talk first?** Email **[sachin@casualoffice.org](mailto:sachin@casualoffice.org)** —
about contributing, about joining, about whether an idea is worth building, or about using any
of this at your company. Questions from people who have not written a line of code yet are
welcome too.

Security reports go to `security@casualoffice.org` rather than a public issue; code-of-conduct
concerns go to `conduct@casualoffice.org`.

## License

**Apache-2.0** across the suite. Permissive use; self-host and modify freely. Some
components preserve upstream MIT attribution where required.

## Contact

- Web: [casualoffice.org](https://casualoffice.org)
- Email: [sachin@casualoffice.org](mailto:sachin@casualoffice.org) — contributing, membership, or using this at work
- Issues + discussions live in each repo above.
