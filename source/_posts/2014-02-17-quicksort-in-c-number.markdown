---
layout: post
title: "Quicksort in C#"
date: 2014-02-17 13:36:31 -0500
comments: true
categories: [Algorithms, Sorting, C#]
---

I have written an implementation of quicksort in C#. It can be found here.<br>
[https://github.com/mattnedrich/algorithms/blob/master/csharp/sorting/quicksort/Quicksorter.cs](https://github.com/mattnedrich/algorithms/blob/master/csharp/sorting/quicksort/Quicksorter.cs)

It operates on anything that implements `IComparable<T>`. Suppose you have a class `SortableObject` that implements `IComparable<SortableObject>`. The usage model in this scenario would be

{% coderay lang:java%}
List<SortableObject> items = ...
Quicksorter<SortableObject> sorter = new Quicksorter<SortableObject>();
sorter.Sort(items);
{% endcoderay %}

My implementation contains a main `Partition` method that is called recursively to perform the sorting. In addition, there are two helper methods. The first, `ChoosePivotIndex` is responsible for choosing a pivot element from a list to be sorted. The current implementation naively selects a pivot at random. This method is virtual to keep it [Open/Closed](http://en.wikipedia.org/wiki/Open/closed_principle) and allow others to extend it with a better pivot selection method if desired. The other method, `Swap`, swaps two elements in a list. I've found that in-lining the swapping yields faster performance, though I decided not to do so for clarity.

I benchmarked my implementation on different types of input of varying length. Results below.

<img width="425px" src="{{ root_url }}/images/quicksort_csharp/quicksort_perf.png"/>

As displayed, my implementation (even with the naive pivot selection) is fairly robust to random, sorted, and reverse sorted lists.
