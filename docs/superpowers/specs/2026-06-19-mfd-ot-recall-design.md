# MFD Firefighter Overtime Recall System — Demo Prototype

**Date:** 2026-06-19
**Owner:** Tanner Mosher (HFFA Local 1463) / Frontline Web Designs
**Status:** Approved design — building

## Purpose

A standalone, working **demo/prototype** of a *fair* Firefighter Overtime (Rank-for-Rank)
Recall System for the **Maui Fire Department**, built to show MFD admin/management what a
proper tool looks like — and to work toward a real rollout.

The argument it makes: **MFD already signed the Rank-for-Rank Recall Policy (effective
Nov 1, 2025) but has no tool to run it.** Today's reality is manual — "relocate the 5th
person from another station, and if nobody can relocate, that person is stuck holding back."
This prototype delivers the equitable distribution and the county-wide cascade the policy
already promises.

## References (not to be mirrored slavishly)

- **HFD "Fire Fighter Overtime Recall System" User Guide** — the reference tool (Honolulu).
  Three processes: No-Bid (in-company direct fill), Bid (advance calendar), Day-Of (same-day
  availability). Homepage hour panel (Accrued/Worked/Remaining/Cap), bid status taxonomy
  (Pending/Override/Selected/Not Selected/Retracted), shift-details popup (station, apparatus,
  times), bid color legend, 4-day-off eligibility gate, text+email confirmations.
- **County of Maui Rank-for-Rank Recall Program, eff. 11/1/2025** — the legal architecture.
  288 hr / fiscal-year offered; 12 hr minimum if shift not fully covered; 48 hr / 0800
  scheduled notice; 30 min unscheduled notice; "Similar Function Capabilities"
  (Engine↔Engine, Ladder↔Ladder, Tanker↔Tanker, Hazmat↔Hazmat, Rescue↔Rescue, BC Aid↔BC Aid)
  **regardless of battalion or island**; in-company counterparts first, then county-wide
  first-come via First Due; **Hold-Back hours** (held for min staffing) do NOT count against
  the 288.

## Decisions (locked)

| Decision | Choice |
|---|---|
| Scope | Both sides — firefighter app + Battalion Chief console |
| Fairness rule | **Equity-first** (fewest fiscal-year recall hours wins) |
| Hero demo | "The stuck fix" — same-day unscheduled fill, county-wide, in seconds |
| Data | Anonymized badge IDs (e.g. `FF-1042`) on MFD's real 14-station map |
| Stack | Single self-contained `index.html`, vanilla JS, `localStorage`, no build |
| Location | `C:\Users\Tanne\mfd-ot-recall` (own git repo, outside OneDrive) |

## Architecture

Single `index.html`, CSS + JS inline (runs from `file://` or any static host). Internal modules:

1. **Seed data** — stations/companies/apparatus map + ~200 anonymized members.
2. **Schedule engine** — Kelly 9-day cycle `R,G,B,G,B,R,B,R,G`, anchored to match the real
   June 2026 platoon calendar; computes each platoon's on/off + the 4-day-off window.
3. **Eligibility + ranking engine** — the "brain" (see below).
4. **State store** — one object, auto-saved to `localStorage`; **Reset demo** restores baseline.
5. **View renderers** — hash-router; firefighter views + BC views; role switch in top bar.

Confirmations (text/email) are **simulated** as on-screen toasts + a notification log.
Accessibility: semantic landmarks, skip link, `:focus-visible`, labeled controls,
`aria-live` on results, `prefers-reduced-motion`, WCAG 2.1 AA contrast.

## Equity engine

**Eligibility (must pass all):**
- Similar-function: opening's apparatus class ∈ member's qualified classes
- Qualified flag for the position
- Available: scheduled → within member's upcoming 4-day-off window & off-duty;
  day-of → marked available that day & off-duty
- Under 288 (with the `<12 hr` remaining-balance rule)
- 34-hr continuous straight-time cap not breached
- Not on disqualifying leave (industrial/suspension), not suspended from program
- No double-booking

**Ranking:**
- Primary: ascending fiscal-year recall hours (fewest first)
- Secondary (visible tiebreaker): in-company tier — same truck → station → battalion → county-wide
- Each candidate row shows the **"why"**: hours, in-company badge, qualified, available, 34-hr OK

