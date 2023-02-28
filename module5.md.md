# Module 5: T-Tests
## Hypothesis Testing Using a T-distribution
## Hawken Hass
## University of North Carolina Wilmington

# One Sample T-Test
T-tests are appropriate for identifying a significant mean difference between two groups. A one sample t-test is used when you want to compare a sample mean to a population mean. This test will tell you if the sample mean is signficantly different from the population mean. First, let's read in some data and activate the necessary packages.


```R
lake_data<-read.table("lake_data.txt")
lake_data
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>SeaHawk</th><th scope=col>Randall</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td> 3.125990</td><td>10.915324</td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>12.441331</td><td> 5.148206</td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td> 3.230104</td><td> 5.982370</td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>10.907835</td><td> 6.179094</td></tr>
	<tr><td>May      </td><td>2001-2019</td><td> 9.674827</td><td> 8.532295</td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td> 8.033946</td><td> 6.132740</td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>12.318558</td><td>14.437468</td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td> 6.557286</td><td>11.269315</td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td> 6.057994</td><td>15.059383</td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>12.040164</td><td>10.124065</td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td> 5.369271</td><td> 2.171475</td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td> 8.132666</td><td>12.947003</td></tr>
</tbody>
</table>




```R
library(dplyr)
library(lsr)
library(car)
```

    
    Attaching package: 'dplyr'
    
    The following objects are masked from 'package:stats':
    
        filter, lag
    
    The following objects are masked from 'package:base':
    
        intersect, setdiff, setequal, union
    
    Loading required package: carData
    
    Attaching package: 'car'
    
    The following object is masked from 'package:dplyr':
    
        recode
    
    

## Assumptions
Before running the analyses we first need to check our assumptions. The assumptions are as follows:
1. One continuous dependent variable (interval or ratio)
2. One discrete independent variable (categorical, ordinal, or dichotomous with no more than two levels)
3. Each data point is independent of one another
4. Data is normally distributed (bell-curve shaped)
5. No outliers

Assumptions 1-3 are typically accounted for when designing the study prior to analysis. Assumptions 4-5 can be checked using graphs and statistical tests.

### Normality


Normality can be visually assessed with various types of graphs. In this example the dependent variable is the average monthly rainfall at Randall Lake. There are a few graphs that are appropriate for assessing normality: a histogram, a density plot, or a QQ-plot. Let's activate the ggplot2 package so we can visualize our data.


```R
library(ggplot2)
```

    Registered S3 methods overwritten by 'ggplot2':
      method         from 
      [.quosures     rlang
      c.quosures     rlang
      print.quosures rlang
    


```R
ggplot(lake_data, aes(x=Randall))+geom_histogram(binwidth=2, colour="black",aes(y=..density..))+
stat_function(fun=dnorm,color="red",args=list(mean=mean(lake_data$Randall),  sd=sd(lake_data$Randall)))+ scale_x_continuous(limits = c(-5, 25))

ggplot(lake_data, aes(x=SeaHawk))+geom_histogram(binwidth=2, colour="black",aes(y=..density..))+
stat_function(fun=dnorm,color="red",args=list(mean=8.15,sd=sd(lake_data$SeaHawk)))+ scale_x_continuous(limits = c(-5, 25))

```


    
![png](output_7_0.png)
    



    
![png](output_7_1.png)
    


The shape of the histogram will tell us if the data is normally distributed. If the data is shaped like a bell-curve, we can assume the data is normally distributed.  With the stat_function() function I added a reference for what a normally distribution curve would look like (red line).Sometimes it is difficult to tell the shape with a histogram, and a density plot may be more appropriate.


```R
ggplot(data=lake_data,aes(x=Randall))+geom_density()+
stat_function(fun=dnorm,color="red",args=list(mean=mean(lake_data$Randall),  sd=sd(lake_data$Randall))) + scale_x_continuous(limits = c(0, 20))

