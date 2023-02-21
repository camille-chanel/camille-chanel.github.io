---
permalink: /lectures102/stringstreams/
title: "String Streams"
author_profile: false
classes: wide
---
So far, we've covered two types of streams - console streams and file streams. This is out last section of stream coverage - string streams. We use string streams to extract data from strings and to use data to create strings. 

| Stream Type     | Object   | Library     |
|-----------------|----------|-------------|
| Console streams | cin/cout | \<iostream> |
| File streams    | fin/fout | \<fstream>  |
| String streams  | sin/sout | \<sstream>  |

As you can see from the chart above, for string stream usage, you'll need to ```#include <sstream>```. You'll also want to ```#include <string>```. You can create strings without this second library, but you'll need it for methods we can use on strings.

Before we jump into string streams, let's talk about operator overloading with strings. You may use this in conjunction with or in replace of string streams.

# Strings in C++: Operator Overloading
Strings in C++ can be used with some arithmetic operators like +, <=, etc. This is called **operator overloading**, which allows you to redefine basic operators to use on objects.

==, != <=, >=, <, >, and + have all been defined for use with strings.

(Note - Operator overloading is rarely intuitive. Few objects have operations which exactly match on to the classic arithmetic operations. Another user reading your code may make incorrect assumptions because of this, and it leads to bugs.)

## Equality with Strings
Two strings are considered equal if
  1. Their sizes are the same.
  2. Each character in location i in the first string is equal to the character in location i in the second string.

The == operator tests for equality, and != for inequality.

## Greater/Less than with Strings
Comparisons with strings are done lexiographically, exactly how your computer would sort filenames by name. 'A' < 'B', etc. Each character actually has a numeric value, called its ASCII value, which we will talk more about later in the semseter.

## Concatentation with Strings
Similarly to Java, you can use the + sign to concatenate strings.
```c++
string s1, s2, s3;
s1 = "Rocky Top, You'll Always Be ";
s2 = "Home Sweet Home to Me";
s3 = s1 + s2;
cout << s3 << endl;
```

# Input String Streams: Extracting from a String
Input string streams are used to take a string and extract data from it. **A string is the data source instead of the console or a file.** For example, if you have ```string text = "123 abc"``` and later you may want to separate this string into ```123``` as an int and ```"abc"``` as a string. You can do this with an input string stream.

The steps for using string streams are simpler than file streams, as we don't need to check for errors when opening the stream.

The steps to extract from a string are
1. Construct an istringstream (sin) object
2. Use extraction operators
3. (Optional) Reuse sin with a new string

## Step 1: Declare an istringstream
When you declare an istringstream object, you can use a constuctor with the input data string as the argument.

```c++
string data = "123 Main St";
istringstream sin(data);
```

With file streams, we had the option of declaring and using the ```.open()``` method separately, but string streams do not have an ```.open()``` method. Instead, if you want to declare an istringstream and set the string data source on different lines, you can use the ```.str()``` method. ```.str()``` will provide the string to ```sin```.
```c++
string data = "123 Main St";
istringstream sin;
sin.str(data);
```

## Step 2: Extract Data
Like cin, use the extraction operators to extract data separated by whitespace.

```c++
int roadNum;
string roadName, roadType;
sin >> roadNum >> roadName >> road Type;
```

## Step 3: (Optional) Reuse istringstream
If you would like to reuse the istringstream with a new string, you must use ```.clear()``` before you give ```sin``` a new string with ```.str()```.

```c++
istringstream sin("Some Data");
sin >> some_string1 >> some_string2;
sin.clear();
sin.str("Some Other Data");
```
If you don't use ```.clear()```, the ```.str()``` call will be ignored!

# Output String Streams: Creating a String
1. Construct an ostringstream (sout) object
2. Write using sout
3. Return the string
(4. reuse)

## Step 1: Declare an ostringstream
When you construct an ostringstream object, you don't connect it to the output string yet. This makes the declaration very simple:

```c++
ostringstream sout;
```

## Step 2: Write using sout
Use the insertion operator to write. Your creation will be temporarily stored in the output stream string.

```c++
int num = 10;
string name = "Broadway St."
sout << num << " " << name;
```

## Step 3: Return the string
When you are done writing, you need to return the string that was built. This uses the ```.str()```. **Yes, we use the .str() method in two different ways, depending on if it's an istringstream or an ostringstream.** When using this method for output, it does **not** take any arguments.

```c++
sout.str();
```

## All Steps - Output
```c++
ostringstream sout;

int num = 10;
string name = "Broadway St."
sout << num << " " << name;

sout.str();
```
