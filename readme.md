# Mercury Levels

A Java algorithm which estimates the value of some missing values that correspond to mercury levels in a river by using linear interpolation.

## Problem

A time series of daily readings of mercury levels in a river is provided. In each test case, the day's highest level is missing for certain days. By analysing the data, try to identify the missing mercury levels for those days. Each row of data contains two tab-separated values: a time-stamp and the day's highest reading.

There are exactly twenty rows marked missing in each input file. The missing values are marked as "Missing_1", "Missing_2", etc. These missing records have been randomly dispersed in the rows of data.

The input list is like so:
```java
1/3/2012 16:00:00   Missing_1
1/4/2012 16:00:00   27.47
1/5/2012 16:00:00   27.728
1/6/2012 16:00:00   28.19
1/9/2012 16:00:00   28.1
1/10/2012 16:00:00  28.15
....
....
....
12/13/2012 16:00:00 27.52
12/14/2012 16:00:00 Missing_19
12/17/2012 16:00:00 27.215
12/18/2012 16:00:00 27.63
12/19/2012 16:00:00 27.73
12/20/2012 16:00:00 Missing_20
12/21/2012 16:00:00 27.49
12/24/2012 13:00:00 
```

## Analysis

It can be see that there are 3 cases of where the missing values are placed in the array.
1) At the start (first element)
2) At the end (last element)
3) Somewhere in the middle

I decided to use linear interpolation to approximate the missing values. To do this, I needed 2 neighbouring values whose positions in the array depend on the case that the missing value falls into (the list above). The logic now looks like the following:

1) If case 1 (first element): Pick the next non-missing value (next neighbour) and the second next non-missing value (neighbour of next neighbour).
2) If case 2(last element): Pick the previous non-missing value (previous neighbour) and the second previous non-missing value (neighbour of previous neighbour).
3) If case 3 (middle): Pick the first non-missing value before (previous neighbour) and the first non-missing value after (next neighbour).

Using the above logic, the equations are derived:

1) ![equation](https://latex.codecogs.com/gif.latex?x_{1}&space;=&space;x_{k}&space;-&space;\frac{&space;x_{a&plus;k}-&space;x_{k}&space;}{a}&space;*&space;k) -> where x(k) is next known element and x(k+a) second next element.


2) ![equation](https://latex.codecogs.com/gif.latex?x_{i}&space;=&space;x_{i-j}&space;&plus;&space;\frac{&space;x_{i&plus;k}-&space;x_{i-j}&space;}{k&plus;j}&space;*&space;j) -> where x(i-j) is the previous neighbour and x(i+k) next neighbour.

3) ![equation](https://latex.codecogs.com/gif.latex?x_{n}&space;=&space;x_{n-k}&space;&plus;&space;\frac{&space;x_{n-k}-&space;x_{n-k-j}&space;}{j}&space;*&space;k) -> where x(n-k) is the previous neighbour and x(n-k-j) the second previous neighbour.


## Solution
1. The string is split into an array of 3 elements. The first element is the date, the second is the time, and the third is the value. i.e: ```array[2]``` returns ```27.49```. This makes it convenient to grab the values for calculation.

2. Neighbours are found using the defined functions. ```getNeighbourForward(i)``` loops from a starting index (i) until it finds an input with a non-missing value. ```getNeighbourBackward(j)``` loops from starting position j backwards until it finds a non-missing value. The existence of a starting index is important as it gives the ability of finding the second next neighbours.

3. The equation is created from the values and the answer is printed out to terminal.

## Accuracy

Using the following input:

```java
1/3/2012 16:00:00   Missing_1
1/4/2012 16:00:00   27.47
1/5/2012 16:00:00   27.728
1/6/2012 16:00:00   28.19
1/9/2012 16:00:00   28.1
1/10/2012 16:00:00  28.15
```

The result returned is ```27.212```. The actual value of 'Missing_1' is ```27.7```. The percentage difference is 1.76% which gives this algorithm an accuracy of >98%.