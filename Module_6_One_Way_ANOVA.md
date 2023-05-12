# Module 6: One-Way Analysis of Variance (ANOVA)
## University of North Carolina Wilmington
## Hawken Hass

# One Way Anova
In this module we are going to talk about how to run ANOVA tests in R. ANOVAs are similar to t-tests in that they both assess mean differences across groups. ANOVAs are useful because they assess mean differences when you have more than two groups, or more than two groups. First, we are going to discuss an one-way ANOVA, which is assessing differences of one independent variable with more than two levels (or more than two groups). Let's first read in some data. The data we are going read in involves the circumference of different types of orange trees. Therefore, the independent variable is the type of orange tree (with three different types) and the dependent variable is the circumference of the tree. 


```R
orange<-read.table("orange.txt")
orange
```


<table>
<thead><tr><th scope=col>Tree</th><th scope=col>age</th><th scope=col>circumference</th></tr></thead>
<tbody>
	<tr><td>1   </td><td> 118</td><td> 30 </td></tr>
	<tr><td>1   </td><td> 484</td><td> 58 </td></tr>
	<tr><td>1   </td><td> 664</td><td> 87 </td></tr>
	<tr><td>1   </td><td>1004</td><td>115 </td></tr>
	<tr><td>1   </td><td>1231</td><td>120 </td></tr>
	<tr><td>1   </td><td>1372</td><td>142 </td></tr>
	<tr><td>1   </td><td>1582</td><td>145 </td></tr>
	<tr><td>2   </td><td> 118</td><td> 33 </td></tr>
	<tr><td>2   </td><td> 484</td><td> 69 </td></tr>
	<tr><td>2   </td><td> 664</td><td>111 </td></tr>
	<tr><td>2   </td><td>1004</td><td>156 </td></tr>
	<tr><td>2   </td><td>1231</td><td>172 </td></tr>
	<tr><td>2   </td><td>1372</td><td>203 </td></tr>
	<tr><td>2   </td><td>1582</td><td>203 </td></tr>
	<tr><td>3   </td><td> 118</td><td> 30 </td></tr>
	<tr><td>3   </td><td> 484</td><td>400 </td></tr>
	<tr><td>3   </td><td> 664</td><td>300 </td></tr>
	<tr><td>3   </td><td>1004</td><td>500 </td></tr>
	<tr><td>3   </td><td>1231</td><td>600 </td></tr>
	<tr><td>3   </td><td>1372</td><td>200 </td></tr>
	<tr><td>3   </td><td>1582</td><td>515 </td></tr>
</tbody>
</table>




```R
orange$Tree<-as.factor(orange$Tree)
```


