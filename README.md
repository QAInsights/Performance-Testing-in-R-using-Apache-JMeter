# Performance-Testing-in-R-using-Apache-JMeter

Detailed instructions with screenshots are [published here](https://qainsights.com/performance-testing-in-r-using-apache-jmeter/)

# Prerequisites
* Download R 3.6.1 for Windows and validate the checksum
* Download R Tools for Windows
* Download Visual Studio Code
* Download Pandoc for Windows
* Download Apache JMeter (latest version would be preferable), this demo uses JMeter 5.2

# Using RGui
After successful installation of R, you could find RGui.exe in your system. RGui runs as a standard Windows GUI executable and provides an R console in its own window.

Launch RGui where you can see the R Console as shown below.

RGui - Performance Testing in R using Apache JMeter
RGui – Performance Testing in R using Apache JMeter
Just type print("Hello World") in the console and hit enter to see the response.

Hello World in R
Hello World in R
Now, let’s configure JMeter to run a simple load test in R. Either you can setup the Environment Variable for JMeter or you can setup during the runtime.

Copy the below R snippet and save it as LoadTestDemo.r in your favorite folder.

```
# Set Environment Variable
Sys.setenv("LOADTEST_JMETER_PATH"="C:\apache-jmeter-5.2\bin\jmeter.bat")
# Install packages
install.packages("remotes")
install.packages("devtools")
install.packages("tidyr")
install.packages("rmarkdown")
remotes::install_github("tmobile/loadtest")

# Load loadtest library
library(loadtest)

# Run loadtest
results <- loadtest(url = "https://example.com", method = "GET", threads = 1, loops = 10)

# Print Results
print(results)

# Plot Results
plot_requests_per_second(results)

# Generate HTML Report
loadtest_report(results,"C:\apache-jmeter-5.2\bin\LoadTestReport.html")
Above code will simulate 1 thread and loops 10 times to hit https://example.com, print the results, plot the graph and generates the HTML report.
```

In RGui, go to File > Open Script and open the LoadTestDemo.r file. R script will open in the R Editor.

Press Ctrl + A to select all the lines and then Press Ctrl + R shortcut key to run the script.

When you run, it will prompt you to select the CRAN mirrors as shown below.

CRAN Mirrors
CRAN Mirrors
Either select the nearest mirror or select 0-Cloud [https] as shown above and hit OK.

After hitting OK, R will download the packages sequentially and executes the load test, print the results, plots the graph, and generates the HTML output.

This is the graph R plotted for the results.

R Graphs
R Graphs
Output has been created at C:\apache-jmeter-5.2\bin\LoadTestReport.html

# How it works?
If you observe the log in R console, after downloading the packages, loadtest package will generate a temporary JMeter script.

It will convert your one line of code into a JMeter script and executes it typically.

> results <- loadtest(url = "https://example.com", method = "GET", threads = 1, loops = 10)
Creating summariser <summary>
Created the tree successfully using C:\Users\Public\Documents\Wondershare\CreatorTemp\Rtmp0A1SAM\file6f5472ca5612.jmx
Starting standalone test @ Sun Nov 10 14:22:36 EST 2019 (1573413756427)
Waiting for possible Shutdown/StopTestNow/HeapDump/ThreadDump message on port 4445
summary =     10 in 00:00:01 =    7.7/s Avg:    88 Min:    29 Max:   572 Err:     0 (0.00%)
Tidying up ...    @ Sun Nov 10 14:22:40 EST 2019 (1573413760446)
... end of run
After successful run, the JMX file will gets deleted automatically.

Please check my GitHub repository for the sample code and the HTML output.

# Using VS Code
I am fond of VS Code and uses daily even for note-taking. VS Code is a very powerful editor and IDE; its very famous among the developers. VS Code Marketplace has a extension for R.

After successful installation of R extension, you need to set the r.rterm.windows to your R.exe Path like "C:\\Program Files\\R\\R-3.3.4\\bin\\x64\\R.exe" in the Preferences.

Open the LoadTestDemo.r in VS Code and hit Ctrl + Shift + P and attach the R console as shown below. This will create a new R terminal in VS Code.

After attaching it, you can run the R script using the shortcut Ctrl + Shift + P again and select the Run Source option.

It will prompt you to select the mirror for CRAN package. After selecting a location, it will execute the script sequentially and generates an output.

# Pros
Developers who are extensively working on R environment wants to load test their APIs can perform without leaving the IDE. loadtest package supports advance operations and developer can customize the graphs.

```
plot_elapsed_times(results)
plot_elapsed_times_histogram(results)
plot_requests_by_thread(results)
plot_requests_per_second(results)
```
When it comes to CI/CD pipeline, you can seamlessly integrate the R script to run the tests quicker.

# Conclusion
loadtest enables developers/testers who are working extensively on R. Under the hood, it uses Apache JMeter to create a script and execution. By default, the results has T-Mobile branded colors.

If you are working on R, definitely you could try this package and suggest the team for the new features and improvements in the GitHub issues.

# Donate
<a target="_blank" href="https://www.buymeacoffee.com/qainsights"><img src="https://cdn.buymeacoffee.com/buttons/bmc-new-btn-logo.svg" alt="Buy me a tea"><span style="margin-left:15px;font-size:28px !important;">Buy me a tea</span></a>
