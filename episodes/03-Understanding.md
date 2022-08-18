---
title: "Understanding the code"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- In what language do we program in the Arduino IDE?
- What do the keywords, void, setup and loop do?
- How can we add explanatory comments to the code?
- What is a variable?
- What is a constant?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Objective
::::::::::::::::::::::::::::::::::::::::::::::::

The language that we use in the Arduino IDE is called `The Arduino Programming Language` which is based on a another programming language called C++. It would have been easy if we could abbreviate The Arduino Programming Language as APL, but we can't because APL refers to a completely different language. The programs written in the Arduino IDE are also not called programs but rather sketches.

The language consists of three parts: functions, values (variables and constants), and structure. If you remember the template that was automatically generated when we opened the IDE in the previous episode, you might have recognised the structure. All sketches require a setup and a loop as in the following code snippet:

```c
void setup() {
  // put your setup code here, to run once:

}

void loop() {
  // put your main code here, to run repeatedly:

}
```
  
There are more structural components which you can find on the language reference page at [https://www.arduino.cc/reference/en/](https://www.arduino.cc/reference/en/) but while `setup` and `loop` are compulsary, the other components are only used when needed. `setup` and `loop` are also `functions`.  `functions` can be thought of as named groups of instructions. The structure of a function always includes the following features:

1. a return value type
2. a unique name
3. parenthesis
4. curly brackets

:::: Challenge

## Challenge 2: Functions

Looking at the `loop` and `setup` functions of the code snippet, can you identify the above features:

```c
void setup() {
  // put your setup code here, to run once:

}

void loop() {
  // put your main code here, to run repeatedly:

}

int getMagicNumber() {
  return 42;
}

```

::::: solution

1. The keywords `void` and `int` are the return types. If a function does not have a value to return, the keyword `void` is used. `int` means that the function returns a value that is an integer. You can find a list of all the `data types` that can be return on the [Language Reference website](https://www.arduino.cc/reference/en/)
2. The words `setup`, `loop` and `getMagicNumber` are the names of the functions.
3. The names of the functions are followed by parenthesis. We will see later on how they are used.
4. The curly bracket indicate the beginning and end of the function. The left curly bracket should appear after the parenthesis. The following lines are instructions that the motorcontroller has to execute. The right curly bracket indicates closes the function and indicates that the end has been reached. If the function does not have a return type of `void`, i.e. it has any of the other data type, then the last line in the function has to contain the keyword `return` followed by a value (or a variable containing a value) of the specified return type.

:::::

::::


Variables and constants are named memory positions in the motorcontroller in which one can store a value. The value of variables can be changed but the values of constants cannot be changed after they have been allocated a value for the first time.

Any instructions that we put in the `setup` will be executed when the program starts.



https://www.arduino.cc/reference/en/

::::::::::::::::::::::::::::::::::::: keypoints 

- Keypoint

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
