# numbly

A package that helps you to specify different numbering formats for different levels of headings.

## Usage example

Suppose you want to specify the following numbering format for your document:

- Appendix A. Guide
  - A.1. Installation
    - Step 1. Download
    - Step 2. Install
  - A.2. Usage

You might use `if` to achieve this:

```typst
#set heading(numbering: (..nums) => {
  nums = nums.pos()
  if nums.len() == 1 {
    return "Appendix " + numbering("A.", ..nums)
  } else if nums.len() == 2 {
    return numbering("A.1.", ..nums)
  } else {
    return "Step " + numbering("1.", nums.last())
  }
})

= Guide
== Installation
=== Download
=== Install
== Usage
```

But with `numbly`, you can do this more easily:

```typst
#import "@preview/numbly:0.1.0": numbly
#set heading(numbering: numbly(
  "Appendix {1:A}.", // use {level:format} to specify the format
  "{1:A}.{2}.", // if format is not specified, arabic numbers will be used
  "Step {3}.", // here, we only want the 3rd level
))

= Guide
== Installation
=== Download
=== Install
== Usage
```

![image](https://github.com/user-attachments/assets/27e81a8e-e154-43b8-bed7-d3cd8ef4b2a4)

Here is another example:
```typst
#import "@preview/numbly:0.1.0": numbly
#set heading(numbering: numbly(
  "Part {1:I}:",
  "{2}.",
  "{2}.{3}.",
))
#show heading.where(level: 1): set align(center)

= Getting Started with Python
== Introduction to Python
=== What is Python?
=== Why Choose Python?
=== Python 2 vs. Python 3
== Setting Up Your Environment
=== Installing Python
=== Setting up an Integrated Development Environment (IDE)
= Core Concepts
== Control Structures
== Functions
```

![image](https://github.com/user-attachments/assets/a80432d1-e679-4354-a2ac-2eb2c5c8edd0)

## General explanation

In general the function `numbly` takes an arbitrary number of string arguments. 
The first argument specifies the formating of the numbering of the first level headers, the second for the second level headers and so on. 
If one wants to access the number of the $n$th level header, one can use `{n:f}`, where `f` is a counting symbol, which states how the number is represented (e.g. `1` stands for arabic numbers, which is the default, `I` for roman numbers from capital letters and so on). A list of all possible counting symbols can be found in the typst documentation of numbering (https://typst.app/docs/reference/model/numbering/).

## Bonus

Although the following scenario was not initially considered, we were surprised to find that this library can also be used to number lists and page numbers!

```typst
#import "@preview/numbly:0.1.0": numbly
#set enum(full: true, numbering: numbly(
  "{1:A}.",
  "{2}.",
  "{3})",
))

+ Item 1
  + Item 2
    + Item 3
    + Item 4
+ Item 5
  + Item 6
  + Item 7
```

![image](https://github.com/user-attachments/assets/03ee2c54-3429-4dba-a832-984be7ad32b4)

```typst
#import "@preview/numbly:0.1.0": numbly
#set text(lang: "zh")
#set page(numbering: numbly("{1}", "第{1}页/共{2}页"))

#outline()
= 你说得对
#pagebreak()
= 但是原神
```

![image](https://github.com/user-attachments/assets/1b1cf577-b5fe-4a74-a0b6-6d3eb7231a7a)
