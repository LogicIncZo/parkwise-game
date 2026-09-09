# ParkWise — Pay & Display

**A counter-narrative project on smart parking surveillance.** A 3D browser game (Three.js), an evidence-bench simulation, five privacy-harm briefs, a policy report, and a historical timeline — all arguing one thesis: *paid parking is civic infrastructure; surveillance is the price quietly added on top.*

By CashlessConsumer · Kaarana (ಕಾರಣ) · LogicIncZo staging.

## Live

| Artifact | URL |
| --- | --- |
| Game (3D, 5 in-game days) | https://cashlessconsumer.zo.space/parkwise-game |
| Evidence bench (multi-city sim) | https://cashlessconsumer.zo.space/parkwise-demo |
| Harm briefs | `/parkwise-demo/{anpr,vahan-chain,who-else,breach-ledger,pattern-of-life}` |
| Report: *Pay & Display* | https://cashlessconsumer.zo.space/parkwise-report |
| Timeline: Chennai parking tech 2010→ | https://cashlessconsumer.zo.space/parkwise-timeline |

## Repo layout

```
routes/pages/   zo.space page routes (React + Three.js via esm.sh)
routes/api/     zo.space API routes (Hono; serve bench HTML + report markdown)
bench/          evidence bench: demo.html + 5 harm pages + ParkWise OpenAPI spec
docs/           PAY-AND-DISPLAY.md (report source) + GAME-MASTER-PROMPT.md (build bible)
```

## Deployment model

These routes run on a Zo Computer space (`cashlessconsumer.zo.space`). Page routes
are synced into the space runtime; API routes read files from the workspace at
request time. `bench/` and `docs/` here are **snapshots** — the canonical
publication copies live in the Kaarana archive:

- Bench canonical: `Kaarana/data/parkwise/` (spec `parkwise-public-api-openapi.json` v1.0.0)
- Report canonical: `Kaarana/research/urban/2026-09-09-pay-and-display-counter-narrative.md`

To redeploy elsewhere: host `routes/api/*` behind any Hono-compatible server and
point the file paths at a copy of `bench/` + `docs/PAY-AND-DISPLAY.md`. The game
and report page routes are self-contained (dependencies load from esm.sh).

## Design constraints (from GAME-MASTER-PROMPT.md)

- Real-kerb gameplay: bay geometry, ANPR poles, hold-to-confirm parking
- Every collected datum is shown at the moment of capture — no hidden fields
- Choice architecture: app vs cash, extend vs leave — each with a data price
- Receipt = surveillance dossier scored 0–100
- Fabricated data only; nothing transmitted; real tolls/limits cited where they appear

## License

MIT — free as in freedom.
