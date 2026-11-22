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

## Inbetween Sums:

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