ggplot(data=lake_data,aes(x=SeaHawk))+geom_density()+stat_function(fun=dnorm,color="red",args=list(mean=mean(lake_data$SeaHawk),  sd=sd(lake_data$SeaHawk))) + scale_x_continuous(limits = c(-5, 25))
```


    
![png](output_9_0.png)
    



    
![png](output_9_1.png)
    


The data seems to follow closely to a normal distribution. Another method of evaluating normality is through a QQ plot.


```R
ggplot(data=lake_data,aes(sample=Randall))+stat_qq()+stat_qq_line()
ggplot(data=lake_data, aes(sample=SeaHawk))+stat_qq()+stat_qq_line()
```


    
![png](output_11_0.png)
    



    
![png](output_11_1.png)
    


The closer the points are to the black diagonal line, the more normally distributed the data.

Another way to test for normality is using the Shapiro-Wilks test. This test will give you a p-value that denotes whether the data is normally distributed or not. This test is helpful if you are having trouble interpreting the visualizations of normality. You will need to run the test on each group.


```R
shapiro.test(lake_data$Randall)
shapiro.test(lake_data$SeaHawk)
```


    
    	Shapiro-Wilk normality test
    
    data:  lake_data$Randall
    W = 0.95665, p-value = 0.7352
    



    
    	Shapiro-Wilk normality test
    
    data:  lake_data$SeaHawk
    W = 0.92449, p-value = 0.3254
    


For this test a significant p-value (<.05) indicates that the distribution is significantly different from a normal distribution. Therefore, a significant p-value indicates that the normality assumption was violated. For both of our groups, the p-value is not less than .05, therefore the data meets the assumption of normality. Now we can proceed with the analysis!

## One Sample T-Test
The one sample T-test is appropriate when you know the population mean of the distribution and are examining whether one sample mean is significantly different from the population mean. For example let's say that the population mean (i.e., the mean monthly rainfall for all lakes in North America) is 64mm. We are comparing the average monthly rainfall in lakes Randall and SeaHawk to this population mean of 64mm to see if there is a statistically significant difference.


```R
onesample_randall<-t.test(lake_data$Randall,alternative=("two.sided"), mu=64)
onesample_randall
onesample_seahawk<-t.test(lake_data$SeaHawk,alternative=("two.sided"),mu=64)
onesample_seahawk
```


    
    	One Sample t-test
    
    data:  lake_data$Randall
    t = -47.187, df = 11, p-value = 4.744e-14
    alternative hypothesis: true mean is not equal to 64
    95 percent confidence interval:
      6.512964 11.636826
    sample estimates:
    mean of x 
     9.074895 
    



    
    	One Sample t-test
    
    data:  lake_data$SeaHawk
    t = -57.394, df = 11, p-value = 5.549e-15
    alternative hypothesis: true mean is not equal to 64
    95 percent confidence interval:
      6.015998 10.298997
    sample estimates:
    mean of x 
     8.157498 
    


Above shows the output of our t-tests. You can see in our code we input the vector in which we are getting our sample mean from, whether we want a directional or non-directional hypothesis, and the population mean. The primary value you are looking for is the p-value. For both of our lakes the p-value is very small, considerably lower than .05. This suggests that for both lakes there is a statistically significant difference between the sample mean and the population mean. If you want to know the direction of this difference, all you have to look at is the sample mean. For example, in Lake Randall the mean monthly rainfall is 9.08. 9.08 is less than 64, therefore we can say that the average monthly rainfall for Lake Randall was significantly lower than the population mean.

## Effect Size
The effect size for a one sample t-test is called Cohen's D. This value will tell you the magnitude of your effect size (whether it is a large effect or a small effect). Below is the interpretation for each effect size value according to Cohen (1992)
![download%20%2811%29.png](attachment:download%20%2811%29.png)


```R
CohensD_singleT_randall<-cohensD(x=lake_data$Randall,mu=64)
CohensD_singleT_randall
CohensD_singleT_seahawk<-cohensD(x=lake_data$SeaHawk,mu=64)
CohensD_singleT_seahawk
```


13.6216577590701



16.5681240164619


According to the Cohen's d effect size interpretation, the effects for each lake are very large! This suggests there is a very large difference in the population mean and the sample means for the two lakes.

# Independent Sample T-Test
Let's say that instead of comparing the sample means of the two lakes to a population mean you want to compare the mean monthly  rainfall in Lake Randall, to the mean monthly rainfall in Lake SeaHawk. Now, we are comparing two independent groups to eachother and thus we should run an Independent Sample T-Test.

## Assumptions
Running an Independent T-test comes with additional assumptions to be tested. The assumption of normality still stands so just like above you should assess normality using histograms, QQ-plots, and/or the Shapiro-Wilks test. Now we are introducing two new assumptions: Homogeneity of Variance (HOV) and Independence of Observations.
### Independence
The assumption of independent groups is not something that we can formally test for, but is something that is taken into consideration when designing your research experiment. You want to make sure that your two groups are completely independent of eachother. In this case the measurements of rainfall are taken from two different lakes, thus our design meets the assumption of independence. A violation of independence would be if measures of rainfall were taken from the same lake at different time periods. Or perhaps if the two lakes were in the same geographical location. Other violations of normality would be if data were taken from the sampe participants at different time points, such as in a pre/post design, of if the participants were in some way related to eachother. For example, if a researcher wanted to examine differences in anxiety scores across males and females, a violation of independence would be if participants in the two groups came from the same family. Research design is a critical component to consider when choosing statistical tests.
### Homogeneity of Variance
Another important assumption of an independent t-test is that the variances between the two groups are equal. Variance is a measure of dispersion of your data. If one group has very high variance (participants have a wider range of different scores) and another group has very low variance (all scores are close to the mean; or a small range of different scores), then this will skew the interpretation of the mean comparison. We can test for homoheneity of variance using a Levene's Test.

First, I am going to convert the data to long format, which is necessary for running the Levene's test and the Independent T-Test. You will also need to activate a package called car.


```R
library(tidyr)
long_lake<-gather(data=lake_data,"Lake","Rainfall_mm",3:4)
long_lake
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Lake</th><th scope=col>Rainfall_mm</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td>SeaHawk  </td><td> 3.125990</td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>SeaHawk  </td><td>12.441331</td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>SeaHawk  </td><td> 3.230104</td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>SeaHawk  </td><td>10.907835</td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>SeaHawk  </td><td> 9.674827</td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>SeaHawk  </td><td> 8.033946</td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>SeaHawk  </td><td>12.318558</td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>SeaHawk  </td><td> 6.557286</td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>SeaHawk  </td><td> 6.057994</td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>SeaHawk  </td><td>12.040164</td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>SeaHawk  </td><td> 5.369271</td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>SeaHawk  </td><td> 8.132666</td></tr>
	<tr><td>Jan      </td><td>2001-2019</td><td>Randall  </td><td>10.915324</td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>Randall  </td><td> 5.148206</td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>Randall  </td><td> 5.982370</td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>Randall  </td><td> 6.179094</td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>Randall  </td><td> 8.532295</td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>Randall  </td><td> 6.132740</td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>Randall  </td><td>14.437468</td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>Randall  </td><td>11.269315</td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>Randall  </td><td>15.059383</td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>Randall  </td><td>10.124065</td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>Randall  </td><td> 2.171475</td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>Randall  </td><td>12.947003</td></tr>
