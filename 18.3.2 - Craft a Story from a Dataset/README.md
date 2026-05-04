# 🌍 IMF Global Fossil Fuel Subsidies (2015–2030)

[![Kaggle Dataset](https://img.shields.io/badge/Kaggle-Dataset-blue?logo=kaggle)](https://www.kaggle.com/datasets/khurramshahzad/imf-global-fossil-fuel-subsidies-2015-2030)
[![License: IMF](https://img.shields.io/badge/License-IMF%20Terms-orange)](https://www.imf.org/en/About/copyright-and-terms#data)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-green?logo=python)](https://python.org)
[![Last Updated](https://img.shields.io/badge/Updated-March%202026-brightgreen)]()

---

## 📋 Overview

This dataset provides **comprehensive fossil fuel subsidy estimates** published by the **International Monetary Fund (IMF)** covering **168 countries** from **2015 to 2030** (including IMF projections).

Fossil fuel subsidies represent the extent to which prices paid by consumers **do not reflect** the fuels' full financial and social costs. The dataset disaggregates subsidies into:

- **Explicit Subsidies** — Direct government price support (supply cost exceeds consumer price)
- **Implicit Subsidies** — Undercharging for environmental/social externalities (local air pollution, climate change, congestion, road damage, accidents)

> ⚠️ The concept of "subsidies" used here differs from the definition of subsidies in macroeconomic statistics.

---

## 📊 Dataset Summary

| Property | Value |
|---|---|
| **Rows** | 112,896 |
| **Columns** | 38 |
| **Countries** | 168 |
| **Time Period** | 2015 – 2030 |
| **Indicators** | 21 unique subsidy categories |
| **Units** | % of GDP · USD (2021 constant prices) |
| **Frequency** | Annual |
| **Source** | IMF Climate Change Indicators |
| **File Format** | CSV + JSON metadata |

---

## 📂 Files

| File | Description | Size |
|---|---|---|
| `IMF_FFS.csv` | Main dataset — all subsidy observations | ~40 MB |
| `IMF_FFS.json` | Original IMF metadata (provenance, topics, countries) | ~15 KB |
| `dataset-metadata.json` | Kaggle dataset metadata | ~4 KB |
| `README.md` | This documentation file | — |
| `fossil-fuel-subsidies-eda.ipynb` | Complete EDA notebook (Kaggle-ready) | — |
| `requirements.txt` | Python dependencies | — |
| `environment.yml` | Conda environment specification | — |

---

## 🔬 21 Subsidy Indicators

### Total Subsidies
| # | Indicator |
|---|---|
| 1 | Fossil Fuel Subsidies — Total Implicit and Explicit |
| 2 | Fossil Fuel Subsidies — Total Implicit and Explicit — Coal |
| 3 | Fossil Fuel Subsidies — Total Implicit and Explicit — Natural Gas |
| 4 | Fossil Fuel Subsidies — Total Implicit and Explicit — Petroleum |
| 5 | Fossil Fuel Subsidies — Total Implicit and Explicit — Electricity |

### Explicit Subsidies (Direct Government Support)
| # | Indicator |
|---|---|
| 6 | Explicit Fossil Fuel Subsidies — Total |
| 7 | Explicit Fossil Fuel Subsidies — Coal |
| 8 | Explicit Fossil Fuel Subsidies — Natural Gas |
| 9 | Explicit Fossil Fuel Subsidies — Petroleum |
| 10 | Explicit Fossil Fuel Subsidies — Electricity |

### Implicit Subsidies (Externality Undercharging)
| # | Indicator |
|---|---|
| 11 | Implicit Fossil Fuel Subsidies — Total |
| 12 | Implicit Fossil Fuel Subsidies — Coal |
| 13 | Implicit Fossil Fuel Subsidies — Natural Gas |
| 14 | Implicit Fossil Fuel Subsidies — Petroleum |
| 15 | Implicit Fossil Fuel Subsidies — Electricity |
| 16 | Implicit Fossil Fuel Subsidies — Global Warming |
| 17 | Implicit Fossil Fuel Subsidies — Local Air Pollution |
| 18 | Implicit Fossil Fuel Subsidies — Congestion |
| 19 | Implicit Fossil Fuel Subsidies — Road Damage |
| 20 | Implicit Fossil Fuel Subsidies — Accidents |
| 21 | Implicit Fossil Fuel Subsidies — Foregone VAT |

---

## 🏗️ Key Columns

| Column | Type | Description |
|---|---|---|
| `REF_AREA` | `str` | ISO 3166-1 alpha-3 country code |
| `REF_AREA_LABEL` | `str` | Full country name |
| `INDICATOR` | `str` | Indicator code |
| `INDICATOR_LABEL` | `str` | Human-readable indicator name |
| `UNIT_MEASURE` | `str` | `PT_GDP` or `USD_K_2021` |
| `UNIT_MEASURE_LABEL` | `str` | Unit description |
| `TIME_PERIOD` | `int` | Year (2015–2030) |
| `OBS_VALUE` | `float` | Observed / projected value |
| `OBS_STATUS_LABEL` | `str` | Data quality flag |
| `OBS_CONF_LABEL` | `str` | Confidentiality status |

---

## 🚀 Quick Start

### Option 1 — Kaggle Notebook (Recommended)
```python
import pandas as pd

df = pd.read_csv("/kaggle/input/imf-global-fossil-fuel-subsidies-2015-2030/IMF_FFS.csv")
df.head()
```

### Option 2 — Local Setup with Conda
```bash
conda env create -f environment.yml
conda activate kag_go
jupyter notebook fossil-fuel-subsidies-eda.ipynb
```

### Option 3 — Local Setup with pip
```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook fossil-fuel-subsidies-eda.ipynb
```

---

## 📈 Sample Analysis Queries

```python
import pandas as pd

df = pd.read_csv("IMF_FFS.csv")

# Top 10 countries by total subsidies (% of GDP) in 2025
total_gdp = df[
    (df['INDICATOR_LABEL'] == 'Fossil Fuel Subsidies - Total Implicit and Explicit') &
    (df['UNIT_MEASURE'] == 'PT_GDP') &
    (df['TIME_PERIOD'] == 2025)
].nlargest(10, 'OBS_VALUE')[['REF_AREA_LABEL', 'OBS_VALUE']]

# Global subsidy trend over time (USD)
trend = df[
    (df['INDICATOR_LABEL'] == 'Fossil Fuel Subsidies - Total Implicit and Explicit') &
    (df['UNIT_MEASURE'] == 'USD_K_2021')
].groupby('TIME_PERIOD')['OBS_VALUE'].sum()

# Explicit vs Implicit breakdown for a specific country
country = df[
    (df['REF_AREA_LABEL'] == 'United States') &
    (df['UNIT_MEASURE'] == 'USD_K_2021') &
    (df['INDICATOR_LABEL'].str.contains('Explicit|Implicit')) &
    (df['INDICATOR_LABEL'].str.contains('Total'))
]
```

---

## 🔗 Data Source & Citation

- **Publisher**: International Monetary Fund (IMF)
- **Portal**: [IMF Climate Change Indicators — Mitigation](https://climatedata.imf.org/pages/mitigation/#mi3)
- **Direct Data**: [IMF Fossil Fuel Subsidies Dataset](https://climatedata.imf.org/datasets/d48cfd2124954fb0900cef95f2db2724_0/about)
- **Methodology**: [IMF Working Paper — Still Not Getting Energy Prices Right](https://www.imf.org/en/Publications/WP/Issues/2021/09/23/Still-Not-Getting-Energy-Prices-Right-A-Global-and-Country-Update-of-Fossil-Fuel-Subsidies-466004)

### Suggested Citation
```
International Monetary Fund (IMF). "Fossil Fuel Subsidies."
IMF Climate Change Indicators, 2025.
https://climatedata.imf.org/
```

---

## ⚖️ License

This dataset is published under **IMF Data Terms of Use**. Permission is required to copy or download IMF Content in any systematic way or to re-use, publish, and disseminate a substantial amount beyond "Fair Use," whether for commercial or noncommercial purposes.

See: [IMF Copyright and Terms](https://www.imf.org/en/About/copyright-and-terms#data)

---

## 🤝 Contributing

Found an issue or have ideas for additional analysis? Feel free to:
- Open a discussion on the Kaggle dataset page
- Fork the notebook and share your findings
- Submit pull requests for improved visualizations

---

## 📌 Tags

`fossil-fuels` · `subsidies` · `climate-change` · `IMF` · `energy-policy` · `global-warming` · `environmental-economics` · `GDP` · `coal` · `petroleum` · `natural-gas` · `carbon-emissions` · `sustainability` · `EDA` · `data-visualization`

---

<p align="center">
  <b>Made with ❤️ for Climate Data Research</b><br>
  If you find this dataset useful, please <b>upvote</b> it on Kaggle! ⬆️
</p>
