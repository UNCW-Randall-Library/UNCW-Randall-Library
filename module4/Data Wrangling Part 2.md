# Module 4: Data Wrangling Part 2
## How to Clean and Manipulate your Data in R
## Hawken Hass
## University of North Carolina Wilmington

# Manipulating your Data

Once your data is all clean, you can manipulate your data. Data manipulation can include grouping data, combining data sets, summarizing data, and making new variables. For this, we are going to use the dplyr package.


```R
library(dplyr)

```

    
    Attaching package: 'dplyr'
    
    The following objects are masked from 'package:stats':
    
        filter, lag
    
    The following objects are masked from 'package:base':
    
        intersect, setdiff, setequal, union
    
    

For this module, let's use our data we just cleaned in the last module. I refactored the data to make sure that the levels are in the correct order.


```R
clean_data<-read.table("clean_data.txt")
clean_data$Month<-factor(clean_data$Month, levels=unique(clean_data$Month))
clean_data
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Lake</th><th scope=col>Rainfall_mm</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td>Victoria </td><td>3.18     </td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>Victoria </td><td>3.48     </td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>Victoria </td><td>4.69     </td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>Victoria </td><td>7.00     </td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>Victoria </td><td>9.36     </td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>Victoria </td><td>3.43     </td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>Victoria </td><td>1.76     </td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>Victoria </td><td>2.81     </td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>Victoria </td><td>3.98     </td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>Victoria </td><td>5.32     </td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>Victoria </td><td>5.12     </td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>Victoria </td><td>4.17     </td></tr>
	<tr><td>Jan      </td><td>2001-2019</td><td>Simiyu   </td><td>2.91     </td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>Simiyu   </td><td>1.80     </td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>Simiyu   </td><td>2.98     </td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>Simiyu   </td><td>4.75     </td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>Simiyu   </td><td>4.08     </td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>Simiyu   </td><td>1.05     </td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>Simiyu   </td><td>0.20     </td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>Simiyu   </td><td>0.33     </td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>Simiyu   </td><td>1.21     </td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>Simiyu   </td><td>2.45     </td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>Simiyu   </td><td>3.09     </td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>Simiyu   </td><td>3.89     </td></tr>
</tbody>
</table>



## Filtering Data by Rows
The filter function allows you to subset rows from your data frame. For example, let's say we only want to look at rainfall in the month of January. We can do that using the filter function. 


```R
filter(clean_data, Month=="Jan")
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Lake</th><th scope=col>Rainfall_mm</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td>Victoria </td><td>3.18     </td></tr>
	<tr><td>Jan      </td><td>2001-2019</td><td>Simiyu   </td><td>2.91     </td></tr>
</tbody>
</table>



Since there are only two observations for each month, this is a pretty small data frame. However, this function is quite useful for large datasets. In the second argument, you denote an operate that tells the function how you would like to filter the column. Since we wanted only values for the month of January, we used the "==" operator. There are many different operators you can use to filter your data. For example, the code below would filter out any rainfall measurement that is less than 1.