</tbody>
</table>



Let's check for homogeneity of variance with the Levene's Test.


```R
HOV<-leveneTest(Rainfall_mm~Lake, center=mean, data=long_lake)
HOV

```

    Warning message in leveneTest.default(y = y, group = group, ...):
    "group coerced to factor."


<table>
<thead><tr><th></th><th scope=col>Df</th><th scope=col>F value</th><th scope=col>Pr(&gt;F)</th></tr></thead>
<tbody>
	<tr><th scope=row>group</th><td> 1       </td><td>0.6757906</td><td>0.4198607</td></tr>
	<tr><th scope=row> </th><td>22       </td><td>       NA</td><td>       NA</td></tr>
</tbody>
</table>



A significant p-value on a Levene's test indicates that the variances of each group are significantly different from eachother. Thus, we do **not** want our p-value to be less than .05. If you look at the Pr(>F) column our p-value is greater than .05. Therefore we meet the assumption of homogeneity of variance and can proceed with our analysis. However, I will also discuss a correction that can be used if the homogeneity of variance assumption is violated.

## Independent T-Test
Let's run our t-test! Use the code below.

lake_w_HOV<-t.test(Rainfall_mm~Lake, var.equal=TRUE, data=long_lake)
lake_w_HOV

It looks like our p-value is 0.55. This indicates that there is no significant difference in the mean monthly rainfall for Lake Randall and Lake SeaHawk. Thus, we fail to reject our null hypothesis. The code below is for the Welch's t-test which corrects for unequal variance. You would use this test instead if your data violated the homogeneity of variance assumption.


```R
lake_no_HOV<-t.test(Rainfall_mm~Lake, data=long_lake)
lake_no_HOV
```


    
    	Welch Two Sample t-test
    
    data:  Rainfall_mm by Lake
    t = 0.60471, df = 21.329, p-value = 0.5518
    alternative hypothesis: true difference in means is not equal to 0
    95 percent confidence interval:
     -2.234599  4.069393
    sample estimates:
    mean in group Randall mean in group SeaHawk 
                 9.074895              8.157498 
    


Unsurprisingly, the test results are the same because we did not violate the HOV assumption. However, it is important to use this correct when necessary.

### Effect Size
We are once again going to use Cohen's D for the measure of effect size even though the result was not significant. For this code I am going to use our original data which is in wide format.


```R
independenttCD<-cohensD(lake_data$Randall, lake_data$SeaHawk, method="pooled")
independenttCD
```


0.246871524226037


As suspected, the Cohen's D test suggests a small effect size. 

# Paired Samples T-Test

The final t-test is the paired samples t-test. This is also called a dependent t-test or a related samples t-test because the assumption of independence is no longer needed. The two groups are related to eachother. This test is common in pre/post designs where the same subjects are tested at different time points. For example, a paired t-test would be appropriate if average monthly rainfall data was calculated for one lake in 2020 and measured again in 2021. Thus, the test would allow us to see if there was a significant difference in average monthly rainfall in 2020 compared to 2021. Let's read in some new data!


