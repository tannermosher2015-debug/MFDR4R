# MFD Firefighter Overtime Recall System — Demo

A standalone, working **demo/prototype** of a *fair* Firefighter Overtime (Rank-for-Rank)
Recall System for the **Maui Fire Department** — built for HFFA Local 1463 to show MFD
admin/management what a proper tool looks like, and to work toward a real rollout.

**The argument:** MFD signed the Rank-for-Rank Recall Policy (effective 11/1/2025) but has no
tool to run it. Today it's manual — relocate someone from another station, and if nobody can,
that person is *stuck holding back*. This prototype delivers the equitable distribution and the
county-wide cascade the policy already promises.

## Run it

It's a single self-contained file. Any of these work:

- **Double-click** `index.html` (opens in your browser, no server needed), or
- Serve it: `python -m http.server 4203 --directory .` then open <http://localhost:4203>, or
- Drop `index.html` on any static host (Netlify / Hostinger).

State is saved in the browser (`localStorage`). Use **↺ Reset demo** in the header to return to
the starting scenario at any time.

## The 90-second demo script

1. Open as **Battalion Chief** → **Command Center**. A Kīhei Engine sick call is open for today
   and flagged **"Hold-back risk."**
2. Click **Fill fairly →**. The system shows every eligible counterpart **county-wide**, ranked
   by **fewest recall hours first**, each row showing *why* (qualified, 4-day off, 34-hr OK,
   proximity). Note the same-station member near the cap ranks *below* lower-hours people elsewhere.
3. Click **Assign** on the top candidate → confirmation fires and the **"Today vs. With the
   system"** payoff appears.
4. Switch **Viewing as → a firefighter** to show their side: hours vs the 288 guarantee,
   hold-back hours tracked separately, the bid calendar gated to their 4-day-off window.
5. Back as BC → **Equity Dashboard** shows overtime distribution leveling across all members.

## What's modeled

- Kelly 9-day rotation (`R,G,B,G,B,R,B,R,G`), anchored to the real June 2026 platoon calendar
- Similar-function matching (Engine↔Engine, etc.) regardless of battalion/island
- 288-hour fiscal-year guarantee + the `<12 hr` remaining-balance rule
- 34-hour continuous straight-time cap
- Hold-back hours tracked separately (never counted against the 288)
- In-company → battalion → island → county-wide priority tiers (as a visible tiebreaker)
- Scheduled (advance/bid) vs unscheduled (day-of) paths
- Simulated text/email confirmations

## Status

Demo prototype with **anonymized** data on MFD's real 14-station map. Not connected to any live
system. Real auth, multi-user sync, real SMS/email, and a backend are the **rollout** phase — the
data model and equity engine here are written to port directly to that.

See `docs/superpowers/specs/2026-06-19-mfd-ot-recall-design.md` for the full design.