```R
filter(clean_data, Rainfall_mm>1)
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Lake</th><th scope=col>Rainfall_mm</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td>Victoria </td><td>3.18     </td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>Victoria </td><td>3.48     </td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>Victoria </td><td>4.69     </td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>Victoria </td><td>7.00     </td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>Victoria </td><td>9.36     </td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>Victoria </td><td>3.43     </td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>Victoria </td><td>1.76     </td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>Victoria </td><td>2.81     </td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>Victoria </td><td>3.98     </td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>Victoria </td><td>5.32     </td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>Victoria </td><td>5.12     </td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>Victoria </td><td>4.17     </td></tr>
	<tr><td>Jan      </td><td>2001-2019</td><td>Simiyu   </td><td>2.91     </td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>Simiyu   </td><td>1.80     </td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>Simiyu   </td><td>2.98     </td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>Simiyu   </td><td>4.75     </td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>Simiyu   </td><td>4.08     </td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>Simiyu   </td><td>1.05     </td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>Simiyu   </td><td>1.21     </td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>Simiyu   </td><td>2.45     </td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>Simiyu   </td><td>3.09     </td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>Simiyu   </td><td>3.89     </td></tr>
</tbody>
</table>



Here are some more examples of operators.

- `== (Equal to)`
- `!= (Not equal to)`
- `< (Less than)`
- `<= (Less than or equal to)`
- `> (Greater than)`
- `>= (Greater than or equal to)`

## Filtering by Columns

The select function is used to filter out specific columns. The select function also uses helper functions to filter out columns based on certain properties. Using double quotes, you can filter out columns of a certain name.


```R
select(clean_data, contains("Lake"))
```


<table>
<thead><tr><th scope=col>Lake</th></tr></thead>
<tbody>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Victoria</td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
	<tr><td>Simiyu  </td></tr>
</tbody>
</table>



The "-" operator can be used to denote which columns you want to exclude.


```R
select(clean_data, -Period)
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Lake</th><th scope=col>Rainfall_mm</th></tr></thead>
<tbody>
	<tr><td>Jan     </td><td>Victoria</td><td>3.18    </td></tr>
	<tr><td>Feb     </td><td>Victoria</td><td>3.48    </td></tr>
	<tr><td>Mar     </td><td>Victoria</td><td>4.69    </td></tr>
	<tr><td>Apr     </td><td>Victoria</td><td>7.00    </td></tr>
	<tr><td>May     </td><td>Victoria</td><td>9.36    </td></tr>
	<tr><td>Jun     </td><td>Victoria</td><td>3.43    </td></tr>
	<tr><td>Jul     </td><td>Victoria</td><td>1.76    </td></tr>
	<tr><td>Aug     </td><td>Victoria</td><td>2.81    </td></tr>
	<tr><td>Sep     </td><td>Victoria</td><td>3.98    </td></tr>
	<tr><td>Oct     </td><td>Victoria</td><td>5.32    </td></tr>
	<tr><td>Nov     </td><td>Victoria</td><td>5.12    </td></tr>
	<tr><td>Dec     </td><td>Victoria</td><td>4.17    </td></tr>
	<tr><td>Jan     </td><td>Simiyu  </td><td>2.91    </td></tr>
	<tr><td>Feb     </td><td>Simiyu  </td><td>1.80    </td></tr>
	<tr><td>Mar     </td><td>Simiyu  </td><td>2.98    </td></tr>
	<tr><td>Apr     </td><td>Simiyu  </td><td>4.75    </td></tr>
	<tr><td>May     </td><td>Simiyu  </td><td>4.08    </td></tr>
	<tr><td>Jun     </td><td>Simiyu  </td><td>1.05    </td></tr>
	<tr><td>Jul     </td><td>Simiyu  </td><td>0.20    </td></tr>
	<tr><td>Aug     </td><td>Simiyu  </td><td>0.33    </td></tr>
	<tr><td>Sep     </td><td>Simiyu  </td><td>1.21    </td></tr>
	<tr><td>Oct     </td><td>Simiyu  </td><td>2.45    </td></tr>
	<tr><td>Nov     </td><td>Simiyu  </td><td>3.09    </td></tr>
	<tr><td>Dec     </td><td>Simiyu  </td><td>3.89    </td></tr>
</tbody>
</table>



### Piping
It may be unproductive to create one line of code for each data cleaning and manipulating function. Sometimes you will see programmers use a pipe. A pipe looks like this: %>%. Piping can make your code easier to read and efficient. For example, everything I just did above can be done in one line of code using a couple of pipes.


```R
new_data<-clean_data%>%select(-Period)%>%filter(Rainfall_mm>1)%>%filter(Lake=="Victoria")
print(new_data)
```

       Month     Lake Rainfall_mm
    1    Jan Victoria        3.18
    2    Feb Victoria        3.48
    3    Mar Victoria        4.69
    4    Apr Victoria        7.00
    5    May Victoria        9.36
    6    Jun Victoria        3.43
    7    Jul Victoria        1.76
    8    Aug Victoria        2.81
    9    Sep Victoria        3.98
    10   Oct Victoria        5.32
    11   Nov Victoria        5.12
    12   Dec Victoria        4.17
    

