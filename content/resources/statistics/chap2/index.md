    knitr::opts_chunk$set(echo = TRUE)

# Mediation with latent variables

## What is mediating/indirect effects?

    ## Loading required package: Rcpp

![](Moderation-moderated_mediation_new_files/figure-markdown_strict/unnamed-chunk-1-1.png)

-   A given variable may be said to function as a mediator to the extent
    that it accounts for the relation between the predictor and the
    criterion. Mediators explain how external physical events take on
    internal psychological significance (Baron & Kenny, 1986, p. 1176).

    -   Mediation analysis answers “how” or “why” the observed effect
        occurs.

    -   “The underlying mechanism”

-   The “Mediating Effects”

    -   Also known as indirect effect in sociology or economics; in
        Structural Equation Modeling

        -   Direct effect: The direct influence of the predictor to the
            dependent variable after controlling the mediator

        -   Indirect effect: The influence of the predictor to the
            dependent variable via the mediator

        -   Total effect: The sum of direct and indirect effects

-   Types of mediation

    -   Complete mediation

    -   Partial mediation

    -   Notes. Current research reports rarely stress the difference.
        Why?

    -   Mediators can be more than one variable

-   The equations？

    -   *M* = *β*<sub>01</sub> + *β*<sub>11</sub>*X*

    -   *Y* = *β*<sub>02</sub> + *β*<sub>12</sub>*X* + *β*<sub>22</sub>*M*

-   Is it enough to make the following claim if it refers to
    “mediation”?

> If our model fit the data well, we are safe to conclude that the
> hypothesize causational pattern between 𝐴, 𝐵 and 𝐶 is well supported
> by data, it can be use to explain the observed correlation among these
> 3 variables.

## Then, how?

## The “traditional” approach: Baron and Kenny’s 4-step approach

1.  The predictor significantly correlates with the dependent variable
2.  The predictor is related to the mediator
3.  The mediator predicts the dependent variable
4.  The strength of the relation between the predictor and the dependent
    variable is significantly reduced when the mediator is added to the
    model

-   How to test these steps in regression?

<!-- -->

    set.seed(233)
    x <- rnorm(300,4,1)
    med <- 0.5*x + rnorm (300,0,0.5)
    y <- 0.1*x + 0.6*med + rnorm(300,0,0.5)
    df2 <- data.frame(x,med,y)

1.  The predictor significantly correlates with the dependent variable:
    b<sub>yx</sub>

<!-- -->

    model.step1 <- lm(y~x,df2)
    summary(model.step1)

    ## 
    ## Call:
    ## lm(formula = y ~ x, data = df2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.66491 -0.41371  0.03236  0.39688  2.14289 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  0.02799    0.15699   0.178    0.859    
    ## x            0.39168    0.03793  10.328   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.6179 on 298 degrees of freedom
    ## Multiple R-squared:  0.2636, Adjusted R-squared:  0.2611 
    ## F-statistic: 106.7 on 1 and 298 DF,  p-value: < 2.2e-16

1.  The predictor is related to the mediator: b<sub>mx</sub>

<!-- -->

    model.step2 <- lm(med~x,df2)
    summary(model.step2)

    ## 
    ## Call:
    ## lm(formula = med ~ x, data = df2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.28852 -0.33578 -0.00593  0.37153  1.29721 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -0.35855    0.12734  -2.816  0.00519 ** 
    ## x            0.59257    0.03076  19.263  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5012 on 298 degrees of freedom
    ## Multiple R-squared:  0.5546, Adjusted R-squared:  0.5531 
    ## F-statistic: 371.1 on 1 and 298 DF,  p-value: < 2.2e-16

1.  The mediator predicts the dependent variable: b<sub>ym</sub>

<!-- -->

    model.step3 <- lm(y~med,df2)
    summary(model.step3)

    ## 
    ## Call:
    ## lm(formula = y ~ med, data = df2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.72668 -0.35811 -0.00736  0.33982  1.44663 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  0.29468    0.08876    3.32  0.00101 ** 
    ## med          0.64636    0.04102   15.76  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5318 on 298 degrees of freedom
    ## Multiple R-squared:  0.4545, Adjusted R-squared:  0.4526 
    ## F-statistic: 248.2 on 1 and 298 DF,  p-value: < 2.2e-16

1.  The strength of the relation between the predictor and the dependent
    variable is significantly reduced when the mediator is added to the
    model: b<sub>yx.m</sub>

<!-- -->

    model.step4 <- lm(y~x+med,df2)
    summary(model.step4)

    ## 
    ## Call:
    ## lm(formula = y ~ x + med, data = df2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.71829 -0.35459 -0.00681  0.34935  1.42725 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  0.25321    0.13710   1.847   0.0658 .  
    ## x            0.01946    0.04898   0.397   0.6915    
    ## med          0.62814    0.06156  10.205   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5326 on 297 degrees of freedom
    ## Multiple R-squared:  0.4548, Adjusted R-squared:  0.4511 
    ## F-statistic: 123.9 on 2 and 297 DF,  p-value: < 2.2e-16

-   The a,b,c,c’ in mediation model

    -   a: b<sub>mx</sub>

    -   b: b<sub>ym.x</sub>

    -   c: b<sub>yx</sub>

    -   c’: b<sub>yx.m</sub>

-   The criterion of complete mediation: c’ = 0

-   Comments on the 4-step approach

    -   Advantage: Easy for understanding

    -   Disadvantage: Low power

        -   How many hypotheses have been tested in this 4 steps?

    -   Test of significance of c’

        -   Not significant: Complete mediation

        -   Significant: Then what?

## Sobel test: The significance of a\*b

-   Among the a, b, c, c’, what is the effect of mediation?

-   Which one is the total effect (the overall effect of x on y)? Which
    one is the direct effect (the distinctive effect of x on y after
    controlling for med)?

-   Mediation effect, or, more precisely, indirect effect = total
    effect - direct effect= c - c’

-   Prove c-c’=a\*b

    -   *z*<sub>*y*</sub> = *β*<sub>*y**x*</sub>*z*<sub>*x*</sub>; *z*<sub>*m*</sub> = *β*<sub>*m**x*</sub>*z*<sub>*x*</sub>; *z*<sub>*y*</sub> = *β*<sub>*y**x*.*m*</sub>*z*<sub>*x*</sub> + *β*<sub>*y**m*.*x*</sub>*z*<sub>*m*</sub>

    -   $\beta\_{yx}=r\_{xy}=\frac{\sum z\_xz\_y}{n-1}=\frac{\sum z\_x(\beta\_{yx.m}z\_x+\beta\_{ym.x}z\_m)}{n-1}=\frac{\beta\_{yx.m}\sum z\_x^2+\beta\_{ym.x}\sum z\_x(\beta\_{mx}z\_x)}{n-1}=\beta\_{yx.m}+\beta\_{ym.x}\beta\_{mx}$

    -   c=c’+a\*b

-   To test whether indirect effect is significant, we can test whether
    a\*b is significantly from 0.

    -   Sobel test: $z=\frac{ab}{\sqrt{b^2s^2\_a+a^2\*s^2\_b}}$

    -   Aroian test:
        $z=\frac{ab}{\sqrt{b^2\*s^2\_a+a^2\*s^2\_b+s^2\_a\*s^2\_b}}$

    -   Goodman test:
        $z=\frac{ab}{\sqrt{b^2\*s^2\_a+a^2\*s^2\_b-s^2\_a\*s^2\_b}}$

    -   Baron & Kenny recommended the Aroian formula

<!-- -->

    library(bda)

    ## Loading required package: boot

    ## bda - 18.2.2

    mediation.test(df2$med,df2$x,df2$y)

    ##                Sobel       Aroian      Goodman
    ## z.value 9.017374e+00 9.007901e+00 9.026877e+00
    ## p.value 1.926512e-19 2.100374e-19 1.766401e-19

-   What is the problem of sobel test (family)?

-   How did we generate the z score?

-   What is the underlying assumption of z-test?

## Bootstrap: The mainstream test for indirect effect

-   Bootstrap is suggested as a better approach: A nonparametric
    approach

    -   Bollen, K. A., & Stine, R. (1990). Direct and Indirect Effects:
        Classical and Bootstrap Estimates of Variability. *Sociological
        Methodology*, *20*, 115. <https://doi.org/10.2307/271084>

-   To determine whether a regression coefficient is different from 0,
    what would we do?

    -   t-test; sampling distribution; p-value

    -   ![](nhst.jpg)

-   The bootstrapping approach: By resampling with replacement

    -   ![](bootstrap_illustration.png)

-   How bootstrapping procedure determine the significance of a\*b?

    -   ![](bootstrap_sampling.png)

    -   How to do it?

    -   The famous PROCESS

        -   Hayes, A. F., & Preacher, K. J. (2014). Statistical
            mediation analysis with a multicategorical independent
            variable. *British Journal of Mathematical and Statistical
            Psychology*, *67*(3), 451–470.
            <https://doi.org/10.1111/bmsp.12028>

            Preacher, K. J., & Hayes, A. F. (2004). SPSS and SAS
            procedures for estimating indirect effects in simple
            mediation models. *Behavior Research Methods, Instruments, &
            Computers*, *36*(4), 717–731.

<!-- -->

    library(psych)

    ## 
    ## Attaching package: 'psych'

    ## The following object is masked from 'package:boot':
    ## 
    ##     logit

    my.model3 <- mediate(y~x+(med),data=df2,n.iter=100)

![](Moderation-moderated_mediation_new_files/figure-markdown_strict/unnamed-chunk-8-1.png)

    summary(my.model3)

    ## Call: mediate(y = y ~ x + (med), data = df2, n.iter = 100)
    ## 
    ## Direct effect estimates (traditional regression)    (c') X + M on Y 
    ##              y   se     t  df     Prob
    ## Intercept 0.25 0.14  1.85 297 6.58e-02
    ## x         0.02 0.05  0.40 297 6.91e-01
    ## med       0.63 0.06 10.20 297 3.72e-21
    ## 
    ## R = 0.67 R2 = 0.45   F = 123.85 on 2 and 297 DF   p-value:  7.66e-40 
    ## 
    ##  Total effect estimates (c) (X on Y) 
    ##              y   se     t  df     Prob
    ## Intercept 0.03 0.16  0.18 298 8.59e-01
    ## x         0.39 0.04 10.33 298 1.42e-21
    ## 
    ##  'a'  effect estimates (X on M) 
    ##             med   se     t  df     Prob
    ## Intercept -0.36 0.13 -2.82 298 5.19e-03
    ## x          0.59 0.03 19.26 298 2.85e-54
    ## 
    ##  'b'  effect estimates (M on Y controlling for X) 
    ##        y   se    t  df     Prob
    ## med 0.63 0.06 10.2 297 3.72e-21
    ## 
    ##  'ab'  effect estimates (through all  mediators)
    ##      y boot   sd lower upper
    ## x 0.37 0.37 0.04  0.28  0.42

-   What if you don’t have the original data? Or, the above methods are
    not applicable?

-   Monte Carlo Method: Bootstrapping (parameter) in mediation

    -   <http://quantpsy.org/medmc/medmc.htm>

    -   If possible, the traditional bootstrapping method is preferable

    -   ![](bootstrap_parameter.png)

