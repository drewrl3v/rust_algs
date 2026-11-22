# Recusions v.s. Iteration

Let's begin by solving problems using iterative and recursive approaches.
As you will see sometimes solving a problem using one approach will feel more 
natural than the other.

The idea is to hone your intuition between the two approaches. First we solve 
a problem recursively then we solve it iteratively.


## Factorial (Linear Recursive)

Write a recursive function for the factorial function. You may assume that the input `n` is greater or equal to `0`.

<details>
<summary>Solution 1</summary>

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
fn factorial(n: i32) -> i32 {
	if n == 0 {
		return 1;
	}
	else {
		return n * factorial(n-1);
	}
}

println!("{}", factorial(5));
```
</details>

<details>
<summary>Solution 2</summary>

The firs solution is good. But This is `Data Structures and Algorithms in Rust`. So let's 
touch a tiny bit of some concepts in rust to whet your appetite.

Firstly, the problem states that `n >= 0`. But an `i32` type allows for negative values.
Let's changed the type to an unsigned int, i.e. `usize`.

```rust
fn factorial(n: usize) -> usize {
	if n == 0 {
		return 1;
	}
	else {
		return n * factorial(n-1);
	}
}

println!("{}", factorial(5));
```
</details>


<details>
<summary>Solution 3</summary>

But what if we pick an `n` so large that the multiplication `n * factorial(n-1)` overflows?
Surely we should handle something like that? The way we annotate this is in the following 
fashion: 

Since we know that our factorial function should return a `usize` type or we 
can overflow and throw an error. In otherwords the function has an **option** to return 
`usize` or not. We use the `Option` to denote this:

```rust
fn factorial(n: usize) -> Option<usize> { // NOTE: Option<>
	if n == 0 {
		return Some(1); // NOTE: Some()
	}
	else {
		return Some(n * factorial(n-1)?); // NOTE: ?
	}
}

println!("{}", factorial(5).unwrap()); // NOTE: .unwrap()
```

Notice the `Some` keyword, with either return something we intended to or we don't.

There are two other oddities: 

1. The question mark: `?`

Think about it like this: When we pass `n = 5` into our factorial function it has type `usize`.
However the `factorial(n-1)` must return an `Option<usize>` type. We can't just mix multiplicaiton 
between the two types. So when we write `factorial(n-1)?` with the question mark, what we are asking
is for Rust to give us the `usize` type assuming we didn't get some error.

2. The `.unwrap()` method.

Okay now it's time to call the factorial function on `5`. Assuming our function succeeds, we should
get `Some(120)` as the answer, but we really just want the numerial answer, so we `unwrap` the package
to get 120.
</details>


`TODO:` `Time to point out some Rust obscurities`: When you `return` something in rust, you don't need 
to explicitly write `return` you can just write the value that is returned.

<details>
<summary>Solution 4</summary>

`TODO:` Introduce a pattern matching example.

```rust
fn factorial(n: usize) -> usize {
	match n {
		0 => 1,
		_ => n * factorial(n-1),
	}
}

println!("{}", factorial(5));
```
</details>









