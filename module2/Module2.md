# Module 2: Getting Started on R
## Learning the Basics
### Hawken Hass
### University of North Carolina Wilmington

## Preparing Your Work Space

Welcome to the basics of R! In this document you will learn the basics of R. In this lesson you will learn:

-   Setting up your R work space
-   Installing packages
-   Creating a data frame
-   Importing a data file

However, before this step you must install R and Rstudio. To do so click this link:

https://www.r-project.org/

# Setting Up Your R Work Space

The first step to setting up your digital work space is to set your **Working Directory**. This will tell R where to save your files, plots, code, and anything else you want to save to your computer. Therefore, you will need to create a file on your computer dedicated toall of your R files.

Once you create your folder you will need to set your working directory. Use the path that brings you to your specified folder. An example might look like this:

    setwd("c:/Documents/R Stuff")

In this case the folder you made would be called "R stuff" and is located in your Documents file on your computer.

## Installing Packages

Packages are important if you want to run certain analyses on your data.To install and activate a package type the following code:


install.package(package.name)

library(package.name)

Once you have a package installed you will not have to re install it every time you open RStudio. You will however, need to activate it every time you open RStudio using the library function.

## Importing Data

Now that you have you work space all set up, you can now start working with your data! If you don't have an existing data set you will need to make one. You can make one in RStudio (which we will cover below), or you can make one in other data analysis software. R accepts the following file types:

-   SPSS (.sav)
-   Excel (.xlsx)
-   SAS (.sas)
-   Stata (.dta)
-   Text file (.csv, .txt)

In order to import your data you will need it downloaded onto your computer and then activate a package called "Haven". Haven allows you to read in these different data file types with code.
```
    library(haven)
```
Then type the following code:

**For Excel:**
```
    library(readxl)
    mydata<-read_excel("filename.xlsx")
```
**For SPSS**
```
    library(haven)
    mydata<-read.spss("filename.sav")
```
**For SAS**

        library(haven)
        mydata<-read_sas("filename.sas")

**For Stata**
```
    library(haven)
    mydata<-read_dta("filename.dta")
```
**For Text**
```
    library(haven)
    mydata<-read.table("filename.txt")

    OR

    mydata<-read.csv("filename.csv")
```
Always name your data in R. As you can see, I named the data "mydata" followed by an arrow and the code. Make sure the data file is in your working directory

# Creating an Object

Everything in R is an object and every object must be defined before you can include it in your functions. In R, we define objects using an arrow. For example, if we run the following code:


```R
x<-20
```

You will see that in your global environment x is now defined as a value, 20. If you type in "x" in your command center you will get the following output:


```R
x
```


20


Now you can use this variable in functions! For example:


```R
x+20
```


40


# Common Functions in R

- **<-** : Assigns a value to a variable
- **$** : Extracts part of an object (for example a single column from a data set)
- **#** : Allows you to add comments into script that will not be treated as code
- **c()** : Combines objects into a vector
- **library()** : Loads a package specificed inside parentheses
- **setwd()** : Sets a file path for your working directory
- **factor()** : Allows you to transform a categorical variable into a factor variable
- **data.frame** : Creates a data frame
- **View()** : Opens a new tab to view data as a spreadsheet
- **print()** : Prints argument into output zone
- **ggplot()** : Creates a base canvas for a plot object
- **summarytools::freq()** : Creates a frequency table
- **describe()** : Reports descriptive statistics (such as mean, median, standard deviation...)
- **describeBy()** : Reports basic descriptive statistics organized by a grouping variable of your choice

# Creating a Data Frame
## Creating a Vector
Lets say you don't have a data file yet and you need to create one in R. No problem! First, you are going to need to create a vector of data. If you remember from the previous module, a vector is simply a column of data for whatever variable you choose. To do so, you will need to use the concatenate function. 


Note: You can make any type of data structure (matrix, array...) that we looked at in Module 1 and use it to create a data frame. 

Name your variable and type each data point separated by commas.


```R
Names<-c("Theodore", "Charles", "Elizabeth", "Marcus", "Megan", "Cynthia")
```

Adding a "c" before the parentheses combines all data points into one vector

Lets say you need to denote on your data file if a participant is male or female. Since this variable is categorical, you will need to create a vector of factor variables. Factor variables categorize the data as numerical values so it can still be used in statistical analysis. Let's say males are "1" and females are "2".

## Factoring a Variable


```R
Gender<-c(1,1,2,1,2,2)
```

Make sure that the order of the number matches with the order of your other vectors. If you have a lot of data points you can use this code:


```R
Gender_2<-c(rep(1:2, each=3))
```

This will repeat 1 and 2 three times each. Just put "each = n" with n being the number of times you want to repeat the data point. 


```R
print(Gender_2)
```

    [1] 1 1 1 2 2 2
    

Now we need to factor the data so that R can run statistical analyses and plot the data.



```R
Gender<-factor(Gender, levels = c(1:2), labels=c("Male", "Female"))
```

Now if you view this vector you can see that it has two levels: Male and Female.


```R
print(Gender)
```

    [1] Male   Male   Female Male   Female Female
    Levels: Male Female
    

This function is particularly helpful to show group assignment. For instance "1" could be factored as "Control Group" and "2" could be factored as "Experimental Group".

Now let's put in one more vector.


```R
scores<-c(19,20,4,17,2,5)
```

Once you input all of your vectors, or variables, you can combine them all into one data set using the "data.frame" function. Type each vector name in the order in which you want it to show up in the data file. Make sure you name your data!


```R
Data_Example<-data.frame(Names, Gender, scores)
```

As you can see, this put all of our variables into one file.


```R
print(Data_Example)
```

          Names Gender scores
    1  Theodore   Male     19
    2   Charles   Male     20
    3 Elizabeth Female      4
    4    Marcus   Male     17
    5     Megan Female      2
    6   Cynthia Female      5
    

Now you can start running analyses on your data!

# Practice
Make your own data frame using at least one factor variable and run the code to see the output.


```R
Variable_1<-c()
Variable_2<-c()
Factor_Variable<-c()
Factor_Variable<-factor()
Data_Name_Practice<-data.frame()
```
