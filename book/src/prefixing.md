# Prefixing

An important tool is the idea of using prefixes to obtain additional statistics
or properties about data.

Firstly let's consider the following unassuming problem:

## Cumulative Sums:                                                         
                                                                            
Given an array of numbers, `nums`, write a function that returns an array of
the cumulative sums of the numbers of nums. For example if
`nums = [0.3, 4.0, -2.0]`, then you should return `[0.3, 4.3, 2.3]`.

One possible solution is as follows:

```rust
fn cumulative_sum(nums: Vec<f32>) -> Vec<f32> {
	let mut cur_sum = 0.0;
	let mut res = vec![0.0; nums.len()];
	for i in 0..nums.len() {
		cur_sum += nums[i];
		res[i] = cur_sum;
	}
	res
}

println!("{:?}", cumulative_sum(vec![0.3, 4.0, -2.0]));
```

Okay, so what? Well, we created an array of cummulative sums, and note that 
after we did the $O(N)$ work of creating the array, it now takes an $O(1)$
cost to look up any of the precomputed sums.

If we know we're going to have to compute these cumulative sums
many many many times, then the $O(1)$ lookup benefit becomes significant.

So much so that when you average everything out, the cost of computing 
any cumulative sum is effectively $O(1)$. We sometimes call this `ammortized`
$O(1)$ and denote this $O^{*}(1)$.

It's worth pointing out if we are allowed to modify the vector then we don't 
need to pay the extra $O(N)$ for a new vector to store the cumulative sum
array.

Additionally these cumulative sums are also referred to as `prefix sums`.
because you can think of a contiguous sub array/vector/list as being defined
by indexing from `0` upto it's `i-th` index, i.e. the prefix is `i` where the 
suffix will be `0`.

## Inbetween Sums

Suppose I have a list of numbers `psums` and where the `i-th` entry is the 
`i-th` prefix sum of a list of numbers `nums`. Compute the sum between 
the `i-th` and `j-th` entry (inclusive) of `nums` in $O(1)$ time. 


<details>
<summary>Solution</summary>

Let's make the following observation: For simplicity call the array of prefix
sums $psum$, then notice that the sum of the array `nums` from `nums[i]` to 
`nums[j]` can be written as:

$$
psum[j] - psum[i]
$$

Recall that indexing $psum$ is $O(1)$ time.


```rust
fn inbetween_sum(psum: &[i32], i: usize, j: usize) -> i32 {
	psum[j] - psum[i]
}


// TODO: Double check this prefix sum and indexing
// A quick inplace prefix sum vector.
let mut psum: Vec<i32>  = (1..=100).collect();
let mut acc = 0;
for x in &mut psum {
	acc += *x;
	*x = acc;
}

println!("{}", inbetween_sum(&psum, 20, 30));
```
</details>


## Maximum Sum Subarray (Single Pass)

Given and array of integers `nums`, find the contiguous subarray with maximum sum.

<details>
<summary>Solution</summary>

Unsurprisingly, this has to do something with prefix sums. We can compute the 
contiguous sums as a difference of a prefix sum array:

$$
psum[j] - psum[i]
$$

However this problem is also an optimization problem. So let's write out this 
optimization problem formally. We want the value of the maximum sum subarray:

$$
\max_{0 \leq i \leq j \leq n} psum[j] - psum[i]
$$

where $n$ is the length of $psum$.

To determine an algorithm that solves this, let's think about what makes a sum larger or smaller:


1. If $i$ is fixed, then we want $psum[j]$ to be as large as possible.
2. If $j$ is fixed, then we want $psum[i]$ to be as small as possible.

This strategy of figuring out what optimizes an objective function by isolating the variables is 
sometimes referred to as `seperation of variables`.

If we think back to our calculus days, pretend we have a multivariable function $f(i, j) = psum[j] - psum[i]$. 
And we want to find the maximum.

Then in calculus, we compute the gradient $\nabla_{(i,j)} f(i,j)$ set it to $0$ and solve for $i$ and $j$.
In the most ideal of cases, $i$ and $j$ are independent and we can just solve for each one separately.

However, the problem we have here is that:

* $f$ is definitely not differentiable everywhere.

But we made a powerful observation. We found out that if $j$ is fixed, then picking $i$ so that $psum[i]$ is as small
as possible increases $f(i,j)$. Likewise, fixing $i$, then picking $j$ so that $psum[j]$ is as large as possible also
increase $f(i,j)$. Since there is no dependence of $psum[i]$ and $psum[j]$ as long as we keep a variable fixed, we may
re-write the optimization problem as follows:

$$
\max_{0 \leq j \leq n} psum[j] - \left(\min_{0 \leq i \leq j} psum[i]\right)
$$

If you look at the fomulation of the optimization problem, you can get a sense that you can iterate through $j$, 
then on a particular $j$, we then iterate through $i$ to see if we can find a smaller $psum[i]$. So the most naive
implmentation of this optimization program is to first compute the $psum$ array in $O(N)$ time and complexity, then 
do two for-loops on $j$ and $i$ which takes $O(N^2)$ time. 

```rust
fn maximum_sum_subarray(nums: Vec<i32>) -> i32 {
	let mut psum = vec![0];
	let mut acc = 0;
	for x in nums {
		acc += x;
		psum.push(acc);
	}

	let mut cur_max = i32::MIN;
	for j in 1..psum.len() {
		let mut cur_min = i32::MAX;
		for i in 0..j {
			cur_min = cur_min.min(psum[i]);
		}
		cur_max = cur_max.max(psum[j] - cur_min);
	}
	cur_max
}

let nums = vec![-2,1,-3,4,-1,2,1,-5,4];
println!("{:?}", maximum_sum_subarray(nums));
```
</details>


## Maximum Sum Subarray (Online)


<details>
<summary>Solution</summary>

An online or incremental or on-the-fly algorithm means that instead of
computing everything ahead of time or instead of having knowledge of 
the information ahead of time, we keep track of things in the moment.


Notice that for a prefix sum, if the only thing that's important is the
prefix sum at index $i$, then we can keep track of this value by just
incrementing a sentinel variable, call it `cur_sum`. If we need the
prefix sum for $i+1$ just add `nums[i]` to `cur_sum`.

This is all opposed to computing every prefix sum in advanced. The 
benefit is that we can solve this problem while computing the prefix
sums online which is faster than precompting the sum and storing it 
all in memory first.

```rust
fn maximum_sum_subarray(nums: &[i32], i: usize, j: usize) -> i32 {
	todo!();
}
```
</details>










