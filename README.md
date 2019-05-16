






      
  <div id="readme" class="Box-body readme blob instapaper_body js-code-block-container">
    <article class="markdown-body entry-content p-3 p-md-6" itemprop="text"><h3><a id="user-content-introduction" class="anchor" aria-hidden="true" href="#introduction"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Introduction</h3>
<p>This second programming assignment will require you to write an R
function that is able to cache potentially time-consuming computations.
For example, taking the mean of a numeric vector is typically a fast
operation. However, for a very long vector, it may take too long to
compute the mean, especially if it has to be computed repeatedly (e.g.
in a loop). If the contents of a vector are not changing, it may make
sense to cache the value of the mean so that when we need it again, it
can be looked up in the cache rather than recomputed. In this
Programming Assignment you will take advantage of the scoping rules of
the R language and how they can be manipulated to preserve state inside
of an R object.</p>
<h3><a id="user-content-example-caching-the-mean-of-a-vector" class="anchor" aria-hidden="true" href="#example-caching-the-mean-of-a-vector"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Example: Caching the Mean of a Vector</h3>
<p>In this example we introduce the <code>&lt;&lt;-</code> operator which can be used to
assign a value to an object in an environment that is different from the
current environment. Below are two functions that are used to create a
special object that stores a numeric vector and caches its mean.</p>
<p>The first function, <code>makeVector</code> creates a special "vector", which is
really a list containing a function to</p>
<ol>
<li>set the value of the vector</li>
<li>get the value of the vector</li>
<li>set the value of the mean</li>
<li>get the value of the mean</li>
</ol>

<pre><code>makeVector &lt;- function(x = numeric()) {
        m &lt;- NULL
        set &lt;- function(y) {
                x &lt;&lt;- y
                m &lt;&lt;- NULL
        }
        get &lt;- function() x
        setmean &lt;- function(mean) m &lt;&lt;- mean
        getmean &lt;- function() m
        list(set = set, get = get,
             setmean = setmean,
             getmean = getmean)
}
</code></pre>
<p>The following function calculates the mean of the special "vector"
created with the above function. However, it first checks to see if the
mean has already been calculated. If so, it <code>get</code>s the mean from the
cache and skips the computation. Otherwise, it calculates the mean of
the data and sets the value of the mean in the cache via the <code>setmean</code>
function.</p>
<pre><code>cachemean &lt;- function(x, ...) {
        m &lt;- x$getmean()
        if(!is.null(m)) {
                message("getting cached data")
                return(m)
        }
        data &lt;- x$get()
        m &lt;- mean(data, ...)
        x$setmean(m)
        m
}
</code></pre>
<h3><a id="user-content-assignment-caching-the-inverse-of-a-matrix" class="anchor" aria-hidden="true" href="#assignment-caching-the-inverse-of-a-matrix"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Assignment: Caching the Inverse of a Matrix</h3>
<p>Matrix inversion is usually a costly computation and there may be some
benefit to caching the inverse of a matrix rather than computing it
repeatedly (there are also alternatives to matrix inversion that we will
not discuss here). Your assignment is to write a pair of functions that
cache the inverse of a matrix.</p>
<p>Write the following functions:</p>
<ol>
<li><code>makeCacheMatrix</code>: This function creates a special "matrix" object
that can cache its inverse.</li>
<li><code>cacheSolve</code>: This function computes the inverse of the special
"matrix" returned by <code>makeCacheMatrix</code> above. If the inverse has
already been calculated (and the matrix has not changed), then
<code>cacheSolve</code> should retrieve the inverse from the cache.</li>
</ol>
<p>Computing the inverse of a square matrix can be done with the <code>solve</code>
function in R. For example, if <code>X</code> is a square invertible matrix, then
<code>solve(X)</code> returns its inverse.</p>
<p>For this assignment, assume that the matrix supplied is always
invertible.</p>
<p>In order to complete this assignment, you must do the following:</p>
<ol>
<li>Fork the GitHub repository containing the stub R files at
<a href="https://github.com/rdpeng/ProgrammingAssignment2">https://github.com/rdpeng/ProgrammingAssignment2</a>
to create a copy under your own account.</li>
<li>Clone your forked GitHub repository to your computer so that you can
edit the files locally on your own machine.</li>
<li>Edit the R file contained in the git repository and place your
solution in that file (please do not rename the file).</li>
<li>Commit your completed R file into YOUR git repository and push your
git branch to the GitHub repository under your account.</li>
<li>Submit to Coursera the URL to your GitHub repository that contains
the completed R code for the assignment.</li>
</ol>
<h3><a id="user-content-grading" class="anchor" aria-hidden="true" href="#grading"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Grading</h3>
<p>This assignment will be graded via peer assessment.</p>
</article>
  </div>

 
