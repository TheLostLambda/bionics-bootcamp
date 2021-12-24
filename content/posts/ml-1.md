---
title: "Machine Learning 1: Data Preprocessing"
date: 2021-03-03T18:04:28+01:00
authors: ["tll"]
categories:
  - Software
tags:
  - Week 3
  - Semester 2
  - Level 1
  - Python
  - Programming
  - Machine Learning
---

Getting Help
============

Even if you've missed the session, you'll be able to ask questions about whatever, whenever on [our Discord](https://discord.gg/N4k7ECt). Feel free to drop your question in the #bootcamp channel or just message an instructor directly.

Recording
=========

This session has [an accompanying blog post on the main Bionics blog](https://sheffield-bionics.gitlab.io/blog/summer-of-ml2/) that was written after this recording took place. As a result, there may be small improvements present in the written version of this lesson that are missing from the video recording.

{{< youtube KRZd23h_tn0 >}}

## The Big Picture

As even the most sophisticated machine learning algorithms are nothing more than a set of equations, they are inherently fickle and require data to be carefully pre-processed and "shaped" before they can do much meaningful with it. As data scientists / computer programmers, it's our job to frame the problems we want to solve in an ML friendly way.

As discussed in [the previous Summer of ML post](/blog/summer-of-ml/), we're looking to classify EMG patterns into certain "gestures". Ultimately, we need to provide our ML algorithm with a set of "good examples" for it to learn from. In our case, this means generating pairs of raw EMG data and gesture classifications. However, before we go any further, we should take a closer look at how our unprocessed data is structured.

## Our Data Set

Our data is split in two ways: first into gestures and then into repeats of the same gesture. Each unique gesture is given its own CSV (Comma-Separated Values) file, and the repeats therein are separated by divider lines.

In the end, we recorded 8 gestures stored in the following files:

```
data
├── emgData-G0.csv
├── emgData-G1.csv
├── emgData-G2.csv
├── emgData-G3.csv
├── emgData-G4.csv
├── emgData-G5.csv
├── emgData-G6.csv
└── emgData-G7.csv
```

Each file contains a header, then a series of data blocks separated by a divider containing the word "Repeat" and the number of the repeat:

```csv
time,emg0,emg1,emg2,emg3,emg4,emg5,emg6,emg7,quat0,quat1,quat2,quat3,acc0,acc1,acc2,gyro0,gyro2,gyro2
19,68,115,38,30,36,98,147,64,12304,-1895,2349,-10389,395,-417,1822,19,-125,37
36,66,132,39,29,39,94,152,60,12297,-1853,2436,-10385,-943,-988,1932,-20,119,-63
...
2389,79,98,27,32,40,136,187,95,12392,-1777,2419,-10289,-370,-575,1484,71,-132,-34
2412,84,110,29,32,42,135,188,100,12397,-1780,2394,-10289,-115,-495,1133,116,-125,-1
3,Repeat 2
26,92,122,28,37,38,98,169,91,12399,-1816,2312,-10299,346,-542,1004,97,-75,-1
48,94,119,25,36,40,97,171,90,12394,-1766,2406,-10292,-1182,-881,1299,75,35,-76
... and so on
```

Much like a spreadsheet, the first line of a CSV file stores a list of named columns, and each line that follows it represents a row of data in the table.

If you'd like a closer look at our data, feel free to [view or download it here](https://gitlab.com/sheffield-bionics/arms-and-glory/-/tree/a53222cb30df8f6057390d95ae8565c5e052ca63/data/final).

## Loading Everything Into Pandas

The first step of data processing is almost always loading the data! Normally this is a trivial task in Python, but because we've split our data into several files, we'll need to do some extra work.

Let's start with a couple of imports!

```py
import pandas as pd
import numpy as np
import os
import re
```

Let's go through these imports one at a time and talk a little bit about why we need them to load our data.

Pandas is an incredibly popular Python library replicating the "DataFrames" found in the statistical programming language R. DataFrames are very similar to spreadsheets in structure – storing data in a series of (optionally named) columns and rows. Pandas provides us with not only DataFrames, but also several ways of easily creating them (like the `read_csv()` function we'll be using later). There is a lot you can learn about Pandas, so it might be worth your time to check out the [User Guide](https://pandas.pydata.org/pandas-docs/stable/user_guide/index.html) at some point.

Numpy is another commonly-used Python library for working with numerical data. Most relevant to us are the computationally-efficient arrays that ML libraries like TensorFlow often expect as inputs. If you're looking to do any sort of scientific computing in Python, you'll likely run into Numpy somewhere – the [Learn Numpy](https://numpy.org/learn/) page could come in handy.

The `os` module of the Python standard library (available on all Python installs) helps in writing platform-independent code. Different operating systems (Linux, macOS, Windows, etc) can have different ways of accessing files, running programs, or querying system information – the `os` module provides a single, consistent interface for these actions across all platforms. We'll be primarily using this module to list and load files, but check out the [Module Documentation](https://docs.python.org/3/library/os.html) if you'd like to know more.

Finally, we have the `re` import. This once again comes from the standard library, but now provides us with "regular expressions". In a nutshell, regular expressions (sometimes called "regex" for short) are carefully constructed strings encoding some sort of search pattern. While regex strings can be intimidating (the string `'^\(*\d{3}\)*( |-)*\d{3}( |-)*\d{4}$'` might not look much like a phone number), we'll only be using some very basic features to extract a gesture number from a filename. [Regex101](https://regex101.com/) is a wonderful place to learn and test regular expressions, and the Python [Module Documentation](https://docs.python.org/3/library/re.html) can fill you in on the specifics.

With that out of the way, let's get to writing some real code! We'll start by defining a function to load a single gesture from one of our CSV files.

```py
def read_repeats(file):
    return pd.read_csv(file)
```
 Now let's test our super-simple function by pointing it to our data:
 ```py
 # My data is stored in the `data/final/` directory
>>> read_repeats('data/final/emgData-G0.csv')
        time emg0   emg1  emg2  emg3  ...   acc1    acc2  gyro0  gyro2  gyro2.1
0        19   68  115.0  38.0  30.0  ... -417.0  1822.0   19.0 -125.0     37.0
1        36   66  132.0  39.0  29.0  ... -988.0  1932.0  -20.0  119.0    -63.0
2        56   53  108.0  35.0  29.0  ... -988.0  1932.0  -20.0  119.0    -63.0
3        72   52  104.0  31.0  27.0  ... -502.0  2451.0  -64.0  -77.0     49.0
4        87   63  119.0  27.0  29.0  ... -375.0  1670.0    7.0  -91.0     62.0
...     ...  ...    ...   ...   ...  ...    ...     ...    ...    ...      ...
3833  24413   17   24.0  18.0  20.0  ... -663.0  1771.0   -6.0   -9.0     11.0
3834  24436   17   25.0  19.0  22.0  ... -680.0  1775.0  -22.0    1.0     15.0
3835  24453   16   22.0  20.0  21.0  ... -684.0  1776.0  -33.0    4.0     17.0
3836  24473   17   22.0  19.0  23.0  ... -684.0  1787.0  -43.0    9.0     16.0
3837  24496   15   20.0  20.0  24.0  ... -707.0  1774.0  -53.0   11.0     13.0

[3838 rows x 19 columns]
```

That's certainly a good start, but we've got a bit of a problem: we're loading in data we don't need! In addition to just EMG signals, the Myo armband also records orientation and acceleration information that are currently cluttering our DataFrame.

Luckily for us, Pandas contains the `loc[]` accessor that we can use to select certain rows and columns from our DataFrame – let's use this to keep only the EMG data:

```py
def read_repeats(file):
    # `:` implicitly selects everything between the first and last rows
    # 'emg0':'emg7' just selects the range of columns containing the EMG data
    return pd.read_csv(file).loc[:, 'emg0':'emg7']
```

Let's test this new data-loading function!

 ```py
>>> read_repeats('data/final/emgData-G0.csv')
      emg0   emg1  emg2  emg3  emg4  emg5   emg6  emg7
0      68  115.0  38.0  30.0  36.0  98.0  147.0  64.0
1      66  132.0  39.0  29.0  39.0  94.0  152.0  60.0
2      53  108.0  35.0  29.0  40.0  79.0  141.0  64.0
3      52  104.0  31.0  27.0  37.0  70.0  135.0  62.0
4      63  119.0  27.0  29.0  36.0  57.0  120.0  63.0
...   ...    ...   ...   ...   ...   ...    ...   ...
3833   17   24.0  18.0  20.0  25.0  34.0   57.0  50.0
3834   17   25.0  19.0  22.0  26.0  34.0   55.0  51.0
3835   16   22.0  20.0  21.0  22.0  29.0   52.0  45.0
3836   17   22.0  19.0  23.0  25.0  28.0   51.0  46.0
3837   15   20.0  20.0  24.0  25.0  29.0   50.0  41.0

[3838 rows x 8 columns]
```

That's much more manageable! If we take a bit of a closer look though, we still have a problem... Let's look at some of the rows around 121:

```py
>>> read_repeats('data/final/emgData-G0.csv')[117:126]
         emg0   emg1  emg2  emg3  emg4   emg5   emg6   emg7
117        72   98.0  29.0  34.0  41.0  144.0  208.0  106.0
118        70  101.0  30.0  33.0  41.0  139.0  192.0  100.0
119        79   98.0  27.0  32.0  40.0  136.0  187.0   95.0
120        84  110.0  29.0  32.0  42.0  135.0  188.0  100.0
121  Repeat 2    NaN   NaN   NaN   NaN    NaN    NaN    NaN
122        92  122.0  28.0  37.0  38.0   98.0  169.0   91.0
123        94  119.0  25.0  36.0  40.0   97.0  171.0   90.0
124        82  125.0  26.0  33.0  37.0   87.0  154.0   90.0
125        89  108.0  32.0  36.0  42.0   94.0  153.0  104.0
```

Uh oh, our repeat-dividers seem to be causing us a bit of trouble! We're going to have to track them down and manually split up our data!

## Splitting Up Repeats

Note that this style of divider could easily be considered a design flaw on my part. If I'd instead added a column to every row of the DataFrame that indicated which repeat the data belonged to, I could have easily separated things using the [`.groupby()` and `.get_group()` methods](https://pandas.pydata.org/pandas-docs/stable/user_guide/groupby.html#selecting-a-group) in Pandas.

With that being said, let's make the best of a poor situation and find all of the dividers using [boolean indexing](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#boolean-indexing). One way to identify these divider lines is to look for `NaN` values in the `emg1:emg7` columns:

```py
def read_repeats(file):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    # `np.isnan(df['emg'])` marks every row as either a divider or a normal row
    # Divider rows (marked with `True`) are selected by the outer `df[...]`
    return df[np.isnan(df['emg1'])]
```

Testing things:

 ```py
>>> read_repeats('data/final/emgData-G0.csv')
            emg0  emg1  emg2  emg3  emg4  emg5  emg6  emg7
121    Repeat 2   NaN   NaN   NaN   NaN   NaN   NaN   NaN
236    Repeat 3   NaN   NaN   NaN   NaN   NaN   NaN   NaN
356    Repeat 4   NaN   NaN   NaN   NaN   NaN   NaN   NaN
...         ...   ...   ...   ...   ...   ...   ...   ...
3512  Repeat 29   NaN   NaN   NaN   NaN   NaN   NaN   NaN
3637  Repeat 30   NaN   NaN   NaN   NaN   NaN   NaN   NaN
3778  Repeat 31   NaN   NaN   NaN   NaN   NaN   NaN   NaN
```

That's looking great! We certainly don't need all of the `NaN`s and "Repeat" strings though, so let's return only the indices:

```py
def read_repeats(file):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    # `.index` returns only the row numbers of the dividers – not the whole row
    return df[np.isnan(df['emg1'])].index
```

```py
>>> read_repeats('data/final/emgData-G0.csv')
Int64Index([ 121,  236,  356,  478,  622,  742,  877, 1019, 1180, 1308, 1415,
            1530, 1668, 1773, 1890, 2006, 2133, 2267, 2389, 2515, 2644, 2762,
            2890, 3010, 3138, 3263, 3390, 3512, 3637, 3778],
           dtype='int64')
```

Nearly there! Just one last thing to do before we start chunking up our data – we need to add the implicit repeat-boundary at position -1 (since index 0 starts the first repeat).

```py
def read_repeats(file):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    # `.insert(0,-1)` inserts the value -1 at the start of the list (index 0)
    return df[np.isnan(df['emg1'])].index.insert(0,-1)
```

```py
>>> read_repeats('data/final/emgData-G0.csv')
Int64Index([  -1,  121,  236,  356,  478,  622,  742,  877, 1019, 1180, 1308,
            1415, 1530, 1668, 1773, 1890, 2006, 2133, 2267, 2389, 2515, 2644,
            2762, 2890, 3010, 3138, 3263, 3390, 3512, 3637, 3778],
           dtype='int64')
```

The last thing to do is pair up these boundaries so that we have a start and stop index for each repeat.

```py
def read_repeats(file):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    bounds = df[np.isnan(df['emg1'])].index.insert(0,-1)
    # `zip()` will take two lists (the bounds list and the bounds list shifted
    # by one) and turn them into a single list of tuples. `list()` is
    # temporarily here to let us view the result
    return list(zip(bounds, bounds[1:]))
```

```py
>>> read_repeats('data/final/emgData-G0.csv')
[(-1, 121),
 (121, 236),
 (236, 356),
 (356, 478),
 (478, 622),
 (622, 742),
 (742, 877),
 (877, 1019),
 (1019, 1180),
 (1180, 1308),
 (1308, 1415),
 (1415, 1530),
 (1530, 1668),
 (1668, 1773),
 (1773, 1890),
 (1890, 2006),
 (2006, 2133),
 (2133, 2267),
 (2267, 2389),
 (2389, 2515),
 (2515, 2644),
 (2644, 2762),
 (2762, 2890),
 (2890, 3010),
 (3010, 3138),
 (3138, 3263),
 (3263, 3390),
 (3390, 3512),
 (3512, 3637),
 (3637, 3778)]
 ```

 Here we can see why adding the implicit boundary at -1 was necessary! It's also worth noting that we've *not* added a boundary at the end of the data. This is an intentional omission so that we don't collect the final repeat of each file (which, due to our collection methodology, does not actually contain a gesture). Peculiarities like these are why it's important to know the dataset you are working with!

 Finally, let's actually split the data on these boundaries (using a list comprehension):

```py
def read_repeats(file):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    bounds = df[np.isnan(df['emg1'])].index.insert(0,-1)
    # `.iloc[]` allows for slicing DataFrame rows by their indices – the +1 is
    # to avoid including the boundary itself and the `end` bound is exclusive
    return [df.iloc[start+1:end] for start,end in zip(bounds, bounds[1:])]
```

```py
>>> read_repeats('data/final/emgData-G0.csv')
[    emg0   emg1  emg2  emg3  emg4   emg5   emg6   emg7
 0     68  115.0  38.0  30.0  36.0   98.0  147.0   64.0
 1     66  132.0  39.0  29.0  39.0   94.0  152.0   60.0
 2     53  108.0  35.0  29.0  40.0   79.0  141.0   64.0
 3     52  104.0  31.0  27.0  37.0   70.0  135.0   62.0
 4     63  119.0  27.0  29.0  36.0   57.0  120.0   63.0
 ..   ...    ...   ...   ...   ...    ...    ...    ...
 116   72  126.0  33.0  35.0  43.0  147.0  230.0  102.0
 117   72   98.0  29.0  34.0  41.0  144.0  208.0  106.0
 118   70  101.0  30.0  33.0  41.0  139.0  192.0  100.0
 119   79   98.0  27.0  32.0  40.0  136.0  187.0   95.0
 120   84  110.0  29.0  32.0  42.0  135.0  188.0  100.0
 
 [121 rows x 8 columns],

... a lot of stuff here ...

      emg0   emg1  emg2  emg3   emg4   emg5   emg6   emg7
 3638  304  230.0  59.0  71.0  121.0  486.0  601.0  141.0
 3639  337  232.0  66.0  81.0  141.0  513.0  644.0  167.0
 3640  327  258.0  65.0  79.0  137.0  459.0  611.0  171.0
 3641  342  251.0  62.0  79.0  132.0  419.0  589.0  170.0
 3642  393  253.0  73.0  89.0  146.0  441.0  711.0  212.0
 ...   ...    ...   ...   ...    ...    ...    ...    ...
 3773  159  169.0  38.0  35.0   63.0  288.0  336.0   83.0
 3774  174  195.0  40.0  35.0   62.0  292.0  313.0   81.0
 3775  169  202.0  44.0  36.0   60.0  294.0  342.0   79.0
 3776  163  171.0  43.0  36.0   50.0  253.0  325.0   80.0
 3777  177  177.0  41.0  35.0   50.0  282.0  347.0   74.0
 
 [140 rows x 8 columns]]
```

Nice! Looks like a perfect split! Finally, let's wrap things up into one big summary DataFrame where each repeat is a single row:

```py
def read_repeats(file):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    bounds = df[np.isnan(df['emg1'])].index.insert(0,-1)
    repeats = [df.iloc[start+1:end] for start,end in zip(bounds, bounds[1:])]
    # This creates a new DataFrame with a single column ('emg') and one row for
    # each item (repeat) in our list
    return pd.DataFrame({'emg': repeats})
```

```py
>>> read_repeats('data/final/emgData-G0.csv')
                                                  emg
0       emg0   emg1  emg2  emg3  emg4   emg5   emg...
1       emg0   emg1  emg2  emg3  emg4   emg5   emg...
2       emg0   emg1  emg2  emg3  emg4   emg5   emg...
3       emg0   emg1   emg2  emg3   emg4   emg5   e...
4       emg0   emg1  emg2  emg3  emg4   emg5   emg...
5       emg0   emg1  emg2  emg3  emg4   emg5   emg...
6       emg0   emg1  emg2  emg3  emg4   emg5   emg...
7        emg0   emg1  emg2  emg3  emg4   emg5   em...
8        emg0   emg1  emg2  emg3  emg4   emg5   em...
9        emg0   emg1  emg2  emg3  emg4   emg5   em...
10       emg0   emg1   emg2  emg3   emg4   emg5   ...
11       emg0   emg1  emg2  emg3   emg4   emg5   e...
12       emg0   emg1  emg2  emg3   emg4   emg5   e...
13       emg0   emg1  emg2  emg3  emg4   emg5   em...
14       emg0   emg1  emg2  emg3   emg4   emg5   e...
15       emg0   emg1  emg2  emg3  emg4   emg5   em...
16       emg0   emg1   emg2  emg3   emg4   emg5   ...
17       emg0   emg1   emg2  emg3   emg4   emg5   ...
18       emg0   emg1  emg2  emg3  emg4   emg5   em...
19       emg0   emg1  emg2  emg3  emg4   emg5   em...
20       emg0   emg1  emg2  emg3  emg4   emg5   em...
21       emg0   emg1  emg2  emg3  emg4   emg5   em...
22       emg0   emg1  emg2  emg3  emg4   emg5   em...
23       emg0   emg1  emg2  emg3  emg4   emg5   em...
24       emg0   emg1  emg2  emg3  emg4   emg5   em...
25       emg0   emg1  emg2  emg3  emg4   emg5   em...
26       emg0   emg1   emg2  emg3  emg4   emg5   e...
27       emg0   emg1   emg2  emg3  emg4   emg5   e...
28       emg0   emg1   emg2  emg3  emg4   emg5   e...
29       emg0   emg1  emg2  emg3   emg4   emg5   e...
```

Err... That's not terribly helpful, is it? Let's take a bit of a closer look at what's happening here...

```py
>>> # Let's look at the first row of the 'emg' column using a "new" method
>>> read_repeats('data/final/emgData-G0.csv')['emg'][0]
    emg0   emg1  emg2  emg3  emg4   emg5   emg6   emg7
0     68  115.0  38.0  30.0  36.0   98.0  147.0   64.0
1     66  132.0  39.0  29.0  39.0   94.0  152.0   60.0
2     53  108.0  35.0  29.0  40.0   79.0  141.0   64.0
3     52  104.0  31.0  27.0  37.0   70.0  135.0   62.0
4     63  119.0  27.0  29.0  36.0   57.0  120.0   63.0
..   ...    ...   ...   ...   ...    ...    ...    ...
116   72  126.0  33.0  35.0  43.0  147.0  230.0  102.0
117   72   98.0  29.0  34.0  41.0  144.0  208.0  106.0
118   70  101.0  30.0  33.0  41.0  139.0  192.0  100.0
119   79   98.0  27.0  32.0  40.0  136.0  187.0   95.0
120   84  110.0  29.0  32.0  42.0  135.0  188.0  100.0
```

Ah, that makes a bit more sense – we've been nesting our DataFrames! While you can certainly make something like this work, there is little point in keeping the column names around (they don't mean anything to our machine-learning algorithms, which only look at the position of the data). Luckly, we can easily convert our DataFrame to a Numpy array – removing the superfluous nesting:

```py
def read_repeats(file):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    bounds = df[np.isnan(df['emg1'])].index.insert(0,-1)
    # The `.to_numpy()` method dumps the data of our DataFrame as a 2D array
    repeats = [df.iloc[start+1:end].to_numpy()
               for start,end in zip(bounds, bounds[1:])]
    return pd.DataFrame({'emg': repeats})
```

```py
>>> read_repeats('data/final/emgData-G0.csv')
                                                  emg
0   [[68, 115.0, 38.0, 30.0, 36.0, 98.0, 147.0, 64...
1   [[92, 122.0, 28.0, 37.0, 38.0, 98.0, 169.0, 91...
2   [[95, 160.0, 47.0, 46.0, 74.0, 281.0, 421.0, 1...
3   [[166, 175.0, 39.0, 48.0, 69.0, 363.0, 521.0, ...
4   [[149, 207.0, 67.0, 49.0, 67.0, 293.0, 474.0, ...
5   [[86, 126.0, 39.0, 43.0, 56.0, 193.0, 259.0, 1...
6   [[177, 246.0, 92.0, 42.0, 68.0, 279.0, 391.0, ...
7   [[101, 174.0, 37.0, 46.0, 61.0, 233.0, 282.0, ...
8   [[48, 147.0, 30.0, 35.0, 45.0, 113.0, 138.0, 5...
9   [[316, 100.0, 32.0, 33.0, 30.0, 76.0, 102.0, 1...
10  [[261, 102.0, 39.0, 37.0, 46.0, 76.0, 119.0, 2...
11  [[158, 188.0, 36.0, 40.0, 72.0, 285.0, 403.0, ...
12  [[205, 271.0, 75.0, 91.0, 164.0, 643.0, 803.0,...
13  [[147, 104.0, 29.0, 30.0, 27.0, 88.0, 118.0, 8...
14  [[106, 184.0, 41.0, 43.0, 73.0, 290.0, 478.0, ...
15  [[171, 206.0, 33.0, 36.0, 52.0, 204.0, 416.0, ...
16  [[226, 197.0, 55.0, 53.0, 75.0, 296.0, 397.0, ...
17  [[239, 245.0, 97.0, 74.0, 133.0, 620.0, 844.0,...
18  [[227, 176.0, 44.0, 49.0, 75.0, 319.0, 322.0, ...
19  [[270, 167.0, 45.0, 45.0, 57.0, 229.0, 353.0, ...
20  [[145, 166.0, 40.0, 37.0, 53.0, 204.0, 297.0, ...
21  [[54, 99.0, 46.0, 29.0, 40.0, 94.0, 135.0, 47....
22  [[155, 180.0, 42.0, 37.0, 55.0, 201.0, 316.0, ...
23  [[208, 171.0, 44.0, 55.0, 86.0, 371.0, 520.0, ...
24  [[84, 159.0, 35.0, 40.0, 59.0, 236.0, 422.0, 1...
25  [[198, 236.0, 61.0, 49.0, 69.0, 300.0, 475.0, ...
26  [[163, 140.0, 45.0, 43.0, 60.0, 214.0, 317.0, ...
27  [[154, 185.0, 70.0, 44.0, 67.0, 306.0, 471.0, ...
28  [[179, 236.0, 122.0, 47.0, 64.0, 245.0, 449.0,...
29  [[304, 230.0, 59.0, 71.0, 121.0, 486.0, 601.0,...
```

That's looking much better! We have 30 repeats of 2D EMG arrays! Oh, and just in case you'd like to see one of those arrays, here you are:

```py
>>> read_repeats('data/final/emgData-G0.csv')['emg'][0]
array([['68', 115.0, 38.0, 30.0, 36.0, 98.0, 147.0, 64.0],
       ['66', 132.0, 39.0, 29.0, 39.0, 94.0, 152.0, 60.0],
       ['53', 108.0, 35.0, 29.0, 40.0, 79.0, 141.0, 64.0],
       ['52', 104.0, 31.0, 27.0, 37.0, 70.0, 135.0, 62.0],
       ['63', 119.0, 27.0, 29.0, 36.0, 57.0, 120.0, 63.0],
        ... a lot of stuff here ...
       ['72', 126.0, 33.0, 35.0, 43.0, 147.0, 230.0, 102.0],
       ['72', 98.0, 29.0, 34.0, 41.0, 144.0, 208.0, 106.0],
       ['70', 101.0, 30.0, 33.0, 41.0, 139.0, 192.0, 100.0],
       ['79', 98.0, 27.0, 32.0, 40.0, 136.0, 187.0, 95.0],
       ['84', 110.0, 29.0, 32.0, 42.0, 135.0, 188.0, 100.0]], dtype=object)
```

Ruh roh... It's a good idea we took a bit of a closer look at that! It looks like, while most of our values are "floats" (numbers with a decimal point), our entire first column seems to be made of strings (as evidenced by the quotes)! Our ML algorithm certainly won't take kindly to that!

If you're curious why this is, it's because our original dataset did indeed contain strings in the first column! Remember our good friend "Repeat N"? This is yet another argument in favour of distinguishing between repeats in a different way. Learn from my mistakes!

There are a lot of ways to convert the types here, but the most convenient is probably reusing the `.to_numpy()` method we're already calling:

```py
def read_repeats(file):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    bounds = df[np.isnan(df['emg1'])].index.insert(0,-1)
    # The option `dtype` argument allows a Numpy datatype to be specified
    repeats = [df.iloc[start+1:end].to_numpy(dtype='int32')
               for start,end in zip(bounds, bounds[1:])]
    return pd.DataFrame({'emg': repeats})
```

```py
>>> read_repeats('data/final/emgData-G0.csv')['emg'][0]
array([[  68,  115,   38,   30,   36,   98,  147,   64],
       [  66,  132,   39,   29,   39,   94,  152,   60],
       [  53,  108,   35,   29,   40,   79,  141,   64],
       [  52,  104,   31,   27,   37,   70,  135,   62],
       [  63,  119,   27,   29,   36,   57,  120,   63],
        ... a lot of stuff here ...
       [  72,  126,   33,   35,   43,  147,  230,  102],
       [  72,   98,   29,   34,   41,  144,  208,  106],
       [  70,  101,   30,   33,   41,  139,  192,  100],
       [  79,   98,   27,   32,   40,  136,  187,   95],
       [  84,  110,   29,   32,   42,  135,  188,  100]], dtype=int32)
```

That looks much better! As a quick note before we go on though, let's break down `int32`. The first part, the `int` means we're converting to our values to integers (numbers *without* a decimal point). Why is that? Weren't they floats before? Indeed they were, but a closer inspection reveals that all of those "numbers with decimals" just had a `.0` at the end! Pandas loads numbers from CSV as floats by default, but we have no need for fractional values here.

Why bother converting to integers even if we don't need floats? Well, floats are limited in their precision on computers, so avoiding them wherever they aren't needed is usually a good idea. The loss of precision can be seen in the following expression, which should evaluate to 0 but doesn't:

```py
>>> 1 - (1/3) - (1/3) - (1/3)
1.1102230246251565e-16
```
The `32` in `int32` indicates that these are 32-bit integers. How values are represented in computer memory probably isn't something you're used to thinking about in Python (as the language normally hides these sorts of things), but Numpy is primarily programmed in a language called C (where you do need to think about how many bits you are using)! Long-story short, the 32 means that 32 1s-and-0s are used to represent your integer, giving you 2^32 = 4294967296 possible values.

Tangents aside, let's think about the last thing we need to add to our `read_repeats()` function! Currently, we're just loading one file (and consequentially one gesture) at a time, but we'll be extending things shortly to load all of the gestures at once. To keep track of which gesture each repeat belongs to, let's add a new column called `gesture` to the result of `rand_repeats()` – for now, we'll just pass the gesture number as an argument to the function:

```py
# Our final `read_repeats()` function
def read_repeats(file, gesture):
    df = pd.read_csv(file).loc[:, 'emg0':'emg7']
    bounds = df[np.isnan(df['emg1'])].index.insert(0,-1)
    repeats = [df.iloc[start+1:end].to_numpy(dtype='int32')
               for start,end in zip(bounds, bounds[1:])]
    # We've added a second column containing the gesture name / number
    return pd.DataFrame({'emg': repeats, 'gesture': gesture})
```

```py
>>> # Note we are calling `read_repeats()` with a second argument now
>>> read_repeats('data/final/emgData-G0.csv', 0)
                                                  emg  gesture
0   [[68, 115, 38, 30, 36, 98, 147, 64], [66, 132,...        0
1   [[92, 122, 28, 37, 38, 98, 169, 91], [94, 119,...        0
2   [[95, 160, 47, 46, 74, 281, 421, 107], [87, 16...        0
3   [[166, 175, 39, 48, 69, 363, 521, 137], [168, ...        0
4   [[149, 207, 67, 49, 67, 293, 474, 99], [166, 1...        0
5   [[86, 126, 39, 43, 56, 193, 259, 126], [72, 12...        0
6   [[177, 246, 92, 42, 68, 279, 391, 115], [172, ...        0
7   [[101, 174, 37, 46, 61, 233, 282, 104], [102, ...        0
8   [[48, 147, 30, 35, 45, 113, 138, 59], [47, 153...        0
9   [[316, 100, 32, 33, 30, 76, 102, 177], [319, 1...        0
10  [[261, 102, 39, 37, 46, 76, 119, 285], [284, 1...        0
11  [[158, 188, 36, 40, 72, 285, 403, 104], [125, ...        0
12  [[205, 271, 75, 91, 164, 643, 803, 187], [208,...        0
13  [[147, 104, 29, 30, 27, 88, 118, 82], [146, 10...        0
14  [[106, 184, 41, 43, 73, 290, 478, 113], [110, ...        0
15  [[171, 206, 33, 36, 52, 204, 416, 101], [170, ...        0
16  [[226, 197, 55, 53, 75, 296, 397, 127], [160, ...        0
17  [[239, 245, 97, 74, 133, 620, 844, 221], [220,...        0
18  [[227, 176, 44, 49, 75, 319, 322, 85], [226, 1...        0
19  [[270, 167, 45, 45, 57, 229, 353, 101], [256, ...        0
20  [[145, 166, 40, 37, 53, 204, 297, 86], [133, 1...        0
21  [[54, 99, 46, 29, 40, 94, 135, 47], [47, 100, ...        0
22  [[155, 180, 42, 37, 55, 201, 316, 103], [195, ...        0
23  [[208, 171, 44, 55, 86, 371, 520, 139], [199, ...        0
24  [[84, 159, 35, 40, 59, 236, 422, 101], [74, 16...        0
25  [[198, 236, 61, 49, 69, 300, 475, 105], [212, ...        0
26  [[163, 140, 45, 43, 60, 214, 317, 108], [138, ...        0
27  [[154, 185, 70, 44, 67, 306, 471, 97], [152, 1...        0
28  [[179, 236, 122, 47, 64, 245, 449, 89], [178, ...        0
29  [[304, 230, 59, 71, 121, 486, 601, 141], [337,...        0
```

Looking good! 30 repeats of well-formatted EMG data and the gesture they come from! Next up is loading all of the gestures at once!

## Loading All Gestures

Here is where the `os` and `re` imports from earlier come into play! First up, let's define a new function called `load_emg_from_folder()` and list all of the files in the data directory.

```py
def load_emg_from_folder(path):
    # `listdir()` from the `os` module will list all files in a directory
    return os.listdir(path)
```

```py
>>> load_emg_from_folder('data/final')
['gestures.txt',
 'emgData-G0.csv',
 'emgData-G1.csv',
 'emgData-G2.csv',
 'emgData-G4.csv',
 'emgData-G5.csv',
 'emgData-G6.csv',
 'emgData-G7.csv',
 'emgData-G3.csv']
```

That's a good start! It's listing all of the files in the `data/final` directory (including the `gestures.txt` file which gives each gesture an English name so we could keep track of which was which). While `gestures.txt` is helpful to keep around for us programmers, it's not really something we need to load into Python, so the first thing to do is filter that out! This is where regex comes into play!

```py
def load_emg_from_folder(path):
    # `match` returns whether something matches the given regex pattern or not
    # The list comprehension then only includes the files that did match
    return [file for file in os.listdir(path)
            if re.match(r'emgData-G(\d).csv', file)]
```

Hmm, okay, before we test that out, it's worth explaining what is going on with that whole `r'emgData-G(\d).csv'` thing.

First of all, strings preceded by `r` are "raw" strings. In normal Python strings, backslashes (`\`) have a special meaning – they are used to include some whitespace characters like a newline (`\n`), tab (`\t`), etc. This also means that, in normal strings, if you want to include a literal backslash, you need to "escape" that with a second backslash, like this: `\\`. Since regex makes heavy use of `\`, it's preferable to use "raw" strings that don't assign any special meaning to backslashes. If we wanted to write the simple regex above using a regular Python string, we would need to write `'emgData-G(\\d).csv'`.

Just to drive the point home:

```py
>>> print('This is one line\nThis is the next!')
```

Prints:

```
This is one line
This is the next!
```

But

```py
>>> print(r'This is one line\nThis is the next!')
```

Prints:

```
This is one line\nThis is the next!
```

As for the rest of the regex, the `emgData-G` and `.csv` bits just match like a literal search string, requiring every match to share that general structure, and the `\d` will match any numerical digit (0-7 in our case). The parentheses around `\d` form something called a "capture group", which can be used to extract specific parts of a match; here we are interested in capturing the digit, as that tells us which gesture the file contains. More on capture groups in a moment, first we need to see if our regex has worked:

```py
>>> load_emg_from_folder('data/final')
['emgData-G0.csv',
 'emgData-G1.csv',
 'emgData-G2.csv',
 'emgData-G4.csv',
 'emgData-G5.csv',
 'emgData-G6.csv',
 'emgData-G7.csv',
 'emgData-G3.csv']
```

Looking good! We've got all of the same files as before, but `gestures.txt` has been filtered out! The next step is extracting the gesture number, let's take a quick look at `re.match()` in isolation.

```py
>>> # When there is no match, nothing is returned (a false result)
>>> re.match(r'emgData-G(\d).csv', 'gestures.txt')
>>> # When there is a match, a match object is returned
>>> re.match(r'emgData-G(\d).csv', 'emgData-G0.csv')
<re.Match object; span=(0, 14), match='emgData-G0.csv'>
```

That's interesting! Python treats no return as a `False` result and the `re.Match` object as `True`. The `re.Match` object shows us a little bit of information by default, but let's take a look at something we are more interested in: the capture groups!

```py
>>> # `.groups()` returns strings for all of the captured groups in the match
>>> re.match(r'emgData-G(\d).csv', 'emgData-G0.csv').groups()
('0',)
```

It looks like Python is returning a tuple of all of the captured groups (you can have as many as you'd like in any given regex) and it contains our digit `'0'`! We can index tuples just like lists and get the first capture group all on its own:

```py
>>> # Getting just the first group...
>>> re.match(r'emgData-G(\d).csv', 'emgData-G0.csv').groups()[0]
'0'
```

Let's take a bit of a leap and try to load all of our gestures in one go (don't worry, they'll be a full explanation right after):

```py
def load_emg_from_folder(path):
    # We're now calling `rand_repeats` on each file and naming the gesture
    # according to the captured digit from our regex
    return [read_repeats(file, matches.groups()[0])
            for file in os.listdir(path)
            # We've also changed this line to store the `re.Match` object
            # in the `matches` variable
            if (matches := re.match(r'emgData-G(\d).csv', file))]
```

Okay, there are a couple of changes to talk about there – the biggest being the introduction of the [walrus operator](https://realpython.com/lessons/assignment-expressions/) (`:=`). In short, this operator allows us to assign to a variable and return the new value in a single step. In an example:

```py
>>> # Assignment outside of an expression is fine
>>> walrus = False
>>> print(walrus)
False
>>> not walrus
True
```

```py
>>> # Normal assignment inside of an expression is not allowed
>>> print(walrus = True)
TypeError: 'walrus' is an invalid keyword argument for print()
```

```py
>>> # The walrus operator is the solution!
>>> print(walrus := True)
True
>>> not walrus
False
```

After we've saved any capture groups in the `matches` variable, we can pass then to `read_repeats()`! Let's see how that code from earlier actually works:

```py
>>> load_emg_from_folder('data/final')
FileNotFoundError: [Errno 2] No such file or directory: 'emgData-G0.csv'
```

Ruh roh round two... That's not quite right! It seems that Python can't find the file that we're passing to `read_repeats()`! Fortunately though, this is something to be expected. We're listing the files in the `data/final` folder, but `read_repeats()` is still looking for things in the current directory. It looks like we'll have to combine our path and file names! Luckily, the `os` module has just what we need:

```py
def load_emg_from_folder(path):
    # `os.path.join()` sticks together filepaths while avoiding all of the
    # pitfalls inherent in building paths (do you need to join things with
    # a `/`? Maybe you'll need to use a `\` on Windows?)
    return [read_repeats(os.path.join(path, file), matches.groups()[0])
            for file in os.listdir(path)
            if (matches := re.match(r'emgData-G(\d).csv', file))]
```

```py
>>> load_emg_from_folder('data/final')
[                                                  emg gesture
 0   [[68, 115, 38, 30, 36, 98, 147, 64], [66, 132,...       0
 1   [[92, 122, 28, 37, 38, 98, 169, 91], [94, 119,...       0
 2   [[95, 160, 47, 46, 74, 281, 421, 107], [87, 16...       0
 3   [[166, 175, 39, 48, 69, 363, 521, 137], [168, ...       0
 4   [[149, 207, 67, 49, 67, 293, 474, 99], [166, 1...       0
     ... a lot of stuff here ...
 25  [[83, 116, 25, 32, 36, 79, 103, 69], [68, 98, ...       3
 26  [[27, 68, 20, 20, 22, 32, 55, 37], [37, 73, 22...       3
 27  [[91, 128, 38, 34, 26, 36, 74, 66], [92, 118, ...       3
 28  [[47, 46, 49, 31, 27, 31, 64, 58], [46, 62, 36...       3
 29  [[26, 19, 22, 23, 20, 26, 45, 36], [27, 19, 22...       3]
```

Excellent! It looks like we have a list of DataFrames, each containing all of the repeats for a single gesture (as you can see from the 'gesture' column). Now all that's left to do is to stick all of these together into a single DataFrame! Luckily, Pandas has our back again:

```py
def load_emg_from_folder(path):
    all_gestures = [read_repeats(os.path.join(path, file), matches.groups()[0])
                    for file in os.listdir(path)
                    if (matches := re.match(r'emgData-G(\d).csv', file))]
    # `concat()` takes a list of DataFrames and squishes all of them into one
    return pd.concat(all_gestures)
```

```py
>>> load_emg_from_folder('data/final')
                                                  emg gesture
0   [[68, 115, 38, 30, 36, 98, 147, 64], [66, 132,...       0
1   [[92, 122, 28, 37, 38, 98, 169, 91], [94, 119,...       0
2   [[95, 160, 47, 46, 74, 281, 421, 107], [87, 16...       0
3   [[166, 175, 39, 48, 69, 363, 521, 137], [168, ...       0
4   [[149, 207, 67, 49, 67, 293, 474, 99], [166, 1...       0
..                                                ...     ...
25  [[83, 116, 25, 32, 36, 79, 103, 69], [68, 98, ...       3
26  [[27, 68, 20, 20, 22, 32, 55, 37], [37, 73, 22...       3
27  [[91, 128, 38, 34, 26, 36, 74, 66], [92, 118, ...       3
28  [[47, 46, 49, 31, 27, 31, 64, 58], [46, 62, 36...       3
29  [[26, 19, 22, 23, 20, 26, 45, 36], [27, 19, 22...       3

[231 rows x 2 columns]
```

Overall this is looking really good! We have 231 rows (showing that all of our repeats from all eight gestures were loaded in) and two columns (for 'emg' and 'gesture' data). There's just one small problem: our indexes only seem to go as high as 29. Let's take a closer look at one of the gesture boundaries:

```py
>>> load_emg_from_folder('data/final')[25:35]
                                                  emg gesture
25  [[198, 236, 61, 49, 69, 300, 475, 105], [212, ...       0
26  [[163, 140, 45, 43, 60, 214, 317, 108], [138, ...       0
27  [[154, 185, 70, 44, 67, 306, 471, 97], [152, 1...       0
28  [[179, 236, 122, 47, 64, 245, 449, 89], [178, ...       0
29  [[304, 230, 59, 71, 121, 486, 601, 141], [337,...       0
0   [[21, 23, 19, 24, 25, 29, 68, 61], [21, 23, 18...       1
1   [[59, 108, 43, 45, 71, 204, 314, 125], [56, 11...       1
2   [[51, 108, 42, 41, 72, 322, 387, 106], [53, 12...       1
3   [[52, 90, 47, 37, 65, 297, 408, 116], [53, 86,...       1
4   [[62, 122, 52, 42, 56, 204, 315, 80], [64, 122...       1
```

Oh dear! When we called `pd.concat()`, it looks like it kept all of the old indices, so it restarts our numbering every 30 rows! It's often helpful to keep the original indices, especially if they were strings, but in this case we can renumber things by passing an additional argument to `pd.concat()` asking it to ignore the original indices:

```py
def load_emg_from_folder(path):
    all_gestures = [read_repeats(os.path.join(path, file), matches.groups()[0])
                    for file in os.listdir(path)
                    if (matches := re.match(r'emgData-G(\d).csv', file))]
    # `ignore_index` throws away the original indices and renumbers the rows
    return pd.concat(all_gestures, ignore_index=True)
```

```py
>>> load_emg_from_folder('data/final')
                                                   emg gesture
0    [[68, 115, 38, 30, 36, 98, 147, 64], [66, 132,...       0
1    [[92, 122, 28, 37, 38, 98, 169, 91], [94, 119,...       0
2    [[95, 160, 47, 46, 74, 281, 421, 107], [87, 16...       0
3    [[166, 175, 39, 48, 69, 363, 521, 137], [168, ...       0
4    [[149, 207, 67, 49, 67, 293, 474, 99], [166, 1...       0
..                                                 ...     ...
226  [[83, 116, 25, 32, 36, 79, 103, 69], [68, 98, ...       3
227  [[27, 68, 20, 20, 22, 32, 55, 37], [37, 73, 22...       3
228  [[91, 128, 38, 34, 26, 36, 74, 66], [92, 118, ...       3
229  [[47, 46, 49, 31, 27, 31, 64, 58], [46, 62, 36...       3
230  [[26, 19, 22, 23, 20, 26, 45, 36], [27, 19, 22...       3

[231 rows x 2 columns]
```

Aced it! Now things are numbered correctly and we can move on to some finishing touches!

## Shaping Our EMG Data

Since most machine-learning algorithms rely on inputs being the same size every time (this enables vectorization optimizations), we'll need to make sure all of our repeats are the same size. We can break this down into two main steps:

1. Find the median length of all of our repeats
2. Pad or truncate all repeats so that they are the same (median) size

To do this, we'll need a couple more imports:

```py
from tensorflow.keras.preprocessing.sequence import pad_sequences
from statistics import median
```

Dealing with those in reverse-order: `median` does what it says on the tin and finds the median of some numbers. `pad_sequences` is a special function from our machine-learning library, TensorFlow, that will truncate a sequence if it's too long, or pad it with some number (zero by default) if it's too short.

Let's start by trying to get the shape of our EMG repeats:

```py
# Takes a DataFrame from either `read_repeats()` or `load_emg_from_folder()`
def shape_ml_data(df):
    # Returns the shape (dimensions) of every array in the 'emg' column
    return [x.shape for x in df['emg']]
```

```py
>>> df = load_emg_from_folder('data/final')
>>> shape_ml_data(df)
[(121, 8),
 (114, 8),
 (119, 8),
 (121, 8),
 (143, 8),
  ...
 (115, 8),
 (119, 8),
 (125, 8),
 (114, 8),
 (115, 8)]
```

We can see that all of our EMG data is 8 "wide" (which corresponds to the 8 EMG channels of the Myo), but it's length certainly varies! We can pick out just the first of these shape values (since the 8 never changes), then find the median as follows:

```py
def shape_ml_data(df):
    # Indexing the shape tuple and calculating the median from the list
    return median([x.shape[0] for x in df['emg']])
```

```py
>>> shape_ml_data(df)
118
```

That looks great, we can pad / trim all of our sequences to be 118 long! But there is a hidden danger lurking here (since Python makes no guarantee about the types returned by a function). Let's see what happens if we have an even number of repeats:

```py
>>> # We currently have an odd number of repeats (231)
>>> df.shape
(231, 2)
>>> # We can drop one by slicing the DataFrame
>>> df[1:].shape
(230, 2)
>>> # Then recalculate the median
>>> shape_ml_data(df[1:])
117.5
```

Oh dear... Because of the way the median is calculated, even-length datasets involve the averaging of the "middle" values, potentially resulting in a non-integer result. Normally this is fine, but in our case it makes no sense to pad our EMG sequence to some half-value. We can simply convert back to an integer using the `int()` function:

```py
def shape_ml_data(df):
    # `int()` ensures that the result is a whole number
    return int(median([x.shape[0] for x in df['emg']]))
```

```py
>>> shape_ml_data(df[1:])
117
```

Much better! Now lets actually pad and trim things using `pad_sequences()`:

```py
def shape_ml_data(df):
    middle = int(median([x.shape[0] for x in df['emg']]))
    # Generate a list of our repeats with each padded to the `maxlen`
    return [pad_sequences(x, maxlen = middle) for x in df['emg']]
```

Now we'll look at the shape of our first few repeats:

```py
>>> [x.shape for x in shape_ml_data(df)[:5]]
[(121, 118), (114, 118), (119, 118), (121, 118), (143, 118)]
```

Oh dear... That's not quite what we were looking for... It looks like it's padded the wrong dimension! Somewhat annoyingly, at the time of writing, `pad_sequences()` does not allow the user to specify an "axis" to pad along, so we'll need to do something a little tricky: we can transpose the array (swapping the length and width), pad the transposed array, then transpose the result to get back to our original shape. Let's play around with that idea a little:

```py
>>> # Looking at how transposing (`.T`) affects shape
>>> df['emg'][0].shape
(121, 8)
>>> df['emg'][0].T.shape
(8, 121)
>>> # Transposing, padding, and untransposing:
>>> emg = df['emg'][0]
>>> pad_sequences(emg.T, maxlen = 118).T.shape
(118, 8)
```

Let's apply this to our function:

```py
def shape_ml_data(df):
    middle = int(median([x.shape[0] for x in df['emg']]))
    # Applying the transpose trick!
    return [pad_sequences(x.T, maxlen = middle).T for x in df['emg']]
```

Again looking at the shape of our first few repeats:

```py
>>> [x.shape for x in shape_ml_data(df)[:5]]
[(118, 8), (118, 8), (118, 8), (118, 8), (118, 8)]
```

That's looking much better! As a final processing step for the EMG data, we can take this list of 2D Numpy arrays and just turn it into a single 3D Numpy array (which is something we can directly feed into TensorFlow).

```py
def shape_ml_data(df):
    middle = int(median([x.shape[0] for x in df['emg']]))
    # `stack` turns a list of 2D arrays into a single 3D array
    return np.stack([pad_sequences(x.T, maxlen = middle).T for x in df['emg']])
```

```py
>>> shape_ml_data(df).shape
(231, 118, 8)
```

And that's that! 231 repeats of EMG data, each 118 values long with data from 8 channels. The EMG data is all ready to go, but we've got one last thing to do with our gesture classifications!

# One-Hot Encoding Our Gestures

When tackling a categorization problem, a single, continuous output variable (like our 'gesture' column, which ranges from 0-7) can cause some issues. No ML algorithm can ever be 100% certain about its classification, so outputs are often expressed as probabilities on a sliding-scale. In this case then, you could interpret a 6.5 as some gesture that looks half like gesture 6 and half like gesture 7.

That works fine in the simplest case, but what if some gesture looks half like 3 and half like 7? Do you then output something in the middle like 5? How do you distinguish this from something that just looks like gesture 5 on it's own?

We can resolve this ambiguity by using something called one-hot encoding. This encoding creates a new column for every value that the 'gesture' column holds – turning our single 'gesture' column into eight. Each of these columns then holds either a 1 or a 0, indicating which gesture the repeat belongs to. This can be thought of as a probability distribution where a 1 in the 'gesture_4' column indicates a given repeat unambiguously belongs to gesture 4.

Let's one-hot encode our 'gesture' column using a purpose-built function from Pandas:

```py
>>> # First we'll try a slicing with a step, so we can see things clearly
>>> df['gesture'][::30]
0      0
30     1
60     2
90     4
120    5
150    6
180    7
210    3
Name: gesture, dtype: object
>>> # Now let's try one-hot encoding (identical in this case to dummy encoding)
>>> pd.get_dummies(df['gesture'][::30])
     0  1  2  3  4  5  6  7
0    1  0  0  0  0  0  0  0
30   0  1  0  0  0  0  0  0
60   0  0  1  0  0  0  0  0
90   0  0  0  0  1  0  0  0
120  0  0  0  0  0  1  0  0
150  0  0  0  0  0  0  1  0
180  0  0  0  0  0  0  0  1
210  0  0  0  1  0  0  0  0
>>> # Finally, we can encode the whole column and get just the values
>>> pd.get_dummies(df['gesture']).values
array([[1, 0, 0, ..., 0, 0, 0],
       [1, 0, 0, ..., 0, 0, 0],
       [1, 0, 0, ..., 0, 0, 0],
       ...,
       [0, 0, 0, ..., 0, 0, 0],
       [0, 0, 0, ..., 0, 0, 0],
       [0, 0, 0, ..., 0, 0, 0]], dtype=uint8)
```

Perfect! We now have an unambiguous way to represent which gesture is which (even when the ML algorithm is a little unsure about things). Better yet, this one is a Numpy array just like our processed EMG data – ready to be fed directly into TensorFlow.

Finally, let's wrap this into our `shape_ml_data` function and return both the EMG data and gesture data together in a tuple:

```py
def shape_ml_data(df):
    middle = int(median([x.shape[0] for x in df['emg']]))
    data = np.stack([pad_sequences(x.T, maxlen = middle).T for x in df['emg']])
    labels = pd.get_dummies(df['gesture']).values
    # Returning things as a tuple allows us to destructure them easily later
    return (data, labels)
```

We can check that our shapes are in order by running the following:

```py
>>> emg, gestures = shape_ml_data(df)
>>> emg.shape
(231, 118, 8)
>>> gestures.shape
(231, 8)
```

And there we are! All done! I'll leave you with a couple of final notes then get out of here.

## Parting Notes

Sometimes ML requires a lot of data preprocessing, but you can greatly reduce the amount that's needed by changing the way your data is collected. As I mentioned before, recording all of our data in a single file with two additional columns to track which repeat and gesture each row of data belonged to would have vastly simplified the preprocessing process.

With that being said, you're not always in control of data collection (if you'd just downloaded a dataset from the internet, for example), so these sort of processing skills are important to have.

That's all for this session, but I'll see you in the next session where we'll be building some deep-learning networks with TensorFlow and exploring the differences between LSTMs and CNNs!
