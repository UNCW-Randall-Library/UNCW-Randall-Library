# Module 4: Data Wrangling Part 1
## How to Clean and Manipulate your Data in R
## Hawken Hass
## University of North Carolina Wilmington

# Cleaning Your Data
Cleaning your data is an essential part of data analysis. Before you can run any analysis your data must be structured in a certain way. This will ensure that your analyses will run smoothly in any program. First, let's take a look at some messy data. This data file summarizes average monthly rainfall between the years of 2001 and 2019 in two different lakes: Lake Victoria and Lake Simiyu.


```R
library(readxl)
read_excel("messy-data.xlsx")
```

    New names:
    * `` -> ...2
    * `` -> ...3
    


<table>
<thead><tr><th scope=col>Seasonal rainfall in Lake Victoria and Simiyu</th><th scope=col>...2</th><th scope=col>...3</th></tr></thead>
<tbody>
	<tr><td>NA           </td><td>NA           </td><td>NA           </td></tr>
	<tr><td>Month, period</td><td>Lake Victoria</td><td>Simiyu       </td></tr>
	<tr><td>Jan,2001-2019</td><td>3.176mm      </td><td>2.908473684  </td></tr>
	<tr><td>Feb,2001-2019</td><td>3.477mm      </td><td>1.8mm        </td></tr>
	<tr><td>Mar,2001-2019</td><td>4.687052632  </td><td>2.981052632  </td></tr>
	<tr><td>Apr,2001-2019</td><td>7.004526316  </td><td>4.753578947  </td></tr>
	<tr><td>May,2001-2019</td><td>9.362789474  </td><td>4.077473684  </td></tr>
	<tr><td>Jun,2001-2019</td><td>3.430210526  </td><td>1.046947368  </td></tr>
	<tr><td>Jul,2001-2019</td><td>1.764421053  </td><td>0.1952105263 </td></tr>
	<tr><td>Aug,2001-2019</td><td>2.812631579  </td><td>0.3336315789 </td></tr>
	<tr><td>Sep,2001-2019</td><td>3.978894737  </td><td>1.205842105  </td></tr>
	<tr><td>Oct,2001-2019</td><td>5.318421053  </td><td>2.454736842  </td></tr>
	<tr><td>Nov,2001-2019</td><td>5.118473684  </td><td>3.091421053  </td></tr>
	<tr><td>Dec,2001-2019</td><td>4.168105263  </td><td>3.890052632  </td></tr>
</tbody>
</table>



You can see there are a few things wrong with this data set.

- The column headers are not accurate
- There is no data in row one
- The headers are in row two
- The month and period columns are combined into one column
- The total rainfall for each lake is rounded to different decimal points in some rows
- Some rows have characters (mm) while others do not
- The data is in wide format

Clearly this data set needs some serious tidying. But first, what does a tidy data set look like?

## Tidy Data

A tidy data set has two key features:

- Each variable is saved in its own column 
- Each variable is saved in its own row

This structure is specifically called long format, in which each observation has its own row. For example, in our data set there are 12 observations of rainfall for each lake. Therefore, we need 24 rows in our data set. Additionally, each variable needs to be saved in its own column. Thus, we need to create a column that denotes which group (Lake Victoria or Lake Simiyu) each observation belongs to. The data frame below shows what this data will look like once it is cleaned.


