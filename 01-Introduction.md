# 01 - Introduction to Logging

2023-09-08

-   [What is Logging?](#what-is-logging)
-   [Why Log?](#why-log)
    -   [Debugging and Troubleshooting](#debugging-and-troubleshooting)
    -   [Performance Analysis](#performance-analysis)
    -   [Monitoring](#monitoring)
-   [Wrap Up](#wrap-up)

# What is Logging? {#what-is-logging}

Think of logging as information that is generated, and possibly stored, as a result of your code running.

This can be simple `print()` or `cat()` functions throughout your code to help indicate what sections are running. For example:

``` r
print("Starting to load data")

# Load data

print("Cleaning and Prepping data")

# Clean data

print("Create final df for analysis")

# Final DF 
```

Logging can also involve writing information to a database. Maybe you want to track all users who are visiting your Shiny app, or the inputs and outputs of your API. This is transactional logging information that can help you identify the on-going activity of your code and overall health.

# Why Log? {#why-log}

For most data professionals (analysts, scientists, and the like), using logging in code is not something that is often considered. If something breaks, why not just hunt down for the line of code that is causing the issue, and then move on with life?

This troubleshooting approach can potentially eat up time away from actually doing analytically work -- I've certainly been down that road! I would argue that in most cases, it makes sense to spend a bit of extra time implementing logging into your code so that you not only have a better way to troubleshoot and debug your code, but it enables you to monitor the performance of your code, and review what is happening underneath the hood when you run into unexpected errors and unusual behavior.

The reasons below provide use cases for logging. The reasons are not fully encompassing, ad the examples are meant to simple. In the upcoming sections, we'll dive into better approaches for logging (e.g., using something other than `print()` or `cat()` functions). However, I imagine that for many who have some logging implemented, this is the approach you're using today!

## Debugging and Troubleshooting {#debugging-and-troubleshooting}

One reason to use logging is that it helps with the debugging process. They act as trail markers for when things go wrong, so you have some general idea of what code ran successfully, and where to start troubleshooting code. This is especially true if your code is running in an non-interactive way, where you don't have an IDE to set breakpoints in your code.

For example, you can have a very simple `cat()` statement while running a for loop:

``` r
number_list <- c(2, 3, 6, 0, 3, 2)
for (i in number_list) {
  result <- 5/i
  
  cat("Result: ", result, "\n")
}
```

```         
## Result:  2.5 
## Result:  1.666667 
## Result:  0.8333333 
## Result:  Inf 
## Result:  1.666667 
## Result:  2.5
```

What happened here? What caused the `Inf` value? Let's add another `cat()` outputting what the value of `i` is for each iteration.

``` r
number_list <- c(2, 3, 6, 0, 3, 2)
for (i in number_list) {
  cat("Dividing 5 by", i, "\n")
  
  result <- 5/i
  
  cat("Result: ", result, "\n")
}
```

```         
## Dividing 5 by 2 
## Result:  2.5 
## Dividing 5 by 3 
## Result:  1.666667 
## Dividing 5 by 6 
## Result:  0.8333333 
## Dividing 5 by 0 
## Result:  Inf 
## Dividing 5 by 3 
## Result:  1.666667 
## Dividing 5 by 2 
## Result:  2.5
```

Imagine if `number_list` was a random list of numbers. Having `cat` print out each value of `i` as the for loop ran is helpful to understand the result of `Inf`: because it was attempting to divide by zero.

## Performance Analysis {#performance-analysis}

It can also be useful to know and document the time it takes to run an analysis, especially if you have multiple steps involved and you want to monitor the time it takes to run Chunk A, Chunk B, Chunk C, etc.

There's a few useful packages that help with this ([{tictoc}](https://github.com/collectivemedia/tictoc), [{microbenchmark}](https://github.com/olafmersmann/microbenchmark)), but you can also use `Sys.time`.

``` r
# start timer
# Assume there's a lot of steps involving data processing for a large file
# and you know the data size will grow over time. Perhaps it's something you want
# to track. 

start_time <- Sys.time()

# 1. load data

# 2. clean and process

# 3. output final dataframe

# adding sleep for purpose of example
Sys.sleep(5)

end_time <- Sys.time()

time_elapsed <- end_time - start_time

cat("Total time elapsed: ", time_elapsed, "seconds")
```

```         
## Total time elapsed:  5.007979 seconds
```

## Monitoring {#monitoring}

Finally, logging is certainly useful in informing you the route being taken in your code. It's especially helpful if you have `if` and `else` statements, or error handling with `try` and `tryCatch`.

With this example below, perhaps you only want data to be processed if it meets certain conditions. It would be nice to know you have some way of checking that, instead of relying on code blindly making the decision for you. Sometimes the code you write does not behave as intended!

``` r
# generate 5 random numbers
# Check if number if even or odd

rand_int <- sample(1:100, 5)

for (i in rand_int) {
  print(paste0("The numerator value is ", i))
  modulo_result <- i%%2
  if (modulo_result == 0) {
    print("Divided an even number") 
    # proceed to do some additional steps here
    
  } else 
    print("Divided by an odd number")
    # don't do anything else
}
```

```         
## [1] "The numerator value is 60"
## [1] "Divided an even number"
## [1] "The numerator value is 15"
## [1] "Divided by an odd number"
## [1] "The numerator value is 44"
## [1] "Divided an even number"
## [1] "The numerator value is 76"
## [1] "Divided an even number"
## [1] "The numerator value is 18"
## [1] "Divided an even number"
```

Let's do a `tryCatch` example as another illustration for logging.

1.  We'll take our variable `i`, and check if it's even
    1.  If it's even, then we'll try to do `log(i)`
2.  If `i` is odd, then don't do anything else.

We should expect that when `i` is non-positive, there will be a warning, which should trigger additional messages.

``` r
log(-1)
```

```         
## Warning in log(-1): NaNs produced

## [1] NaN
```

Let's run a full example.

``` r
rand_int <- sample(-2:1, 3)

for (i in rand_int) {
  print(paste0("Starting loop. The value of i is ", i))
  modulo_result <- i%%2
  if (modulo_result == 0) {
    
    print("Variable i is an EVEN number") 
    # proceed to do some additional steps here
    tryCatch(
      #let's try to take the log  
      print(paste("The log of i is: ", log(i)))
      , warning = function(w) {
        
        #print error message
        print(paste0("Uh oh. Something went wrong. Is i a negative number?:", w))
        
        #do something else instead if warning is triggered
      }
    
    )
  } else 
    print("Variable i is an ODD number")
    # don't do anything else
}
```

```         
## [1] "Starting loop. The value of i is -1"
## [1] "Variable i is an ODD number"
## [1] "Starting loop. The value of i is 1"
## [1] "Variable i is an ODD number"
## [1] "Starting loop. The value of i is -2"
## [1] "Variable i is an EVEN number"
## [1] "Uh oh. Something went wrong. Is i a negative number?:simpleWarning in log(i): NaNs produced\n"
```

Now we're starting to see the path the code is taking as the value of `i` changes for each iteration.

If we wanted to, we can also check for `Inf` values produced through `log(0)`, which is a different result when you attempt to log a non-zero, non-positive value.

# Wrap Up {#wrap-up}

With some very basic `print()` or `cat()` statements in your code, you can get a bit more insight into what's happening in your code, and provide some guidance into understanding why something may not be working as you expect it to.

In the next section, we'll move away from using `print()` or `cat()` and introduce a few logging packages in R that I have personally found useful.
