# base R vs. tidyverse vs. data.table vs. sqldf

This is a short translator between the four common ways to do basic data preparation queries in R, illustrated with data set `iris` with 150 observations of four numeric columns and one factor `Species`.

|Task   | base R  | tidyverse  |  data.table | sqldf  | Comments |
|-|-|-|-|-|-|

|select rows|`iris[cond, ]` or `subset(iris, cond)`|`filter(iris, cond)`|`iris[cond]`|`sqldf("select * from iris where cond)`||
|   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |
