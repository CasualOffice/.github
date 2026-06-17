<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/profile/casual-office-mark-dark.svg">
  <img alt="Casual Office" src=".github/profile/casual-office-mark.svg" width="128" height="128">
</picture>

# Casual Office

**An open-source office suite you run on your own server.**

Spreadsheets · Documents · File manager — real-time co-editing, .xlsx / .docx round-trip,
no vendor lock-in.

[casualoffice.org](https://casualoffice.org)

</div>

---

## The suite

| Product | What it is | Status |
| --- | --- | --- |
| **[sheets](https://github.com/CasualOffice/sheets)** | Web spreadsheet with .xlsx fidelity and y-websocket co-editing. React + Univer + TypeScript on the front, Bun-based snapshot worker on the back. | live demo at [sheet.casualoffice.org](https://sheet.casualoffice.org) |
| **[docs](https://github.com/CasualOffice/docs)** | WYSIWYG .docx editor with ProseMirror schema and OOXML-preserving layout. Real-time co-editing via Yjs; Go gateway behind it. | live demo at [docs.casualoffice.org](https://docs.casualoffice.org) |
| **[drive](https://github.com/CasualOffice/drive)** | Self-hosted file manager that opens .xlsx and .docx inline in the editors above. Rust + Axum + OpenDAL + WOPI. | live demo at [drive.casualoffice.org](https://drive.casualoffice.org) |
| **[univer-revamp](https://github.com/CasualOffice/univer-revamp)** | Internal fork of the Univer Sheets engine with patches the sheets product depends on. | tracked upstream |

## What it replaces

If you've used Google Workspace or Microsoft 365 and want the same UX without the
data-handover, this is the same shape, self-hosted:

- **Sheets** → Google Sheets / Excel Online
- **Docs** → Google Docs / Word Online
- **Drive** → Google Drive / OneDrive

Every product opens the canonical Microsoft format directly (.xlsx, .docx) — no
proprietary container, no one-way export.

## How it's built

- **Frontend** — React 18/19, TypeScript, Tailwind / IBM Plex Sans + JetBrains Mono.
  Sheets is on Univer (forked at [univer-revamp](https://github.com/CasualOffice/univer-revamp));
  docs is a ProseMirror schema with a custom layout painter that preserves OOXML;
  drive is a Vite SPA embedded into a Rust binary via `rust-embed`.
- **Backend** — Go gateways for the realtime y-websocket / CRDT layer (docs).
  Rust + Axum for the drive control plane. Storage is pluggable per product
  (inline / WOPI / JWT-API host).
- **Realtime** — Yjs + y-websocket, with per-document rooms drained back to host
  storage on disconnect. The gateway holds no on-disk state.

## Self-hosting

Each product ships a Docker image and a docker-compose for the demo stack —
check that product's README for the current step-by-step. Drive is the
recommended entry point if you want one process that orchestrates the others.

## License

**Apache-2.0** across the suite — sheets, docs, drive, and the univer-revamp
fork. Permissive use; you can self-host and modify freely.

## Contact

- Web: [casualoffice.org](https://casualoffice.org)
- Issues + discussions live in each repo above.
