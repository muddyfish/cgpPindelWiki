# Execution

There are 2 ways to execute cgpPindel processing depending on the systems available to you.

1. [Single host execution](#Single-host-execution).
2. [Farm style execution](#Farm-style-execution).

Option (2) provides more control over the individual steps in the process flow but for most low usage users the single host execution method should be suitable.

## Single host execution

Single host execution does require a host with a decent level of resources.  Minimum specification:

* CPU - 2 (recommend 4)
    * 1,2 can result in very long run times
    * 6 is possible but high memory usage
    * must be divisible by 2 (other than 1)
* GB RAM - 8GB per Core used
    * Some data will require much higher values

These are based on processing of human WGS data sets of 30X coverage.

Please note, the code will recover from any failure point and not re-do anything
un-necessarily.  You can freely modify `-cpus` on each re-start if you are hitting memory limits.

Please see the command line help for the most up to date options.


## Farm style execution

# Overview of workflow

![](wiki/images/cgpPindelDetailed.png)
