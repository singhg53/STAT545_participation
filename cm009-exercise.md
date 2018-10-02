cm009 Exercises: tidy data
================
Gurjot Singh
02/10/2018

``` r
suppressPackageStartupMessages(library(tidyverse))
```

## Reading and Writing Data: Exercises

getwd() get the working directory setwd("") sets the working directory

Each row in a csv has a line and each comma seperates the new columns

Make a tibble of letters, their order in the alphabet, and then a
pasting of the two columns together.

``` r
tibble(let = letters,
       num = 1:length(letters),
       com = paste(let, num)) #letters store all lowercase letters
```

    ## # A tibble: 26 x 3
    ##    let     num com  
    ##    <chr> <int> <chr>
    ##  1 a         1 a 1  
    ##  2 b         2 b 2  
    ##  3 c         3 c 3  
    ##  4 d         4 d 4  
    ##  5 e         5 e 5  
    ##  6 f         6 f 6  
    ##  7 g         7 g 7  
    ##  8 h         8 h 8  
    ##  9 i         9 i 9  
    ## 10 j        10 j 10 
    ## # ... with 16 more rows

``` r
#num has a list of functions
#using paste0 it puts no space in-between
```

Make a tibble of three names and commute times.

``` r
tribble(
  ~name, ~time, #this tells the variable names in the data function
  "Frank", 30,
  "Lisa", 15,
  "Fred", 40
)
```

    ## # A tibble: 3 x 2
    ##   name   time
    ##   <chr> <dbl>
    ## 1 Frank    30
    ## 2 Lisa     15
    ## 3 Fred     40

Write the `iris` data frame as a
`csv`.

``` r
write_csv(iris, "iris.csv") #first argument is the data frame, this prints it out in the working directory
```

Write the `iris` data frame to a file delimited by a dollar sign.

``` r
write_delim(iris, "iris.txt", delim = "$")
```

Read the dollar-delimited `iris` data to a
tibble.

``` r
read_delim("iris.txt", delim = "$") #delim means how the columns are deliminited
```

    ## Parsed with column specification:
    ## cols(
    ##   Sepal.Length = col_double(),
    ##   Sepal.Width = col_double(),
    ##   Petal.Length = col_double(),
    ##   Petal.Width = col_double(),
    ##   Species = col_character()
    ## )

    ## # A tibble: 150 x 5
    ##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ##           <dbl>       <dbl>        <dbl>       <dbl> <chr>  
    ##  1          5.1         3.5          1.4         0.2 setosa 
    ##  2          4.9         3            1.4         0.2 setosa 
    ##  3          4.7         3.2          1.3         0.2 setosa 
    ##  4          4.6         3.1          1.5         0.2 setosa 
    ##  5          5           3.6          1.4         0.2 setosa 
    ##  6          5.4         3.9          1.7         0.4 setosa 
    ##  7          4.6         3.4          1.4         0.3 setosa 
    ##  8          5           3.4          1.5         0.2 setosa 
    ##  9          4.4         2.9          1.4         0.2 setosa 
    ## 10          4.9         3.1          1.5         0.1 setosa 
    ## # ... with 140 more rows

Read these three LOTR csv’s, saving them to `lotr1`, `lotr2`, and
`lotr3`:

  - <https://raw.githubusercontent.com/jennybc/lotr-tidy/master/data/The_Fellowship_Of_The_Ring.csv>
  - <https://raw.githubusercontent.com/jennybc/lotr-tidy/master/data/The_Two_Towers.csv>
  - <https://github.com/jennybc/lotr-tidy/blob/master/data/The_Return_Of_The_King.csv>

<!-- end list -->

``` r
lotr1 <- read_csv("https://raw.githubusercontent.com/jennybc/lotr-tidy/master/data/The_Fellowship_Of_The_Ring.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Film = col_character(),
    ##   Race = col_character(),
    ##   Female = col_integer(),
    ##   Male = col_integer()
    ## )

``` r
lotr2 <- read_csv("https://raw.githubusercontent.com/jennybc/lotr-tidy/master/data/The_Two_Towers.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Film = col_character(),
    ##   Race = col_character(),
    ##   Female = col_integer(),
    ##   Male = col_integer()
    ## )

``` r
lotr3 <- read_csv("https://github.com/jennybc/lotr-tidy/blob/master/data/The_Return_Of_The_King.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `<!DOCTYPE html>` = col_character()
    ## )

    ## Warning in rbind(names(probs), probs_f): number of columns of result is not
    ## a multiple of vector length (arg 1)

    ## Warning: 26 parsing failures.
    ## row # A tibble: 5 x 5 col     row col   expected  actual   file                                      expected   <int> <chr> <chr>     <chr>    <chr>                                     actual 1    46 <NA>  1 columns 4 colum… 'https://github.com/jennybc/lotr-tidy/bl… file 2    73 <NA>  1 columns 3 colum… 'https://github.com/jennybc/lotr-tidy/bl… row 3    81 <NA>  1 columns 3 colum… 'https://github.com/jennybc/lotr-tidy/bl… col 4    85 <NA>  1 columns 3 colum… 'https://github.com/jennybc/lotr-tidy/bl… expected 5    89 <NA>  1 columns 3 colum… 'https://github.com/jennybc/lotr-tidy/bl…
    ## ... ................. ... .......................................................................... ........ .......................................................................... ...... .......................................................................... .... .......................................................................... ... .......................................................................... ... .......................................................................... ........ ..........................................................................
    ## See problems(...) for more details.

## `gather()`

(Exercises largely based off of Jenny Bryan’s [gather
tutorial](https://github.com/jennybc/lotr-tidy/blob/master/02-gather.md))

This function is useful for making untidy data tidy (so that computers
can more easily crunch the numbers).

1.  Combine the three LOTR untidy tables (`lotr1`, `lotr2`, `lotr3`) to
    a single untidy table by stacking them.

2.  Convert to tidy. Also try this by specifying columns as a range, and
    with the `contains()` function.

3.  Try again (bind and tidy the three untidy data frames), but without
    knowing how many tables there are originally.
    
      - The additional work here does not require any additional tools
        from the tidyverse, but instead uses a `do.call` from base R – a
        useful tool in data analysis when the number of “items” is
        variable/unknown, or quite large.

## `spread()`

(Exercises largely based off of Jenny Bryan’s [spread
tutorial](https://github.com/jennybc/lotr-tidy/blob/master/03-spread.md))

This function is useful for making tidy data untidy (to be more pleasing
to the eye).

Read in the tidy LOTR data (despite having just made
it):

``` r
lotr_tidy <- read_csv("https://raw.githubusercontent.com/jennybc/lotr-tidy/master/data/lotr_tidy.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Film = col_character(),
    ##   Race = col_character(),
    ##   Gender = col_character(),
    ##   Words = col_integer()
    ## )

Get word counts across “Race”. Then try “Gender”.

Now try combining race and gender. Use `unite()` from `tidyr` instead of
`paste()`.

## Other `tidyr` goodies

Check out the Examples in the documentation to explore the following.

`expand` vs `complete` (trim vs keep everything). Together with
`nesting`. Check out the Examples in the `expand` documentation.

`separate_rows`: useful when you have a variable number of entries in a
“cell”.

`unite` and `separate`.

`uncount` (as the opposite of `dplyr::count()`)

`drop_na` and `replace_na`

`fill`

`full_seq`

## Time remaining?

Time permitting, do [this
exercise](https://github.com/jennybc/lotr-tidy/blob/master/02-gather.md#exercises)
to practice tidying data.
