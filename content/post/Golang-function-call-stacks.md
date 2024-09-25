---
title: "Golang Function Call Stacks" 
date: 2024-09-25T22:25:19+01:00 
draft: false 
featured: false
toc: false 

usePageBundles: false 
codeMaxLines: 10 
codeLineNumbers: false 
figurePositionShow: true
categories:
  - coding
tags:
  - golang
  - learn-a-new-language-every-year
series:
  - Book-study-Introducing-Go

---

In Go, a function call stack diagram represents the sequence of function calls and how they relate to each other during the execution of a program. The diagram typically includes the following elements:

1. **Function names** – each block represents a function.
2. **Stack frames** – each function's local variables, return address, and parameters.
3. **Call relationships** – arrows indicating which function calls which, and the order of execution.
4. **LIFO (Last In, First Out)** order – illustrating how the stack operates, with the most recent function on top.

Let’s build a simple call stack diagram based on a Go program:

Diagram:
![DALL·E_Function_call_stack_diagram_for_a_imple_Go_program.png](../../static/images/golang/func_call_stacks_post/DALL%C2%B7E_Function_call_stack_diagram_for_a_imple_Go_program.png)

### Corresponding Function Call Stack Diagram:

1. **main**
    - This is the starting point of the program. When the `main` function is called, a frame is created on the stack.

2. **first**
    - Inside `main`, the `first()` function is called. The frame for `first` is pushed onto the stack.

3. **second**
    - Inside `first`, the `second()` function is called, which pushes the `second` frame onto the stack.

4. **third**
    - Inside `second`, the `third()` function is called, and its frame is added to the stack.

5. **Returning**
    - After the `third()` function completes, its frame is popped from the stack. Then the program returns to `second()`, pops that frame, and so on until the `main()` function completes.

```
|--------------------|     <- third (called by second)
|     third()        |
|--------------------|     <- second (called by first)
|     second()       |
|--------------------|     <- first (called by main)
|     first()        |
|--------------------|     <- main (start of program)
|     main()         |
|--------------------|
```

### Key points:

- **LIFO**: Each function that is called adds a new frame to the top of the stack, and when a function finishes, its frame is removed from the top.
- **Call Order**: The arrows between functions in a diagram show the function calling relationships.
- **Returning**: Once the deepest function completes, the stack begins to "unwind," returning to the previous function.


## References

- Introducing Go | Doxsey.net (no date). https://www.doxsey.net/blog/introducing-go/.

## Tools
- Image source: [DALL·E](https://openai.com/dall-e/)
- Grammar checker [language tool](https://languagetool.org/?utm_campaign=addon2-popup-logo)