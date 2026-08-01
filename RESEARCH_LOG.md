# Basement Water Investigation — Work Log

Project: Water oozing up through glued-down laminate flooring in a finished basement (Weatogue/Simsbury, CT) while the sump pit stays relatively dry.
Deliverable: Research-backed analysis + fix plan, published as an HTML page on GitHub Pages (public repo).

---

## 2026-08-01

### 13:24 — Session start & setup
- User reported: sump pump installed a few years ago fixed prior water issues; spring 2026 heavy rain had pump running constantly with no flooding; now in summer the pit is relatively dry yet water oozes up through the laminate seams. Flooring recently installed (finished basement), being damaged.
- Attempted to read the 3 photos directly from the iMessage "Katie Beautiful" thread via the Messages database (`~/Library/Messages/chat.db`) — blocked by macOS privacy protection (TCC). Asked user to provide the photos another way.
- Checked GitHub CLI: **not logged in** — asked user to run `gh auth login` interactively.

### 13:26 — Clarifying questions asked & answered
- **Where:** water appears between the sump pump and ~25% of the way across the room (localized zone near the sump corner).
- **When:** squeezes up at plank seams under foot pressure; not clearly rain-dependent, but worse after recent rain; currently very humid.
- **Flooring:** glued down directly to the concrete slab.
- **Repo:** public (free GitHub Pages). Decision: keep the street address OFF the public page — refer only to "Weatogue/Simsbury, CT."

### 13:28 — Research workflow launched (multi-agent fan-out)
Launched background workflow `basement-water-investigation` (run ID `wf_73be4a3e-b03`) with 6 agents:
1. **local-hydrology** — Simsbury/Weatogue rainfall (July 2026), Farmington River valley water table, glacial soils, groundwater lag after a wet spring.
2. **differential-diagnosis** — every mechanism that puts liquid water under glued flooring with a dry pit: hydrostatic pressure via slab cracks, capillary rise, vapor drive/condensation, plumbing/AC leak, sump discharge recirculation, clogged drain tile (incl. iron ochre); each with a cheap at-home confirmation test; ranked.
3. **sump-system** — the dry-pit paradox: water table sitting between slab underside and pit intake, drain tile clogging, float switch set-points, discharge line failures.
4. **flooring-forensics** — glued-down flooring on below-grade slabs: moisture limits (ASTM F710/F2170/F1869), adhesive failure, mold timeline, salvageability, appropriate replacement flooring.
5. **remediation-costs** — 3-tier fix plan (triage / targeted repair / systemic) with CT-area 2025–26 pricing; insurance and installer-responsibility angles.
6. **lead-synthesis** — adversarial synthesis: ranks causes with evidence for/against, ordered diagnostic tests, action plan, flooring verdict.

### 13:30–13:33 — Photos recovered & analyzed
- User pasted direct file paths to the 3 HEIC attachments; the attachment *files* were readable even though the Messages database was not. Copied to scratchpad, converted HEIC → PNG with `sips`, and analyzed:
- **IMG_9888 (floor seam close-up) — KEY EVIDENCE:** ooze at plank seams is **orange/rust-brown, not clear** → consistent with **iron ochre** (iron bacteria), very common in CT groundwater. Points to groundwater through the slab (not condensation, not supply plumbing, not AC condensate — those run clear). Iron ochre is also notorious for clogging perimeter drain tile — would explain water bypassing a dry pit.
- **IMG_9887 (utility closet, sump/pump area):** white **efflorescence** and damp staining on exposed slab near the pump — evidence of chronic moisture migration up through the concrete in exactly the failing zone.
- **IMG_9885:** blurry close-up of a textured white surface (likely parged foundation wall); minor staining lower-left; not independently diagnostic.
- Plan: feed photo findings into the lead-synthesis agent (re-run synthesis with cached research via workflow resume) so the rankings reflect the orange ooze.

### 13:35 — Research workflow completed (6/6 agents, ~180k tokens, 63 web lookups, 6.6 min)
Ranked causes from the adversarial synthesis (before seeing the photos — the orange ooze photo independently corroborates #1):
1. **65% — Clogged/iron-ochre-fouled drain tile** (or pit-only install): perched sub-slab water can't reach the pit, exits up through the cut-and-patch ring around the sump basin. Only mechanism explaining all 4 case facts at once. **Key insight: with the Farmington River still at 2.5× baseline on Aug 1 (USGS Tariffville gauge: 250→1,410 cfs peak Jul 29-30, ~640 cfs Aug 1), a properly connected pit should NOT be dry — the dry pit is itself evidence of collection failure.**
2. **45% — Elevated valley water table sitting between slab underside (~4") and float trigger (~18-24")** — "relatively dry" pit may be an eyeball artifact. Likely co-driver with #1.
3. **25% — Sump discharge line failure** recirculating pumped water beside the foundation at that exact corner. Free 15-min test; must rule out.
4. **10% — Plumbing/AC condensate leak** — rain correlation argues against, but $5 dye/meter tests first because cost-of-missing is high.
5. **35% chronic — Capillary/vapor drive through unmitigated slab**, trapped by the vapor-tight glued floor. Not the acute source (order of magnitude too small), but why the assembly fails visibly and why glue-down will fail again.
Local context: July 2026 exceptionally wet (4.30" at Bradley Jul 28-29 + 2-3" Jul 31); Weatogue sits on ~90 ft of transmissive glacial outwash tied to the river; Aug 1 dew point ~66°F → slab cannot dry by ventilation (dehumidifier mandatory, windows closed).
Flooring verdict: wet-zone flooring is unsalvageable (adhesive re-emulsification; assume mold — water likely present since spring; IICRC S500 requires lifting it). Installer angle: ASTM F710 requires slab moisture testing before glue-down — send written request for their F2170/F1869 records; expect a negotiated contribution, not full recovery.
8 diagnostic tests sequenced ($0-$10 each before any contractor spend); 3-tier action plan ($300-900 triage → $500-4,500 targeted → $5-6k systemic if needed).

### 13:38 — Report build started
- Extracted synthesis + research JSON from workflow output; 131 sources collected across 5 researchers.
- Loaded dataviz design skill for the likelihood chart / stat tiles.
- Sanitized photos for the PUBLIC repo: re-encoded via PIL with **all EXIF/GPS metadata stripped**, downscaled to 1600px → `photos/seam-ooze.jpg`, `photos/utility-closet.jpg`, `photos/wall-closeup.jpg`.
- gh CLI still not authenticated — building everything locally, ready to push.

### 13:50 — Report page built and committed locally
- Wrote `index.html`: self-contained report (no external dependencies) — hero + key insight, 4 case facts, 3 photos with evidence captions, local hydrology stat tiles (4.30" storm / 5.6× river spike / 2.5× still-elevated / 66°F dew point), likelihood bar chart, 5 ranked-cause cards with evidence for/against, SVG cross-section diagram of the dry-pit mechanism, 8-step diagnostic test sequence, 3-tier fix plan, flooring verdict (incl. installer ASTM F710 angle + insurance), curated sources, method disclosure + disclaimer. Light + dark mode.
- Chart colors validated with the dataviz palette validator: ALL CHECKS PASS in both light and dark modes.
- Wrote `README.md`. Initialized git repo on `main`, committed everything.
- No street address anywhere in the published content; photos re-encoded with EXIF/GPS stripped.

### Pending
- [ ] `gh auth login` (user action) — STILL NOT DONE; blocks publishing.
- [ ] Create public GitHub repo `basement-water-investigation`, push, enable Pages, verify live URL.