<!-- -->

    summary(model.step2)

    ## 
    ## Call:
    ## lm(formula = med ~ x, data = df2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.28852 -0.33578 -0.00593  0.37153  1.29721 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -0.35855    0.12734  -2.816  0.00519 ** 
    ## x            0.59257    0.03076  19.263  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5012 on 298 degrees of freedom
    ## Multiple R-squared:  0.5546, Adjusted R-squared:  0.5531 
    ## F-statistic: 371.1 on 1 and 298 DF,  p-value: < 2.2e-16

    summary(model.step4)

    ## 
    ## Call:
    ## lm(formula = y ~ x + med, data = df2)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.71829 -0.35459 -0.00681  0.34935  1.42725 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  0.25321    0.13710   1.847   0.0658 .  
    ## x            0.01946    0.04898   0.397   0.6915    
    ## med          0.62814    0.06156  10.205   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5326 on 297 degrees of freedom
    ## Multiple R-squared:  0.4548, Adjusted R-squared:  0.4511 
    ## F-statistic: 123.9 on 2 and 297 DF,  p-value: < 2.2e-16

    RMediation::medci(model.step2$coefficients["x"],
                      model.step4$coefficients["med"],
                      se.x=summary(model.step2)$coefficients["x","Std. Error"],
                      se.y=summary(model.step4)$coefficients["med","Std. Error"],
                      type="MC")

    ## $`95% CI`
    ##      2.5%     97.5% 
    ## 0.2936716 0.4552401 
    ## 
    ## $Estimate
    ## [1] 0.3722536
    ## 
    ## $SE
    ## [1] 0.04138573
    ## 
    ## $`MC Error`
    ## [1] 4.138573e-07

-   What if… your theory predicts more than one mediator?

    -   ![](Mediators.png)

-   Or, you have more than 1 DVs?

    -   ![](dvs.jpg)

-   Generally speaking, to deal with these models. Using lavaan package
    to fit them as observed variables in Structural Equation Modeling is
    easier.

-   Sometimes, you may have the need to compare the indirect effects
    from 2 different independent samples.

    -   e.g., whether a mediation is larger in China than in the US.

    -   *I*<sub>*d**i**f**f*</sub> = *I*<sub>1</sub> − *I*<sub>2</sub>

    -   $SE\_{I\_{diff}}=\sqrt{SE^2\_{I\_1}+SE^2\_{I\_2}}$

    -   $z=\frac{I\_{diff}}{SE\_{I\_diff}}$

    -   The logic is the same as the change score approach in mixed
        design. Therefore, it could be regarded as a mediation model
        with a moderator. Or, a multi-group analysis in SEM.

## How to conduct equivalent GLM mediation using lavaan package?

### Two intermediate mediators (serial mediation)

    library(lavaan)

    ## This is lavaan 0.6-17
    ## lavaan is FREE software! Please report any bugs.

    ## 
    ## Attaching package: 'lavaan'

    ## The following object is masked from 'package:psych':
    ## 
    ##     cor2cov

    library(semPlot)

    ## Warning in check_dep_version(): ABI version mismatch: 
    ## lme4 was built with Matrix ABI version 0
    ## Current Matrix ABI version is 1
    ## Please re-install lme4 from source or restore original 'Matrix' package

    ## Set seed for replicability of results
    set.seed(3091003)
    my.df <- read.table("SEM03.dat", header=TRUE)

    my.med1 <- 'y ~ x + m1 + c*m2
    m2 ~ x + b*m1
    m1 ~ a*x
    ## Define indirect effect
    indirct := a*b*c'

    ## Usually bootstrap is set with resampling 5000 times. here to speed up the calculation, the value was set at 200
    my.fit1 <- sem(my.med1, data=my.df, se="bootstrap", bootstrap=200)
    summary(my.fit1)

    ## lavaan 0.6.17 ended normally after 1 iteration
    ## 
    ##   Estimator                                         ML
    ##   Optimization method                           NLMINB
    ##   Number of model parameters                         9
    ## 
    ##   Number of observations                           100
    ## 
    ## Model Test User Model:
    ##                                                       
    ##   Test statistic                                 0.000
    ##   Degrees of freedom                                 0
    ## 
    ## Parameter Estimates:
    ## 
    ##   Standard errors                            Bootstrap
    ##   Number of requested bootstrap draws              200
    ##   Number of successful bootstrap draws             200
    ## 
    ## Regressions:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##   y ~                                                 
    ##     x                 0.074    0.134    0.548    0.584
    ##     m1               -0.115    0.129   -0.893    0.372
    ##     m2         (c)    0.361    0.122    2.960    0.003
    ##   m2 ~                                                
    ##     x                -0.076    0.109   -0.694    0.487
    ##     m1         (b)    0.693    0.079    8.775    0.000
    ##   m1 ~                                                
    ##     x          (a)    0.703    0.098    7.167    0.000
    ## 
    ## Variances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##    .y                11.706    1.971    5.938    0.000
    ##    .m2                9.029    1.235    7.310    0.000
    ##    .m1               11.030    1.311    8.416    0.000
    ## 
    ## Defined Parameters:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##     indirct           0.176    0.064    2.761    0.006

![](serial_mediation.png)

    semPaths(my.fit1, whatLabels="est", color="green")

![](Moderation-moderated_mediation_new_files/figure-markdown_strict/unnamed-chunk-11-1.png)

### Two speciﬁc mediators (parallel mediation)

    ## Set seed for replicability of results
    set.seed(3091003)
    my.df <- read.table("SEM04.dat", header=TRUE)

    my.med2 <- 'y ~ x + b*m1 + d*m2
    m2 ~ c*x
    m1 ~ a*x
    ## Free the residuals between m1 and m2
    m1 ~~ m2
    ## Define total indirect effect
    ind_tot := a*b+c*d
    ## Define difference on indirect effects
    ind_dif := a*b-c*d'

    my.fit2 <- sem(my.med2, data=my.df, se="bootstrap", bootstrap=200)
    summary(my.fit2)

    ## lavaan 0.6.17 ended normally after 10 iterations
    ## 
    ##   Estimator                                         ML
    ##   Optimization method                           NLMINB
    ##   Number of model parameters                         9
    ## 
    ##   Number of observations                           100
    ## 
    ## Model Test User Model:
    ##                                                       
    ##   Test statistic                                 0.000
    ##   Degrees of freedom                                 0
    ## 
    ## Parameter Estimates:
    ## 
    ##   Standard errors                            Bootstrap
    ##   Number of requested bootstrap draws              200
    ##   Number of successful bootstrap draws             200
    ## 
    ## Regressions:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##   y ~                                                 
    ##     x                 0.143    0.160    0.896    0.370
    ##     m1         (b)    0.316    0.105    2.992    0.003
    ##     m2         (d)    0.361    0.122    2.960    0.003
    ##   m2 ~                                                
    ##     x          (c)    0.560    0.096    5.842    0.000
    ##   m1 ~                                                
    ##     x          (a)    0.703    0.098    7.167    0.000
    ## 
    ## Covariances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##  .m2 ~~                                               
    ##    .m1                2.133    0.866    2.462    0.014
    ## 
    ## Variances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##    .y                11.706    1.971    5.938    0.000
    ##    .m2                9.441    1.291    7.315    0.000
    ##    .m1               11.030    1.311    8.416    0.000
    ## 
    ## Defined Parameters:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##     ind_tot           0.424    0.094    4.504    0.000
    ##     ind_dif           0.020    0.115    0.173    0.862

    ## Obtain the bootstrap CI
    parameterEstimates(my.fit2)

    ##        lhs op     rhs   label    est    se     z pvalue ci.lower ci.upper
    ## 1        y  ~       x          0.143 0.160 0.896  0.370   -0.170    0.485
    ## 2        y  ~      m1       b  0.316 0.105 2.992  0.003    0.132    0.572
    ## 3        y  ~      m2       d  0.361 0.122 2.960  0.003    0.082    0.548
    ## 4       m2  ~       x       c  0.560 0.096 5.842  0.000    0.356    0.711
    ## 5       m1  ~       x       a  0.703 0.098 7.167  0.000    0.484    0.892
    ## 6       m2 ~~      m1          2.133 0.866 2.462  0.014    0.599    4.022
    ## 7        y ~~       y         11.706 1.971 5.938  0.000    7.646   15.654
    ## 8       m2 ~~      m2          9.441 1.291 7.315  0.000    6.286   12.071
    ## 9       m1 ~~      m1         11.030 1.311 8.416  0.000    8.412   13.604
    ## 10       x ~~       x         10.790 0.000    NA     NA   10.790   10.790
    ## 11 ind_tot := a*b+c*d ind_tot  0.424 0.094 4.504  0.000    0.239    0.629
    ## 12 ind_dif := a*b-c*d ind_dif  0.020 0.115 0.173  0.862   -0.170    0.268

    semPaths(my.fit2, whatLabels="est", color="green")

![](Moderation-moderated_mediation_new_files/figure-markdown_strict/unnamed-chunk-14-1.png)

## What are the latent variables?

-   Latent variables:

    -   Abstract and hypothetical constructs.

    -   They cannot be measured directly.

    -   Most constructs in psychology and social sciences are
        unobservable.

        -   For example, motivation, stress, depression, intelligence
            and satisfaction.

    -   What is the common strategy to measure psychological constructs
        in psychology?

-   Observed or measured variables:

    -   Indicators of the latent constructs.

        -   For example, items to measure depression, test scores of
            intelligence.

    -   How many items do we need to measure depression?

### CFA (Conﬁrmatory factor analysis) Model speciﬁcation

-   What is the proposed model?

-   *x* = *μ*<sub>*x*</sub> + *Λ**f* + *e*

    -   For example,
        *x*<sub>1</sub> = *μ*<sub>*x*<sub>1</sub></sub> + *λ*<sub>11</sub>*f*<sub>1</sub> + *e*<sub>1</sub>

    -   x<sub>1</sub>: observed continuous variable

    -   µ<sub>x1</sub>: intercept of the observed variable. It is
        usually set at the sample mean.

    -   f<sub>1</sub>: latent factor. Its mean is usually ﬁxed at 0. The
        mean can be non-zero in multiple-group analysis and latent
        growth model.

    -   λ<sub>11</sub>: factor loading. It is similar to regression
        coeﬃcient.

    -   e<sub>1</sub>: measurement error (and unique factor).

## Mediation with latent variables

-   The X, M, and Y are all latent variables

