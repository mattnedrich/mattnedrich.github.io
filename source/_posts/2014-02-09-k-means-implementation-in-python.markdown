---
layout: post
title: "K-Means in Python"
date: 2014-02-09 14:45:25 -0500
comments: true
categories: 
---

I've written a general K-Means implementation in Python. It can be found here:<br>
[https://github.com/mattnedrich/algorithms/blob/master/python/clustering/kmeans/kmeans.py](https://github.com/mattnedrich/algorithms/blob/master/python/clustering/kmeans/kmeans.py)

Everything is contained in one file, <code>kmeans.py</code> The usage model is the following
{% coderay lang:python%}
import kmeans

# observations
#  - list of items to be clustered
# numMeans
#  - number of clusters desired
# featureVectorFunc
#  - function that extracts a feature vector (in the form of a Tuple) from an observation
# maxIterations
#  - optional param, can be used to set a max number of iterations
[clusters, error, numIter] = kmeans.cluster(observations, numMeans, featureVectorFunc, maxIterations)
{% endcoderay %}

My goal was to make something simple and accessible that doesn't require additional libraries (e.g., numpy is not required). The user must do one thing - provide a `featureVectorFunc`{:lang="python"} that takes in an observation from the input, and returns a feature vector in the form of a Tuple. This function would look something like this:
{% coderay lang:python%}
def someFeatureVectorFunc(obs):
    featureVectorAsTuple = extractFeatureVector(obs)
    return featureVectorAsTuple
{% endcoderay %}

The output clusters is a list of cluster objects. Each cluster has a field called `observations`{:lang="python"}. This is subset of the original input observations (stored in a list).

<div class="note">
<b>This implementation is not designed for speed</b>. Rather, it's designed for flexibility. The user can specify a list containing any type of objects. The required featureVectorFunc is used by the K-Means implementation to extract feature vectors from the input observations.
</div>

## Example Results
I ran my implementation on the three synthetic datasets below. For each dataset, I used K=5. I also ran K-Means 10 times and selected the result with the lowest error.

### Dataset 1

<img width="350px" src="{{ root_url }}/images/kmeans_python/basicDataPoints.png"/>
<img width="350px" src="{{ root_url }}/images/kmeans_python/basicDataResult.png"/>

### Dataset 2

<img width="350px" src="{{ root_url }}/images/kmeans_python/roundDataSetPoints.png"/>
<img width="350px" src="{{ root_url }}/images/kmeans_python/roundDataSetResult.png"/>


### Dataset 3

<img width="350px" src="{{ root_url }}/images/kmeans_python/gaussianWithRoundPoints.png"/>
<img width="350px" src="{{ root_url }}/images/kmeans_python/gaussianWithRoundResult.png"/>
