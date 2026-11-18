# One Jar, Two Spoons

**Should a shared Genie budget (terminal CLI + workspace) get a daily per-user cap, and at what value?** A seeded Monte Carlo "dart board" of 2,000 simulated months decides.

- **Live demo:** https://shriyashekatkar25-byte.github.io/one-jar-two-spoons/
- **Gallery:** https://shriyashekatkar25-byte.github.io/apm-lab/
- APM Lab project by Shriya Shekatkar · UCLA · Stats 102C Monte Carlo Methods (Ch1)

## The launch that inspired it

On **Nov 18, 2026**, Databricks made **Genie Code CLI generally available**, a terminal-based coding agent for data and AI work. Its docs say Genie Code CLI is billed the same way as Genie Code, and that its usage counts toward the same Genie budget as the workspace. A week earlier (Nov 12), budgets scoped to Unity Gateway, including Genie budgets, gained **daily per-user thresholds** that reset at 00:00 UTC, and on Nov 10 budget alerts gained webhooks (Beta).

Sources (checked this run):
- [Databricks release notes, November 2026](https://docs.databricks.com/aws/en/release-notes/product/2026/november)
- [Genie Code CLI docs](https://docs.databricks.com/aws/en/genie-code/genie-code-cli)
- [Budgets docs](https://docs.databricks.com/aws/en/admin/account-settings/budgets)

## The PM decision

When engineers' terminal agent loops and analysts' workspace usage draw from one jar, a single stuck loop can empty the budget on day 24, and the people blocked are rarely the people who spent it. The prototype asks: **when an admin creates a shared Genie budget, should setup pre-fill a daily per-user threshold, and what dollar value?**

- **Success metric:** P(shared budget runs dry before month end) ≤ 5%
- **Guardrails:** real (non-runaway) CLI workdays paused by the cap ≤ 5% · analyst workdays lost to an empty jar ≤ 1% · budget utilization ≥ 75% · no rise in "budget blocked me" support cases
- **Stakeholders:** account admins / FinOps, analysts, data engineers, Genie Code and gateway engineering, billing UX, support, field CS

The director memo rewrites itself from the simulation: it picks the loosest cap that passes, recommends an alert-only threshold when the team already has headroom, or says a bigger budget is needed when no cap is safe.

## What this shows about product judgment

| Databricks APM skill | Where it shows up |
|---|---|
| Customer problem framing | The person blocked by an empty jar is not the person who spent it; the decision is framed around fairness across two surfaces |
| Prototyping | A working, interactive simulation with sliders, a cap finder and a self-writing memo |
| SQL / Python | Illustrative SQL over per-user daily spend and a Python resampling replay of real months |
| Data-driven decisions | Explicit success metric, guardrails with thresholds, and a sweep using common random numbers |
| Clear writing | A director memo and a one-sentence VP summary generated from the evidence |

## Methods

| Method (Stats 102C) | Used for |
|---|---|
| Ch1: seeded pseudorandom numbers | Reproducible months: same seed, same answer |
| Ch1: probability as a fraction of hits | P(dry) = share of simulated months that hit the budget; count is Binomial, SE = √(p̂(1−p̂)/n) |
| Ch1: expectations as sample means, Strong Law of Large Numbers | Guardrail metrics as averages; running-estimate chart with a ±1.96·SE band |
| Common random numbers (Ch1 practice) | Cap finder re-uses one seed across all cap values so differences are the policy's effect |

Synthetic model: 40 seats; analysts ~$4/workday (exponential); CLI users ~$10/workday with an exponential tail, an 8% chance of an honest big day (+$30–$120) and a slider-controlled chance of a runaway loop (+$150–$600); weekends at 15%; pool is a hard stop once spent (a modelling assumption, not a product claim).

## Design baseline

Four pins from Shriya's Pinterest home feed, redrawn from scratch in CSS/SVG (no pin images are used):

- **Hero** ← [End the Loop: Learn the Lesson, Choose Differently (Bloom PG&D)](https://www.pinterest.com/pin/473581717091196151/): terracotta poster ground (#C6704C), heavy condensed black all-caps type, a round sticker badge (redrawn as an original jar-and-spoons badge).
- **Cards & buttons** ← [red-marker handwritten note (imnot_arushi.n)](https://www.pinterest.com/pin/975029388146955553/): white paper cards with red felt-marker headings (#C8102E) in a handwritten face, wobbly hand-drawn button borders.
- **KPI badges** ← [Hair & Nails Subliminal (The Superhero Guide)](https://www.pinterest.com/pin/1124774075722804809/): pale blush tiles (#FCEFF6) with bold rounded magenta (#A3207F) affirmation type.
- **Simulation room** ← [record-shelf film still with yellow caption (k.ga / @andkogaa)](https://www.pinterest.com/pin/1040472320210871008/): dusky blue-grey record-shelf stripes with walnut tones and film grain, yellow (#F1EA5E) condensed subtitle text.

---

Unofficial concept prototype by Shriya Shekatkar. All data is simulated. Not affiliated with any company.