<!-- -->

    dat <- read.csv("exampleMod.csv")

    my.med3 <- '
    tvalue =~ t1 + t2 + t3 + t4
    sources =~ s1 + s2 + s3 + s4 + s5
    tip =~ tip1 + tip2 + tip3


    tip ~ tvalue + b*sources 
    sources ~ a*tvalue
    indirect:= a*b
    '
    my.fit3 <- sem(my.med3, data=dat, se="bootstrap", bootstrap=200)
    summary(my.fit3)

    ## lavaan 0.6.17 ended normally after 30 iterations
    ## 
    ##   Estimator                                         ML
    ##   Optimization method                           NLMINB
    ##   Number of model parameters                        27
    ## 
    ##                                                   Used       Total
    ##   Number of observations                          7037        7314
    ## 
    ## Model Test User Model:
    ##                                                       
    ##   Test statistic                              1261.996
    ##   Degrees of freedom                                51
    ##   P-value (Chi-square)                           0.000
    ## 
    ## Parameter Estimates:
    ## 
    ##   Standard errors                            Bootstrap
    ##   Number of requested bootstrap draws              200
    ##   Number of successful bootstrap draws             200
    ## 
    ## Latent Variables:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##   tvalue =~                                           
    ##     t1                1.000                           
    ##     t2                0.988    0.012   83.152    0.000
    ##     t3                0.809    0.014   57.256    0.000
    ##     t4                0.653    0.016   40.678    0.000
    ##   sources =~                                          
    ##     s1                1.000                           
    ##     s2                1.164    0.027   43.456    0.000
    ##     s3                0.860    0.022   38.981    0.000
    ##     s4                0.967    0.021   45.905    0.000
    ##     s5                1.280    0.028   45.502    0.000
    ##   tip =~                                              
    ##     tip1              1.000                           
    ##     tip2              0.297    0.027   10.900    0.000
    ##     tip3              0.980    0.037   26.329    0.000
    ## 
    ## Regressions:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##   tip ~                                               
    ##     tvalue           -0.080    0.015   -5.159    0.000
    ##     sources    (b)    0.559    0.019   29.897    0.000
    ##   sources ~                                           
    ##     tvalue     (a)   -0.167    0.014  -11.883    0.000
    ## 
    ## Variances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##    .t1                0.290    0.013   23.157    0.000
    ##    .t2                0.193    0.010   19.800    0.000
    ##    .t3                0.339    0.012   27.418    0.000
    ##    .t4                0.879    0.015   59.438    0.000
    ##    .s1                0.642    0.017   38.928    0.000
    ##    .s2                0.724    0.018   40.463    0.000
    ##    .s3                1.000    0.017   57.218    0.000
    ##    .s4                0.643    0.017   38.241    0.000
    ##    .s5                0.546    0.019   28.824    0.000
    ##    .tip1              0.736    0.023   31.627    0.000
    ##    .tip2              0.985    0.017   56.270    0.000
    ##    .tip3              0.777    0.025   31.692    0.000
    ##     tvalue            0.750    0.018   42.485    0.000
    ##    .sources           0.552    0.020   26.930    0.000
    ##    .tip               0.342    0.021   16.587    0.000
    ## 
    ## Defined Parameters:
    ##                    Estimate  Std.Err  z-value  P(>|z|)
    ##     indirect         -0.093    0.008  -11.182    0.000

    ## Obtain the bootstrap CI
    parameterEstimates(my.fit3)

    ##         lhs op     rhs    label    est    se       z pvalue ci.lower ci.upper
    ## 1    tvalue =~      t1           1.000 0.000      NA     NA    1.000    1.000
    ## 2    tvalue =~      t2           0.988 0.012  83.152      0    0.962    1.014
    ## 3    tvalue =~      t3           0.809 0.014  57.256      0    0.782    0.837
    ## 4    tvalue =~      t4           0.653 0.016  40.678      0    0.623    0.688
    ## 5   sources =~      s1           1.000 0.000      NA     NA    1.000    1.000
    ## 6   sources =~      s2           1.164 0.027  43.456      0    1.110    1.217
    ## 7   sources =~      s3           0.860 0.022  38.981      0    0.817    0.906
    ## 8   sources =~      s4           0.967 0.021  45.905      0    0.926    1.006
    ## 9   sources =~      s5           1.280 0.028  45.502      0    1.228    1.337
    ## 10      tip =~    tip1           1.000 0.000      NA     NA    1.000    1.000
    ## 11      tip =~    tip2           0.297 0.027  10.900      0    0.242    0.348
    ## 12      tip =~    tip3           0.980 0.037  26.329      0    0.901    1.059
    ## 13      tip  ~  tvalue          -0.080 0.015  -5.159      0   -0.110   -0.046
    ## 14      tip  ~ sources        b  0.559 0.019  29.897      0    0.522    0.596
    ## 15  sources  ~  tvalue        a -0.167 0.014 -11.883      0   -0.195   -0.142
    ## 16       t1 ~~      t1           0.290 0.013  23.157      0    0.264    0.314
    ## 17       t2 ~~      t2           0.193 0.010  19.800      0    0.171    0.212
    ## 18       t3 ~~      t3           0.339 0.012  27.418      0    0.315    0.367
    ## 19       t4 ~~      t4           0.879 0.015  59.438      0    0.849    0.909
    ## 20       s1 ~~      s1           0.642 0.017  38.928      0    0.608    0.672
    ## 21       s2 ~~      s2           0.724 0.018  40.463      0    0.690    0.758
    ## 22       s3 ~~      s3           1.000 0.017  57.218      0    0.965    1.032
    ## 23       s4 ~~      s4           0.643 0.017  38.241      0    0.608    0.672
    ## 24       s5 ~~      s5           0.546 0.019  28.824      0    0.510    0.584
    ## 25     tip1 ~~    tip1           0.736 0.023  31.627      0    0.688    0.779
    ## 26     tip2 ~~    tip2           0.985 0.017  56.270      0    0.948    1.021
    ## 27     tip3 ~~    tip3           0.777 0.025  31.692      0    0.728    0.827
    ## 28   tvalue ~~  tvalue           0.750 0.018  42.485      0    0.712    0.786
    ## 29  sources ~~ sources           0.552 0.020  26.930      0    0.510    0.597
    ## 30      tip ~~     tip           0.342 0.021  16.587      0    0.301    0.381
    ## 31 indirect :=     a*b indirect -0.093 0.008 -11.182      0   -0.109   -0.079

    semPaths(my.fit3, whatLabels="est", color="green")

![](Moderation-moderated_mediation_new_files/figure-markdown_strict/unnamed-chunk-17-1.png)

## Last things about mediation

-   Let’s look back on the 4-step approach, are all the 4 steps
    necessary?

    -   e.g., the first step c!=0?

        -   There is now, however, an emerging consensus (see Kenny,
            > Kashy, & Bolger, 1998; MacKinnon, Lockwood, Hoffman, West,
            > & Sheets, 2002; Shrout & Bolger, 2002) that this first
            > condition is unnecessary.
            >
            > (Judd & Kenny, 2010)

    -   What is needed to determine a significant indirect effect?

        -   ab=c-c’!=0

        -   The conclusion that only a single condition must hold to
            > demonstrate mediation, i.e., ab = c = c’ != 0, has led to
            > the **unfortunate conclusion among many social
            > psychologists that there is a statistical “test” of
            > mediation and all that one needs to do to argue for
            > mediation is to have that “test” be statistically
            > significant.** This is far from true. The “test” simply
            > amounts to a test of whether a partial slope is different
            > from an unpartialed or zero-order slope.
            >
            > …..
            >
            > What the statistics do is estimate and test the indirect
            > path if the model is true.
            >
            > (Judd & Kenny, 2010)

-   Statistics analyses should test your theory or model.

-   And this leads to another misunderstanding that mediation can test
    for causality.

-   Experimental psychology: 3 criteria for causality

    -   Association

    -   Temporal sequence

    -   No explanation by third variable

-   The causality inference should based on research design not
    statistical analyses

-   Things you may consider–manipulating the mediator(s)

    -   Pirlott, A. G., & MacKinnon, D. P. (2016). Design approaches to
        experimental mediation. *Journal of Experimental Social
        Psychology*, *66*, 29–38.
        <https://doi.org/10.1016/j.jesp.2015.09.012>

-   Or, cross-lagged panel model.

    -   ![](CLPM.png)

## The main course: Moderation with latent variables

Schoemann, A. M., & Jorgensen, T. D. (2021). Testing and Interpreting
Latent Variable Interactions Using the semTools Package. *Psych*,
*3*(3), 322–335. <https://doi.org/10.3390/psych3030024>

### What we already know/ or you don’t know :)

### Moderating eﬀect between two latent variables

-   There are several approaches to model latent interactions.

-   Cross-products on the observed variables, e.g., Kenny and
    Judd (1984) and Marsh, Wen, and Hau (2004).

    -   Whether to model all possible pairs of indicators;

    -   Whether to impose constraints on the factor loadings and error
        variances.

![](unconstrained_interaction.png)

-   Maximum likelihood estimation of latent interaction (e.g., Klein &
    Moosbrugger, 2000, LMS)

    -   No product term on the indicators;

    -   ML estimates are obtained by maximizing the log-likelihood of
        the joint distribution of indicators for the focal predictor and
        outcome, conditional on indicators of the moderator—essentially,
        a mixture distribution across the moderator indicators

    -   Only available in Mplus and nlsem package in R.

-   Quasi-ML estimator (QML, Klein and Muthén 2007)

    -   Less computationally intensive than LMS

    -   More robust to distributional assumptions

-   Markov chain Monte Carlo (MCMC) estimation

    -   Incorporate latent product terms directly into the model

    -   In a Bayesian framework, factor scores can be sampled from their
        posterior distribution at each iteration of the Markov chain,
        along with all other unknown parameters

    -   Only available in Mplus

![](comparison.png)

------------------------------------------------------------------------

### LMS and QML in R: nlsem

#### Let’s try lavaan() first

    my.med4 <- '
      tvalue =~ t1 + t2 + t3 + t4
      sources =~ s1 + s2 + s3 + s4 + s5
      tip =~ tip1 + tip2 + tip3
      tip ~ tvalue + sources + tvalue:sources
    '
    fit4 <- sem(my.med4, data=dat, se="bootstrap", bootstrap=200)

    ## Error in lavaan::lavaan(model = my.med4, data = dat, se = "bootstrap", : lavaan ERROR:
    ##     Interaction terms involving latent variables (tvalue:sources) are
    ##     not supported. You may consider creating product indicators to
    ##     define the latent interaction term. See the indProd() function in
    ##     the semTools package.

    summary(fit4, fit.measures = TRUE)

    ## Error in h(simpleError(msg, call)): error in evaluating the argument 'object' in selecting a method for function 'summary': object 'fit4' not found

    semPaths(fit4)

    ## Error in eval(expr, envir, enclos): object 'fit4' not found

    parameterEstimates(fit4)

    ## Error in eval(expr, envir, enclos): object 'fit4' not found

#### LMS or QML in nlsem

    library(nlsem)
    model5 <- '
      tvalue =~ t1 + t2 + t3 + t4
      sources =~ s1 + s2 + s3 + s4 + s5
      tip =~ tip1 + tip2 + tip3
      tip ~ tvalue + sources + tvalue:sources
    '
    nl_model5<- lav2nlsem(model5, constraints="direct1",class.spec="class")
    start <- runif(count_free_parameters(nl_model5))
    ## qml=FALSE, use LMS
    ## Notice NA
    em5 <- em(nl_model5, na.omit(dat[,2:13]), start, qml=TRUE)
    summary(em5)

------------------------------------------------------------------------

### Product–indicator approaches

-   Product indicators are assumed to be continuous by virtue of
    multiplying their values

    -   parceling strategy

-   The number of observed variables

#### Unconstrained approach

-   It is similar to creating cross-products. However, it skips some
    constaints.

-   It is called the unconstrained approach (Marsh, Wen, & Hau, 2004).

-   Model:

    -   y1 to y3: y = λ ∗fy + ey

    -   x1 to x6 are centered scores.

    -   x1 to x3: x = λ ∗f1 + e

    -   x4 to x6: x = λ ∗f2 + e

    -   x7 to x9: x = λ ∗ff1f2 + e where x7=x1\*x4, x8=x2\*x5 and
        x9=x3\*x6.