```R
read.table("clean_data.txt")
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



This looks much better! You'll notice all rainfall observations are in one column and each observation has its own row. Additionally, we have a column denoting group membership. This data is ready for analysis. So how did I get here? We are going to use a package called "tidyr".

## Cleaning using the tidyr package
First, I am going to read in my data and activate the tidyr package.


```R
library(tidyr)
messy_data<-read_excel("messy-data.xlsx")
print(messy_data)
```

    New names:
    * `` -> ...2
    * `` -> ...3
    

    # A tibble: 14 x 3
       `Seasonal rainfall in Lake Victoria and Simiyu` ...2          ...3        
       <chr>                                           <chr>         <chr>       
     1 <NA>                                            <NA>          <NA>        
     2 Month, period                                   Lake Victoria Simiyu      
     3 Jan,2001-2019                                   3.176mm       2.908473684 
     4 Feb,2001-2019                                   3.477mm       1.8mm       
     5 Mar,2001-2019                                   4.687052632   2.981052632 
     6 Apr,2001-2019                                   7.004526316   4.753578947 
     7 May,2001-2019                                   9.362789474   4.077473684 
     8 Jun,2001-2019                                   3.430210526   1.046947368 
     9 Jul,2001-2019                                   1.764421053   0.1952105263
    10 Aug,2001-2019                                   2.812631579   0.3336315789
    11 Sep,2001-2019                                   3.978894737   1.205842105 
    12 Oct,2001-2019                                   5.318421053   2.454736842 
    13 Nov,2001-2019                                   5.118473684   3.091421053 
    14 Dec,2001-2019                                   4.168105263   3.890052632 
    

The first step I am going to take is to separate the Month and Period column. If you'll notice, the month and period are both in one column, and separated by a comma. It is important that in your data set all variables have their own column. Thus, we are going to use the separate function to separate this column into two.


```R
messy_data<-separate(messy_data, "Seasonal rainfall in Lake Victoria and Simiyu", c("Month", "Period"), ",")
print(messy_data)
```

    # A tibble: 14 x 4
       Month Period    ...2          ...3        
       <chr> <chr>     <chr>         <chr>       
     1 <NA>  <NA>      <NA>          <NA>        
     2 Month " period" Lake Victoria Simiyu      
     3 Jan   2001-2019 3.176mm       2.908473684 
     4 Feb   2001-2019 3.477mm       1.8mm       
     5 Mar   2001-2019 4.687052632   2.981052632 
     6 Apr   2001-2019 7.004526316   4.753578947 
     7 May   2001-2019 9.362789474   4.077473684 
     8 Jun   2001-2019 3.430210526   1.046947368 
     9 Jul   2001-2019 1.764421053   0.1952105263
    10 Aug   2001-2019 2.812631579   0.3336315789
    11 Sep   2001-2019 3.978894737   1.205842105 
    12 Oct   2001-2019 5.318421053   2.454736842 
    13 Nov   2001-2019 5.118473684   3.091421053 
    14 Dec   2001-2019 4.168105263   3.890052632 
    

The separate function uses the following arguments:
- The first argument is your data file
- The second argument is the new names of the new columns you want to create in a vector format
- The third argument is how the two values are separate in the current column, in this case it is a comma


 Next, we are going to change the names of the columns to refelct more accurate names. This function will get rid of the "..2" and "..3" in columns 3 and 4. First, we are going to create a vector of all of our column names. Then assign these column names to our data file using the names function.


```R
varname<-c("Month", "Period", "Victoria", "Simiyu")
names(messy_data)<-varname
print(messy_data)
```

    # A tibble: 14 x 4
       Month Period    Victoria      Simiyu      
       <chr> <chr>     <chr>         <chr>       
     1 <NA>  <NA>      <NA>          <NA>        
     2 Month " period" Lake Victoria Simiyu      
     3 Jan   2001-2019 3.176mm       2.908473684 
     4 Feb   2001-2019 3.477mm       1.8mm       
     5 Mar   2001-2019 4.687052632   2.981052632 
     6 Apr   2001-2019 7.004526316   4.753578947 
     7 May   2001-2019 9.362789474   4.077473684 
     8 Jun   2001-2019 3.430210526   1.046947368 
     9 Jul   2001-2019 1.764421053   0.1952105263
    10 Aug   2001-2019 2.812631579   0.3336315789
    11 Sep   2001-2019 3.978894737   1.205842105 
    12 Oct   2001-2019 5.318421053   2.454736842 
    13 Nov   2001-2019 5.118473684   3.091421053 
    14 Dec   2001-2019 4.168105263   3.890052632 
    

The next step is to delete any missing data.


```R
messy_data<-na.omit(messy_data)
print(messy_data)
```

    # A tibble: 13 x 4
       Month Period    Victoria      Simiyu      
       <chr> <chr>     <chr>         <chr>       
     1 Month " period" Lake Victoria Simiyu      
     2 Jan   2001-2019 3.176mm       2.908473684 
     3 Feb   2001-2019 3.477mm       1.8mm       
     4 Mar   2001-2019 4.687052632   2.981052632 
     5 Apr   2001-2019 7.004526316   4.753578947 
     6 May   2001-2019 9.362789474   4.077473684 
     7 Jun   2001-2019 3.430210526   1.046947368 
     8 Jul   2001-2019 1.764421053   0.1952105263
     9 Aug   2001-2019 2.812631579   0.3336315789
    10 Sep   2001-2019 3.978894737   1.205842105 
    11 Oct   2001-2019 5.318421053   2.454736842 
    12 Nov   2001-2019 5.118473684   3.091421053 
    13 Dec   2001-2019 4.168105263   3.890052632 
    

We also want to delete Row 1 because this row was the names of the columns in the excel file.


```R
messy_data<-messy_data[-c(1),]
print(messy_data)
```

    # A tibble: 12 x 4
       Month Period    Victoria    Simiyu      
       <chr> <chr>     <chr>       <chr>       
     1 Jan   2001-2019 3.176mm     2.908473684 
     2 Feb   2001-2019 3.477mm     1.8mm       
     3 Mar   2001-2019 4.687052632 2.981052632 
     4 Apr   2001-2019 7.004526316 4.753578947 
     5 May   2001-2019 9.362789474 4.077473684 
     6 Jun   2001-2019 3.430210526 1.046947368 
     7 Jul   2001-2019 1.764421053 0.1952105263
     8 Aug   2001-2019 2.812631579 0.3336315789
     9 Sep   2001-2019 3.978894737 1.205842105 
    10 Oct   2001-2019 5.318421053 2.454736842 
    11 Nov   2001-2019 5.118473684 3.091421053 
    12 Dec   2001-2019 4.168105263 3.890052632 
    

The '-c' denotes that you want to delete a column or a row. If you wanted to delete mulitple rows you could type:

```{r}
[-c(1,2,3,4),]
```
The argument after the comma represents columns. If you wanted to delete column 1 you could type:

```{r}
[, -c(1)]
```

Next, we want to convert our data to the long format. In this case we want a column that denotes which lake the data point is from, and a column denoting how much rainfall. We want to make sure each group is paired with the correct data point. For this, we are going to be using the gather function. 


```R
messy_data<-gather(messy_data, key="Lake", value="Rainfall_mm", 3:4)
print(messy_data)
```

    # A tibble: 24 x 4
       Month Period    Lake     Rainfall_mm
       <chr> <chr>     <chr>    <chr>      
     1 Jan   2001-2019 Victoria 3.176mm    
     2 Feb   2001-2019 Victoria 3.477mm    
     3 Mar   2001-2019 Victoria 4.687052632
     4 Apr   2001-2019 Victoria 7.004526316
     5 May   2001-2019 Victoria 9.362789474
     6 Jun   2001-2019 Victoria 3.430210526
     7 Jul   2001-2019 Victoria 1.764421053
     8 Aug   2001-2019 Victoria 2.812631579
     9 Sep   2001-2019 Victoria 3.978894737
    10 Oct   2001-2019 Victoria 5.318421053
    # ... with 14 more rows
    

The gather function uses the following arguments:

- The first argument is the data frame name
- The second argument is the new name of the column you want to create for the grouping variable. I chose "Lake" because I want to take the names of each lake and repeat it in the column twelve times; the number of observations of rainfall for that lake.
- The third argument is the name of the column for the value you want to pair with the grouping variable. I chose "Rainfall_mm" because that is the observation or data point I want to pair with each grouping variable.
- The final argument is the range of columns you want to turn into key-value pairs

Let's now remove some unnecessary characters. 


```R
messy_data$Rainfall_mm<-gsub('mm', '',messy_data$Rainfall_mm)
messy_data$Rainfall_mm<-as.numeric(messy_data$Rainfall_mm)
print(messy_data)
```

    # A tibble: 24 x 4
       Month Period    Lake     Rainfall_mm
       <chr> <chr>     <chr>          <dbl>
     1 Jan   2001-2019 Victoria        3.18
     2 Feb   2001-2019 Victoria        3.48
     3 Mar   2001-2019 Victoria        4.69
     4 Apr   2001-2019 Victoria        7.00
     5 May   2001-2019 Victoria        9.36
     6 Jun   2001-2019 Victoria        3.43
     7 Jul   2001-2019 Victoria        1.76
     8 Aug   2001-2019 Victoria        2.81
     9 Sep   2001-2019 Victoria        3.98
    10 Oct   2001-2019 Victoria        5.32
    # ... with 14 more rows
    

The gsub function uses the following arguments:

- The first argument is the string of characters you want to remove from each data point. I want to remove the "mm: off the end data points that have it
- The second argument denotes what you want to replace the "mm" character string with. In this case I put a space because I don't want anything to replace it
- The final argument is the column you want to apply this function to

When using the gsub function, the column is automatically converted to a character variable. Use the as.numeric function to convert the column back to a numeric variable.

Finally, let's ensure that all observations in the rainfall column are rounded to the same decimal point.


```R
messy_data[,'Rainfall_mm']=round(messy_data[,'Rainfall_mm'],2)
print(messy_data)
```

    # A tibble: 24 x 4
       Month Period    Lake     Rainfall_mm
       <chr> <chr>     <chr>          <dbl>
     1 Jan   2001-2019 Victoria        3.18
     2 Feb   2001-2019 Victoria        3.48
     3 Mar   2001-2019 Victoria        4.69
     4 Apr   2001-2019 Victoria        7   
     5 May   2001-2019 Victoria        9.36
     6 Jun   2001-2019 Victoria        3.43
     7 Jul   2001-2019 Victoria        1.76
     8 Aug   2001-2019 Victoria        2.81
     9 Sep   2001-2019 Victoria        3.98
    10 Oct   2001-2019 Victoria        5.32
    # ... with 14 more rows
    

Our data is clean now! Let's rename it "clean data"


```R
clean_data<-messy_data
```

To save it as new data file, use the write.table function. This will save the new version of your data file to your computer.


```R
write.table(clean_data, file = "clean_data.txt")
read.table("clean_data.txt")
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



