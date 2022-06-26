---
title : Pytest fixtures - introduction
notetype : feed
date : 26-06-2022
---

# Introduction

Pytest is a test framework for Python. In this post, I will provide a very quick introduction to the library and discuss the details of it's `autouse` and `parametrize` features.

# Fixtures Introduction
Fixture initialize test functions so that test become more modular, scalable and with easier teardown procedures. 
## Testing Sorting Algorithms with fixtures
I implemented a number of sorting algorithms, and needed to test them on the same lists. This is one of the sample algorithms, `BubbleSort`. _I changed the code a bit as compared to the repo - some elements of the code (numbering of the test, object naming convention) would only be confusing out of context._

```python
from sort.AbstractSort import AbstractSort

class BubbleSort(AbstractSort):  
    @staticmethod  
    def swap(arr, i, j):  
        arr[i], arr[j] = arr[j], arr[i]  
  
    def _sort(self, arr):  
        for i in range(len(arr) - 1):  
            for j in range(len(arr) - 1):  
                if arr[j] > arr[j + 1]:  
                    BubbleSort.swap(arr, j, j + 1)  
        return arr
	
```

## Requirements for the tests
A single test would initialize `BubbleSort` class, pass an unsorted array to it, and compared it against a sorted version of that array. I would also like to record statistics about the test run - I would like to record and compare runtimes of algorithms.

I would like for all of my algorithms to run on the same unsorted arrays. Furthermore, since I have to test if they were sorted correctly, I want to perform the _canonical sort_ (sort with standard python `sorted` method) only once.

## The baseline code
```python
import random
from search.BubbleSort import BubbleSort
import math

def test_bubble_sort():
	# random seed, to ensure consistency across tests
	random.seed(42)
	# generate an array of lenght 2^5 
	# of random integers x such that 0 <= x < 2^5 
	arr = [random.randint(0,len(math.pow(2,5))) for _ in range(len(math.pow(2,5)))]
	# sort this array
	canonical_sorted_arr = sorted(arr)
	# initialize the bubbleSort class
	bubble_sort = BubbleSort()
	
	# sort
    arr_sorted, time_elapsed = bubble_sort.sort_and_time(arr)  
    # compare
	assert arr_sorted == canonical_sorted_arr

```

## What is wrong with this code?
This code has following issues:
1. Lack of separation of AAA (Arrange, Act and Assert)
2. Code initializing the array within the test - should be separated and reused
3. Code sorting the array within the test - should be separated and reused, especially because sorting the array might take significant time if N is large.
4. Hardcoded values into the code - random seed, lenght of the array

# Fixing the code
##  Initialize the array and algorithm instance in fixtures
We should move the array initialization to a fixture.

```python
import random
from search.BubbleSort import BubbleSort
import math
import pytest


@pytest.fixture
def bubble_sort():
	bubble_sort = BubbleSort()
	return bubble_sort

	
@pytest.fixture
def arr():
	random.seed(42)
	arr = [random.randint(0,len(math.pow(2,5))) for _ in range(len(math.pow(2,5)))]
	return arr

def test_bubble_sort(arr, bubble_sort):
	canonical_sorted_arr = sorted(arr)
	
    arr_sorted, time_elapsed = bubble_sort.sort_and_time(arr)  
	assert arr_sorted == canonical_sorted_arr

```

Fixture is a function decorated with `@pytest.fixture`. Those functions can be used as parameters (_requested_) by all pytest tests in a class. When fixture is requested by a test, it's return / yield value is really returned.

##  Sort the array in a fixture
And set the scope to `session` so that the sort operation is performed only once.

```python
import random
from search.BubbleSort import BubbleSort
import math
import pytest

@pytest.fixture
def bubble_sort():
	bubble_sort = BubbleSort()
	return bubble_sort

	
@pytest.fixture
def arr():
	random.seed(42)
	arr = [random.randint(0,len(math.pow(2,5))) for _ in range(len(math.pow(2,5)))]
	return arr

@pytest.fixture(scope="session")
def canonical_sorted_arr(arr)
	return sorted(arr)


def test_bubble_sort(arr, canonical_sorted_arr, bubble_sort):
    arr_sorted, time_elapsed = bubble_sort.sort_and_time(arr)  
	assert arr_sorted == canonical_sorted_arr

```
