---
title: "Getting Started: C/C++"
description: "Introduction to C and C++"
author: june
categories: [Programming, Getting Started]
tags: ["C", "C++", "Software Development", "Getting Started", "Basics"]
math: true
mermaid: true
---
## Introduction
Before we can get to the good stuff, I wanted to give a quick overview of how to write a program in C, and by extension C++, which is after all, C with classes. 


This won't cover every feature of the language but it will aim to cover enough to get started with so you can learn more by getting hands on.


If had to start again, I think I'd want to start with C. Not to say you have to, but I would advise that you do and then decide if you want the features C++ provides. In my opinion, learning C first would provide a better foundation than jumping straight into C++ because it has less features. 


This might seem counter intuitive, why would I use a language that provides less features?


Well, I could write volumes about every C++ feature I have a minor gripe about, but to keep it on topic; you have write code using only the base building blocks, getting you closer to what's actually going on in your program. You can still do a lot of what C++ offers in C. But in doing it with these base tools, you'll develop a better understanding of how the additional features work. The more you understand about what the computer is actually doing, the stronger you'll become as a programmer. No longer stabbing in the dark at problems, as I did for many years, you'll just know the issue is by thinking about it. Although, to be honest, there will still be moments of stabbing in the dark, just more informed stabbing into a less dimly lit void.


## Compiling

### What does a compiler do and why do I need to use it?
If I remember correctly, this was probably the first major hurdle hit while studying at university. Something about it was daunting and I just didn't get it.


I had been used to interpreted languages like C# and Python. Which do not require compiling. These languages are setup so another application called an interpreter, translates the code into instructions your CPU can perform, as a sort of middleman. If you're in the game development world, examples of this are: C# in Unity, Blueprints in Unreal and GD script in Godot.


While you can write code for games and other graphical applications in these languages just fine. You do get a performance increase by using a compiled language like C or C++ due to it's pre-compiled nature. It doesn't have to do any extra work to make the code go, the information the CPU needs (assembly code) is already right there in the executable.


Trying not to get too in-depth on that topic, all we need to know is that we have to perform this additional step to make the code run on the CPU, since it doesn't understand C or C++.


This is done by a compiler, like I said, this takes the code you wrote in C or C++ and then translates it into Assembly code so the CPU can actually do something with it.


There are different compilers for C and C++ depending on what operating system you're using. Most commonly, MSVC for Windows, and gcc/g++ or clang on Linux based systems. Though you can use the latter two on Windows as well.


> Some code editors will take care of compilation by hitting the run button, however, if you're working on your own projects outside of a game engine, you'll still need to setup the compiler options somewhere in the application's settings.
{: .prompt-info }


All compilers follow a set of rules supplied by the language standard, which defines what is and isn't valid code. Though, some parts of the C and C++ standard are more loosely defined than others. Meaning some compilers may be fine with somethings that the others are not. Just something to watch out for if you're working in a multi-platform team.


C and C++ compilers do this in three stages:
- Pre-processing
- Building
- Linking


#### Pre-processing
This will become clearer later on in this post, but during this stage the compiler goes over all the code that is not actual C code or instructions you can write in C files that tell the compiler what to do in the next step.


#### Building
This is the stage where we actually convert the C code into assembly for the CPU's consumption. This will output what are referred to as an "intermediate" files, these are kind of a half-baked application. It's all the instructions that the CPU needs to do, but no context for how they interconnect. In a lot of cases, you'll probably only output one file here. But there can be cases where you want to output multiple intermediates for linking together later. This is where you generate any compile errors or warnings. Anything that isn't valid code.


#### Linking
Linking takes all the intermediate files we generated in the last step and binds them together in an executable you can run. Making sure every function you described actually exists and can be called. This is also where any external code that's pre-packaged in what's referred to as a "library" is added to your executable. There's a lot more finer details here but for now, a basic understanding of each stage is all you need to get started. This is where you may have "linking errors", usually denoted by "[LINK]" before an error message in your compiler output. This usually means, the building step was able to see that the code you're trying to call was declared somewhere, but not defined. We'll be going into what that means later.


#### Compiler Arguments
Compilers also allow you to tell them how it is you want it to compile the code you give it. I won't go into too much detail about it just now, but you can find these on the documentation web pages for the compiler you're using. Usually easiest to search something like "msvc options" or "msvc compiler flags". Where "msvc" is replaced with the compiler you're using.


