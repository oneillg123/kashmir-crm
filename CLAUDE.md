# Kashmir CRM — Project Context for Claude

## Who I Am
**Griffin O'Neill** — DJ (performs as BRIFFEY), RevOps background, co-founder of The Dig booking agency.

## The Two Projects This Repo Serves

### 1. Kashmir (Venue)
Kashmir is a Chicago music venue/night that Griffin manages. It runs recurring events with a fixed 2-slot format:
- **Opener:** 10:00 PM – 12:00 AM
- **Headliner:** 12:00 AM – 2:00 AM

The CRM was built to manage Kashmir's operations: booking artists, tracking nights, managing the guest list, and tracking payments to artists.

### 2. The Dig (Booking Agency) — context only, not in this repo yet
A boutique DJ booking agency co-founded by Griffin (BRIFFEY) and Andy Wolf (DJ Woolf). Positioning: "DJs who book DJs." Three revenue channels: club bookings, large-scale events (via major Chicago promoter connection), and brand activations (via event coordinator contact — higher budgets). Not built yet, referenced for future planning.

---

## This Project: Kashmir CRM

### What It Is
A single-file HTML CRM (`index.html`) for managing Kashmir venue operations. No build step, no framework, no dependencies beyond Google Fonts. Designed to be hosted on GitHub Pages with Supabase as the database backend.

### Current Status
- **CRM is fully built** as a standalone HTML file
- **Supabase integration is in progress** — tables created, credentials pending
- **Hosted on GitHub Pages** (or will be shortly)
- localStorage used as fallback during development

### Tech Stack
- **Frontend:** Single HTML file — vanilla JS, CSS custom properties, Google Fonts (DM Mono + Syne)
- **Database:** Supabase (Postgres via REST API) — free tier
- **Hosting:** GitHub Pages
- **No build process, no npm, no framework**

### Design System
- Dark theme: `--bg: #0c0c0d`, `--surface: #141416`, `--accent: #c8f060` (lime green)
- Fonts: Syne (headings, 800 weight), DM Mono (body, monospace)
- Inline editing everywhere — click any field to edit in place
- Drag and drop for scheduling artists into lanes
- Consistent badge system for statuses

---

## Data Model

### Artists
```
id, name, timeslot (Headliner|Opener|Flexible|Unassigned),
status (Prospect|Contacted|Negotiating|Confirmed|Cancelled),
ig, phone, email, fee (standard), contract (N/A|Pending|Signed),
repName, repContact, owner (Griffin|Andy|Venue Staff|External),
notes
```

### Nights
```
id, name, date, status (draft|upcoming|past),
notes, cover (ticket price),
slots: [{id, lane (opener|headliner), isOpen, openNote, artistId, fee, payment, rating, perfNote}],
guests: [{id, name, count, vip, addedBy, notes}]
```

### Supabase Tables
- `artists` — columns: id (text PK), data (jsonb), updated_at
- `nights` — columns: id (text PK), data (jsonb), updated_at
- `guests` — columns: id (text PK), night_id (FK), data (jsonb), updated_at

---

## Features Built

### Dashboard (homepage)
- Next upcoming night hero card with countdown
- Stat cards: upcoming nights, open slots, guests on list, unpaid fees
- Upcoming nights list, action needed column (open slots + unpaid past nights)

### Nights
- Grid of nights filterable by upcoming/past
- Night detail overlay: lineup scheduling (drag & drop), payment summary, guest list
- Inline editing for name, date, status

### Roster
- 107 Chicago DJs pre-loaded
- Sortable, filterable table (status, timeslot, rep, owner)
- Artist drawer with full edit form: booking details, rep/agent info, contact, play history
- Search finds by name, IG handle, or rep name

### Scheduling
- Drag artists from sidebar into Opener/Headliner lanes
- Drag between lanes to swap
- Mark lanes as open slots (generates tasks)

### Guest List
- Per-night guest management: name, party count, VIP flag, added by
- Intake form mock-up UI (not wired to Google Form yet)
- Guest list data surfaces in global search

### Roles (simulated, no real auth yet)
- **Owner** (Griffin/Andy): full access including payments and deletes
- **Manager**: edit nights, lineup, guest list — payment data hidden
- **Read Only**: view only, no editing

### Payments (owner only)
- Per-slot fee + payment status (Unpaid/Partial/Paid/Waived)
- Payment summary inside night detail view
- Total booked vs total paid
- Unpaid fees surface in dashboard

### Save/Load
- Auto-saves to localStorage every 800ms after any change
- 💾 Save button downloads JSON backup + writes localStorage
- ↑ Load button restores from JSON backup file
- Supabase integration: IN PROGRESS

### Global Search
- Searches artists, nights, and guests simultaneously
- Results grouped by type, click to navigate directly

### Tasks
- Auto-generated from open slots
- Shows night name, date, link to go schedule

---

## Artist Roster
107 Chicago DJs pre-loaded. Key names:
- BRIFFEY (Griffin, id 18) — Headliner, Owner: Griffin
- Pinto (id 1) — Headliner, Owner: Andy
- Deep State Disco (id 3) — Headliner, Owner: Griffin
- Alex Wolf (id 24) — Flexible, Owner: Andy

---

## What's NOT Built Yet
- **Supabase live sync** — credentials needed from Griffin to wire up
- **Real authentication** — currently role is just a toggle, no login
- **Google Form → Guest List sync** — mock-up only, needs Sheet URL
- **The Dig agency CRM** — separate project, not started

---

## Preferences & Patterns
- **No frameworks** — keep it a single HTML file
- **Inline editing** preferred over modals where possible
- **Auto-save** everything — users shouldn't have to think about saving
- **Dark aesthetic** — don't touch the color palette
- **DM Mono for body text, Syne for headings** — don't change fonts
- **Drag and drop** for scheduling — must be preserved
- **Role-based visibility** — payments always hidden from non-owners
- Keep CSS compact, prefer CSS custom properties for theming
- JS: vanilla only, no libraries

---

## Repo Structure
```
index.html        ← entire app lives here
CLAUDE.md         ← this file
```

---

## Next Steps (as of March 2026)
1. Griffin sends Supabase project URL + anon key → wire up live database
2. Add real Supabase Auth (email/password login, roles stored in DB)
3. Wire Google Sheet guest list URL for auto-import
4. Deploy to GitHub Pages at custom URL
5. Eventually: build The Dig agency CRM as separate project