You can see how piping made our code more efficient. In one line of code we were able to create a new data set that removed the period column, and outputed only values for Lake Victoria that were greater than 1 mm. This is very useful for large datasets.

## Summarize Data

The dplyr function also allows you to summarize your data using different summary functions.


```R
summarise(clean_data, avg=mean(Rainfall_mm))
```


<table>
<thead><tr><th scope=col>avg</th></tr></thead>
<tbody>
	<tr><td>3.46</td></tr>
</tbody>
</table>



You can add more than one summary function in each argument.


```R
summarise(clean_data, avg=mean(Rainfall_mm), n=n(), sd=sd(Rainfall_mm), var=var(Rainfall_mm),median=median(Rainfall_mm), min=min(Rainfall_mm), max=max(Rainfall_mm))
```


<table>
<thead><tr><th scope=col>avg</th><th scope=col>n</th><th scope=col>sd</th><th scope=col>var</th><th scope=col>median</th><th scope=col>min</th><th scope=col>max</th></tr></thead>
<tbody>
	<tr><td>3.46    </td><td>24      </td><td>2.055855</td><td>4.226539</td><td>3.305   </td><td>0.2     </td><td>9.36    </td></tr>
</tbody>
</table>



It may be more useful to look at the average rainfall for both groups. We can find the means of separate groups by using the group_by function and piping it to the summarise function. The code would look like this:


```R
clean_data%>%group_by(Lake)%>%summarise(avg=mean(Rainfall_mm), sd=sd(Rainfall_mm), min=min(Rainfall_mm), max=max(Rainfall_mm))
```


<table>
<thead><tr><th scope=col>Lake</th><th scope=col>avg</th><th scope=col>sd</th><th scope=col>min</th><th scope=col>max</th></tr></thead>
<tbody>
	<tr><td>Simiyu  </td><td>2.395   </td><td>1.488236</td><td>0.20    </td><td>4.75    </td></tr>
	<tr><td>Victoria</td><td>4.525   </td><td>2.036613</td><td>1.76    </td><td>9.36    </td></tr>
</tbody>
</table>



## Creating New Variables

Dplyr also allows you to create new variables within your data set using the mutate function. In this function you are computing a new variable from an existing variable. The basic code for the function is:

- mutate(df, name of new column= formula)

For example, let's say we want to transform the Rainfall_mm to measure rainfall in inches. 


```R
clean_data_2<-mutate(clean_data, Rainfall_inches=Rainfall_mm*0.039)
clean_data_2
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Lake</th><th scope=col>Rainfall_mm</th><th scope=col>Rainfall_inches</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td>Victoria </td><td>3.18     </td><td>0.12402  </td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>Victoria </td><td>3.48     </td><td>0.13572  </td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>Victoria </td><td>4.69     </td><td>0.18291  </td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>Victoria </td><td>7.00     </td><td>0.27300  </td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>Victoria </td><td>9.36     </td><td>0.36504  </td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>Victoria </td><td>3.43     </td><td>0.13377  </td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>Victoria </td><td>1.76     </td><td>0.06864  </td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>Victoria </td><td>2.81     </td><td>0.10959  </td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>Victoria </td><td>3.98     </td><td>0.15522  </td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>Victoria </td><td>5.32     </td><td>0.20748  </td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>Victoria </td><td>5.12     </td><td>0.19968  </td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>Victoria </td><td>4.17     </td><td>0.16263  </td></tr>
	<tr><td>Jan      </td><td>2001-2019</td><td>Simiyu   </td><td>2.91     </td><td>0.11349  </td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>Simiyu   </td><td>1.80     </td><td>0.07020  </td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>Simiyu   </td><td>2.98     </td><td>0.11622  </td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>Simiyu   </td><td>4.75     </td><td>0.18525  </td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>Simiyu   </td><td>4.08     </td><td>0.15912  </td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>Simiyu   </td><td>1.05     </td><td>0.04095  </td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>Simiyu   </td><td>0.20     </td><td>0.00780  </td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>Simiyu   </td><td>0.33     </td><td>0.01287  </td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>Simiyu   </td><td>1.21     </td><td>0.04719  </td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>Simiyu   </td><td>2.45     </td><td>0.09555  </td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>Simiyu   </td><td>3.09     </td><td>0.12051  </td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>Simiyu   </td><td>3.89     </td><td>0.15171  </td></tr>
</tbody>
</table>



## Combining Data Sets

Dplyr also has the ability to combine data sets into one. Let's read in some more lake data and combine it with our current data set!


```R
lake_data<-read.table("lake_data.txt")
print(lake_data)

