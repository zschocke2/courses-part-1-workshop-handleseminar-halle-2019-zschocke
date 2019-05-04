---
title: 'Arrays and pattern-based signal averaging'
description: 'Here we will learn how to find simple patterns in signals and how to use them for further anylsis.'
---

## Introduction

```yaml
type: NormalExercise
key: ecd209faa9
xp: 100
```

In the second part of this workshop we will use a **matrix** -- a two dimensional (rectangular) arrangement of data with identical type. Note that the term **array** is used for three (or even higher) dimensional objects in R.

We will implement an approach called **phase rectified signal analysis** (PRSA), which is used to calculated a **deceleration capacity** (DC) index for long-term (24h) ECG recordings. The DC has been shown to predict the risk of mortality after an initial myocardial infarction, so that it can be used to identify subjects that would benefit from implanting an automated defibrillator.

The approach can be generalized for an analysis of inter-relations between two (or even more) signals, leading to the so-called **bivariate phase rectified signal analysis** (BPRSA).  However, we begin with the mono-variate version.

`@instructions`
1. Load the long-term heartbeat recording "data.rri" from the previous chapter and calculate the RR intervals in seconds.  Remember that the numbers in the file are in sampling units at 256 Hz and you need the differences.
2. Set up a matrix with one row of 40 zeros; employ ```rep``` and ```matrix``` for this.
3. Append to your matrix a row that is a partial copy of the RR interval time series around an anchor point -- here: RR interval number 100. Specifically, select 20 RR intervals prior to the anchor, the anchor itself, and 19 RR intervals behind the anchor. Employ ```rbind``` for the appending.
4. Print the transposed matrix to check what has been done; employ ```transpose``` for this.

`@hint`
The row that must be appended is rri[(100-20):(100+19)].

`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

```

`@sample_code`
```{r}
# Load data and divide by sampling rate to obtain signal in seconds; calculate RR-intervals
rri <- diff(scan('data.rri')/256)

# Create matrix with one row of 60 zeros.
mat <- matrix(rep(0,40), 1)

# Append another row, which is a copy of the RR interval series around the element with index 100.
mat <- rbind(mat, rri[(100-20):(100+19)])

# Print the matrix
transpose(mat)

```

`@solution`
```{r}
# Load data and divide by sampling rate to obtain signal in seconds; calculate RR-intervals
rri <- diff(scan('data.rri')/256)

# Create matrix with one row of 60 zeros.
mat <- matrix(rep(0,40), 1)

# Append another row, which is a copy of the RR interval series around the element with index 100.
mat <- rbind(mat, rri[(100-20):(100+19)])

# Print the matrix
transpose(mat)

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
Run a loop over the RR interval time series, beginning with RR interval number 30 and ending 30 intervals before the last one.
4. Identify the anchor points, where the RR interval is longer than the preceding RR interval.

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
