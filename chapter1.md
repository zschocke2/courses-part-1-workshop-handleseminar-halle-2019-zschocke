---
title: 'Cross correlation'
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

In our second workshop we will have a look at heartbeat intervals and respiration data.

R peaks are large spikes in the ECG that mark the occurrence of the main heart contraction. Here we use a dataset of 36 000 heart beats, i.e. the clock-times of each R peak, from a single subject, single night.

First the R-peaks are stored in sampling units of ```256 Hz```, which means we have to divide each point by the sampling rate to obtain the time in seconds. 

`@instructions`
1. Load data from the file "data.rri" and convert R-peak points into time in seconds (in one line).
2. Calculate the RR intervals and save them to ```rri```. Use ```diff()```!
3. Create a ```time```, a list which contains the timestamps of each RRI, directly taken from ```data```. (Check length of ```rri``` and ```time```; they should be the same)

`@hint`
- Do you remember the function ```scan()``` to load data?
- You can divide a whole list by a divisor to divide each element of the list!
- ```diff``` will reduce the data by one (because you calculated intervals)
- ```plot(x,y)```

`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

```

`@sample_code`
```{r}
# Load data
data <- ___

# Calculate RRI
rri <- ___

# Calculate timestamps
time <- ___
             
# Plot RRI

```

`@solution`
```{r}
# Load data
data <- scan('data.rri')/256

# Calculate RRI
rri <- diff(data)

# Calculate timestamps
time <- data[1:length(data)-1]
             
# Plot RRI
plot(time,rri)
```

`@sct`
```{r}
ex() %>% check_function("scan") %>% check_arg("file") %>% check_equal()
ex() %>% check_object("data") %>% check_equal()

ex() %>% check_function("diff") %>% check_arg("x") %>% check_equal()
ex() %>% check_object("time") %>% check_equal()


ex() %>% check_function("plot") %>% {
  check_arg(.,"x")
  check_arg(.,"y")
} %>% check_equal()

ex() %>% check_error()
success_msg("Perfect!")
```

---

## Heart rate from RRI

```yaml
type: NormalExercise
key: e8803cd778
xp: 100
```

Now we will transform our RRIs to heart rates. The heart rate or the heart frequency is the reciprocal of the RRI value but in units of beats per minute (not beats per second).

`@instructions`
The RRIs are still available under ```rri```.
1. Calculate the heart rate in beats per minute.
2. Plot the heart rate against the time, ```time``` is still available.

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
hf <- ___

# plot heart rate
___
```

`@solution`
```{r}
# Calculate heart rate in beats per minute
hf <- 60/rri

# plot heart rate
plot(time,hf)
```

`@sct`
```{r}
ex() %>% check_object("hf") %>% check_equal()

ex() %>% check_function("plot") %>% {
  check_arg(.,"x")
  check_arg(.,"y")
} %>% check_equal()

ex() %>% check_error()
success_msg("Nice!")
```

---

## Filter

```yaml
type: NormalExercise
key: 5f3db91f6e
xp: 100
```

As we could see in the previous results some heart frequencies are not as expected. During sleep the heart rate should be between 40 and 120 beats/min*. All others are artifacts and should be removed.


*for this specific case

`@instructions`
```time``` and ```hf``` are still available.
1. First create two empty vectors ```time_new``` and ```hf_new``` by using ```c()``` (without any arguments).
2. Set up a ```for``` loop, which loops over all heart rates. Use ```i``` as loop variable.
3. Use ```if``` control to select frequencies between 40 and 120 beats/minute and append this values to ```hf_new```, also append the timestamp to ```time_new```. To append use ```new <- append(new, value)``` - it appends ```value``` to ```new```
4. Plot the filtered data.

`@hint`
- for loop has the structure:
	for (i in 1:1000){
    	INNER PART OF THE LOOP
    }
- if :
	if(CONDITION){
    	WHAT TO DO IF CONDITION IF FULFILLED
    }
- with & you can combine conditions as logical AND
- with | you can combine conditions as logical OR

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
# Create hf_new and time_new as emtpy vectors
hf_new <- ___
time_new <- ___

# Define for loop for all hear rates 


# Plot data 

```

`@solution`
```{r}
# Create hf_new and time_new as emtpy vectors
hf_new <- c()
time_new <- c()

# Define for-loop for all hear rates 
for (i in 1:length(hf)){
  if ((hf[i] > 40) & (hf[i] <120)){
   hf_new <- append(hf_new,hf[i])
   time_new <- append(time_new,time[i])
  } 
}

# Plot data
plot(time_new[1:100],hf_new[1:100])
```

