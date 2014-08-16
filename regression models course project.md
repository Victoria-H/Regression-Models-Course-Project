Regression Models Course Project (part of the Coursera Data Science specialisation from John Hopkins, Baltimore, taught by Brian Caffo PhD) - Victoria Hunt, August 2014

Analysis of the effect of automatic vs manual transmission on fuel efficiency for motorvehicles using the R data set "mtcars"
---------------------------------------------------------------------------------------

### Executive Summary / Abstract
The mtcars data was extracted from the 1974 Motor Trend US magazine, and comprises fuel consumption and 10 aspects of automobile design and performance for 32 automobiles (1973-74 models). This report explores the relationship between these design aspects and fuel consumption, miles per gallon (mpg) using linear regression models. In particular, the link between automatic and manual transmission and mpg will be be investigated.
Due to the small size and composition of the data set, statistically significant and quantifiable relationships between transmission and mpg are difficult to establish, nevertheless some insight can be gained into the effect of vehicle construction and fuel consumption.



### Introduction
As part of a module on Regression Models, we were asked to use the mtcars data set and R to investigate the effect of automatic vs manual transition on fuel consumption for motor vehicles.The context given in the assignment was as follows:-   
_You work   for Motor Trend, a magazine about the automobile industry. Looking at a data set of a collection of cars, they are interested in exploring the relationship between a set of variables and miles per gallon (MPG) (outcome). They are particularly interested in the following two questions:_

* _"Is an automatic or manual transmission better for MPG?"_
* _"Quantify the MPG difference between automatic and manual transmissions"_


The objective was therefore to use linear regression to investigate the above questions. Limitations of the data set (representitavness, sample size) should be noted.


To access the data set in R type  `mtcars` and consult  `help(mtcars)` for more information and the list of variables.



### Method
The general method employed in this report is as follows


* Exploratory data analysis - Overview of the data, identification of the main factors which influence mpg

* Choice of model - A parsimonius model with easily interpretable coefficients

* Diagnostics - Residual analysis and identification of any observations that have large infuence on the model chosen.

* R  - All analysis is done using R and this dynamic Rmd document available here contains  reproducible code. knitR package used to convert to html.

### Results

The dataset comprises observations on 32 vehicles, 19 of which have automatic transmission.



```
           n mean.mpg
automatic 19    17.15
manual    13    24.39
```
In this data set, the manual cars have a higher mpg on average than the automatic cars however this does not in any way imply a causal relationship as other aspects affecting mpg must be taken into account.

Fig1 shows the relationship between mpg and all 10 aspects. Below each graph is the coefficient of determination obtained when regressing mpg with the variable alone. It enables us to have a preliminary idea of which variables have most impact on mpg. R^2 indicates the change in mpg that is explained by that variable.  






We can see that the potential variables of interest are weight, number of cylinders, displacement, and gross horse power.  

Fig2 : Running an exploratory linear regression of mpg against these variables + transmission we can see by looking at the p values ascociated with the coefficients that this model indicates weight and horse power are significantly explanatory of mpg. Although this model yields a high R^2 it should be simplified to ease interpretation and avoid variance inflation. It is already clear that the effect of transmission on mpg is not ovbious from this dataset, but as we are particularly interested in this variable we must continue to consider it in our model. 



Fig3 : Looking a a series of nested regression models the ANOVA output table1 indicates that regressing mpg angainst both weight and horsepower is valid. The addition  of transmission (am) does not significantly improve the model. Nor does the inclusion of displacement and number of cylinders.



However our principal concern is the effect of transmission. The second ANOVA table2 fig.3 confirms that we should also include weight and horsepower as regressors if we include transmission.

__Hence the model mpg~weight+horse power+transmission(am)  was chosen with output...__

fig4 - CHOSEN MODEL


```

Call:
lm(formula = mpg ~ am + wt + hp, data = mtcars)

Residuals:
   Min     1Q Median     3Q    Max 
-3.422 -1.792 -0.379  1.225  5.532 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 34.00288    2.64266   12.87  2.8e-13 ***
am1          2.08371    1.37642    1.51  0.14127    
wt          -2.87858    0.90497   -3.18  0.00357 ** 
hp          -0.03748    0.00961   -3.90  0.00055 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 2.54 on 28 degrees of freedom
Multiple R-squared:  0.84,	Adjusted R-squared:  0.823 
F-statistic:   49 on 3 and 28 DF,  p-value: 2.91e-11
```

__Interpretation of coefficients__    
Intercept : Not usefully interpretable estimated mpg for automatic  vehicle zero weight and hp    
am1 : The expected difference in mpg (increase here) if a car of certain weight and horse power had manual transmission rather than automatic.     
wt :The expected change in mpg per 1000lb increase in weight (all other variables fixed)       
hp :The expected change in mpg per unit increase in horse power (all other variables fixed)   
associated p=Pr(>|t|) : The probability that the coefficient is not null. i.e. no correlation effect observed.        
R^2 : =0.84 The fraction of variability of mpg that is explained by this model.      

We conclude at 95% significance that both weight and horse power decrease mpg.   

