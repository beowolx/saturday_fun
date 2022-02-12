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

So, in the `Cargo.toml` we define some optimisations for the `profile.release`, I'm not gonna get too much in details but 
we use LLVM's Link Time Optimizations, set code gen size to 1 and optimise for binary size.

### Code optimisations

We need to really optimise our CPU cycles and strip out all the unnecessary stuffs that we don't need, like the std & core dependencies. 

- First we are using a stable compiler version (1.58.1) but we can use nightly features, so we declare and use them.
- We use `no_core` to remove all the core dependencies
- As we need to output the result (as because we are not using any core lib 😅), we need to link to the system (OSX in this case) `libc` to use `printf` to print the result
- We add all the Macros that we need (we don't derive because we cannot implement them to primitives)
- We define the traits that we will need (only what we need)

### Algorithm:

The algorigtm is pretty simple and at a first look, a not-attentious observer will say it is `O(n^2)` but not really!

The use of the `const`word just before the function `burn()` causes the compiler to make this constant time and space! 


## Conclusion

The following code outputs that:

```zsh
saturday_fun on  main via 🦀 v1.58.1 
➜ RUSTC_BOOTSTRAP=1 cargo run --release
    Finished release [optimized] target(s) in 0.01s
     Running `target/release/saturday_fun`
The value is: 7
```

As you can see it takes 0.01s to run this program band the number 7 (the duplicate number in our array) is there!!

It has been pushed into a register and printed out, so it's constant time and space in practice!

But what I think is really cool is when we compile the code with the flags to emit `assembly`:

```zsh
RUSTC_BOOTSTRAP=1 cargo rustc --release -- --emit asm
   Compiling saturday_fun v0.1.0 (/Users/luisflaviooliveira/Desktop/Projects/Rust/saturday_fun)
    Finished release [optimized] target(s) in 0.23s
```

And then, if we check the output of our `assembly` code, it is pretty neat:

```zsh
saturday_fun on  main [⇣] via 🦀 v1.58.1 
➜ cat target/release/deps/saturday_fun-6726b766d0af9d47.s
        .section        __TEXT,__text,regular,pure_instructions
        .build_version macos, 11, 0
        .globl  _main
        .p2align        2
_main:
        sub     sp, sp, #32
        stp     x29, x30, [sp, #16]
        add     x29, sp, #16
        mov     w8, #7
        str     x8, [sp]
Lloh0:
        adrp    x0, l___unnamed_1@PAGE
Lloh1:
        add     x0, x0, l___unnamed_1@PAGEOFF
        bl      _printf
        mov     w0, #0
        ldp     x29, x30, [sp, #16]
        add     sp, sp, #32
        ret
        .loh AdrpAdd    Lloh0, Lloh1

        .section        __TEXT,__const
l___unnamed_1:
        .asciz  "The value is: %d\n"

.subsections_via_symbols
```

Only 26 lines of assembly code!!! No handwritten assembly needed.


