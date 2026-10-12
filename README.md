# Stock-Risk-Return-Analysis
Comparative stock risk, volatility, and return analysis (BEML vs. GLAXO) in Python.

This project performs a financial risk–return and statistical inference analysis on historical daily stock price data for two companies—BEML (manufacturing/engineering sector) and GLAXO (GlaxoSmithKline Pharmaceuticals)—covering the period from 2010 to 2016. The primary objective is to measure, visualize, and compare the volatility, probability of extreme gains/losses, and statistical confidence intervals for daily stock returns.

Step-by-Step Breakdown of What Was Done
1. Data Ingestion & Preparation : Import the core analytical and visualization packages using import pandas as pd, import numpy as np, import matplotlib.pyplot as plt, import seaborn as sns, and from scipy import stats to set                                    up the data analysis environment.
                                  Load the historical daily stock data for both equities using beml = pd.read_csv("BEML.csv") and glaxo = pd.read_csv("GLAXO.csv") to bring the trading records into pandas DataFrames.
                                  Subset down to the Date and Close columns and convert them into date-indexed time series using beml1 = beml[['Date','Close']].set_index(pd.DatetimeIndex(beml['Date'])) and
                                  glaxo1 = glaxo[['Date','Close']].set_index(pd.DatetimeIndex(glaxo['Date'])) to make chronological analysis and plotting straightforward.
                                  Visualize closing price trajectories across the 2010–2016 period using plt.plot(beml1['Close']) and plt.plot(glaxo1['Close']) to observe long-term price behavior and trends.

2. Daily Return / Gain Calculation : Calculate daily percentage returns from one closing price to the next using beml1['gain'] = beml1['Close'].pct_change(periods=1) and glaxo1['gain'] = glaxo1['Close'].pct_change(periods=1)  
                                     to standardize daily gains across different price levels.
                                     Clean the data by dropping the first record's NaN value using beml1.dropna(inplace=True) and glaxo1.dropna(inplace=True) so statistical calculations execute without errors.

4. Distribution & Summary Statistics : Plot the return distributions and kernel density estimates using sns.distplot(beml1['gain']) and sns.distplot(glaxo1['gain']) to visually check for bell-shaped normality and compare       
                                       return spread.
                                       Calculate the mean daily return and standard deviation using beml_mean, beml_std = beml1['gain'].mean(), beml1['gain'].std() and glaxo_mean, glaxo_std = glaxo1['gain'].mean(),                                                    glaxo1['gain'].std() to quantify expected return (0.027% vs 0.039%) and volatility (2.64% vs 1.34%).

7. Probability & Tail-Risk Profiling : Compute the cumulative probability of suffering a daily loss of 2% or greater using stats.norm.cdf(-0.02, beml_mean, beml_std) and stats.norm.cdf(-0.02, glaxo_mean, glaxo_std), revealing 
                                       that BEML has roughly 3.5\times higher downside risk (22.16%) than GLAXO (6.35%).
                                       Calculate the upper-tail probability of gaining 2% or more on any given day using 1 - stats.norm.cdf(0.02, beml_mean, beml_std) and 1 - stats.norm.cdf(0.02, glaxo_mean, glaxo_std) to   
                                       measure upside potential (22.77% for BEML vs. 7.10% for GLAXO).

9. Confidence Intervals & Statistical Inference : Retrieve standard normal critical values Z-scores using stats.norm.ppf(0.95) ($\approx 1.64$ for $90\%$ confidence) and stats.norm.ppf(0.975) ($\approx 1.96$ for $95\%$  
                                                  confidence) to set distribution boundary multipliers.
                                                  Compute the 95% confidence intervals under the normal distribution using stats.norm.interval(0.95, beml_mean, beml_std) and stats.norm.interval(0.95, glaxo_mean, glaxo_std) to                                                    determine the expected range of daily returns.
                                                  Construct the 95% confidence intervals under Student's t-distribution using stats.t.interval(0.95, 1738, beml_mean, beml_std) and stats.t.interval(0.95, 1737, glaxo_mean,   
                                                  glaxo_std), demonstrating that with a large sample size (N > 1700), the $t$-intervals ([-5.16\%, +5.21\%] for BEML and [-2.58\%, +2.66\%] for GLAXO) closely match the  
                                                  normal distribution.

10. Conclusion : This comparative analysis demonstrates the classic financial principle of the risk–return trade-off between a defensive asset and a high-beta cyclical asset.
                 Risk & Volatility Profile - BEML exhibits nearly double the daily standard deviation (2.64%) compared to GLAXO (1.34%), making it a substantially more volatile investment.  
                 Downside vs. Upside Exposure - Under a normal distribution assumption, BEML carries a 22.16% probability of incurring a daily loss of 2% or more, which is roughly 3.5 times higher than GLAXO's downside risk 
                 (6.35%). While BEML offers a higher chance of large daily upside swings (22.77% vs. 7.10%), GLAXO delivered a slightly higher average daily return with much tighter, more predictable bounds.   Investor 
                 Investor Implication - GLAXO acts as a defensive, low- volatility stock, well-suited for capital preservation and risk-averse investors seeking steady performance.   BEML behaves as an aggressive, high-
                 volatility stock, appropriate for active traders and risk-tolerant investors looking to capitalize on broader price momentum. 