__Focussing on automatic vs manual transmission__  
The model indicates that manual transmission increases mpg. However testing at 95% significance.   
H0: transmission has no effect am1=0 (when we adjust for weight and horsepower)      
H1: transmission has an effect on mpg mod(am1)>0      
p>0.05 so we cannot reject the null hypthesis and conclude there is not sufficient evidence (at95%) in this dataset to support the idea that transmission has an effect on mpg.   

Attempting to quantify this effect further is therefore not very useful.

__Diagnostics__

To investigate the validity of this model in more detail, a simple residual analysis was conducted.

Fig5 shows the residual plots of weight, horse power and transmission when used as indivdual regressors, together with the residual plot of the combined model.






Fig6 is a table of influence measures for each observation.   
rstandard is the standardised residual of the observation   
hatvalues measures leverage      
dfbetas (dfb) gives the expected change in the respective model coefficients if that data point was excluded.   


Several vehicles appear to have more than average influence on our model.
I will not omit these vehicles from the analysis as there is no reason to believe that these observations are spurious and we have no information as to whether these observations form part of a representative sample of vehicles. Indeed we have no information as to the representativeness of the sample in general.

However it may be worth investigating the robustness of our model against outliers by running the same analysis excluding these outlying cases.


Fig7 : Running the  regression model excluding (rather arbitrarily) the observations with the 3 highest hat values then secondly the observations with the 3 highest standardised residuals (mod).




With these models weight and horse power remain significant with little change in the coefficients. The manual transmission coefficient (am1) is significantly changed when the highest residuals are omitted (indicating sensitivity of the results to outliers) and, as when using the complete data set, any change due to transmission is not significant. 



### Discussion and Conclusion
While the data set does not allow us to conclude a relationship between automatic/manual transmission and mpg, the effect of both vehicle weight and horsepower is significant. 
The data set was not particularly well adapted to answer our key question. Firstly the number of observations is small and secondly I see no evidence that this data set was compiled in order to answer our question. We have significantly more automatic cars in our data and the data set does not allow comaprison between the same (or similar) model of car with manual transmission and without manual transmission. In addition we do not know how representative a sample this is.
 

#### References

 R Core Team (2014). R: A language and environment for statistical computing. R Foundation for Statistical
  Computing, Vienna, Austria. URL http://www.R-project.org/.
  
________________________
  
Appendix of figures (tables and plots)
-------
VARIABLE KEY mpg -Miles/(US) gallon	 cyl-Number of cylinders	 disp-Displacement (cu.in.)	 hp-Gross horsepower	 drat-Rear axle ratio	 wt-Weight (lb/1000)	 qsec-1/4 mile time	 vs-V/S	 am-Transmission (0 = automatic, 1 = manual)	 gear-Number of forward gears	 carb-Number of carburetors

       
            
fig1 : PLOTS OF mpg AGAINST THE 10 ASPECTS




![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1.png) 

.                   
fig2 - EXPLORATORY REGRESSION (mpg ~ am + wt + hp + cyl + disp)


```

Call:
lm(formula = mpg ~ am + wt + hp + cyl + disp, data = mtcars)

Residuals:
   Min     1Q Median     3Q    Max 
 -3.94  -1.33  -0.39   1.19   5.08 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 33.86428    2.69542   12.56  2.7e-12 ***
am1          1.80610    1.42108    1.27    0.215    
wt          -2.73869    1.17598   -2.33    0.028 *  
hp          -0.03248    0.01398   -2.32    0.029 *  
cyl6        -3.13607    1.46909   -2.13    0.043 *  
cyl8        -2.71778    2.89815   -0.94    0.357    
disp         0.00409    0.01277    0.32    0.751    
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 2.45 on 25 degrees of freedom
Multiple R-squared:  0.866,	Adjusted R-squared:  0.834 
F-statistic:   27 on 6 and 25 DF,  p-value: 8.86e-10
```
.  
fig3 - ANOVA TABLES FOR 2 SERIES OF NESTED MODELS


```
[1] table1
```

```
Analysis of Variance Table

Model 1: mpg ~ wt
Model 2: mpg ~ wt + hp
Model 3: mpg ~ wt + hp + am
Model 4: mpg ~ wt + hp + am + cyl
Model 5: mpg ~ wt + hp + am + cyl + disp
  Res.Df RSS Df Sum of Sq     F Pr(>F)   
1     30 278                             
2     29 195  1      83.3 13.84  0.001 **
3     28 180  1      14.8  2.45  0.130   
4     26 151  2      29.3  2.43  0.108   
5     25 150  1       0.6  0.10  0.751   
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```
[1] table2
```

```
Analysis of Variance Table

Model 1: mpg ~ am
Model 2: mpg ~ am + wt
Model 3: mpg ~ am + wt + hp
  Res.Df RSS Df Sum of Sq    F  Pr(>F)    
1     30 721                              
2     29 278  1       443 68.7 5.1e-09 ***
3     28 180  1        98 15.2 0.00055 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


.  
fig5 - RESIDUAL ANALYSIS 

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 

.   
fig6 - TABLE OF INFLUENCE MEASURES