If you're interested, you can find them here:
- MSVC: [https://learn.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-alphabetically?view=msvc-170](https://learn.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-alphabetically?view=msvc-170)
- gcc: [https://gcc.gnu.org/onlinedocs/gcc/Option-Summary.html](https://gcc.gnu.org/onlinedocs/gcc/Option-Summary.html)

- clang: [https://clang.llvm.org/docs/UsersManual.html#command-line-options](https://clang.llvm.org/docs/UsersManual.html#command-line-options)


Thankfully there is a lot of overlap, especially between gcc/g++ and clang.


> gcc and g++ have the same page for command line options but gcc is for C and g++ is for C++. In most cases g++ will still compile C code but there are some features in C that are incompatible.
{: .prompt-info }


### Actually Compiling C
Ok that was a lot of talk about something we don't even know how to use yet. 


We'll be using the command line to compile here, which unless you're a linux elemental, might look a bit scary. Don't worry it's actually not too bad once you get the hang of it.

#### Source files
First you'll need a source file (a file that contains code) to compile. You can make an empty file to write code in later. The standard naming convention is usually main.c or main.cpp for the file where your program starts, but really you can call this whatever you like as long as it ends in .c (for C) or .cpp (for C++).


> It's best to keep your .c and .cpp files in a directory called `src` or `source` to keep your project directory clean. E.g. our main.c will be in `/project_path/src/main.c`
{: .prompt-tip }


#### Entry point
You also need to add what's called an "Entry Point". This tells the compiler "hey, start here in this function". We'll go more into functions and entry points later on, but for now open the file you just created inside your preferred text editor. Then enter the following and save:

```
int main(int argc, char** argv)
{
    return 0;
}
```


#### Build script
Next we're going to create a bash script, we won't go to hard on the bashing, but making a script that runs the compile command is a lot more convenient than having to type it out every time.


Create a file called build.sh in your project directory. Then open it in your text editor.


Here we're actually going to write the command we need to build the project.


Each compiler mentioned has a different command you need to run to make it go, but the basic format is pretty much the same.


```
command input_file.c
```


So our build command will look like...

MSVC:
```
cl src/main.c
```
gcc:
```
gcc src/main.c
```
clang:
```
clang src/main.c
```


Type the relevant command into your build.sh file, save and run the script by double clicking it or by opening your command line in the project directory, and enter the command `./build.sh`

Congrats! You just compiled your first C application.
> For MSVC you'll need to run the `build.sh` script from the command line, running the command `vcvarsall [cpu architecture]` before you do so, for example, my system is 64 bit, so my command would be `vcvarsall x64`. This is due to MSVC needing to setup environment variables in order to access the correct `cl` command. These variables are only set for the command line window that `vcvarsall` was ran in, once the window is closed you'll need to run it again to compile. While you could add `vcvarsall x64` to your `build.sh` script, in my experience, it's usually best to run this once in the command line then call `build.sh` in the same window as this command takes a few seconds to run, which you don't want to have to do every time you build.
{: .prompt-info }

> Depending on how you installed your compiler, you may need to add something to your PATH variable on your operating system. If you're on linux and installed through your package manager, the command should be available to you. But on windows, you may need to add it manually so the command is accessible in any directory. The easiest way to do this is to hit the windows key and type "PATH" into the search box, there should be an option named something along the lines of "PATH" and or "environment variables". Select this option, find the "PATH" variable, click "Edit", add a semi-colon to the end of the existing text, then add the path to where your compiler is installed. My MSVC path is: `c:/Program Files/Microsoft Visual Studio/2022/Community/VC/Auxiliary/Build`. This folder contains `vcvarsall.bat` which sets up command line to be able to run the compile commands.
{: .prompt-info }
> Depending on your code editor, you may want to pass an absolute path into the compiler rather than a relative one. For example the windows path `C:/dev/project/src/main.c` rather than `src/main.c` At least in my case, if I use relative paths, and select an error in the console to jump to it. It will create a temporary copy of the file file instead of opening the actual file itself.
{: .prompt-info }

#### Changing the output directory

By default, the compiler will spit out your executable in the directory the command was called from. It's usually best to make a separate directory called `build` to store all your output files in. This makes it easier to delete all built files, and ignore this directory if you're using source control.


To output to this directory  we can make the following changes to our `build.sh`:

MSVC:
```
# makes directory, adding -p will only create the directory, 
# if it doesn't already exist.
mkdir -p ./build

# change directory to the build directory
cd ./build

# ../ prefix to go "up" one directory, as we're
# no longer in the project root
cl ../src/main.c
```
gcc:
```
# makes directory, adding -p will only create the directory, 
# if it doesn't already exist.
mkdir -p ./build

# change directory to the build directory
cd ./build

# ../ prefix to go "up" one directory, as we're
# no longer in the project root
gcc ../src/main.c 
```
clang:
```
# makes directory, adding -p will only create the directory, 
# if it doesn't already exist.
mkdir -p ./build

# change directory to the build directory
cd ./build

# ../ prefix to  "up" one directory, as we're
# no longer in the project root
clang ../src/main.c
```
> \# in bash is a comment, anything following this will not be considered when running the bash script.
{: .prompt-info }
> You can also specify the output directory along with the output file name by using compiler options, however this only changes the directory that the final executable will be found in. In my experience, I've found it's usually just easiest to change directory to the build folder and then run the compile command from there.
{: .prompt-info }

#### Executable Name
You may also want to change the name of the executable you've built, while you can just rename it yourself every time you build. There are compiler options to name the output after it's compiled to make things easier.

MSVC:
```
mkdir -p ./build
cd ./build

# -link tells the compiler know you're talking about the linking stage, 
# -out: tells it to output the final result under the name provided
cl ../src/main.c -link -out:main.exe
```
gcc:
```
mkdir -p ./build
cd ./build

# -o: tells the compiler to output the executable using the name provided
gcc ../src/main.c -o main.exe
```
clang:
```
mkdir -p ./build
cd ./build

# -o: tells the compiler to output the executable using the name provided
clang ../src/main.c -o main.exe
```
> As said in the last section, you can also specify a directory with your output, to do this just prefix the directory to the text after the output option. E.g. for gcc: `gcc ../src/main.c -o main/main.exe`. Will output into the "main" directory if it exists, creating a file called main.exe within.
{: .prompt-info }
> Linux executables need not have the `.exe` extension, while windows requires extensions to know what to do with the file, linux will know what to do with the file based on it's contents. However, it doesn't hurt to add an extension if you'd like.
{: .prompt-info }

#### Debug info
*Just, one more thing...*


By default the compiler will optimise your code, which is a great when you're releasing. But for development, this would be painful. Not having debug info means our debuggers won't know much about the program. So when we get a error, or want to step through the code to see what it's doing, we'll likely not get the info we're looking for. 


To generate debug info, we need to turn compiler optimisations off. 


To do this we need to change our `build.sh` script to the following:


MSVC:
```
mkdir -p ./build 
cd ./build

# -Zi tells msvc to output debug info
cl ../src/main.c -Zi -link -out:main.exe
```
gcc:
```
mkdir -p ./build
cd ./build

# -O0 (capital o) stops gcc from optimising
gcc ../src/main.c -O0 -o main.exe
```
clang:
```
mkdir -p ./build
cd ./build

# -O0 (capital o) stops clang from optimising
clang ../src/main.c -O0 -o main.exe
```
> For MSVC, additional files ending in `.pdb` should be created in your build directory if this is successful. This contains all the debug info required for the executable. Using clang and gcc, no additional files will be generated.
{: .prompt-info }


That's it! You're finally setup and ready to write code.


## Basic rules
Before getting into anything else, let's go over the basic rules or "syntax" of C.


Syntax is the grammar of a programming language. It informs the compiler about what parts of your code are related.


The old example:

*"The Panda eats shoots and leaves"*


is very different from:

*"The Panda eats, shots, and leaves"*


We as humans may be able to infer when someone has made a logical error and correct it in our heads, but the compiler requires specificity.


These will begin to make more sense as we go but the basic rules are as follows:
- A `;` (semi-colon) must appear at the end of a statement, exceptions being: conditional statements like `if`, `for`, `while`, `switch`, etc and function implementations. Anything before a `;` is considered part of the same statement, so statements can span multiple lines.
- `//` Denotes a comment until the end of the line, the text is not considered for compilation.
- `/*` Denotes a comment until a matching `*/`  is found, this can span over multiple lines.
- `()` Denote that whatever is inside them should happen before anything outside of them. The same as in mathematics. 

Other syntactic rules will be explained when they are relevant to what's being discussed.


Outside of syntax there is one rule that is fundamental to mathematics, dividing by `0` is undefined in C, and can cause unexpected behaviour such as a crash. However, multiplying by `0` is valid. 


We cannot divide something by `0` because `0` goes into any number infinite times. Whereas multiplying by `0`; `0` sets of any number is still `0`. We can make a human judgement what to do in the case of dividing by `0`, but a computer has to follow a set of procedures to the letter, since this behaviour is undefined, it's difficult to determine what the computer will actually do. So it's safest for us, the programmers, to tell it to not divide, but instead, skip, or do something else instead dependant on the situation.

## Variables
Before I explain what a variable is, it might help to know why we use them. Fundamentally, a program is a state that output in some way. If we modify the state with inputs the state changes and therefore the output changes.


The way we store the state is by using variables.

### What is a variable?
A variable is value that is defined by the programmer and given a name to refer back to later. 


Essentially you're telling the compiler: "Hey here's this thing I want to talk about, whenever I use this word, I'm specifically referring to this instance of this thing"


It's a pretty broad definition, so might not be clear at first. To be more specific, it's a way for us to map a point and size in memory to a name, so we can describe what we want to do with it more easily. 


This might also be a bit difficult to understand at first if you aren't already familiar with how a computer works. So to give a brief overview:
- In your computer you have Physical Memory (RAM) and a CPU.
- Physical Memory is used to store the current state of applications, this state is lost when you close the application or the computer is turned off.
- The CPU runs instructions, some of these instructions will tell it to grab some point in memory. Further instructions will tell the CPU to modify that memory in some way and return it to where it found it.

In order to tell our computer which memory to grab, we use variables as a human readable shorthand.

> We won't go into it too much here but there's also another step in-between your CPU and Physical Memory. Your OS will have what's called Virtual Memory which is used to manage physical allocations.
{: .prompt-info }
> There are quite a few terms that are used interchangeably when talking about variables, sometimes people will use different terminology based on the context. For example, "argument" usually refers to a variable that is being passed into another process, method or function. However, sometimes people will also call these "parameters". These are all the same concept.
{: .prompt-info }

### Variables in C
C and C++ are "Strongly Typed" languages, this means we have to be explicit about the size and layout of the variables we use.


Other languages, will let you describe variables in a looser way, often just using the word `var` or `let` to say you're defining a variable. In C we need to tell the compiler what kind or `type` of variable we're describing.


A `type` will tell the compiler what, and how big, a variable is.


To declare a variable in C and C++ use a type and a variable name, formatted like this: `type VariableName = value;`


This will create a variable of type `type` with the name `VariableName` and then assign it the initial value `value`.


For a more real example, if we wanted to create an unsigned 8-bit integer value called `Byte`, with the initial value `0` we would write the following: 
`unsigned char Byte = 0;`


We can then change the value this variable stores like this: `Byte = 2;`


After which, the variable `Byte` will equal `2`


We'll get more into what types of variables you can use in the following sections.


> The capitalisation of variable and type names is a stylistic choice, as long as there are no spaces in the name it will compile. The type must match the case of it's definition.
{: .prompt-info }


#### Bits and Bytes

To properly describe types in C, it will be useful to have a basic understanding of how memory is used.


In a conventional computer, data is stored as either 0 or 1, this is called a "bit". 


Since we want to represent numbers that are bigger than 1, computers have a way of expressing larger numbers by using multiple bits. A block of 8 bits is called a "byte". All memory in C is accessed in bytes, meaning the minimum number of bits a variable can have is 8.


The computer can then assign a whole number value to each of the bits in a byte. A combination of these bits can represent values in-between these whole number values. 


Bits are read right to left, the value each bit represents is double the one that came before it. For example:


```
In a single byte variable each bit maps to 
the following values
0   0   0   0   0   0   0   0
^   ^   ^   ^   ^   ^   ^   ^
128 64  32  16  8   4   2   1

so:

0 0 0 0 0 0 0 1 = 1,
0 0 0 0 0 0 1 0 = 2,
0 0 0 0 0 0 1 1 = 3,
...
1 1 1 1 1 1 1 1 = 256

```

The computer can then string multiple bytes together to express larger numbers, continuing to double the value from the last bit of the previous byte. Making it possible to store even bigger values.
```
     Byte Boundary
00000000  |  0   0   0   0   0   0   0   0
       ^     ^   ^   ^   ^   ^   ^   ^   ^
       256   128 64  32  16  8   4   2   1

```

> Values that require more than 1 byte can be read in different ways. This is referred to as Endianess. Big-Endian means the bytes that represent larger values are stored first. Little-Endian means the bytes that represent smaller values are stored first. E.g. a 2 byte (16 bit) value with the bytes 00000001 00000000 would be 256 in Big-Endian, and 1 in Little-Endian. Commonly Big-Endian is used, but some use cases may mean Little-Endian is optimal.
{: .prompt-info }

#### Variable Types
Base types in C are:

| Type               | Size      | Description           |
| :----------------- | :-------- | :-------------------- |
| bool               | 1 Byte    | 1-bit integer         |
| char               | 1 Byte    | 8-bit integer         |
| short              | 2 Bytes   | 16-bit integer        |
| long               | 4 Bytes   | 32-bit integer        |
| long long          | 8 Bytes   | 64-bit integer        |
| float              | 4 Bytes   | 32-bit floating point |
| double             | 8 Bytes   | 64-bit floating point |

> An Integer is a whole number, or a number without a decimal point.
{: .prompt-info }

> A Floating point is a number that contains a decimal point.
{: .prompt-info }

> There is also the type `int`, however this is loosely defined and is sometimes a 16-bit integer and other times a 32-bit integer. It's usually best to be explicit and use `short` and `long` instead so there's no mistaking what the `type` actually is.
{: .prompt-info }

For integers, there are also `signed` and `unsigned` keywords that can go before the type. These determine if the value can be negative (`signed`) or if it can only be positive (`unsigned`).


For example an `unsigned short` is a 16-bit integer that ranges from 0 to 65,535.


Where as a `signed short` is a 16-bit integer that ranges from -32,768 to 32,768. 


The `signed` and `unsigned` keywords do not change the size of the type, which is why the maximum positive value of an `unsigned` integer, is double the maximum positive value of a `signed` integer. The `signed` type needs to use half of it's bits to store values below zero. As a result if you assign an `unsigned short` with the value `-1`, when you read the value of the `unsigned short`, it will return `65,535`, because it cannot store negative values, it wraps around to the max.


There are other keywords that tell the compiler what to expect when using this variable. These are:

- `const`: Tells the compiler that this value will never change after being set, this can be helpful for optimisation and let's other programmers better understand the intent of the variables use.
- `static`: Tells the compiler that this variable is the same memory in every instance it's used. This means it's value is persistent even if it would ordinarily be removed from memory. Often this is used for global variables or functions (variables or functions that can be accessed from anywhere).
- `volatile`: Tells the compiler that this variable is likely to change, this might not be obviously useful at first, but when it comes to multi-threading it's necessary to tell the CPU to re-fetch the variable's value, even if it wouldn't need to do it under regular circumstances.


In addition to these types there is a special non-type called `void` void stores no information and is a way to tell the compiler that nothing is expected. This will become more relevant later on.


> When assigning values to variables, there are suffixes that tell the compiler what kind it is. The number `10` for example. `10` by itself implies it's an integer, `10.0` implies it's a double, and `10.0f` implies it's a float. There are more, but these are the most common. 
{: .prompt-info }

> You can use the word `long` before a type to double it's bit count. E.g `long long long` results in a 128 bit integer, and `long double` results in a 128 bit floating point.
{: .prompt-info }


#### Operators
Operators are the name we use for mathematical symbols, or characters that function like mathematical symbols, in programming. For example `-`, `+`, `/`, `*`, and `=` are all used as operators in C.

- `+` is used for addition.
- `-` is used for subtraction, or to make the number to the right of it negative.
- `/` is used for division.
- `%` is used for modulo, the remainder of an integer division.
- `*` is used for multiplication, as well as defining and accessing pointers.
- `=` is used to set the value of a variable.

In addition to this, you can use any `+`, `-`, `/` and `*` operator in conjunction with the `=` operator as a short hand for:

`Value = Value [operator] OtherValue`

For example, `Value += OtherValue` is the same operation as: `Value = Value + OtherValue`


You can also use `--` or `++` as a shorthand for adding/subtracting `1` to/from the value.


E.g. `Value++` is the same as: `Value = Value + 1`

`Value--` is the same as: `Value = Value - 1`


All of these you may have used before when typing into a calculator, but C also has some that you might not have seen.

##### Comparisons
- `==` Equal, used for comparing values, results in `true` if values to the left and right are equal, otherwise results in `false`. E.g. `2 == 2` results in `true`, `1 == 2` results in `false`


- `!=` Not equal, used for comparing values, results in `false` if the left and right values are the same, results in `true` if they are different. E.g. `1 == 2` will result in `true`, `2 == 2` results in `false`.


- `>` Greater than, used for comparing values, if the left value is greater than the right, results in `true` otherwise results in `false`. E.g. `1 > 0` results in `true`, `1 > 2` results in `false`.


- `<` Less than, used for comparing values, if the left value is less than the right value, results in `true`, otherwise results in `false`. E.g. `0 < 1` results in `true`, `1 < 0` results in `false`.


- `>=` Greater than or equal, used for comparing values. If the left value is greater than, or the same as the right value, results in `true`, otherwise `false`. E.g. `1 >= 1` results in `true`, `1 >= 0` results in `false`.


- `<=` less than or equal, used for comparing values. If the left value is less than or equal to the right value, results in `true`, otherwise results in `false`. E.g. `1 <= 1` results in `true`, `1 <= 0` results in `false`.


- `&&` logical AND, results in `true` if the statements to the left and right are `true`, otherwise returns `false`. E.g. `( 2 == 2 ) && ( 1 == 1 )` returns `true`, `( 2 == 1 ) && (1 == 1)` results in `false`.


- `||` logical OR, results in `true` if either of the statements to the left or right are `true`. E.g. `(1 == 0) || (2 == 2)` results in `true`, `(1 == 0) || (2 == 0)` results in `false`


- `!` inverts the result of a comparison. E.g. `!(1 == 1)` results in `false`, `!(1 == 2)` results in `true`.


> `true` and `false` are actually just `1` and `0` respectively. This means you can use the result of a comparison as a multiplier by casting, e.g.: `Value = (long)(OtherValue > 0) * Multiplier`. This will only factor in the multiplier value if `OtherValue` is greater than `0`. Useful for what's called "branchless" programming, which is a more advanced topic, but may be useful to know.
{: .prompt-info }

##### Bitwise operations

> Bitwise operations are a bit more of an advanced topic. I wanted to include them here to encourage thinking about data as bits and bytes, rather than a higher level concept. Long term this mindset can help you come up with solutions that exploit the layout of memory to gain performance or reduce code complexity. So don't worry too much if you don't 100% internalise the following information.
{: .prompt-warning }


- `&` used for AND-ing bits together, results in a value where both values have the same bit set. E.g. `0010 & 0110` will result in a value with the bits `0010`. `&` is also used to get the memory address of the variable to the right of it e.g. to get the memory address of `Value` use `&Value`.


- `|` used for OR-ing bits together, results in a value where either value have a bit set. E.g. `1010 | 0110` will result in a value with the bits `1110`.


- `^` is used for XOR-ing bits together, results in a value where a bit is set to `1` but only in one of the values not both. E.g. `1011 ^ 1001` will result in a value with the bits `0010`.


- `~` used for flipping the bits of a single value. E.g. `~1011` will result in a value with the bits `0100`


- `<<` Used for left shifting the bits of a value, this is when all the bits are moved a number of places to the left. E.g. `0010 << 1` results in `0100`, `0010 << 2` results in `1000`. This also has the property of doubling an unsigned integer.


- `>>` Used for right shifting the bits of a value, this is when all the bits are moved a number of places to the right. E.g. `0100 >> 1` results in `0010`, `0100 >> 2` results in `0001`. This also has the property of halving an unsigned integer.


- `[]` Used with pointers as a shorthand for getting the variable that's at: `Ptr + (sizeof(ptr_type) * X)` number of bytes ahead of the ptr specified, where `X` is an integer value. Used like `type Value = Ptr[2];`

#### Typedef
It can be useful to define shorthand for these types so we don't have to keep typing out things like `unsigned long long` or try to parse all the visual noise this would produce when reading though your code.


We can do this by using the `typedef` keyword.


`typedef` allows you to assign an alias to another type, telling the compiler that when you use that name, you're referring a variable that is the same size and layout of this type.


The syntax for this is: `typedef type new_type;`

For example if I want to map `unsigned short` to `u8` (shorthand for an unsigned integer with 8-bits). I would do the following:


`typedef unsigned short u8;`


Now I can use `u8` throughout my code in place of `unsigned short`.


The standard library has some typedefs for common types already, but personally I prefer to write my own.


> In base C,`typedef` is also required for any custom types we create to tell the compiler to expect them to be used as arguments for functions (more on that later).
{: .prompt-info }

#### Enum
`enum`, short for Enumerated Type, is a way of grouping constant integer values, when it is beneficial to map these integers to a more descriptive name.


For example: 

`u32 Type = entity_type_enemy;`


Is more descriptive and easier to remember than:


`u32 Type = 2;`


While you could do this just by defining a series of constant integers like this:

```
const u32 entity_type_none = 0;
const u32 entity_type_player = 1;
const u32 entity_type_trader = 2;
const u32 entity_type_enemy = 3;
const u32 entity_type_physics_object = 4;
```
It's often better to create an `enum` like this:
```
typedef enum entity_type
{
    entity_type_none,
    entity_type_player,
    entity_type_trader,
    entity_type_enemy,
    entity_type_physics_object
}entity_type;
```
> The `typedef` is not required in C++, you define an enum by just using the word `enum` and then the contents, like this: `enum entity_type { ... };`
{: .prompt-info }

You can then use the `enum` type `entity_type` as a variable for storing data and passing into functions.

Like so: `entity_type Type = entity_type_enemy;`

This Makes it easier to follow what the code is doing, if you see something that has an `entity_type` you know that it's probably an entity, and you know exactly what to search for when looking for all possible entity types.


Due to their enumerated nature, if you don't explicitly set a value, each entry is `1` greater than the one that came before it. Allowing us to change the order they're in, and add or remove them, without introducing bugs.


The compiler will do this for us:
```
typedef enum entity_type
{
    entity_type_none = 0,
    entity_type_player = 1,
    entity_type_trader = 2,
    entity_type_enemy = 3,
    entity_type_physics_object = 4
}entity_type;
```
> If unlabelled, it is assumed that the first entry is assigned the value `0`.
{: .prompt-info }
Because of this, we can use another trick that helps when we want to iterate over each type.


Iteration will be explained in more depth later, but in this context, the word describing looping over a group of variables, doing something for each. Most commonly we give it a start count and max count of iterations it should do.


With enums, we can add a `_count` or `_max` value to the end like this:
```
typedef enum entity_type
{
    entity_type_none,
    entity_type_player,
    entity_type_trader,
    entity_type_enemy,
    entity_type_physics_object,
    entity_type_count
}entity_type;
```
Then keep adding entity types just before this last value
```
typedef enum entity_type
{
    entity_type_none,
    entity_type_player,
    entity_type_trader,
    entity_type_enemy,
    entity_type_physics_object,
    entity_type_item,
    entity_type_count
}entity_type;
```
`entity_type_count` will always accurately reflect how many entity types there are as long as it's the last value specified.

#### Struct
We can also build up more complex types from these basic types in data structures called `structs`.


A `struct` is a way of grouping multiple variables together in the same `type`. This can be useful for passing around one variable rather than 6 or 7 every time you want to operate on that data. Allowing you to more easily perform common operations on sets of data.


For example, let's take a look at a struct we might expect to see in game development: `vec3`

```
typedef struct vec3
{
    float X;
    float Y;
    float Z;
} vec3;
```
> The `typedef` is not required in C++, you define a struct by just using the word `struct` and then the contents, like this: `struct vec3 { ... };`
{: .prompt-info }

A `vec3`, or 3D Vector, is a mathematical concept used to store both direction and distance (magnitude). I won't go into it too much here, but they are fairly common in game development (and other 3D applications) due to their ability to represent locations.


They are made up of 3 floating point values describing translation in the X, Y and Z axes.


Since vectors are used frequently, and you'll want to perform operations on them to calculate a new state for the game. It makes sense to group it like this for convenience.


You can assign a `struct` like you would any other variable: `vec3 Position;`


However when it comes to setting the initial value, how do we know which value to set? There are 3 values, so how do we tell the compiler which one we want to define?


To do this, you use what is called an "initialiser list":


`vec3 Position = { 10.0f, 0.0f, 1.0f };`

This sets the values of the contained variables in order the list is given. So in this case `X = 10.0f`, `Y = 0.0f`, and `Z = 1.0f`


You can also assign them one by one, over multiple lines, like this:
```
// = {0}; sets all contained values to 0 in C
// = {}; does the same thing in C++
vec3 Position = {0};
Position.X = 10.0f;
Position.Y = 0.0f;
Position.Z = 1.0f;
```


One feature unique to C that C++ doesn't have, is the ability to assign variables in a struct through an initialiser list without using the order they appear in the struct definition.

`vec3 Position = { .X = 10.0f, .Y = 0.0f, Z = 1.0f };`

Here we explicitly set each of the values, so if we change the order of the data in the struct, we won't create any unforeseen bugs.


You can also assign `struct` values the values of another `struct` if they are of the same type. For example, if I wanted to copy the value of `Position` into a new variable, so I can modify it without changing the original. I could do this:


```
// not quite a real use case, simplified for explanation
float Speed = 10.0f;
vec3 Position = { 10.0f, 0.0f, 1.0f };
vec3 PredictedPosition = Position;
PredictedPosition.X += Speed;
//... check predicted location if it's safe to move in
```
In this case, `Position` still has the value `X = 10.0f, Y = 0.0f, Z = 1.0f`, and `PredictedPosition` has the values `X = 20.0f, Y = 0.0f, Z = 1.0f`.

> You can initialise structs with `const` values like this `const vec3 ZeroVec3 = {0}; ... vec3 Position = ZeroVec3;` This can be useful for easily resetting variables to a zeroed state.
{: .prompt-tip }

#### Union
A `union` is pretty similar to a `struct`, however rather than containing one instance of each variable contained. It only contains one instance of the largest variable contained. Each entry in the `union` shares memory, so When you modify one variable in the `union` you're modifying all variables in a union.

Ok so I lied a little in the last section, I don't use `struct vec3` in my code. I actually use `union vec3`

I can explain, I promise.


The mathematical concept of a Vector allows you to scale the number of axes to any value you'd like. Most commonly I find myself using Vector 2D, Vector 3D and Vector 4D. Depending on the problem I'm trying to solve. As a result I would end up with structs: `vec2`, `vec3` and `vec4`, and if I just wanted to get the X and Y components from a `vec3` and use them as a `vec2`, I'd have to create a new `vec2` and assign it like this:
```
vec3 Position = { 10.0f, 0.0f, 1.0f };
vec2 Position2D = { Position.X, Position.Y };
//... do stuff, pass vec2 into functions,
// assign it to a new value
Position.X = Position2D.X;
Position.Y = Position2D.Y;
```

While this only adds a couple lines of code, if you're doing it often, it can quickly build up. It also adds extra steps for the CPU.


Using a `union` solves this problem, because we can actually just say that a `vec2` is a union containing a `struct` with `X` and `Y` values, and a `vec3` is a `union` containing a `struct` with a `vec2` and a `Z` component.

Like this:
```
typedef union vec2
{
    struct 
    {
        float X;
        float Y;
    }
    float Array[2];
}vec2;

typedef union vec3
{
    struct
    {
        float X;
        float Y;
        float Z;
    }
    struct
    {
        vec2 XY;
        float Z;
    }
    struct
    {
        float X;
        vec2 YZ;
    }
    float Array[3];
}vec3;
```
*The `float Array[3];` and arrays in general will be explained later but I wanted to include them to show you can use them inside a union.*
> The `typedef` is not required in C++, you define a union by just using the word `union` and then the contents, like this: `union vec3 { ... };`
{: .prompt-info }


Using the `union` `vec3` we can access the values `X`, `Y`, and `Z` as we could with a struct, but we can also use `XY` to get the values of `X` and `Y` in the form of a `vec2`.


Since in a `union` the memory is the same for each entry, and since each entry is a `struct` containing 3 floats. Anything we do to `XY` will change the value of both `X` and `Y`.

## Functions
### What is a function?
Functions are a type of variable that is used creating groups of code so that you can run it multiple times.


Taking the `vec3` example from before. If we want to add two vectors together, the mathematical procedure is to take each of the components from the two vectors and add them together like you would any regular number. 


In the case of `vec3` we have 3 values, `X`, `Y` and `Z`, so the code to do that would look like this:

```
// assuming these are defined somewhere
// vec3 PositionA;
// vec3 PositionB;

vec3 Result = {0};
Result.X = PositionA.X + PositionB.X;
Result.Y = PositionA.Y + PositionB.Y;
Result.Z = PositionA.Z + PositionB.Z;
```


Imagine having to write that out every time you wanted to add two vectors together. As this is an pretty common operation to do in a game or 3D application, you'd quickly find yourself spending more time writing those 3-4 lines of code, than you would actually making the application.


This is where functions come in, we can define an output given a set of inputs. These inputs are variables, often called arguments or parameters. Functions are defined like this:
```
vec3 vec3_add_vec3(vec3 VecA, vec3 VecB)
{
    vec3 Result = {0};
    Result.X = VecA.X + VecB.X;
    Result.Y = VecA.Y + VecB.Y;
    Result.Z = VecA.Z + VecB.Z;

    return Result;
}

// this function can then be called like this

int main(int argc, char** argv)
{
    vec3 PositionA = { 1.0f, 2.0f, 3.0f };
    vec3 PositionB = { 0.0f, 10.0f, 3.0f };
    vec3 Result = vec3_add_vec3(VecA, VecB);
}

```

> `return` followed by a value is used to tell the compiler: *"this function is going to exit here, and the result of this function is this value"*
{: .prompt-info }

Going through the top line of the function vec3_add_vec3 left to right:
- The first string of text before a space is the "`return` type" this specifies what type of variable this function will give back to you, or what it 'returns'. In this case it returns a variable of the type `vec3`
- Then after a space we have the function name, this is the variable name the function will be assigned to, whenever you write this name, passing it the correct parameters, the function will be ran or 'called'. The name of this function is `vec3_add_vec3`
- Finally, contained within `()` we have the variables this function takes as inputs these are separated by `,`. These are variables local to this function and will cease to exist after we return a value. The arguments of this function are a `vec3` called `VecA` and a `vec3` called VecB.

Then it's use:
- The variable the result / output will be stored in, you can declare the variable in the same line as the function. Here we are storing the result in a new `vec3` called `Result`, though we could actually store this in an existing variable like `VecA = vec3_add_vec3(PositionA, PositionB);`. As in this case, the function does not modify the values being passed into it.
- Then after the `=` operator, the function you are calling. Here we're storing the output of `vec3_add_vec3` in the new `vec3` variable; `Result`.
- Then within `()`, the variables you want to give to the function. Here we're passing in `PositionA` and `PositionB`, to be used as `VecA` and `VecB` inside the function respectively.

> You need not actually store the result of the function for you to be able to call it, sometimes the result is an integer value that describes what state the function exited in. For example the entry point function: `int main(int argc, char** argv)` returns an exit code, if this value is not `0` it means something happened causing the application to not finish successfully. If you didn't care about the status the function exited in, you could just ignore it by calling `main(argc, argv);` rather than `int Result = main(argc, argv);`
{: .prompt-info }

Functions (outside of structs and classes) are entirely self contained and have no information from where they were called from. The only context the above function knows are the two variables that were passed into it, as well as other globally defined variables and functions. While this might seem like a hassle, it can be a blessing in disguise. If your function only knows about the stuff passed into it, you're encouraged to write code that is completely decoupled from external context. Making it much easier to reuse it in different situations where you may not have that context. The best kind of function, in my experience, is one that can be called from anywhere at any time without issues.


You can also "forward declare" a function, forward declaration is a process in which we tell the compiler: 

*"ok so there will be something called this coming up later, work under the assumption that it will exist when you get to the linking stage"*


This works with types and functions where we don't actually need to know the layout or implementation just yet. This will come up more in the next section, but focusing on functions, we can tell the compiler that this function exists ahead of it's implementation. This can be useful when you want to call the function from another file without needing to share the implementation and it's dependencies. The value of doing this will hopefully become clearer when we get to later sections.


You can forward declare a function by doing the following:

`vec3 vec3_add_vec3(vec3 VecA, vec3 VecB);`


As long as you actually implement the function fully somewhere else.


The most common practice is to declare functions you want other files to be able to call inside a "header" file (a type of C file that has the extension .h rather than .c or .cpp). Then define the implementation of the function within a .c or .cpp file of the corresponding name. However there are no hard and fast rules within C that enforce this, so if a situation arises where it's advantageous to do it differently, there's nothing stopping you. You can even do it within the same file if you want to like this:
```
vec3 vec3_add_vec3(vec3 VecA, vec3 VecB);

void other_function()
{
    //.. code assuming:
    // vec3 Position;
    // vec3 Movement;
    // are defined
    vec3 NewPosition = vec3_add_vec3(Position, Movement);
    //.. code
}

vec3 vec3_add_vec3(vec3 VecA, vec3 VecB)
{
    vec3 Result = {0};
    Result.X = VecA.X + VecB.X;
    Result.Y = VecA.Y + VecB.Y;
    Result.Z = VecA.Z + VecB.Z;

    return Result;
}

```

This is valid code because we told the compiler it will exist before using it, then gave the compiler the definition.


If we want to write a function that doesn't return anything, the `void` type is used, telling the compiler that the function will not return anything. In this case, the function doesn't have to use the `return` keyword. It is assumed that at the end of the `{}`, the function returns.


Operators like `+`, `-`, `/`, and `*` can be considered a type of built in function. `long Result = Integer + OtherInteger;` can be thought of as calling a function defined as: 
```
// sorry, this might not be as beginner friendly as I thought
// but it's too late to turn back now
// because it is a good example.
// I've never actually implemented this now I think about it,
// so we're both learning something today.
// might overlook some issues but I *think* this is how a binary
// add works
long +(long LeftHandValue, long RightHandValue)
{
    long XOR = LeftHandValue ^ RightHandValue;
    long AND = LeftHandValue & RightHandValue;

    while (AND != 0)
    {
        AND = AND << 1;
        XOR = AND ^ XOR;
        AND = AND & XOR;
    }

    return XOR;
}
```


In C++ you can actually `overload` these operators. This tells the compiler, if we see two variables of this `type` with the operator between them, call this function. This is not possible in C as C does not support multiple functions with the same name.


Using the example of adding two `vec3` variables we had earlier, you would write it like this:

```
union vec3
{
    struct
    {
        float X, Y, Z;
    }; 
    struct
    {
        vec2 XY;
        float Z;
    };
    struct 
    {
        float X;
        vec2 YZ;
    }
};

vec3 operator +(vec3 LeftVec, vec3 RightVec)
{
    Result = {0};
    Result.X = LeftVec.X + RightVec.X;
    Result.Y = LeftVec.Y + RightVec.Y;
    Result.Z = LeftVec.Z + RightVec.Z;
    return Result;
}
```

Now, whenever we type `vec3 Result = VecA + VecB;`, this function will be called.


> Operators that modify the value left of the `=`; like `+=` must be defined within the `struct` or `class` you're using the `+=` operator on, the function defined like this: `vec3& operator +=(vec3 RightVec)`. This uses something we haven't covered called a "reference" denoted by the `&` symbol after the type name. References are a lot like pointers, only they cannot be null or `0`. This means you have to assign them to point to a value when they are initialised. I prefer to use pointers instead because they are more versatile. However, if you're using C++, they are required for this operator.
{: .prompt-info }


#### Returning Early
There are some cases where you can take an early exit from a function, either because the inputs fall into a case where calculating the output is simplified, or calculating the output would be impossible given the inputs. 


A simple case for this is dividing by `0`, this is not a valid operation and will cause unexpected behaviour, like a crash. In this case, if our function is going to divide by a number that could possibly be `0`. We may want to `return` early.


As an example, dividing a vector by a scalar value is often used to normalise it:
```
float vec3_length(vec3 Vec)
{
    return sqrtf((Vec.X * Vec.X) + (Vec.Y * Vec.Y) + (Vec.Z * Vec.Z));
}

vec3 vec3_div(vec3 Vec, float Divisor)
{
    vec3 Result = {0};
    
    // returning early if the value we're dividing by is 0
    // we haven't gone into 'if' statements yet,
    // we'll cover them more later.
    if (Divisor == 0.0f)
    {
        return Result;
    }

    Result.X = Vec.X / Divisor;
    Result.Y = Vec.Y / Divisor;
    Result.Z = Vec.Z / Divisor;

    return Result;
}

vec3 vec3_normalise(vec3 Vec)
{
    return vec3_div(Vec, vec3_length(Vec));
}
```
Don't worry too much about the maths here, the key take away is we want to divide in the `vec3_div` function. We check before doing the division, if the value is `0` we return the zeroed result.

## Pointers
Pointers are often something that puts people off C and C++. Even if you're not just getting started, they can appear to be complex and a bit scary. In my opinion, they're Cs strongest feature and can be as simple as you like.

### What is a pointer?
Every time we've created a variable so far, we've been allocating memory for that variable behind the scenes. A pointer is a `type` of variable that does not contain any data from the variable itself, but rather *points* to a memory address of where an actual full value is. On a base level it's just an integer type that stores a memory index, the size of the integer is dependant on your system architecture. These days it's pretty rare to have a 32-bit system, but if you do have one, it's a 32-bit integer. On 64-bit architecture it's a 64-bit integer. It just needs to be large enough to express the theoretical maximum memory address your system can have. 

### How to use pointers
You can create a pointer by using the `*` operator after the `type` you want it to point to, and assign it to the address of another variable using `&` like so:
```
vec3 Vec = {0};

// VecPtr now points to the variable Vec
vec3* VecPtr = &Vec;
```
> Depending on personal preference it is sometimes written as `vec3 *VecPtr = &Vec`. Both are valid and will compile.
{: .prompt-info }


Now that `VecPtr` points to the memory address of `Vec` we assign `Vec` using `VecPtr` using a process called "dereferencing", accessing the variable it's pointing to.


This is done using the `*` operator again, like this:

```
vec3 NewValue = { .X = 2.0f, .Y = 0.5f, .Z = 1.0f };

*VecPtr = NewValue;
```
The value `Vec` will now be set to the same value as `NewVec`.

We can also modify variables within a `struct` or `union` by using `->` where we would use `.` to access a variable of a `struct` or `union` before. For Example:
```
VecPtr->X = 1.0f;
VecPtr->Y = 5.0f;
VecPtr->Z = 0.5f;
```
The `X`, `Y` and `Z` values of Vec will now be equal to: `1.0f`, `5.0f` and `0.5f` respectively.


You can also use pointers as function arguments, allowing you to modify the value the pointer points to. 

As an example, we can re-arrange the `vec3_add_vec3` so we no longer return a `vec3` but instead take a `vec3*` as an argument.

*Though I wouldn't personally recommend it in this case but that's more of a personal preference*

```
void vec3_add_vec3(vec3* Base, vec3 Added)
{
    Base->X += Added.X;
    Base->Y += Added.Y;
    Base->Z += Added.Z;
}

// calling it
int main(int argc, char** argv)
{
    //... code defining
    // vec3 VecA;
    // vec3 VecB;
    
    // this will result in VecA equaling VecA + VecB
    vec3_add_vec3(&VecA, VecB);
}
```

That's it really. The basic use is fairly simple once you get used to it.


#### Function pointers
Functions are also a kind of variable with a static memory address, this means we can also create a pointer to a function and call it through that pointer. I was actually surprised how simple this can be. After using game engines and the STL I had this idea that getting function pointers to work was complicated. It was only when I started writing code from scratch, I learnt how simple it actually is.


To create a function pointer, first you need to create a type to store it in. We can do this by using our old friend `typedef`:

```
// define the function type to store the functions address
typedef void os_open_file_func(const char* Filepath);


// then to use...
// create a function that matches the above return type and arguments
void linux_open_file(const char* Filepath)
{
    //... code
}


int main(int argc, char** argv)
{
    // create a pointer variable,
    // and assign it the address of linux_open_file
    os_open_file_func* OpenFile = linux_open_file;

    // call the function through the pointer
    OpenFile("/path/to/file.file");
}
```

Function pointers can be useful for a number of things, in this example, we're using them to map a platform specific implementation to a variable we can access in our main body of code. This makes it much easier to port applications to other platforms later on. For example, we can create a struct called `platform` which stores all necessary platform function pointers and application data. Then use the data on that struct throughout our program. Now any changes we make to the `platform` struct, and the functions it points to will just work when we assign new implementations for a different platform. 

### Considerations when using pointers
One of the things I like about pointers in C is they don't restrict you. As long as the types match, the code will compile and run. It will even allow you to interpret a variable as if it's another type entirely.


This does open the door to runtime errors if you don't have a game plan, if the pointer is not assigned (`0`), or if it points to memory the application shouldn't have access to. Then you try and dereference it, the program will crash. Due to memory security, if you were able to set a pointer to a space in memory that is in use by your OS, then use it to modify that memory. You could cause unexpected behaviour across your system, and not just contained to your application. Malicious programmers could also exploit this to modify your OS to allow remote access. As a result your OS just won't allow you to read or write memory that it takes ownership of.


C and C++ however, are generally considered "memory unsafe" which means that there is no checking if you're trying to access a memory address outside the bounds of the memory allocation you asked for. This is somewhat a double edged sword, with less rules, you're able to do more with pointers, however it can lead to bugs. For example, if you create a pointer and assign it the memory address of a value. If that value stops existing for whatever reason, and you try to modify the data through the pointer. You'll now modify whatever took it's place in memory. Causing undefined behaviour in your application, potentially leading to a crash. These issues are pretty hard to track down since the errors they produce will often be entirely unrelated to the actually issue that caused them.


These pain points can be eased by how you manage memory, but that's a topic for another post.

## Arrays

### What is an Array?
An array is the name we use to refer to a buffer, group or list of variables.


There are cases where it makes sense to store your variables in an array so you can go over each of them and perform some operation without having to write all that code by hand, naturally scaling with however many variables you put in the array.


I won't get too into it, since the concept of an array overlaps with memory management, which I wanted to cover in it's own blog post. But the simplest version of an array can be done without going into that topic by using an area of memory called the "stack". Just know there will be more in the future... you are not safe from arrays...


### Creating an Array on the Stack
Creating an array uses similar syntax to creating a variable, only you have to specify how many variables you'd like in the array. Like this:

`float FloatArray[10] = {0};`


This gets enough memory for `10` floats from the stack, and then assigns `FloatArray` to the point in memory where that block starts. Because of this, the value `FloatArray` is actually a `float` pointer, not a float itself.

The variables inside an array are often referred to as "elements" or "items". This is just the word used to describe `1` thing inside the array.


### Accessing Array elements
To access each variable in the array, you'll need to use the `[]` operator like we did when we created the array.

For example, if I wanted to get the first element, I'd write this:

`float Value = FloatArray[0];`


You can also set elements in the same way:

`FloatArray[0] = 10.0f;`


The number inside `[]` is often referred to as the "index".


Much like everything else in C, `0` is the first element not `1`. This due to how the `[]` operator works. 


Arrays use a concept called a Stride, this Stride is equal to the size of one element in the array. So in the case of an array of type `float` the stride is 4 bytes. 


The stride is used to determine how many bytes we have to jump forward to get to the next element. The `[]` operator when passed the value `X`. Calculates: `*(BaseByteAddress + (Stride * X))` Getting us the de-referenced value of the `X`th element in the array.

> If you try to access an index that is out of bounds; a number larger than (ArrayLength - 1), your program will throw an error. Since the compiler knows exactly how long the array is, it is able to catch when you're doing something potentially unsafe and throw an error. This is not the case if we treat the array as a pointer, e.g. `float*` as it doesn't really know it's an array anymore.
{: .prompt-warning }

### Getting the length of an Array
You can get the length (sometimes referred to as count) of an array created on the stack by using the `sizeof` keyword. `sizeof` works like a function, and allows you to pass in a type or variable, returning it's size in bytes.

In the case of:

`long Integer = 0;`

Either:

`sizeof(long);`

or

`sizeof(Integer);`

Will return the same number of bytes. 

>It can be beneficial to use the variable name instead of the type where you can, this allows us to change the type of the variable without having to make any changes. Whereas if we just used the type name, and we forgot to change the type passed into a `sizeof`, we will not get the correct array length. Resulting in unexpected behaviour that can be hard to track down. If we change the variable name, the compiler will throw an error where we forgot to use the new name. Meaning we are relatively safe from this bug as long as the program compiles.
{: .prompt-tip }


We can use this to find the total size of the array on the stack, then divide the total size by the size of one element in the array. Like this:

```
{
    char Filepath[256] = {0};
    long FilepathLength = sizeof(Filepath) / sizeof(Filepath[0]);
}
```
> We can assume all arrays on the stack have a length of at least `1`, since the C specification does not allow for stack arrays to have a size of 0. So it's safe here to use `[]` to access the first element and get it's size.
{: .prompt-info }
> Note: this will give us the maximum number of elements that *can* be inside the array, not how many we put inside it. You'll need to keep track of that yourself if needed. More on this later down the line!
{: .prompt-info }


### Passing an Array into a function
Since Arrays are just a pointer to the start of a block of memory, you can pass an array into a function as a pointer. It's important to also pass in the length of the array along with it, since once it's a pointer, we won't be able to determine the size of the full Array. Using `sizeof` on a pointer, will just give you the size of the pointer, which on a 64 bit system, will always be 8 bytes. 

In the case of the Array `char Filepath[256] = {0};`

The function declaration would like this:

```

// function that does something to the array
void do_something_with_array(char* Array, long ArrayLength);

int main(int argc, char** argv)
{
    char Filepath[256] = {0};
    long FilepathLength = sizeof(Filepath) / sizeof(Filepath[0]);

    do_something_to_with_array(Filepath, FilepathLength);
}

```


More on iterating over arrays later, but that's the basic concept!

## Conditionals: if, switch, for, and while
In a lot of cases, we want our program to do different things depending on it's current state. While there may be ways to avoid it using a "branchless" approach in some cases, the most common ways of doing this are by using `if`, `switch`, `for` and `while`. 

### if
#### What is an if statement?
`if` allows you to specify a condition, `if` that condition is met or `true`, the code after it contained within `{}` will be ran. This is done using the operators we talked about before. An example of this would be:
```
void some_func(char* Ptr)
{
    // or if you'd prefer if (Ptr == 0)
    if (!Ptr)
    {
        return;
    }

    // rest of function
}
```
> `if` statements are not ended with a `;` they are ended when a matching `}` is found
{: .prompt-info }

This is an early `return`, similar to the example I've used before. In this case `if` the pointer we pass in is null (`0`), then we `return` out of the function early without running any code that comes after it. Ensuring we do not generate a crash here. 

#### else and else if
There are two other variants of `if`: `else if` and `else`. These can only be used after an initial `if` statement. An `else if` statement is only entered if the initial `if` condition was not met and the statement inside the `else if` is met. For `else` the code inside the block is always ran if the previous `if` condition was not met.


`if` a statement is true, and has following `else if` checks, we do not perform any of the operations inside the following statements. Instead, we just skip over all code until the end of the last `else` or `else if`


An example of this from code I've written in the past is comparing file extensions when loading assets for a game.

```
// iterates through the `Src` string and copies into the `Dst`
// string until a character inside the 'Delimiter' string is
// found. 'Reverse' causes this function to start at the end
// chopping off anything to the left of a 'Delimiter'
// else, chops anything to the right of the 'Delimiter'
void get_string_section(char* Dst, char* Src, char* Delimiter, bool Reverse);

// returns the number of differences between the two strings
// if the strings are the same, this returns 0
long string_compare(char* StrA, char* StrB);

// returns null or 0 if the asset was not found
// otherwise returns a valid pointer to the asset.
asset* find_loaded_asset(Database, Filepath);


asset* load_asset(asset_database* Database, char* Filepath)
{
    asset* Result = find_loaded_asset(Database, Filepath);

    // if Result is a non-zero value, then we already
    // have the asset loaded and we can just return it
    if (Result)
    {
        return Result;
    }

    // otherwise, load the asset
    char FileExtension[6] = {0};
    get_string_section(FileExtension, Filepath, ".", true);

    if (string_compare(FileExtension, "png") == 0)
    {
        Result = load_image(Database, Filepath);
    }
    else if (string_compare(FileExtension, "ttf") == 0)
    {
        Result = load_font(Database, Filepath);
    }
    else if (string_compare(FileExtension, "fbx") == 0)
    {
        Result = load_mesh(Database, Filepath);
    }
    else
    {
        LOG_WARN("load_asset: Unsupported file type, returning nullptr");
    }

    return Result;
}
```
*This is more of an approximation of the function I wrote, the real function had a couple more steps and I'm not sure I'd write it the same now, but this works as a real world example.* 

Here we have a function that returns a pointer to an `asset` that is contained within another data structure; `asset_database`. 


There are a few things going on here that aren't relevant to this topic so let's focus on this section:

```
    if (string_compare(FileExtension, "png") == 0)
    {
        Result = load_image(Database, Filepath);
    }
    else if (string_compare(FileExtension, "ttf") == 0)
    {
        Result = load_font(Database, Filepath);
    }
    else if (string_compare(FileExtension, "fbx") == 0)
    {
        Result = load_mesh(Database, Filepath);
    }
    else
    {
        LOG_WARN("load_asset: Unsupported file type, returning nullptr");
    }

    return Result;
```
Going through it top to bottom:

Here we check `if` the file extension is the same as the string `"png"`, `if` the difference between the two strings is `0` then we enter into the code contained within `{}`, and load this asset as a image. Then skip over the remaining `else if` and `else` statements. Going straight to the line `return Result;`


`if` this was not the case we then check the difference between `FileExtension` and the string `"ttf"`. `if` the difference between these two strings is `0` then we run the code within `{}`. In this case, loading the asset as a font. Then jump over the other `if else` and `else` statements.


We then do the same to compare `FileExtension` with the string `"fbx"`.


Finally, if none of the above conditions were met, we run the code within the `else` statement. Causing a warning to be logged before returning `Result` which we set to the default value of `0`


#### Conditional operators
Like I mentioned before, you can use the conditional operators inside `if` statements. 


`if (Ptr && Ptr->Enabled)`: Will first check `if` `Ptr` is a non-zero value, and then check `if` the value on `Enabled` on the pointer is non-zero. Only `if` both of these conditions are met will the code inside be ran.


`if (!Ptr || !Ptr->Enabled)`: Will check `if` `Ptr` is a zero value or `if` `Enabled` on the pointer is a zero value. `if` either of these conditions are met, the code within will be ran. This is the inverse of the last case, meaning it's the same as: `if(!(Ptr && Ptr->Enabled))`


`if (Value > 0.0f)`: Will only run the contained code `if` value is greater than zero.


You can string together `&&` and `||` in the same `if` statement. 


Assuming byte is a 1 byte `char` pointer:
`if (Byte && (*Byte == '\t' || *Byte == '\n' || *Byte == ' '))`: Will first check if `Byte` is a non-zero pointer, then will check if the value pointed to is either a tab character, a newline character or a space.


Comparisons are done left to right, if we reach a point in the statement where the result can be determined, we do not continue to perform the comparisons.


This is useful in this case, since if `Byte` is a null pointer, the comparison will stop, meaning we won't try and dereference it, which would result in a crash.


It is good practice to wrap your conditions in `()` to make it clear to the compiler how the condition is determined. Anything inside `()` is compared as if it was one expression.


Without these:

`if (Byte && *Byte == '\t' || *Byte == '\n' || *Byte == ' ')` 

Could also mean: 

`if ((Byte && *Byte == '\t') || (*Byte == '\n' || *Byte == ' '))`

Which will not result in the same outcome. In this case, we will only ever run the code contained, `if` `Byte` is not null and `*Byte` is a tab character. Basically ignoring the other values we wanted to check for.
### switch
One issue with `if` statements is that we have to evaluate each of them in order until we reach one that is valid. This means, in a worst case scenario, where the valid condition is the last one. We have to do all the work contained within the `if` statements before it.


While sometimes this is necessary, if we're just comparing two integer values like this: 

```
if (Integer == 0)
{
    do_something();
}
else if (Integer == 1)
{
    do_something_else();
}
else if (Integer == 2)
{
    do_a_third_thing();
}
//...ect...
```

We can use a `switch` statement. 


#### What is a switch statement?
A `switch` statement allows you to jump straight to the condition that was met, rather than evaluating each one in order, as long as the value can be interpreted as an integer type.


Rather than writing a chain of `if`s we can do the following:
```
switch (Integer)
{
    case 0:
    {
        do_something();
    }break;

    case 1:
    {
        do_something_else();
    }break;

    case 2:
    {
        do_a_third_thing();
    }break;
}
```

#### break and switch case fall through
The `break` keyword tells the compiler to jump out of this condition, skipping the rest of the code contained. It can be used for other conditional statements as well, but here it's used here to denote where the code ran for each condition ends.

Without using `break`, the switch statement will jump to the matching case and continue to run all the code for cases below it, falling through.


Because of this, you can have two or more cases share the same codepath.


For example, if we wanted the cases for `0` and `1` to run the same code. 

Equivalent to: `if (Integer == 0 || Integer == 1)`

We can do the following:

```
switch (Integer)
{
    case 0:
    case 1:
    {
        do_something();
    }break;

    case 2:
    {
        do_something_else();
    }break;
}
```
Here if `Integer` is `0` it will jump to `case 0:` then fall through to `case 1:` running `do_something()`, then `break` out of the `switch` statement.

If `Integer` is `1` it will jump to `case 1:` and run `do_something()` then `break` out of the `switch` statement.

If `Integer` is `2` it will jump to `case 2:` and run `do_something_else()`, then `break` out of the `switch` statement.

#### default case
There is also another keyword that can be used with switch statements: `default`


This provides a `case` for any values not explicitly mentioned in the `switch` statement, if a `default` case is implemented.


For example:
```
switch (Integer)
{
    case 0:
    case 1:
    {
        do_something();
    }break;

    case 2:
    {
        do_something_else();
    }break;

    default:
    {
        LOG_WARN("Generic undefined case warning!");
    }break;
}
```
Now we've implemented a `default` case, if `Integer` is a value that isn't `0`, `1`, or `2`. The code within the `default` case will be ran.


#### Switch on enum type

Since `enum` values are just constant integer types, we can also use them in a switch statement. Using the example of `entity_type` from earlier:

```
typedef enum entity_type
{
    entity_type_none,
    entity_type_player,
    entity_type_trader,
    entity_type_enemy,
    entity_type_physics_object,
    entity_type_count
}entity_type;

typedef struct entity
{
    entity_type Type;
} entity;


{
    // assuming there's an entity pointer passed in:
    // entity* Entity
    switch (Entity->Type)
    {
        case entity_type_player:
        {

        }break;

        case entity_type_trader:
        {

        }break;

        case entity_type_enemy:
        {

        }break;
        //... ect
    }
}
```

#### Switch on string character
`switch` can also be used for characters in a string, since characters are encoded as integer values depending on the text standard used (e.g. ASCII or UTF-8).


For example, similar to our earlier example:
```
switch (*Byte)
{
    case '\t':
    case '\n':
    {
        do_something();
    }break;

    case ' ':
    {
        do_something_else();
    }break;
}
```

This is especially useful when parsing text files to extract data stored in JSON, YAML or whatever human readable type your project uses. As it allows us to create a case for skipping over whitespace like tabs, spaces and newlines. Then only actually parse anything that is noteworthy like variable names, arrays, objects, values.

### for

I may have mentioned a few times along the way that there are situations where we want to do the same thing for sets of multiple data. For example, if we have an array of `vec3` we want to go through and find the closest point to a location specified. To do this, we can use a `for` loop.

A `for` loop allows you to run the same code with a condition for how many times it should do it.

Within this condition you can specify: an initial value, a requirement for the loop to continue, and an operation to do each time the current iteration completes.

You do this by using syntax that is specific to for loops: 

`for (initial variable; condition; on iteration)`

The most common example of this, iterating over an array, looks something like this:


```
{
    for (long Index = 0; Index < ArrayCount; ++Index>)    
    {
        // do stuff
    }
}
```

In the `vec3` example I gave, it would end up looking something like this:
```

// function that gets the distance between VecA and VecB
float vec3_distance(vec3 VecA, vec3 VecB);

// this probably isn't the fastest way to do this but for simplicity:
vec3 get_closest_point(vec3* Points, long PointsCount, vec3 TestPoint)
{
    // FLT_MAX is a constant float defined as the maximum
    // positive value a float can express.
    float ShortestDistance = FLT_MAX;
    vec3 Result = {0};
    for (long PointIndex = 0; PointIndex < PointsCount; ++PointIndex)
    {
        vec3* Point = &Points[PointIndex];
        float Distance = vec3_distance(*Point, TestPoint);
        if (Distance < ShortestDistance)
        {
            ShortestDistance = Distance;
            Result = *Point;
        }
    }

    return Result;
}
```

This is probably the most common case of `for` loop use. We initialise some integer value to `0`, check the value is less than the array count, then increment the value after we finish one iteration.


But `for` loops can do much more than this. Since it's more loosely defined as `for (init; condition; iteration)`, we don't have to follow the above pattern. We can use it for all sorts of things.


For example, if we have a buffer of memory, and we know the start and end pointers, we can use those instead.

`for (float* Current = FloatBlockStart; Current != FloatBlockEnd; ++Current)`

> using `++` operators on pointers results in the pointer jumping forward 1 `sizeof(type)` ahead in memory. Resulting in the pointer now pointing to the element that comes next in the buffer / block / array.
{: .prompt-info }

You can also omit parts a `for` loop, if you want. Say you don't want the condition part, you can skip it like this:

`for (float* Current = FloatBlockStart;;++Current)`

The only requirement is that there at least two semi-colons `;` in there.


Meaning if you want an infinite `for` loop, you can do this:

`for (;;)`


But won't an infinite loop cause the program to get stuck?

This brings me nicely to the next topic: Flow control.

#### for loop flow control: break and continue
Sometimes you want to dynamically alter the flow of your for loop, e.g. if an element is invalid, skip over it, or stop looping inside the loop itself. To do this we can use the `break` keyword, mentioned in the section about `switch` statements, and the `continue` keyword.

Break here works pretty similarly to in a `switch` statement. It tells the computer to jump out of this conditional. This can be used if we want to end the loop early.

Going with the example of an infinite `for` loop I mentioned. This can be used to `break` out of it if a condition is met. Like this:

```
// Takes an array of pointers as an argument
// no length provided, program is structured in a way that there is
// 1 nullptr at the end of the array
void do_something_to_array(float** Array)
{
    float** CurrentFloatPtr = Array;
    for (;;)
    {
        if (!*CurrentFloat)
        {
            break;
        }

        ++CurrentFloat;
    }
}
```
This is an example of what I think of as an indirect array. The array doesn't contain any *real* data, it just contains pointers to where that data is. This can be useful in a number of situations, but that's a post for another time. It might be a bit tricky to follow at first, since it's an array of pointers, making the pointer to the first element a pointer to a pointer, but hopefully it makes sense in time.


Here we iterate over the pointers in the array, if we hit a null pointer (`0`), then we break the loop, meaning the program does not get stuck in here unless there is a bug somewhere else in the code.


While it's usually best to use a condition you know you'll meet, defining it inside the `for` statement itself, there are some situations where this becomes tricky or it's just easier to not. It's really down to a case by case basis about what makes your life easier as a programmer, and what makes life easier for the CPU.

`continue` on the other hand, does not `break` out of the loop entirely, but instead just skips over the code below it within the loop. For example if you have an array of pointers that are stored in a way where there are gaps with null pointers between valid data. You could do the following:

```
{
    for (float** CurrentFloat = FloatBlockStart; 
        CurrentFloat != FloatBlockEnd; ++CurrentFloat)
    {
        if (!(*CurrentFloat))
        {
            continue;
        }

        // do stuff to the float
    }
}
```

Here we iterate over every possible value in the pointer array, but if we come across one that isn't set, we skip it.


We could also use this kind of behaviour if the data is invalid in some other way. Maybe there's a mathematical shortcut that allows us to determine that this value cannot possibly be valid, even though it's a valid pointer.


The point I'm trying to get at here rather poorly and ambiguously is how you use `for` loops depends heavily on the situation. So consider using them in ways that deviate from the standard `for (int; int < max; ++int)` if it fits the situation. Of course there are going to be a lot of cases where the common path is the best one, but just something to keep in mind.

### while

## Casting
### What is Casting?
Casting is the act of taking one type of variable and translating it into another type. For example, if you have a variable that's a `float` and you want to store it in a `long` integer. We have to do something to the data so it results in the expected outcome. Floating point types and integer types are stored in very different ways, we can't just copy the exact bytes of an integer into a float. To do this in C we can use `(type)` before the variable we want to cast.


So in the case where we want to store the value of a `float` inside a `long` we would do the following:

`long IntegerValue = (long)FloatValue;`

If `FloatValue` is `54.0f` then `IntegerValue` will be set to `54`.


But what happens in cases where we have numbers after the decimal point?


In these cases, the resulting integer value is the value of the float, minus the decimal part. 


If `FloatValue` is `54.6f` then `IntegerValue` will still equal `54`.


Both `float` and `long` are 32-bit values. What happens when we're casting to types with a higher or lower number of bits?


In cases where we cast from a lower number of bits to a higher one. E.g. a 32-bit integer to a 64-bit integer. We can safely store all the data from the 32-bit integer inside the 64-bit one. So the numbers will always match.


However, when we cast from a 64-bit integer to a 32-bit one, we may experience "truncation". This is where we have to chop off the higher byte values of the 64-bit integer so it will fit inside a 32-bit value. If there was any data above the 32-bit mark, the new value will be the maximum value of a 32-bit integer.


In the case of casting from `double` down to `float` we end up losing precision, a `double` can represent twice the number of decimal places a `float` can. Which means if we cast down, we cannot store all these decimal places inside the `float`. Leading to some rounding. While this isn't usually an issue, there are some mathematical equations where incredibly small numbers make a big difference.


> Your compiler may or may not allow you to actually cast values to a value of a smaller number of bits. It will usually output a warning, but sometimes warnings may be treated as errors depending on your build setup.
{: .prompt-info }
### Interpret Casting
Say we *did* actually want to just take the bytes of one type and treat them as another type. We can do this by using an "Interpret Cast", meaning, interpreting the bytes of this type as another.


We can do this by using pointers.


Taking the bytes of a `float` and treating them as a long. We can create a `long` pointer, get the memory address of the `float` using the `&` operator. Then cast that memory address to a `long` pointer. Now you can change the value of the `float` as if it were a `long`.


For example:
```
{
    float FloatValue = 10.42f;
    long* LongValuePtr = (long*)&FloatValue;

    // do some stuff to the long ...
}
```
This basically skips the cast step that just using `(long)FloatValue` would do.


If you wanted to modify this `long` value without modifying the initial `float` value, you would need to copy the bytes of the `long` into a new long. To copy by byte, the easiest way is usually to use interpret casting again.

```
// assumes both Destination and Source are a size in bytes
// bigger than Count.
void copy_bytes(char* Destination, char* Source, long Count)
{
    for (long Byte = 0; Byte < Count; ++Byte)
    {
        Destination[Byte] = Source[Byte];
    }
}

{
    float FloatValue = 10.42f;
    long LongValue = 0;

    copy_bytes((char*)&LongValue, (char*)&FloatValue, 8);
}
```

By casting the address of both variables to a `char*` we can read them one byte at a time, since a `char` is a 1 byte type, therefore the stride of `char` when using the  `[]` operator is 1 byte.

## Macros and Preprocessor tags


## Includes


## Entry Point



## Strings
Strings are a what we call text, a series, or *string* of individual characters that make up words. Though they need not be actual words, it's really just a broad category for data that contains multiple characters.

These are somewhat a special case in C, as they aren't really a traditional type. They are more of a buffer, meaning they are a series of bytes which, most of the time, have an arbitrary length. They are often non deterministic, meaning we don't know exactly how long it's going to be until it exists.