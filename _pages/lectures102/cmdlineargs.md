---
permalink: /lectures102/cmdlineargs/
title: "Command Line Arguments"
author_profile: false
classes: wide
---

An incredibly useful tool in C++ (and other languages) is the ability to pass information to the program via the command line. In C++, we do this using ```argc``` and ```argv```.

```argc``` and ```argv``` are two optional parameters for ```main()``` written: 

```c++
int main(int argc, char **argv)
```

You may also see `char *argv[]` instead of `char **argv`, i.e.

```c++
int main(int argv, char *argv[])
```

# argc
```argc``` is an integer that stands for **argument count**. It stores the _number_ of arguments on the command line, including the program executable itself.

The following graphic is an example of running a program with some command line arguments. Here, ```argc``` will equal 5 when I run the following on the command line:

```console
./myprogram 10 20.22 30 file.txt
```

# argv
```argv``` is an array of c-style strings that stores the **argument values**. Each index into this array holds an argument from the command line, including the program executable itself. Using the example above, the following value will be held in ```argv```:

| Index     | Value       |
|-----------|-------------|
| `argv[0]` | ./myprogram |
| `argv[1]` | 10          |
| `argv[2]` | 20.22       |
| `argv[3]` | 30          |
| `argv[4]` | file.txt    |

### A Note on C-Style Strings
A c-style string is an array of chars. The C programming language doesn't have the string class like we do in C++. (Why? Because C doesn't have classes at all.)

While this may seem uneventful (a string really is just some characters in a row, right?), the strings give use a lot of functionality we take for granted. It's easy to see how long the string is by using the .size() method. Primitive data types, like ints and arrays, don't have methods.

When we use C language functions and tools in C++, like printf or argv/argc, we need to consider if and when we need to convert C++ strings to C-style strings and vice versa.

Lucklily, storing a c-style string (the array of chars) as a C++ is pretty easy. Just set the array equal to a freshly declared string.

```c++
// argument1 is a C++ string, argv[1] is a c-style string
string argument1 = argv[1];
```

If you ever need to change a C++ string to a c-style string, you can use the `.c_str()` method. You will mostly encounter this when using `printf`, since you can only print c-style strings using this function. Read [I/O In-Depth](/lectures102/ioindepth/) for more information.
```c++
string name = "Happy";
printf("My name is %s\n", name.c_str());
```

# Using argc/argv in Programs

When you use arguments on the command line, there are certain steps you usually will take with the arguments before using them in your program. Generally, these steps are

1. Error Checking the Argument Count
2. Changing the Arguments' Data Types (istringstreams)

## Step 1: Error Checking Argc

While it's not necessary to error check argc to make your program work, it's incredibly helpful to do immediately as a reminder to put your arguments on the command line. At this point, you might be so used to writing `./programname` that it's very easy to forgot that a certain program needs arguments. Without an error message, you may think the program has a bug if nothing happens, when it's really operator (user) error.

A simple if statement does the trick. Let's say I'm writing a program that takes 2 files as the command line arguments, an input and output file. I expect argc to be 3 then (remember the ./executable counts).
```c++
if (argc != 3) {
    cerr << "usage: ./executable inputfile outputfile" << endl;
    return 1;
}
```

## Step 2: Getting the Right Data Types
Once you've checked argc, you'll begin looking at what you received as data with argv. Depending on your program, you might want to keep the data as strings. You're in luck that this is simple - it's covered above in "A Note on C-Style Strings". You just set that particular argv index equal to a C++ string.

What gets tricker is when the data is numerical, and you need it stored in a numerical data type. It's been given to you as a c-style string - not ideal for calculating an average or summing up values. Therefore, you'll want to change the data storage from a c-style string to an int or a double.

How do you do this? **Well, you have options.**

### atof(), stod(), stoi()
These are functions that allow you to change strings to numerical values. `atof()` = array (c-style string) to float, `stod()` = string to double, `stoi` = string to int.

```c++
string str1 = "12.34";
char[] str2 = "56.78";
string str3 = "90";

double num1 = stod(str1);
float num2 = atof(str2;)
int num3 = stoi(str3);
```

**PITFALLS**: 

-C++98. Unfortunately, with this version of C++ (1998) you won't have stoi or stod.

-Errors. There are some strings that will break these functions. For example, a string like "123abc" **should fail** with stoi(), but it doesn't. It saves 123 as the int. "abc123" does fail - the program will output a runtime error. **Because strings like "123abc" succeed with stoi(), many people consider this function to have unsatisfactory error checking.**

### String Streams
 As you can see, sometimes you won't be able to use those nice functions. Instad, you'll need use string streams, specifically istringstreams. You'll use the argument values as the string input.
 
 Would you rather use `stoi()` and stringstreams make you nervous? (There's no need to be, but some people find them intimidating). Write your own version of `stoi()` to just use stringstreams once.

```c++
static int stoi(string & s) {
    int i;
    istringstream sin(s);
    if (!(sin >> i)
        cerr << "Failed to convert string to int." << endl;
    return i;
}
```

## Examples

This program takes 2 integers on the command line and sums them together. It uses a string stream to convert the arguments from strings to ints.

```c++
/* This program takes two integers on the command line,
   adds them together, and prints the sum. */
#include <iostream>
#include <sstream>
using namespace std;

int main(int argc, char **argv) {
    if (argc != 3) {
        cerr << "./sum num1 num2" << endl;
        return 1;
    }

    istringstream sin(argv[1]);
    int i, j;
    if (!(sin >> i)) {
        cerr << "First argument is not an integer." << endl;
        return 1;
    }
    sin.clear();
    sin.str(argv[2]);
    if (!(sin >> j)) {
        cerr << "Second argument is not an integer." << endl;
        return 1;
    }

    cout << i + j << endl;

}
```

To improve this program, you can create a function that checks the validity of the arguments to assure they are integers. In this version, we create our own `stoi`function. You could also use the built-in C++ `stoi` function, but remember the pitfalls mentioned above if you go that route.

```c++
/* This program takes two integers on the command line,
   adds them together, and prints the sum. */
#include <iostream>
#include <sstream>
using namespace std;

static int stoi(string &s) {
    int n;
    istringstream sin(s);
    if (!(sin >> n)) {
        cerr << "Conversion from string to int failed." << endl;
        return 1;
    }
    return n;
}

int main(int argc, char **argv) {
    if (argc != 3) {
        cerr << "./sum num1 num2" << endl;
        return 1;
    }

    int i = stoi(argv[1]);
    int j = stoi(argv[2]);
    cout << i + j << endl;
}
```

**Notes on this improvement**: You'll notice we no longer have to use `sin.clear()`. This is because `sin` is created and destroyed each time the function is called. It technically never gets "reused". Additionally, you'll notice that an ampersand is in front of the string parameter `s`. This is to pass the argument by reference, which we will discuss in more detail later.