```

       Month    Period   SeaHawk   Randall
    1    Jan 2001-2019  3.125990 10.915324
    2    Feb 2001-2019 12.441331  5.148206
    3    Mar 2001-2019  3.230104  5.982370
    4    Apr 2001-2019 10.907835  6.179094
    5    May 2001-2019  9.674827  8.532295
    6    Jun 2001-2019  8.033946  6.132740
    7    Jul 2001-2019 12.318558 14.437468
    8    Aug 2001-2019  6.557286 11.269315
    9    Sep 2001-2019  6.057994 15.059383
    10   Oct 2001-2019 12.040164 10.124065
    11   Nov 2001-2019  5.369271  2.171475
    12   Dec 2001-2019  8.132666 12.947003
    

Now we have two more lakes: Lake SeaHawk and Lake Randall along with their monthly rainfall in mm. Let's clean and tidy our data to make it look like our clean_data data frame.


```R
library(tidyr)
lake_data<-lake_data %>% gather(key="Lake",value="Rainfall_mm",3:4)%>%mutate(Rainfall_inches=Rainfall_mm*0.039)
lake_data
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Lake</th><th scope=col>Rainfall_mm</th><th scope=col>Rainfall_inches</th></tr></thead>
<tbody>
	<tr><td>Jan       </td><td>2001-2019 </td><td>SeaHawk   </td><td> 3.125990 </td><td>0.12191361</td></tr>
	<tr><td>Feb       </td><td>2001-2019 </td><td>SeaHawk   </td><td>12.441331 </td><td>0.48521192</td></tr>
	<tr><td>Mar       </td><td>2001-2019 </td><td>SeaHawk   </td><td> 3.230104 </td><td>0.12597405</td></tr>
	<tr><td>Apr       </td><td>2001-2019 </td><td>SeaHawk   </td><td>10.907835 </td><td>0.42540556</td></tr>
	<tr><td>May       </td><td>2001-2019 </td><td>SeaHawk   </td><td> 9.674827 </td><td>0.37731825</td></tr>
	<tr><td>Jun       </td><td>2001-2019 </td><td>SeaHawk   </td><td> 8.033946 </td><td>0.31332390</td></tr>
	<tr><td>Jul       </td><td>2001-2019 </td><td>SeaHawk   </td><td>12.318558 </td><td>0.48042378</td></tr>
	<tr><td>Aug       </td><td>2001-2019 </td><td>SeaHawk   </td><td> 6.557286 </td><td>0.25573414</td></tr>
	<tr><td>Sep       </td><td>2001-2019 </td><td>SeaHawk   </td><td> 6.057994 </td><td>0.23626178</td></tr>
	<tr><td>Oct       </td><td>2001-2019 </td><td>SeaHawk   </td><td>12.040164 </td><td>0.46956640</td></tr>
	<tr><td>Nov       </td><td>2001-2019 </td><td>SeaHawk   </td><td> 5.369271 </td><td>0.20940156</td></tr>
	<tr><td>Dec       </td><td>2001-2019 </td><td>SeaHawk   </td><td> 8.132666 </td><td>0.31717398</td></tr>
	<tr><td>Jan       </td><td>2001-2019 </td><td>Randall   </td><td>10.915324 </td><td>0.42569765</td></tr>
	<tr><td>Feb       </td><td>2001-2019 </td><td>Randall   </td><td> 5.148206 </td><td>0.20078002</td></tr>
	<tr><td>Mar       </td><td>2001-2019 </td><td>Randall   </td><td> 5.982370 </td><td>0.23331245</td></tr>
	<tr><td>Apr       </td><td>2001-2019 </td><td>Randall   </td><td> 6.179094 </td><td>0.24098468</td></tr>
	<tr><td>May       </td><td>2001-2019 </td><td>Randall   </td><td> 8.532295 </td><td>0.33275951</td></tr>
	<tr><td>Jun       </td><td>2001-2019 </td><td>Randall   </td><td> 6.132740 </td><td>0.23917686</td></tr>
	<tr><td>Jul       </td><td>2001-2019 </td><td>Randall   </td><td>14.437468 </td><td>0.56306127</td></tr>
	<tr><td>Aug       </td><td>2001-2019 </td><td>Randall   </td><td>11.269315 </td><td>0.43950329</td></tr>
	<tr><td>Sep       </td><td>2001-2019 </td><td>Randall   </td><td>15.059383 </td><td>0.58731592</td></tr>
	<tr><td>Oct       </td><td>2001-2019 </td><td>Randall   </td><td>10.124065 </td><td>0.39483852</td></tr>
	<tr><td>Nov       </td><td>2001-2019 </td><td>Randall   </td><td> 2.171475 </td><td>0.08468754</td></tr>
	<tr><td>Dec       </td><td>2001-2019 </td><td>Randall   </td><td>12.947003 </td><td>0.50493311</td></tr>
