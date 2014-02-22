---
layout: post
title: "C# Sorting Woes"
date: 2014-02-16 16:22:30 -0500
comments: true
categories: [Algorithms, C#, Sorting]
---

I was having some fun implementing sorting algorithms and thought it might be interesting to compare my implementations against the C# built in `List<T>.Sort`. That was where the rabbit hole started. 

### Slow Quicksort
After finishing an implementation of quicksort I decided to give it a try on some big input. I wrote something to generate integer lists of arbitrary size with uniformly random elements. I ran some benchmarks, and then ran the same input against `List<T>.Sort`. My implementation was about <b>ten times</b> slower than the built in sort. I thought I must be doing something wrong. There must be some bug in my code. After all, the description of `List<T>.Sort` states that (from [here](http://msdn.microsoft.com/en-us/library/b0zbh7b6%28v=vs.110%29.aspx))

* If the partition size is fewer than 16 elements, it uses an insertion sort algorithm.
* If the number of partitions exceeds 2 * LogN, where N is the range of the input array, it uses a Heapsort algorithm.
* Otherwise, it uses a Quicksort algorithm.

So the built-in sorting should be using Quicksort *most* of the time. I thought my implementation might just be slow, so I started to make some optimizations. My base implementation can be found here:<br>
[https://github.com/mattnedrich/algorithms/blob/master/csharp/sorting/quicksort/Quicksorter.cs](https://github.com/mattnedrich/algorithms/blob/master/csharp/sorting/quicksort/Quicksorter.cs)

First I tried to in-line all of the function calls (e.g., <code>ChoosePivotIndex</code> and <code>Swap</code>). This yielded some performance improvement (about 10%). Next I played around with the pivot selection method. I was using a naive random pivot selection so I tried a median of three approach. This actually didn't do much, though I suspect it is because my input was uniformly random. I tried parallelizing the calls to <code>Partition</code>. This actually slowed things down (probably due to the overhead of thread creation). Lastly, I cheated a bit and switched to insertion sort for small inputs, as the library sort does. This yielded the latest performance gain of around 20%. However, after all of my optimization, I was still around eight times slower than `List<T>.Sort`.

I was stuck, so I posted [this](http://stackoverflow.com/questions/21818889/why-is-my-c-sharp-quicksort-implementation-significantly-slower-than-listt-sor), and learned that the .NET Framework special cases the sorting of built-in types (ints, string, etc). Just how much of a performance gain is achieved by doing this? I wanted to find out so I set up yet another experiment.

### Int vs. IntInWrapper
I compared the performance of `List<T>.Sort` when using plain ol' <code>int</code>'s to the performance when using <code>int</code>'s in a wrapper class. For the wrapper class I used:

{% coderay lang:java %}
public class IntInWrapper : IComparable<IntInWrapper>
{
    public int Val { get; set; }
    public IntInWrapper(int val)
    {
        Val = val;
    }

    public int CompareTo(IntInWrapper other)
    {
        if (this.Val < other.Val)
            return -1;
        else if (this.Val == other.Val)
            return 0;
        else
            return 1;
    }
}
{% endcoderay %}

I benchmarked sorting Lists of various sizes for the two input classes. Results are shown below. For each List size, sorting was performed ten times (each time on a different randomized list), and the average execution time was kept.

<img width="450px" src="{{ root_url }}/images/c_sharp_sorting/ListT.png"/>

Sorting on wrapped `int`'s drastically degrades performance compared to plain  `int`'s. From my understanding, this is largely due to the `IComparable<T>.CompareTo` function pointer being used for element comparison, rather than a simple <code>></code> or <code><</code> (which likely trickles down to a single assembly instruction). Anyway, the key learning from this is that use built-in types when you need sorting performance and be aware of the penalty associated with using custom types! 