##### Mean-centering ﬁrst-order indicators (Marsh et al., 2004); matched-pairs strategy

    ## Center the items on the predictors
    ## Be careful! It replaces the original data.
    my.df <- read.csv("SEM01.csv", header=TRUE)
    my.df[, 4:9] <- as.data.frame(scale(my.df[, 4:9], scale=FALSE))
    ## Create product terms
    my.df$x7 <- with(my.df, x1*x4)
    my.df$x8 <- with(my.df, x2*x5)
    my.df$x9 <- with(my.df, x3*x6)

    my.model1 <- '## Measurement model on fy
                fy =~ y1 + y2 + y3
                ## Measurement model on f1 and f2
                f1 =~ x1 + x2 + x3
                f2 =~ x4 + x5 + x6
                ## Measurement model for the latent interaction
                f1f2 =~ x7 + x8 + x9

                ## Covariance between f1 and f2
                f1 ~~ cf1f2*f2
                ## Covariances between f1 and f1f2, and f2 and f1f2 are fixed at 0
                f1 ~~ 0*f1f2
                f2 ~~ 0*f1f2

                ## Intercepts on the f1 and f2
                f1 ~ 0*1
                f2 ~ 0*1
                ## Intercept on f1f2 is fixed at cov(f1,f2)
                f1f2 ~ cf1f2*1

                ## Intercepts on items are fixed at 0
                x1 ~ 0*1
                x2 ~ 0*1
                x3 ~ 0*1
                x4 ~ 0*1
                x5 ~ 0*1
                x6 ~ 0*1
                x7 ~ 0*1
                x8 ~ 0*1
                x9 ~ 0*1

                ## Structural model
                fy ~ f1 + f2 + f1f2'

    my.fit1 <- sem(my.model1, data=my.df)
    summary(my.fit1, fit.measures=TRUE, standardized=TRUE, rsquare=TRUE)

    ## lavaan 0.6.17 ended normally after 71 iterations
    ## 
    ##   Estimator                                         ML
    ##   Optimization method                           NLMINB
    ##   Number of model parameters                        32
    ##   Number of equality constraints                     1
    ## 
    ##   Number of observations                           500
    ## 
    ## Model Test User Model:
    ##                                                       
    ##   Test statistic                                44.805
    ##   Degrees of freedom                                59
    ##   P-value (Chi-square)                           0.914
    ## 
    ## Model Test Baseline Model:
    ## 
    ##   Test statistic                               647.120
    ##   Degrees of freedom                                66
    ##   P-value                                        0.000
    ## 
    ## User Model versus Baseline Model:
    ## 
    ##   Comparative Fit Index (CFI)                    1.000
    ##   Tucker-Lewis Index (TLI)                       1.027
    ## 
    ## Loglikelihood and Information Criteria:
    ## 
    ##   Loglikelihood user model (H0)              -8751.313
    ##   Loglikelihood unrestricted model (H1)      -8728.911
    ##                                                       
    ##   Akaike (AIC)                               17564.627
    ##   Bayesian (BIC)                             17695.280
    ##   Sample-size adjusted Bayesian (SABIC)      17596.884
    ## 
    ## Root Mean Square Error of Approximation:
    ## 
    ##   RMSEA                                          0.000
    ##   90 Percent confidence interval - lower         0.000
    ##   90 Percent confidence interval - upper         0.011
    ##   P-value H_0: RMSEA <= 0.050                    1.000
    ##   P-value H_0: RMSEA >= 0.080                    0.000
    ## 
    ## Standardized Root Mean Square Residual:
    ## 
    ##   SRMR                                           0.028
    ## 
    ## Parameter Estimates:
    ## 
    ##   Standard errors                             Standard
    ##   Information                                 Expected
    ##   Information saturated (h1) model          Structured
    ## 
    ## Latent Variables:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##   fy =~                                                                 
    ##     y1                1.000                               0.731    0.613
    ##     y2                1.259    0.113   11.120    0.000    0.920    0.721
    ##     y3                1.638    0.150   10.891    0.000    1.197    0.796
    ##   f1 =~                                                                 
    ##     x1                1.000                               0.440    0.423
    ##     x2                1.206    0.205    5.884    0.000    0.531    0.546
    ##     x3                1.582    0.293    5.389    0.000    0.696    0.678
    ##   f2 =~                                                                 
    ##     x4                1.000                               0.561    0.534
    ##     x5                1.036    0.246    4.218    0.000    0.581    0.579
    ##     x6                0.613    0.144    4.266    0.000    0.344    0.337
    ##   f1f2 =~                                                               
    ##     x7                1.000                               0.275    0.240
    ##     x8                1.887    0.850    2.221    0.026    0.519    0.559
    ##     x9                0.496    0.312    1.590    0.112    0.136    0.126
    ## 
    ## Regressions:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##   fy ~                                                                  
    ##     f1                0.149    0.116    1.284    0.199    0.089    0.089
    ##     f2                0.210    0.102    2.063    0.039    0.161    0.161
    ##     f1f2              0.491    0.273    1.799    0.072    0.185    0.185
    ## 
    ## Covariances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##   f1 ~~                                                                 
    ##     f2      (cf12)    0.056    0.019    2.881    0.004    0.227    0.227
    ##     f1f2              0.000                               0.000    0.000
    ##   f2 ~~                                                                 
    ##     f1f2              0.000                               0.000    0.000
    ## 
    ## Intercepts:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##     f1                0.000                               0.000    0.000
    ##     f2                0.000                               0.000    0.000
    ##     f1f2    (cf12)    0.056    0.019    2.881    0.004    0.204    0.204
    ##    .x1                0.000                               0.000    0.000
    ##    .x2                0.000                               0.000    0.000
    ##    .x3                0.000                               0.000    0.000
    ##    .x4                0.000                               0.000    0.000
    ##    .x5                0.000                               0.000    0.000
    ##    .x6                0.000                               0.000    0.000
    ##    .x7                0.000                               0.000    0.000
    ##    .x8                0.000                               0.000    0.000
    ##    .x9                0.000                               0.000    0.000
    ##    .y1                3.761    0.056   67.766    0.000    3.761    3.155
    ##    .y2                3.534    0.060   58.626    0.000    3.534    2.769
    ##    .y3                3.253    0.072   45.291    0.000    3.253    2.163
    ## 
    ## Variances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##    .y1                0.887    0.070   12.756    0.000    0.887    0.624
    ##    .y2                0.782    0.081    9.633    0.000    0.782    0.480
    ##    .y3                0.829    0.121    6.882    0.000    0.829    0.367
    ##    .x1                0.889    0.067   13.288    0.000    0.889    0.821
    ##    .x2                0.662    0.066    9.992    0.000    0.662    0.701
    ##    .x3                0.568    0.094    6.033    0.000    0.568    0.540
    ##    .x4                0.789    0.090    8.718    0.000    0.789    0.715
    ##    .x5                0.671    0.091    7.358    0.000    0.671    0.665
    ##    .x6                0.920    0.066   13.841    0.000    0.920    0.886
    ##    .x7                1.233    0.088   13.980    0.000    1.233    0.942
    ##    .x8                0.594    0.147    4.033    0.000    0.594    0.688
    ##    .x9                1.148    0.075   15.331    0.000    1.148    0.984
    ##    .fy                0.494    0.078    6.358    0.000    0.925    0.925
    ##     f1                0.194    0.053    3.686    0.000    1.000    1.000
    ##     f2                0.315    0.089    3.535    0.000    1.000    1.000
    ##     f1f2              0.076    0.044    1.730    0.084    1.000    1.000
    ## 
    ## R-Square:
    ##                    Estimate
    ##     y1                0.376
    ##     y2                0.520
    ##     y3                0.633
    ##     x1                0.179
    ##     x2                0.299
    ##     x3                0.460
    ##     x4                0.285
    ##     x5                0.335
    ##     x6                0.114
    ##     x7                0.058
    ##     x8                0.312
    ##     x9                0.016
    ##     fy                0.075

    semPaths(my.fit1, whatLabels="est", color="green")

![](Moderation-moderated_mediation_new_files/figure-markdown_strict/unnamed-chunk-21-1.png)

-   Although performing well in simulation, different pairs
    substantially alter the results in real data

#### DMC or residual centering is recommended

-   residual centering

    -   *x*<sub>*i*</sub>*z*<sub>*i*</sub> = *a*<sub>0</sub> + *a*<sub>1</sub>*x*<sub>*i*</sub> + *a*<sub>2</sub>*z*<sub>*i*</sub> + *r*<sub>*i*</sub>

    -   *r*<sub>*i*</sub> = *x*<sub>*i*</sub>*z*<sub>*i*</sub> − (*a*<sub>0</sub>+*a*<sub>1</sub>*x*<sub>*i*</sub>+*a*<sub>2</sub>*z*<sub>*i*</sub>)

    -   The residual r<sub>i</sub> (uncorrelated with lower-order terms)
        is used in place of the usual product term

-   DMC (double mean centering)

    -   combine the advantage of residual centering with the simplicity
        of mean centering

    -   DMC begins with mean centering

    -   After calculating product indicators, the product indicators are
        additionally mean centered

