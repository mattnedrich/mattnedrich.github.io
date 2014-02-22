---
layout: post
title: "Benchmarking C# Code"
date: 2014-02-07 00:01:49 -0500
comments: true
categories: [C#, Benchmark, Tools]
---

I occasionally need to benchmark some code. In C#, this can easily be done using the Stopwatch class. After writing the same boilerplate Stopwatch wrapper enough times I decided to throw together a simple Benchmarker class.

{% coderay lang:java %}
public static class Benchmarker
{
    public static TimeSpan RunOnce(Action action)
    {
        Stopwatch stopwatch = Stopwatch.StartNew();
        action();
        stopwatch.Stop();
        return stopwatch.Elapsed;
    }
    
    public static IEnumerable<TimeSpan> RunMany(Action action, int runCount)
    {
        List<TimeSpan> results = new List<TimeSpan>();
        for (int i = 0; i < runCount; i++)
            results.Add(RunOnce(action));
        return results;
    }
}
{% endcoderay %}

This allows any code to be easily called and timed. For example, suppose you have a void Method called `Foo`. This can be timed via:

{% coderay lang:java%}
TimeSpan timespan = Benchmarker.RunOnce(() => Foo());
{% endcoderay %}

Similarly, if you want to benchmark a method that happens to return something like this

{% coderay lang:java%}
int bar = FooWithReturnValue();
{% endcoderay %}

you can use the above Benchmarker class via

{% coderay lang:java%}
int bar;
TimeSpan timespan = Benchmarker.RunOnce(() => bar = FooWithReturnValue());
{% endcoderay %}

I've put this on github:
[https://github.com/mattnedrich/tools/tree/master/csharp/Benchmarker.cs](https://github.com/mattnedrich/tools/tree/master/csharp/Benchmarker.cs)