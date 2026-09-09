# Pay & Display: A Counter-Narrative on Smart Parking

**Kaarana (ಕಾರಣ) × CashlessConsumer · 2026-09-09 · [DPI]**

*Companion artefacts: [play the game](/parkwise-game) · [run the live API simulator](/parkwise-demo) · [the five privacy harms](/parkwise-demo/anpr) · [the API spec](/parkwise-demo/spec)*

---

## 1. The story we are sold

The smart-parking pitch is uniform across every Indian city procuring one: parking is scarce, drivers circle and burn fuel, and a platform with sensors, an app and digital payments will guide you to a free bay in seconds. The ULB gets occupancy dashboards and cleaner footpaths; the citizen gets convenience; the vendor gets a management fee. Everyone wins, nothing is lost.

This narrative is not false so much as **framed by omission**. It describes the transaction's front-end — find bay, pay, park — and says nothing about the transaction's back-end: what is recorded, who holds it, for how long, and what else it can be joined to. The omission is structural, because the back-end is where the platform's actual asset lives.

We tested the omission against the industry's own document: a public OpenAPI specification for a city parking platform ("ParkWise", modelled on the Beckn-style BAP integrations and ULB-governed zone deployments now common across Indian smart cities). Everything below is grounded in that spec and in documented incidents. Where we reason beyond it, the text says *inference*.

## 2. What the machine actually records

The spec defines seven endpoints. Read them as a ledger rather than as an API, and the back-end comes into focus:

| Step | Endpoint | What it writes down |
| --- | --- | --- |
| Find | `POST /search` | Your latitude/longitude (to the metre), search radius, vehicle type — and it answers with distance to every zone |
| Choose | `POST /select` | The zone you considered, with price estimates — an *interest signal* even if you never book |
| Book | `POST /init` | Vehicle plate, vehicle type, zone, slot, duration, phone, email, amount |
| Pay | `POST /confirm` | Session ID and payment status via a hosted payment rail (Juspay-style links) |
| Linger | `GET /status` | Remaining minutes — the platform and app agree on where your plate is, until when |
| Change plans | `POST /update` | The extension, the new end time — *that your plans changed* |
| Leave | `POST /cancel` | Actual exit time, duration parked, final amount — your departure, exactly |

One 45-minute park therefore generates, at minimum: **a precise location fix, a state-assigned vehicle identifier, a phone number, an email address, exact arrival and departure times, and a payment trail**. The convenience app displays a zone name and a countdown. The platform's ledger holds a person's location, vehicle, contact and payment history, retained on the platform's clock, under a contract the public has not read. That asymmetry — *the receipt you can't see* — is the real product.

## 3. The economics: paying twice

The convenience story prices parking as a fee. The ledger story prices it as a **fee plus data rent**. The motorist pays money and simultaneously produces behavioural data — an unpaid by-product of living in a city — which is appropriated, refined and resold or leveraged downstream. In classical terms: the parker performs unpaid data labour; the platform captures it as surplus. Who buys the surplus?

- **The ULB**, legitimately, for occupancy and enforcement — this is the advertised use.
- **Payment institutions**, who see payer and instrument per transaction.
- **Third-party BAP apps**, who by the spec's own words "search parking, book sessions and manage payments" — strangers in the loop by design, each with an API key and a contract.
- **Unknown downstream**, bounded by nothing in the spec: no retention schedule, no deletion endpoint, no purpose limitation, no sub-processor list appears anywhere in the API document. The blast radius is a *policy choice*, made in a contract, in private.

Note what the spec's convenience framing conceals: the fields that enable the deepest harms are the ones the app could least justify. Phone and email are **optional in the spec** — no payment rail actually requires them — yet every real integration collects them, because the platform's data asset is worth more when it is person-linked.

## 4. It has already happened

The counter-narrative is not speculative. Each harm we simulate has a documented precedent:

**Parking ledgers leak, and they leak plates.** In April 2021, ParkMobile — the parking app of numerous US cities — lost data on roughly 21 million users: emails, phone numbers, **license plate numbers**, mailing addresses and hashed passwords.[^1][^2][^3] In December 2023, EasyPark Group (RingGo, ParkMobile Europe) reported a breach exposing names, phone numbers, addresses, emails and parts of card numbers, reported to the UK ICO.[^4] A breach of parking data is not card fraud risk; it is a *presence history* — where someone was, when, how to reach them.

**The plate is one join from your name.** India's VAHAN register holds registration records for on the order of 28–40 crore vehicles, retrievable by plate; public lookups now mask owner names, but SMS lookup services, third-party "RC check" ecosystems and enforcement channels keep the chain alive.[^5][^6] Plate-linked data already powers India's e-challan phishing industry, where fraudsters use plausible vehicle details to defraud motorists.[^7] A parking ledger that logs plate + place + time is the missing column in that spreadsheet.

**Cameras complete the network.** Nashik's smart parking — 4,500 slots, 22 locations — was explicitly built with "sensors and cameras for data collection".[^8] Once enforcement reads plates by camera, every park becomes a camera read with a name attached. The UK's ANPR trajectory — a policing tool grown into a network capable of tens of millions of reads per day, with vehicle-movement tracking envisaged in strategy documents — shows where the on-ramp leads.[^9]

## 5. The policy floor

India now has the legal scaffolding. The DPDP Rules, 2025 (notified 13 November 2025, G.S.R. 846(E)) operationalise the 2023 Act in phases: consent-manager provisions by November 2026, full operational compliance — notices, breach reporting, retention, security safeguards — by **May 2027**.[^10][^11][^12] Parking systems being procured *now* will run for years, so the test is not "does the vendor comply by 2027" but "what did the city write into the contract before the deadline made it unavoidable."

