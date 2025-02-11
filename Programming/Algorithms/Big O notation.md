The Big o notation is a way to describe how much computation we need to solve a given problem.

More precisely the Big O notation is a way to describe how the computational resources (both time and space) required by an algorithm grow as the input size increases.

"Computational resources" - Includes both:
- Processing time (how long it takes to run)
- Memory usage (how much storage space is needed)

"Growth rate" - We care about the pattern of increase, not the exact numbers. This is why we drop constants and lower-order terms. An algorithm that takes 2n steps and one that takes 100n steps are both O(n), because they grow linearly.

"Upper bound" - Big O tells us the maximum resources needed. The algorithm might sometimes perform better, but it won't perform worse. This is crucial for planning and ensuring reliability.

"As the input size increases" - We're particularly interested in how the algorithm behaves with large inputs, because that's where the differences between algorithms become most dramatic.

This definition helps us understand why Big O notation is so valuable: it lets us predict how our programs will perform as they deal with more data, helping us choose the right algorithms for our specific needs.

## The most common Big O complexities:
**O(1) - Constant Time** 
This is like looking up a word in a dictionary when you know the exact page number. No matter how big the dictionary gets, it takes the same amount of time. An example would be accessing an array element by its index.

**O(log n) - Logarithmic**
Time Imagine looking up a word in a dictionary by starting in the middle and eliminating half the remaining pages each time. Each time you double the dictionary's size, you only need one more step. Binary search is a classic example of this complexity.

**O(n) - Linear Time**
Think of this like reading through a book page by page. If the book doubles in length, it takes twice as long. Finding the maximum value in an unsorted array is a good example - you need to check every element exactly once.

**O(n log n) - Linearithmic**
Time This is like sorting papers by repeatedly dividing them into piles and then merging those piles in order. Most efficient sorting algorithms like mergesort and quicksort have this complexity.

**O(n²) - Quadratic Time**
Picture arranging people in a line by comparing each person with everyone else. When you double the number of people, it takes four times as long. Bubble sort is a classic example - it compares each element with every other element.

**O(2ⁿ) - Exponential Time**
This is like trying every possible combination on a lock. Adding just one more digit doubles the time needed. The classic example is calculating Fibonacci numbers recursively without memoization.

**O(n!) - Factorial Time**
This is like trying to find every possible route between cities by checking every possible order. It grows incredibly fast - with just 10 cities, you're checking millions of possibilities. The traveling salesman problem solved by brute force has this complexity.