<!-- -->

    library(semTools)

    ## 

    ## ###############################################################################

    ## This is semTools 0.5-6

    ## All users of R (or SEM) are invited to submit functions or ideas for functions.

    ## ###############################################################################

    ## 
    ## Attaching package: 'semTools'

    ## The following objects are masked from 'package:psych':
    ## 
    ##     reliability, skew

    ## Compute product indicators (residual centering)

    #create names for interaction variables
    #not required, but makes syntax easier to write
    tvalue <- paste0("t", 1:4)
    sources <- paste0("s", 1:5)

    intNames <- paste0(rep(tvalue, each = length(sources)), sources)

    dat2 <- indProd(dat, var1 = c("t1", "t2", "t3", "t4"),
                    var2 = c("s1", "s2", "s3", "s4", "s5"),
                    match = FALSE, residualC = TRUE,
                    namesProd = intNames)

    ### Residual Centering interaction model

    modintRC <- '
    tvalue =~ t1 + t2 + t3 + t4
    sources =~ s1 + s2 + s3 + s4 + s5
    tip =~ tip1 + tip2 + tip3
    int =~ t1s1 + t1s2 + t1s3 + t1s4 + t1s5 +        
           t2s1 + t2s2 + t2s3 + t2s4 + t2s5 + 
           t3s1 + t3s2 + t3s3 + t3s4 + t3s5 + 
           t4s1 + t4s2 + t4s3 + t4s4 + t4s5 


    #Fix covariances between interaction and predictors to 0
    tvalue ~~ 0*int
    sources ~~ 0*int

    tip ~ tvalue + sources + int

    #Residual covariances between terms from the same indicator 
    #covariances between the same indicator are constrained to equality
    t1s1 ~~ th1*t1s2 + th1*t1s3 + th1*t1s4 + th1*t1s5
    t1s2 ~~ th1*t1s3 + th1*t1s4 + th1*t1s5
    t1s3 ~~ th1*t1s4 + th1*t1s5
    t1s4 ~~ th1*t1s5

    t2s1 ~~ th2*t2s2 + th2*t2s3 + th2*t2s4 + th2*t2s5
    t2s2 ~~ th2*t2s3 + th2*t2s4 + th2*t2s5
    t2s3 ~~ th2*t2s4 + th2*t2s5
    t2s4 ~~ th2*t2s5

    t3s1 ~~ th3*t3s2 + th3*t3s3 + th3*t3s4 + th3*t3s5
    t3s2 ~~ th3*t3s3 + th3*t3s4 + th3*t3s5
    t3s3 ~~ th3*t3s4 + th3*t3s5
    t3s4 ~~ th3*t3s5

    t4s1 ~~ th4*t4s2 + th4*t4s3 + th4*t4s4 + th4*t4s5
    t4s2 ~~ th4*t4s3 + th4*t4s4 + th4*t4s5
    t4s3 ~~ th4*t4s4 + th4*t4s5
    t4s4 ~~ th4*t4s5

    t1s1 ~~ th5*t2s1 + th5*t3s1 + th5*t4s1 
    t2s1 ~~ th5*t3s1 + th5*t4s1 
    t3s1 ~~ th5*t4s1 

    t1s2 ~~ th6*t2s2 + th6*t3s2 + th6*t4s2 
    t2s2 ~~ th6*t3s2 + th6*t4s2 
    t3s2 ~~ th6*t4s2 

    t1s3 ~~ th7*t2s3 + th7*t3s3 + th7*t4s3 
    t2s3 ~~ th7*t3s3 + th7*t4s3 
    t3s3 ~~ th7*t4s3 

    t1s4 ~~ th8*t2s4 + th8*t3s4 + th8*t4s4 
    t2s4 ~~ th8*t3s4 + th8*t4s4 
    t3s4 ~~ th8*t4s4 

    t1s5 ~~ th9*t2s5 + th9*t3s5 + th9*t4s5 
    t2s5 ~~ th9*t3s5 + th9*t4s5 
    t3s5 ~~ th9*t4s5 
    '

    fitintRC <- sem(modintRC, data = dat2, std.lv = TRUE, meanstructure = TRUE)
    summary(fitintRC, fit.measure = TRUE, standardized = TRUE)

    ## lavaan 0.6.17 ended normally after 51 iterations
    ## 
    ##   Estimator                                         ML
    ##   Optimization method                           NLMINB
    ##   Number of model parameters                       170
    ##   Number of equality constraints                    61
    ## 
    ##                                                   Used       Total
    ##   Number of observations                          7037        7314
    ## 
    ## Model Test User Model:
    ##                                                       
    ##   Test statistic                              8636.795
    ##   Degrees of freedom                               451
    ##   P-value (Chi-square)                           0.000
    ## 
    ## Model Test Baseline Model:
    ## 
    ##   Test statistic                            142820.384
    ##   Degrees of freedom                               496
    ##   P-value                                        0.000
    ## 
    ## User Model versus Baseline Model:
    ## 
    ##   Comparative Fit Index (CFI)                    0.942
    ##   Tucker-Lewis Index (TLI)                       0.937
    ## 
    ## Loglikelihood and Information Criteria:
    ## 
    ##   Loglikelihood user model (H0)            -286472.134
    ##   Loglikelihood unrestricted model (H1)    -282153.737
    ##                                                       
    ##   Akaike (AIC)                              573162.269
    ##   Bayesian (BIC)                            573909.893
    ##   Sample-size adjusted Bayesian (SABIC)     573563.516
    ## 
    ## Root Mean Square Error of Approximation:
    ## 
    ##   RMSEA                                          0.051
    ##   90 Percent confidence interval - lower         0.050
    ##   90 Percent confidence interval - upper         0.052
    ##   P-value H_0: RMSEA <= 0.050                    0.083
    ##   P-value H_0: RMSEA >= 0.080                    0.000
    ## 
    ## Standardized Root Mean Square Residual:
    ## 
    ##   SRMR                                           0.035
    ## 
    ## Parameter Estimates:
    ## 
    ##   Standard errors                             Standard
    ##   Information                                 Expected
    ##   Information saturated (h1) model          Structured
    ## 
    ## Latent Variables:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##   tvalue =~                                                             
    ##     t1                0.866    0.010   83.934    0.000    0.866    0.849
    ##     t2                0.856    0.010   89.777    0.000    0.856    0.890
    ##     t3                0.700    0.010   73.141    0.000    0.700    0.769
    ##     t4                0.565    0.013   44.117    0.000    0.565    0.516
    ##   sources =~                                                            
    ##     s1                0.757    0.012   60.565    0.000    0.757    0.687
    ##     s2                0.881    0.014   64.360    0.000    0.881    0.719
    ##     s3                0.651    0.014   45.599    0.000    0.651    0.545
    ##     s4                0.732    0.012   59.161    0.000    0.732    0.674
    ##     s5                0.969    0.013   73.532    0.000    0.969    0.795
    ##   tip =~                                                                
    ##     tip1              0.582    0.016   36.641    0.000    0.731    0.648
    ##     tip2              0.173    0.012   14.434    0.000    0.218    0.214
    ##     tip3              0.572    0.016   36.646    0.000    0.719    0.633
    ##   int =~                                                                
    ##     t1s1              0.669    0.014   46.845    0.000    0.669    0.578
    ##     t1s2              0.873    0.015   58.513    0.000    0.873    0.688
    ##     t1s3              0.591    0.015   38.633    0.000    0.591    0.491
    ##     t1s4              0.686    0.014   48.809    0.000    0.686    0.598
    ##     t1s5              0.961    0.015   65.346    0.000    0.961    0.748
    ##     t2s1              0.663    0.014   48.642    0.000    0.663    0.598
    ##     t2s2              0.848    0.014   60.113    0.000    0.848    0.704
    ##     t2s3              0.587    0.015   40.434    0.000    0.587    0.512
    ##     t2s4              0.677    0.014   50.017    0.000    0.677    0.611
    ##     t2s5              0.938    0.014   67.637    0.000    0.938    0.768
    ##     t3s1              0.537    0.014   38.354    0.000    0.537    0.484
    ##     t3s2              0.664    0.014   46.262    0.000    0.664    0.572
    ##     t3s3              0.483    0.015   32.158    0.000    0.483    0.413
    ##     t3s4              0.554    0.014   40.398    0.000    0.554    0.507
    ##     t3s5              0.745    0.014   52.698    0.000    0.745    0.641
    ##     t4s1              0.413    0.018   23.517    0.000    0.413    0.309
    ##     t4s2              0.501    0.019   27.030    0.000    0.501    0.355
    ##     t4s3              0.374    0.020   18.941    0.000    0.374    0.251
    ##     t4s4              0.444    0.017   25.668    0.000    0.444    0.336
    ##     t4s5              0.569    0.018   32.100    0.000    0.569    0.420
    ## 
    ## Regressions:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##   tip ~                                                                 
    ##     tvalue           -0.117    0.019   -6.070    0.000   -0.094   -0.094
    ##     sources           0.726    0.027   27.293    0.000    0.578    0.578
    ##     int              -0.069    0.020   -3.442    0.001   -0.055   -0.055
    ## 
    ## Covariances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##   tvalue ~~                                                             
    ##     int               0.000                               0.000    0.000
    ##   sources ~~                                                            
    ##     int               0.000                               0.000    0.000
    ##  .t1s1 ~~                                                               
    ##    .t1s2     (th1)    0.200    0.007   29.799    0.000    0.200    0.230
    ##    .t1s3     (th1)    0.200    0.007   29.799    0.000    0.200    0.201
    ##    .t1s4     (th1)    0.200    0.007   29.799    0.000    0.200    0.229
    ##    .t1s5     (th1)    0.200    0.007   29.799    0.000    0.200    0.248
    ##  .t1s2 ~~                                                               
    ##    .t1s3     (th1)    0.200    0.007   29.799    0.000    0.200    0.207
    ##    .t1s4     (th1)    0.200    0.007   29.799    0.000    0.200    0.236
    ##    .t1s5     (th1)    0.200    0.007   29.799    0.000    0.200    0.255
    ##  .t1s3 ~~                                                               
    ##    .t1s4     (th1)    0.200    0.007   29.799    0.000    0.200    0.207
    ##    .t1s5     (th1)    0.200    0.007   29.799    0.000    0.200    0.224
    ##  .t1s4 ~~                                                               
    ##    .t1s5     (th1)    0.200    0.007   29.799    0.000    0.200    0.255
    ##  .t2s1 ~~                                                               
    ##    .t2s2     (th2)    0.138    0.006   23.672    0.000    0.138    0.181
    ##    .t2s3     (th2)    0.138    0.006   23.672    0.000    0.138    0.157
    ##    .t2s4     (th2)    0.138    0.006   23.672    0.000    0.138    0.177
    ##    .t2s5     (th2)    0.138    0.006   23.672    0.000    0.138    0.198
    ##  .t2s2 ~~                                                               
    ##    .t2s3     (th2)    0.138    0.006   23.672    0.000    0.138    0.164
    ##    .t2s4     (th2)    0.138    0.006   23.672    0.000    0.138    0.184
    ##    .t2s5     (th2)    0.138    0.006   23.672    0.000    0.138    0.206
    ##  .t2s3 ~~                                                               
    ##    .t2s4     (th2)    0.138    0.006   23.672    0.000    0.138    0.160
    ##    .t2s5     (th2)    0.138    0.006   23.672    0.000    0.138    0.179
    ##  .t2s4 ~~                                                               
    ##    .t2s5     (th2)    0.138    0.006   23.672    0.000    0.138    0.201
    ##  .t3s1 ~~                                                               
    ##    .t3s2     (th3)    0.254    0.006   40.662    0.000    0.254    0.275
    ##    .t3s3     (th3)    0.254    0.006   40.662    0.000    0.254    0.246
    ##    .t3s4     (th3)    0.254    0.006   40.662    0.000    0.254    0.278
    ##    .t3s5     (th3)    0.254    0.006   40.662    0.000    0.254    0.293
    ##  .t3s2 ~~                                                               
    ##    .t3s3     (th3)    0.254    0.006   40.662    0.000    0.254    0.251
    ##    .t3s4     (th3)    0.254    0.006   40.662    0.000    0.254    0.283
    ##    .t3s5     (th3)    0.254    0.006   40.662    0.000    0.254    0.299
    ##  .t3s3 ~~                                                               
    ##    .t3s4     (th3)    0.254    0.006   40.662    0.000    0.254    0.253
    ##    .t3s5     (th3)    0.254    0.006   40.662    0.000    0.254    0.267
    ##  .t3s4 ~~                                                               
    ##    .t3s5     (th3)    0.254    0.006   40.662    0.000    0.254    0.302
    ##  .t4s1 ~~                                                               
    ##    .t4s2     (th4)    0.593    0.013   45.851    0.000    0.593    0.354
    ##    .t4s3     (th4)    0.593    0.013   45.851    0.000    0.593    0.324
    ##    .t4s4     (th4)    0.593    0.013   45.851    0.000    0.593    0.375
    ##    .t4s5     (th4)    0.593    0.013   45.851    0.000    0.593    0.380
    ##  .t4s2 ~~                                                               
    ##    .t4s3     (th4)    0.593    0.013   45.851    0.000    0.593    0.311
    ##    .t4s4     (th4)    0.593    0.013   45.851    0.000    0.593    0.360
    ##    .t4s5     (th4)    0.593    0.013   45.851    0.000    0.593    0.365
    ##  .t4s3 ~~                                                               
    ##    .t4s4     (th4)    0.593    0.013   45.851    0.000    0.593    0.330
    ##    .t4s5     (th4)    0.593    0.013   45.851    0.000    0.593    0.334
    ##  .t4s4 ~~                                                               
    ##    .t4s5     (th4)    0.593    0.013   45.851    0.000    0.593    0.387
    ##  .t1s1 ~~                                                               
    ##    .t2s1     (th5)    0.479    0.011   45.560    0.000    0.479    0.569
    ##    .t3s1     (th5)    0.479    0.011   45.560    0.000    0.479    0.522
    ##    .t4s1     (th5)    0.479    0.011   45.560    0.000    0.479    0.399
    ##  .t2s1 ~~                                                               
    ##    .t3s1     (th5)    0.479    0.011   45.560    0.000    0.479    0.556
    ##    .t4s1     (th5)    0.479    0.011   45.560    0.000    0.479    0.425
    ##  .t3s1 ~~                                                               
    ##    .t4s1     (th5)    0.479    0.011   45.560    0.000    0.479    0.390
    ##  .t1s2 ~~                                                               
    ##    .t2s2     (th6)    0.396    0.011   37.487    0.000    0.396    0.503
    ##    .t3s2     (th6)    0.396    0.011   37.487    0.000    0.396    0.452
    ##    .t4s2     (th6)    0.396    0.011   37.487    0.000    0.396    0.326
    ##  .t2s2 ~~                                                               
    ##    .t3s2     (th6)    0.396    0.011   37.487    0.000    0.396    0.486
    ##    .t4s2     (th6)    0.396    0.011   37.487    0.000    0.396    0.351
    ##  .t3s2 ~~                                                               
    ##    .t4s2     (th6)    0.396    0.011   37.487    0.000    0.396    0.315
    ##  .t1s3 ~~                                                               
    ##    .t2s3     (th7)    0.574    0.012   46.586    0.000    0.574    0.556
    ##    .t3s3     (th7)    0.574    0.012   46.586    0.000    0.574    0.514
    ##    .t4s3     (th7)    0.574    0.012   46.586    0.000    0.574    0.380
    ##  .t2s3 ~~                                                               
    ##    .t3s3     (th7)    0.574    0.012   46.586    0.000    0.574    0.549
    ##    .t4s3     (th7)    0.574    0.012   46.586    0.000    0.574    0.405
    ##  .t3s3 ~~                                                               
    ##    .t4s3     (th7)    0.574    0.012   46.586    0.000    0.574    0.375
    ##  .t1s4 ~~                                                               
    ##    .t2s4     (th8)    0.445    0.010   44.382    0.000    0.445    0.551
    ##    .t3s4     (th8)    0.445    0.010   44.382    0.000    0.445    0.513
    ##    .t4s4     (th8)    0.445    0.010   44.382    0.000    0.445    0.388
    ##  .t2s4 ~~                                                               
    ##    .t3s4     (th8)    0.445    0.010   44.382    0.000    0.445    0.539
    ##    .t4s4     (th8)    0.445    0.010   44.382    0.000    0.445    0.408
    ##  .t3s4 ~~                                                               
    ##    .t4s4     (th8)    0.445    0.010   44.382    0.000    0.445    0.379
    ##  .t1s5 ~~                                                               
    ##    .t2s5     (th9)    0.310    0.010   30.186    0.000    0.310    0.465
    ##    .t3s5     (th9)    0.310    0.010   30.186    0.000    0.310    0.407
    ##    .t4s5     (th9)    0.310    0.010   30.186    0.000    0.310    0.295
    ##  .t2s5 ~~                                                               
    ##    .t3s5     (th9)    0.310    0.010   30.186    0.000    0.310    0.443
    ##    .t4s5     (th9)    0.310    0.010   30.186    0.000    0.310    0.322
    ##  .t3s5 ~~                                                               
    ##    .t4s5     (th9)    0.310    0.010   30.186    0.000    0.310    0.282
    ##   tvalue ~~                                                             
    ##     sources          -0.191    0.013  -14.223    0.000   -0.191   -0.191
    ## 
    ## Intercepts:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##    .t1                2.137    0.012  175.782    0.000    2.137    2.095
    ##    .t2                1.995    0.011  174.038    0.000    1.995    2.075
    ##    .t3                1.987    0.011  183.098    0.000    1.987    2.183
    ##    .t4                2.535    0.013  194.217    0.000    2.535    2.315
    ##    .s1                3.917    0.013  298.071    0.000    3.917    3.553
    ##    .s2                3.104    0.015  212.563    0.000    3.104    2.534
    ##    .s3                2.947    0.014  207.222    0.000    2.947    2.470
    ##    .s4                3.986    0.013  308.100    0.000    3.986    3.673
    ##    .s5                3.433    0.015  236.370    0.000    3.433    2.818
    ##    .tip1              3.661    0.013  272.189    0.000    3.661    3.245
    ##    .tip2              3.631    0.012  299.769    0.000    3.631    3.573
    ##    .tip3              3.060    0.014  225.814    0.000    3.060    2.692
    ##    .t1s1              0.008    0.014    0.565    0.572    0.008    0.007
    ##    .t1s2              0.007    0.015    0.437    0.662    0.007    0.005
    ##    .t1s3              0.002    0.014    0.167    0.868    0.002    0.002
    ##    .t1s4              0.000    0.014    0.036    0.971    0.000    0.000
    ##    .t1s5              0.008    0.015    0.533    0.594    0.008    0.006
    ##    .t2s1              0.005    0.013    0.352    0.725    0.005    0.004
    ##    .t2s2              0.004    0.014    0.254    0.800    0.004    0.003
    ##    .t2s3              0.003    0.014    0.183    0.854    0.003    0.002
    ##    .t2s4              0.002    0.013    0.184    0.854    0.002    0.002
    ##    .t2s5              0.008    0.015    0.530    0.596    0.008    0.006
    ##    .t3s1              0.006    0.013    0.444    0.657    0.006    0.005
    ##    .t3s2              0.010    0.014    0.692    0.489    0.010    0.008
    ##    .t3s3              0.003    0.014    0.183    0.855    0.003    0.002
    ##    .t3s4              0.002    0.013    0.123    0.902    0.002    0.001
    ##    .t3s5              0.009    0.014    0.632    0.528    0.009    0.008
    ##    .t4s1              0.007    0.016    0.443    0.657    0.007    0.005
    ##    .t4s2              0.004    0.017    0.260    0.795    0.004    0.003
    ##    .t4s3             -0.000    0.018   -0.016    0.987   -0.000   -0.000
    ##    .t4s4              0.001    0.016    0.085    0.932    0.001    0.001
    ##    .t4s5              0.006    0.016    0.395    0.693    0.006    0.005
    ## 
    ## Variances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##    .t1                0.290    0.008   37.400    0.000    0.290    0.278
    ##    .t2                0.193    0.007   29.027    0.000    0.193    0.208
    ##    .t3                0.339    0.007   48.057    0.000    0.339    0.408
    ##    .t4                0.879    0.016   56.656    0.000    0.879    0.733
    ##    .s1                0.642    0.013   48.879    0.000    0.642    0.529
    ##    .s2                0.724    0.016   46.681    0.000    0.724    0.482
    ##    .s3                1.000    0.018   54.446    0.000    1.000    0.703
    ##    .s4                0.642    0.013   49.585    0.000    0.642    0.545
    ##    .s5                0.546    0.014   39.132    0.000    0.546    0.368
    ##    .tip1              0.738    0.022   33.969    0.000    0.738    0.580
    ##    .tip2              0.985    0.017   57.794    0.000    0.985    0.954
    ##    .tip3              0.775    0.022   35.795    0.000    0.775    0.600
    ##    .t1s1              0.894    0.014   66.114    0.000    0.894    0.666
    ##    .t1s2              0.847    0.014   60.203    0.000    0.847    0.526
    ##    .t1s3              1.102    0.016   69.204    0.000    1.102    0.759
    ##    .t1s4              0.848    0.013   64.952    0.000    0.848    0.643
    ##    .t1s5              0.725    0.014   53.462    0.000    0.725    0.440
    ##    .t2s1              0.791    0.013   61.800    0.000    0.791    0.643
    ##    .t2s2              0.732    0.013   55.519    0.000    0.732    0.504
    ##    .t2s3              0.968    0.015   64.837    0.000    0.968    0.738
    ##    .t2s4              0.769    0.012   61.673    0.000    0.769    0.626
    ##    .t2s5              0.612    0.013   48.175    0.000    0.612    0.410
    ##    .t3s1              0.940    0.013   70.776    0.000    0.940    0.766
    ##    .t3s2              0.907    0.014   66.430    0.000    0.907    0.673
    ##    .t3s3              1.130    0.016   72.458    0.000    1.130    0.829
    ##    .t3s4              0.889    0.013   69.621    0.000    0.889    0.743
    ##    .t3s5              0.796    0.013   60.542    0.000    0.796    0.589
    ##    .t4s1              1.608    0.020   78.905    0.000    1.608    0.904
    ##    .t4s2              1.743    0.023   76.602    0.000    1.743    0.874
    ##    .t4s3              2.076    0.026   80.686    0.000    2.076    0.937
    ##    .t4s4              1.551    0.020   77.961    0.000    1.551    0.887
    ##    .t4s5              1.514    0.021   72.587    0.000    1.514    0.824
    ##     tvalue            1.000                               1.000    1.000
    ##     sources           1.000                               1.000    1.000
    ##    .tip               1.000                               0.633    0.633
    ##     int               1.000                               1.000    1.000

    semPaths(fitintRC, whatLabels="est", color="green")

