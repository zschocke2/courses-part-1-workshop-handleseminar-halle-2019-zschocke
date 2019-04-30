---
title: Crosscorrelation
description: 'Chapter description goes here.'
free_preview: true
---

## Load and plot RRI data

```yaml
type: NormalExercise
key: 2bafef99a3
lang: r
xp: 100
skills: 1
```

In our second workshop we will have a look on R-peaks and respiration data.

But let's simply start with R-peaks, R-peaks are derived from ECGs and describes the point of the main heart contraction. Here we use a dataset of 36 000 heart beats from a signle night of a random subject in the sleep laboratories of Charité Berlin. 

First the R-peaks are stored in sampling units of ```256 Hz```, which means we have to devide each point by the sampling rate to obtain the time in seconds.
Furthermore we can calculate the heart rate as the reciprocal of the R-R distance/RR-interval (RRI)

`@instructions`
1. Load data 
2. Convert R-peak-points into time in seconds
3. Calculate the heart rate. Use the first R-Interval as the time point of the heart rate.

`@hint`


`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

```

`@sample_code`
```{r}
# Load data
data <- scan('data.rri')/256

# Calculate RRI
rri <- diff(data)

# Calculate timestamps
time <- data[1:length(data)-1]
             
# Plot
plot(time,rri)
```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Heart rate from RRI

```yaml
type: NormalExercise
key: e8803cd778
xp: 100
```

Calculate heart rate from RRI

`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

# Load data
data <- scan('data.rri')/256

# Calculate RRI
rri <- diff(data)

# Calculate time
time <- data[1:(length(data)-1)]
```

`@sample_code`
```{r}
# Calculate heart rate in beats per minute
hf <- 60/rri

# plot heart rate
plot(time,hf)
```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Filter

```yaml
type: NormalExercise
key: 5f3db91f6e
xp: 100
```

Unrealisic heart frequence during sleep lower 40 higher 120 beats/min

`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

# Load data
data <- scan('data.rri')/256

# Calculate RRI
rri <- diff(data)

# Calculate time
time <- data[1:(length(data)-1)]

# Calculate heart rate in beats per minute
hf <- 60/rri

```

`@sample_code`
```{r}
# Delete all entries lower then 40 beats/min and higher then 120 beats/min. 
hf_new <- c()
time_new <- c()

for (i in 1:length(hf)){
  if ((hf[i] > 40) & (hf[i] <120)){
   hf_new <- append(hf_new,hf[i])
    time_new <- append(time_new,time[i])
  } 
}

plot(time_new[1:100],hf_new[1:100])
```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Resampling

```yaml
type: NormalExercise
key: d276edac58
xp: 100
```

In the next step we want to compare RRI and resample heart rate data from RRI (to obtain sampling rate as respiration signal)

approx(x,y,xout)
x - x values
y - values
xout - series of new datapoints

`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

# Load data
data <- scan('data.rri')/256

# Calculate RRI
rri <- diff(data)

# Calculate time
time <- data[1:(length(data)-1)]

# Calculate heart rate in beats per minute
hf <- 60/rri

# Delete all entries lower then 40 beats/min and higher then 120 beats/min. 
hf_new <- c()
time_new <- c()

for (i in 1:length(hf)){
  if ((hf[i] > 40) & (hf[i] <120)){
   hf_new <- append(hf_new,hf[i])
    time_new <- append(time_new,time[i])
  } 
}

```

`@sample_code`
```{r}
# Resample hf_new data
xout <- seq(0,3600,1/32)
approx <- approx(x = time_new, y = hf_new,xout=xout)
time <- approx$x
hf <- approx$y

plot(time,hf)



```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Load and plot respiration data

```yaml
type: NormalExercise
key: 4fec7988a8
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/f2663aa0a45d1bce64f9bbb6c8eb733aabbaca9e/SL196_thorax.txt',destfile='respiration.dat')
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

# Load data
data <- scan('data.rri')/256

# Calculate RRI
rri <- diff(data)

# Calculate time
time <- data[1:(length(data)-1)]

# Calculate heart rate in beats per minute
hf <- 60/rri

# Delete all entries lower then 40 beats/min and higher then 120 beats/min. 
hf_new <- c()
time_new <- c()

for (i in 1:length(hf)){
  if ((hf[i] > 40) & (hf[i] <120)){
   hf_new <- append(hf_new,hf[i])
    time_new <- append(time_new,time[i])
  } 
}
```

`@sample_code`
```{r}
# Load respiration data
data <- scan('respiration.dat')
length(data)
time_data <- seq(0,(3600-1/32),1/32)

plot(time_data,data,xlim=c(0,100),ylim=c(-100,100))






```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Synchronisation via Crosscorrelation - Part I

```yaml
type: NormalExercise
key: 678902c4d8
xp: 100
```

How to calculate crosscorrelation

`@instructions`


`@hint`


`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/f2663aa0a45d1bce64f9bbb6c8eb733aabbaca9e/SL196_thorax.txt',destfile='respiration.dat')
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

# Load data
data <- scan('data.rri')/256

# Calculate RRI
rri <- diff(data)

# Calculate time
time <- data[1:(length(data)-1)]

# Calculate heart rate in beats per minute
hf <- 60/rri

# Delete all entries lower then 40 beats/min and higher then 120 beats/min. 
hf_new <- c()
time_new <- c()

for (i in 1:length(hf)){
  if ((hf[i] > 40) & (hf[i] <120)){
   hf_new <- append(hf_new,hf[i])
    time_new <- append(time_new,time[i])
  } 
}

# Resample hf_new data
xout <- seq(1,3600,1/32)
approx <- approx(x = time_new, y = hf_new,xout=xout)
time <- approx$x
hf <- approx$y
hf <- hf[1:(length(hf)-129)]

# Load respiration data
data <- scan('respiration.dat')
data <- data[1:(length(data)-128)]
```

`@sample_code`
```{r}
# create a correlation function for a defined timeshift ts in seconds
# ts - timeshift
# fs - sampling frequency (both the same)
# series1 and series2 must have the same length
correlation <- function(series1,series2,ts,fs) {
  # Calculate lenght 
  n1 <- length(series1)
  n2 <- length(series2)
  
  if (n1!=n2){
    stop("Series1 and series2 have unequal length!")
  }
  
  # lenght of used data
  n <- as.integer(n1-ts*fs)
  if (ts >= 0){
  	corr <- sum(series1[1:as.integer(n1-ts*fs)]*series2[as.integer(1+ts*fs):n1])/n
  } else{
    corr <- sum(series1[as.integer(1+abs(ts)*fs):n1]*series2[1:as.integer(n1-abs(ts)*fs)])/n
  }
  # Calculate correlation of series1 and series2 with shift ts of series2
  
  return(corr)
}

length(data)
length(hf)

corr <- c()
corr_ts <- seq(-3,3,by=0.1)
for (ts in corr_ts){
	corr <- append(corr,correlation(data,hf,ts,32))
}
plot(corr_ts,corr)
```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Synchronisation via Crosscorrelation - Part II

```yaml
type: NormalExercise
key: cc151fae3d
xp: 100
```

Find timeshift

`@instructions`


`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

```
