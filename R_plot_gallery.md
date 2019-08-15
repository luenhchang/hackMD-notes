###### tags: `R` `exp` `odds ratio` `relative risk` `confidence interval` `beta` `standard error` `glm(family=binomial(link="logit"))` `logistic regression` `pchisq()`

# Calculation and interpretation for statistics

### How to interpret output from generalized linear model in R? 

#### **Deviance Residuals:** This is simply a non-parametric description of the distribution of the outcome variable.

#### **null deviance** and **residual deviance** To evaluate the overall performance of the model, look at the null deviance and residual deviance. **Null deviance** shows how well the outcome variable is predicted by a model with nothing but an intercept (grand mean). This is essentially a chi square value on 13916 degrees of freedom. Adding 19 predictors (caffeine.per.day ~ rs4410790) in the model decreases the deviance by 3464 (18557-15093) points on 19 (13916-13897) degrees of freedom. The **residual deviance** is a chi square value, 15093. A chi square of 15093 on 13897 df yields a p value of 1.456817e-12 `pchisq(15093, df=13897, lower.tail=FALSE)`. The null hypothesis is rejected. Notice that the degrees of freedom associated with the null deviance and residual deviance differs by 19, the number of covariates/predictors added in the model.

#### **X observations deleted due to missingness** It shows up here because you had 347366 observations for which either the outcome, predictors, or both are missing.

