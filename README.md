# Factorial Programming
#### by Justin Papreck"
10/7/2021

## Dependencies
```{r setup, include=FALSE}
library(microbenchmark)
library(purrr)
library(magrittr)
```

## Functional Programming

1. The first function is to produce a factorial using a for-loop. I included a 'stopifnot' prior to running the loop to prevent an infinite loop from occuring in the case of entry of a negative number.  

```{r for_loop}
loopy_factorial <- function(n){
    stopifnot(n >= 0) # prevents entry of a negative number which will prevent an infinite loop
    number <- 1
    for(i in n:1){
        number <- i*number
    }
    if(number == 0){
        1
    } else {
        number
    }
}
```


2. The reduce function reduces the values of a vector to a single value, however this is driven by the input of a function. This can be exploited when calculating a factorial, as the function x*y will always multiply the next value by the result of the last value. This eliminates the need for a loop. 

```{r reduction}
reducy_factorial <- function(n){
    stopifnot(n >= 0)
    if(n == 0){
        1
    } else {
        reduce(n:1, function(x,y){
            x*y  # x*y in reduce will actually provide a factorial of length n
        })
    }
}

```


3. Using recursion is another option - by setting a logical statement to continue to evaluate a recursive function until a specific condition is met, we can apply the condition to be that (n-1) = 0 by simply apply if-else conditions. 

```{r recursion}
funky_factorial <- function(n){
    stopifnot(n >= 0)
    if(n == 0){
        1
    } else if (n == 1){
        1
    } else {
        n*funky_factorial(n-1) # this is the recursion line, multiplying n by the last multiple
    }
}
```

4. Memoization isn't a method to which the functions will be evaluated, but instead a way to speed up the process when making massive calculations. The idea is that a recursive function doesn't need to recalculate every number each time if the values are stored in a new environment. If the number exists in that environemtn, we can use it; if not the function calculates a new value and stores it. 

```{r memoization}
memoizer <- function(){    
    res <- 1
    memoizy_factorial <- function(n){
        stopifnot(n >= 0)
        if (n == 0) return(1)
        if (n == 1) return (1)
        
        if (!is.na(res[n])) return(res[n])
        
        res[n] <<- n * funky_factorial(n-1)  #replace the function to be used here
        res[n]
    }
    memoizy_factorial
} 
memoizy_factorial <- memoizer()
```


## Benchmarking
The functions were benchmarked to check efficiency of calculating factorials.
```{r benchmarks}
bm <- microbenchmark(loopy_factorial(20), reducy_factorial(20), funky_factorial(20), memoizy_factorial(20))
write.table(print(bm), file = "factorial_output.txt", sep = '  ', row.names = FALSE, col.names = TRUE)
```


