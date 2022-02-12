# saturday_fun - Small and complex rust program to solve a simple problem

So, you know when you are bored and with envy to do something very complex? 

Take Rust and build a very complex (but hyper fast) program to solve an algorithm!

## The Problem:

This is based in a very common interview/tech challenge question:

>Given an array of size n + 1 with the integers 0 to n. Find the duplicate in the list using an algorithm that optimizes for size and one that optimizes for execution in terms of Big O.

## The Challenge

Do it with 1 algorithm that can optimizes for size and execution in pratically O(1).

I know it looks difficult (not imposible, this word shouldn't exist), but it can be done!

## The Solution

Right, the reasoning here is not that "simple", so try to stay with me! 

I'll try to make it simple, promise 😈

### Optimisations for `code gen` size and `assembly` instructions:

So, in the `Cargo.toml`

We need to really optimise our CPU cycles and strip out all the unnecessary stuffs that we don't need, like the std & core dependencies. 

- First we are using a stable compiler version (1.58.1) but we can use nightly features, so we declare and use them.
- 