`@sct`
```{r}
ex() %>% check_code("c()",times=2,fixed=TRUE)

ex() %>% check_for() %>% {
  check_cond(.) %>% {
    check_code(., "in")
    check_code(., "1")
    check_code(., c("length(hf)","length(time)","3615"),fixed=TRUE)
  }
  check_body(.) %>% {
    check_if_else(.) %>%  {
      check_cond(.) %>% {
        check_code(., c("hf[i]"),fixed=TRUE,times=2)
        check_code(.,c(">","<"),fixed=TRUE)
        check_code(.,c("40"),fixed=TRUE)   
        check_code(.,c(">","<"),fixed=TRUE)
        check_code(.,c("120"),fixed=TRUE)   
        check_code(.,"&")
      }
    check_if(.) %>% {
        check_code(., c("append("),fixed=TRUE,times=2)
        check_code(.,c("<-","=",times=2),fixed=TRUE)
      	check_code(.,"hf_new",times=2,fixed=TRUE)
      	check_code(.,"time_new",times=2,fixed=TRUE)
      	check_code(.,"hf[i]",fixed=TRUE)
        check_code(.,"time[i]",fixed=TRUE)   
    }
  }
    
 }
}
ex() %>% check_object("hf_new") %>% check_equal()
ex() %>% check_object("time_new") %>% check_equal()
ex() %>% check_error()
success_msg("Nice!")
```

---

## Resampling

```yaml
type: NormalExercise
key: d276edac58
xp: 100
```

In the following task we want to compare heart rate and respiration. To compare both signals, we need the same sampling rate for both, with equidistant time steps.
Thats why we need to resample our filtered heart rate signal ```hf_new``` to equidistant time steps and the same frequency as the respiration signal, 32 Hz.

Here we will use a linear interpolation from the function ```approx(x=,y=,xout=)```. ```x```and ```y``` are the values of the signal, while ```xout``` contains the x-values we want to interpolate.

`@instructions`
Your filtered data, which you have to use in the following exercises are still availabel under ```hf_new``` and ```time_new```.

1. Create a time series from 0 to 3600 seconds with a sampling rate of 32 Hz. Store it to ```time_rs``` (rs = resampled)
2. Use the ```approx()``` function to create a resampled signal! Store it to ```signal_approx```.
3. ```approx()``` will return a nested list. To access the new hf-values, you have to use ```signal_approx$y```. Store it to ```hf_rs```.
4. Plot the first ten seconds of the resampled data to see how the resampling worked.

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

# Define for-loop for all hear rates 
for (i in 1:length(hf)){
  if ((hf[i] > 40) & (hf[i] <120)){
   hf_new <- append(hf_new,hf[i])
   time_new <- append(time_new,time[i])
  } 
}

```

`@sample_code`
```{r}
# Create time series from 0 to 3600 seconds, with a sampling rate of 32 Hz
time_rs <-

# Use the approx() function to resample hf_new
signal_approx <- 

# Read the resampled signal from the nested list signal_approx
hf_rs <- 

