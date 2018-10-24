---
title: Quick-Select
description: Except for quicksort, Tony Hoare also writes algorithm for linear time selection of numbers in unsorted array.
categories:
 - notes
tags: quicksort, quickselect, median
---

> Linear Time Median Finding

<!-- more -->

## Simplest Median

The most straightforward way to find the median is to sort the list and just pick the median by its index. The fastest comparison-based sort is `O(nlogn)`, so that dominates the runtime

```python
def nlogn_median(array_like):
    l = sorted(array_like)
    if len(l) % 2 == 1:
        return l[len(l) / 2]
    else:
        return 0.5 * (l[len(l) / 2 - 1] + l[len(l) / 2])
```

Although this algorithm is the fastest to implement, its efficiency is limited by the sorting step.

## Finding Median in average of `O(n)`

This **quickselect** algorithm runs recursively in finding any number of order k in a given list.

### Steps

1. Pick an index in the list. It doesn’t matter how you pick it, but choosing one at random works well in practice. The element at this index is called the **pivot**.
2. Split the list into 2 groups:
	1. Elements less than or equal to the pivot, `lesser_elems`
	2. Elements strictly greater than the pivot, `greater_elems`
3. We know that one of these groups contains the median. Suppose we’re looking for the kth element:
	- If there are k or more elements in `lesser_elems`, recurse on list `lesser_elems`, searching for the kth element.
	- If there are fewer than k elements in `lesser_elems`, recurse on list `greater_elems`. Instead of searching for k, we search for `k-len(lesser_elems)`.

### Quickselect for Median

Therefore, once we know the length of the given array, we can use quickselect to find the number in the middle.

```python
import random
def quickselect_median(l, pivot_fn=random.choice):
    if len(l) % 2 == 1:
        return quickselect(l, len(l) / 2, pivot_fn)
    else:
        return 0.5 * (quickselect(l, len(l) / 2 - 1, pivot_fn) +
                      quickselect(l, len(l) / 2, pivot_fn))


def quickselect(l, k, pivot_fn):
    """
    Select the kth element in l (0 based)
    :param l: List of numerics
    :param k: Index
    :param pivot_fn: Function to choose a pivot, defaults to random.choice
    :return: The kth element of l
    """
    if len(l) == 1:
        assert k == 0
        return l[0]

    pivot = pivot_fn(l)

    lows = [el for el in l if el < pivot]
    highs = [el for el in l if el > pivot]
    pivots = [el for el in l if el == pivot]

    if k < len(lows):
        return quickselect(lows, k, pivot_fn)
    elif k < len(lows) + len(pivots):
        # We got lucky and guessed the median
        return pivots[0]
    else:
        return quickselect(highs, k - len(lows) - len(pivots), pivot_fn)
```
