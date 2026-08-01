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

### 14:05 — New case fact from homeowner: basin is a drilled black barrel
- The sump basin is a black barrel with field-drilled holes — a DIY-grade, pit-only install, almost certainly with NO perimeter drain tile. This strengthens ranked cause #1: small drilled holes are the geometry most easily blinded by iron ochre/silt, and with no tile the system never collected beyond the barrel's local drawdown radius.
- Updated `index.html`: added a fifth "what we observed" fact card, folded the barrel evidence into cause #1, and rewrote diagnostic test 8 (no tile to camera-scope; restore = clean/ream or replace the basin, systemic = add real perimeter collection). Committed.
- Advised homeowner on direct clog checks: scrape holes for ochre gel, bucket-hold test (sealed-bucket behavior = blinded holes), and the ¼" slab test hole comparing standing water level vs. pit level (higher = disconnected = clogged).

### 14:15 — Homeowner field test: screwdriver through barrel holes
- Result: hard to push through (holes packed with dense material — silt/ochre cake or native soil pressed against the barrel), and no water flowed in afterward.
- Interpretation given: resistance = holes were not open (supports blinded-basin diagnosis). No inflow ≠ exoneration: silty soil seeps slowly (hours, not seconds), a blinding cake can extend into the soil beyond the hole, and hole depth matters — perched water sits just under the slab, so upper holes are the ones that would intercept it. Advised: inspect screwdriver tip for orange residue, clear multiple holes (especially upper ones), mark pit level and re-check over hours, then run the bucket-hold and ¼" slab test hole comparisons.

### 14:25 — Homeowner follow-up: cleared hole still admits no water; asked about cleaning beyond the barrel
- Advised (reach is inches, not feet — no way to clean soil at distance without a pipe to travel through):
  1. Ream ALL holes with a drill bit + bottle brush, not one screwdriver pass.
  2. Backflush each hole outward with a garden hose; hot-water soaks above the hole line to soften ochre gel (standard extension-service method). Orange return water = cake breaking up.
  3. Drill a fresh row of 3/8"–1/2" holes just below slab level — perched water sits at slab underside, and fresh holes at that elevation bypass the ochre-skinned lower wall. Accept minor silt intrusion as the cost.
  4. No muriatic acid in an enclosed pit (fumes/corrosion); mechanical + hot water only.
- Decisive overnight experiment defined: refreshed basin + marked pit level + ¼" test hole in wet zone. Pit fills → reconnected (repeat flush seasonally). Test hole stands while pit stays dry → sub-slab layer is disconnected from the barrel; fix is slab-level collection (wet-side partial French drain to this pit, ~$1,800–2,800, report Tier 3).

