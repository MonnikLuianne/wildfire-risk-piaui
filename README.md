# Daily Wildfire Risk Prediction in Piauí, Brazil

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/MonnikLuianne/wildfire-risk-piaui/blob/main/wildfire_risk_piaui.ipynb)

Final project of the course **Artificial Intelligence** (UFPI, Prof. Dr. Pedro Almir Martins de Oliveira).
Author: **Monnik Luianne Oliveira Nascimento Veloso Barbosa**.

The project predicts, **one day in advance**, whether at least one active fire will be detected in each of the
**224 municipalities of Piauí**, using only public data.

**Research question.** Do meteorological variables add predictive power beyond the seasonal and spatial pattern of
fires and the recent fire activity of each municipality? Secondary question: can an interpretable fuzzy rule base,
hand-crafted or tuned by a genetic algorithm, approach a data-driven model?

## Repository structure

| Path | Content |
|---|---|
| `wildfire_risk_piaui.ipynb` | Self-contained notebook: code, explanations, results and conclusions (all outputs saved) |
| `data/raw/` | Active fire detections from INPE (reference satellite AQUA_M-T), one CSV per year, 2015–2025 |
| `data/processed/weather_PI_2015_2025.csv.gz` | Daily weather from NASA POWER for the 224 municipalities, 2015–2025 |
| `data/processed/dataset_integrated_PI_2015_2025.csv.gz` | **Integrated dataset**: one row per municipality-day with daily weather and the fire label |
| `LICENSE` | MIT license (code) |
| `wildfire_risk_piaui_SBC.pdf` | Expanded abstract (SBC format, English) |
| `wildfire_risk_piaui_SBC.md` | Same expanded abstract in Markdown |
| `Previsão de queimadas.pptx` | Presentation slides (also available on Canva: https://canva.link/tepnv3z2bvue33f) |

## How to run

1. Open the notebook in Google Colab with the badge above.
2. Select **Runtime → Run all**.

All data are loaded directly from this repository: no file upload, login or API key is needed.
The full run takes about **1h20**. For a quick run (≈20 min), set `RUN_STABILITY = False` and
`RUN_WALK_FORWARD = False` in **Section 3** of the notebook. Random seeds are fixed, so results are reproducible.

## Data sources and conditions of use

| Source | Use in this project | Conditions |
|---|---|---|
| [INPE – Programa Queimadas (BDQueimadas)](https://terrabrasilis.dpi.inpe.br/queimadas/bdqueimadas/) | Active fire detections (target) | Open data; source must be cited |
| [NASA POWER](https://power.larc.nasa.gov) | Daily weather per municipality | Free use with acknowledgment of the NASA Langley Research Center POWER Project |
| [kelvins/municipios-brasileiros](https://github.com/kelvins/municipios-brasileiros) | Coordinates of the municipalities (IBGE-based) | Open-source repository (see its license) |

## Method (summary)

- **Target:** fire detected (1) or not (0) for each municipality-day; 4.3% positives.
- **Features (16):** previous-day and accumulated weather, recent fire activity and context (month, location), all
  computed with information up to the previous day to prevent leakage.
- **Protocol:** chronological split — training 2015–2022, validation 2023–2024, test 2025; main metric **PR-AUC**.
- **Models:** persistence, climatology and logistic regression (baselines); Random Forest (main model);
  Mamdani fuzzy index, hand-crafted and tuned by a genetic algorithm.
- **Analyses:** feature importance, ablation study, stability over 5 seeds, error analysis and walk-forward evaluation.

## Main results (test set, 2025)

| Model | PR-AUC |
|---|---|
| Random (reference) | 0.051 |
| Persistence | 0.082 |
| Climatology | 0.256 |
| Logistic regression | 0.262 |
| **Random Forest** | **0.286** |
| Fuzzy (hand-crafted) | 0.193 |
| Fuzzy + genetic algorithm | 0.226 |

- The Random Forest outperformed climatology and logistic regression in all five years of the walk-forward evaluation.
- Most of the gain comes from **recent fire activity**; weather adds only a small increment.
- Main limitation: **new outbreaks** (no fire in the previous week) are almost never anticipated.

## Use of Large Language Models

Claude (Anthropic) was used to brainstorm the topic, discuss the experimental design, debug errors and explain concepts.
All code was executed and all outputs were checked by
the author, who made the methodological decisions and is responsible for all content.

## License

Code released under the MIT License. Data belong to their original providers and follow their conditions of use.
