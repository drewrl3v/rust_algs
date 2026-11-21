# Recusions v.s. Iteration

Let's begin by solving problems using iterative and recursive approaches.
As you will see sometimes solving a problem using one approach will feel more 
natural than the other.

The idea is to hone your intuition between the two approaches. First we solve 
a problem recursively then we solve it iteratively.


## Factorial (Linear Recursive)

Write a recursive function for the factorial function. You may assume that the input `n` is greater or equal to `0`.

<details>                            
<summary>Solution</summary>          


From math we may recall that the factorial may be defined in this piece-wise recursive form:

$$
n! = \begin{cases} 
          1 & \text{if}~~~ n = 0 \\
          n * (n-1)!  & \text{else}
       \end{cases}
$$

The definition we are given is naturally recursive and there are two cases. This suggests
some kind of if/else statement based on whether $n = 0$ or $n > 0$. We can write this code
as follows:

```rust
fn rec_factorial(n: i32) -> i32 {
	if n == 0 {
		return 1;
	}
	else {
		return n * rec_factorial(n-1);
	}
}
```

</details>
