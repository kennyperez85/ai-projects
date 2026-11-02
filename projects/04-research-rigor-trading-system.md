# How I catch my own false positives — research methods from a quantitative trading system

**Status:** a long-running research and trading system with an event-driven backtester. Source private. This page covers the research methods, not the strategies or their performance.

This is the largest system I've built: a multi-process trading platform with a minute-level backtester, nightly reconciliation against broker records, and an automated pipeline that turns each day's results into testable hypotheses. Its most useful lessons had little to do with markets. They were about how easily a convincing result can be wrong, and how to build a process that finds out.

## The errors I found in my own results

| What looked true | What was actually happening | What changed |
|---|---|---|
| Strong historical returns | The backtests ranked stocks from **today's** large-cap list, so failed companies were invisible (survivorship bias) | Withdrew the results everywhere they appeared, then rebuilt on a point-in-time data store that passed an 11-point validation before any new result was reported |
| Spectacular returns on one machine | That machine's price data wasn't adjusted for stock splits, so splits looked like crashes and recoveries | Added a pre-flight check that aborts any run with an implausible one-day move, and classifies each flag against corporate-action records |
| Clean price histories | One ticker's file spliced two different companies that had used the same symbol; a spin-off looked like a 57% loss | Corporate actions are classified automatically; anything unexplained stops the run for a human |
| Strategies seeing the market in real time | Several **look-ahead bugs**: the simulator showed a day's closing price at the open, and a minute's final price mid-minute | Strict time cut-offs on every data read, with the fixes documented so new code can't repeat them |
| Filters that clearly improved results in-sample | Walk-forward testing showed the in-sample "winners" failed out of sample; one would have discarded the year's most profitable trades | No filter reaches the live system without out-of-sample re-simulation on every fold |
| A bootstrap showing a result "wasn't a fluke" | The bootstrap was centered on the observed result, so it would land near the 50th percentile for **any** result | Corrected the write-up: that statistic carries no information, and only the probability and interval estimates are reported |

## The process that came out of it

- **Primary sources before claims.** Any statement about the live system must cite the config file, the dated deployment log or the code. Memory and summaries lose to the source when they disagree.
- **Hypothesis, not finding.** New research is labeled as a hypothesis until it survives validation. A dashboard tracks each one from first detection through 90-day and 1-year tests.
- **No auto-apply.** An LLM (Claude) reviews each week's results and proposes patterns worth testing, but nothing it suggests changes the live system without independent validation and a human decision.
- **Dated deployment record.** Every change that affects the live system is written down the day it happens, with how to roll it back.
- **Dead ends are recorded.** Ideas that were tested and failed stay documented, so they aren't rediscovered and re-tested.

## What this taught me about AI research

The failure modes here are the ones I see in AI evaluation: test sets that leak the answer, benchmarks that silently change composition, metrics that can't fail, and impressive in-sample results that don't transfer. The habits that protect against them are the same too: look for the reason a result might be wrong before believing it, keep held-out data held out, and treat a withdrawn result as a success of the process rather than a failure of the researcher.
