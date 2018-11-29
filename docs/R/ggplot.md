ggplot2
===

# Basic heatmap

````R
source("https://bioconductor.org/biocLite.R")
biocLite("ggplot2") # install ggplot2
library(ggplot2) # load library

# get data into R
setwd("D:/UKJ_Project_Folder/KPC_Letter/mesh")
df.datafile <- read.table("results_all.tsv", sep="", header = FALSE)

# get you column names with
head(df.datafile)

# say which column for what (V1 to V3 here)
gg <- ggplot(data = df.datafile, aes(x = V2, y = V1)) + geom_tile(aes(fill = V3))
# to add more heatmap modification to it do "gg <- gg + (...)"

# plotting the output
gg
````
