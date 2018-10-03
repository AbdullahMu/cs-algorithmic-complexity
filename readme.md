# Algorithmic Complexity + Big O Notation

## Learning Objectives
* Identify what makes a good algorithm
* Use Big-O Analysis to Evaluate Algorithms

### Framing

Throughout this course, we have been using functions and ML models without much concern for training time or funtion exectuion time. However, with the use of Grid Search, SVM and Neural Nets or complex models and especially as we start using Big Data, we have and will continue to notice that our model training beigns to take signifigant computatial time. We can optimize for this by understaninf the concept of BIG O as it realtes to time and space complexity. In data science there is no only a trade off between bias and variance with our algorithms, but between complexity/accuracy and training time. In a world of inifinite computing power and time, this point becomes moot, but we don't live in that world and even so optimizing code is fundamental. 

Our main concerns in terms of efficiency are *time* and *memory*. 

## Run Time and Big-O Analysis 

We can look at a program and say -- "Oh that took two seconds to run". But that two seconds is dependent on a lot of factors. That two seconds is for a very specific input - your computer on your network with a certain version of your programming language. Instead, we should generalize the algorithm's complexity.

We do so using a notation that mathematicians and computer scientists use, called Big-O notation. This notation standardizes how we discuss the efficiency of algorithms. ***Big-O really tells us how quickly the runtime grows as the input becomes arbitrarily large***.  Most of the time, we use Big-O notation to describe time complexity, but we can also use it to describe memory efficiency.

Big-O notation is not an exact metric for benchmarking algorithms. Rather, it gives us an abstract idea about how costly or efficient an algorithm is, with respect to how much computing power it takes. With Big-O notation, we are comparing orders of magnitude.