![](Moderation-moderated_mediation_new_files/figure-markdown_strict/unnamed-chunk-22-1.png)

    ## Compute product indicators (double mean centering)

    #create names
    #not required, but makes syntax easier to write
    tvalue <- paste0("t", 1:4)
    sources <- paste0("s", 1:5)

    intNames <- paste0(rep(tvalue, each = length(sources)), sources)

    dat3 <- indProd(dat, var1 = c("t1", "t2", "t3", "t4"),
                    var2 = c("s1", "s2", "s3", "s4", "s5"),
                    match = FALSE, doubleMC = TRUE,
                    namesProd = intNames)
    ## Mean Centering interaction model

    modintMC <- '
    tvalue =~ t1 + t2 + t3 + t4
    sources =~ s1 + s2 + s3 + s4 + s5
    tip =~ tip1 + tip2 + tip3
    int =~ t1s1 + t1s2 + t1s3 + t1s4 + t1s5 +        
           t2s1 + t2s2 + t2s3 + t2s4 + t2s5 + 
           t3s1 + t3s2 + t3s3 + t3s4 + t3s5 + 
           t4s1 + t4s2 + t4s3 + t4s4 + t4s5 


    tip ~ tvalue + sources + int

    #Residual covariances between terms from the same indicator 
    #covariances between the same indicator are constrained to equality
    t1s1 ~~ th1*t1s2 + th1*t1s3 + th1*t1s4 + th1*t1s5
    t1s2 ~~ th1*t1s3 + th1*t1s4 + th1*t1s5
    t1s3 ~~ th1*t1s4 + th1*t1s5
    t1s4 ~~ th1*t1s5

    t2s1 ~~ th2*t2s2 + th2*t2s3 + th2*t2s4 + th2*t2s5
    t2s2 ~~ th2*t2s3 + th2*t2s4 + th2*t2s5
    t2s3 ~~ th2*t2s4 + th2*t2s5
    t2s4 ~~ th2*t2s5

    t3s1 ~~ th3*t3s2 + th3*t3s3 + th3*t3s4 + th3*t3s5
    t3s2 ~~ th3*t3s3 + th3*t3s4 + th3*t3s5
    t3s3 ~~ th3*t3s4 + th3*t3s5
    t3s4 ~~ th3*t3s5

    t4s1 ~~ th4*t4s2 + th4*t4s3 + th4*t4s4 + th4*t4s5
    t4s2 ~~ th4*t4s3 + th4*t4s4 + th4*t4s5
    t4s3 ~~ th4*t4s4 + th4*t4s5
    t4s4 ~~ th4*t4s5

    t1s1 ~~ th5*t2s1 + th5*t3s1 + th5*t4s1 
    t2s1 ~~ th5*t3s1 + th5*t4s1 
    t3s1 ~~ th5*t4s1 

    t1s2 ~~ th6*t2s2 + th6*t3s2 + th6*t4s2 
    t2s2 ~~ th6*t3s2 + th6*t4s2 
    t3s2 ~~ th6*t4s2 

    t1s3 ~~ th7*t2s3 + th7*t3s3 + th7*t4s3 
    t2s3 ~~ th7*t3s3 + th7*t4s3 
    t3s3 ~~ th7*t4s3 

    t1s4 ~~ th8*t2s4 + th8*t3s4 + th8*t4s4 
    t2s4 ~~ th8*t3s4 + th8*t4s4 
    t3s4 ~~ th8*t4s4 

    t1s5 ~~ th9*t2s5 + th9*t3s5 + th9*t4s5 
    t2s5 ~~ th9*t3s5 + th9*t4s5 
    t3s5 ~~ th9*t4s5 
    '

    fitintMC <- sem(modintMC, data = dat3, std.lv = TRUE, meanstructure = TRUE)
    summary(fitintMC, fit.measure = TRUE, standardized = TRUE)

    ## lavaan 0.6.17 ended normally after 52 iterations
    ## 
    ##   Estimator                                         ML
    ##   Optimization method                           NLMINB
    ##   Number of model parameters                       172
    ##   Number of equality constraints                    61
    ## 
    ##                                                   Used       Total
    ##   Number of observations                          7037        7314
    ## 
    ## Model Test User Model:
    ##                                                       
    ##   Test statistic                              9457.709
    ##   Degrees of freedom                               449
    ##   P-value (Chi-square)                           0.000
    ## 
    ## Model Test Baseline Model:
    ## 
    ##   Test statistic                            144424.133
    ##   Degrees of freedom                               496
    ##   P-value                                        0.000
    ## 
    ## User Model versus Baseline Model:
    ## 
    ##   Comparative Fit Index (CFI)                    0.937
    ##   Tucker-Lewis Index (TLI)                       0.931
    ## 
    ## Loglikelihood and Information Criteria:
    ## 
    ##   Loglikelihood user model (H0)            -286882.592
    ##   Loglikelihood unrestricted model (H1)    -282153.737
    ##                                                       
    ##   Akaike (AIC)                              573987.183
    ##   Bayesian (BIC)                            574748.525
    ##   Sample-size adjusted Bayesian (SABIC)     574395.793
    ## 
    ## Root Mean Square Error of Approximation:
    ## 
    ##   RMSEA                                          0.053
    ##   90 Percent confidence interval - lower         0.052
    ##   90 Percent confidence interval - upper         0.054
    ##   P-value H_0: RMSEA <= 0.050                    0.000
    ##   P-value H_0: RMSEA >= 0.080                    0.000
    ## 
    ## Standardized Root Mean Square Residual:
    ## 
    ##   SRMR                                           0.039
    ## 
    ## Parameter Estimates:
    ## 
    ##   Standard errors                             Standard
    ##   Information                                 Expected
    ##   Information saturated (h1) model          Structured
    ## 
    ## Latent Variables:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##   tvalue =~                                                             
    ##     t1                0.866    0.010   83.911    0.000    0.866    0.849
    ##     t2                0.856    0.010   89.818    0.000    0.856    0.890
    ##     t3                0.701    0.010   73.198    0.000    0.701    0.769
    ##     t4                0.565    0.013   44.119    0.000    0.565    0.516
    ##   sources =~                                                            
    ##     s1                0.757    0.012   60.566    0.000    0.757    0.687
    ##     s2                0.881    0.014   64.355    0.000    0.881    0.719
    ##     s3                0.651    0.014   45.598    0.000    0.651    0.545
    ##     s4                0.732    0.012   59.164    0.000    0.732    0.674
    ##     s5                0.969    0.013   73.529    0.000    0.969    0.795
    ##   tip =~                                                                
    ##     tip1              0.583    0.016   36.647    0.000    0.731    0.648
    ##     tip2              0.173    0.012   14.436    0.000    0.218    0.214
    ##     tip3              0.573    0.016   36.651    0.000    0.719    0.633
    ##   int =~                                                                
    ##     t1s1              0.676    0.014   47.219    0.000    0.676    0.581
    ##     t1s2              0.873    0.015   58.395    0.000    0.873    0.686
    ##     t1s3              0.598    0.015   38.899    0.000    0.598    0.493
    ##     t1s4              0.697    0.014   49.348    0.000    0.697    0.602
    ##     t1s5              0.966    0.015   65.629    0.000    0.966    0.750
    ##     t2s1              0.670    0.014   49.083    0.000    0.670    0.601
    ##     t2s2              0.847    0.014   59.906    0.000    0.847    0.701
    ##     t2s3              0.594    0.015   40.671    0.000    0.594    0.514
    ##     t2s4              0.691    0.014   50.697    0.000    0.691    0.617
    ##     t2s5              0.945    0.014   67.928    0.000    0.945    0.769
    ##     t3s1              0.547    0.014   38.989    0.000    0.547    0.491
    ##     t3s2              0.668    0.014   46.378    0.000    0.668    0.572
    ##     t3s3              0.492    0.015   32.609    0.000    0.492    0.418
    ##     t3s4              0.569    0.014   41.263    0.000    0.569    0.516
    ##     t3s5              0.757    0.014   53.348    0.000    0.757    0.646
    ##     t4s1              0.419    0.018   23.828    0.000    0.419    0.313
    ##     t4s2              0.502    0.019   27.007    0.000    0.502    0.354
    ##     t4s3              0.380    0.020   19.161    0.000    0.380    0.254
    ##     t4s4              0.456    0.017   26.208    0.000    0.456    0.342
    ##     t4s5              0.576    0.018   32.435    0.000    0.576    0.423
    ## 
    ## Regressions:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##   tip ~                                                                 
    ##     tvalue           -0.125    0.020   -6.369    0.000   -0.099   -0.099
    ##     sources           0.725    0.027   27.273    0.000    0.577    0.577
    ##     int              -0.060    0.020   -2.960    0.003   -0.048   -0.048
    ## 
    ## Covariances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##  .t1s1 ~~                                                               
    ##    .t1s2     (th1)    0.201    0.007   29.912    0.000    0.201    0.229
    ##    .t1s3     (th1)    0.201    0.007   29.912    0.000    0.201    0.201
    ##    .t1s4     (th1)    0.201    0.007   29.912    0.000    0.201    0.230
    ##    .t1s5     (th1)    0.201    0.007   29.912    0.000    0.201    0.249
    ##  .t1s2 ~~                                                               
    ##    .t1s3     (th1)    0.201    0.007   29.912    0.000    0.201    0.206
    ##    .t1s4     (th1)    0.201    0.007   29.912    0.000    0.201    0.235
    ##    .t1s5     (th1)    0.201    0.007   29.912    0.000    0.201    0.255
    ##  .t1s3 ~~                                                               
    ##    .t1s4     (th1)    0.201    0.007   29.912    0.000    0.201    0.206
    ##    .t1s5     (th1)    0.201    0.007   29.912    0.000    0.201    0.223
    ##  .t1s4 ~~                                                               
    ##    .t1s5     (th1)    0.201    0.007   29.912    0.000    0.201    0.255
    ##  .t2s1 ~~                                                               
    ##    .t2s2     (th2)    0.141    0.006   24.007    0.000    0.141    0.183
    ##    .t2s3     (th2)    0.141    0.006   24.007    0.000    0.141    0.159
    ##    .t2s4     (th2)    0.141    0.006   24.007    0.000    0.141    0.179
    ##    .t2s5     (th2)    0.141    0.006   24.007    0.000    0.141    0.201
    ##  .t2s2 ~~                                                               
    ##    .t2s3     (th2)    0.141    0.006   24.007    0.000    0.141    0.165
    ##    .t2s4     (th2)    0.141    0.006   24.007    0.000    0.141    0.185
    ##    .t2s5     (th2)    0.141    0.006   24.007    0.000    0.141    0.208
    ##  .t2s3 ~~                                                               
    ##    .t2s4     (th2)    0.141    0.006   24.007    0.000    0.141    0.161
    ##    .t2s5     (th2)    0.141    0.006   24.007    0.000    0.141    0.181
    ##  .t2s4 ~~                                                               
    ##    .t2s5     (th2)    0.141    0.006   24.007    0.000    0.141    0.204
    ##  .t3s1 ~~                                                               
    ##    .t3s2     (th3)    0.255    0.006   40.503    0.000    0.255    0.274
    ##    .t3s3     (th3)    0.255    0.006   40.503    0.000    0.255    0.245
    ##    .t3s4     (th3)    0.255    0.006   40.503    0.000    0.255    0.277
    ##    .t3s5     (th3)    0.255    0.006   40.503    0.000    0.255    0.293
    ##  .t3s2 ~~                                                               
    ##    .t3s3     (th3)    0.255    0.006   40.503    0.000    0.255    0.249
    ##    .t3s4     (th3)    0.255    0.006   40.503    0.000    0.255    0.281
    ##    .t3s5     (th3)    0.255    0.006   40.503    0.000    0.255    0.298
    ##  .t3s3 ~~                                                               
    ##    .t3s4     (th3)    0.255    0.006   40.503    0.000    0.255    0.252
    ##    .t3s5     (th3)    0.255    0.006   40.503    0.000    0.255    0.266
    ##  .t3s4 ~~                                                               
    ##    .t3s5     (th3)    0.255    0.006   40.503    0.000    0.255    0.301
    ##  .t4s1 ~~                                                               
    ##    .t4s2     (th4)    0.596    0.013   45.846    0.000    0.596    0.354
    ##    .t4s3     (th4)    0.596    0.013   45.846    0.000    0.596    0.324
    ##    .t4s4     (th4)    0.596    0.013   45.846    0.000    0.596    0.375
    ##    .t4s5     (th4)    0.596    0.013   45.846    0.000    0.596    0.381
    ##  .t4s2 ~~                                                               
    ##    .t4s3     (th4)    0.596    0.013   45.846    0.000    0.596    0.311
    ##    .t4s4     (th4)    0.596    0.013   45.846    0.000    0.596    0.360
    ##    .t4s5     (th4)    0.596    0.013   45.846    0.000    0.596    0.365
    ##  .t4s3 ~~                                                               
    ##    .t4s4     (th4)    0.596    0.013   45.846    0.000    0.596    0.329
    ##    .t4s5     (th4)    0.596    0.013   45.846    0.000    0.596    0.334
    ##  .t4s4 ~~                                                               
    ##    .t4s5     (th4)    0.596    0.013   45.846    0.000    0.596    0.387
    ##  .t1s1 ~~                                                               
    ##    .t2s1     (th5)    0.480    0.011   45.508    0.000    0.480    0.569
    ##    .t3s1     (th5)    0.480    0.011   45.508    0.000    0.480    0.522
    ##    .t4s1     (th5)    0.480    0.011   45.508    0.000    0.480    0.399
    ##  .t2s1 ~~                                                               
    ##    .t3s1     (th5)    0.480    0.011   45.508    0.000    0.480    0.555
    ##    .t4s1     (th5)    0.480    0.011   45.508    0.000    0.480    0.424
    ##  .t3s1 ~~                                                               
    ##    .t4s1     (th5)    0.480    0.011   45.508    0.000    0.480    0.389
    ##  .t1s2 ~~                                                               
    ##    .t2s2     (th6)    0.403    0.011   37.918    0.000    0.403    0.506
    ##    .t3s2     (th6)    0.403    0.011   37.918    0.000    0.403    0.455
    ##    .t4s2     (th6)    0.403    0.011   37.918    0.000    0.403    0.329
    ##  .t2s2 ~~                                                               
    ##    .t3s2     (th6)    0.403    0.011   37.918    0.000    0.403    0.489
    ##    .t4s2     (th6)    0.403    0.011   37.918    0.000    0.403    0.353
    ##  .t3s2 ~~                                                               
    ##    .t4s2     (th6)    0.403    0.011   37.918    0.000    0.403    0.318
    ##  .t1s3 ~~                                                               
    ##    .t2s3     (th7)    0.584    0.013   46.672    0.000    0.584    0.558
    ##    .t3s3     (th7)    0.584    0.013   46.672    0.000    0.584    0.518
    ##    .t4s3     (th7)    0.584    0.013   46.672    0.000    0.584    0.382
    ##  .t2s3 ~~                                                               
    ##    .t3s3     (th7)    0.584    0.013   46.672    0.000    0.584    0.551
    ##    .t4s3     (th7)    0.584    0.013   46.672    0.000    0.584    0.407
    ##  .t3s3 ~~                                                               
    ##    .t4s3     (th7)    0.584    0.013   46.672    0.000    0.584    0.378
    ##  .t1s4 ~~                                                               
    ##    .t2s4     (th8)    0.449    0.010   44.278    0.000    0.449    0.552
    ##    .t3s4     (th8)    0.449    0.010   44.278    0.000    0.449    0.514
    ##    .t4s4     (th8)    0.449    0.010   44.278    0.000    0.449    0.389
    ##  .t2s4 ~~                                                               
    ##    .t3s4     (th8)    0.449    0.010   44.278    0.000    0.449    0.539
    ##    .t4s4     (th8)    0.449    0.010   44.278    0.000    0.449    0.408
    ##  .t3s4 ~~                                                               
    ##    .t4s4     (th8)    0.449    0.010   44.278    0.000    0.449    0.380
    ##  .t1s5 ~~                                                               
    ##    .t2s5     (th9)    0.309    0.010   30.098    0.000    0.309    0.462
    ##    .t3s5     (th9)    0.309    0.010   30.098    0.000    0.309    0.406
    ##    .t4s5     (th9)    0.309    0.010   30.098    0.000    0.309    0.294
    ##  .t2s5 ~~                                                               
    ##    .t3s5     (th9)    0.309    0.010   30.098    0.000    0.309    0.441
    ##    .t4s5     (th9)    0.309    0.010   30.098    0.000    0.309    0.320
    ##  .t3s5 ~~                                                               
    ##    .t4s5     (th9)    0.309    0.010   30.098    0.000    0.309    0.281
    ##   tvalue ~~                                                             
    ##     sources          -0.191    0.013  -14.224    0.000   -0.191   -0.191
    ##     int              -0.115    0.014   -8.131    0.000   -0.115   -0.115
    ##   sources ~~                                                            
    ##     int               0.004    0.015    0.274    0.784    0.004    0.004
    ## 
    ## Intercepts:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##    .t1                2.137    0.012  175.782    0.000    2.137    2.095
    ##    .t2                1.995    0.011  174.038    0.000    1.995    2.075
    ##    .t3                1.987    0.011  183.098    0.000    1.987    2.183
    ##    .t4                2.535    0.013  194.217    0.000    2.535    2.315
    ##    .s1                3.917    0.013  298.071    0.000    3.917    3.553
    ##    .s2                3.104    0.015  212.563    0.000    3.104    2.534
    ##    .s3                2.947    0.014  207.222    0.000    2.947    2.470
    ##    .s4                3.986    0.013  308.100    0.000    3.986    3.673
    ##    .s5                3.433    0.015  236.370    0.000    3.433    2.818
    ##    .tip1              3.661    0.013  272.192    0.000    3.661    3.245
    ##    .tip2              3.631    0.012  299.770    0.000    3.631    3.574
    ##    .tip3              3.060    0.014  225.816    0.000    3.060    2.692
    ##    .t1s1              0.004    0.014    0.255    0.799    0.004    0.003
    ##    .t1s2              0.005    0.015    0.307    0.759    0.005    0.004
    ##    .t1s3              0.001    0.014    0.089    0.929    0.001    0.001
    ##    .t1s4             -0.003    0.014   -0.248    0.804   -0.003   -0.003
    ##    .t1s5              0.005    0.015    0.356    0.722    0.005    0.004
    ##    .t2s1              0.001    0.013    0.065    0.948    0.001    0.001
    ##    .t2s2              0.002    0.014    0.130    0.897    0.002    0.002
    ##    .t2s3              0.001    0.014    0.082    0.935    0.001    0.001
    ##    .t2s4             -0.001    0.013   -0.104    0.917   -0.001   -0.001
    ##    .t2s5              0.005    0.015    0.361    0.718    0.005    0.004
    ##    .t3s1              0.002    0.013    0.161    0.872    0.002    0.002
    ##    .t3s2              0.007    0.014    0.530    0.596    0.007    0.006
    ##    .t3s3              0.001    0.014    0.084    0.933    0.001    0.001
    ##    .t3s4             -0.002    0.013   -0.152    0.879   -0.002   -0.002
    ##    .t3s5              0.006    0.014    0.407    0.684    0.006    0.005
    ##    .t4s1              0.004    0.016    0.272    0.786    0.004    0.003
    ##    .t4s2              0.003    0.017    0.163    0.871    0.003    0.002
    ##    .t4s3             -0.002    0.018   -0.112    0.911   -0.002   -0.001
    ##    .t4s4             -0.001    0.016   -0.069    0.945   -0.001   -0.001
    ##    .t4s5              0.004    0.016    0.265    0.791    0.004    0.003
    ## 
    ## Variances:
    ##                    Estimate  Std.Err  z-value  P(>|z|)   Std.lv  Std.all
    ##    .t1                0.290    0.008   37.516    0.000    0.290    0.279
    ##    .t2                0.193    0.007   29.079    0.000    0.193    0.208
    ##    .t3                0.338    0.007   48.045    0.000    0.338    0.408
    ##    .t4                0.879    0.016   56.660    0.000    0.879    0.733
    ##    .s1                0.642    0.013   48.876    0.000    0.642    0.529
    ##    .s2                0.724    0.016   46.682    0.000    0.724    0.482
    ##    .s3                1.000    0.018   54.445    0.000    1.000    0.703
    ##    .s4                0.642    0.013   49.582    0.000    0.642    0.545
    ##    .s5                0.546    0.014   39.131    0.000    0.546    0.368
    ##    .tip1              0.738    0.022   33.943    0.000    0.738    0.580
    ##    .tip2              0.985    0.017   57.792    0.000    0.985    0.954
    ##    .tip3              0.775    0.022   35.780    0.000    0.775    0.600
    ##    .t1s1              0.898    0.014   66.105    0.000    0.898    0.663
    ##    .t1s2              0.857    0.014   60.610    0.000    0.857    0.529
    ##    .t1s3              1.115    0.016   69.235    0.000    1.115    0.757
    ##    .t1s4              0.854    0.013   64.875    0.000    0.854    0.637
    ##    .t1s5              0.727    0.014   53.490    0.000    0.727    0.438
    ##    .t2s1              0.795    0.013   61.839    0.000    0.795    0.639
    ##    .t2s2              0.742    0.013   56.017    0.000    0.742    0.508
    ##    .t2s3              0.984    0.015   64.989    0.000    0.984    0.736
    ##    .t2s4              0.776    0.013   61.634    0.000    0.776    0.619
    ##    .t2s5              0.616    0.013   48.303    0.000    0.616    0.408
    ##    .t3s1              0.942    0.013   70.641    0.000    0.942    0.759
    ##    .t3s2              0.915    0.014   66.642    0.000    0.915    0.673
    ##    .t3s3              1.143    0.016   72.364    0.000    1.143    0.825
    ##    .t3s4              0.895    0.013   69.411    0.000    0.895    0.734
    ##    .t3s5              0.800    0.013   60.471    0.000    0.800    0.583
    ##    .t4s1              1.616    0.020   78.879    0.000    1.616    0.902
    ##    .t4s2              1.756    0.023   76.774    0.000    1.756    0.875
    ##    .t4s3              2.096    0.026   80.758    0.000    2.096    0.936
    ##    .t4s4              1.566    0.020   77.952    0.000    1.566    0.883
    ##    .t4s5              1.519    0.021   72.521    0.000    1.519    0.821
    ##     tvalue            1.000                               1.000    1.000
    ##     sources           1.000                               1.000    1.000
    ##    .tip               1.000                               0.634    0.634
    ##     int               1.000                               1.000    1.000

