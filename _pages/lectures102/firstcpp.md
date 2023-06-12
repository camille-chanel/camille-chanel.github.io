---
permalink: /lectures102/ioindepth/
title: "Basic C++"
author_profile: false
classes: wide
---

While everyone that starts CS102 has some sort of programming experience, mostly Java, we will be begin with some very basic C++ programs before we ramp up quickly. This reading will cover the basic skeleton of a C++ program, basic input and output, data types, and control flow (loops and conditional statements) compared to Java.

# The Basic C++ Skeleton
While Java is a pretty verbose, even to write a simple program with a single output line, C++ slims things down. Below I've written an example Hello World program, and you can see that none of the `public static void main` mess is there, nor do you have to force `main` inside a class.

```c++
#include <iostream>
using namespace std;

int main() {
    cout << "Hello World" << endl;
}
```

To break this down...
## Include Statements
 `#include <iostream>` is similar to an import statement. Just like we have to import the Scanner for reading input in Java, we need to `#include` the library to use `cin` (reading input) and `cout` (printing output). You'll hear `#include` out loud as "pound include" (not hashtag include). In Java, you might hear a TA or me say "don't forget your import statements" - the equivalent informal C++ saying is "don't forget your pound includes".

## Namespaces
`using namespace std;` means we are using the namespace called std, which is an abbreviation of "standard". You might hear someone refer to this as "using the standard namespace". I assume you might be asking, "What the heck is a namespace?" 

You don't really need to know much about this, but essentially it's a way of organizing variable/function/class names. `cin` and `cout` are a part of the standard namespace, and if you don't have this line in your program, you would need to clarify them in the code by writing `std::cout` or `std::cin` everywhere you would normally type `cout` or `cin`. 

We won't ever make our own namespaces and won't use anything besides the standard namespace, but it is possible to make your own. Let's say you make two different namespaces, `n1` and `n2`. Within each namespace, all the variables must have unique names (like we are used to - no naming conflicts!). However, a second namespace can use those same names all over again. Let's say both namespaces have an `x` variable. The `x` in namespace 1 would be written as `n1::x` and the x in namespace 2 would be written as `n2::x`. 

Kind of silly, but there's a lot of disagreement over whether using the standard namespace is "good practice". You'll find out pretty quickly in this field that people can be very opinionated on what might seem to be trivial topics (like using tab versus spaces). In this class, we will use it. The more we can reduce the visual clutter as you learn, the better.

## The Main Function
This is the last part of the skeleton!
`int main() {}` is the main function, the "starting point" where your code will begin executing, just like the main function in Java. You will put your code inside the curly braces. 

**One small difference between Java and C++ here**: if you remember, the main function in Java was `void`; it didn't have a return value. Here, `main` actually returns an integer, but it's kind of special and not enforced. As you can see in the "Hello World" program, I didn't return an integer and your computer would have no complaints. As we get further into the course though, we will start returning either a `0` or `1` from `main`. Traditionally, `main` returns `0` if it completes with no issues, and it returns a `1` if it's terminated because of an error.

# Input and Output
As shown above in the example, you will use `cout` to print output to the screen. `cin` is used to receive input. Read the C++ reference guide for input/output (stop after "Cin and Strings"). 
https://cplusplus.com/doc/tutorial/basic_io/

## Example Programs
```c++

```

# Minor (but Common) Differences: Java vs C++
Besides the "skeletal" differences above with `main`, there are a few other common hiccups between Java and C++ for beginning programmers.

The first one is that the data type `string` is not capitalized in C++ (it is in Java). Here's an example of declaring some strings in C++:

```c++
string firstName, lastName;
string streetName, streetType;
```

The second is that you use cin/cout for input and output.