</tbody>
</table>



Let's combine our two data sets!


```R
All_lakes<-bind_rows(clean_data_2,lake_data)
All_lakes$Month<-as.factor(All_lakes$Month)
All_lakes$Lake<-as.factor(All_lakes$Lake)
print(All_lakes)

```

    Warning message in bind_rows_(x, .id):
    "binding factor and character vector, coercing into character vector"Warning message in bind_rows_(x, .id):
    "binding character and factor vector, coercing into character vector"

       Month    Period     Lake Rainfall_mm Rainfall_inches
    1    Jan 2001-2019 Victoria    3.180000      0.12402000
    2    Feb 2001-2019 Victoria    3.480000      0.13572000
    3    Mar 2001-2019 Victoria    4.690000      0.18291000
    4    Apr 2001-2019 Victoria    7.000000      0.27300000
    5    May 2001-2019 Victoria    9.360000      0.36504000
    6    Jun 2001-2019 Victoria    3.430000      0.13377000
    7    Jul 2001-2019 Victoria    1.760000      0.06864000
    8    Aug 2001-2019 Victoria    2.810000      0.10959000
    9    Sep 2001-2019 Victoria    3.980000      0.15522000
    10   Oct 2001-2019 Victoria    5.320000      0.20748000
    11   Nov 2001-2019 Victoria    5.120000      0.19968000
    12   Dec 2001-2019 Victoria    4.170000      0.16263000
    13   Jan 2001-2019   Simiyu    2.910000      0.11349000
    14   Feb 2001-2019   Simiyu    1.800000      0.07020000
    15   Mar 2001-2019   Simiyu    2.980000      0.11622000
    16   Apr 2001-2019   Simiyu    4.750000      0.18525000
    17   May 2001-2019   Simiyu    4.080000      0.15912000
    18   Jun 2001-2019   Simiyu    1.050000      0.04095000
    19   Jul 2001-2019   Simiyu    0.200000      0.00780000
    20   Aug 2001-2019   Simiyu    0.330000      0.01287000
    21   Sep 2001-2019   Simiyu    1.210000      0.04719000
    22   Oct 2001-2019   Simiyu    2.450000      0.09555000
    23   Nov 2001-2019   Simiyu    3.090000      0.12051000
    24   Dec 2001-2019   Simiyu    3.890000      0.15171000
    25   Jan 2001-2019  SeaHawk    3.125990      0.12191361
    26   Feb 2001-2019  SeaHawk   12.441331      0.48521192
    27   Mar 2001-2019  SeaHawk    3.230104      0.12597405
    28   Apr 2001-2019  SeaHawk   10.907835      0.42540556
    29   May 2001-2019  SeaHawk    9.674827      0.37731825
    30   Jun 2001-2019  SeaHawk    8.033946      0.31332390
    31   Jul 2001-2019  SeaHawk   12.318558      0.48042378
    32   Aug 2001-2019  SeaHawk    6.557286      0.25573414
    33   Sep 2001-2019  SeaHawk    6.057994      0.23626178
    34   Oct 2001-2019  SeaHawk   12.040164      0.46956640
    35   Nov 2001-2019  SeaHawk    5.369271      0.20940156
    36   Dec 2001-2019  SeaHawk    8.132666      0.31717398
    37   Jan 2001-2019  Randall   10.915324      0.42569765
    38   Feb 2001-2019  Randall    5.148206      0.20078002
    39   Mar 2001-2019  Randall    5.982370      0.23331245
    40   Apr 2001-2019  Randall    6.179094      0.24098468
    41   May 2001-2019  Randall    8.532295      0.33275951
    42   Jun 2001-2019  Randall    6.132740      0.23917686
    43   Jul 2001-2019  Randall   14.437468      0.56306127
    44   Aug 2001-2019  Randall   11.269315      0.43950329
    45   Sep 2001-2019  Randall   15.059383      0.58731592
    46   Oct 2001-2019  Randall   10.124065      0.39483852
    47   Nov 2001-2019  Randall    2.171475      0.08468754
    48   Dec 2001-2019  Randall   12.947003      0.50493311
    

