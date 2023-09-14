03 - Logging Levels
================
2023-09-08

- [Introduction](#introduction)
- [What Logging Levels Exist?](#what-logging-levels-exist)

# Introduction

When you start to use a proper logging package, you first need to
understanding what logging levels are.

Logging levels allow you to categorize the type of message you want to
communicate. When we used the `cat()` function to print out different
steps in our code, you cannot easily tell if what your message is
communicating is just informational, a warning, or perhaps even a error
message. It may be clear to you, but it also needs to be clear to anyone
else that may be interacting with your code. And writing
`cat("INFO: An informational message")` is not very efficient.

Another drawback to using only `cat()` is that maybe you want control
over what messages get printed out based on some minimum threshold. In
other words, maybe you have several informational messages in your code,
but you only really need to see that output when you’re developing, and
not when you’ve pushed your code to production. Having to delete those
messages is inconvenient in case you need them again.

Logging levels allows you to:

1.  Define the type of message
2.  Define a threshold for what messages display and when.

Let’s get into examples with log4r

``` r
suppressPackageStartupMessages(
  library(log4r)
)
# We need to create a logger object. 
# By default, logs will print to console 
# and only show messages INFO and above

logger <- logger()

log4r::debug(logger, "This is a debug message")
log4r::info(logger, "This is a informational message")
log4r::warn(logger, "This is a warning message")
log4r::error(logger, "This is an error message")
```

    ## INFO  [2023-09-13 23:21:42] This is a informational message
    ## WARN  [2023-09-13 23:21:42] This is a warning message
    ## ERROR [2023-09-13 23:21:42] This is an error message

See how in the above example, we have four different logging levels
(debug, info, warn, and error). Only three of them printed out
something. That’s because our `logger` object was left to the default
`logger()` object of `INFO`.

Let’s define it to print `DEBUG` so we can get that first `debug`
message printed out.

``` r
library(log4r)

logger <- logger(threshold = "DEBUG")

log4r::debug(logger, "This is a debug message")
log4r::info(logger, "This is a informational message")
log4r::warn(logger, "This is a warning message")
log4r::error(logger, "This is an error message")
```

    ## DEBUG [2023-09-13 23:21:42] This is a debug message
    ## INFO  [2023-09-13 23:21:42] This is a informational message
    ## WARN  [2023-09-13 23:21:42] This is a warning message
    ## ERROR [2023-09-13 23:21:42] This is an error message

# What Logging Levels Exist?

We’ve gotten into a few logging levels, but there are a few more to be
aware of. And what do they mean anyways?

A great resource to start off with is actually from the [python logging
package.](https://docs.python.org/3/library/logging.html#logging-levels).

| Level   | Description                                                                                                                                      | Example                                                                                                                                                                            |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DEBUG   | Messages that are helpful in providing an exceptional level of detail that you normally would not need to see, especially for debugging purposes | Printing out the output of something mid-way through a series of processing steps, just to check that things look right.                                                           |
| INFO    | Describes steps or processes happening in your code                                                                                              | If you are connecting to a database, you may have a message saying “Connecting to external database”                                                                               |
| WARNING | Outputs a message that is recoverable and does not affect the final output of your code                                                          | Maybe a step involves changing the data type (numeric to character, for example), assuming it’s okay to change it. It shouldn’t cause problems, but you should let your user know. |
| ERROR   | Something bad happened, but there is an alternative solution and is recoverable.                                                                 | You attempt to connect to a database, but it failed, so it’s using a different backup database instead.                                                                            |
| FATAL   | Something bad happened, and it is not recoverable. The program must quit.                                                                        | You cannot connect to any databases, and it’s required for your program to run, and there are no alternatives.                                                                     |
