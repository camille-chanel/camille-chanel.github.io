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

With C++, we can accomplish this two ways. The first way is wrapping cin >> i inside an if statement. Each cin statement is actually a Boolean value that returns true/false if the cin statement succeeded/failed. Wrapping it in an if statement checks if the cin succeeds. See this example:

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

The second cin call failed just like the first one does, even though 44 is an integer!! What?! This is because when you call cin.clear() it resets cin out of it's panicked state, but it does **not** move cin past the bad input. It is still trying to read "Fred". To fix this, you have to go ahead and read the erroneous integer as a garbage string before moving on OR use **cin.ignore()**. I recommend using cin.ignore() - who knows how large that data be that you will never use. Also, it's better for code readability.

To ignore all characters until '\n' (the user pressing enter), you will write
```c++
cin.ignore(numeric_limits<streamsize>::max(), '\n');
```
You **must** remember to #include <limits> in order to use numeric limits!! VS Code might do this automatically for you, but your code will **NOT COMPILE** on the Hydra/Tesla machines without it! This can result in a 0.

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

## Cin.get()

# Output

## Printf & Formatting

## Formatting with Cout & \<iomanip>
Manipulators are tools that allow us to edit streams (i.e. output streams, file streams, etc) to polish our formatting. Some examples of I/O manipulation would be left-justifying text, or rounding data to a particular decimal place. Manipulators are found in \<iostream> and \<iomanip>.

#### \<iostream> Manipulators


