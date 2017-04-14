-   [Documentation](#documentation)
-   [1, Introduction](#introduction)
-   [2, Data Exploration](#data-exploration)
-   [3, Data Manipulation](#data-manipulation)
-   [4, Data Analysis](#data-analysis)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets only.

------------------------------------------------------------------------

Documentation
-------------

-   Microsoft R Server: previously called Revolution R Enterprise for Hadoop, Linux and Teradata and included new Microsoft enterprise support and purchasing options. Microsoft R Server was further made available to students through the Microsoft DreamSpark programme.
-   Microsoft R Server Developer Edition: a gratis version for developers that with a feature set akin to the commercial edition.
-   Microsoft Data Science Virtual Machine: an analytics tool developed by the Revolution Analytics division premiered in January 2015.
-   Microsoft R Open: a rebranded version of Revolution R Open.

1, Introduction
---------------

**Importing data with `rxImport` function**

``` r
# Declare the file paths for the csv and xdf files
# find the path or directory where the file is, load the path variable
myAirlineCsv <- file.path(rxGetOption('sampleDataDir'), '2007_subset.csv')

# fin the data in this directory and load the data variable
myAirlineXdf <- '2007_subset.xdf'

# Use rxImport to import the data into xdf format
# rxImport(inData = myAirlineCsv, outFile = myAirlineXdf, overwrite = TRUE)
# or
# function within a function for more stats
system.time(rxImport(inData = myAirlineCsv, 
                     outFile = myAirlineXdf, 
                     overwrite = TRUE))
list.files()
```

**Functions for summarizing data**

``` r
# Get basic information about your data
rxGetInfo(data = myAirlineXdf, 
          getVarInfo = TRUE, 
          numRows = 10)

# Summarize the variables corresponding to actual elapsed time, time in the air, departure delay, flight Distance
rxSummary(formula = ~ ActualElapsedTime + AirTime + DepDelay + Distance, 
          data = myAirlineXdf)

# Histogram of departure delays
rxHistogram(formula = ~DepDelay, 
            data = myAirlineXdf)

# Use parameters similar to a regular histogram to zero in on the interesting area
rxHistogram(formula = ~DepDelay, 
            data = myAirlineXdf, 
            xAxisMinMax = c(-100, 400), 
            numBreaks = 500,
            xNumTicks = 10)
```

**Creating new variables using `rxDataStep`**

``` r
# Calculate an additional variable: airspeed (distance traveled / time in the air)
rxDataStep(inData = myAirlineXdf, 
           outFile = myAirlineXdf, 
           varsToKeep = c('Distance', 'AirTime'),
           transforms = list(airSpeed = Distance / Airtime),
           append = 'cols',
           overwrite = TRUE)

# Get Variable Information for airspeed
rxGetInfo(data = myAirlineXdf, 
          getVarInfo = TRUE,
          varsToKeep = 'airSpeed')

# Summary for the airspeed variable
rxSummary(~airSpeed, 
          data = myAirlineXdf)

# Construct a histogtam for airspeed
# We can use the xAxisMinMax argument to limit the X-axis
rxHistogram(~airSpeed, 
            data = myAirlineXdf)

rxHistogram(~airSpeed, 
            data = myAirlineXdf,
            xNumTicks = 10,
            numBreaks = 1500,
            xAxisMinMax = c(0,12))
```

**Transforming variables using `rxDataStep`**

``` r
# Conversion to miles per hour
rxDataStep(inData = myAirlineXdf, 
         outFile = myAirlineXdf, 
         varsToKeep = c('airSpeed'),
           transforms = list(airSpeed = airSpeed * 60),
         overwrite = TRUE)

# Histogram for airspeed after conversion
rxHistogram(~airSpeed, 
            data = myAirlineXdf)
```

**Correlations**

``` r
# Correlation for departure delay, arrival delay, and air speed
rxCor(formula = ~ DepDelay + ArrDelay + airSpeed,
      data = myAirlineXdf,
      rowSelection = (airSpeed > 50) & (airSpeed < 800))
```

**Linear regression**

``` r
# Regression for airSpeed based on departure delay
myLMobj <- rxLinMod(formula = airSpeed ~ DepDelay, 
         data = myAirlineXdf,
         rowSelection = (airSpeed > 50) & (airSpeed < 800))

summary(myLMobj)
```

2, Data Exploration
-------------------

**`RevoScaleR` options**

``` r
# Extract the names of the possible options
names(rxOptions())

# Extract the sample data directory
rxGetOption('sampleDataDir')

# View the current value of the reportProgress option
rxGetOption('reportProgress')

# Set the value of the reportProgress option to 0
rxOptions(reportProgress = 0)
```

**Import and explore Dow Jones data**

``` r
# Set up the variable that has the address of the relevant data file
djiXdf <- file.path(rxGetOption('sampleDataDir'), 'DJIAdaily.xdf')

# Get information about that dataset
rxGetInfo(djiXdf, getVarInfo = TRUE)
```

**Extracting meta data about a variable using `rxGetVarInfo`**

``` r
# Get variable information for the dataset
djiVarInfo <- rxGetVarInfo(djiXdf)
names(djiVarInfo)

# Extract information about the closing cost variable
(closeVarInfo <- djiVarInfo[['Close']])

# Get the class of the closeVarInfo object
class(closeVarInfo)

# Examine the structure of the closeVarInfo object
str(closeVarInfo)

# Extract the global maximum of the closing cost variable
closeMax <- closeVarInfo[['high']]
```

**Summarizing variables with `rxSummary`**

``` r
# Basic summary statistics
rxSummary(~ DayOfWeek + Close + Volume, 
          data = djiXdf)

# Frequency weighted
rxSummary(~ DayOfWeek + Close, 
          data = djiXdf, 
          fweights = 'Volume')

# Basic frequency count
rxCrossTabs(~ DayOfWeek, 
            data = djiXdf)
```

**Exploring a distribution with `rxHistogram`**

``` r
# Numeric Variables
rxHistogram(~ Close, 
            data = djiXdf)

# Categorical Variable
rxHistogram(~ DayOfWeek, 
            data = djiXdf)

# Different panels for different days of the week
rxHistogram(~ Close | DayOfWeek, 
            data = djiXdf)

# Numeric Variables with a frequency weighting
rxHistogram(~ Close, data = djiXdf, 
            fweights = 'Volume')
```

**Plotting bivariate relationships with `rxLinePlot`**

``` r
# Simple bivariate line plot
rxLinePlot(Close ~ DaysSince1928, 
           data = djiXdf)

# Using different panels for different days of the week
rxLinePlot(Close ~ DaysSince1928 | DayOfWeek, 
           data = djiXdf)

# Using different groups
rxLinePlot(Close ~ DaysSince1928, 
           groups = DayOfWeek, 
           data = djiXdf)

# Simple bivariate line plot, after taking the log() of the ordinate (y) variable
rxLinePlot(log(Close) ~ DaysSince1928, 
           data = djiXdf)
```

**Summarzing variables with `rxCrossTabs`**

``` r
# Compute the the summed volume for each day of the week
rxCrossTabs(formula = Volume ~ DayOfWeek, 
            data = djiXdf)

# Compute the the summed volume for each day of the week for each month
rxCrossTabs(formula = Volume ~ F(Month):DayOfWeek, 
            data = djiXdf)

# Compute the the average volume for each day of the week for each month
rxCrossTabs(formula = Volume ~ F(Month):DayOfWeek, 
            data = djiXdf, 
            means = TRUE)

# Compute the the average closing price for each day of the week for each month, using volume as frequency weights
rxCrossTabs(formula = Close ~ F(Month):DayOfWeek, 
            data = djiXdf, 
            means = TRUE, 
            fweights = 'Volume')
```

**Summarzing variables with `rxCube`**

``` r
# Compute the the summed volume for each day of the week
rxCrossTabs(Volume ~ DayOfWeek, 
            data = djiXdf)

rxCube(Volume ~ DayOfWeek, 
       data = djiXdf, 
       means = FALSE)

# Compute the the summed volume for each day of the week for each month
rxCrossTabs(Volume ~ F(Month):DayOfWeek, 
            data = djiXdf)

rxCube(Volume ~ F(Month):DayOfWeek, 
       data = djiXdf, 
       means = FALSE)

# Compute the the average volume for each day of the week for each month
rxCube(Volume ~ F(Month):DayOfWeek, 
       data = djiXdf)

# Compute the the average closing price for each day of the week for each month, using volume as frequency weights
rxCube(Close ~ DayOfWeek, 
       data = djiXdf, 
       fweights = 'Volume')
```

3, Data Manipulation
--------------------

**Using `rxDataStep` to transform data**

``` r
# Get information on mortData
rxGetInfo(mortData)

## Set up my personal copy of the data
myMortData <- 'myMD.xdf'

# Create the transform
rxDataStep(inData = mortData, 
           outFile = myMortData, 
           transforms = list(highDebtRow = ccDebt > 8000), 
           overwrite = TRUE)

#rxDataStep(inData = mortData, outFile = myMortData, transforms = list(highDebtRow = ccDebt > 8000))
# Get the variable information
rxGetVarInfo(myMortData)

# Get the proportion of values that are 1
rxSummary( ~ highDebtRow, 
           data = myMortData)

# Compute multiple transforms!
rxDataStep(inData = myMortData, outFile = myMortData,
           transforms = list(
             newHouse = houseAge < 10,
             ccsXhd = creditScore * highDebtRow),
           append = 'cols',
           overwrite = TRUE)
```

**More complex transforms using `transformFuncs`**

``` r
# Compute the summary statistics
(csSummary <- rxSummary(~ creditScore, data = mortData))

# Extract the mean and std. deviation
meanCS <- csSummary$sDataFrame$Mean[1]
sdCS <- csSummary$sDataFrame$StdDev[1]

# Create a function to compute the scaled variable
scaleCS <- function(mylist){
  mylist[['scaledCreditScore']] <- (mylist[['creditScore']] - myCenter) / myScale
  return(mylist)
}

# Run it with rxDataStep (A above in B below)
myMortData <- 'myMD.xdf'
rxDataStep(inData = mortData, outFile = myMortData,
           transformFunc = scaleCS,
           transformObjects = list(myCenter = meanCS, myScale = sdCS))

# Check the new variable
rxGetVarInfo(myMortData)
rxSummary(~ scaledCreditScore, 
          data = myMortData)
```

4, Data Analysis
----------------

**Preparing data for analysis: import**

``` r
# Declare the file paths for the csv and xdf files
myAirlineCsv <- file.path(rxGetOption('sampleDataDir'), 'AirlineDemoSmall.csv')
myAirlineXdf <- 'ADS.xdf'

# Use rxImport to import the data into xdf format
rxImport(inData = myAirlineCsv, 
         outFile = myAirlineXdf, 
         overwrite = TRUE,
         colInfo = list( 
           DayOfWeek = list(
            type = 'factor', 
            levels = c('Monday', 'Tuesday', 'Wednesday', 
                       'Thursday', 'Friday', 'Saturday', 'Sunday'))))
```

**Preparing data Ffor analysis: exploration**

``` r
# Summarize arrival delay for each day of the week
rxSummary(formula = ArrDelay ~ DayOfWeek, 
          data = myAirlineXdf)

# Vizualize the arrival delay histogram
rxHistogram(formula = ~ArrDelay, 
            data = myAirlineXdf)
```

**Construct a linear model**

``` r
# predict arrival delay by day of the week
myLM1 <- rxLinMod(ArrDelay ~ DayOfWeek, 
                  data = myAirlineXdf)

# summarize the model
smmary(myLM1)

# Use the transforms argument to create a factor variable associated with departure time 'on the fly,'
# predict Arrival Delay by the interaction between Day of the week and that new factor variable
myLM2 <- rxLinMod(ArrDelay ~ DayOfWeek, 
                  data = myAirlineXdf,
                  transforms = list(
                    catDepTime = cut(CRSDepTime, breaks = seq(from = 5, to = 23, by = 2))),
                    cube = TRUE)

# summarize the model
summary(myLM2)
```

**Generating predictions and residuals**

``` r
# Summarize model first
summary(myLM2)

# Path to new dataset storing predictions
myNewADS <- 'myNEWADS.xdf'

# Generate predictions
rxPredict(modelObject = myLM2, 
          data = myAirlineXdf, 
          outData = myNewADS, 
          writeModelVars = TRUE)

# Get information on the new dataset
rxGetInfo(myNewADS, getVarInfo = TRUE)

# Generate residuals.
rxPredict(modelObject = myLM2, 
          data = myAirlineXdf, 
          outData = myNewADS, 
          writeModelVars = TRUE, 
          computeResiduals = TRUE, 
          overwrite = TRUE)

# Get information on the new dataset
rxGetInfo(myNewADS, getVarInfo = TRUE)
```

**Logistic regression**

``` r
# look at the meta data
ls()
rxGetInfo(data = mortData, getVarInfo = TRUE)

# Construct the logit model
logitModel <- rxLogit(formula = default ~ houseAge + F(year) + ccDebt + creditScore + yearsEmploy, 
                      data = mortData)

# Summarize the result contained in logitModel
summary(logitModel
```

**Individual mortgage information**

``` r
# Summarize the model
summary(logitModel)

# view the first few rows
head(newData)

# Make predictions
dataWithPredictions <- rxPredict(modelObject = logitModel, 
                                 data = newData, 
                                 outData = newData, 
                                 type = 'response')

# view the predictions
dataWithPredictions
```

**Computing k-means with `rxKmeans`**

``` r
# Examine the mortData dataset
rxGetInfo(mortData, getVarInfo = TRUE)

# Set up a path to a new xdf file
myNewMortData = 'myMDwithKMeans.xdf'

# Run k-means
KMout <- rxKmeans(formula = ~ ccDebt + creditScore + houseAge, 
         data = mortData,
         outFile = myNewMortData,
         rowSelection = year == 2000,
         numClusters = 4,
         writeModelVars = TRUE)

print(KMout)

# Examine the variables in the new dataset:
rxGetInfo(myNewMortData, getVarInfo = TRUE)

# Summarize the cluster variable:
rxSummary(~ F(.rxCluster), data = myNewMortData)

# Read into memory 10% of the data:
mydf <- rxXdfToDataFrame(myNewMortData,
                         rowSelection = randSamp == 1,
                         varsToDrop = 'year',
                         transforms = list(randSamp = sample(10, size = .rxNumRows, replace = TRUE)))

## Visualize the clusters
plot(mydf[-1], col = mydf$.rxCluster)
```

**Create some decision trees**

``` r
# regression tree
regTreeOut <- rxDTree(default ~ creditScore + ccDebt + yearsEmploy + houseAge, 
                      rowSelection = year == 2000, 
                      data = mortData, maxdepth = 5)

# print out the object
print(regTreeOut)

# plot a dendrogram, and add node labels
plot(rxAddInheritance(regTreeOut))
text(rxAddInheritance(regTreeOut))

# Another visualization
#library(RevoTreeView)
#createTreeView(regTreeOut)
# predict values
myNewData = 'myNewMortData.xdf'

rxPredict(regTreeOut,
          data = mortData,
          outData = myNewData,
          writeModelVars = TRUE,
          predVarNames = 'default_RegPred')

# visualize ROC curve
rxRocCurve(actualVarName = 'default', 
           predVarNames = 'default_RegPred', 
           data = myNewData)
```