## Other Tidy R Functions

The above functions only scratch the surface of the tidyr package. The reshape functions mentioned above have their reverse counterparts. For example, if you want to spread rows across columns, you will want to use the spread function. For example, if you want two columns for rainfall in each lake, you would use the following code.


```R
wide_data<-spread(clean_data, "Lake", "Rainfall_mm")
print(wide_data)
```

    # A tibble: 12 x 4
       Month Period    Simiyu Victoria
       <chr> <chr>      <dbl>    <dbl>
     1 Apr   2001-2019   4.75     7   
     2 Aug   2001-2019   0.33     2.81
     3 Dec   2001-2019   3.89     4.17
     4 Feb   2001-2019   1.8      3.48
     5 Jan   2001-2019   2.91     3.18
     6 Jul   2001-2019   0.2      1.76
     7 Jun   2001-2019   1.05     3.43
     8 Mar   2001-2019   2.98     4.69
     9 May   2001-2019   4.08     9.36
    10 Nov   2001-2019   3.09     5.12
    11 Oct   2001-2019   2.45     5.32
    12 Sep   2001-2019   1.21     3.98
    

Like before, we used the separate function to separate the Month and Period columns, but you can reunite these columns using the unite function.


```R
wide_data<-unite(wide_data,col="Month,Period",c("Month", "Period"), sep=",")
print(wide_data)
```

    # A tibble: 12 x 3
       `Month,Period` Simiyu Victoria
       <chr>           <dbl>    <dbl>
     1 Apr,2001-2019    4.75     7   
     2 Aug,2001-2019    0.33     2.81
     3 Dec,2001-2019    3.89     4.17
     4 Feb,2001-2019    1.8      3.48
     5 Jan,2001-2019    2.91     3.18
     6 Jul,2001-2019    0.2      1.76
     7 Jun,2001-2019    1.05     3.43
     8 Mar,2001-2019    2.98     4.69
     9 May,2001-2019    4.08     9.36
    10 Nov,2001-2019    3.09     5.12
    11 Oct,2001-2019    2.45     5.32
    12 Sep,2001-2019    1.21     3.98
    

In Part 2 we will discuss how to manipulate your data using the dplyr package!