A few notes on Big-O notation:
* When we calculate the Big-O of the function, we are calculating the **worst** possible runtime for a given function.
* Sometimes, Big-O notation is referred to as [asymptotic analysis](http://www.cs.cornell.edu/courses/cs3110/2013sp/supplemental/lectures/lec19-asymp/review.html).

For time complexity, we want to count how many times the code is run in context of how large the input to the code is. For example, O(1) is a very efficient piece of code, O(N!) is very inefficient. Let's first look at an example of what time cplexity is and how it relates to Big O:

[Big O Intro Video](https://www.youtube.com/watch?v=v4cd1O4zkGw)

Lets now break this down into categories of Big-O.

### O(1) Complexity (aka Constant Complexity)

O(1) means that an algorithm's runtime is static or constant. *The complexity stays the same no matter the input*.

```javascript
function helloWorld (arr) {
	console.log('hello world')
}

function returnFirstItem (arr) {
	return arr[0]
}
```

In both of the above examples, no matter what size the `arr` argument is, the function will run once.

### O(N) Complexity (aka Linear Complexity)

O(N) complexity means that, as the input sizes increase, the processing time increases linearly. Or, more simply, *the code runs once for each input*.

```javascript
function iterate (arr) {
	arr.forEach(item => console.log(item))
}

function iterateLoop (arr) {
	for (let i = 0; i < arr.length; i++) {
		console.log(arr[i])
	}
}

function addOne (arr) {
	return arr.map(item => item + 1)
}
```

In each of the above examples, we go through the array and perform an action with each item in it. If we have the array `[1]`, each will execute once. If we have the array `[3, 5, 1000]` the code will run 3 times. If our array has 1000 items, the code will execute 1000 times!

### O(N^2) Complexity (aka Quadratic Complexity)

For an input with the size n, *quadratically complex algorithms execute `n*n` times*.

```javascript
function consoleLogLots (arr) {
	for (let i = 0; i < arr.length; i++) {
		for (let j = 0; j < arr.length; j++) {
			console.log(arr[i], arr[j])
		}
	}
}
```
For the array `[1, 3]`, this function will print:
```js
[1, 1]
[1, 3]
[3, 1]
[3, 3]
```
For a 2 item array, the code executes 4 times. For 3 items, the code executes 9 times.  This scales pretty fast -- for an array with 100 items this code will `console.log` 10,000 times!

### O(log n) and O(n log n) Complexity

O(log n) refers to algorithms which cut the problem in half each time. These have significantly lower complexity than O(n). We don't actually have to calculate logarithms or anything like that! Technically, a logarithm is a "quantity representing the power to which a fixed number (the base) must be raised to produce a given number." [source](https://en.wikipedia.org/wiki/Logarithm)
>Examples...

- the base 10 logarithm of 1000 is 3 since 10^3 is 1000.
- log<sub>2</sub> 32 = 5

One example of an O(log n) algorithm is a **binary search**. In an *unsorted* array, if we want to find the index of an item with a given value, we have to iterate through it and check if each item is equal to the item we are searching for. However, if we know that we have a **sorted** array, we can do this a lot easier!

For the array `[1, 3, 5, 7, 9, 11, 13]`, if we want to find the index of the 5, we can do so like this:
* Find the item at the midpoint of the array. This ends up being `7`.
* Our item is below 7, so then, since our array is sorted, we only have to search the half of the array before the 7.
* The midpoint of the sub array from 1-7 or `[1, 3, 5]` is `3`.
* This time, 5 is larger than 3, so we search the sub-array `[5]`. Since the midpoint of that array `5` is equal to the number we are searching for, we just return that number.

Let's checkout this video [Binary Search Video](https://www.youtube.com/watch?v=P3YID7liBug)
Let's checkout a ***[visualization](https://www.cs.usfca.edu/~galles/visualization/Search.html)***

An implementation of that algorithm is below:
```javascript
function binarySearch(arr, item, first = 0, last = null) {
	if (!last) last = arr.length - 1
  
	let midpoint = Math.floor((last - first) / 2) + first

	if (arr[midpoint] === item) return midpoint
	if (arr[midpoint] > item) return binarySearch(arr, item, first, midpoint)
	if (arr[midpoint] < item) return binarySearch(arr, item, midpoint, last)
}
```

The above function ran 3 times instead of the 7 that we would need if we iterated through the entire array! This algorithm is super efficient -- even if we have a million items in our array, on average we will only need to execute the binary search 20 times.

O(n log n) algorithms are ones that are faster than O(n^2) but slower than O(n). Let's come back to O(n log n) in a minute -- a lot of sorting algorithms fall under this category.

### O(n!) and O(2^n)

O(n!) and O(2^n) complexities should make you very nervous! These should be avoided at all costs. One example of an O(n!) algorithm is the Bogosort - aka the slowsort. This sort is when an array is randomly ordered over and over again until it is in the correctly sorted order. For an array with the length 10, this sort may have to run up to 3,628,800 times! Sometimes you will have to look at all the available combinations and writing code that are in these complexity categories can't be avoided, but they should bring up some red flags!

### Drop the Coefficients, Constants, and less Significant Terms

With Big-O analysis, it is convention to drop the coefficients and constants. This helps us generalize our how efficient our algorithms will be.  

For example:
```javascript
function iter (arr) {
	// Big-O: N
	arr.forEach(item => console.log(item))
	arr.forEach(item => console.log(item))
	console.log('hello world')
}

function helloWorld () {
	// Big-O: 1
	console.log('hello world')
	console.log('hello world')
}
```
The above examples, at first look would have complexities of O(2N + 1) and O(2) respectively; however, in order to keep things simple, we can drop the coefficients. The time complexities are still linear and constant respectively. We take the least efficient operation within the block of code to measure its efficiency.

Here are some more examples where you want to drop the less significant terms...

<code>O(n​<sup>3</sup> ​​+ 50n<sup>​2</sup>​​ + 10000) = O(n​<sup>3</sup>​​)</code>

<code>O(n+30) x O(n+5) = O(n​<sup>2</sup>​​)</code>

The most significant term always wins out in the end!

### Big-O Summary
![](https://i.stack.imgur.com/jIGhf.png)

The following table shows how algorithms with different complexities scale when given different numbers of inputs. Note: some values are rounded.

|Complexity |1|10      |100  |
|-----------|-|--------|-----|
|O(1)       |1| 1      |1    |
|O(log N)   |0| 2      |5    |
|O(N)       |1|10      |100                            |
|O(N log N) |0|20      |461                            |
|O(N^2)     |1|100     |10000                          |
|O(2^N)     |1|1024    |1267650600228229401496703205376|       
|O(N!)      |1|3628800 |doesn't fit on screen! |

<table class="table table-bordered table-striped">
    <tbody><tr>
      <th>Data Structure</th>
      <th colspan="8">Time Complexity</th>
      <th>Space Complexity</th>
    </tr>
    <tr>
      <th></th>
      <th colspan="4">Average</th>
      <th colspan="4">Worst</th>
      <th>Worst</th>
    </tr>
    <tr>
      <th></th>
      <th>Access</th>
      <th>Search</th>
      <th>Insertion</th>
      <th>Deletion</th>
      <th>Access</th>
      <th>Search</th>
      <th>Insertion</th>
      <th>Deletion</th>
      <th></th>
    </tr>

    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Array_data_structure">Array</a></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Stack_(abstract_data_type)">Stack</a></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Queue_(abstract_data_type)">Queue</a></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Singly_linked_list#Singly_linked_lists">Singly-Linked List</a></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Doubly_linked_list">Doubly-Linked List</a></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="yellow">Θ(n)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="green">O(1)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Skip_list">Skip List</a></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="orange">O(n log(n))</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Hash_table">Hash Table</a></td>
      <td><code class="gray">N/A</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="green">Θ(1)</code></td>
      <td><code class="gray">N/A</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Binary_search_tree">Binary Search Tree</a></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="https://en.wikipedia.org/wiki/Cartesian_tree">Cartesian Tree</a></td>
      <td><code class="gray">N/A</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="gray">N/A</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/B_tree">B-Tree</a></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/Red-black_tree">Red-Black Tree</a></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="https://en.wikipedia.org/wiki/Splay_tree">Splay Tree</a></td>
      <td><code class="gray">N/A</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="gray">N/A</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/AVL_tree">AVL Tree</a></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow-green">O(log(n))</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
    <tr>
      <td><a href="http://en.wikipedia.org/wiki/K-d_tree">KD Tree</a></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow-green">Θ(log(n))</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
      <td><code class="yellow">O(n)</code></td>
    </tr>
</tbody>
</table>

![](http://www.bigocheatsheet.com/img/big-o-cheat-sheet-poster.png)

[Sorting Algorithms Visual Time Complexity](https://www.youtube.com/watch?v=BeoCbJPuvSE)


### You Do: Study Big-O Families (10 min / 9:50)

[Write down the complexities of these functions](https://git.generalassemb.ly/ga-wdi-lessons/cs-algorithmic-complexity/blob/master/big-o-exercise.md).


## Learn More!
* https://www.youtube.com/watch?v=V6mKVRU1evU
* http://www.cs.cmu.edu/afs/cs/academic/class/15451-s10/www/lectures/lect0203.pdf
* https://stackoverflow.com/questions/487258/what-is-a-plain-english-explanation-of-big-o-notation/487278
* https://www.codenewbie.org/basecs
* http://bigocheatsheet.com/