### 14:40 — Major field-evidence update: fresh holes dry + rusty hatchway upslope
- Homeowner drilled several NEW 3/8" holes in the barrel: no water entered → soil at the barrel is not saturated → groundwater-at-the-pit theories weakened.
- New case fact: an old rusty hatchway/bulkhead across the basement has leaked before; tiles near its door show discoloration; the slab GRADES toward the sump corner. Water entering there would travel on top of the slab under the glued flooring to the low point — pooling beside a pit that structurally cannot see it, picking up rust (orange tint!) from the steel.
- Revised the report: new co-leading cause card "Hatchway surface entry" (~55%), blinded-barrel collection failure demoted to 40%, water table to 30%; field-update callout added under the hero insight and in the causes section; chart + aria-label revised; new starred "do this first" test (stairwell drain check + garden-hose test on the bulkhead, $0, <1 hr); test 5 (¼" slab hole) rewritten to discriminate top-of-slab vs under-slab water; renumbered remaining cause cards.
- Fix path if hatchway confirms: stairwell drain cleanout ($0) / door re-seal ($20-100) / grading + downspouts / new bulkhead (~$1,500-2,500) — far cheaper than drainage retrofits.

### 14:55 — Stairwell inspection: NO drain, base still damp days after rain
- Homeowner inspected the bulkhead stairwell: no drain at the base of the steps; base still damp 3-4 days post-storm. Interpretation: the drainless stairwell is a storm-water holding tank whose only outlets are concrete joints and the gap under the door; still-damp days later = it held real volume and is discharging slowly — which also EXPLAINS the 1-3 day lag that had been the main argument for groundwater.
- Explained "silt lines" (bathtub-ring sediment marks from standing water) to the homeowner.
- Report updated: hatchway cause raised 55% → 70% ("LEADING"), evidence rewritten around the drainless-reservoir finding, starred test card updated with the inspection result + pre-rain mitigations (bulkhead cover/tarp, caulk door seams + sill joint, hydraulic cement inside once dry, downspout extension), chart aria-label revised.
- Remaining confirmation steps: interior threshold/sill inspection for rust wicking + seam-trail origin; hose test after things dry.

### 15:05 — Bulkhead inspection: rusted through with daylight visible → diagnosis effectively confirmed
- Homeowner reports the metal hatchway is rusted essentially entirely around the bulkhead, with rusted-through holes open to daylight. Combined with the drainless still-damp stairwell, dry fresh holes at the barrel, the graded slab, the seam trail, and the rust-orange ooze, the surface-entry diagnosis is effectively confirmed (85% in the report; formal proof = interior sill trail check / hose test / dry floor through a covered-bulkhead rain).
- Report updated to final form on this evidence: hero "key insight" rewritten to the landed conclusion (water rides on top of the slab from the hatchway; the pump guards the ground and the water never touches the ground; fix starts at the top of the stairs), cause #1 at 85% "EFFECTIVELY CONFIRMED", fact card updated, starred test card now records the inspection results + urgent tarp/cover guidance, Tier 1 gains "cover the bulkhead" ($20-150, highest-leverage dollar), Tier 2 gains "replace the bulkhead" (~$1,500-2,500, now the primary water-path repair).
- Advice to homeowner: bulkhead is beyond caulk/gaskets — replacement; tarp it before the next rain; flooring verdict and mold precautions unchanged.

### 15:15 — Flooring identified: LifeProof (Home Depot) — salvage path likely
- LifeProof = rigid-core SPC luxury vinyl: waterproof planks (no swelling/rot), and most LifeProof lines are CLICK-LOCK FLOATING, not glue-down — which conflicts with the earlier "glued down" description. Asked homeowner to check a plank edge at the wet seam (unclicks + no adhesive on slab = floating).
- If floating: planks salvageable — lift wet zone + 2 ft margin (number with painter's tape), wash/disinfect both faces, inspect attached cork/foam underlayment pad (replace planks with moldy/musty pads, ~$3-4/sq ft for those only), scrub + disinfect slab, dry to passing plastic-sheet test, reinstall only after the bulkhead is covered/replaced and the floor stays dry through one real rain.
- Non-negotiable either way: trapped water under vapor-tight vinyl never dries in place — "fix the leak and leave tiles down" is NOT an option; wet zone must come up; respirator/gloves/photos (assume mold after weeks wet). LifeProof warranty excludes standing-water damage.
- Report updated: green "LifeProof update" callout added above the flooring verdict; original glue-down verdict retained beneath it, conditioned on the planks actually being glued.

### 15:25 — Volume verification: can hatchway water travel 25 ft? Yes, 5-10× over
- New homeowner observation: seam seepage traces CONTINUOUSLY from the hatchway door ~25 ft to the sump room; hatchway end now dry, sump end still expressing water days after rain — the signature of a drained flow path with a terminal pool (and further confirmation the wet zone marks where water collects, not where it enters).
- Math: Jul 28-29 storm = 4.3" on a ~30 sq ft bulkhead footprint ≈ 80 gallons direct rainfall through the rusted-open doors (before any downspout/grade contribution). Wetting the whole 25 ft × ~8 ft trail as a 1/16" film under the flooring ≈ 8 gallons (1/8" film ≈ 16 gal). One storm supplies 5-10× the full-path requirement; the surplus tens of gallons form the standing pool at the low corner. Distance is trivial: waterproof floor over smooth slab = sealed tilted channel; 1/8"/ft grade = 3" drop over 25 ft.
- Report updated: fact card now records the continuous trail + drying gradient; cause #1 card gains the volume-check paragraph.

### 15:35 — Homeowner: tiles are not click-lock → glued LifeProof confirmed
- Glue-down verdict stands for the wet zone: tear out the hatchway→sump trail + 2-ft margin, scrape re-emulsified adhesive (heat gun + scraper, respirator), replace that strip only. Dry majority of floor stays — with the acute source being the hatchway (not groundwater), the rest of the glued floor faces only ordinary slab vapor once the bulkhead is fixed.
- Re-floor the strip with matching LifeProof, re-glued, only after: bulkhead replaced → slab cleaned + passes taped plastic-sheet test → ideally one dry rain cycle. Cost: ~$3-4/sq ft material DIY; $3-7/sq ft hired tear-out+install. Installer ASTM F710 letter still worth sending.
- Report: flooring-update callout rewritten (click-lock salvage path removed; conditional language dropped).

### Pending
- [ ] `gh auth login` (user action) — STILL NOT DONE; blocks publishing.
- [ ] Create public GitHub repo `basement-water-investigation`, push, enable Pages, verify live URL.
