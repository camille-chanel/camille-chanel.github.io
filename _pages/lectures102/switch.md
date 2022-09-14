---
permalink: /lectures102/switch/
title: "Switch Statements in C++"
author_profile: false
classes: wide
---

## The Case for Switch Statements

When we think of conditional statements as a programming tool, the first thought that comes to mind is often if/else statements. However, there is simpler way of programming if **all** the conditional statements are checking for **equality** of one particular variable. For example, a variable "weather" can be set to 'r' for raining, 's' for snowing, 'o' for overcast, and 'c' for clear, 'i' for ice. Instead of writing:

```c++
// given weather is declared as a char
if (weather == 'r') {
    // execute rain code
}
else if (weather == 's') {
    // execute snow code
}
else if (weather == 'o') {
    // execute overcast code
}
else if (weather == 'c') {
    // execute clear code
}
else if (weather == 'i') {
    // execute ice code
}
else {
    // executes code for any invalid value of weather
}

```

This could be written as a switch statement.

## Syntax

Switch statements have 3 main parts:
1. the variable we switch on
2. the cases
3. the break statements

Continuing with the example above, we would switch on the "weather" variable. 

Each value of the variable that we care about will have a "case". Any invalid cases or values we don't care about can fall in a default, catch-all case. (The default case is optional syntatically). 

We place "break" statements after the code we execute for that particular case to exit the switch statement. Sometimes you might have multiple cases execute the same code, so we don't always have the same number of break statements as we do cases. 

The weather example as a switch statement would look as follows:

```c++
switch (weather) {
    case 'r':
        // execute rain code
        break;
    case 's':
        // execute snow code
        break;
    case 'o':
        // execute overcast code
        break;
    case 'c':
        // execute clear code
        break;
    case 'i':
        // execute ice code
        break;
    default:
        // execute code for any invalid value of weather
        break; 
}
```

C++ will run this code by reading the switch variable ( weather) and scanning through each case until it find its current value. It will then execute code until it sees a break statement. 
If weather = 'i', it will find the 'i' case, execute the ice code, then break out of the switch statement. **HOWEVER**, if you forget to write "break;" after the ice code, it will also read the default case code and continue executing until it sees "break;".

## Combining Cases

Let's say that rain and snow will execute the same code; we'll call this "precipitation code". To write this with if/else statements, we would write

```c++
if (weather == 'r' || weather == 's') {
    // execute precipitation code
}
else if (weather == 'o') {
    // execute overcast code
}
else if (weather == 'c') {
    // execute clear code
}
else if (weather == 'i') {
    // execute ice code
}
else {
    // executes code for any invalid value of weather
}
```

This could get rather lengthly, if sleet ('l') and hail ('h') also were valid and needed to execute precipitation code.

```c++
if (weather == 'r' || weather == 's' || weather == 'l' || weather == 'h') {
    // execute precipitation code
}
else if (weather == 'o') {
    // execute overcast code
}
else if (weather == 'c') {
    // execute clear code
}
else if (weather == 'i') {
    // execute ice code
}
else {
    // executes code for any invalid value of weather
}
```

In writing a switch statement for this logic, we use the break statements to our advantage.

```c++
switch (weather) {
    case 'r':
    case 's':
    case 'l':
    case 'h':
        // execute precipitation code
        break;
    case 'o':
        // execute overcast code
        break;
    case 'c':
        // execute clear code
        break;
    case 'i':
        // execute ice code
        break;
    default:
        // execute code for any invalid value of weather
        break; 
}
```

The values 'r', 's', 'l', 'h' will all execute the same code. If weather = 'r', then C++ will find the 'r' case, and continue to execute code until it sees the break statement.

## Limitation of Switch Statements
We can only switch on certain types of variables, namely integers and chars for this class (in totality: shorts, bytes, ints and chars).

Here is an example of using ints intead of chars. Note that we do not put single quotes around the case numbers. Char literals are encased by single quotes, string literals are encased by double quotes, but doubles/ints/other numerical values are not encased in quotes.

```c++
// given number is an int
switch (number) {
    case 1:
        // execute code
        break;
    case 2:
        // execute code
        break;
    case 3:
        // execute code
        break;
}
```

Oftentimes, we want to give our cases more meaning ('l' wasn't a very good representation for "sleeting" above, but 's' was taken by "snow".) We can use another tool, **enums** to help give our cases more meaning.

## Enums
Enum is short for enumeration. We can use enumerations in C++ to essentially make aliases for whole numbers, and those aliases can be complete words (not strings! think more like keywords instead). 

For example, let's say we want to have a switch statement to execute code for a puzzle based on 3 levels of difficulty: easy, medium, or hard. I'll call the switch variable `puzzleDifficulty`. I could make this a char, set to 'e', 'm', or 'h'. However, if we add an extra hard level, neither 'e' nor 'h' can be used, so we end up having to pick a random char to represent that (which is kind of cringey, like having 'l' stand for sleet).

Another option would be to have `puzzleDifficulty` be an integer, then have 1 stand for easy, 2 - medium, 3 - hard, and 4 - extra hard. This isn't as cringey, but it does make for very hard to read code later (especially if your values don't have a natural progression, like rain - 1, snow - 2, clear - 3 would make for hard to decipher cases). 

```c++
// given number is an int
switch (number) {
    case 1:
        // rain code
        break;
    case 2:
        // snow code
        break;
    case 3:
        // clear code
        break;
}
```
Your "snow" case may not be instantly recognizable as code about snow once the comment is removed! Then, before you know it, it takes 20 whole minutes to understand code you wrote 5 days ago.

This is where **enums** become a handy solution to our readability issue and desire to have cases beyond single letters or numbers.

If we create an enum for levels of difficulty, we will assign each word a number. Let's call it `Levels`.

```c++
enum Levels { easy, medium , hard };
```
By default, easy = 0, medium = 1, and hard = 2.

We could also assign the numbers ourselves:
```c++
enum Level { 
    easy = 1,
    medium = 5,
    hard = 10
};
```

If the first enumerator does not have an initializer, the associated value is 0. For any other enumerator that does not have an initializer, the associated value is the previous enumerator plus one.

```c++
enum Example { a, b, c = 10, d, e = 1, f, g = f + c };
//a = 0, b = 1, c = 10, d = 11, e = 1, f = 2, g = 12
```


Now, we can declare `puzzleDifficulty`
as a Levels type (where its underlying type is an integer).

`Level puzzleDifficulty;`

 We can both declare and initialize puzzleDifficulty to easy by

 `Level puzzleDifficulty = easy;`
 
 Whenever the compiler reads `easy` in our code (note: it's not in quotes! not a string!), it will automatically replace it with the assigned number. Now we can make a switch statement have pretty cases, but still be using ints.

 ```c++
// given puzzleDifficulty is a Level
switch (puzzleDifficulty) {
    case easy:
        // easy code goes here
        break;
    case medium:
        // medium code goes here
        break;
    case hard:
        // hard code goes here
        break;
}
```

#### Aside: Naming Enums
You'll note that I named the enum "Level" and not "Levels". This is by convention. You'll want your enum name to be singular, because puzzleDifficulty cannot hold more than one value at a time. Just like declaring an integer is `int data`, not `ints data` (even though the integer type has a range of possible values).