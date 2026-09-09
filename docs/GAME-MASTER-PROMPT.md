# MASTER PROMPT — "Pay & Display"
## An interactive 3D game exposing parking surveillance · Kaarana × CashlessConsumer

> Paste this whole document as the build prompt for any capable AI agent or dev team.
> It encodes the design, the evidence base, the ethics rules, and the acceptance criteria.
> The reference implementation lives at `/parkwise-game` (Three.js route on zo.space).

---

## 1 · ROLE

You are building a playable counter-narrative. Smart parking is sold as convenience:
green bays, QR codes, ₹40/hr, "smart city". Your job is to make the player **feel**
the data extraction hiding under that story — not through exposition, but through
game mechanics. The player must *experience* being turned into columns in a ledger.

## 2 · SOURCE MATERIAL (ground every mechanic)

- **API spec**: `Kaarana/data/parkwise/parkwise-public-api-openapi.json` — every
  field the game "collects" must exist in this spec. No invented fields.
- **Harm pages**: `/parkwise-demo/anpr`, `/vahan-chain`, `/who-else`,
  `/breach-ledger`, `/pattern-of-life` — each game day ends by demonstrating one.
- **Report**: `/parkwise-report` — the economic argument the game dramatizes.
- **Evidence rules**: real incidents (ParkMobile 21M users incl. plate numbers,
  2023 EasyPark/RingGo breach, VAHAN-chain disclosures, Nashik ANPR tender) are
  facts; everything player-specific is clearly *fictional simulation*.

## 3 · DESIGN PILLARS (violating any = rebuild)

1. **The collection is visible at the moment of payment.** When the player parks,
   labeled data orbs (geo, plate, phone, UPI instrument, arrival…) physically fly
   from the car to a tower. The receipt is never hidden — that's the point.
2. **No shield fantasy.** There is no privacy-optimal path. Cash skips the phone
   but the plate + VAHAN re-identifies. Circulating to avoid the app triggers MORE
   ANPR reads and an "evasive loitering" flag. Every choice costs.
3. **Mechanics carry the argument.** Choices (app vs cash, extend vs circle)
   change orb counts, buyer cards, and consequences — not cutscene text alone.
4. **Dual view.** TAB toggles Convenience view (sunny city) and Surveillance view
   (wireframe city, movement trail, camera cones). Same street, two truths.
5. **Label every inference.** Buyer cards say "inference, not verdict". Never
   show a real person, plate, or account. The car's plate is a fabricated sample.
6. **Nothing leaves the browser.** No network calls, no analytics, no accounts.
   The game about surveillance must not surveil.

## 4 · GAME SPEC

**Structure**: 5 days, one curb, escalating extraction.

| Day | Mechanic | Lesson (buyer card) |
| --- | --- | --- |
| MON | Forced app pay — baseline 6 orbs | ULB dashboard works; columns invisible |
| TUE | Choice: app (6 orbs) vs cash kiosk (4 orbs) | Insurer repricing by curb geography |
| WED | Choice: extend (3 orbs) vs circulate (ANPR flag orbs) | HR screening scores "discipline" |
| THU | Kiosk offline — forced app, 9 orbs | Location broker infers profession |
| FRI | ANPR poles read the plate WHILE DRIVING | Merged file: presence score, resold |

**Core loop per day**: intro card → drive (WASD/arrows, touch on mobile) →
reverse into the highlighted bay (hold 1.2 s to confirm) → payment/choice modal →
collection cutscene (orbs, counter ticks) → buyer interstitial → next day.
After day 5: **the receipt** — total data points, choices made, ANPR reads while
driving, and what the merged file enables.

**Scene**: one street (two lanes, dashed center), procedural buildings both
sides, 6 curbside bays (3 occupied), P₹40/hr sign, 2 ANPR poles with visible
cones, data tower with pulsing ring + beacon, 2 pedestrians, chase camera.

**Failure is impossible; the game is unwinnable by design** — the only "score"
is how much of you got collected. Show this sentence in the receipt screen.

## 5 · TECHNOLOGY CONSTRAINTS

- Three.js via pinned `esm.sh` import (`three@0.170.0`), single React page route,
  no build step, no npm install (zo.space fixed-dependency model).
- Low-poly primitives only (boxes, cylinders, capsules, sprites with CanvasTexture
  labels). Chase-cam lerp. PixelRatio ≤ 2. Dispose on unmount.
- HUD as React overlays; engine ↔ React via a state-push callback.
- Controls: keyboard (WASD/arrows, TAB view toggle, R reset) + on-screen touch
  buttons for mobile.
- Expose a `window.__pw` debug hook for automated testing.

## 6 · TONE & LANGUAGE

Kaarana voice: claims → realities → rationale. Indian cities and rupee amounts.
Plain, sharp sentences. Never sneering at the player or the parking attendant —
punch at the architecture, not people. Player is never punished with difficulty;
the punishment is knowledge.

## 7 · ACCEPTANCE CRITERIA

1. Fresh load → intro → all 5 days playable → receipt, zero console errors.
2. TAB toggle visibly changes the city and reveals the movement trail.
3. Every orb label matches a field name in the OpenAPI spec.
4. Cash choice yields fewer orbs AND still produces a buyer consequence.
5. Day 5 produces ANPR reads before the player parks.
6. Mobile viewport: touch controls usable, HUD not clipped.
7. Network tab: zero outbound requests after page load (except fonts/three CDN).

## 8 · EXTENSION ROADMAP (v2+)

- **Platform Mode**: play as the operator — bid for data, price the premium,
  watch retention drop when the privacy policy is honest.
- **Second city**: same week, different curb, different bylaws — same ledger.
- **Vernacular**: Kannada / Tamil / Hindi UI + narration tracks.
- **Ghost mode**: replay your run as the broker, watching your own trail.
- Export "receipt" as a shareable image card linking to the report.

---

*Fork line: API spec → demo simulator → 5 harm pages → this game → the report.
One evidence bench, five surfaces. Build whichever your audience will actually touch.*
