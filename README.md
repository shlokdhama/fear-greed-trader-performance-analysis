# Bitcoin Market Sentiment vs. Hyperliquid Trader Performance

This project analyzes how Bitcoin market sentiment relates to trader performance on Hyperliquid. It combines historical Hyperliquid trade data with the Bitcoin Fear & Greed Index to understand how profitability, win rates, position sizing, and trading behavior change across different sentiment regimes.

## Project Objective

The analysis investigates whether market sentiment influences:

- Trader profitability
- Win rates
- Position sizing
- Buy and sell behavior
- Long and short positioning decisions

By combining sentiment data with more than 211,000 Hyperliquid trades, the project explores behavioral and performance patterns across different market conditions.

## Executive Summary

The analysis shows a clear relationship between Bitcoin market sentiment and trader behavior.

Key findings:

- Extreme Greed periods generated the highest average profitability.
- Fear conditions outperformed Greed conditions in both profitability and win rate.
- Traders remained profitable despite win rates below 50%.
- Trading behavior showed strong contrarian patterns.
- Position size alone did not explain profitability differences.

One of the strongest findings is that profitability remained positive even when win rates stayed below 50% across all sentiment regimes. This suggests trader success was driven more by favorable risk-reward ratios than by prediction accuracy alone.

## Datasets

### Bitcoin Fear & Greed Index

Daily Bitcoin sentiment data with the following fields:

| Column | Description |
| --- | --- |
| `value` | Numeric sentiment score |
| `classification` | Sentiment label |
| `date` | Daily sentiment date |

Sentiment categories:

- Extreme Fear
- Fear
- Neutral
- Greed
- Extreme Greed

### Hyperliquid Historical Trading Data

Trade-level Hyperliquid dataset containing 211,224 trades.

Important columns:

| Column | Description |
| --- | --- |
| `Account` | Trader account identifier |
| `Coin` | Traded asset |
| `Execution Price` | Trade execution price |
| `Size Tokens` | Trade size in tokens |
| `Size USD` | Trade size in USD |
| `Side` | Buy or sell side |
| `Direction` | Position direction |
| `Closed PnL` | Realized profit or loss |
| `Timestamp IST` | Trade timestamp in IST |

## Data Cleaning

### Timestamp Standardization

The Hyperliquid dataset stored timestamps in `DD-MM-YYYY HH:MM` format.

Example:

```text
25-02-2025 13:22
```

Using Pandas' default datetime parsing produced more than 132,000 invalid timestamps. The issue was resolved by explicitly specifying the timestamp format:

```python
trades["Timestamp IST"] = pd.to_datetime(
    trades["Timestamp IST"],
    format="%d-%m-%Y %H:%M"
)
```

### Date Alignment

Trading timestamps were converted to daily granularity:

```python
trades["date"] = trades["Timestamp IST"].dt.normalize()
```

Fear & Greed dates were also converted to datetime format:

```python
fear["date"] = pd.to_datetime(fear["date"])
```

### Dataset Merge

The datasets were merged using the daily `date` field:

```python
merged = trades.merge(
    fear,
    on="date",
    how="left"
)
```

Only 6 observations lacked matching sentiment data and were removed, resulting in nearly complete coverage of the trading dataset.

## Methodology

The analysis was conducted in five stages:

1. **Data inspection**
   - Dataset dimensions
   - Column validation
   - Missing value checks

2. **Data cleaning**
   - Timestamp correction
   - Date extraction
   - Merge preparation

3. **Sentiment analysis**
   - Average profitability
   - Median profitability
   - Total profitability across sentiment regimes

4. **Behavioral analysis**
   - Buy vs. sell activity
   - Open long vs. open short activity
   - Position sizing behavior

5. **Performance analysis**
   - Average Closed PnL
   - Win rate
   - Total profitability by sentiment condition

## Key Results

### Profitability by Sentiment

