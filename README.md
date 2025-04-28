# A (very) short introduction to R and the tidyverse


- [Preamble](#preamble)
- [R basics](#r-basics)
  - [Data types](#data-types)
  - [Vectors](#vectors)
  - [Variables](#variables)
  - [Functions](#functions)
  - [Relational and logical
    operators](#relational-and-logical-operators)
  - [Comments](#comments)
- [Tidyverse](#tidyverse)
  - [Pipe operator](#pipe-operator)
  - [Manipulating data](#manipulating-data)
    - [mutate()](#mutate)
    - [select()](#select)
    - [drop_na()](#drop_na)
    - [filter()](#filter)
    - [if_else() & case_when()](#if_else--case_when)
    - [distinct()](#distinct)
    - [.by and group_by()](#by-and-group_by)
    - [summarize()](#summarize)
  - [Importing data](#importing-data)
  - [Plotting data](#plotting-data)
  - [Saving a plot](#saving-a-plot)
- [Projects](#projects)
- [Exercise](#exercise)
- [To go further](#to-go-further)

# Preamble

This is a brief and condensed guide to help you grasp the
**fundamentals** of the **R language** and **tidyverse** as quickly as
possible.

After completing this module, you should be able to:

- make the distinction between the different data types
- create and use variables, vectors and functions
- use the most common logical operators
- use the basic tidyverse functions to import, transform and visualize
  your data

# R basics

Before you start, download and install **both** R ([download
R](https://cran.rstudio.com)) **and** RStudio ([download
RStudio](https://rstudio.com/products/rstudio/download/#download)) if
you haven’t yet.

I recommend working in Rstudio for its user-friendly interface and
useful additional functionalities.

Start RStudio and open a new script: `File` \>`New File` \> `R Script`.
The **R script window** is what you’ll use to **write and save** your
code. Write in your new script `"Welcome!"` and run the line using
`Ctrl`+`Enter` (Windows/Linux) or `Cmd`+`Return` (Mac). The **output**
is displayed in the **console window** (by default, it’s the bottom left
window) and should be `[1] "Welcome!"` in this case.

Just like other coding and programming languages, R is prone to syntax
errors, e.g. typos, forgetting a quote, bracket or comma. Such mistakes
will break your code and throw an error. Syntax errors are the most
common type of error, so always double-check your code whenever R is
unhappy.

## Data types

R treats all data as objects (think objects as nouns and adjectives).
The most basic type of object in R is called a vector. E.g. `24` and
`"hi"` are two different vectors of one element. Different vectors have
different data types.

There’re six data types in R. Here, we’ll focus on the four most common
ones: `character`, `logical`, `double` and `integer`.

| Value          | Type                                            | Abbrevation |
|:---------------|:------------------------------------------------|:------------|
| -1, 0, 2.38    | double                                          | dbl         |
| -1L, 0L, 2L    | integer                                         | int         |
| “Hello world!” | character (enclosed in double or single quotes) | chr         |
| TRUE, FALSE    | logical                                         | lgl         |

> \[!NOTE\] `double` and `integer` types belong to the `numeric` class
> type.

You can perform mathematical operations on `numeric` and `logical`
vectors but not on `character` vectors.

``` r
1 + 1
#> [1] 2

TRUE + TRUE
#> [1] 2

"1" + "1"
#> Error in "1" + "1": non-numeric argument to binary operator
```

## Vectors

You can combine multiple vectors into one using the `c()`
[function](#functions). In this case, the vector is a sequence of
**elements** of the **same type** (also known as atomic vector).

``` r
# double-precision vector
c(1, -2, 430.78)

# integer vector
c(1L, -2L, 430L)

# character vector
c("foo", "bar")

# logicial vector
c(TRUE, FALSE, FALSE)
```

`c()` automatically converts each element to the same type.

``` r
# despite having a single character element,
# the entire set is converted to the character type

c(1, "Hi", TRUE)
#> [1] "1"    "Hi"   "TRUE"
```

You can combine vectors of different type using `list()`:

``` r
list(1, "Hi", TRUE)
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> [1] "Hi"
#> 
#> [[3]]
#> [1] TRUE
```

## Variables

Variables provide a storage space to R objects. You can store all type
of data in a variable, including chunk of code or another variable.

To assign a name to your object, use the assignment operator `<-`
(keyboard shorcut: `Alt`+`-` (Windows & Linux) or `Option`+`-` (Mac)).

``` r
my_variable <- my_object
```

> \[!NOTE\] The name of the variable can be a combination of `letters`,
> `digits`, `.` and `_`. You’re free to give your variable the name you
> want as long as it **starts with** a `letter` or `.`. Choose a name
> that is both **concise** and **explicit**.

To print what is stored in a variable, simply call that variable.

``` r
hello_wold <- "Hello world!"
hello_wold
#> [1] "Hello world!"

x <- 2
x
#> [1] 2

y <- 4
z <- x * y
z
#> [1] 8
```

If two or more variables in the same environment have the same name, R
will return the value of the variable defined the last.

``` r
value <- 3
value <- 4
value
#> [1] 4
```

> \[!NOTE\] R is case sensitive. This means that lower and upper case
> letters are interpreted as different characters. E.g. `foo` is **not**
> the same as `Foo`.

## Functions

A function is what you use to perform an action on your objects.
Similarly to objects, which can be thought as nouns and adjectives,
functions can be thought as verbs.

A function can take one, several or no arguments at all.

``` r
# a function with multiple arguments
function_a(arg_1, arg_2)

# a function with no arguments
function_b()
```

> \[!NOTE\] Most arguments are optional. Arguments are separated by a
> `,`.

Let’s sum our previous `x` and `y` objects using the `sum()` function:

``` r
sum(x, y)
#> [1] 6
```

You can also create your own functions:

``` r
# function to convert meters to feet
m_to_feet <- function(value) {
  value / .3048
}

m_to_feet(100)
#> [1] 328.084
```

Values are assigned to argument names in the order they’re defined.
Writing the name of each argument isn’t mandatory but can lead to
unwanted results.

``` r
divide <- function(dividend, divisor) {
  dividend / divisor
}

divide(x, y)
#> [1] 0.5

divide(y, x)
#> [1] 2

divide(divisor = y, dividend = x)
#> [1] 0.5
```

It’s common to omit the first two argument names of frequently used
functions but I highly recommend writing the name of any subsequent
argument in functions that take more than two arguments.

## Relational and logical operators

You can use relational and logical operators to compare values and
perform boolean operations. These operators yield a boolean value, that
is either `TRUE` or `FALSE`.

Below is a list of R’s most common relational and logical operators.

| Operator | Description                                     |
|:---------|:------------------------------------------------|
| !        | not                                             |
| ==       | equal to                                        |
| !=       | not equal to                                    |
| \<       | less than                                       |
| \>       | greater than                                    |
| \<=      | less than or equal to                           |
| \>=      | greater than or equal to                        |
| %in%     | check if one or more elements exist in a vector |
| &        | and                                             |
| \|       | or                                              |

Here’re some examples to illustrate how these operators work:

``` r
"foo" == "foo"
#> [1] TRUE

2 != 3.5
#> [1] TRUE

4 < -2
#> [1] FALSE

c("Hello", "Bye") %in% c("Hello", "world", "!")
#> [1]  TRUE FALSE

!(2 %in% c(3, 6, 2))
#> [1] FALSE

TRUE & FALSE
#> [1] FALSE

TRUE | FALSE
#> [1] TRUE
```

## Comments

You can write comments in your script to add details and information to
your code, or to stop a line from running. Comments are muted, meaning
they aren’t interpreted by the console.

To add a comment, simply insert a `#` before the statement you want to
comment.

``` r
# comments aren't interpreted by the console
x <- 3

# x <- 4

x
#> [1] 3
```

# Tidyverse

Tidyverse is a **collection** of **packages** specifically designed to
import, manipulate, transform and visualize data. All the tidyverse
packages incorporate a common phylosophy and grammar to form a
**coherent framework** that facilitates the use of the tidyverse
functions.

First off, install tidyverse. It might take a few minutes to download.

``` r
install.packages("tidyverse")
```

When the download is complete, you can load the package with the
following function:

``` r
library(tidyverse)
```

> \[!NOTE\] You must load packages every time you open or start a new
> RStudio session before being able to use them.

## Pipe operator

One of the key features of the tidyverse is the possibility to chain
functions in an effective way using the pipe operator `|>`. The pipe
operator forwards an object to a function. It reduces the number of
intermediate objects to create and prevents the nesting of functions,
consequently making the code easier to read and write.

> \[!NOTE\] `|>` was introduced in R 4.1.0. To use it, go to Tools \>
> Global Options \> Code \> Use native pipe operator. The keyboard
> shorcut for `|>` is `Ctrl`+`Shift`+`M` (Windows & Linux) or
> `Cmd`+`Shift`+`M` (Mac).

Consider the following examples:

`x |> f()` is equivalent to `f(x)`

`x |> f(y)` is equivalent to `f(x, y)`

`x |> f() |> g() |> h()` is equivalent to `h(g(f(x)))`

`|>` can be read as `then`. E.g. `x |> f() |> g()` could be translated
into words as: **take x** *then* **do something** *then* **do something
else**.

> \[!NOTE\] By default, `|>` forwards the object on the left to the
> **first argument** of the function on the right.

To pass the object to a different argument in a function, you can use
the `_` placeholder as shown in the examples\* below:

`x |> f(y, z = _)` is equivalent to `f(y, z = x)`

> \[!NOTE\] There’s an older pipe `%>%` operator imported from the
> [magrittr package](https://magrittr.tidyverse.org). Both pipes work
> differently although they behave similarly in most cases (see [\|\> vs
> %\>%](https://www.tidyverse.org/blog/2023/04/base-vs-magrittr-pipe/#-vs)
> to know more about the differences between the two).

## Manipulating data

Here, you’ll learn some key functions from the tidyverse to handle data.

Let’s make a small data set to begin with. We’ll name our data set
`trees`:

``` r
trees <- tribble(
  ~species,               ~height, ~elevation,
  "Larix decidua Mill.",  10,      2900,
  "Fagus sylvatica L.",   17,      600,
  "Fagus sylvatica L.",   29,      600,
  "Larix decidua Mill.",  7,       2900,
  "Larix decidua Mill.",  34,      2300,
  "Fagus sylvatica L.",   22,      NA,
  NA,                     9,       1100,
  "Larix decidua Mill.",  6,       NA,
  "Betula pendula Roth.", 4,       1700,
)
trees
#> # A tibble: 9 × 3
#>   species              height elevation
#>   <chr>                 <dbl>     <dbl>
#> 1 Larix decidua Mill.      10      2900
#> 2 Fagus sylvatica L.       17       600
#> 3 Fagus sylvatica L.       29       600
#> 4 Larix decidua Mill.       7      2900
#> 5 Larix decidua Mill.      34      2300
#> 6 Fagus sylvatica L.       22        NA
#> 7 <NA>                      9      1100
#> 8 Larix decidua Mill.       6        NA
#> 9 Betula pendula Roth.      4      1700
```

### mutate()

The `mutate()` function adds or transforms an existing column.

Let’s create a `location` column with the value `Alps`:

``` r
trees <- trees |> mutate(location = "Alps")
trees
#> # A tibble: 9 × 4
#>   species              height elevation location
#>   <chr>                 <dbl>     <dbl> <chr>   
#> 1 Larix decidua Mill.      10      2900 Alps    
#> 2 Fagus sylvatica L.       17       600 Alps    
#> 3 Fagus sylvatica L.       29       600 Alps    
#> 4 Larix decidua Mill.       7      2900 Alps    
#> 5 Larix decidua Mill.      34      2300 Alps    
#> 6 Fagus sylvatica L.       22        NA Alps    
#> 7 <NA>                      9      1100 Alps    
#> 8 Larix decidua Mill.       6        NA Alps    
#> 9 Betula pendula Roth.      4      1700 Alps
```

To edit an existing column, simply `mutate` the column you want to edit.

E.g. let’s convert the elevation from meter to feet using the
`m_to_feet()` function we previously made:

``` r
trees |> mutate(elevation = m_to_feet(elevation))
#> # A tibble: 9 × 4
#>   species              height elevation location
#>   <chr>                 <dbl>     <dbl> <chr>   
#> 1 Larix decidua Mill.      10     9514. Alps    
#> 2 Fagus sylvatica L.       17     1969. Alps    
#> 3 Fagus sylvatica L.       29     1969. Alps    
#> 4 Larix decidua Mill.       7     9514. Alps    
#> 5 Larix decidua Mill.      34     7546. Alps    
#> 6 Fagus sylvatica L.       22       NA  Alps    
#> 7 <NA>                      9     3609. Alps    
#> 8 Larix decidua Mill.       6       NA  Alps    
#> 9 Betula pendula Roth.      4     5577. Alps
```

> \[!NOTE\] I used the name `elevation` here to keep the example simple.
> It’s usually a good habit to add the unit to the name,
> e.g. `elevation_m` or `elevation_ft`.

You can apply this change to all the `dbl` columns at once using your
own function inside `across()`:

``` r
trees |>
  mutate(across(
    \(x) is.double(x),
    \(x) x / 0.3048
  ))
#> # A tibble: 9 × 4
#>   species              height elevation location
#>   <chr>                 <dbl>     <dbl> <chr>   
#> 1 Larix decidua Mill.    32.8     9514. Alps    
#> 2 Fagus sylvatica L.     55.8     1969. Alps    
#> 3 Fagus sylvatica L.     95.1     1969. Alps    
#> 4 Larix decidua Mill.    23.0     9514. Alps    
#> 5 Larix decidua Mill.   112.      7546. Alps    
#> 6 Fagus sylvatica L.     72.2       NA  Alps    
#> 7 <NA>                   29.5     3609. Alps    
#> 8 Larix decidua Mill.    19.7       NA  Alps    
#> 9 Betula pendula Roth.   13.1     5577. Alps
```

Here, `\(x) x / 0.3048` is an anonymous function. It’s the shorthand for
`function(x) x / 0.3048`.

The code above targets all the columns of type `dbl` and applies our
anonymous function to divide the values of the selected column by
0.3048.

You could of course use `m_to_feet()` instead:

``` r
trees |>
  mutate(across(
    \(x) is.double(x),
    m_to_feet
  ))
```

### select()

The `select()` function subsets columns by name, number and/or type.

Subsetting columns by name or number:

``` r
select(col_name1, col_name2, col_name3)

# or

select(1, 2, 3)
```

To subset columns by type, you can use [helper
functions](https://dplyr.tidyverse.org/reference/select.html):

``` r
trees |> select(\(x) is.character(x))
#> # A tibble: 9 × 2
#>   species              location
#>   <chr>                <chr>   
#> 1 Larix decidua Mill.  Alps    
#> 2 Fagus sylvatica L.   Alps    
#> 3 Fagus sylvatica L.   Alps    
#> 4 Larix decidua Mill.  Alps    
#> 5 Larix decidua Mill.  Alps    
#> 6 Fagus sylvatica L.   Alps    
#> 7 <NA>                 Alps    
#> 8 Larix decidua Mill.  Alps    
#> 9 Betula pendula Roth. Alps
```

Let’s just remove the `location` column for now:

``` r
trees <- trees |> select(-location)
trees
#> # A tibble: 9 × 3
#>   species              height elevation
#>   <chr>                 <dbl>     <dbl>
#> 1 Larix decidua Mill.      10      2900
#> 2 Fagus sylvatica L.       17       600
#> 3 Fagus sylvatica L.       29       600
#> 4 Larix decidua Mill.       7      2900
#> 5 Larix decidua Mill.      34      2300
#> 6 Fagus sylvatica L.       22        NA
#> 7 <NA>                      9      1100
#> 8 Larix decidua Mill.       6        NA
#> 9 Betula pendula Roth.      4      1700
```

### drop_na()

The `drop_na()` function removes `NA` values in the entire data frame or
specific columns.

Let’s remove the row containing `NA` in the `species` column:

``` r
trees <- trees |> drop_na(species)
trees
#> # A tibble: 8 × 3
#>   species              height elevation
#>   <chr>                 <dbl>     <dbl>
#> 1 Larix decidua Mill.      10      2900
#> 2 Fagus sylvatica L.       17       600
#> 3 Fagus sylvatica L.       29       600
#> 4 Larix decidua Mill.       7      2900
#> 5 Larix decidua Mill.      34      2300
#> 6 Fagus sylvatica L.       22        NA
#> 7 Larix decidua Mill.       6        NA
#> 8 Betula pendula Roth.      4      1700
```

### filter()

The `filter()` function subsets a data frame by keeping rows that
satisfy one or more conditions.

Let’s filter trees whose the height is higher than 25 m **or** species
is *Betula pendula*:

``` r
trees |> filter(height > 25 | species == "Betula pendula Roth.")
#> # A tibble: 3 × 3
#>   species              height elevation
#>   <chr>                 <dbl>     <dbl>
#> 1 Fagus sylvatica L.       29       600
#> 2 Larix decidua Mill.      34      2300
#> 3 Betula pendula Roth.      4      1700
```

### if_else() & case_when()

The `if_else()` and `case_when()` functions allows you to replace column
values with another based on one or more conditions.

The syntax of `if_else()` is as follows:

``` r
if_else(condition, value_if_cond_TRUE, value_if_cond_FALSE)
```

E.g. let’s replace the missing elevation of *Larix decidua*:

``` r
trees |>
  mutate(elevation = if_else(
    is.na(elevation) & species == "Larix decidua Mill.", # check if the elevation is NA and the species is Larix decidua
    2900, # replace values that satisfy the condition by 2900
    elevation # keep the original values when the condition isn't met
  ))
#> # A tibble: 8 × 3
#>   species              height elevation
#>   <chr>                 <dbl>     <dbl>
#> 1 Larix decidua Mill.      10      2900
#> 2 Fagus sylvatica L.       17       600
#> 3 Fagus sylvatica L.       29       600
#> 4 Larix decidua Mill.       7      2900
#> 5 Larix decidua Mill.      34      2300
#> 6 Fagus sylvatica L.       22        NA
#> 7 Larix decidua Mill.       6      2900
#> 8 Betula pendula Roth.      4      1700
```

Use `case_when()` if you want to test more than one condition and edit
values based on each condition. The syntax is a bit different than that
of `if_else()` but not much complicated.

``` r
case_when(
  condition1 ~ value_when_condition1_is_true,
  condition2 ~ value_when_condition2_is_true,
  conditionN ~ value_when_conditionN_is_true,
  .default = value_when_previous_conditions_are_false
)
```

Let’s expand the previous example to the two species that have no
elevation:

``` r
trees <- trees |>
  mutate(elevation = case_when(
    is.na(elevation) & species == "Larix decidua Mill." ~ 2900,
    is.na(elevation) & species == "Fagus sylvatica L." ~ 600,
    .default = elevation # to keep the original values when the conditions above aren't met
  ))
trees
#> # A tibble: 8 × 3
#>   species              height elevation
#>   <chr>                 <dbl>     <dbl>
#> 1 Larix decidua Mill.      10      2900
#> 2 Fagus sylvatica L.       17       600
#> 3 Fagus sylvatica L.       29       600
#> 4 Larix decidua Mill.       7      2900
#> 5 Larix decidua Mill.      34      2300
#> 6 Fagus sylvatica L.       22       600
#> 7 Larix decidua Mill.       6      2900
#> 8 Betula pendula Roth.      4      1700
```

### distinct()

The `distinct()` function subsets unique rows from a data frame.

Subsetting unique species:

``` r
# use `.keep_all = TRUE` to retain all the columns
trees |> distinct(species, .keep_all = TRUE)
#> # A tibble: 3 × 3
#>   species              height elevation
#>   <chr>                 <dbl>     <dbl>
#> 1 Larix decidua Mill.      10      2900
#> 2 Fagus sylvatica L.       17       600
#> 3 Betula pendula Roth.      4      1700
```

### .by and group_by()

Many tidyverse verbs used in data transformation have a `.by` parameter
that allows you to apply a operation on groups of data defined by one or
more variables.

Let’s calculate the mean height by species:

``` r
trees |>
  mutate(
    height_mean = mean(height),
    .by = species
  )
#> # A tibble: 8 × 4
#>   species              height elevation height_mean
#>   <chr>                 <dbl>     <dbl>       <dbl>
#> 1 Larix decidua Mill.      10      2900        14.2
#> 2 Fagus sylvatica L.       17       600        22.7
#> 3 Fagus sylvatica L.       29       600        22.7
#> 4 Larix decidua Mill.       7      2900        14.2
#> 5 Larix decidua Mill.      34      2300        14.2
#> 6 Fagus sylvatica L.       22       600        22.7
#> 7 Larix decidua Mill.       6      2900        14.2
#> 8 Betula pendula Roth.      4      1700         4
```

If you need to apply different operations on a same group, you can use
the `group_by()` function:

``` r
trees |>
  group_by(species) |>
  filter(elevation > 1000) |>
  mutate(height_mean = mean(height)) |>
  ungroup()
```

Always `ungroup()` after using `group_by()` to ensure that future
operations don’t apply to the grouping variables.

### summarize()

The `summarize()` (or `summarise()`) function summarizes and reduces the
dimensions of your data frame by grouping variables.

E.g. summarising the `trees` data frame by `species` and `height_mean`:

``` r
trees <- trees |>
  summarize(
    height_mean = mean(height),
    .by = c(species, elevation)
  )
```

The code above is the equivalent to:

``` r
trees |>
  mutate(
    height_mean = mean(height),
    .by = c(species, elevation)
  ) |>
  distinct(species, elevation, height_mean) |>
  arrange(species, elevation) # to sort rows by species and elevation in ascending order
```

## Importing data

You can import `.csv` files using the `read_*()` [function
family](https://readr.tidyverse.org/reference/read_delim.html). There’re
different functions for different separators.

| Function      | Value            | Decimal |
|:--------------|:-----------------|:--------|
| read_csv()    | ,                | .       |
| read_csv2()   | ;                | ,       |
| read_tsv()    | \t (tab)         | .       |
| read_delim()  | custom character | .       |
| read_table2() | space            | .       |

E.g. to read a comma-separated values file:

``` r
"path_to_file/data.csv" |> read_csv()

# or

read_csv("path_to_file/data.csv")
```

Use the more general `read_delim()` function to read cases not presented
in the table above and specify the separator in the `delim` argument.

``` r
# to read a pipe-separated values file
"path_to_file/data.csv" |> read_delim(delim = "|")
```

If the `read_csv2()` function isn’t appropriate despite having a file
that uses a `,` as decimal separator, you need to specify the decimal
separator in the `locale` argument as shown below:

``` r
# to read a tab-separated values file using comma as decimal separator
read_tsv("path_to_file/data.csv", locale = locale(decimal_mark = ","))

# or

read_delim(
  "path_to_file/data.csv",
  delim = "\t",
  locale = locale(decimal_mark = ",")
)
```

## Plotting data

You can plot graphs using the ggplot2 package (part of the tidyverse).

> \[!WARNING\] ggplot2 functions are chained using a `+` sign.

We’ll use the `penguins` dataset from the `palmerpenguins` package in
this example. It’s a dataset of size measurements for three penguins
species, namely *Pygoscelis adeliae* (adelie penguin), *Pygoscelis
papua* (gentoo penguin) and *Pygoscelis antarctica* (chinstrap penguin).

First, install and load the package:

``` r
install.packages("palmerpenguins")
library(palmerpenguins)
```

``` r
penguins
#> # A tibble: 344 × 8
#>    species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
#>    <fct>   <fct>              <dbl>         <dbl>             <int>       <int>
#>  1 Adelie  Torgersen           39.1          18.7               181        3750
#>  2 Adelie  Torgersen           39.5          17.4               186        3800
#>  3 Adelie  Torgersen           40.3          18                 195        3250
#>  4 Adelie  Torgersen           NA            NA                  NA          NA
#>  5 Adelie  Torgersen           36.7          19.3               193        3450
#>  6 Adelie  Torgersen           39.3          20.6               190        3650
#>  7 Adelie  Torgersen           38.9          17.8               181        3625
#>  8 Adelie  Torgersen           39.2          19.6               195        4675
#>  9 Adelie  Torgersen           34.1          18.1               193        3475
#> 10 Adelie  Torgersen           42            20.2               190        4250
#> # ℹ 334 more rows
#> # ℹ 2 more variables: sex <fct>, year <int>
```

Assuming you want to see if there’s a relationship between flipper
length and body mass for each species. You can visualize such
relationship by plotting the flipper length against the body mass in a
scatter plot:

``` r
penguins |>
  ggplot(aes(
    x = body_mass_g,
    y = flipper_length_mm,
    color = species # to assign a color to each group
  )) +
  geom_point() +
  labs(
    x = "Body mass (g)",
    y = "Flipper length (mm)"
  )
```

![](figures/geom-point-1.svg)

If you want to compare the bill length of each species, you can do so by
plotting a boxplot:

``` r
penguins |>
  ggplot(aes(
    x = species,
    y = bill_length_mm
  )) +
  geom_boxplot() + # to plot a boxplot
  labs(
    x = NULL, # to remove the label of the x axis
    y = "Bill length (mm)"
  )
```

![](figures/geom-boxplot-1.svg)

ggplot2 offers many plotting possibilities. You can learn more
[here](https://ggplot2-book.org).

## Saving a plot

You can save a plot by clicking on the `Export` button in the `Plots`
window (bottom right window by default).

I recommend saving your plots as `.svg` or `.pdf` if you aren’t limited
by the weight of the files. Otherwise, save plots as `.png`.

# Projects

A **RStudio project** creates a **workspace** and **working directory**
to help you organise and work with files that belong to a same project.

To create a new project, go to `File` \> `New Project…` \>
`New Directory` (or `Existing Directory` if you want to create your
project from an existing folder) \> `New Project` and choose a
`Directory name` for your project.

I recommend making a new RStudio project **everytime you start a new
research project**. As a rule of thumb, whenever you have to work with
at least two related files.

Because the working directory is relative to the project, it makes it
very easy to navigate and access any files within a project.

E.g., to get the full path of a file, simply open quotes `""` and write
the beginning of the file name or press `tab` to list all the files and
folders in a project. Filter the list by typing part of the name and
press `tab` to validate. The file is now ready to be read as
demonstrated in the importing data section.

![](figures/project-import-file.gif)

You can learn more about RStudio projects
[here](https://support.rstudio.com/hc/en-us/articles/200526207-Using-Projects).

# Exercise

Using the `penguins` dataset, look at the relationship between bill area
and body mass in adelie and chinstrap penguins.

In a new column, compute the bill area for each individual. Plot the
bill area against the body mass by species group using `ggplot()` and
`geom_point()`.

For the purpose of this exercise, we’ll make the assumption that the
bill is flat and that the length and depth don’t change along the bill.

This time, we’ll change the default settings to make the scatter plot
look prettier.

Copy-paste the code below in a new R script. Replace the
`# insert your code here` comment with your own code.

Add in the `aes()` function (inside `ggplot()`) the arguments
`shape = species` and `fill = species` to assign a shape and fill color
to your groups, respectively. Finally, add the argument `size = 3`
inside `geom_point()` to increase the size of the points.

``` r
penguins_colors <- c("#FF9671", "#00D2FC")

# insert your code here
# don't forget to link your code and the lines below with a '+'
  scale_color_manual(values = penguins_colors) + # to change the colors of the points
  scale_shape_manual(values = c(21, 23)) + # to change the shape of the points
  scale_fill_manual(values = alpha(penguins_colors, .4)) + # to add the fill colors and transparency
  labs(
    x = "Body mass (g)",
    y = expression(Bill~area~(mm^{2}))
  ) +
  theme_minimal() + # to change the general theme of the plot
  theme(
    legend.title = element_blank(), # to remove the legend title
    legend.text = element_text(size = 11), # to increase the size of the legend
    axis.text = element_text(size = 11), # to increase the size of the axis tick labels
    axis.title = element_text(size = rel(1.1)) # to increase the size of the axis title, relative to the default size (1.1 times as big as the default size here)
```

Here’s the output you’re supposed to get:

![](figures/exercise-output-1.svg)

If you ended up with the same plot, congratulations!

If not, try a bit more. Don’t hesitate to go back and read the different
sections again. All the information needed to make the plot is in this
page. If you have an error, check that you used a proper chaining sign
and didn’t forget a comma or bracket.

# To go further

Here’re some recommendations to expand your knowledge of R:

- [BioStats](https://biostats.w.uib.no)
- [Rstudio Cloud](https://rstudio.cloud/learn/primers) (video tutorials)
- [Official tidyverse website](https://www.tidyverse.org)
- [R for Data Science](https://r4ds.had.co.nz/index.html)
- [ggplot2: Elegant Graphics for Data
  Analysis](https://ggplot2-book.org)
- [Fundamentals of Data
  Visualization](https://serialmentor.com/dataviz/)

RStudio useful functionalities:

- [Code
  completion](https://support.rstudio.com/hc/en-us/articles/205273297-Code-Completion)
- [Data
  viewer](https://support.rstudio.com/hc/en-us/articles/205175388-Using-the-Data-Viewer)
- [R
  project](https://support.rstudio.com/hc/en-us/articles/200526207-Using-Projects)
