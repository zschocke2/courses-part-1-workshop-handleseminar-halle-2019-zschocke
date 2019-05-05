---
title: 'Advanced Task'
description: ""
---

## Phase synchronization

```yaml
type: NormalExercise
key: a1fcf27204
xp: 100
```

An alternative way to study the inter-relation between heart rate and respiratory activity is phase synchronization.  To calculate phase synchronization, you have to determine the instantaneous phases of both signals via a Hilbert transform, calculate the complex average of exp(i (phi_1 - phi_2)) over a segment of 60 seconds (i is the complex unit here), take the absolute value, and finally average over all 60 segments (of the 1h recordings).

`@instructions`
From chapter 1, we have the filtered heart rate data (```hf```) and the respiratory data (```resp```) available, both sampled at 32 Hz, and the time axis (```time```).
1. Transfer the time series into frequency space via FFT, assign 0 to the first coefficient $a_1$ and to all coefficients following $a_{n/2}$, including the whole mirror part of the spectrum, and transfer back to the time domain.
2. Use the function ```Arg(___)``` to calculate the phases phi of both time series.
3. Average the complex exponential **exp(complex(real=0, imaginary=1) * (phi_1 - phi_2))** over one minute and take the absolute value using ```abs(___)```.
4. Average the results for each of the 60 1-minute segments.

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
resp <- scan('respiration.dat')
resp <- resp[1:(length(resp)-128)]
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