| Sentiment | Average PnL |
| --- | ---: |
| Extreme Greed | 67.89 |
| Fear | 54.29 |
| Greed | 42.74 |
| Extreme Fear | 34.54 |
| Neutral | 34.31 |

### Win Rate by Sentiment

| Sentiment | Win Rate |
| --- | ---: |
| Extreme Greed | 46.49% |
| Fear | 42.08% |
| Neutral | 39.70% |
| Greed | 38.48% |
| Extreme Fear | 37.06% |

## Major Findings

### 1. Extreme Greed Produced the Highest Profitability

Extreme Greed conditions generated the highest average PnL and the highest win rate. This suggests traders benefited from strong market momentum and elevated volatility during highly optimistic market conditions.

### 2. Fear Outperformed Greed

Fear conditions outperformed Greed conditions across profitability and win rate.

| Metric | Fear | Greed |
| --- | ---: | ---: |
| Average PnL | 54.29 | 42.74 |
| Win Rate | 42.08% | 38.48% |
| Average Position Size | 7,816 | 5,737 |

This may indicate that fearful markets create profitable trading opportunities through volatility and overreaction.

### 3. Profitability Did Not Require High Accuracy

All sentiment regimes had win rates below 50%. Despite this, both average profitability and total profitability remained positive.

This suggests profitability was driven by favorable risk-reward ratios rather than simply winning more trades.

### 4. Evidence of Contrarian Trading

Buy and sell behavior showed clear sentiment-dependent patterns.

#### Buy vs. Sell Distribution

| Sentiment | Buy % | Sell % |
| --- | ---: | ---: |
| Extreme Fear | 51.10% | 48.90% |
| Extreme Greed | 44.86% | 55.14% |

#### Open Long Activity

| Sentiment | Open Long % |
| --- | ---: |
| Extreme Fear | 32.73% |
| Extreme Greed | 15.75% |

#### Open Short Activity

| Sentiment | Open Short % |
| --- | ---: |
| Extreme Fear | 14.83% |
| Extreme Greed | 19.16% |

These patterns suggest traders frequently positioned against prevailing market sentiment.

### 5. Position Size Alone Did Not Explain Performance

Average position sizes varied meaningfully across sentiment categories.

| Sentiment | Average Position Size (USD) |
| --- | ---: |
| Fear | 7,816 |
| Greed | 5,737 |
| Extreme Fear | 5,350 |
| Neutral | 4,783 |
| Extreme Greed | 3,112 |

Despite having the smallest average position size, Extreme Greed produced the highest profitability. This suggests market conditions and trade quality played a larger role than risk exposure alone.

## Conclusions

The analysis suggests that Bitcoin market sentiment is associated with measurable differences in trader profitability, positioning behavior, and risk-taking patterns.

Key takeaways:

- Extreme Greed delivered the strongest performance.
- Fear markets provided surprisingly attractive trading opportunities.
- Traders appeared to use contrarian strategies during emotional market extremes.
- Positive profitability was achieved despite sub-50% win rates.
- Market sentiment provides useful context for understanding trader behavior.

Overall, sentiment appears to be a valuable feature for analyzing and potentially predicting trading performance.

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Jupyter Notebook

## Repository Structure

```text
fear-greed-trader-performance-analysis/
├── analysis.ipynb
├── fear_greed_index.csv
├── historical_data.csv
├── requirements.txt
├── charts/
│   ├── avg_pnl_by_sentiment.png
│   ├── avg_position_size_by_sentiment.png
│   ├── direction_distribution_by_sentiment.png
│   ├── side_distribution_by_sentiment.png
│   └── win_rate_by_sentiment.png
└── README.md
```

## How to Run

Clone the repository:

```bash
git clone https://github.com/shlokdhama/fear-greed-trader-performance-analysis.git
cd fear-greed-trader-performance-analysis
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Open `analysis.ipynb` and run all cells.

## Author

**Shlok Dhama**

Interests:

- Machine Learning
- NLP
- Data Science
- FastAPI
- AI Applications
- Quantitative Analysis
