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
- What is a function?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Identify the different parts of a sketch
- Explain the purposes of the different parts of a sketch
::::::::::::::::::::::::::::::::::::::::::::::::

The language that we use in the Arduino IDE is called `The Arduino Programming Language` which is based on a another programming language called C++. It would have been easy if we could abbreviate The Arduino Programming Language as APL, but we can't because APL refers to a completely different language. The programs written in the Arduino IDE are also not called programs but rather sketches.

The language consists of three parts: **functions**, **values** (variables and constants), and **structures**. If you remember the template that was automatically generated when we opened the IDE in the previous episode, you might have recognised some of these parts. All sketches require a setup and a loop as in the following code snippet:

```c
void setup() {
  // put your setup code here, to run once:

}

void loop() {
  // put your main code here, to run repeatedly:

}
```

You can find more information about functions, values and structures in the online reference manual at [https://www.arduino.cc/reference/en/](https://www.arduino.cc/reference/en/). 

:::: challenge

### Challenge 1: The Parts of a Sketch (Program)

Looking at the following code snippet, can you identify the above mentioned features?

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
4. The curly brackets indicate the beginning and end of the function. The left curly bracket should appear after the parenthesis. The following lines are instructions that the micocontroller has to execute. The right curly bracket closes the function and indicates that the end has been reached. If the function does not have a return type of `void`, i.e. it has any of the other data type, then the last line in the function has to contain the keyword `return` followed by a value (or a variable containing a value) of the specified return type.

:::::

::::

## Functions

All functions have the following:

1. a return value type
2. a unique name
3. parenthesis
4. curly brackets

All sketches **must** have a setup and a loop function. Any instructions that we put in the `setup` will be executed when the program starts. After completing the instructions in setup the microcontroller continues to the `loop` function where the microcontroller will execute all the instruction over and over again - in an infinite loop. As in the example, there can be other functions. Such functions are created as needed and can be called from any other function. Functions can even call themselves. There are many built-in functions, some of which we will be using. It is also possible to load `libraries` that will give us even more functions for more functionality. We will also be using some of those in this lesson.

## Variables and Constants
Variables and constants are named memory positions in the motorcontroller in which one can store a value. The value of variables can be changed but the values of constants cannot be changed after they have been allocated a value for the first time. Before a vlue can be assigned to a variable, the variable has to be declared. The declaration tells the compiler of what `type` of value will be stored in the variable. 

### An example sketch

Let's start by creating a new sketch to experiment with the things we have mentioned so far. We will also introduce one **structure**, the for-loop. On the menu click `File` and then `New`. When you create a new sketch, the IDE will automatically add the setup and loop functions for you. There will be no code in the functions, just two comments. Comment lines start with `//` and are ignored by the compiler but allows the programmer to enter comments to document what the program does. Do use the comment feature. It makes it much easier when you come back to the code or when you have to pass the code onto someone else:

```c
// This program will print numbers starting at the value allocated to the
// variable `minimum` up to the variable called `maximum`

// declare a constant of type integer called minimum
const int minimum = 0;

// declare a constant of type integer called maximum
const int maximum = 100;

// delcare a variable of type float called pi
// a float type is a number that a decimal point.
float pi = 3.14159265358979;

void setup() {
  // Initialise the `Serial` library and tell it what speed to 
  // use for communication with the computer via the USB port
  // The Serial library is included in the language by default
  // so we don't have to import it.
  Serial.begin(9600)
}

void loop() {
  // Use a loop structure to count from minimum to maximum
  // Notice the `int i` variable that is used as the counter
  // To understand the for-loop structure, read it as:
  // For i equals minimum to i less-than-or-equal-to maximum, 
  // increment i by one (the ++ means increment by 1)
   
  for (int i = minimum; i <= maximum; i++) {
    
    // create a new value called multiply_pi and allocate
    // the value of `i * pi` to it
    
    float multiply_pi = i * pi;
    
    // Use the print function in the Serial library to 
    // print `i` to the Serial Monitor
    Serial.println(multiply_pi);
  }
  
  // create a delay of 5000 milliseconds (5 seconds)
  delay(5000);
  // After the delay the microcontroller will start at the 
  // beginning of loop() and run all the code inside it again.
  
  }
}
```

:::: challenge

### Challenge 2:

With the person next to you, can you do the following?

1. Identify variables, constants, functions and structures
2. Of what types are the variables and constants?
3. Explain what the code is doing

:::::: solution

### Show me the solution

#### Constants
minimum - of type integer
maximum - of type integer

#### Variables
i - of type integer
pi - of type float
multiply_pi - of type float

#### Functions
setup()
loop()
Serial.println()
delay()

#### What does the code do?
1. Firstly we declare two constants of type integer, `minimum` and `maximum` and we give them values of 0 and 100.
2. We declare on variable of type float called `pi` and we give it a value of 3.14159265358979.
3. The code will first enter the `setup()` function and,
4. initialise the serial communications from the ESP to the computer at a speed of 9600 [baud](https://en.wikipedia.org/wiki/Baud). Baud is the data transmission rate in bits per second. 
5. Next the code will enter the loop function.
6. Inside the loop function we create another loop with the for-loop structure. This loop will use the variable `i` which will be of type `integer` and it will start with `i` equal to `minimum`. It will then increment `i` by one for each iteration of the loop while `i` is less than `maximum`, until it becomes equal to `maximum`. 
7. On each iteration of loop a variable of type float is declared that is called `multiply_pi`. 
8. `multiply_pi' is given the value of whatever `i` is at the time multiplied by `pi`.
9. The current value of `multiply_pi` is then sent and printed in the serial monitor
10. When `i` reaches the value `maximum`, the loop will terminate and the program will pause for 5000 milliseconds.
11. Because this happens in the `loop()` function the whole process will start all over again and this will go on indefinitely

::::::
::::


https://www.arduino.cc/reference/en/

::::::::::::::::::::::::::::::::::::: keypoints 

- Identify variables, constants, functions and structures
- Know how to identify a function
- Recognise the different types of values that variables and constants can have and what type of information each of these can hold.
- Know where to find the Arduino Programming Language reference

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
