---
permalink: /lectures102/filestreams/
title: "File Streams"
author_profile: false
classes: wide
---

A powerful step forward in learning to program is expanding your input and output beyond the console. We will discuss how to read from and write to files in C++ using the library \<fstream>. You can think of a **stream** as a temporary place we keep data to read/write. For example, with reading a file, C++ will copy text from the file to the stream for us to decide how to extract (i.e. should we store this data as a string? 3 ints and a double? etc.). You might also hear this as a **buffer** colloquially. Technically, the difference between a stream and a buffer is that a buffer has a definite length, whereas a stream is indefinite.

# Reading from Files (File Input Streams)
Reading from files allows us to expand the amount of input data we receive; we are no longer constricted by how tired our fingers might get from typing everything ourselves in a console window.

Here are 5 general steps we will take when reading from files. I will go into each step in detail. (Remember, a pre-requiste for reading/writing is ```#include <fstream>```. I think of that as Step 0.)
1. **Create** an input file stream object
2. **Connect** the stream to the file itself using ```.open()```
3. **Error check** to make sure if it opened correctly
4. **Extract** from the file like cin
5. **Close** the input file stream 

## Step 1: Creating an Input File Stream Object
Declaring an input file stream object is the first step in our file reading process. I typically name the input file stream object ```fin``` to remind myself that I use the same operators as ```cin```. However, you can choose other names. This object is type ```ifstream``` - stands for "input file stream".

Example:
```c++
ifstream fin;
```
## Step 2: Connecting the Stream Using .open()
Next is connecting the input file stream to the file itself. We use the ```.open(string f)``` method on the input file stream object to do this. ```.open()``` takes a single argument, the filename. Remember that string **literals** are in double quotes - so you could either saved the filename to a string variable, or place the filename as the argument in double quotes.

Example, assuming an ```ifstream fin``` has been declared:
```c++
string filename = "myinput.txt";
fin.open(filename);
```

**Note**: You can combine Step 1 and Step 2 in a single line **if** you know the file name by the time you create the ifstream object. You do this by using the **ifstream constructor**, taking the filename as an argument. Using the constructor means **you don't use .open() at all**. I typically don't do this, because I like to declare all my variables/objects immediately at the top of the main function. 

Example:
```c++
ifstream fin("myinput.txt");
```

## Step 3: Error Checking Step Two
Next, you'll want to make sure that the file stream connected to the file. Some reasons why the ```.open()``` might **NOT** work is because you don't have the permissions to access the file, or the file doesn't exist in this directory or at all. You can check this in two different ways:

```.fail()``` returns true is the file failed to open.
```.is_open()``` returns true if the file successfully opened and has not been closed yet.

Example 1:
```c++
if (fin.fail()) {
    cerr << "File failed to open." << endl;
}
```
This example uses **cerr**. It's like **cout** and prints to the console, but is designated for error output. You can use cerr or cout for these error messages in your labs.

Example 2:
```c++
if (!fin.is_open()) {
    cerr << "File failed to open." << endl;
}
```

## Step 4: Extracting from the File
Finally, we get to the actual file reading! You'll use your file input object, in this example ```fin```, to extract data just like you would ```cin``` with the extraction operator.

If we know the file contains 2 integers then a string, we would write
```c++
int a, b;
string c;
fin >> a >> b >> c;
```
If you know the file contains many lines of 2 integers then a string, you can write this in a loop. Since a, b, c will be overwritten each time you read a new line, you'll probably need to save all the data into vectors for use later. 
```c++
int a, b;
string c;
vector <int> allAs, allBs;
vector <string> allCs;
while (fin >> a >> b >> c) {
    allAs.push_back(a);
    allBs.push_back(b);
    allCs.push_back(c);
}
```

Remember, you'll need to know how the file is organized in order to use the ```fin >> a >> b >> c``` snippet (i.e. you know it's organized as two integers then a string).

## Step 5: Closing the Input File Stream
Finally, you'll close the input file stream as soon as you are done reading from the file. This is done for safety. 
```c++
fin.close();
```

# Writing to Files (File Output Streams)
Just like reading from files, I've boiled writing to files into 5 basic steps. I will go into each step in detail. (Remember, a pre-requiste for reading/writing is ```#include <fstream>```. I think of that as Step 0.)

1. **Create** an output file stream object
2. **Connect** the stream to the file (either creates or replaces)
3. **Error check** to make sure it opened
4. **Write** to file like cout
5. **Close** the output file stream

## Step 1: Creating an Output File Stream Object
Declaring an outupt file stream object is the first step in writing to a file. I typically name the input file stream object ```fout``` to remind myself that I use the same operators as ```cout```. However, you can choose other names. This object is type ```ofstream``` - stands for "output file stream".

Example:
```c++
ofstream fout;
```

## Step 2: Connecting the Stream Using .open()
Similarly to input file reading, next is connecting the output file stream to the file itself. We use the ```.open(string f)``` method on the output file stream object to do this. ```.open()``` takes a single argument, the filename. 

Example, assuming an ```ofstream fout``` has been declared:
```c++
string filename = "myoutput.txt";
fout.open(filename);
```

**Note**: You can combine Step 1 and Step 2 in a single line. You do this by using the **ofstream constructor**, taking the filename as an argument. Using the constructor means **you don't use .open() at all**. I typically don't do this, because I like to declare all my variables/objects immediately at the top of the main function. 

Example:
```c++
ofstream fout("myoutput.txt");
```

**WARNING! This step either creates the file OR overwrites the file if it already exists! There is no undo, so be VERY CAREFUL not to overwrite important files!**

## Step 3: Error Checking Step Two
Next, you'll want to make sure that the file stream connected to the file. You'll use the same methods as described for input file reading, ```.fail()``` and ```is_open()```. Scroll back up for more information.

Example 1:
```c++
if (fout.fail()) {
    cerr << "File failed to open for writing." << endl;
}
```

## Step 4: Writing to the File
Finally, time to write to the file! Just like using ```cout```, you'll use the insertion operator, << (named for inserting data into the file stream). You can also use **formatting** when writing the file with ```#include <iomanip>```.

Example:
```c++
double pi = 3.1415;
fout << fixed << setprecision(2) << setw(10) << pi << endl;
```

## Step 5: Closing the Output File Stream
Finally, when you are done writing to the file, remember to close the output file stream. An output file stream stores data in RAM (Random Access Memory) first, because writing to the file is "slow" (relative to other operating system processes). This gives the OS (operating system) time to write to the file when ready.

If you don't close the output stream file, there is no guarantee that the data will be written to the file.

