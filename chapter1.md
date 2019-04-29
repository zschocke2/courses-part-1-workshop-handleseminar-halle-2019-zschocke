---
title: Crosscorrelation
description: 'Chapter description goes here.'
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

First the R-peaks are stored in sampling units of ```256 Hz```, which means we have to 

`@instructions`
1. Load data 
2. Convert R-peak-points into

`@hint`


`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/a47837f5a7199b0c6ffad81d989c52011c62ada5/SL196.rri',destfile='data.rri')

```

`@sample_code`
```{r}
# Load data
rri <- scan('data.rri')
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

## Resampling

```yaml
type: NormalExercise
key: d276edac58
xp: 100
```

resample heart rate data from RRI (to obtain sampling rate as respiration signal)

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

## Respiration and Heart rate

```yaml
type: NormalExercise
key: 4fec7988a8
xp: 100
```



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
