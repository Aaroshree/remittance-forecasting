# Remittance Forecasting — Nepal



## Introduction



Nepal is one of the most remittance-dependent economies in the world, with remittance inflows accounting for a significant share of its GDP. Accurate forecasting of these inflows is critical for monetary policy, foreign exchange management, and economic planning. This project applies machine learning models — ARIMA, XGBoost, and LSTM — to forecast Nepal's monthly remittance inflows.



---



## Research Gap



Ghimire (2023) laid important groundwork in Nepal remittance forecasting, however several limitations remain. The study predicted remittance only one month ahead, limiting its usefulness for medium and long-term planning. It also relied on data only up to 2023 and did not account for the delayed relationship between worker departures and remittance flows — workers who leave Nepal today typically begin sending money home 3 to 9 months later. Additionally, Department of Foreign Employment (DoFE) departure data was not used as a predictor variable.



---



## Our Approach



This study addresses those gaps in three ways. First, we extend the forecasting horizon to 1-month, 3-month, and 9-month predictions. Second, we update the dataset through November 2025 to capture recent trends. Third, we introduce DoFE monthly departure data as a new predictor and explicitly encode the remittance delay by creating 3-month, 6-month, and 9-month lag features — allowing the model to learn how many workers left Nepal months ago are now sending money home.



---





## Dataset



The original dataset (Ghimire, 2023) covered Aug 2012 – Dec 2023. We extended it through \*\*November 2025\*\* by combining multiple official sources:



| Source | Variable | Period |

|--------|----------|--------|

| NRB Balance of Payments (BPM5) | `remittance` | Jan 2024 – Jul 2024 |

| NRB Current Macroeconomic Reports | `remittance` | Aug 2024 – Nov 2025 |

| NRB Exchange Rate file | `exchange_rate` | Jan 2024 – Nov 2025 |

| World Bank Pink Sheet | `oil_price` | Jan 2024 – Nov 2025 |

| Dept. of Foreign Employment (DoFE) | `dofe_departures` | Jan 2024 – Nov 2025 |



Remittance values for Aug 2024 – Nov 2025 were reconstructed by differencing cumulative figures from official NRB monthly PDF reports.



&#x20;FY2024/25 sum verified: 136.93 + 126.00 + 144.38 + ... = \*\*Rs. 1,723.27B\*\* (matches NRB published full-year total)



---



## Features



Workers who leave Nepal do not send money home immediately — there is typically a \*\*3 to 9 month delay\*\* before remittances arrive. To capture this, we added lag features:



| Column | Meaning |

|--------|---------|

| `dofe_lag3` | DoFE departures from 3 months ago |

| `dofe_lag6` | DoFE departures from 6 months ago |

| `dofe_lag9` | DoFE departures from 9 months ago |



---



## Data Pipeline



```

remittance_2012_2023_ready.csv        (original — 136 rows)
         |
         |  Extended to Nov 2025
         ↓
remittance_2012_2025_extended.csv     (160 rows, 5 cols)
         |
         |  Lag features added, NaN rows dropped
         ↓
remittance_2012_2025_model_ready.csv  (151 rows, 8 cols) ← ready for modelling

```



---



## Data Sources

- \[Nepal Rastra Bank](https://www.nrb.org.np) — remittance, exchange rate

- \[World Bank Pink Sheet](https://www.worldbank.org/en/research/commodity-markets) — oil price

- \[Department of Foreign Employment](https://dofe.gov.np) — migrant worker departures