**Hold-Back hours** are a separate bucket and **never count against the 288** — the person who
got stuck holding back is credited, not penalized.

## Data model

- `Member { id:"FF-1042", rank, homeStationId, qualifiedClasses[], platoon(R/G/B), fyHours, holdBackHours, dayOfPrefs[], suspendedUntil? }`
- `Station { id, name, battalion, island, companies[] }`
- `Company { id, class(Engine|Tanker|Ladder|Hazmat|Rescue|BCAid), minStaffing }`
- `Vacancy { id, companyId, date, segment(full24|0730-1930|1930-0730), type(scheduled|unscheduled), status, assigneeId?, candidates[] }`
- `Assignment { memberId, vacancyId, hours, confirmedAt }`

Real map: Engine ×14, Tanker ×6 (Hāna / Lānaʻi / Molokaʻi remote), Ladder ×2, Hazmat ×1,
Rescue ×1, BC Aid ×6; plausible battalions/islands.

## Screens

**Firefighter app:** My Recall (home) · Opportunities (bid calendar) · Availability (day-of) · Profile
**BC console:** Command Center (today) · Fill a Vacancy (hero flow) · Create Opportunity ·
Equity Dashboard · Roster · Reports (CSV export)

## Hero flow — "the stuck fix"

Reset loads a June 2026 baseline: an Engine FF calls in sick (unscheduled), the station can't
backfill, today's answer = someone gets stuck holding back. The app shows the BC the
**county-wide equity-ranked eligible list**; the BC assigns the lowest-hours qualified Engine FF
from another battalion; confirmation fires; min staffing restored; the would-be-stuck member
goes home — with a **"Today vs. With the system"** side-by-side.

## Look & feel

Credible public-safety / command-software aesthetic — legible, organized, dark-capable, not
flashy. Restrained palette (deep navy/charcoal + one action accent + green/amber/red staffing
status), strong neutral type (Archivo). No logos.

## Out of scope (demo stage)

Real auth, multi-user sync, real SMS/email, a backend/database, county-IT integration. These
are the **rollout** phase (a Next.js + backend follow-on), for which the data model and equity
engine here are designed to port directly.

## Update 2026-06-19 — authenticity pass (from MFD's real First Due reports)

After reviewing MFD's actual daily *Schedule Report* and 7-day *Shift Board* (both First Due
outputs), the roster + terminology were rebuilt to match reality:

- **Real station map** (towns + islands): Wailuku (Engine, Mini), Pāʻia, Lahaina (Engine/Ladder/
  Tanker, Bn 2), Kaunakakai *(Molokaʻi, remote)*, Makawao, **Kīhei (Engine 6 — hero)**, Hāna
  *(remote)*, Lānaʻi *(remote)*, Hoʻolehua *(Molokaʻi, remote)*, Kahului (Engine/Tanker/**Hazmat**/
  **Rescue**, Bn 1), Nāpili, Pukoʻo *(Molokaʻi, remote)*, Kula (Engine/Tanker), Wailea (Engine/
  Ladder/Tanker). Apparatus totals: Engine ×14, **Tanker ×7**, **Mini ×1**, Ladder ×2, Hazmat ×1,
  Rescue ×1.
- **Ranks:** FF I, FF II, FF III, Captain, BC. Recall is **rank-for-rank** — a vacancy carries a
  rank *and* apparatus class, and eligible counterparts must match **both** (e.g. `Vacancy FF3`,
  `Vacancy LCAP` in First Due). Hero vacancy = **FF I · Engine** sick call at Kīhei.
- **Tiering:** same station → same island → county-wide (off-island), since the reports show only
  Bn 1 (Kahului) / Bn 2 (Lahaina) + remote. This also surfaces the policy's "regardless of island."
- **Terminology:** staffing ratios (on/required, e.g. `4/5`), Watch / **Remote-72** language, and
  real absence codes as vacancy reasons (`VAC, SL, FL, SA, SUBO`).
- **Discovery:** First Due is MFD's scheduling system of record (and the policy's §VI notification
  channel) — so this tool is framed as the *fair-recall complement* that fills the holes First Due
  surfaces, not a replacement.
- IDs stay **anonymized** (the reports contain real personnel).
