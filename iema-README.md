# iEMA: In-the-Wild Stress Detection Feature Analysis

> Undergraduate research project (Summer 2026) — Hanyang University IMC Lab, advised by Prof. Youngtae Noh

📄 [Presentation (iEMA_OM_feature_discussion.pptx)](./docs/iEMA_OM_feature_discussion.pptx)

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
| Lab sessions (2 participants) | 4-phase protocol per participant — public speech, math stress task, cold pressor test (4 min each), plus a 12-min rest period. Each task session split into train (3 min) / test (1 min) |
| In-the-wild wearable data (1 participant, 1 week) | `WEAR_HEART_RATE.jsonl` — ~300K samples over 7 days, used to validate the IM-merging threshold against real-world variability |
| Additional physiological streams | Gyroscope, magnetometer, EDA (electrodermal activity), skin temperature |

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

- The IM-merging gap threshold, re-estimated via a 2-component GMM decision boundary,
  converged around **6.23 minutes** — distinct from the paper's 300-second (5-minute)
  sensor-dropout session-splitting threshold, which serves a different purpose.
- All five physiological features (HR, EDA mean/variability, gyroscope variability,
  geomagnetic variability, skin temperature) showed statistically significant
  differences across stability states (p < .005); skin temperature notably showed an
  *inverse* pattern (decreasing rather than increasing toward the IM state).

## My Role

<!-- TODO: 본인이 직접 담당한 파트를 적어주세요 -->
- (담당 파트를 채워주세요 — 예: 데이터 수집/실험 진행 / GMM 임계값 분석 / feature significance 분석 등)

## Repo Structure

```
├── docs/                 # final presentation (iEMA_OM_feature_discussion.pptx)
├── notebooks/            # (실제 분석 코드가 있다면 여기에)
└── README.md
```

## Reference

- The framework validated here follows: *iEMA: High-Fidelity, Low-Burden self-report
  acquisition framework for In-the-Wild Stress Detection*
