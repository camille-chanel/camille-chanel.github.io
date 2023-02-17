---
permalink: /lectures102/ioindepth/
title: "I/O In-Depth"
author_profile: false
classes: wide
---

While cin and cout make printing and reading in data very easy in C++, there are all sorts of topics on input/output to cover beyond the basics with C++. This lecture will cover error checking with cin, reading chars, cin.ignore(), and printf.

# Input

## Error Checking with Cin
We have practiced using cin repeatedly, but never with unexpected or bad input. What happens if you use cin to read in an integer, but the user types a double? It's now time to consider these situations.

Consider the following code. What happens if we give Fred and George as input? What about Fred and 44?
```c++
int i, j; 
cin >> i; 
cout << "I is " << i << endl; 
cin >> j; 
cout << "J is " << j << endl;
```
With the first set of input ("Fred", "George"), it's pretty clear why both cin calls failed -- there was no good input. "Fred" is not an integer; "George" is not an integer. However, with the second set of input ("Fred", 44), you might think that since the second value is a whole number, the second cin call should succeed and j should become 44. 

*Well, cin is tricky and this doesn't happen!* Both cin calls still fail. This is because once cin fails the very first time, it goes into a **fail state** and stays there until you tell it otherwise. Think of it as panicking and freezing. Once it fails, it stays that way!

So how do we revive cin and get it out of that state? We call **cin.clear()**.
So is this snippet of code any better?
```c++
int i, j; 
cin >> i; 
cin.clear();
cout << "I is " << i << endl; 
cin >> j; 
cin.clear();
cout << "J is " << j << endl;
```
Well, not really. If 20 and 40 are the input, then using cin.clear() is pointless. If "Fred" and "George" are the input, it is correct to clear cin because we have bad input, *but* we still should only print i and j **if** cin actually read integers (i.e. **if** cin did not fail).

So, we need an if statement to check if we read the correct data type! We used the similar logic in Java - remember myScanner.hasNextInt()? 

With C++, we can accomplish this in two ways. The first way is wrapping ```cin >> i``` inside an if statement. Each cin statement is actually a Boolean value that returns true/false if the cin statement succeeded/failed. Wrapping it in an if statement checks if the cin succeeds. See this example:

```c++
int i, j; 
if (cin >> i)
    cout << "I is " << i << endl;
else
    cin.clear(); 
```
(Note - I can get away with skipping the {} only because I have a single line of code for each condition.)

The second way of checking to see if cin failed is using **cin.fail()**. This is another method we can use on cin. As expected, it returns true if cin is in a failure state. See this code snippet for an example:
```c++
int i; 
cin >> i;
if (!cin.fail())
    cout << "I is " << i << endl;
else
    cin.clear(); 
```

So, using cin.clear() and cin.fail() should make our original example work, right?
```c++
int i, j; 
cin >> i;
if (!cin.fail())
    cout << "I is " << i << endl;
else
    cin.clear(); 

cin >> j;
if (!cin.fail())
    cout << "J is " << j << endl;
else
    cin.clear();
```
Still no! Try with Fred and 44.

The second cin call failed just like the first one does, even though 44 is an integer!! What?! This is because when you call cin.clear() it resets cin out of its panicked state, but it does **not** move cin past the bad input. It is still trying to read "Fred". To fix this, you have to go ahead and read the erroneous integer as a garbage string before moving on OR use **cin.ignore()**. I recommend using cin.ignore() - who knows how large that garbage data could be. Also, it's better for code readability.

```cin.ignore()``` takes two arguments, a number and a character. It will ignore until it reads the specified character OR until it's already ignored X number of characters, whichever comes first. For example, ```cin.ignore(5, 'r')``` will ignore 5 characters or will ignore until it reads an 'r', whichever comes first.

For example, consider this program.
```c++
string codeName;
cout << "Type your last name: ";
cin.ignore(5, 'r');
cin >> codeName;
cout << "Your codename: " << codeName << endl;
```

The console would look like this:
```console
Type your last name: Crumpton
Your codename: umpton
```

