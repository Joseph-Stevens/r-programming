#R Programming
##Subsetting
2015-06-01. Initial examples were from the Coursera R Programming.
They rely on sourcing the diet_data so I might want to keep that after all.

TODO. Generate some data and rewrite the example.

##Subsetting Vectors
Create a vector of 100 integers in random order.
```
myV <- sample(1:100)
```
####Task: Return a single element.
Syntax: vector[index]
Example: Return the eighth element.
```
myV[8]
[1] 16
```

####Task: Return a range of elements.
Syntax: vector[from:to]
Example: Return elements 1 through 10.
```
myV[1:10]
[1] 32 98 90 28 36 77 87 16 30  3
```

####Task: Find the index for the lowest value or the highest value in the vector.
Min and Max have the option to omit NA values. na.rm = TRUE
```
min(myV)
[1] 1
which.min(myV)
[1] 90
myV[90]
[1] 1

max(myV)
[1] 100
which.max(myV)
[1] 18
myV[18]
[1] 100
```

####Task: Return the last indices and first elements. (The endpoints.)
```
end(myV)
[1] 100   1
```
####Task: Return the elements of the vector in order of their value.
```
myV[order(myV)]
[1]   1   2   3   4   5   6   7   8   9  10 ...
```

####Task: Returns **index** of values by order of the value. 
```
order(myV)
[1]  17  86  67  38  79  27   5  13   7  95 ...
```  

###Other Examples
####Task: Return the NA values in a logical vector.
```
x[is.na(x)]
[1] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
```

####Task: Return all the values which are not NA.
Create a new vector of just the non-NA values.
```
 > newV <- x[!is.na(x)]
```


##Subsetting Lists
Per HW: "Subsetting a list works in the same way as subsetting an atomic vector. 
* Using `[` will always return a list
* `[[` and `$`, as described below, let you pull out the components of the list."

TODO Add examples

##Subsetting Matrices
TODO Add examples

##Subsetting Data Frames
* Subset syntax: table[row, column]
* Subset syntax: dataframe[observation, variable]

###Subsetting based on a condition: using the which() function to check a value.

####Task: Return all columns of any row (observation) where the value of Day is 30.
Note: If returning all rows or all columns, just leave it blank.
You don't even have to leave a space. I just do it now to remind myself.

**Example**
```
andy_david[which(andy_david[ ,"Day"]==30), ]
Result
   Patient.Name Age Weight Day
30         Andy  30    135  30
60        David  35    201  30
```

####Task: Return just the Weight column for any row (observation) where the value of Day is 30.
```
andy_david[which(andy_david[ ,"Day"]==30), "Weight"]
[1] 135 201
```

####Task: Return the Median Weight of All Observations for Day 30: Elegant and compressed.
**Example 1**
```
>  median(andy_david[which(andy_david[ ,"Day"]==30), "Weight"])
Result
[1] 168
```

**Example 2**
Another way to write this which is easier to read but takes two steps: first get all the rows for the 30th day and then take the median of the result.
```
> day_30 <- andy_david[which(andy_david[ ,"Day"]==30), ]
> day_30
   Patient.Name Age Weight Day
30         Andy  30    135  30
60        David  35    201  30

> median(day_30$Weight)
[1] 168
```

**Example 3: From JSON converted to data frame**
Returns the value in the created_at column for any rows where the name column has a value of "datasharing".
```
> my_json[which(my_json$name=="datasharing"), "created_at"]
[1] "2013-11-07T13:25:07Z"
```.

##Subsetting using the dplyr package
Notes: Initial notes on the Swirl tutorial.
> "We've seen how to select a subset of columns and rows from our dataset using select() and filter(), respectively. Inherent in select() was also the ability to arrange our selected columns in any order we please.

> Sometimes we want to order the rows of a dataset according to the values of a particular variable. This is the job of arrange()."

TODO. Not all these commands are related to subsetting. Need to move them to their functional categories.

##Select
Syntax: select(dt, fromColumn:toColumn)
Example:
cran2 <- select(cran, size:ip_id)

##Filter
###Task: Return if both conditions are true.
Syntax:
Example:
```
filter(cran, r_version=="3.1.1", country=="US")
```

###Task: Return if either condition is true. Using | (or).
Syntax:
Example:
```
filter(cran, country=="US" | country=="IN")
```

###Task: Return values for r_version which are not NA.
```
filter(cran, !is.na(r_version))
```

##Arrange
###Task: Sort (arrange) ascending on variable ip_id.

Syntax:
Example 1: ascending
```
arrange(cran2, ip_id)
```

Example 2: descending
> Swirl note: "To do the same, but in descending order, change the second argument to desc(ip_id), where desc() stands for 'descending'."

```
arrange(cran2,desc(ip_id))
```

Example 3: Sort (arrange) using multiple variables.
"For example, arrange(cran2, package, ip_id) will first arrange by package names (ascending alphabetically), then by ip_id. This means that if there are multiple rows with the same value for package, they will be sorted by ip_id (ascending numerically)."
```
arrange(cran2, package, ip_id)
```

```
arrange(cran2, country, desc(r_version),ip_id)
```

##Mutate
Create a new variable based on the value of one or ore existing variables.

Example
> "Add a column called size_mb that contains the download size in megabytes."

```
mutate(cran3, size_mb = size / 2^20)
```
> "One very nice feature of mutate() is that you can use the value computed for your second column (size_mb) to create a third column, all in the same line of code. To see this in action, repeat the exact same command as above, except add a third argument creating a column that is named size_gb and equal to size_mb / 2^10."

```
mutate(cran3, size_mb = size / 2^20, size_gb = size_mb/2^10)

mutate(cran3, correct_size = size + 1000)
```

##Summarize
####Task: Calculate the mean value of the size variable.
summarize(cran, avg_bytes = mean(size)) will 

> "That's not particularly interesting. summarize() is most useful when working with data that has been grouped by the values
of a particular variable."



####Task: Calculate the mean value for each group in the data table.
The dplyr function summarize() calculates the requested value by group."

Example:
```
summarize(by_package, mean(size))
Source: local data frame [6,023 x 2]

       package mean(size)
1           A3   62194.96
2  ABCExtremes   22904.33
3     ABCoptim   17807.25
4        ABCp2   30473.33
5       ACCLMA   33375.53
6          ACD   99055.29
7         ACNE   96099.75
8        ACTCD  134746.27
9    ADGofTest   12262.91
10        ADM3 1077203.47
.
.
.
```