```
                    rstandard hatvalues dfb intercept    dfb wt      dfb hp    dfb am
Maserati Bora         0.89711   0.41220     -0.129606 -0.1617994  0.5809822  0.156374
Lincoln Continental   0.03163   0.27258     -0.014699  0.0162068 -0.0075160  0.009183
Cadillac Fleetwood   -0.36370   0.23496      0.147792 -0.1676264  0.0821777 -0.090778
Chrysler Imperial     2.11268   0.23032     -0.929900  0.9726881 -0.3350091  0.540021
Ford Pantera L       -0.56642   0.22279      0.045546  0.0590818 -0.2090926 -0.086002
Duster 360           -0.10703   0.19244     -0.025312  0.0345914 -0.0433384  0.032127
Toyota Corona        -0.76647   0.17016     -0.320510  0.2481860 -0.0721203  0.285783
Lotus Europa          1.24763   0.15871      0.365268 -0.3897100  0.2364049 -0.097882
Camaro Z28           -0.19895   0.14476     -0.027777  0.0421529 -0.0640516  0.043209
Merc 240D             0.80238   0.12599      0.096360  0.0461991 -0.1938065 -0.086683
Honda Civic           0.38390   0.12507      0.064385 -0.0344926 -0.0341059  0.018557
Mazda RX4 Wag        -1.13127   0.12316      0.202267 -0.2552249  0.2182570 -0.349183
Fiat 128              2.14035   0.11135     -0.018831  0.2100033 -0.4329640  0.432246
Volvo 142E           -1.08644   0.11126      0.159426 -0.2069045  0.1847404 -0.305620
Toyota Corolla        2.30628   0.10653      0.281998 -0.1101555 -0.2400377  0.223195
Fiat X1-9            -0.30931   0.10400     -0.024325  0.0026897  0.0357986 -0.034833
Ferrari Dino         -0.76760   0.09384      0.043471 -0.0002458 -0.0740807 -0.130987
Mazda RX4            -1.41620   0.09321      0.130806 -0.1807027  0.1758178 -0.335273
Datsun 710           -1.28905   0.08856      0.017220 -0.0783625  0.1440618 -0.230234
Porsche 914-2        -0.21268   0.08630     -0.009100  0.0003430  0.0148624 -0.027237
Merc 230              0.58744   0.08596      0.100664 -0.0289072 -0.0560882 -0.097796
Hornet Sportabout     0.47552   0.07867      0.084015 -0.0763870  0.0673946 -0.099876
Valiant              -0.82322   0.07623     -0.068478 -0.0310092  0.1123906  0.081141
Hornet 4 Drive        0.31735   0.07518      0.053117 -0.0208474 -0.0168311 -0.054036
Merc 280             -0.11835   0.06310     -0.013049  0.0022167  0.0070696  0.015281
Merc 280C            -0.68835   0.06310     -0.076524  0.0130000  0.0414595  0.089615
AMC Javelin          -1.33988   0.06184     -0.200961  0.1289822 -0.0626120  0.236798
Merc 450SL            0.31718   0.05986      0.027940 -0.0213105  0.0271702 -0.043914
Merc 450SE            0.34892   0.05850     -0.007142  0.0194759 -0.0001892 -0.016994
Merc 450SLC          -0.47733   0.05781     -0.034469  0.0234630 -0.0348796  0.059860
Dodge Challenger     -1.11551   0.05719     -0.134295  0.0710246 -0.0269461  0.169105
Pontiac Firebird      1.14445   0.05436      0.052721 -0.0141413  0.0435194 -0.117330
```

.   
fig7 - REGRESSION MODEL WITH REDUCED DATA SET (table1 excluding top 3 hat values, table2 excluding top 3 standardised residuals)

```

Call:
lm(formula = mpg ~ am + wt + hp, data = mtcars[-c(15, 16, 31), 
    ])

Residuals:
   Min     1Q Median     3Q    Max 
 -3.38  -1.89  -0.40   1.14   5.55 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  33.8572     3.5076    9.65  6.5e-10 ***
am1           2.0257     1.6123    1.26   0.2206    
wt           -2.5450     1.2504   -2.04   0.0526 .  
hp           -0.0440     0.0125   -3.51   0.0017 ** 
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 2.64 on 25 degrees of freedom
Multiple R-squared:  0.805,	Adjusted R-squared:  0.781 
F-statistic: 34.3 on 3 and 25 DF,  p-value: 5.12e-09
```

```

Call:
lm(formula = mpg ~ am + wt + hp, data = mtcars[-c(17, 18, 20), 
    ])

Residuals:
   Min     1Q Median     3Q    Max 
-3.186 -1.113  0.076  1.236  3.263 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 35.47152    2.07199   17.12  2.6e-15 ***
am1          0.50558    1.03967    0.49  0.63100    
wt          -3.75030    0.71234   -5.26  1.9e-05 ***
hp          -0.02802    0.00715   -3.92  0.00061 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 1.83 on 25 degrees of freedom
Multiple R-squared:  0.887,	Adjusted R-squared:  0.874 
F-statistic: 65.7 on 3 and 25 DF,  p-value: 5.44e-12
```





