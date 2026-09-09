# Capstone: Smart Reminder A/B Experiment — Injaz

**Experimentation & Causal Inference — github.com/SDAIAAcademy

## Overview

This project designs and evaluates a randomized A/B experiment for a **"Smart Reminder"**
feature on *Injaz* — a fictional Saudi digital-government services platform used for this
course. The reminder is a nudge (push notification / SMS) sent partway through a user's
session to encourage them to finish the online service they started.

The analysis walks through the full lifecycle of a credible experiment: simulating a
known ground-truth effect to validate the methodology, then designing, running, and
analyzing a real randomized test — ending in a data-driven launch recommendation.

## Business question

Before rolling the reminder out nationally, product management asked for:

1. **Why** they can't just turn it on for everyone and compare before/after
2. A properly **designed, simulated, and analyzed randomized experiment**
3. A **one-page decision memo** for the product lead: ship, hold, or don't ship

## Methodology

| Step | What it covers |
|---|---|
| **Counterfactual simulation** | Potential-outcomes framework, oracle ATE, selection bias vs. randomization, covariate balance (SMD), Fisher's randomization test |
| **Experiment design** | Power analysis, minimum detectable effect (MDE), required sample size, expected runtime |
| **A/A sanity check** | Sample-ratio mismatch (SRM) test, pipeline validation before trusting any result |
| **Primary metric analysis** | Two-proportion z-test on `reminder_completed` (the OEC) |
| **Guardrail analysis** | CUPED-adjusted estimate on `post_support_calls`, using a pre-period covariate to cut variance |
| **Sequential testing** | Quantifying inflation from naive daily peeking vs. an always-valid (mixture-SPRT) boundary |
| **Design credibility checks** | Full covariate balance table, multiple-comparisons (FWER/Bonferroni) illustration, pre-registered vs. post-hoc subgroup analysis |

## Key results

| Metric | Result |
|---|---|
| Oracle (true) ATE | +12.63 pp |
| Naive / self-selected estimate | +16.20 pp (biased — inflated by selection) |
| Randomized estimate | +12.26 pp (95% CI covers the oracle ATE) |
| Required sample size | 4,131 per arm (8,262 total); 5-day runtime |
| **Primary result (OEC)** | **+12.32 pp lift**, 95% CI [+10.31, +14.34] pp, p < 0.0001 |
| Guardrail (CUPED) | −0.173 support calls, 95% CI [−0.208, −0.138] |
| CUPED variance reduction | 27.9% |
| Naive daily-peeking false-positive rate | 14.8% (vs. nominal 5%) |
| Always-valid boundary false-positive rate | 1.0% |

**Recommendation: ship nationally.** The lift clears both the statistical bar and the
product team's 2 pp practical-significance threshold, with a guardrail that moved in a
helpful direction and no sign of a broken randomization (max |SMD| across covariates:
0.025).

## Stack

`numpy` · `pandas` · `matplotlib` · `scipy.stats`

## Contents

- `Capstone_Smart_Reminder_SOLVED.ipynb` — full notebook, executed end to end
- `injaz_users.csv` — synthetic Injaz user pool (20,000 users) used throughout

---
*Part of the Experimentation & Causal Inference track, SDAIA Academy.*
