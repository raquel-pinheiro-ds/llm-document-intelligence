# Methods

Notation: $n$ items, $\alpha = 0.05$, all intervals are two-sided 95 % intervals. Bracketed numbers refer to the references at the end.

## 1. Statistical inference

### 1.1 Percentile bootstrap

For a statistic $\hat\theta = s(x_1, \dots, x_n)$, draw $B = 10{,}000$ resamples $x^{*}_{b}$ of size $n$ with replacement and compute $\hat\theta^{*}_{b} = s(x^{*}_{b})$. The interval is

$$\left[\hat\theta^{*}_{(\alpha/2)},\ \hat\theta^{*}_{(1-\alpha/2)}\right],$$

the empirical quantiles of the bootstrap distribution [1].

### 1.2 Cluster bootstrap

When items share a source (questions on the same document, fields of the same filing), items are not independent. The $G$ clusters are resampled with replacement and every item of a drawn cluster is kept, so the resampling unit matches the unit that was sampled independently [2]. Intervals follow 1.1.

### 1.3 Paired comparisons

Two systems $A$ and $B$ scored on the same items are compared on the same resamples. For a metric $m$ (for example macro-F1), each paired resample gives

$$\Delta^{*}_{b} = m(A^{*}_{b}) - m(B^{*}_{b}),$$

the interval for $\Delta = m(A) - m(B)$ uses the quantiles of $\Delta^{*}_{b}$, and the p-value uses the bootstrap distribution centered at the null [1, 3, 4]:

$$p = \frac{1 + \#\{b : |\Delta^{*}_{b} - \hat\Delta| \ge |\hat\Delta|\}}{B + 1}.$$

For paired binary correctness, the exact McNemar test uses only the discordant pairs: $n_{10}$ items where $A$ is correct and $B$ is wrong, $n_{01}$ the reverse. Under $H_0$, $n_{10} \mid (n_{10} + n_{01}) \sim \mathrm{Binomial}(n_{10} + n_{01}, 1/2)$ and

$$p = \min\left\{1,\ 2\,P\left(X \le \min(n_{10}, n_{01})\right)\right\}, \quad X \sim \mathrm{Binomial}(n_{10} + n_{01}, 1/2) \ [5].$$

### 1.4 Multiple comparisons

Within each task, the pairwise comparisons on the primary metric form one family. With ordered p-values $p_{(1)} \le \dots \le p_{(m)}$, Holm's adjusted p-values are

$$\tilde p_{(i)} = \max_{j \le i} \min\left\{1,\ (m - j + 1)\,p_{(j)}\right\},$$

and $H_{(i)}$ is rejected when $\tilde p_{(i)} \le \alpha$. The procedure controls the family-wise error rate at $\alpha$ under any dependence between tests [6].

### 1.5 Minimum detectable effect

For paired binary outcomes with discordance probability $\psi = p_{10} + p_{01}$ and effect $\delta = p_{10} - p_{01}$, the sample size needed for a two-sided test at level $\alpha$ with power $1 - \beta$ is approximately [7]

$$n = \frac{\left(z_{1-\alpha/2}\sqrt{\psi} + z_{1-\beta}\sqrt{\psi - \delta^{2}}\right)^{2}}{\delta^{2}}.$$

The minimum detectable effect is the smallest $|\delta|$ that satisfies this equation at the observed $n$, with $1 - \beta = 0.8$ and $\psi$ estimated from the observed discordant pairs. It states which differences each sample size can resolve [8].

### 1.6 Clustered binary outcomes

Correctness $Y_{ij}$ of item $j$ in cluster $i$ is modeled with generalized estimating equations [9]:

$$\operatorname{logit} P(Y_{ij} = 1 \mid x_{ij}) = x_{ij}^{\top}\beta,$$

with an exchangeable working correlation $\operatorname{corr}(Y_{ij}, Y_{ik}) = \rho$. $\hat\beta$ solves $\sum_i D_i^{\top} V_i^{-1}(y_i - \mu_i) = 0$, where $D_i = \partial\mu_i / \partial\beta^{\top}$ and $V_i$ is the working covariance. The sandwich covariance

$$\widehat{\operatorname{Var}}(\hat\beta) = A^{-1} \left(\sum_i D_i^{\top} V_i^{-1} r_i r_i^{\top} V_i^{-1} D_i\right) A^{-1}, \quad A = \sum_i D_i^{\top} V_i^{-1} D_i,$$

is consistent when the working correlation is misspecified. With tens of clusters it underestimates the variance, so residuals are replaced by $r_i = (I - H_{ii})^{-1}(y_i - \mu_i)$ with $H_{ii} = D_i A^{-1} D_i^{\top} V_i^{-1}$ [10]. Coefficients are reported as odds ratios with 95 % intervals.

## References

1. Efron, B., & Tibshirani, R. J. (1993). *An Introduction to the Bootstrap*. Chapman and Hall/CRC. doi:10.1201/9780429246593
2. Field, C. A., & Welsh, A. H. (2007). Bootstrapping clustered data. *Journal of the Royal Statistical Society Series B*, 69(3), 369-390. doi:10.1111/j.1467-9868.2007.00593.x
3. Berg-Kirkpatrick, T., Burkett, D., & Klein, D. (2012). An empirical investigation of statistical significance in NLP. *Proceedings of EMNLP-CoNLL 2012*, 995-1005. https://aclanthology.org/D12-1091
4. Dror, R., Baumer, G., Shlomov, S., & Reichart, R. (2018). The hitchhiker's guide to testing statistical significance in natural language processing. *Proceedings of ACL 2018*, 1383-1392. doi:10.18653/v1/P18-1128
5. McNemar, Q. (1947). Note on the sampling error of the difference between correlated proportions or percentages. *Psychometrika*, 12, 153-157. doi:10.1007/BF02295996
6. Holm, S. (1979). A simple sequentially rejective multiple test procedure. *Scandinavian Journal of Statistics*, 6(2), 65-70. JSTOR 4615733
7. Connor, R. J. (1987). Sample size for testing differences in proportions for the paired-sample design. *Biometrics*, 43(1), 207-211. doi:10.2307/2531961
8. Card, D., Henderson, P., Khandelwal, U., Jia, R., Mahowald, K., & Jurafsky, D. (2020). With little power comes great responsibility. *Proceedings of EMNLP 2020*, 9263-9274. doi:10.18653/v1/2020.emnlp-main.745
9. Liang, K.-Y., & Zeger, S. L. (1986). Longitudinal data analysis using generalized linear models. *Biometrika*, 73(1), 13-22. doi:10.1093/biomet/73.1.13
10. Mancl, L. A., & DeRouen, T. A. (2001). A covariance estimator for GEE with improved small-sample properties. *Biometrics*, 57(1), 126-134. doi:10.1111/j.0006-341X.2001.00126.x