We ignore until we 'r', then it saves the rest of the input. Let's change the character to ignore to 'q'.
```c++
string codeName;
cout << "Type your last name: ";
cin.ignore(5, 'q');
cin >> codeName;
cout << "Your codename: " << codeName << endl;
```

The console would look like this:
```console
Type your last name: Crumpton
Your codename: ton
```
Since we read 5 characters and there is no 'q' so far, we stop ignoring at 5 characters then save the rest of the string.

Most times when you use ```cin.ignore()```, it's to ignore bad input. That input could be incredibly long, and at times, impossible to know how long. To ignore the maximum amount of input possible until '\n' (the user pressing enter), we need to ignore the maximum of the streamsize until we reach '\n'. You will write
```c++
cin.ignore(numeric_limits<streamsize>::max(), '\n');
```
You **must** remember to ```#include <limits>``` in order to use numeric limits!! VS Code might do this automatically for you, but your code will **NOT COMPILE** on the Hydra/Tesla machines without it! This can result in a 0.

The final, working snippet:
```c++
int i, j; 
cin >> i;
if (!cin.fail())
    cout << "I is " << i << endl;
else {
    cin.clear();
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
}

cin >> j;
if (!cin.fail())
    cout << "J is " << j << endl;
else {
    cin.clear();
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
}
```

### Summary
To check for bad input, you must check if cin failed. Either wrap the cin call inside an if statement, or check with **cin.fail()**. If cin *did* fail, make sure to clear the error state before reading in any other input by using **cin.clear()**. Then move past the bad input by using **cin.ignore(numeric_limits<streamsize>::max(), '\n') with #include \<limits\>**. 

## Using cin.get()
So far, we have just been using cin with the extraction operator, >>, which skips over whitespace. What if we want to read and save whitespace with cin, or just read a single character from a string from console input? One way to do this is by using **cin.get()**.
There are a few ways of using this function, but one way is by reading input char by char.
If I type "AB" as my initials, then 'A' will be saved in c1, and 'B' will be saved in c2.
```c++
char c1, c2;
cout << "What are your initials? ";
cin.get(c1);
cin.get(c2);
```
Compare this to using the extraction operator.
```c++
char c1, c2;
cout << "What are you initials? ";
cin >> c1 >> c2;
```
These snippets will behave differently depending on if you put a space between your initials. If I type "AB" (no space), then ```cin.get()``` will save c1 as 'A' and c2 as 'B'. If I type "A B", then ```cin.get()``` will save c1 as 'A' and c2 as ' '. ```cin.get()``` reads whitespace, whereas the extraction operator skips over it!

Depending on how you need to read data, ```cin.get()``` might be useful if you need the whitespace.

# Output

## Printf & Formatting
```printf()``` is a C-language function that we can use in C++ for formatting. While the shortcut can look unreadable, many students prefer it because it's the same shorthand they learned for ```System.out.format()``` in Java. With ```printf()```, we can round numbers, give a specified width to print in, pad with leading zeros, and left/right justify.
If you need a review of the format specifiers for ```System.out.format()```, ask a TA or instructor. Below is a reference table of the specifiers for review.

| Format Specifier | Data Type                  |
|------------------|----------------------------|
| %d               | integer                    |
| %c               | character                  |
| %f               | float (or "small" doubles) |
| %lf              | double                     |
| %e               | scientific notation        |
| %s               | string                     |
| %%               | prints '%' character       |

The only major difference you'll encounter regularly with ```printf()``` versus what you learned in Java is the treatment of strings. ```printf()``` is a c-style function, and the C language doesn't have strings. All strings are just arrays of chars (I'll call an array of chars "C-style strings"). So when we use a C++ string in printf, we need to convert it to a C-style string. We use ```.c_str()``` to do this conversion.

```c++
string text = "Go VOLS";
    printf("%s\n", text.c_str());
```
If you don't use the .c_str() method, you will get a compile error.

## Formatting with Cout & \<iomanip>
Manipulators are tools that allow us to edit streams (i.e. output streams, file streams, etc) to polish our formatting. Some examples of I/O manipulation would be left-justifying text, or rounding data to a particular decimal place. Manipulators are found in \<iostream> and \<iomanip>.

#### \<iostream> Manipulators


