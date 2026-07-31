# Backstop — NeoCloud lease risk & hedge structuring desk

**The problem.** Data-center landlords increasingly refuse to sign the 5–7 year leases that
venture-backed AI-cloud ("NeoCloud") tenants need: the tenant's credit does not cover the
landlord's fit-out capex and re-leasing exposure, so deals stall. There is no single fix —
but the risk *can* be measured, layered, and hedged until a rational landlord signs.

**The tool.** `index.html` is a fully self-contained, zero-dependency interactive app
(open it in any browser). It lets a landlord, broker, or tenant CFO:

1. **Underwrite the tenant** — funding stage, cash runway, contracted backlog and its
   counterparty quality, customer concentration → an annual default-hazard curve over the
   lease term, with a seasoning shape and AI-market regime weighting.
2. **Price the true exposure** — loss-given-default by year of default: rent arrears,
   vacancy downtime, the PV of the re-let rent spread, and re-leasing costs, under
   boom / base / correction regimes. (Boom paths can produce re-let *gains* — powered
   shell is scarce.)
3. **Build a credit-support stack** — six instruments applied in seniority order:
   - Security deposit (escrow)
   - Letter of credit with ratable burn-down
   - GPU fleet lien (30 %/yr depreciation, 45 % fire-sale haircut, junior-lien discount)
   - Contracted-revenue assignment (counterparty survival × collection realization)
   - Sponsor guarantee (90 % honor probability)
   - Lease-default insurance, excess of collateral — **premium priced endogenously**
     off the model's own expected layer loss × 1.75 load
4. **Monte-Carlo the deal** — 6,000 seeded paths with common random numbers across
   configurations, producing expected loss, CVaR₉₅, loss distributions hedged vs.
   unhedged, a stress waterfall, and a cost-vs-protection frontier of standard packages.
5. **Solve it** — a greedy optimizer assembles the minimum-cost stack whose residual
   tail loss clears the landlord's stated tolerance, then renders a draft term sheet.
6. **Open the exotic desk** — a second tab for tenants the standard stack can't sign,
   with frontier instruments that price off the same simulation (feasibility-tagged
   from "street-legal" to "never been done"):
   - **CIR** — Compute-indexed rent: rent floats with a GPU spot-price index, so
     correction-regime default hazard falls and the re-let spread softens
   - **DSP** — Dark-shell put: a hyperscaler backfills at 85 % of rent within 90 days
   - **NDS-B** — Binary NeoCloud default swap: digital CDS paying full notional on
     the default event (2.2× load; overshoots small losses, undershoots big ones)
   - **AIW-CAT** — AI-winter cat bond: SPV notes wiped on a dual trigger
     (tenant default × correction regime)
   - **GRID** — Mutual guarantee pool: tenants insure each other, with an honest
     80 % honor rate exactly when corrections drain the pool
   - **RVG** — GPU residual-value guarantee: OEM resale floor upgrading the lien layer
   - **WAR** — Warrant coverage: pays the landlord *for* the risk instead of
     reducing it (income line, zero tail effect — shown honestly)

   A second solver button is allowed to use the whole desk.
7. **Switch to Market engineering** — a second page-level view answering *"how do we
   make these conditions possible?"*:
   - **Why this market has to be built** — the §502(b)(6) bankruptcy cap (most of a
     7-year lease claim is unrecoverable from the estate, so support must sit outside
     it), the wrong-way nature of the risk, and the missing plumbing
   - **Instrument anatomy** — for each of eight instruments, a Wall Street-style deal
     structure diagram (SPVs, dealers, trusts, calc agents, index administrators, with
     labeled money/risk flows) plus a component checklist tagged exists / emerging /
     to build, a readiness score, time-to-market, and who leads
   - **What to build first** — build difficulty vs. tail-risk transfer power, where
     power is computed live from the simulation (marginal CVaR₉₅ cut of each
     instrument alone for the tenant currently on the desk)
   - **Market build-out roadmap** — ten quarters across five workstreams: benchmark
     index, documentation standards, risk vehicles, consortium, regulatory

The headline output is a **SIGNABLE / MARGINAL / NOT SIGNABLE** verdict: residual
CVaR₉₅ against the landlord's tolerance, with the annual cost of getting there.

## Visualizations

- **Exposure vs. credit support by year of default** — the hero chart: stacked
  instrument capacity against expected and stress loss-given-default, with the
  uncovered stress gap washed in red. Where the red shows, the landlord is naked.
- **Default hazard over the term** — annual hazard bars + cumulative default curve.
- **Conditional loss distribution** — hedged vs. unhedged histograms with CVaR markers.
- **Stress waterfall** — who absorbs the loss, layer by layer, for any default year.
- **Cost vs. protection frontier** — standard packages priced on identical paths.

Every chart has a hover/tooltip layer, a table-view twin, and full light/dark theming.

## Running

Open `index.html`. No build step, no network calls, no dependencies. All simulation
runs client-side and re-runs live as any input moves.

## Disclaimer

Backstop is a negotiation-support model driven entirely by its stated assumptions
(see the in-app methodology note). It is not investment, insurance, or legal advice.