```R
pre_post<-read.table("pre_post_lake.txt")
pre_post$month<-as.factor(pre_post$month)
pre_post$year<-factor(pre_post$year, levels=c(1:2),labels=c("2020","2021"))
pre_post
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>rainfall_mm</th></tr></thead>
<tbody>
	<tr><td>2020      </td><td>1         </td><td>  5.647385</td></tr>
	<tr><td>2020      </td><td>2         </td><td> 27.186771</td></tr>
	<tr><td>2020      </td><td>3         </td><td> 55.505624</td></tr>
	<tr><td>2020      </td><td>4         </td><td>  1.919519</td></tr>
	<tr><td>2020      </td><td>5         </td><td> 11.236393</td></tr>
	<tr><td>2020      </td><td>6         </td><td> 52.256103</td></tr>
	<tr><td>2020      </td><td>7         </td><td> 59.832627</td></tr>
	<tr><td>2020      </td><td>8         </td><td> 12.586680</td></tr>
	<tr><td>2020      </td><td>9         </td><td> 47.366872</td></tr>
	<tr><td>2020      </td><td>10        </td><td> 24.724103</td></tr>
	<tr><td>2020      </td><td>11        </td><td> 12.905138</td></tr>
	<tr><td>2020      </td><td>12        </td><td> 28.059708</td></tr>
	<tr><td>2021      </td><td>1         </td><td> 93.849514</td></tr>
	<tr><td>2021      </td><td>2         </td><td> 96.505384</td></tr>
	<tr><td>2021      </td><td>3         </td><td>108.568889</td></tr>
	<tr><td>2021      </td><td>4         </td><td>104.754431</td></tr>
	<tr><td>2021      </td><td>5         </td><td> 90.229946</td></tr>
	<tr><td>2021      </td><td>6         </td><td> 94.588361</td></tr>
	<tr><td>2021      </td><td>7         </td><td> 82.428635</td></tr>
	<tr><td>2021      </td><td>8         </td><td>117.216454</td></tr>
	<tr><td>2021      </td><td>9         </td><td> 82.236660</td></tr>
	<tr><td>2021      </td><td>10        </td><td>105.348053</td></tr>
	<tr><td>2021      </td><td>11        </td><td>112.862754</td></tr>
	<tr><td>2021      </td><td>12        </td><td>101.902190</td></tr>
</tbody>
</table>



You additionally want to ensure that both groups are normally distributed using the shapiro-wilks test. I've already checked this assumption and we are all set to continue with the analysis. Before we begin, unlike the independent sample t-test, the data needs to be in wide format. We need a column for each group. Just like you learned in module four, we should use the spread function.


```R
pre_post<-spread(pre_post, key="year", value="rainfall_mm")
names<-c("month","time_one","time_two")
names(pre_post)<-names
pre_post
```


<table>
<thead><tr><th scope=col>month</th><th scope=col>time_one</th><th scope=col>time_two</th></tr></thead>
<tbody>
	<tr><td>1        </td><td> 5.647385</td><td> 93.84951</td></tr>
	<tr><td>2        </td><td>27.186771</td><td> 96.50538</td></tr>
	<tr><td>3        </td><td>55.505624</td><td>108.56889</td></tr>
	<tr><td>4        </td><td> 1.919519</td><td>104.75443</td></tr>
	<tr><td>5        </td><td>11.236393</td><td> 90.22995</td></tr>
	<tr><td>6        </td><td>52.256103</td><td> 94.58836</td></tr>
	<tr><td>7        </td><td>59.832627</td><td> 82.42864</td></tr>
	<tr><td>8        </td><td>12.586680</td><td>117.21645</td></tr>
	<tr><td>9        </td><td>47.366872</td><td> 82.23666</td></tr>
	<tr><td>10       </td><td>24.724103</td><td>105.34805</td></tr>
	<tr><td>11       </td><td>12.905138</td><td>112.86275</td></tr>
	<tr><td>12       </td><td>28.059708</td><td>101.90219</td></tr>
</tbody>
</table>



### Paired T-Test


```R
pairedt<-t.test(pre_post$time_one,pre_post$time_two,paired=TRUE)
pairedt
```


    
    	Paired t-test
    
    data:  pre_post$time_one and pre_post$time_two
    t = -8.9796, df = 11, p-value = 2.143e-06
    alternative hypothesis: true difference in means is not equal to 0
    95 percent confidence interval:
     -88.32652 -53.55087
    sample estimates:
    mean of the differences 
                   -70.9387 
    


Taking a look at our p-value, it is less than .05 suggesting a signficant difference in rainfall between the two time points. We can reject the null hypothesis.

### Effect Size

Just like our other tests, we are going to use Cohen's D as the measure of effect size. 


```R
pairedtcohensd<- cohensD(pre_post$time_two, pre_post$time_one, method = "paired")
pairedtcohensd
```


2.59217510960909

