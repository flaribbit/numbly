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
```



## General explanation

In general the function `numbly` takes an arbitrary number of string arguments. 
The first argument specifies the formating of the numbering of the first level headers, the second for the second level headers and so on. 
If one wants to access the number of the $n$th level header, one can use `{n:f}`, where `f` is a counting symbol, which states how the number is represented (e.g. `1` stands for arabic numbers, which is the default, `I` for roman numbers from capital letters and so on). A list of all possible counting symbols can be found in the typst documentation of numbering (https://typst.app/docs/reference/model/numbering/).
