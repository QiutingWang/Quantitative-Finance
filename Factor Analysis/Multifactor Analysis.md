# Multifactor Analysis

- Basic Financial Theory
  - CAPM and Fama-French Three Factors
  - APT

- Factors List
  - Value Factor
  - Momentum Factor
  - Liquidity Factor
  - Growth Factor
  - Volatility Factor
  - Size Factor
  - Quality Factor
  - Growth Factor
  - Sentiment Factor
  - Dividend Yield

- Multi-Factor Modeling and Back-testing
  - Data Pre-processing
    - Handle Outliers
      - MAD
      - Quantize
    - Neutralization
    - Standardization
    - Handle Missing Value
  - Single Factor Testing
    - Single Factor Back-Testing
    - Evaluation Matrices
      - Factor Return
      - IR
      - T-test
      - IC
      - IR_IC
      - Maximum Drawdown
      - Monotony
    - Multicollinearity analysis
    - Correlation Analysis
  - Factor Combination
    - Scoring
    - Linear Regression
      - LASSO
      - Ridge
    - Other Machine Learning Methods
      - Random Forest
      - Neutral Network
      - PCA
- Optimization
  - AutoEncoder
  - MVO

----------------------------------------------------------

### CAPM

- Formula:
  - $R_i=R_f+\beta*(R_m-R_f)$
  - $R_i-R_f=\alpha+\beta(R_m-R_f)+\epsilon$
    - $R_f$: take yield of ten-year Treasury bonds
    - $R_m$: market return大盘收益率
    - $\beta=Cov(R_i,R_m)/Var(R_m)$

### Fama-French Three Factor

- Formula:
  - $R_{it}-R_{ft}=\alpha_{it}+\beta_1(R_{mt}-R_{ft})+\beta_2SMB_t+\beta_3HML_t+\epsilon_it$
    - $SMB_t$: small minus big, size premium. `Small-cap stocks` run better than big-cap stocks
    - $HML_t$: high minus low, value premium. `Higher book-to-markets ratio` firms generating higher return.
    - $\beta_{1,2,3}$: factor coefficients
- More Variants:
  - Fama-French Five Factors: SMB HML MKT RMW CMA
  - Carhart Four Factors: SMB HML MKT UMD

### Arbitrage Pricing Theory(APT)

- Asset returns can be predicted by researching the **linear relationship** with some macroeconomic variables that induces systematic risk.
- Formula:
  - $E(R_i)=E(R_f)+\sum_{i=1}^n\beta_i(R_{m_i}-R_{f_i})$
    - $E(R_i)$: expected return on asset
    - $\beta_i$: the asset price sensitivity to factor
    - $R_{m_i}-R_{f_i}$: risk premium related to the factor

--------------------------------------------------------------

## Factor List

--------------------------------------------------------------

## Multi-Factor Modeling and Back-testing

### Data Preparation

#### Handle Outliers

- MAD(Median Absolute Deviation)
$$\widetilde{X_i}=
\begin{cases}
m-nMAD_e, if \space X_i<m-nMAD_e\\
m+nMAD_e, if \space X_i>m+nMAD_e\\
X_i, else\\
\end{cases}
$$
where $X_i$ is the factor exposure at factor $i$, $m$ is median of $X_i$, $MAD_e$ is the median of $|x_i-m|$
  
#### Neutralization

- $X_{i,k}=\beta_s*Size_i+\sum_j\beta_j*Industry_{i,j}=\epsilon_k$
  - $Industry_{i,j}$: a binary indicator.=1: stock $i$ in industry $j$
  - $X_{i,k}$: factor $k$ exposure of stock $i$

#### Standardization

$\widetilde{x_i}=\frac{x_i-\mu}{\sigma}$

#### Handle Missing Value

Drop the stocks with ST* and NaN.

### Single Factor Testing

#### Evaluation Matrices

1. Information Ratio稳定性
   - Formula: $IR=\frac{R_p-Rm}{\sigma_t}$
     - $\sigma_t$: tracking error
   - the bigger,the better. If IR>0.5, stably obtaining extra return ability of the factor is good.
   - portfolio return beyond the return of a benchmark, which usually chosen as an index
  
2. Information Coefficient相关性
   - Formula: $IC=Corr(X_t,r_{t+1})$
     - $X_t$: factor exposures, potential percentage of loss to a specific asset if a threat is realized.
       - $X=\sum_{j∊U}W_j*Z_j$ with factor weights and z-score
   - The correlation between predicted and actual stock returns
   - range: $[-1,1]$, the bigger abs value of IC, the more accuracy of prediction
     - |IC|>0.05, efficient factor; |IC|>0.1, a good factor
     - Sign of IC: - represents opposite indicator,+ represents positive indicator

3. Maximum Drawdown(MDD)
   - Definition: the maximum observed loss from a peak to a bottom of a portfolio, before a new peak is attained
   - Purpose: measurement of risk.
   - Formula: $MDD=\frac{P-L}{P}$
     - $P$: peak value
     - $L$: lowest value before a new highest observed
   - Normally, the PM wants to get MDDs are 50% of annual portfolio return or loss.
     - Higher MDD, higher expectation for returns.

4. T-test
   - Aim: test whether factor k is correlated with stock return significantly. Namely, test whether $\widetilde{f_k^t}\not ={0}$ or not.
     - $t=\frac{\bar{x}-\mu}{\sigma/\sqrt[2]{n-1}}$
  
5. Return Analysis

   - Consider the market and industry

### Multicollinearity analysis

- Indicator1: Variance Inflation Factor
  - Calculation Process:
    - regression one factor with other factors
      - $X_{ik}=\sum_{k'≠k}X_{ik'}b_{k'}+\epsilon_{ik}$
    - get the $R^2$ from the regression above
    - $VIF_k=\frac{1}{1-R^2_k}$
  - the bigger VIF, the heavier multicollinearity with other factors.

- Indicator2: RSI(relative strength index)
  - Calculation: $RSI_{AB}=\frac{mean(Corr_t^{AB})}{std(Corr_t^{AB})}, t=1,2,...,T$

### Correlation Analysis

- Pearson Correlation Coefficient
- Spearman Correlation Coefficient