# Plot the first 10 seconds of the resampled data
plot(
```

`@solution`
```{r}
# Create time series from 0 to 3600 seconds, with a sampling rate of 32 Hz
time_rs <- seq(0,3600,1/32)

# Use the approx() function to resample hf_new
signal_approx <- approx(x = time_new, y = hf_new, xout=time_rs)

# Read the resampled signal from the nested list signal_approx
hf_rs <- signal_approx$y

# Plot the new data
plot(time_rs[1:320],hf_rs[1:320])
```

`@sct`
```{r}
ex() %>% check_function("seq") %>% {
  check_arg(.,"from")%>% check_equal()
  check_arg(.,"to")%>% check_equal()
  check_arg(.,"by")%>% check_equal()
} %>% check_equal()
ex() %>% check_object("time_rs") %>% check_equal()

ex() %>% check_function("approx") %>% {
  check_arg(.,"x")%>% check_equal()
  check_arg(.,"y")%>% check_equal()
  check_arg(.,"xout")%>% check_equal()
} %>% check_equal()
ex() %>% check_object("signal_approx") %>% check_equal()

ex() %>% check_object("hf_rs") %>% check_equal()

ex() %>% check_function("plot") %>% {
  check_arg(.,"x") %>% check_equal()
  check_arg(.,"y") %>% check_equal()
} #%>% check_equal()

ex() %>% check_error()
success_msg("Well resampled!")
```

---

## Load and plot respiration data

```yaml
type: NormalExercise
key: 4fec7988a8
xp: 100
```

Now we check out the other data set. The **respiration signal** comes from thorax belt which measure the extension of the chest. It has a **sampling rate of 32 Hz**

`@instructions`
1. Load the data from ```respiration.dat``` to ```data```
2. Create a time series fitting to ```data``` and store it in ```time_data```
3. Plot the first 100 seconds of the data, by using the argument ```xlim``` of plot.

`@hint`
- Do you remember ```seq(from=,to=,by=)```? 
- You have different lengths? So maybe you have to reduce your time series by one?
- You have already a time series in seconds in ```time_data```, so ```ylim``` is also in seconds!

`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/f2663aa0a45d1bce64f9bbb6c8eb733aabbaca9e/SL196_thorax.txt',destfile='respiration.dat')
```

`@sample_code`
```{r}
# Load respiration data
data <- 

# Create time series
time_data <- 

# Plot the first 100 seconds (replace ___)
plot(x=___ ,y=___, xlim=c(___,___), ylim=c(-100,100))
```

`@solution`
```{r}
# Load respiration data
data <- scan('respiration.dat')

# Create time series
time_data <- seq(0,length(data)/32-1/32,1/32)

# Plot the first 100 seconds 
plot(x=time_data, y=data, xlim=c(0,100), ylim=c(-100,100))
```

`@sct`
```{r}
ex() %>% check_function("scan") %>% check_arg("file") %>% check_equal()
ex() %>% check_object("data") %>% check_equal()

ex() %>% check_function("seq") %>% {
  check_arg(.,"from")%>% check_equal()
  check_arg(.,"to")%>% check_equal()
  check_arg(.,"by")%>% check_equal()
} %>% check_equal()
ex() %>% check_object("time_data") %>% check_equal()

ex() %>% check_function("plot") %>% {
  check_arg(.,"x") %>% check_equal()
  check_arg(.,"y") %>% check_equal()
  check_arg(.,"xlim") %>% check_equal()
} %>% check_equal()

ex() %>% check_error()

success_msg("Awwsome!")
```

---

## Cross correlation function

```yaml
type: NormalExercise
key: 111b254e59
xp: 100
```

Finally we have a resampled series of the heart rate and we have the respiration, both at 32 Hz sampling rate. Physiologically, the respiration is modulating the heart rate via the so-called **respiratory sinus arrhythmia**. We want to answer two questions:

(a) How strong is the respiratory sinus arrhythmia is this subject?  While the strength depends on the respiratory frequency, it can also be considered as a marker of cardiovascular health.

(b) Is there a characteristic time delay associated with the respiratory sinus arrhythmia?  I.e., do the peaks in heart rate coincide with the peaks in the respiration signal or are they typically shifted by a small amount of time, either backwards or forwards in time?

To address both questions, we have to calculate the cross correlation function between both time series.

`@instructions`
We have to set up a function called ```cross_correlation```. Its parameters are the two time series and the time delay tau that we want to look at.  Please complete the function definition.
1. Both time series must have equal length. An error message should be printed if the lengths are unequal (```!=```). 
2. Another error message should be printed if the time delay exceeds half the length of the time series, since there would be insufficient statistics in such cases.
3. The average values (means) of both time series must be calculated and then be subtracted from the data. The function ```mean(series)``` can be used for this purpose.
4. The actual cross correlation is defined as the mean product of ```series1[i]``` times ```series2[i+tau]```. Take care that the mean includes only terms with indices ```i``` and ```i+tau``` that are within the defined range of the series, i.e. between 1 and the length. This can be implemented more easily, if the case of negative tau is considered separately.

`@hint`
Remember length() for n1 and n2. If tau is positive, the mean for calculating the cross correlation can begin with series1[1], but must end before series1[n1], since series2[n1+tau] does not exist. The last existing series2[i+tau] is for i=n1-tau.

`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
# create a correlation function for a defined timeshift tau in seconds
___ <- function(___,___,___) {
  # Calculate length 
  n1 <- ___
  n2 <- ___
  if (___){
    stop("Series1 and series2 have unequal length!")
  }
  if (___){
    stop("The value of tau exceeds half of the length of the series!")
  }
  # Calculate and subtract the means
  series1_detr = ___
  series2_detr = ___
  # Calculate 
  if (tau >= 0){
    corr <- mean(series2_detr[___:___]*series1_detr[___:___])
  } else{
    corr <- mean(series2_detr[___:___]*series1_detr[___:___])
  }
  return(corr)
}

```

`@solution`
```{r}
# create a correlation function for a defined timeshift tau in seconds
cross_correlation <- function(series1,series2,tau) {
  # Calculate length 
  n1 <- length(series1)
  n2 <- length(series2)
  if (n1 != n2){
    stop("Series1 and series2 have unequal length!")
  }
  if (tau > n1/2){
    stop("The value of tau exceeds half of the length of the series!")
  }
  # Calculate and subtract means
  series1_detr = series1 - mean(series1)
  series2_detr = series2 - mean(series2)
  # Calculate 
  if (tau >= 0){
    corr <- mean(series1_detr[1:(n1-tau)]*series2_detr[(1+tau):n1])
  } else{
    corr <- mean(series1_detr[(1-tau):n1]*series2_detr[1:(n1+tau)])
  }
  return(corr)
}

```

`@sct`
```{r}

```

---

## Application of cross correlation

```yaml
type: NormalExercise
key: 678902c4d8
xp: 100
```

We still want to answer our two questions:

(a) How strong is the respiratory sinus arrhythmia is this subject?  

(b) Is there a characteristic time delay associated with the respiratory sinus arrhythmia? 

Therefore, we have to look at the cross correlation function for various time delays tau, e.g. within the range from -4 to +4 seconds. The position of the largest peak will give the characteristic time delay, while the height of the peak will characterize the strength of respiratory sinus arrhythmia.



`@instructions`
Our cross correlation function ```cross_correlation(series1,series2,tau)``` is available. The heart rate is sill stored in ```hf``` and the respiration signal in ```resp```.
1. Create an empty vector ```corr```.
2. Calculate the correlation values for tau from -4 seconds to +4 seconds. But for the loop you need the sampling units -- remember the 32 Hz. Use the heart rate as ```series1```.
3. Plot the cross correlation function; remember that you also need a time axis for that.

`@hint`
Since the sampling rate is 32 Hz, -4 seconds corresponds to tau=-128, while +4 seconds corresponds to tau=+128.

`@pre_exercise_code`
```{r}
# create a correlation function for a defined timeshift tau in seconds
cross_correlation <- function(series1,series2,tau) {
  # Calculate length 
  n1 <- length(series1)
  n2 <- length(series2)
  if (n1 != n2){
    stop("Series1 and series2 have unequal length!")
  }
  if (tau > n1/2){
    stop("The value of tau exceeds half of the length of the series!")
  }
  # Calculate and subtract means
  series1_detr = series1 - mean(series1)
  series2_detr = series2 - mean(series2)
  # Calculate 
  if (tau >= 0){
    corr <- mean(series1_detr[1:(n1-tau)]*series2_detr[(1+tau):n1])
  } else{
    corr <- mean(series1_detr[(1-tau):n1]*series2_detr[1:(n1+tau)])
  }
  return(corr)
}

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
resp <- scan('respiration.dat')
resp <- resp[1:(length(resp)-128)]
```

`@sample_code`
```{r}
# Create an empty vector corr
corr <- c()

# Calculate the cross ccorrelation from -4 to 4 seconds and append the value to corr (replace ___)
for (t in ___:___){
	corr <- append(corr, cross_correlation(___,___,t))
}

# Create a time axis for the cross ccorrelation values
corr_time <- 

# Plot the correlation

```

`@solution`
```{r}
# Create an empty vector corr
corr <- c()

# Calculate the cross ccorrelation from -4 to 4 seconds and append the value to corr (replace ___)
for (tau in -128:128){
	corr <- append(corr,cross_correlation(hf,resp,tau))
}

# Create a time axis for the cross correlation values
corr_time <- seq(-4,4,1/32)

# Plot the correlation
plot(corr_time,corr)
```

`@sct`
```{r}
ex() %>% check_object("corr") %>% check_equal(incorrect_msg="Did you create an empty array? And did you use hf as series1?")

ex() %>% check_for() %>% {
  check_cond(.) %>% {
    check_code(., "-128")
    check_code(., "128")
  }
  check_body(.)  %>% check_function("correlation") %>% {
     check_arg(.,"series1") %>% check_equal()
     check_arg(.,"series2") %>% check_equal()
  } %>% check_equal()
}

ex() %>% check_function("seq") %>%{
  check_arg(.,"from") %>% check_equal()
  check_arg(.,"to") %>% check_equal()
  check_arg(.,"by") %>% check_equal()
}


ex() %>% check_function("plot") %>% {
  check_arg(.,"x") %>% check_equal()
  check_arg(.,"y") %>% check_equal()
} %>% check_equal()


ex() %>% check_error()

success_msg("Awwsome!")
```

---

## Insert exercise title here

```yaml
type: PureMultipleChoiceExercise
key: 11a8387479
xp: 50
```

Let's check the result from the last task again!
![](https://assets.datacamp.com/production/repositories/4882/datasets/19cb7f8ba735d63a8c65f4e3c23c6fb19e60534a/corr.png)

So we moved shifted the heart rate again the respiration. What is the time shift between respiration signal and heart rate signal?

`@hint`


`@possible_answers`
1. [+1.5 s]
2. -1.5 s
3. 7 s
4. 0 s

`@feedback`
1. Well done!
2. No
3. No
4. No
