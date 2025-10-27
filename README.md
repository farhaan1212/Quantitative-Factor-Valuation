# My Analysis of MSTR / BTC / QQQ Econometric Dynamics & Scenarios

In this Jupyter Notebook, I explore the complex and evolving relationship between MicroStrategy (MSTR), Bitcoin (BTC), and the Nasdaq-100 (QQQ). My main focus was to quantitatively understand the significant shift in their dynamics after August 2020 (when MSTR began its Bitcoin treasury strategy) and then build a framework to model MSTR's potential future price based on BTC movements. This involved applying various econometric techniques using Python with libraries like Pandas, Statsmodels, Arch, and Scikit-learn.

## My Analysis Workflow & Key Findings

1.  **Data Foundation:** I started by loading and aligning daily price data for MSTR, BTC, and QQQ to calculate the necessary log returns for the analysis.
2.  **Initial Exploration:** I examined basic statistics, rolling correlations/betas, and ran standard tests like cointegration and Granger causality to get a baseline understanding of how MSTR and BTC historically moved together.
3.  **Identifying the Shift:** I focused on visualizing and quantifying the structural break around August 2020 using comparative scatter plots and highlighting this date on the rolling metric charts. The difference was quite clear.
4.  **Modeling the Dynamics:** To capture the changing relationship more accurately, I implemented several advanced time-series models:
    * **VAR Models:** I compared Vector Autoregressions fitted separately pre/post-August 2020, looking at impulse responses (IRFs) and variance decomposition (FEVD) to see how the interactions changed.
    * **DCC-GARCH:** I modeled the time-varying volatility of both assets and, importantly, their dynamic conditional correlation (`rho_t`), offering a more sophisticated view than simple rolling correlation.
    * **Kalman Filter (Time-Varying Beta):** Using `statsmodels.SARIMAX`, I estimated MSTR's daily evolving sensitivity (`beta_t`) to BTC, which provided a smoother and potentially more responsive measure than standard rolling OLS. I also used this to calculate dynamic hedge ratios.
    * **Markov Switching Regression:** This model helped identify distinct market regimes (e.g., high-beta vs. low-beta) and track the probability of being in the dominant "BTC-proxy" regime over time.
    * **Copula Analysis:** To specifically analyze **tail risk**, I fitted a t-copula. This allowed me to estimate the tail dependence (`lambda_tail`) and calculate multi-horizon Conditional VaR (CoVaR) and Expected Shortfall (CoES) for MSTR, quantifying its risk during sharp BTC downturns. Comparing pre/post-2020 copula fits highlighted changes in extreme event dependence.
5.  **Building a Scenario Forecaster:** Finally, I applied these insights to build a practical forecasting tool:
    * Developed a **Monte Carlo engine** leveraging the Kalman Filter outputs (latest `beta_t`, its uncertainty, residual variance).
    * Generated **probabilistic forecasts** for MSTR's price over 21 days, explicitly incorporating model uncertainty.
    * Explored how different narratives ('Base Beta', 'Convex Beta', 'Scarcity Premium') impact the forecast distribution.
    * Calculated **key threshold probabilities** (e.g., P(MSTR>600), P(MSTR<300)) **conditional on BTC performance**, linking the statistical analysis directly back to valuation context and assessing risks around analyst price targets.

## Tech Stack Used
* **Python 3** (via Jupyter Lab/Notebook)
* **Pandas**, **NumPy**
* **Statsmodels** (OLS, RollingOLS, VAR, SARIMAX, Markov Regression, HAC errors)
* **Arch** (for GARCH models)
* **Scikit-learn** 
* **SciPy** (Optimization, Stats distributions)
* **Matplotlib**

## Running the Notebook
This is a Jupyter Notebook (`.ipynb`). To run it:
1.  Set up a Python environment (like Anaconda) with Jupyter.
2.  Install the libraries listed in `requirements.txt`.
3.  Launch Jupyter and open the notebook file.
4.  Make sure the data paths for MSTR, BTC, and QQQ are correctly set within the notebook.
5.  Run the cells sequentially to reproduce the analysis.