* [Interpreting Residual and Null Deviance in GLM R](https://github.com/bnamatherdhala/mint/wiki/Interpreting-Residual-and-Null-Deviance-in-GLM-R)
* [R: calculate p-value given Chi Squared and Degrees of Freedom](https://stats.stackexchange.com/questions/250819/r-calculate-p-value-given-chi-squared-and-degrees-of-freedom)
* [Interpretation of R's output for binomial regression](https://stats.stackexchange.com/questions/86351/interpretation-of-rs-output-for-binomial-regression)

```r!
Call:
glm(formula = formula, family = binomial(link = "logit"), data = data)

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-2.7414  -0.8439  -0.5069   0.9681   2.7273  

Coefficients:
                                 Estimate Std. Error z value Pr(>|z|)    
(Intercept)                     6.900e+00  2.524e-01  27.341  < 2e-16 ***
caffeine.per.day               -4.823e-05  1.379e-04  -0.350  0.72665    
merged_pack_years_20161         1.051e-02  1.403e-03   7.489 6.95e-14 ***
complete_alcohol_unitsweekly    6.130e-03  1.213e-03   5.054 4.33e-07 ***
age                            -1.248e-01  3.067e-03 -40.678  < 2e-16 ***
PC1                             1.654e-02  1.285e-02   1.287  0.19803    
PC2                             1.596e-02  1.348e-02   1.184  0.23644    
PC3                             2.455e-02  1.276e-02   1.923  0.05447 .  
PC4                            -2.103e-02  8.043e-03  -2.615  0.00892 ** 
PC5                             3.837e-03  3.594e-03   1.068  0.28573    
PC6                             1.129e-02  1.179e-02   0.958  0.33789    
PC7                             1.737e-02  9.253e-03   1.877  0.06054 .  
PC8                             1.420e-02  9.098e-03   1.560  0.11869    
TDI                             1.257e-01  7.195e-03  17.468  < 2e-16 ***
overall_health_rating          -6.002e-02  3.000e-02  -2.000  0.04546 *  
factor(inferred.sex)1          -2.246e-01  4.304e-02  -5.220 1.79e-07 ***
quali.edu.6138.recoded.z.score  6.331e-01  2.359e-02  26.835  < 2e-16 ***
factor(X20116_recodeFinal)2     4.098e-01  9.434e-02   4.344 1.40e-05 ***
rs2472297                      -5.597e-02  3.176e-02  -1.762  0.07801 .  
rs4410790                      -1.725e-02  2.932e-02  -0.588  0.55627    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 18557  on 13916  degrees of freedom
Residual deviance: 15093  on 13897  degrees of freedom
  (347366 observations deleted due to missingness)
AIC: 15133

Number of Fisher Scoring iterations: 4
```

```r!
# Calculate number of observations used in the modelling
numb.obs.raw.data <- nrow(data)
numb.obs.deleted <- length(logi.summ.outc.CI.expo.CCPD.predictor.covar.2SNPs[["na.action"]])
numb.obs.analysed <- numb.obs.raw.data-numb.obs.deleted
```

---

#### Standardise, center, normalise
* [What does “normalization” mean and how to verify that a sample or a distribution is normalized?](https://stats.stackexchange.com/questions/70553/what-does-normalization-mean-and-how-to-verify-that-a-sample-or-a-distribution/70555#70555)


#### Estimate R^2^ (proportion of trait variance explained by a predictor) for continuous and binary traits
* [How does the correlation coefficient differ from regression slope?](https://stats.stackexchange.com/questions/32464/how-does-the-correlation-coefficient-differ-from-regression-slope)
* [How to calculate the amount of phenotype variance explained by a few SNPs?](https://www.researchgate.net/post/How_to_calculate_the_amount_of_phenotype_variance_explained_by_a_few_SNPs)
* [How to calculate pseudo- R2  from R's logistic regression?](https://stats.stackexchange.com/questions/8511/how-to-calculate-pseudo-r2-from-rs-logistic-regression) 
* [How different is Beta computation using Covariance and Linear Regression?](https://math.stackexchange.com/questions/38644/how-different-is-beta-computation-using-covariance-and-linear-regression)

---

#### What is odds ratio?
* [Explaining Odds Ratios](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2938757/)
An odds ratio (OR) is a measure of association between an exposure and an outcome. The OR represents the odds that an outcome will occur given a particular exposure, compared to the odds of the outcome occurring in the absence of that exposure. Odds ratios are most commonly used in case-control studies, however they can also be used in cross-sectional and cohort study designs as well (with some modifications and/or assumptions).

#### Calculate odds ratios and 95% confidence interval from regression coefficient and standard error
* [Calculate odds ratio confidence intervals from plink output?](https://stats.stackexchange.com/questions/66760/calculate-odds-ratio-confidence-intervals-from-plink-output)

```r!
#-----------------------------------------------------------------------------
# Calculate odds ratio and 95% confidence intervals from GSMR beta and se 
#-----------------------------------------------------------------------------
## Calculate 95% confidence interval for the beta as beta+/-1.96*se.
gsmr_result$bxy_lbound <- with(gsmr_result,bxy-1.96*se)
gsmr_result$bxy_ubound <- with(gsmr_result,bxy+1.96*se)

## Exponentiate beta to get odds ratios
gsmr_result$OR <-with(gsmr_result,exp(bxy))

## Convert 95% CI of the beta to 95%CI of the odds ratio. 
gsmr_result$or_lbound <- with(gsmr_result,exp(bxy_lbound))
gsmr_result$or_ubound <-with(gsmr_result,exp(bxy_ubound))
```

#### Interpret odds ratios (OR), relative risk (RR)
* [Explaining Odds Ratios](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2938757/)
* Suppose an exposure trait has been standardised (i.e. mean +/- SDs) and has standard deviation (SD) of 2.181. Interpret the OR in terms of **standard deviation changes**. The interpretation for OR~exposure-->outcome~=0.85 is that 1 standard deviation increase of having the exposure is assoicated with 29.8% decrease of having the outcome/disease.
```r!
> SD=2.181
> OR=0.85
> OR^SD
[1] 0.7015565
> 1-OR^SD/1
[1] 0.2984435
```

* If the trait is not standardised but measured in its natural unit (e.g. cups of coffee consumed per day), the interpretation for OR~exposure-->outcome~=0.85 is that increased consumption of coffee by one cup is associated with 15% decrease `(1-0.85)/1` of having the outcome/disease. 

* OR is usually used to test association in case-control studies. The outcome should be binary (i.e. case 1 or control 0). OR is odds of having exposure in cases divided by odds of having exposure in controls.Suppose your exposure is smoking and outcome is lung cancer (binary):
    * when OR is < 1 and 95% CI do not include a number at or greater than 1 (e.g 0.87-0.93),  smoking reduces lung cancer 
    * when OR > 1 and 95% CI do not include a number smaller than 1 (e.g. 1.1 - 1.3), smoking increases lung cancer
    *  The interpretation of OR~(BMI→T2D)~ = 3.29 is that people whose BMI are 1 SD (SD = 3.98 for BMI in European men corresponding to ~12 kg of weight for men of 175 cm stature; see Supplementary Table 6 for the SD of the risk factors) above the population mean will have 3.29 times increase in risk to T2D compared with the population prevalence (~8% in the US) [Causal associations between risk factors and common diseases inferred from GWAS summary data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5768719/) 
    * when OR=1, there is no association between smoking and lung cancer. 
    * when OR is < 1 and 95% CI include a number at or greater than 1 (e.g 0.87-1.13), the result is similar to having OR=1
* Interpretation of the OR depends on how you calculate it. Usually, OR=odds of event in cases/odds of event in controls. The odds are the `P/(1-P)`, where P= probability of event in a group (be it case or control). From the formula, you can deduce that if OR>1, the odds of the event is higher in the cases. Of course, you should also calculate the confidence interval (CI) of the OR to determine if your result in statistically significant. If the CI contains 1, then the OR, although >1, is not statistically significant. Statistical significance and what determines it is a different topic and I won't go into it now.[How to interpret odds ratios that are smaller than 1?](https://www.researchgate.net/post/How_to_interpret_odds_ratios_that_are_smaller_than_1) 
    * If OR=1, then the odds of the event in the cases=odds of the event in controls. So, that event makes no difference, it's not a risk factor. 
    * If the OR<1, then the odds of the event in cases is lower than the odds of the event in controls. So, **if OR=0.4**, the easiest thing to do is reverse it. 1/0.4=2.5. Now you have odds of event in controls/odds of event in cases=2.5. So you would interpret: **The cases have a 2.5 lower odds of the event than the controls**, and the result is/is not statistically significant (depending on the CI and its inclusion of the value 1). You can also interpret the result as the odds of the event is 60% (=(1-0.4)*100) lower in cases than in the controls, and the result is/is not statistically significant (and insert CI).
* relative risk (RR) is used in cohort studies and RCTs. RR means probability of having disease in exposed divided by probability of having disease in non exposed. You need to know the difference between OR and RR and also difference between probability and odds. However, OR and RR are used inter changeably in most situations.

#### Run binary logistic regression of a binary outcome (y) on predictors (x) in R and interpret the coefficient estimate
* [Generalized Linear Models (GLMs) in R, Part 4: Options, Link Functions, and Interpretation](https://www.theanalysisfactor.com/generalized-linear-models-glm-r-part4/)
```r!
# Set up a formula for a logistic model
## pred.conti: continuous predictors
## pred.binar: binary predictors
##  
equation <- "y ~ x1 + x2 + x3" 
glm(formula= equation,data=data,family=binomial(link = "logit"))
```
