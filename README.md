[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/pcZN77hC)
# Rust Version: Strings and Memory Management

[![Rust tests](../../workflows/RustTests/badge.svg)](../../actions?query=workflow%3A"RustTests")

This lab assignment is the Rust version of the [C Strings and Memory Management](https://github.com/UMM-CSci-Systems/C-strings-and-memory-management). It includes three tasks: (i) Rust version of Disemvowel, in contrast to the C version of [Disemvowel](https://github.com/UMM-CSci-Systems/C-strings-and-memory-management?tab=readme-ov-file#disemvowel); (ii) Rust version of palindromes, similar to the C version of [palindromes](https://github.com/UMM-CSci-Systems/C-strings-and-memory-management?tab=readme-ov-file#fixing-palindromes) but need you to implement it in Rust; (iii) Creating memory leaks by making [cycles with reference counting](https://doc.rust-lang.org/book/ch15-06-reference-cycles.html) in Rust. This lab assignment is instructed by Prof. Nic McPhee and is built on top of the original [disemvowel-in-rust](https://github.com/UMM-CSci-Systems/disemvowel-in-rust).



## What is Rust?

[Rust](https://www.rust-lang.org/) is a systems programming language designed to give the performance of C or C++ while providing the memory safety of garbage collected languages like Java. It does this through a very strict compiler that will refuse to compile code that is not memory safe. This means that rather than using Valgrind when you run your code like you did in C, the compiler will tell you if and where any potential memory problems exist when you compile your code. This is incredibly useful, but also can be frustrating as code that feels like it should work will refuse to compile over what might appear to be small errors.

Currently no other language promises memory safety while delivering the sort of performance seen in Rust, and in situations where speed and safety are the highest concern, Rust is becoming a more popular choice. Of all [security bugs in Microsoft software](https://www.zdnet.com/article/microsoft-70-percent-of-all-security-bugs-are-memory-safety-issues/), 70% of them are memory related, and Rust has been identified as a potential solution to these issues. Additionally, companies like [Discord](https://blog.discord.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f) have been moving to Rust to gain the performance benefits of the language.

![70% of Microsoft security bugs are related to memory safety](https://user-images.githubusercontent.com/302297/143053772-9077e5e5-cf39-431f-a2c6-e414c0533f87.png)

## Rust Background

### Running code

Cargo is Rust's package manager and build tool.
You can run your code in the command prompt by using

```bash
     cargo run
```

And you can run the tests by using

```bash
     cargo test
```

### Rust Variables

#### Types

The two data types you will be working with in this lab are [Strings](https://doc.rust-lang.org/book/ch08-02-strings.html#storing-utf-8-encoded-text-with-strings) and [Vectors](https://doc.rust-lang.org/book/ch08-01-vectors.html).

Vectors are a variable sized collection that can store data of the same type; it is fairly similar to something like ```ArrayList``` in Java. You can not have a vector with both numbers and strings in it, but you can add or remove as many items as you want to a vector. You can create a vector in a few ways.

```rust
     //Initializes a vector containing 5 numbers.
     let v = vec![1, 2, 3, 4, 5];

     //Initializes an empty vector
     let v2 = Vec::new();
```

The `String` data type in Rust stores text data, but those are implemented as a vector of characters behind the scenes. You initialize a `String` in a number of ways, including.

```rust
     let s = String::from("Hello World");
```

#### Ownership

The main method of ensuring memory safety that Rust uses is [ownership](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html). Ownership means that only one variable can _own_ a value at a given time. This is important because it means that when that owner goes out of scope (e.g., when the function returns), the value can be safely dropped. If two variables were pointing to that value we would not be able to drop the value as the other variable could still exist and need
the value.

To get around this restriction, [borrowing](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html) is used to allow a variable to have a _read-only pointer_ or _reference_ to a value. This allows a new variable to take the same value as another without taking ownership. This is often used to pass a value to a function without losing ownership of the original value.

In this snippet, for example, if we didn't use `&s` to indicate
that `calculate_length` is borrowing the string (instead of
taking ownership of it), then we wouldn't be able to print `s`
after the call to `calculate_length` because we would have lost
ownership over `s`.

```rust
    let s = String::from("Hello");
    let len = calculate_length(&s);
    println!("{}", s);

    fn calculate_length(str: &String) -> usize {
        s.len()
    }
```

### Panics and Error Handling

Rust has a few methods of handling errors or missing values, but the one we will be focusing on is the [Panic](https://doc.rust-lang.org/book/ch09-01-unrecoverable-errors-with-panic.html). When your code reaches an unrecoverable error, you can use the `panic!` macro to terminate the code with a custom error messsage
such as:

```rust
     panic!("There's a problem!");
```

## Disemvowel
This is a simple lab where we'll use Rust to
implement the `disemvowel` function that we covered
[in a previous C lab](https://github.com/UMM-CSci-Systems/C-strings-and-memory-management#disemvowel). Please run your Rust code and submit both the code and the screenshots of the results.

As you've done in your prior lab, "Disemvoweling" is the act of removing all the vowels ('a', 'e', 'i', 'o', and 'u', both upper and lowercase) from a piece of text. This time your code will take an input and output file as arguments and write the disemvoweled contents of the input file to the output file. You will finish a few lines of code to read and write to files, then finish the disemvowel function. Make sure your code passes all cargo tests.

Your job here is to write a small Rust program that takes in two
command line arguments, each of which are file names. The first
should be the file containing the text to disemvowel, and the
second should be the name of the file we'll write the disemvowelled
text to.

The skeleton code is in `src/main.rs`, which includes a number of
unit tests that (among other things) demonstrate Rust's ability to
work with complex unicode text. See comments in that file for
more guidance on what needs to be done to complete the lab.

Running `cargo test` should run the tests, and if you want to
run it "by hand"

```bash
     cargo run this.txt that.txt
```

will run your program with the given command line arguments
(`this.txt` and `that.txt` in the example).

The file `input.txt` has a variety of test text, and
`target_output.txt` has the expected result of disemvowelling
`input.txt`. If you run

```bash
     cargo run input.txt my_output.txt
```

then

```bash
     diff my_output.txt target_output.txt
```

should show you any differences between your output and the
expected output.

**Note:** Because Rust can deal with a variety of languages via
unicode, the question of what a vowel is, and how it should be
handled is actually a little complicated and we don't attempt
to deal with it completely here. There are some edge cases where
an accented vowel shows up in Rust as two separate chars, and
disemvowelling can remove the vowel but leave behind a dangling
accent. We leave addressing that for another day.

## Palindromes
Please recall the C version of [palindromes](https://github.com/UMM-CSci-Systems/C-strings-and-memory-management?tab=readme-ov-file#fixing-palindromes) and start writing a Rust version of palindromes that uses a similar reverse function. Please run your Rust code and submit both the code and the screenshots of the results.

## Creating memory leaks in Rust (Extra Credit)
Will memory-safe programming language Rust have a memory leak issue? This task aims to motivate students to [create memory leaks by making cycles with reference counting](https://doc.rust-lang.org/book/ch15-06-reference-cycles.html). Please run your Rust code and submit both the code and the screenshots of the results.





