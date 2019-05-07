---
title: 'Arrays and pattern-based signal averaging'
description: 'Here we will learn how to find simple patterns in signals and how to use them for further anylsis.'
---

## Set up the matrix

```yaml
type: NormalExercise
key: ecd209faa9
xp: 100
```

In the second part of this workshop we will use a **matrix** -- a two dimensional (rectangular) arrangement of data with identical type. Note that the term **array** is used for three (or even higher) dimensional objects in R.

We will implement an approach called **phase rectified signal analysis** (PRSA), which is used to calculated a **deceleration capacity** (DC) index for long-term (24h) ECG recordings. The DC has been shown to predict the risk of mortality after an initial myocardial infarction, so that it can be used to identify subjects that would benefit from implanting an automated defibrillator (better known as pacemaker).

The approach can be generalized for an analysis of inter-relations between two (or even more) signals, leading to the so-called **bivariate phase rectified signal analysis** (BPRSA).  However, we begin with the mono-variate version.

`@instructions`
1. Load the long-term heartbeat recording ```"data.rri"``` from the previous chapter and calculate the RR intervals in seconds.  Remember that the numbers in the file are in **sampling units at 256 Hz**, and you need the differences.
2. Set up a matrix with one row of 40 zeros; employ ```rep``` and ```matrix``` for this.
3. Append to your matrix a row that is a partial copy of the RR interval time series around an anchor point -- here: RR interval number 100. Specifically, select 20 RR intervals prior to the anchor, the anchor itself, and 19 RR intervals behind the anchor. Employ ```rbind``` for the appending.
4. Print the transposed matrix to check what has been done; employ [```t```](https://www.rdocumentation.org/packages/base/versions/3.6.0/topics/t) for this.

`@hint`
- Use ```?rep```, ```?matrix```, ... to learn more about the functions!
- The row that must be appended is ```rri[(100-20):(100+19)]```.

`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')

```

`@sample_code`
```{r}
# Load data and divide by sampling rate to obtain signal in seconds; calculate RR-intervals
rri <- ___

# Create matrix with one row of 40 zeros.
mat <- matrix(___, ___, ___)

# Append another row, which is a copy of the RR interval series around the element with index 100.
mat <- rbind(___, ___)

# Print the matrix
t(___)

```

`@solution`
```{r}
# Load data and divide by sampling rate to obtain signal in seconds; calculate RR-intervals
rri <- diff(scan('data.rri')/256)

# Create matrix with one row of 40 zeros.
mat <- matrix(data=rep(x=0,times=40), nrow=1, ncol=40)

# Append another row, which is a copy of the RR interval series around the element with index 100.
mat <- rbind(mat, rri[(100-20):(100+19)])

# Print the matrix
t(mat)

```

`@sct`
```{r}
ex() %>% check_object("rri") %>% check_equal(incorrect_msg = "Did you use scan() and diff()? And did you transform the signal in seconds?")
ex() %>% check_function("matrix") %>% {
  check_arg(.,"data") %>% check_equal()
  check_arg(.,"nrow") %>% check_equal()
  check_arg(.,"ncol") %>% check_equal()
}
ex() %>% check_function("rep") %>% {
  check_arg(.,"x") %>% check_equal()
  check_arg(.,"times") %>% check_equal()
}

ex() %>% check_function("rbind") %>% check_result()

ex() %>% check_object("mat") %>% check_equal()

ex() %>% check_function("t") %>% check_arg("x") %>% check_equal()

success_msg("Good job.")
```

---

## Find anchor points

```yaml
type: NormalExercise
key: fd2af61423
xp: 100
```

Now we want to find anchor points in the RRI signal. There are many of them for the calculation of deceleration capacity -- each RR interval that is longer than the preceding one will be regarded as an anchor point. And thus many rows will be added to our matrix.

`@instructions`
1. The first two steps are already there from the previous exercise.
2. Run a loop over the RR interval time series, beginning with RR interval number 21 and ending 20 intervals before the last one.
3. Identify the anchor points, where the RR interval is longer than the preceding RR interval. 
4. For each anchor point, append the corresponding row to the large matrix. (Use brackets if you calculate index values!)
5. What is the size of the matrix?  Employ ```dim``` for this.

`@hint`
The anchor-point condition is rri[i] > rri[i-1]. The row rri[(i-20):(i+19)] must be appended.

`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')
```

`@sample_code`
```{r}
# Load data and divide by sampling rate to obtain signal in seconds; calculate RR-intervals
rri <- diff(scan('data.rri')/256)
# Create matrix with one row of 40 zeros.
mat <- matrix(rep(0,40), 1, 40)

# Loop over the RR intervals 
for (i in (__:___)){
  if (___){
    mat <- rbind(___, ___)
    }
  }

# Print the size of the matrix
dim(___)
     
```

`@solution`
```{r}
# Load data and divide by sampling rate to obtain signal in seconds; calculate RR-intervals
rri <- diff(scan('data.rri')/256)
# Create matrix with one row of 40 zeros.
mat <- matrix(rep(0,40), 1, 40)

# Loop over the RR intervals 
for (i in (21:(length(rri)-20))){
  if (rri[i] > rri[i-1]){
    mat <- rbind(mat, rri[(i-20):(i+19)])
    }
  }

# Print the size of the matrix
dim(mat)
           
```

`@sct`
```{r}
ex() %>% check_for() %>% {
  check_cond(.) %>% {
    check_code(., "20")
    check_code(., "21")
    check_code(., ":")
    check_code(.,"length(rri)",fixed=TRUE)
  }
  check_body(.) %>% {
    check_if_else(.,1) %>%  {
      check_cond(.) %>% {
        check_code(., "rri[i]",fixed=TRUE)
        check_code(., "rri[i-1]",fixed=TRUE)
        }
      check_if(.) %>% check_function(., "rbind")       
      } 
  } 
}
ex() %>% check_object("mat") %>% check_equal()
ex() %>% check_function("dim") %>% check_result()
success_msg("Nice work!")
```

---

## Plot PRSA curve and calculate DC

```yaml
type: NormalExercise
key: cc320eca75
xp: 100
```

The so-called PRSA curve is the average of all rows of our large matrix (except for the first row with zeros).  The DC value is defined as DC = -PRSA(-2) -PRSA(-1) +PRSA(0) +PRSA(1), where the argument indicates the position with respect to the anchor points.

`@instructions`
The matrix ```mat``` is still available from the previous exercise.  
1. Initialize ```PRSA``` as an empty vector.
2. Calculate the average of each column of ```mat``` (excluding the first row) using a for loop and append these values to ```PRSA```.
2. Plot the PRSA curve (of 40 data points).
3. Calculate the DC value in milliseconds.

`@hint`
The columns of the matrix are given by mat[2:(dim(mat)[2]),j].

`@pre_exercise_code`
```{r}
download.file(url='https://assets.datacamp.com/production/repositories/4882/datasets/fefc3f655fd0c9fd6baeeb6528e68d9e55d57db4/SL196_1h.rri',destfile='data.rri')
# Load data and divide by sampling rate to obtain signal in seconds; calculate RR-intervals
rri <- diff(scan('data.rri')/256)
# Create matrix with one row of 40 zeros.
mat <- matrix(rep(0,40), 1, 40)
# Loop over the RR intervals 
for (i in (21:(length(rri)-20))){
  if (rri[i] > rri[i-1]){
    mat <- rbind(mat, rri[(i-20):(i+19)])
    }
  }
```

`@sample_code`
```{r}
# Initialize PRSA
PRSA = ___

# Loop over the 40 columns
for (j in ___:___){
  PRSA <- append(___,mean(___))
}

# Plot of the PRSA curve
plot(seq(___,___,_), ___, "l")

# Calculate DC value
DC <- -PRSA[__]-PRSA[__]+PRSA[__]+PRSA[__]

# Output of DC in milliseconds

```

`@solution`
```{r}
# Initialize PRSA
PRSA = c()

# Loop over the 40 columns
for (j in 1:40){
  PRSA <- append(PRSA,mean(mat[2:(dim(mat)[2]),j]))
}

# Plot of the PRSA curve
plot(seq(-20,19,1), PRSA, "l")

# Calculate DC value
DC <- -PRSA[19]-PRSA[20]+PRSA[21]+PRSA[22]

# Output of DC in milliseconds
DC*1000

```

`@sct`
```{r}
ex() %>% check_function("c")

ex() %>% check_for() %>% {
  check_cond(.) %>% {
    check_code(., "j")
    check_code(., "in")
    check_code(.,"1")
    check_code(., "40")
    }
  check_body(.) %>% check_function("append") %>% {
    check_arg(., "x") %>% check_equal()
    check_arg(., "values") %>% check_equal()
    }
  }



success_msg("You got it!")
```
