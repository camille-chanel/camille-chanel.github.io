---
permalink: /lectures/scanner/
title: "Reading User Input with Java"
author_profile: false
classes: wide
---

## The Need for Reading Input
Oftentimes, we want our program to be **interactive** with the user, and in order to do that, we want to receive input or data from them. One way to do this is to allow the user to type data into the terminal window. 
In this class, we will learn to receive data through a tool called the "Scanner". This will allow us to receive input and store that input into variables of various data types, including strings, chars, integers, and doubles.

## The Scanner

### Import Statements
The Scanner is tool (technically a **class** but we will learn more about that later), and this tool has many functionalities already coded for us. In order to use the Scanner, we need to **import** all of that code in our Java file. To do this, we will have this import statement at the top of our file: `import java.util.Scanner;`

"java", the first keyword in the import statement is lowercase! This is one very large java library. Within that library, we have "util" which stands for utilities. Be careful typing both of these - I see many errors where your file isn't running because "java" is capitalized or "util" is spelled incorrectly (until is a common one). Within utilities, we have the Scanner functionality. (And the S is capitalized here!) This statement will find the Scanner functionality within these libraries and import it only.

In general, we always place import statements before the main Java skeleton.

### Scanner in Use
Once we have the Scanner imported, we are able to use its functionality in our program.

We begin by creating a Scanner variable (more specifically we are "creating an instance of the Scanner class", but we will learn about instances and classes later). I'll call this Scanner `s`. 

We also need to tell Java where our input will be coming from (live typing from a terminal window or reading from a file are two possibilities). To read user input from the terminal (our only use in this class), we write `Scanner s = new Scanner(System.in);`. This both declares the Scanner and tells the Scanner to look at System input.

Now, we will use the Scanner to read. Often, print messages will be used before the capturing data with the Scanner, because we need to **prompt** the user for the data. We use a function on the Scanner variable we created, depending on the data type of the information being read.

The following code will read a GPA and store it as a double.

```java
import java.util.Scanner;
class Example1 {
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        double GPA;

        System.out.print("Enter a GPA: );
        GPA = s.nextDouble();
    }
}
```

If we are reading an int, we would use .nextInt() instead, and have GPA be declared as an int.

Here is a table of the functionality we will use for gathering input:

| Data Type Returned | Scanner Function |
|------------------|------------------|
| string           | .next()          |
| string           | .nextLine()      |
| int              | .nextInt()       |
| double           | .nextDouble()    |
| boolean          | .nextBoolean()   |

Other data types beyond this class are also supported (shorts, longs, bytes, floats, etc).

**.next() and .nextLine()** both return strings, but next() will capture until it reads *any* whitespace (space, tab, or return/enter/newline). This means, if you are prompting for a first name, and user's first name is Mary Ellen, then .next() will only capture "Mary". To capture a string **including spaces**, you must use .nextLine(). This ends the capture when it reads a return/enter/newline.

### Example
```java
import java.util.Scanner;
class Example2 {
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        string name;

        System.out.print("Enter a GPA: );
        GPA = s.nextDouble();
    }
}
```

### Using the Scanner with Conditional Statements