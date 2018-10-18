# Overview of R
## Installing r-base (language) and R-Studio (environment)
* for Linux Mint (Xenial)

```bash
# Installing R language
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu xenial/'
sudo apt-get update
sudo apt-get install r-base
```
Get the R-Studio from [here](https://www.rstudio.com/products/rstudio/download/#download).

If you want to code in terminal type `R`. Now you are in a R environment ("console"). `q()` to exit R in terminal. **But use R Studio for scripting not the terminal.**

**Installing packages in R**

Packages are organized in repositories:
* [CRAN](https://cran.r-project.org),
* [Bioconductor](https://www.bioconductor.org/),
* [R-forge](http://r-forge.r-project.org/),
* [Github](https://github.com) or
* [Googlecode](https://code.google.com)

Installing a package in R and first time source location
```R
# 1 establish bioconductor as a source, first time only
source("https://bioconductor.org/biocLite.R")

# 2 install package methylKit from biocLite
biocLite("methylKit")

# CRAN
# install from CRAN
install.packages("fortunes")
```
## Basics
"file" -> "new project" -> "new directory" and save it to a location

"file" -> "new file" -> "R Script"

* The output of R is given as `[1]` in the console

**Help functions**
Get help in R [here](http://www.rseek.org/) or you use help commands:
```R
help(read.table) #help on a function
?read.table #is the same

help.search("deviation")
??deviation #is the same
```
## Variables
* Variables: any letter or word incl. numbers, `_` or `.`
* Variables are case sensitive and don't have to be declared first

```R
x <- 4+3 #adds sum of 4+3 to the variable x
y <- 9 #also shown in the environment window or with ls()
x+y
[1] 16 #result of x+y
```

## Operators
More [tutorials](https://www.tutorialspoint.com/r/r_operators.htm) about operators

```R
#Arithmetic Operators
    +, -, *, /, %%, %/%, ^
#Relational Operators, TRUE or FALSE as a result
    >, <, >=, <=, ==, !=
#Logical Operators, & = AND, | = OR
    &, | , !, &&, ||      #&& = searches if the value is somewhere in the vectors
    4<3 | 4>3             #4<3 = FALSE, 4>3 = TRUE
    [1] TRUE
#Assignment Operators
    <-, =, <<-, ->, ->>
#Miscellaneous Operators
    %in%, %*%
```

## Value/data types
```R
#Numeric – A floating point number (default data type of R):
    2.4, 9, 888
#Integer – A number with L suffix:
    2L, 25L, 0L
#Character - Any amount of text:
    "a", "school is a house", "66"
#Logical values:
    TRUE, FALSE
#Complex – defined via imaginary value i:
    2+3i, 5+8i
```
## **`class()` and `typeof()` functions**
```R
#class( ) to determine the class of the R object
#typeof( ) to determine the internal type (storage mode) of the object
    typeof(5.67)
    [1] "double"
    class(5)
    [1] "numeric"
# To create integer variable in R, you invoke as.integer() function.
    x <- as.integer(3)
    x
    [1] 3
    class(var_name)
    [1] "integer"
```
## `print()` and `paste()` functions
```R
print("I am at home") #prints its argument in R console
[1] "I am at home"
x <- 6
print(x) #prints also variables
[1] 6
#paste() combines arguments and variables
print(paste("I went there", x, "times"))
[1] "I went there 6 times"
```
## Value storage types (Vectors, Matrices, Dataframes, Lists)
* **Vectors** are one row data storages
    * all of the same class (e.g. *numeric*, *characters* like words or *logicals* like TRUE, FALSE)
    * allows arithmantics
    * access with `x[1]` or `x[c(1,2)]`
    * remove data with `x[-1]` or `x[c(-1,-2)]`
* **Matrices** data table
    * A table with values, all of the same class
    * access with `x[1,2]` `[,2]` or `x[1,c(1,2)]`
* **Dataframes** data table
    * like a matrix but can store serveral classes into it
    * access with `x[1,2]` `[,2]` or `x[1,c(1,2)]`
* **Lists** object list
    * can combine objects like vectors, matrices, dataframes and more in a single variable
    * each objects gets an identifier `[[1]]`, `[[2]]` and so on
    * access with `x[[2]][1]`: the first `[[ ]]` is for the list ID, the second `[ ]` is for the data in the list, its `[ ]` or `[,]` depending of stored vector or dataframe/matrix (see above)

___
## Vectors
* values are included with combine `c(<value1>, <value2>, <etc.>)`

```R
x <- c(2,3,5) #Vector containing three numeric values
x
[1] 2 3 5
class(x)
[1] "numeric" #numeric vector storage
```
**Vector commands**

`length()` function gives length of vector
`class()` function gives value type
`typeof()` also does this somehow

**Combining vectors**
* again with the `c()` command.

```R
x <- c(1,2,3)
y <- c("a","b") #character vector
z <- c(x,y) #new vector will contain elements of both vectors
z
[1] "1" "2" "3" "a" "b"
class(z)
[1] "character" #numeric values will be converted into character string. As a vector contains same data type
```
**Vector arithmetic's**
```R
#each member of vector will be multiplied by 5
    a <- c(1,3,5)
    5*a
    [1] 5 15 25
#Addition or multiplication will be performed element by element
    b <- c(2,4,6)
    a + b
    [1] 3 7 11
    a * b
    [1] 2 12 30
#multiplication as "power to" will be performed element by element
    a^2
    [1]1 9 25  # power 2 for each member
#if different length of vectors: then shorter will recycle - Recycling Rule.
    x <- c(2,4)
    y <- c(3,5,7,9)
    x + y
    [1] 5 9
```
**Accessing Vector data**
* The values of a vector can be retrieved `<variable>[<position>]` or removed `<variable>[-<position>]`
* Use `c()` function to retrieve more then one value
* Use `<-` to redirect the values in new variables

```R
    x <- c("a","b","c","d")
    x[3] #retrive command
    [1] c
    x[-3] # remove command
    [1] "a" "b" "d"
# retrieving more values frome one vector
    x[c(2,3)] #use the combine argument
    [1] "b" "c"
    x[c(2,3,3)]
    [1] "b" "c" "c" #Duplicate indexes
    x[c(2,1,3)]
    [1] "b" "a" "c" #out of order indexes
    x[2:4]
    [1] "b" "c" "d" #Range index with colon ":" operator
```
**Name Vector Members**
* Names can be assigned to vector members by using `names( )`

```bash
    x <- c("Christian", "Brandt")
    names(x)<- c("first_name", "last_name") #naming each value entry of vector x
    x
     first_name    last_name
    "Christian"     "Brandt"
# data retrival works also with names
    x[c("last_name","first_name")]
    last_name     first_name
    "Christian"     "Brandt"
```
**Other commands**
* such commands can all be combined like excel functions

```R
#Create a sequential vector from 10 to 20 steps by 2
    seq(from=10,to=20,by=2)
    [1] 10 12 14 16 18 20
#Sequential vector of vector_length 5 which starts from 1 and incremented by 3
    seq(from=1,by=3,length.out=5)
    [1] 1 4 7 10 13
#replication command
    rep("hello",5)
    [1] "hello" "hello" "hello" "hello" "hello"
#combine rep( ) functions
    c(rep(1,2),rep(2,3),rep(3,4))
    [1] 1 1 2 2 2 3 3 3 3
#advanced examples of vector h    
    x<- 1:10
    x
    [1] 1 2 3 4 5 6 7 8 9 10
    y <- x[x<8 & x>3]
    y
    [1] 4 5 6 7
```
___
## Matrices
* Creating a Matrix using `matrix` e. g.:
 `matrix(c(<values>),nrow= ,ncol=, byrow=TRUE/FALSE)`
* leave `nrow` or `ncol` blank e.g. only `ncol=2, byrow=TRUE` to fill the table by creating 2 columns and as many rows as needed for the data
* combining matrices with the `cbind()` e. g. `cbind(x,y)`

```R
#Create a matrix of 2 rows and 3 columns; byrow=FALSE fills columns first
    z <- matrix ( c(1,2,3,4,5,6), nrow=2, ncol=3, byrow= TRUE)
    z
    [,1] [,2] [,3]
    [1,]  1    2    3
    [2,]  4    5    6
#Accessing the data
    x[2,3]
    [1] 6 # value of 2nd row and 3th column
    x[2,]
    [1] 2 4 6 # values of 2nd row
    x[,c(1,3)]
        [,1] [,3]
    [1,]  1    5
    [2,]  2    6
```
**Naming a matrix**
* using `dimnames`
* then also retrivable by name e.g. `x["row1", c("col1","col2")]`

```R
dimnames(x)<- list( c("row1", "row2"), c("col1", "col2", "col3") )
x
     col1 col2 col3
row1    1    2    3
row2    4    5    6
```
**Show properties of matrix**
```R
class(x)
typeof(x)
nrow(x)
ncol(x)
length(x)
```
___
### Dataframe - This is the Excel of R
* **you want to use this**
* each column is a class like in SPSS or other databases)
* create a dataframe using `data.frame`
* each column is a vector datafile

```R
a<- c(1,2,3) # numeric vector
b<- c("x","y","z") # character vector
c<- c(TRUE,FALSE,TRUE) # logical vector
# creating data frame
df<- data.frame (a,b,c)
# accessing data frame
df
      a b     c # header contains vector names
    1 1 x TRUE
    2 2 y FALSE
    3 3 z TRUE
#extracting data
df["1","b"]
[1] x
```
**Adding and removing data**
* using `rbind` for rows and `cbind` for columns

```R
#creating a dataframe    
    df <- data.frame(first = c('Christian'), last = c('Brandt'), lucky_number = c(5), stringsAsFactors = FALSE)
    df
        first   last lucky_number
    Christian Brandt            5
#adding a row
    df <- rbind(df, list('Erik', 'Bongcam', 10 ))
    df
            first    last lucky_number
    1   Christian  Brandt            5
    2        Erik Bongcam           10
#adding a column
    df <-cbind(df, coffeetime =c(TRUE,TRUE))
# removal of data with
    df1<- df[-2,] # remove 2nd row
    df2<- df[,-3] # remove 3rd column
```
**Assigning names**
```R
# You can assign names to rows and columns as:
    dimnames(df)<- list ( c("row1","row2","row3"), c("col1","col2","col3") )
    df
    col1 col2  col3
    row1    1  TRUE
    row2    2 FALSE
    row3    3  TRUE
# possible ways to get data (all with the same results)
    df[[3]]
    df[["col3"]]
    df$col3
    df[,"col3"]

    [1] TRUE FALSE TRUE #result for all 4 commands
#retrieve multiple columns
    df[c("col2","col3")]
```
___
## List
* use the `list()` command to store objects in a data list
* Each object (vector, matrix etc.) gets a own ID which is `[[1]]`, `[[2]]` and so on.

```R
    my_list <- list(a,b,33) #a and b is a vector, 33 is only a value
    my_list
    [[1]]
    [1] 1 2 3

    [[2]]
    [1] "SLU" "Uppsala"

    [[3]]
    [1] 33
#Accessing data
    my_list[2]
    [[2]]
    [1] "SLU" "Uppsala"

    my_list[[2]][1]
    [1] "SLU"
#Modify the contents of list members directly with [[ ]]
    my_list[[2]][1] = "new_SLU"
```
# Advanced data manipulation, loops, merging data
## IF - ELSE, FOR, WHILE function
**IF ELSE functions**
* example:

```R
x <- 9
if (x >= 10)
{
print("x is greater than or equal to 10")
}
else
{
print("x is less than 10")
}
```
**Loop functions**
* `for(i in <variable>)`. Thats the counter. A interactive tutorial on loops can be found [here](https://www.datacamp.com/community/tutorials/tutorial-on-loops-in-r#gs.7B0m9Qk) (includes coding, while functions).

```R
for(i in 1:10) {print(i)} #output goes from 1 to 10
# or do stuff like this:
m<- c(1:10) #a vector from 1 to 10
for(i in m){print(paste("5*",i,"=",5*i))}
#results is 5*3=15 for one of the 10 rows
```
### Dealing with Working dir
* `getwd()` to get your current working dir
* `setwd("/Users/naat0001/Desktop/")` to specify were your workdir should be now
* You can do this also in rStudio using the GUI

Advanced folder creation with IF loop:
```R
mainDir <- "/Users/naat0001/Desktop"
subDir <- "outputDirectory"
if (file.exists(subDir)) #TRUE or FALSE
{
setwd(file.path(mainDir, subDir))
}
else
{
dir.create(file.path(mainDir, subDir)) #creates dir
setwd(file.path(mainDir, subDir))
}
# file.path(mainDir, subDir) says basically
# /Users/naat0001/Desktop/outputDirectory   
```
### File import and download
* Use delimiter files with `tab` `;` ` ` `,` like *.csv
* Use `read.table` `with sep=""`(enter seperator here) and `header=TRUE`

```R
x <- read.table("path_to_file/file_name.csv", sep=",", header = TRUE)
# read.table needs sep="" and header =TRUE
```
**Download and read a file**
`download.file` does store the file in your working dir with `destfile=`
```R
download.file("https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/gh-pages/_episodes_rmd/data/gapminder-FiveYearData.csv", destfile = "gapminder-FiveYearData.csv")
# read the file to a varible
x <- read.csv("gapminder-FiveYearData.csv")
# you can also directly read the online file to a variable
x <- read.table("https://raw.githubusercontent.com/swcarpentry/r-novice-gapminder/gh-pages/_episodes_rmd/data/gapminder-FiveYearData.csv", sep=",", header = TRUE)
```
**Useful commands**
```R
#x is a dataframe
str(x) # tells you what x is, e.g. a dataframe and what class in each column is stored (e.g. numerical)
head(x) #To view the first few rows of the data
tail(x) #To view the last few rows of the data
summary(x) #shows for each coloum various stuff like minimum, lower quartile, median, mean, upper quartile...
nrow(x) #number of rows
#nrow example with build in functions:
nrow(x[x$BPressure=="high" | x$BPressure=="pre-high",]) #counts rows in colum BPressure with the value high oder pre-high
#like SVERWEIS if value in BCHolesterol >190 show value of column Name in the same row  
x[x$BCholesterol>190,"Name"]
```
**Export data frames into a file**
* You do this with the `write.table()` function
* If you don’t have any row names set up for your data frame use this:

```R
write.table(data,file="out.txt",sep="\t",quote=FALSE,row.names=FALSE)
```
* If you do have row names then use the command below which will keep the row names and will move the column names so they line up correctly with the data.

```R
write.table(data,file="out.txt",sep="\t",quote=FALSE,col.names=NA)`
```
## Merging two files
* using the `merge` command
* you tell them where the ID is (like Excel SVERWEIS)

```R
# lets say i have 2 dataframes:
df1 <- data.frame(Chr=paste0('chr',1:9), Gene=paste0('gene',19:11))
df2 <- data.frame(Chr=paste0('chr',2:10),Position=paste0('pos',22:30))
# they look like this, shortend it with (...)
> df1
   Chr   Gene
1 chr1 gene19
2 chr2 gene18
3 chr3 gene17
4 chr4 gene16
(...)
> df2
    Chr Position
1  chr2    pos22
2  chr3    pos23
3  chr4    pos24
4  chr5    pos25
(...)
# you need two uniq column names, one in each dataframe
# we merge them based on the uniq "Chr" column (our "ID")
df3 <- merge(df1, df2, by="Chr")
# it only works if the column names are the same
# so use this to name them (changes the name of df2 column 1 to "Chr_ID":
colnames(df2)[1] <- "Chr_ID"
> df3
 Chr   Gene Position
1 chr2 gene18    pos22
2 chr3 gene17    pos23
3 chr4 gene16    pos24
4 chr5 gene15    pos25
(...)

# alternatively say which columns should be used for the merging:
df3 <- merge(df1, df2, by.x="Chr", by.y="Chr_ID")
```

## Graphs in R
* Overview of some basic plots can be found [here](https://www.statmethods.net/graphs/index.html).
* Advanced graphs can be found [here](https://www.statmethods.net/advgraphs/index.html).
* This example is based on the hist plot:

```R
x <- rnorm(50) # 50 random values from normal distribution.
hist(x) # plot histogram of values of x
hist(x,main="title of histogram") # change title
hist(x, main = "Title of histogram", col = "orange") # with color bars

# How to save / export graphs from console in pdf format.
pdf(file="my_histogram.pdf")
hist(x, main = "Title of histogram", col = "orange")
dev.off()
```
* [An introduction to R](https://cran.r-project.org/doc/manuals/r-release/R-intro.html)
* [R for data science](http://r4ds.had.co.nz)
