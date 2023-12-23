# Logit_model_4

Logistic model with categorical explanatory variables.

### Data Loading and Preparation:

- `Chapman.Cuali <- read.csv("Chapman_Cuali.csv", header=T, sep=",", stringsAsFactors = T)`: This command reads a CSV file named "Chapman_Cuali.csv" into a DataFrame called Chapman.Cuali. The `header=T` argument means that the first row contains column headers. The `stringsAsFactors = T` argument converts string columns to factors, which is useful for categorical data in logistic regression.

### Releveling Factor Variables:

- `Chapman.Cuali$Presion <- relevel(Chapman.Cuali$Presion, ref="Optima")`
- `Chapman.Cuali$IMC <- relevel(Chapman.Cuali$IMC, ref="Normal")`
- These commands change the reference level of the factor variables `Presion` and `IMC` in the DataFrame. The reference level is used as the baseline category in regression models.

### Logistic Regression Model Fitting:

- `Ajuste.Presion <- glm(Coronarios ~ Presion, data=Chapman.Cuali, family=binomial)`: This fits a logistic regression model with `Coronarios` as the dependent variable and `Presion` as the independent variable. The `family=binomial` argument indicates it's for binary outcomes.
- `summary(Ajuste.Presion)`: Displays a summary of the fitted model, including coefficients, their statistical significance, and other diagnostic measures.

### Tabulation of Data:

- `table(Chapman.Cuali$Presion, Chapman.Cuali$Coronarios)`: Creates a contingency table to see the distribution of `Coronarios` across different levels of `Presion`.

### Fitted Values Analysis:

- `fitted.values(Ajuste.Presion)[1:10]`: Retrieves the first 10 fitted values (predicted probabilities) from the logistic regression model.

### Comparing Fitted Values with Actual Data:

- `table(fitted.values(Ajuste.Presion), Chapman.Cuali$Coronarios)`: Compares the fitted values with the actual `Coronarios` data.

### Confidence Intervals:

- `confint.default(Ajuste.Presion)`: Calculates confidence intervals for the model parameters.
- `exp(confint.default(Ajuste.Presion))`: Exponentiates the confidence intervals, commonly done in logistic regression to interpret the results as odds ratios.

### Model Comparison:

- `Ajuste.0 <- glm(Coronarios ~ 1, data=Chapman.Cuali, family=binomial)`: Fits a null model (with only an intercept) for comparison.
- `anova(Ajuste.0, Ajuste.Presion)`: Compares the null model with the fitted model using an ANOVA test.
- `pchisq(anova(Ajuste.0, Ajuste.Presion)$Deviance[2], anova(Ajuste.0, Ajuste.Presion)$Df[2], lower.tail=F)`: Calculates the p-value for the test statistic.

### Residual Analysis:

- Various commands calculate different types of residuals (`pearson` and `deviance`) and other diagnostic measures like `rstandard`, `hatvalues`, and `cooks.distance`. These are used to evaluate the fit of the model and identify influential data points.

### ROC Curve Analysis:

- `library(pROC)`: Loads the `pROC` library for ROC curve analysis.
- `CurvaROC <- roc(Chapman.Cuali$Coronarios, fitted.values(Ajuste.Presion))`: Calculates the ROC curve using actual and fitted values.
- `plot(CurvaROC)`: Plots the ROC curve to evaluate the diagnostic performance of the model.
