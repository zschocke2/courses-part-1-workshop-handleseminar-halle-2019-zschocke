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

First the R-peaks are stored in sampling units of ```256 Hz```, which means we have to divide each point by the sampling rate to obtain the time in seconds.
Furthermore we can calculate the heart rate as the reciprocal of the R-R distance called RR-interval (RRI).

`@instructions`
1. Load data and Convert R-peak-points into time in seconds (in one line)
2. Calculate the RR-intervals and save it to ```rri```. Use ```diff()```!
3. Create a ```time```, a list which contains the timestamps of each RRI. (Check length of ```rri``` and ```time```, they should be the same)

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

Now we will transform our RRI to a heart rate. The heart rate or the heart frequency is the reciproke of the RRI but in units of beats per minute.

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

As we could see in the previous results some heart frequencies are not as expected. During sleep the heart rate should be between 40 and 120 beats/min*. All others are artefacts and should be removed.


*for this specific case

`@instructions`
```time``` and ```hf``` are still available.
1. First create 2 empty vectors ```time_new``` and ```hf_new``` by using ```c()```.
2. Create a for-loop, which loops over all heart rates. Use ```i``` as loop variable
3. Use as if-control to select frequencies between 40 and 120 beats/minute and append this values to ```hf_new```, also append the timestamp to ```time_new```. To append use ```new <- append(new, value)``` - it appends ```value``` to ```new```
4. Plot the filtered data.

`@hint`
- for loop has the structur:
	for (i in 1:1000){
    	INNER PART OF THE LOOP
    }
- if :
	if(CONDITION){
    	WHAT TO DO IF CONDITION IF FULFILED
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

# Define for-loop for all hear rates 







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

Here we will use a linear interpolation from the function ```approx(x=,y=,xout=)```. ```x```and ```y``` are the values of the signal, while ```xout``` contains the x-valuse we want to interpolate.

`@instructions`
Your filtered data, which you have to use in the following exercises are still availabel under ```hf_new``` and ```time_new```.

1. Create a time series from 0 to 3600 seconds with a sampling rate of 32 Hz. Store it to ```time_rs``` (rs = resampled)
2. Use the ```approx()``` function to create a resampled signal! Store it to ```signal_approx```.
3. ```approx()``` will return a nested list. To access the new hf-values, you have to use ```signal_approx$y```. Store it to ```hf_rs```.
4. Plot the resampled data.

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

# Plot the new data

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
plot(time_rs,hf_rs)
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

## Synchronisation via Crosscorrelation

```yaml
type: NormalExercise
key: 678902c4d8
xp: 100
```

Finally we have a resampled series of the heart rate and we have the respiration. Both signals come from different devices which are not perfect synchronized. Now we have to find out the time difference between both time series. We can do this by cross-correlation (basicly the same as an auto-correlation)! The signal has still **sampling rate of 32 Hz**

`@instructions`
We have already created a correlation function ```correlation(series1,series2,tau)```. The heart rate is sill stored in ```hf``` and the respiration signal in ```resp```.
1. Create an empty vector ```corr```.
2. Calculate the correlation values for tau from -2 seconds to +2 seconds. (But for the loop you need the sampling units!) Use the heart rate as ```series1```.
3. Create the time series ```corr_time``` from -2 to 2 seconds with the sampling rate of 32 Hz.
3. Plot the correlation function

`@hint`
- You can create an empty vector by ```c()```

`@pre_exercise_code`
```{r}
# create a correlation function for a defined timeshift ts in seconds
correlation <- function(series1,series2,tau) {
  # Calculate length 
  n1 <- length(series1)
  n2 <- length(series2)
  
  if (n1!=n2){
    stop("Series1 and series2 have unequal length!")
  }
  corr <- 0
  # Calculate 
  if (tau >= 0){
    print(series2)
    corr <- sum(series2[1:(n1-tau)]*series1[(1+tau):n1])/(n1-tau)
  } else{
    corr <- sum(series2[(1-tau):n1]*series1[1:(n1+tau)])/(n1-tau)
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

# Calculate the correlation from -2 to 2 seconds and append the value to corr (replace ___)
for (___ in ___:___){
	corr <- append(corr,correlation(___,___,___))
}

# Create a time series for the correlation values
corr_time <- 

# Plot the correlation

```

`@solution`
```{r}
# Create an empty vector corr
corr <- c()

# Calculate the correlation from -2 to 2 seconds and append the value to corr (replace ___)
correlation(hf,resp,0)
for (t in 1:64){
	corr <- append(corr,correlation(hf,resp,t))
}

# Create a time series for the correlation values
corr_time <- seq(-2,2,1/32)

# Plot the correlationplot
plot(corr_time,corr)
```

`@sct`
```{r}

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
