# PharmaBot — calibrated forecasting of FDA decisions for psychiatric drugs

**Status:** research prototype. Source private. Trading is paper-only by default, and the live pathway is closed and tested as closed.

PharmaBot asks a narrow question: given an upcoming FDA decision on a psychiatric or CNS medication, what is the calibrated probability of approval? A panel of specialized agents (trial data, regulatory, safety, commercial, clinical expert) scores each event. A probability model combines their outputs, and every prediction is written up as an auditable, reconstructable report with its evidence and provenance.

## Design principles

- **Evidence before output.** Every prediction traces back to sources, and results extracted from labels or press releases are committed as reviewed, provenance-tagged files.
- **A good "no" is a success.** A risk manager with veto power sits after the model, and declining to act is treated as a valid outcome.
- **Neutral stubs, not fake conviction.** Agents that aren't wired to real data yet return explicitly neutral outputs, so the pipeline never invents confidence.

## Calibration results (30 historical psychiatric/CNS decisions: 22 approved, 8 rejected)

| Metric | Value |
|---|---|
| Brier score | 0.185 |
| Log loss | 0.554 |
| Accuracy at 50% | 73% (equal to the approval base rate) |
| AUC | 0.68 |

**The honest reading: discrimination is still weak.** Every one of the 8 rejections scored above 50%, and 73% accuracy is exactly what "always approve" would score.

| Group | n | Mean predicted approval |
|---|--:|--:|
| Approvals **with** a clinician's pre-event prior | 2 | 84% |
| Approvals **without** a prior | 20 | 70% |
| Rejections (no prior) | 8 | 68% |

The data-only approvals are statistically indistinguishable from the rejections. The only cases that separate clearly are the two with my clinical prior recorded before the decision. With n = 2, that is a hypothesis, not a result, and the next step is to record pre-event priors on many more cases.

## What I learned

- **Recalibration can't fix discrimination.** AUC depends only on ranking, and a monotonic recalibration can't change ranking. Better probabilities come from better features, not a better mapping.
- **Know your blind spots.** Some rejections were manufacturing failures with positive trials. The model can't predict them, and they are kept in the dataset on purpose to mark the gap.
- **Domain expertise is a feature you can measure.** This project is a small test of a question that runs through all my work: where does a clinician's judgment add signal that the data alone doesn't?
