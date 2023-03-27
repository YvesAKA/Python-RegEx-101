# Python regular expressions 101

## Purpose

Regular expressions are sequences of characters forming patterns defined according to a specific formalism.
Commonly known as RegEx in the jargon, they are generally used for:

+ Perform string checking
+ Implement checkpoints at the input level of a program
+ Format or modify strings according to specific patterns

In Python, RegEx is implemented by the `re` module.

## Basic usage

### Import the `re` module

Import the re module:

```python  
import re
````

### 2. Simple test

Search the string to see if it starts with '"The" and ends with "Spain":

```python
#Code
import re
txt = "The rain in Spain"
print(re.search("^The.*Spain$", txt))

#Output:
<re.Match object; span=(0, 17), match='The rain in Spain'>
```

The output tell us that the required pattern had been matched.

### inline vs compiling

The re module gives you the possibility to call your patterns directly when you evaluate your expressions or to store these patterns in objects that you can refer to at will. Below is an example.

Inline:
```python
#Code
import re
txt = "The rain in Spain"
print(re.search("^The.*Spain$", txt))

#Output:
<re.Match object; span=(0, 17), match='The rain in Spain'>
```

Compilation:

```python
#Code
import re
txt = "The rain in Spain"
p = re.compile ("^The.*Spain$")
print(p.search(txt))

#Output:
<re.Match object; span=(0, 17), match='The rain in Spain'>
```

We simply called the function `compile` from the `re` module to store our pattern on an object.

### Metacharacters

Metacharacters are characters with a special meaning:

| Caracter  | Description                    | Example|
|:--------- |:------------------------------ |:-------|
|[]         |A set of characters             |"[a-m]" |
|\          |Signals a special sequence (can also be used to escape special characters)|"\d" |
|.      |Any character (except newline character)|"he..o" |
|^     |Starts with                          |^hello"    |
|$      |Ends with                 |"planet$"|
|*       |Zero or more occurrences |"he.*o"|
|+       |One or more occurrences|"he.+o"|
|?      |Zero or one occurrences|"he.?o"|
|{}       |Exactly the specified number of occurrences|"he.{2}o"|
|\|      |Either or|"falls\|stays"|
|()       |Capture and group                                |        |

### Special Sequences

A special sequence is a \ followed by one of the characters in the list below, and has a special meaning:

| Caracter  | Description                    | Example|
|:--------- |:------------------------------ |:-------|
|\A         |Returns a match if the specified characters are at the beginning of the string             |"\AThe" |
|\b         |Returns a match where the specified characters are at the beginning or at the end of a word (the "r" in the beginning is making sure that the string is being treated as a "raw string")            |r"\bain" |
|\B        |Returns a match where the specified characters are present, but NOT at the beginning (or at the end) of a word (the "r" in the beginning is making sure that the string is being treated as a "raw string")            |r"\Bain" r"ain\B"  |
|\d         |Returns a match where the string contains digits (numbers from 0-9)             |"\d" |
|\D         |Returns a match where the string DOES NOT contain digits             |"\D" |
|\s         |Returns a match where the string contains a white space character             |"\s" |
|\S         |Returns a match where the string DOES NOT contain a white space character            |"\S" |
|\w        |Returns a match where the string contains any word characters (characters from a to Z, digits from 0-9, and the underscore _ character)           |"\w" |
|\W         |Returns a match where the string DOES NOT contain any word characters            |"\W" |
|\Z        |Returns a match if the specified characters are at the end of the string             |"Spain\Z" |

### Sets

A set is a set of characters inside a pair of square brackets [] with a special meaning:

| Set  | Description                    |
|:--------- |:------------------------------ |
|[arn]        |Returns a match where one of the specified characters (a, r, or n) is present             |
|[a-n]        |Returns a match for any lower case character, alphabetically between a and n             |
|[^arn]         |Returns a match for any character EXCEPT a, r, and n             |
|[0123]         |Returns a match where any of the specified digits (0, 1, 2, or 3) are present             |
|[0-9]         |Returns a match for any digit between 0 and 9|
|[0-5][0-9]         |Returns a match for any two-digit numbers from 00 and 59	|
|[a-zA-Z]         |Returns a match for any character alphabetically between a and z, lower case OR upper case             |
|[+]         |In sets, +, *, ., |, (), $,{} has no special meaning, so [+] means: return a match for any + character in the string|

## Functions

### The findall() Function

The findall() function returns a list containing all matches.

```python
#code: Print a list of all matches
import re
txt = "The rain in Spain"
x = re.findall("ai", txt)
print(x)

#output
['ai', 'ai']
```

The list contains the matches in the order they are found. If no matches are found, an empty list is returned.

### The search() Function

The search() function searches the string for a match, and returns a Match object if there is a match.
If there is more than one match, only the first occurrence of the match will be returned:

```python
#code: Search for the first white-space character in the string
import re

txt = "The rain in Spain"
x = re.search("\s", txt)

print("The first white-space character is located in position:", x.start())

#output
The first white-space character is located in position: 3
```

If no matches are found, the value None is returned.

### The split() Function

The split() function returns a list where the string has been split at each match:

```python
#code: Split at each white-space character
import re

txt = "The rain in Spain"
x = re.split("\s", txt)
print(x)

#output
['The', 'rain', 'in', 'Spain']
```

You can control the number of occurrences by specifying the maxsplit parameter:

```python
#code: Split the string only at the first occurrence
import re

txt = "The rain in Spain"
x = re.split("\s", txt, 1)
print(x)

#output
['The', 'rain in Spain']
```

### The sub() Function

The sub() function replaces the matches with the text of your choice:

```python
#code: Replace every white-space character with the number 9
import re

txt = "The rain in Spain"
x = re.sub("\s", "9", txt)
print(x)

#output
The9rain9in9Spain
```

You can control the number of replacements by specifying the count parameter:

```python
#code: Replace the first 2 occurrences
import re

txt = "The rain in Spain"
x = re.sub("\s", "9", txt, 2)
print(x)

#output
The9rain9in Spain
```

## Match Object

A Match Object is an object containing information about the search and the result.

Note: If there is no match, the value None will be returned, instead of the Match Object.

```python
#code: Do a search that will return a Match Object
import re

txt = "The rain in Spain"
x = re.search("ai", txt)
print(x) 

#output
<re.Match object; span=(5, 7), match='ai'>
```

The Match object has properties and methods used to retrieve information about the search, and the result:

### span()

**.span()** returns a tuple containing the start-, and end positions of the match.

```python
"""code: Print the position (start- and end-position) of the first match occurrence. 
The regular expression looks for any words that starts with an upper case "S" """

import re

txt = "The rain in Spain"
x = re.search(r"\bS\w+", txt)
print(x.span())

#output
(12, 17)
```

### String

**.string** returns the string passed into the function 

```python
#code: Print the string passed into the function

import re

txt = "The rain in Spain"
x = re.search(r"\bS\w+", txt)
print(x.string)

#output
The rain in Spain
```

### group()

**.group()** returns the part of the string where there was a match

```python
"""code: Print the part of the string where there was a match. 
The regular expression looks for any words that starts with an upper case "S""""

import re

txt = "The rain in Spain"
x = re.search(r"\bS\w+", txt)
print(x.group())

#output
Spain
```

## Sources

+ [pyhton RegEx official doc](https://docs.python.org/3/howto/regex.html)
+ [Google for education: python RegEx](https://developers.google.com/edu/python/regular-expressions)
+ [python doctor](https://python.doctor/page-expressions-regulieres-regular-python)
+ [Programmiz: python RegEx](https://www.programiz.com/python-programming/regex)