```R
library(ggplot2)
library(car)
```

    Registered S3 methods overwritten by 'ggplot2':
      method         from 
      [.quosures     rlang
      c.quosures     rlang
      print.quosures rlang
    Loading required package: carData
    

## Assumptions
Just like any test, we are first going to need to test our assumptions. The assumptions of the one-way ANOVA are the same as the assumptions for the independent t-test. First let's test for normality!

### Normality


```R
library(tidyr)
orange_wide<-spread(orange,key="Tree",value="circumference")
varnames<-c("age","treeone","treetwo","treethree")
names(orange_wide)<-varnames
orange_wide
```


<table>
<thead><tr><th scope=col>age</th><th scope=col>treeone</th><th scope=col>treetwo</th><th scope=col>treethree</th></tr></thead>
<tbody>
	<tr><td> 118</td><td> 30 </td><td> 33 </td><td> 30 </td></tr>
	<tr><td> 484</td><td> 58 </td><td> 69 </td><td>400 </td></tr>
	<tr><td> 664</td><td> 87 </td><td>111 </td><td>300 </td></tr>
	<tr><td>1004</td><td>115 </td><td>156 </td><td>500 </td></tr>
	<tr><td>1231</td><td>120 </td><td>172 </td><td>600 </td></tr>
	<tr><td>1372</td><td>142 </td><td>203 </td><td>200 </td></tr>
	<tr><td>1582</td><td>145 </td><td>203 </td><td>515 </td></tr>
</tbody>
</table>




```R
shapiro.test(orange_wide$treeone)
shapiro.test(orange_wide$treetwo)
shapiro.test(orange_wide$treethree)
```


    
    	Shapiro-Wilk normality test
    
    data:  orange_wide$treeone
    W = 0.92095, p-value = 0.4768
    



    
    	Shapiro-Wilk normality test
    
    data:  orange_wide$treetwo
    W = 0.91193, p-value = 0.4094
    



    
    	Shapiro-Wilk normality test
    
    data:  orange_wide$treethree
    W = 0.95208, p-value = 0.7486
    


It looks like all three of our groups follow a normal distribution. The good news is that ANOVAs are generally robust to violations of normality. Unless your data is extremely skewed, you should still be able to proceed with the analysis even if there is a slight violation. Next, let's test for homogeneity of variance.

### HOV


```R
leveneTest(orange$circumference,orange$Tree,center=mean,data=orange)
```


<table>
<thead><tr><th></th><th scope=col>Df</th><th scope=col>F value</th><th scope=col>Pr(&gt;F)</th></tr></thead>
<tbody>
	<tr><th scope=row>group</th><td> 2         </td><td>8.247068   </td><td>0.002869096</td></tr>
	<tr><th scope=row> </th><td>18         </td><td>      NA   </td><td>         NA</td></tr>
</tbody>
</table>



Our data does not meet the homogeneity of variance assumption that all groups must have a similar variance. Now we can proceed with the analysis but will have to make corrections to account for this violation.

## One Way ANOVA
There are different functions to use depending on if your data meets the HOV assumption or not. The first function, we are going to assume HOV was met.


```R
orange_w_HOV<-aov(circumference~Tree,orange)
summary.aov(orange_w_HOV)
```


                Df Sum Sq Mean Sq F value Pr(>F)   
    Tree         2 287200  143600   9.282 0.0017 **
    Residuals   18 278475   15471                  
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1



```R
orange_wout_HOV<-oneway.test(circumference~Tree,orange)
orange_wout_HOV
```


    
    	One-way analysis of means (not assuming equal variances)
    
    data:  circumference and Tree
    F = 5.7592, num df = 2.000, denom df = 10.351, p-value = 0.02083
    


Both tests show that there is a significant difference in average circumference across tree type. In a typical analysis if the p-value was not significant, we would not further explore these differences since there are none. However, since the p-value was significant, the test only tells us there is a significant mean difference in at least one of the groups, the test does not tell us which groups are different from eachother. Thus further testing is necessary given the p-value is significant. For the purposes of the excercise, I will demonstrate how to determine where the differences lay between groups.

## Planned Contrasts
The first type of post-hoc testing is planned contrast testing. You run planned contrasts when you already know what groups you want to test **before** seeing the results. Let's say we hypothesize that the circumference of tree type 1 is going to be different than tree type 3. This is a planned comparsion because you already know what comparisons you are going to test before seeing the data and the results. 

[Follow this link for more information on when to use planned comparisons after a one-way ANOVA](https://www.graphpad.com/support/faqid/1092/)

### Simple Contrasts ###
Because we hypothesized that tree 1 and tree 3 were going to be different, we are only going to compare those two to see if they are statistically significant. Simple contrasts only compare two groups at a time. The benefit of doing planned contrasts is that by not comparing every group to eachother, each comparison has more statistical power. The more comparisons we run on the same data, the higher the likelihood of a rejecting the null hypothesis when there actually is no difference. Post-hoc tests can control for testing multiple comparisons which will be discussed in greater detail below. But planned contrasts are another way to avoid potential issues with testing multiple comparisons.


```R
library(PMCMRplus)
contrast<-c(1,0,-1)
contrasts(orange$Tree)<-contrast
aov_simple<-aov(circumference~Tree,orange)
summary.aov(aov_simple,split=list(Tree=list("Tree 1 vs Tree 3"=1)))
```

    Warning message:
    "package 'PMCMRplus' was built under R version 3.6.3"


                             Df Sum Sq Mean Sq F value   Pr(>F)    
    Tree                      2 287200  143600   9.282 0.001698 ** 
      Tree: Tree 1 vs Tree 3  1 243936  243936  15.767 0.000896 ***
    Residuals                18 278475   15471                     
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


If we look at the row labeled: "Tree: Tree 1 vs Tree 3", we can see that the difference between Tree 1 and Tree 3 is significantly different. We know this by looking at the last "P" column which shows that our p=value is less than .05. Now let's try a complex contrast!

### Complex Contrast
An example of a complex contrast would be comparing the average circumference of the first two tree types to the average of the third tree type.


```R
contrast2<-c(0.5,0.5,-1)
contrasts(orange$Tree)<-contrast2
aov_complex<-aov(circumference~Tree, orange)
summary.aov(aov_complex,split=list(Tree=list("Tree 1 vs Tree 2 vs Tree 3"=1)))
```


                                       Df Sum Sq Mean Sq F value   Pr(>F)    
    Tree                                2 287200  143600   9.282 0.001698 ** 
      Tree: Tree 1 vs Tree 2 vs Tree 3  1 282736  282736  18.275 0.000456 ***
    Residuals                          18 278475   15471                     
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


By looking at the second row of the above output the p-value indicates that the average circumference between Tree 1 and Tree 2 is significantly different from the average circumference of the third tree type.

## Post Hoc Comparisons
If you do not have any planned comparisons, you have the option of running group comparisons after running your Anova. These comparisons involve comparing each group to eachother. Because statistical power is lost when running multiple statistical tests (in this case multiple pairwise t-tests) some corrections are necessary. The specific post-hoc comparison you choose depends on the characteristics of your dataset. There are multiple types of post hoc comparisons:

- Fisher's
- Tukey's
- Dunnett
- Scheffe

### Fisher's LSD Test

You should use the fisher's test if you have three conditions (i.e., groups).


```R
fisher_test<-lsdTest(circumference~Tree, orange)
summary(fisher_test)
```

    
    	Pairwise comparisons using Least Significant Difference Test
    
    data: circumference by Tree
    alternative hypothesis: two.sided
    P value adjustment method: none
    H0
    

               t value   Pr(>|t|)    
    2 - 1 == 0   0.537 0.59772230    
    3 - 1 == 0   3.971 0.00089621 ***
    3 - 2 == 0   3.434 0.00296221  **
    

    ---
    Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    

### Tukey's HSD Test
You should used Tukey's test if have more than three condtiions and the assumption of HOV is met. While we do not have greater than three conditions, this is what the code would look like.


```R
tukeytest<-tukeyTest(circumference~Tree, orange)
summary(tukeytest)
```

    
    	Pairwise comparisons using Tukey's test
    
    data: circumference by Tree
    alternative hypothesis: two.sided
    P value adjustment method: single-step
    H0
    

               q value  Pr(>|q|)   
    2 - 1 == 0   0.760 0.8541923   
    3 - 1 == 0   5.616 0.0024548 **
    3 - 2 == 0   4.856 0.0079238 **
    

    ---
    Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    

### Dunnett's Test
You should use Dunnett's test if you have greater than three conditions but violated the assumption of HOV.


```R
dunnett<-dunnettT3Test(circumference~Tree,orange)
summary(dunnett)
```

    
    	Pairwise comparisons using Dunnett's T3 test for multiple comparisons
    		with unequal variances
    
    data: circumference by Tree
    alternative hypothesis: two.sided
    P value adjustment method: single-step
    H0
    

               t value Pr(>|t|)  
    2 - 1 == 0   1.193 0.570976  
    3 - 1 == 0   3.408 0.030622 *
    3 - 2 == 0   2.862 0.065311 .
    

    ---
    Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    

### Scheffe's Test
Use Scheffe's test when you have three or more groups and have an unequal sample size across groups.


```R
scheffe<-scheffeTest(circumference~Tree,orange)
summary(scheffe)
```

    
    	Pairwise comparisons using Scheffe's test
    
    data: circumference by Tree
    alternative hypothesis: two.sided
    P value adjustment method: none
    H0
    

               F value  Pr(>|F|)   
    2 - 1 == 0   0.144 0.8666358   
    3 - 1 == 0   7.884 0.0034751 **
    3 - 2 == 0   5.895 0.0107356  *
    

    ---
    Signif. codes: 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    

Since the Fisher's LSD test is most applicable to our dataset, I will interpret those results. For each output for each test however, the rows represents different comparisons for the different groups. The number correspond to whatever value was assigned to that group when factoring the variable. Thus in Fisher's test, the results show that there was a significant difference in average tree circumference between Tree Type 1 and Tree Type 3, as well as Tree Type 2 and Tree Type 3, however not between Tree Type 1 and Tree Type 2. You'll notice that for some of these outputs the results are slightly different. This difference is due to the nature of each test. For example, because Dunnett's test is accounting for violation of the HOV assumption thus making the test more conservative. This is why the difference between Tree Type 2 and Tree Type 3 is no longer significant in the Dunnett test.

## Effect Size

There are multiple measures of effect size for both planned simple and complex comparisons. 

### Cohen's D
To calculate cohen's D you will first need to convert the F value of the contrast into a T value. This is done by square root of the F value. Let's rerun our simple contrast.


```R
summary.aov(aov_simple,split=list(Tree=list("Tree 1 vs Tree 3"=1)))
```


                             Df Sum Sq Mean Sq F value   Pr(>F)    
    Tree                      2 287200  143600   9.282 0.001698 ** 
      Tree: Tree 1 vs Tree 3  1 243936  243936  15.767 0.000896 ***
    Residuals                18 278475   15471                     
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


Looking at the third column and second row of the output, our F value is 15.77. In order to calculate Cohen's D, we need to take the square root of this value to get a T value.


```R
sqrt(15.77)
```


3.97114593033296


Now we can calculate our effect size using the "esc" package.


```R
library(esc)
cohend<-esc_t(t=3.97, grp1n = 7, grp2n = 7)
cohend
```


    
    Effect Size Calculation for Meta Analysis
    
         Conversion: t-value to effect size d
        Effect Size:   2.1221
     Standard Error:   0.6682
           Variance:   0.4465
           Lower CI:   0.8123
           Upper CI:   3.4318
             Weight:   2.2394


The interpretation of Cohen's D effect size is as follows:

- .2 = small effect
- .5 = medium effect
- .8 = large effect

Our effect size for the difference between Tree 1 and Tree 3 is 2.12. This is a very large effect size! Thus, the difference in circumference between these two tree types is quite large.


### Omega-Squared

Another measure of effect size  is omega-squared. Omega squared represents the amount of variance in Y that is due to our independent variable (X variable). We will need a few numbers to calculate omega-squared. 


```R
summary.aov(aov_simple,split=list(Tree=list("Tree 1 vs Tree 2 vs Tree 3"=1)))
```


                                       Df Sum Sq Mean Sq F value   Pr(>F)    
    Tree                                2 287200  143600   9.282 0.001698 ** 
      Tree: Tree 1 vs Tree 2 vs Tree 3  1 243936  243936  15.767 0.000896 ***
    Residuals                          18 278475   15471                     
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


Omega-squared is a proportion, so first we will need to calculated the numerator of the fraction. The numerator formula is:

SScontrast-(dfcontrast*MSerror)

In our output:
- The contrast sum of squares is: 243936
- The contrast degrees of freedom is 1
- The mean squared error is 15471

Let's calculate!


```R
numerator<-(243936-(1*15471))
numerator
```


228465


The denominator formula is:

SScontrast + SS error + MS error


```R
denominator<-(243936+278475)+15471
denominator
omega<-numerator/denominator
omega
```


537882



0.424749294454918


Our omega squared value is 0.42. This means that 42% of the variance in tree circumference can be explained by the type of the tree. In coincidence with Cohen's D this is a very large effect size. Omega squared and Cohen's D both measure effect size but have different interpretations. The unit for Cohen's D is standard deviations. Thus, Cohen's D represents a standardized difference of the two groups. Omega-squared is a proportion that represents the amount of variance explained by the independent variable.
