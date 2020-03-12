# EOSC 410 Assignment 1 

> Angelene Leow, 23162167

### Problem 1

|                    | `x`, `y`        | `x2`, `y2`        | `x3`, `y3`        |
|:------------------:|:---------------:|:-----------------:|:-----------------:|
| Pearson Cor.       | 0.580098        | 0.339721          | -0.901029         |
| Spearman Rank Cor. | 0.57242         | 0.57242           | 0.431895          |
| Linear Regression  | ![](out/xy.png) | ![](out/x2y2.png) | ![](out/x3y3.png) |

<!-- 
Briefly discuss the results (e.g. which correlation coefficient is more resistant to outliers and why?; in potential cases with different data but same correlation coefficients: why are the coefficients unchanged while the data is altered?)
-->

The scatterplots indicate that the (`x3`, `y3`) dataset has the most significant outliers, followed by (`x2`, `y2`). Based on this, the Spearman Rank Correlation is significantly more resistant to such outliers - from (`x`, `y`) to (`x3`, `y3`) we see the Pearson Correlation change by $\approx 1.48$, while the Spearman Rank Correlation only changes by $\approx 0.14$. This happens because the effects of outliers on the correlation coefficient are limited by their *rank* (evidenced by comparing (`x`, `y`) and (`x2`, `y2`), where both sets have the same Spearman Rank Correlation despite an additional outlier).

### Problem 2

Running stepwise regression on the dataset gives the following output:

```
Add  x6      with p-value 1.01879e-09
Add  x4      with p-value 3.45675e-17
Add  x3      with p-value 7.44399e-07
Add  x1      with p-value 0.00532758
```

A smaller p-value indicates high significance, so ordering the predictors by p-value gives us the ranked predictors provided by stepwise regression, `[x4, x3, x6, x1]`. In summary:

|                         | Multiple Linear Reg. | Stepwise Reg. |
|:-----------------------:|:--------------------:|:--------------|
| $r^2$                   | 0.9802415713082825  | 0.9787250523829691
| Regression Coefficients | 84.0383574,  0.393594474, -3.34462207, -6.69057179, 0.182755335, 0.0382950374 | 0.0401516254, -6.69257893, -3.15179908, 81.6604880
| Intercept               | -820.3594995596073 | -796.5486994468278
| Ranked Predictors       | n/a | `x4`, `x3`, `x6`, `x1`

Multiple linear regression and stepwise regression differ in that the latter only uses the significant features it finds (in this case, the 4 features listed), whereas the former uses all provided features. That is why stepwise regression returns fewer regression coefficients than multiple linear regression (4 as opposed to 6).

The results can be further improved by standardizing the data: rescaling the values so that the mean and standard deviation of observed values is 0 and 1 respectively. This can be done with the following formula:

$$
x' = \frac{x - \text{mean}(x)}{\sqrt{\text{var}(x)}}
$$

Using `sklearn.preprocessing.scale()` to perform this normalization, we get the following results for stepwise regression:

```
Add  x6      with p-value 1.01879e-09
Add  x4      with p-value 3.45675e-17
Add  x3      with p-value 7.44399e-07
Add  x1      with p-value 0.00532758
```

On the standardized data, this time around the stepwise regression determines that `x6` is more significant than `x3`, likely due to the larger magnitude of correlation coefficients, as seen in this summary of the updated results across both methods:

|                         | Multiple Linear Reg. | Stepwise Reg. |
|:-----------------------:|:--------------------:|:--------------|
| $r^2$                   | 0.9802415713082822  | 0.9787250523829697
| Regression Coefficients | 0.45279326,  0.21487816, -2.08086634, -3.34371761,  0.11248056, 2.727585174 | 2.85982168, -3.34472071, -1.96090095,  0.43998145
| Intercept               | 19.722171524375096 | 19.722171524375096
| Ranked Predictors       | n/a | `x4`, `x6`, `x3`, `x1`