## The desert: Moderated mediation

    ## Use bootstrapLavaan() by writing a function that takes the simple slopes 
    ## from probe2WayMC() and calculates the indirect effects manually in that 
    ## bootstrap sample.  
    ## From the help-page example, treating the latent outcome as a mediator and 
    ## adding an additional arbitrary outcome:
    library(semTools)
      
      dat2wayMC <- indProd(dat2way, 1:3, 4:6)
      dat2wayMC$newDV <- rowMeans(dat2way) + rnorm(nrow(dat2way))
      model1 <- "
    f1 =~ x1 + x2 + x3
    f2 =~ x4 + x5 + x6
    f12 =~ x1.x4 + x2.x5 + x3.x6
    f3 =~ x7 + x8 + x9
    f3 ~ f1 + f2 + f12
    f12 ~~ 0*f1 + 0*f2
    x1 + x4 + x1.x4 + x7 ~ 0*1 # identify latent means
    f1 + f2 + f12 + f3 ~ NA*1
    newDV ~ b*f3
    "
      fitMC2way <- sem(model1, data = dat2wayMC, meanstructure = TRUE)
      probe <- probe2WayMC(fitMC2way, nameX = c("f1", "f2", "f12"), 
                           nameY = "f3", modVar = "f2", valProbe = c(-1, 0, 1))
      probe$SimpleSlope

    ##   f2   est    se      z pvalue
    ## 1 -1 0.294 0.015 20.081      0
    ## 2  0 0.436 0.012 35.238      0
    ## 3  1 0.578 0.016 36.182      0

      ## conditional indirect effects on newDV
      probe$SimpleSlope$est * coef(fitMC2way)[["b"]]

    ## [1] 0.3531317 0.5239150 0.6946983

      ## custom function to return simple slopes
      condIndFX <- function(fit) {
        condFX <- probe2WayMC(fit, nameX = c("f1", "f2", "f12"), nameY = "f3",
                              modVar = "f2", valProbe = c(-1, 0, 1))
        indFX <- condFX$SimpleSlope$est * coef(fit)[["b"]]
        names(indFX) <- paste("f2 =", condFX$SimpleSlope$f2)
        indFX
      }
      ## test once on original data
      condIndFX(fitMC2way)

    ##   f2 = -1    f2 = 0    f2 = 1 
    ## 0.3531317 0.5239150 0.6946983

      ## (too small) bootstrap sample of simple slopes
      bootOut <- bootstrapLavaan(fitMC2way, R = 100, FUN = condIndFX)
      ## percentile 95% CI
      apply(bootOut, MARGIN = 2, FUN = quantile, probs = c(.025, .975))

    ##         f2 = -1    f2 = 0   f2 = 1
    ## 2.5%  0.3150987 0.5014642 0.664913
    ## 97.5% 0.3753435 0.5450563 0.724830