The bind_rows function adds the rows from your second argument to the dataframe in your first argument. You can also use the bind_cols function to add the columns from one dataset to the other. For this example, let's get our data back into wide format.


```R
clean_data_wide<-clean_data%>%spread(key="Lake",value="Rainfall_mm")
clean_data_wide
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Simiyu</th><th scope=col>Victoria</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td>2.91     </td><td>3.18     </td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>1.80     </td><td>3.48     </td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>2.98     </td><td>4.69     </td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>4.75     </td><td>7.00     </td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>4.08     </td><td>9.36     </td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>1.05     </td><td>3.43     </td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>0.20     </td><td>1.76     </td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>0.33     </td><td>2.81     </td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>1.21     </td><td>3.98     </td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>2.45     </td><td>5.32     </td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>3.09     </td><td>5.12     </td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>3.89     </td><td>4.17     </td></tr>
</tbody>
</table>




```R
lake_data_wide<-read.table("lake_data.txt")
All_lakes_wide<-bind_cols(clean_data_wide,lake_data_wide)
All_lakes_wide
```


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Simiyu</th><th scope=col>Victoria</th><th scope=col>Month1</th><th scope=col>Period1</th><th scope=col>SeaHawk</th><th scope=col>Randall</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td>2.91     </td><td>3.18     </td><td>Jan      </td><td>2001-2019</td><td> 3.125990</td><td>10.915324</td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>1.80     </td><td>3.48     </td><td>Feb      </td><td>2001-2019</td><td>12.441331</td><td> 5.148206</td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>2.98     </td><td>4.69     </td><td>Mar      </td><td>2001-2019</td><td> 3.230104</td><td> 5.982370</td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>4.75     </td><td>7.00     </td><td>Apr      </td><td>2001-2019</td><td>10.907835</td><td> 6.179094</td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>4.08     </td><td>9.36     </td><td>May      </td><td>2001-2019</td><td> 9.674827</td><td> 8.532295</td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>1.05     </td><td>3.43     </td><td>Jun      </td><td>2001-2019</td><td> 8.033946</td><td> 6.132740</td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>0.20     </td><td>1.76     </td><td>Jul      </td><td>2001-2019</td><td>12.318558</td><td>14.437468</td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>0.33     </td><td>2.81     </td><td>Aug      </td><td>2001-2019</td><td> 6.557286</td><td>11.269315</td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>1.21     </td><td>3.98     </td><td>Sep      </td><td>2001-2019</td><td> 6.057994</td><td>15.059383</td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>2.45     </td><td>5.32     </td><td>Oct      </td><td>2001-2019</td><td>12.040164</td><td>10.124065</td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>3.09     </td><td>5.12     </td><td>Nov      </td><td>2001-2019</td><td> 5.369271</td><td> 2.171475</td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>3.89     </td><td>4.17     </td><td>Dec      </td><td>2001-2019</td><td> 8.132666</td><td>12.947003</td></tr>
</tbody>
</table>



You'll notice that this created two new columns "Month1" and "Period1" because they share the same name in both datasets. To account for this you can use the full join function. Using the "by=" argument, you can join columns that have the same name and values.


```R
full_join(clean_data_wide,lake_data_wide, by=c("Month","Period"))
```

    Warning message:
    "Column `Month` joining factors with different levels, coercing to character vector"


<table>
<thead><tr><th scope=col>Month</th><th scope=col>Period</th><th scope=col>Simiyu</th><th scope=col>Victoria</th><th scope=col>SeaHawk</th><th scope=col>Randall</th></tr></thead>
<tbody>
	<tr><td>Jan      </td><td>2001-2019</td><td>2.91     </td><td>3.18     </td><td> 3.125990</td><td>10.915324</td></tr>
	<tr><td>Feb      </td><td>2001-2019</td><td>1.80     </td><td>3.48     </td><td>12.441331</td><td> 5.148206</td></tr>
	<tr><td>Mar      </td><td>2001-2019</td><td>2.98     </td><td>4.69     </td><td> 3.230104</td><td> 5.982370</td></tr>
	<tr><td>Apr      </td><td>2001-2019</td><td>4.75     </td><td>7.00     </td><td>10.907835</td><td> 6.179094</td></tr>
	<tr><td>May      </td><td>2001-2019</td><td>4.08     </td><td>9.36     </td><td> 9.674827</td><td> 8.532295</td></tr>
	<tr><td>Jun      </td><td>2001-2019</td><td>1.05     </td><td>3.43     </td><td> 8.033946</td><td> 6.132740</td></tr>
	<tr><td>Jul      </td><td>2001-2019</td><td>0.20     </td><td>1.76     </td><td>12.318558</td><td>14.437468</td></tr>
	<tr><td>Aug      </td><td>2001-2019</td><td>0.33     </td><td>2.81     </td><td> 6.557286</td><td>11.269315</td></tr>
	<tr><td>Sep      </td><td>2001-2019</td><td>1.21     </td><td>3.98     </td><td> 6.057994</td><td>15.059383</td></tr>
	<tr><td>Oct      </td><td>2001-2019</td><td>2.45     </td><td>5.32     </td><td>12.040164</td><td>10.124065</td></tr>
	<tr><td>Nov      </td><td>2001-2019</td><td>3.09     </td><td>5.12     </td><td> 5.369271</td><td> 2.171475</td></tr>
	<tr><td>Dec      </td><td>2001-2019</td><td>3.89     </td><td>4.17     </td><td> 8.132666</td><td>12.947003</td></tr>
</tbody>
</table>



Always read any warning messages that R outputs. You'll notice that because the month columns have different levels, R automatically converts it to a character vector. In this case, make sure you refactor your data if necessary. 

There are many other functions that dplyr offers. I would recommend exploring different functions to learn how to properly manipulate your data. In the next module we will learn all about t-tests!
