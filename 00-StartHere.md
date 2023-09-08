00 - Start Here
================
2023-09-08

- [Purpose](#purpose)
- [Section Overview](#section-overview)

# Purpose

I wrote this document to provide in-depth information for a five-minute
lighting talk I gave at the Posit::Conf(2023) conference in Chicago, IL
(September 19, 2023).

With only five minutes to talk about logging, there is so much more
information I wanted to share that I just could not get to. I hope
others find this information helpful. This is knowledge I have gained
through trial and error, and finding a solution that worked well for me.
Maybe this information does not apply to you. And if you find the
information not helpful or incorrect, then please let me know!

# Section Overview

I have split up my content into separate sections for easier reading.
All documents were written in R Markdown documents and were meant to
display easily in Github for ease of access.

1.  01 - Introduction to Logging: I discuss what logging is, why it can
    be useful, and some examples that demonstrate its purpose
2.  02 - Logging Packages: I review a few logging packages I have found
    helpful. It is not meant to review all types of logging packages
    available.
3.  03 - Logging Levels: I dive into different logging levels, such as
    INFO, WARN, and ERROR, and how to strategically use them. I also
    discuss why you should not log everything, and how you need to be
    strategic in what you log.
4.  04 - Making Logs Accessible: I discuss how you can review your logs,
    including through cloud-based solutions. I focus on Azure.
5.  05 - Configure Sending Syslogs to Azure Monitor: This extends on the
    previous topic, along with instructions on setting up a Linux VM for
    pushing syslogs to Azure Monitor
6.  06 - Using API to Send Syslogs to Azure Monitor: This is an
    alternative approach to writing logs to syslog. If you’re using
    Docker to run your R code, then this may be a better solution.
7.  07 - Additional Resources