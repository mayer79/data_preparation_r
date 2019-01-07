# base R vs. dplyr vs. data.table vs. sqldf

This is a short translator between the four common ways to do basic data preparation queries in R.

- **base R**: a very good starting point; easy to program with but not always easy to read. A must for R enthusiasts.

- **dplyr/tidyverse**: very easy to write and read since each function cares about exactly one single task; 100% compatible with the chaining approach of `magrittr`. Difficult to understand the internals though.

- **data.table**: extremely fast and memory efficient, so *the* approach for large data; longer queries are not always easy to read.

- **sqldf**: very easy to read, even for people who have never used R before; great to learn SQL on the fly; compared to the other approaches, it is quite slow, so suboptimal for large dage.

Illustrated with data set `iris` with 150 observations of four numeric columns and one factor `Species`.

|Task   | base R  | dplyr/tidyverse  |  data.table | sqldf  | 
|-|-|-|-|-|
|**library**||`dplyr`, `tidyverse`|`data.table`|`sqldf`|
|**view some rows**   |`head(iris)`   | `iris` | `iris` | `sqldf("select * from iris limit 6")`  |
|**select rows**|`iris[cond, ]` or `subset(iris, cond)`|`filter(iris, cond)`|`iris[cond]`|`sqldf("select * from iris where cond)`|
|**sort rows** | `iris[order(cols), ]`  |  `arrange(iris, cols)` | `iris[order(cols)]`  | `sqldf("select * from iris order by cols")`  |
|**select columns**   | `iris[, cols]` or `subset(iris, select = cols)` | `select(iris, cols)`  | `iris[, cols]`  |  `sqldf("select cols from iris")` |
|**remove column** | `iris$Species <- NULL`  | `mutate(iris, Species = NULL)` |  `iris[, Species := NULL]` | `sqldf("select other cols from iris)`  |
|**add column** | `iris$x <- iris$Sepal.Length^2` or `transform(iris, x = Sepal.Length^2)` | `mutate(iris, x = Sepal.Length^2)` |  `iris[, x := Sepal.Length^2)` | `sqldf("select *, power([Sepal.Length], 2) as x from iris")`  |
|**grouped stats**| `aggregate(Sepal.Width ~ Species, data = iris, FUN = median)` | `iris %>% group_by(Species) %>% summarize(med = median(Sepal.Width))` | `iris[, .(med = median(Sepal.Width)), by = Species]` | `sqldf("select Species, median([Sepal.Width]) as med from iris group by Species")` |
|**left join**|`merge(iris, grouped_stats, by = "Species", all.x = TRUE)`|`left_join(iris, grouped_stats, by = "Species)`| `grouped_stats[iris, on = "Species")` or like `merge`| `sqldf("select a.*, b.med from iris a left join grouped_stats b on a.Species = b.Species")`|
|**inner join**|`merge(iris, grouped_stats, by = "Species")`|`inner_join(iris, grouped_stats, by = "Species)`| `grouped_stats[iris, on = "Species", nomatch = 0)` or like `merge`| `sqldf("select a.*, b.med from iris a inner join grouped_stats b on a.Species = b.Species")`|
|**add grouped stats**|`transform(iris, med = ave(Sepal.Width, Species, FUN = median))`|`iris %>% group_by(Species) %>% mutate(med = median(Sepal.Width))` |`iris[, med := median(Sepal.Width), by = Species]`|group by and left join|
|**transpose to long**|`reshape(???, direction = "long")`|`gather`|`melt`|through "union all"|
|**transpose to wide**|`reshape(???, direction = "wide")`|`spread`|`dcast`|through "left joins"|

This is version 1 that will hopefully gets updated from time to time ;).
