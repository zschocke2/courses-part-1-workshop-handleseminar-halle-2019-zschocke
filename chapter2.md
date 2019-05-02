---
title: 'Pattern recognition'
description: 'Here we will learn how to find simple patterns in signals and how to use them for further anylsis.'
---

## Introduction

```yaml
type: NormalExercise
key: ecd209faa9
xp: 100
```

In the second part of this workshop we will search for pattern in our RRI signal.

`@instructions`
1. Load the

`@hint`


`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

```

`@sample_code`
```{r}
# Load data and divide by sampling rate to obtain signal in seconds
data <- scan('data.rri')/256

# Calculate RR-intervals
rri <- diff(data) 

# Calculate time
time <- data[1:(length(data)-1)]

# Resample data to 4 Hz
new_time <- seq(1,3600,1/4)
approx <- approx(x = time, y= rri ,xout=new_time)

# Read rri_rs and time_rs from list approx
time_rs <- approx$x
rri_rs <- approx$y

# Plot data
plot(time,rri,"l")
```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Find anchor points

```yaml
type: NormalExercise
key: fd2af61423
xp: 100
```

Now we want to find anchor points in the resampled RRI signal, which is still available under ```rri_rs``` and ```time_rs```.

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

---

## Insert exercise title here

```yaml
type: NormalExercise
key: cc320eca75
xp: 100
```



`@instructions`


`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
"Hello, world!"
```

`@solution`
```{r}
# Example 2:
"Hello, world!"

```

`@sct`
```{r}

# SCT, robust to small typos
ex() %>% check_code("[H|h]ello,*\\s*[W|w]orld!")

```