A defensible smart-parking concession says, at minimum:

1. **Minimisation** — session tokens, not phone numbers; plate stored only for the billing period; optional fields not collected by default.
2. **Retention with a clock** — per-field expiry; erasure machinery (an endpoint and an SLA), not just a policy page.
3. **Purpose limitation in text** — no VAHAN joins, marketing analytics, or broker resale without separate legal basis and public disclosure.
4. **Breach duty** — CERT-In-empanelled audit, contractual notification timelines, and citizen notification.
5. **Aggregate as open data** — occupancy and revenue published as open municipal data; person-level data never leaves the purpose it was collected for.
6. **Public DPIA and contract** — the concession agreement and the in-app consent notice published alongside the tender.

None of these six lines exist in the spec we examined. That absence is the finding.

## 6. Why we built a game

Reports inform; mechanisms persuade. The failure of privacy discourse is that it describes harms in the abstract — "your data is collected" — while the convenience layer shows you a green bay and a paid checkmark. So we built the counter-experience:

**[PAY & DISPLAY](/parkwise-game)** is a 3D parking game (Three.js, runs in the browser, nothing leaves your machine). You park a yellow hatchback on the same street for five in-game days. Each session, tagged data orbs fly from your car to a network tower while the ledger counts what the platform now holds. A toggle flips the world between **Convenience View** and **Surveillance View** — wireframe buildings, plate-scanner cones, your movement trail. Across the week, buyers arrive: an insurance pilot that reprices you for parking in the "wrong" zone, an employment-screening API, a location broker, an ANPR hookup. Two genuine choices exist — cash versus app payment, extend versus circulate — and neither is clean, because the collection floor (your plate) is structural.

The design rules matter: **you cannot win by hiding** — there is no stealth power-up, because there is none in life; every inference shown is labelled as inference; the punch lands on systems, not players. The game cites the spec; the harm pages analyse it; this report argues it. One evidence chain, four entry points.

## 7. The asks

**For ULBs:** adopt the six contract lines above before the next tender; publish the concession; treat the curb's data as a public asset with a fiduciary, not a vendor's exhaust.

**For citizens:** at the kiosk, pay cash when you can; refuse optional fields; file RTIs for the concession agreement and the consent notice; exercise DPDP erasure on your parking history — every forced refusal builds the public record.

**For developers and researchers:** the reference stack is forkable — simulator, harm pages, report, and game all derive from one spec ([/parkwise-demo/spec](/parkwise-demo/spec)). Build the anonymous-payment reference implementation. Audit vendors. Break the identity-coupled architecture.

A parking fee is one of the smallest transactions in a city's day, and one of the cheapest ways to timestamp a person's presence at a place and time. The convenience is real. The price is simply never shown. **This report is the receipt.**

---

## Sources

[^1]: NY Daily News, "Cashless parking app introduced in NYC last year victim of data breach," Apr 2021 — https://www.nydailynews.com/2021/04/30/cashless-parking-app-introduced-in-nyc-last-year-victim-of-data-breach
[^2]: CBS Pittsburgh, "Parking App Used In Pittsburgh Experiences Data Breach," 2021 — https://www.cbsnews.com/pittsburgh/news/pittsburgh-parking-authority-app-data-breach
[^3]: VTDigger, "Vermonter sues a leading parking app after data breach," Jun 2021 — https://vtdigger.org/2021/06/10/vermonter-sues-a-leading-parking-app-after-data-breach
[^4]: The Guardian via Freevacy, "Parking app operator EasyPark suffers data breach," Dec 2023 — https://www.freevacy.com/news/the-guardian/parking-app-operator-easypark-suffers-data-breach/4671
[^5]: CitizenNest, "VAHAN Parivahan: Vehicle Registration & RC Services" (public lookups, masked owner names) — https://www.citizennest.com/guide/vahan-parivahan-guide
[^6]: OkBima, "How to Check Vehicle Owner Details by Number Plate" (lookup and SMS channels) — https://www.okbima.com/motor-insurance/blog/how-to-check-vehicle-owner-details-by-registration-number-bid-44
[^7]: Jaagruk Bharat, "Fake Vahan Parivahan Challan Scam: How to Identify, Report & Stay Safe," 2026 — https://jaagrukbharat.com/Protect-Yourself-Fake-Vahan-Parivahan-Ticket-Alert-ODMz
[^8]: ETGovernment, "Nashik smart city to launch smart parking app," Jan 2020 — https://government.economictimes.indiatimes.com/news/digital-india/nashik-smart-city-to-launch-smart-parking-app/73730830
[^9]: No CCTV, "What's Wrong With ANPR?" — http://www.no-cctv.org.uk/whats_wrong_with_anpr.asp
[^10]: Ministry of Electronics & IT, "Digital Personal Data Protection Rules, 2025," Gazette G.S.R. 846(E), 13 Nov 2025 — https://www.dpdpa.com/DPDP_Rules_2025_English_only.pdf
[^11]: Deloitte India, "India's DPDP Rules 2025" (phased timeline: Nov 2026 / May 2027) — https://www.deloitte.com/in/en/services/consulting/about/indias-dpdp-rules-2025-leading-digital-privacy-compliance.html
[^12]: PIB, "DPDP Rules, 2025 Notified," Nov 2025 — https://static.pib.gov.in/WriteReadData/specificdocs/documents/2025/nov/doc20251117695301.pdf
