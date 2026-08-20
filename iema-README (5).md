# iEMA: In-the-Wild Stress Detection Feature Analysis

> Undergraduate research project (Summer 2026) — Hanyang University IMC Lab, advised by Prof. Youngtae Noh

📄 [Presentation (iEMA_OM_feature_discussion.pptx)](./iEMA_OM_feature_discussion.pptx)

---

## Overview

This project implements and validates key components of the **iEMA framework**
(*High-Fidelity, Low-Burden self-report acquisition framework for In-the-Wild Stress
Detection*) — a system for triggering ecological momentary assessment (EMA) prompts
based on detected physiological "interesting moments" (IMs), rather than fixed schedules.

The long-term goal: use controlled lab session data (public speaking, math stress task,
cold pressor test) to predict in-the-wild heart rate, ultimately supporting stress
detection outside the lab.

## Data

| Source | Description |
|---|---|
| Lab sessions (2 participants) | TSST (public speech + mental arithmetic) + Cold Pressor Test + rest, conducted at Hanyang University, July 2026. Each ~4-min task phase, plus a 30-min rest period |
| In-the-wild wearable data (1 participant, 1 week) | ~306K wrist heart-rate samples over 7 days, used to validate the IM-merging threshold against real-world variability |
| Additional physiological streams | Gyroscope, geomagnetic sensor, EDA (electrodermal activity), skin temperature |

## Methodology

1. **IM (Interesting Moment) merging threshold calibration** — compared GMM
   (log-gap distribution, BIC-selected), KDE, and IQR methods to determine when two
   detected physiological events should be merged into a single IM window. GMM was
   selected as the most defensible approach.
2. **Feature significance analysis** — ANOVA across Stable / Ambiguous / IM states for
   heart rate, EDA (mean & variability), gyroscope variability, geomagnetic variability,
   and skin temperature. All features were significant (p < .005).
3. **M_phys / residual pipeline** — physiological deviation scoring (window = 300s,
   step = 150s) applied to both in-the-wild and lab data, following the iEMA paper's
   windowing specification.

## Key Findings

- The IM-merging gap threshold was determined via a 2-component GMM on the log-gap
  distribution: **26.4 minutes** (BIC = 305.89, the best fit among GMM/KDE/IQR). This
  is distinct from the paper's 300-second (5-minute) sensor-dropout session-splitting
  threshold, which serves a different purpose.
- All six physiological features (HR, EDA mean, gyroscope variability, EDA
  variability, geomagnetic variability, skin temperature) showed statistically
  significant differences across Stable / Ambiguous / IM states (p < .005).
  EDA variability showed the strongest sympathetic-arousal signal (22x increase in
  the IM state), while skin temperature was the only feature that *decreased*
  (vasoconstriction).

## My Role

- Conducted feature significance analysis (ANOVA) and IM-merging threshold
  calibration (GMM vs. KDE vs. IQR comparison) as part of the iEMA framework
  validation

## Repo Structure

```
├── iEMA_OM_feature_discussion.pptx   # final presentation
├── paper_review_iEMA.pptx            # reference paper review
└── README.md
```

## Reference

- The framework validated here follows: *iEMA: High-Fidelity, Low-Burden self-report
  acquisition framework for In-the-Wild Stress Detection*
- 📄 [Paper review slides](./paper_review_iEMA.pptx)
