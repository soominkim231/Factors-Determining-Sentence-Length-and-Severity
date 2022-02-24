# Factors-Determining-Sentence-Length-and-Severity
Overview: Factors Determining Sentence Length and Severity for U.S. Inmates using R, multivariate linear regression, and logistic regression.

## 1. Question
Of demographic and case-specific factors, which are significant predictors of sentence length and severity?

## 2. Background Research
The American criminal justice system has long been criticized for racial disparities in its sentencing, and federal sentencing guidelines have been introduced to minimize race-based disparities. However, some studies have found that even after these guidelines have been implemented, African Americans, Hispanics, and Native Americans still receive harsher sentences than their white counterparts and that these differences can only partly be explained by differences in the characteristics of their individual offenses (Everett 2002). Multiple studies have produced similar results, including the work of Doerner et al, who found that Hispanics and Blacks receive harsher sentences even after controlling for other factors. Doerner’s work also includes the dimension of age and concludes that younger defendants are more likely to receive longer sentences than older defendants. Interestingly, a recent study, published in 2017 by Ryon et al, focused on age specifically, finds that there is a relationship between age and sentencing, but that it is non-linear, and that those who benefit most from age-related leniency in sentencing are only the youngest and oldest offenders at the extreme ends of the spectrum. However, none of the literature we have encountered thus far explores the specific difference between indefinite sentences (life in prison or death row) vs definite sentences (expected to be completed within the defendant’s lifetime).

## 3. Data (Overview):
We are examining a set of variables from the 2004 Survey of Inmates taken from the Bureau of Justice Statistics. This survey was conducted through personal interviews of a nationally representative sample of inmates across both State and Federal prisons. The survey included responses to questions about demographics, criminal history, sentencing, gender, drug use, and prisoner health. 

## 4. Hypothesis
* H1: Black defendants can expect to receive longer sentences than white defendants.
* H2: The effect of being a violent offender, as opposed to being a non-violent offender on sentence length is greater for Black defendants as opposed to white defendants.
* H3: Older defendants are less likely to receive an extreme sentence (life in prison or the death penalty) than younger defendants. 

## 5. Data and Method
- For H1 & H2, we use a **multivariate linear regression** with an interaction term Black x violent offense as one of the independent variables and length of sentencing (in months) as the dependent variable.
-   Given the heavy right skew of our dependent variable (with 6% classified as outliers and increasing the mean sentencing by 30 months), we estimated three OLS models, with varying degrees of outlier removal. We decided to use the most restrictive model (excluding sentences greater than 2000 months), given that its R-squared value is slightly higher, meaning more variation is explained by the model. Importantly, none of the predictors change in sign or statistical significance between the three models, so the choice of which one to use was not critical.
-   We calculated pairwise correlations between the predictors to test for multicollinearity.
- To study H3, we use a **binomial logit model**. The independent variable of interest is Black and the dependent variable is whether a defendant receives an extreme sentencing (either life in prison or the death penalty). 
-   We created a subset of the data that only included entries for violent offenses, not drug or property offenses, because violent offenses are a near perfect predictor for receiving extreme sentences. 

## 6.1 Discussion on H1 & H2:
- What we are interested in is whether there is a difference in sentence length (in months) for Black vs white offenders conditional on their offense being violent. In our chosen linear model (the rightmost model), the interaction term Black x Violent Offense is statistically significant, confirming our second hypothesis that sentence length increases for the offense being violent is dependent on the offender being Black or white.
- The effect of being a violent offender (not changing the race of the offender from the baseline of being white) induces an increase of 107.5 months. This effect is statistically significant, with a p-value of <2e-16.
- The effect of being black and a violent offender is 129.4 months – 22 months greater than their white counterparts. This effect is statistically significant, with a p-value of 4.91e-160. Note that this p-value was calculated from the standard error found using the variance covariance matrix.
- In regards to our first hypothesis that black defendants can expect to receive longer sentences, our model showed that the isolated effect of being black is a 4.75 month increase, but this result is not statistically significant.

## 6.2 Discussion on H3:
- The binomial logit model showed that only two predictors are statistically significant in predicting whether or not a defendant receives an extreme sentence, those two being Black (β = 0.29) and age (β = 0.42).
- We were intrigued by the significance of age, and plotted the predicted probability of receiving an extreme sentence as a function of age across the five categories given in our data set. As observed in the graph, the Pr(extreme sentence) increases as the defendant’s age increases. To examine the substantive significance, we calculated the change in Pr(extreme sentence) when we change the age category for a hypothetical profile of a defendant. As seen in our table, the predicted probability increases by at least .05 for each unit increase to the subsequent age category. This is a substantively significant effect. In addition, the change in the predicted probability increases as age category increases – e.g., a defendant that is 25-34 as opposed to <25 sees a .05982 increase in their Pr(extreme sentence), but when that increase is to 65-96 from 55-64, that change in probability is as high as .10.
- In conclusion, our findings disprove our final hypothesis that older defendants are more likely to receive an extreme sentence than younger ones. This is because age was a statistically significant predictor of receiving an extreme sentence, but in the direction of older defendants being more likely to receive an extreme sentence. This effect was also determined to be substantively significant.

Please find multivariate linear regression model [code](https://github.com/soominkim231/Factors-Determining-Sentence-Length-and-Severity/blob/main/Final_LinearModel_Submission.rmd), binomial logit model [code](https://github.com/soominkim231/Factors-Determining-Sentence-Length-and-Severity/blob/main/Final_LogitModel_Submission.rmd), [result plots and tables](https://github.com/soominkim231/Factors-Determining-Sentence-Length-and-Severity/tree/main/plots), and [final poster](https://github.com/soominkim231/Factors-Determining-Sentence-Length-and-Severity/blob/main/GOV19_Poster.pdf).

*Works Cited:*
- Doerner, Jill K.; Demuth, Stephen. "The Independent and Joint Effects of Race/Ethnicity, Gender, and Age on Sentencing Outcomes in U.S. Federal Courts," Justice Quarterly vol. 27, no. 1 (February 2010): p. 1-27. 
- Everett, Ronald S; Wojtkiewicz, Roger A. (2002). Difference, Disparity, and Race/Ethnic Bias in Federal Sentencing: [1]. Journal of Quantitative Criminology; New York Vol. 18, Iss. 2, 189-211.
- Ryon, Stephanie Bontrager, et al. “Sentencing in Light of Collateral Consequences: Does Age Matter?” Journal of Criminal Justice, vol. 53, 2017, pp. 1–11., doi:10.1016/j.jcrimjus.2017.07.009.
- Wu, J., & Spohn, C. (2009). Does an Offender’s Age Have an Effect on Sentence Length? Criminal Justice Policy Review,20(4), 379-413.
