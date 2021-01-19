<link rel="stylesheet" type="text/css" media="all" href="./assets/css/book.css" />

Strings
=======

Strings are not like integers, floats, and booleans. A string is a
<span>**sequence**</span>, which means it is an ordered collection of
other values. In this chapter you’ll see how to access the characters
that make up a string, and you’ll learn about some of the methods
strings provide.

A string is a sequence
----------------------

A string is a sequence of characters. You can access the characters one
at a time with the bracket operator:

    >>> fruit = 'banana'
    >>> letter = fruit[1]

The second statement selects character number 1 from <span>fruit</span>
and assigns it to <span>letter</span>.

The expression in brackets is called an <span>**index**</span>. The
index indicates which character in the sequence you want (hence the
name).

But you might not get what you expect:

    >>> letter
    'a'

For most people, the first letter of `'banana'` is <span>b</span>, not
<span>a</span>. But for computer scientists, the index is an offset from
the beginning of the string, and the offset of the first letter is zero.

    >>> letter = fruit[0]
    >>> letter
    'b'

So <span>b</span> is the 0th letter (“zero-eth”) of `'banana'`,
<span>a</span> is the 1th letter (“one-eth”), and <span>n</span> is the
2th letter (“two-eth”).

As an index you can use an expression that contains variables and
operators:

    >>> i = 1
    >>> fruit[i]
    'a'
    >>> fruit[i+1]
    'n'

But the value of the index has to be an integer. Otherwise you get:

    >>> letter = fruit[1.5]
    TypeError: string indices must be integers

<span>len</span>
----------------

<span>len</span> is a built-in function that returns the number of
characters in a string:

    >>> fruit = 'banana'
    >>> len(fruit)
    6

To get the last letter of a string, you might be tempted to try
something like this:

    >>> length = len(fruit)
    >>> last = fruit[length]
    IndexError: string index out of range

The reason for the <span>IndexError</span> is that there is no letter in
<span>’banana’</span> with the index 6. Since we started counting at
zero, the six letters are numbered 0 to 5. To get the last character,
you have to subtract 1 from <span>length</span>:

    >>> last = fruit[length-1]
    >>> last
    'a'

Or you can use negative indices, which count backward from the end of
the string. The expression <span>fruit[-1]</span> yields the last
letter, <span>fruit[-2]</span> yields the second to last, and so on.

Traversal with a <span>for</span> loop {#for}
--------------------------------------

A lot of computations involve processing a string one character at a
time. Often they start at the beginning, select each character in turn,
do something to it, and continue until the end. This pattern of
processing is called a <span>**traversal**</span>. One way to write a
traversal is with a <span>while</span> loop:

    index = 0
    while index < len(fruit):
        letter = fruit[index]
        print(letter)
        index = index + 1

This loop traverses the string and displays each letter on a line by
itself. The loop condition is <span>index \< len(fruit)</span>, so when
<span>index</span> is equal to the length of the string, the condition
is false, and the body of the loop doesn’t run. The last character
accessed is the one with the index <span>len(fruit)-1</span>, which is
the last character in the string.

As an exercise, write a function that takes a string as an argument and
displays the letters backward, one per line.

Another way to write a traversal is with a <span>for</span> loop:

    for letter in fruit:
        print(letter)

Each time through the loop, the next character in the string is assigned
to the variable <span>letter</span>. The loop continues until no
characters are left.

The following example shows how to use concatenation (string addition)
and a <span>for</span> loop to generate an abecedarian series (that is,
in alphabetical order). In Robert McCloskey’s book <span>*Make Way for
Ducklings*</span>, the names of the ducklings are Jack, Kack, Lack,
Mack, Nack, Ouack, Pack, and Quack. This loop outputs these names in
order:

    prefixes = 'JKLMNOPQ'
    suffix = 'ack'

    for letter in prefixes:
        print(letter + suffix)

The output is:

    Jack
    Kack
    Lack
    Mack
    Nack
    Oack
    Pack
    Qack

Of course, that’s not quite right because “Ouack” and “Quack” are
misspelled. As an exercise, modify the program to fix this error.

String slices {#slice}
-------------

A segment of a string is called a <span>**slice**</span>. Selecting a
slice is similar to selecting a character:

    >>> s = 'Monty Python'
    >>> s[0:5]
    'Monty'
    >>> s[6:12]
    'Python'

The operator returns the part of the string from the “n-eth” character
to the “m-eth” character, including the first but excluding the last.
This behavior is counterintuitive, but it might help to imagine the
indices pointing <span>*between*</span> the characters, as in
Figure [fig.banana].

![image](figs/banana.pdf)

[fig.banana]

If you omit the first index (before the colon), the slice starts at the
beginning of the string. If you omit the second index, the slice goes to
the end of the string:

    >>> fruit = 'banana'
    >>> fruit[:3]
    'ban'
    >>> fruit[3:]
    'ana'

If the first index is greater than or equal to the second the result is
an <span>**empty string**</span>, represented by two quotation marks:

    >>> fruit = 'banana'
    >>> fruit[3:3]
    ''

An empty string contains no characters and has length 0, but other than
that, it is the same as any other string.

Continuing this example, what do you think <span>fruit[:]</span> means?
Try it and see.

Strings are immutable
---------------------

It is tempting to use the operator on the left side of an assignment,
with the intention of changing a character in a string. For example:

    >>> greeting = 'Hello, world!'
    >>> greeting[0] = 'J'
    TypeError: 'str' object does not support item assignment

The “object” in this case is the string and the “item” is the character
you tried to assign. For now, an object is the same thing as a value,
but we will refine that definition later (Section [equivalence]).

The reason for the error is that strings are <span>**immutable**</span>,
which means you can’t change an existing string. The best you can do is
create a new string that is a variation on the original:

    >>> greeting = 'Hello, world!'
    >>> new_greeting = 'J' + greeting[1:]
    >>> new_greeting
    'Jello, world!'

This example concatenates a new first letter onto a slice of
<span>greeting</span>. It has no effect on the original string.

Searching {#find}
---------

What does the following function do?

    def find(word, letter):
        index = 0
        while index < len(word):
            if word[index] == letter:
                return index
            index = index + 1
        return -1

In a sense, <span>find</span> is the inverse of the operator. Instead of
taking an index and extracting the corresponding character, it takes a
character and finds the index where that character appears. If the
character is not found, the function returns <span>-1</span>.

This is the first example we have seen of a <span>return</span>
statement inside a loop. If <span>word[index] == letter</span>, the
function breaks out of the loop and returns immediately.

If the character doesn’t appear in the string, the program exits the
loop normally and returns <span>-1</span>.

This pattern of computation—traversing a sequence and returning when we
find what we are looking for—is called a <span>**search**</span>.

As an exercise, modify <span>find</span> so that it has a third
parameter, the index in <span>word</span> where it should start looking.

Looping and counting {#counter}
--------------------

The following program counts the number of times the letter
<span>a</span> appears in a string:

    word = 'banana'
    count = 0
    for letter in word:
        if letter == 'a':
            count = count + 1
    print(count)

This program demonstrates another pattern of computation called a
<span>**counter**</span>. The variable <span>count</span> is initialized
to 0 and then incremented each time an <span>a</span> is found. When the
loop exits, <span>count</span> contains the result—the total number of
<span>a</span>’s.

As an exercise, encapsulate this code in a function named
<span>count</span>, and generalize it so that it accepts the string and
the letter as arguments.

Then rewrite the function so that instead of traversing the string, it
uses the three-parameter version of <span>find</span> from the previous
section.

String methods {#optional}
--------------

Strings provide methods that perform a variety of useful operations. A
method is similar to a function—it takes arguments and returns a
value—but the syntax is different. For example, the method
<span>upper</span> takes a string and returns a new string with all
uppercase letters.

Instead of the function syntax <span>upper(word)</span>, it uses the
method syntax <span>word.upper()</span>.

    >>> word = 'banana'
    >>> new_word = word.upper()
    >>> new_word
    'BANANA'

This form of dot notation specifies the name of the method,
<span>upper</span>, and the name of the string to apply the method to,
<span>word</span>. The empty parentheses indicate that this method takes
no arguments.

A method call is called an <span>**invocation**</span>; in this case, we
would say that we are invoking <span>upper</span> on <span>word</span>.

As it turns out, there is a string method named <span>find</span> that
is remarkably similar to the function we wrote:

    >>> word = 'banana'
    >>> index = word.find('a')
    >>> index
    1

In this example, we invoke <span>find</span> on <span>word</span> and
pass the letter we are looking for as a parameter.

Actually, the <span>find</span> method is more general than our
function; it can find substrings, not just characters:

    >>> word.find('na')
    2

By default, <span>find</span> starts at the beginning of the string, but
it can take a second argument, the index where it should start:

    >>> word.find('na', 3)
    4

This is an example of an <span>**optional argument**</span>;
<span>find</span> can also take a third argument, the index where it
should stop:

    >>> name = 'bob'
    >>> name.find('b', 1, 2)
    -1

This search fails because <span>b</span> does not appear in the index
range from <span>1</span> to <span>2</span>, not including
<span>2</span>. Searching up to, but not including, the second index
makes <span>find</span> consistent with the slice operator.

The <span>in</span> operator {#inboth}
----------------------------

The word <span>in</span> is a boolean operator that takes two strings
and returns <span>True</span> if the first appears as a substring in the
second:

    >>> 'a' in 'banana'
    True
    >>> 'seed' in 'banana'
    False

For example, the following function prints all the letters from
<span>word1</span> that also appear in <span>word2</span>:

    def in_both(word1, word2):
        for letter in word1:
            if letter in word2:
                print(letter)

With well-chosen variable names, Python sometimes reads like English.
You could read this loop, “for (each) letter in (the first) word, if
(the) letter (appears) in (the second) word, print (the) letter.”

Here’s what you get if you compare apples and oranges:

    >>> in_both('apples', 'oranges')
    a
    e
    s

String comparison
-----------------

The relational operators work on strings. To see if two strings are
equal:

    if word == 'banana':
        print('All right, bananas.')

Other relational operations are useful for putting words in alphabetical
order:

    if word < 'banana':
        print('Your word, ' + word + ', comes before banana.')
    elif word > 'banana':
        print('Your word, ' + word + ', comes after banana.')
    else:
        print('All right, bananas.')

Python does not handle uppercase and lowercase letters the same way
people do. All the uppercase letters come before all the lowercase
letters, so:

    Your word, Pineapple, comes before banana.

A common way to address this problem is to convert strings to a standard
format, such as all lowercase, before performing the comparison. Keep
that in mind in case you have to defend yourself against a man armed
with a Pineapple.

Debugging {#debugging-4}
---------

When you use indices to traverse the values in a sequence, it is tricky
to get the beginning and end of the traversal right. Here is a function
that is supposed to compare two words and return <span>True</span> if
one of the words is the reverse of the other, but it contains two
errors:

    def is_reverse(word1, word2):
        if len(word1) != len(word2):
            return False
        
        i = 0
        j = len(word2)

        while j > 0:
            if word1[i] != word2[j]:
                return False
            i = i+1
            j = j-1

        return True

The first <span>if</span> statement checks whether the words are the
same length. If not, we can return <span>False</span> immediately.
Otherwise, for the rest of the function, we can assume that the words
are the same length. This is an example of the guardian pattern in
Section [guardian].

<span>i</span> and <span>j</span> are indices: <span>i</span> traverses
<span>word1</span> forward while <span>j</span> traverses
<span>word2</span> backward. If we find two letters that don’t match, we
can return <span>False</span> immediately. If we get through the whole
loop and all the letters match, we return <span>True</span>.

If we test this function with the words “pots” and “stop”, we expect the
return value <span>True</span>, but we get an IndexError:

    >>> is_reverse('pots', 'stop')
    ...
      File "reverse.py", line 15, in is_reverse
        if word1[i] != word2[j]:
    IndexError: string index out of range

For debugging this kind of error, my first move is to print the values
of the indices immediately before the line where the error appears.

        while j > 0:
            print(i, j)        # print here
            
            if word1[i] != word2[j]:
                return False
            i = i+1
            j = j-1

Now when I run the program again, I get more information:

    >>> is_reverse('pots', 'stop')
    0 4
    ...
    IndexError: string index out of range

The first time through the loop, the value of <span>j</span> is 4, which
is out of range for the string `'pots'`. The index of the last character
is 3, so the initial value for <span>j</span> should be
<span>len(word2)-1</span>.

If I fix that error and run the program again, I get:

    >>> is_reverse('pots', 'stop')
    0 3
    1 2
    2 1
    True

This time we get the right answer, but it looks like the loop only ran
three times, which is suspicious. To get a better idea of what is
happening, it is useful to draw a state diagram. During the first
iteration, the frame for `is_reverse` is shown in Figure [fig.state4].

![image](figs/state4.pdf)

[fig.state4]

I took some license by arranging the variables in the frame and adding
dotted lines to show that the values of <span>i</span> and
<span>j</span> indicate characters in <span>word1</span> and
<span>word2</span>.

Starting with this diagram, run the program on paper, changing the
values of <span>i</span> and <span>j</span> during each iteration. Find
and fix the second error in this function. [isreverse]

Glossary
--------

object:
:   Something a variable can refer to. For now, you can use “object” and
    “value” interchangeably.

sequence:
:   An ordered collection of values where each value is identified by an
    integer index.

item:
:   One of the values in a sequence.

index:
:   An integer value used to select an item in a sequence, such as a
    character in a string. In Python indices start from 0.

slice:
:   A part of a string specified by a range of indices.

empty string:
:   A string with no characters and length 0, represented by two
    quotation marks.

immutable:
:   The property of a sequence whose items cannot be changed.

traverse:
:   To iterate through the items in a sequence, performing a similar
    operation on each.

search:
:   A pattern of traversal that stops when it finds what it is looking
    for.

counter:
:   A variable used to count something, usually initialized to zero and
    then incremented.

invocation:
:   A statement that calls a method.

optional argument:
:   A function or method argument that is not required.

Exercises
---------

Read the documentation of the string methods at
<http://docs.python.org/3/library/stdtypes.html#string-methods>. You
might want to experiment with some of them to make sure you understand
how they work. <span>strip</span> and <span>replace</span> are
particularly useful.

The documentation uses a syntax that might be confusing. For example, in
`find(sub[, start[, end]])`, the brackets indicate optional arguments.
So <span>sub</span> is required, but <span>start</span> is optional, and
if you include <span>start</span>, then <span>end</span> is optional.

There is a string method called <span>count</span> that is similar to
the function in Section [counter]. Read the documentation of this method
and write an invocation that counts the number of <span>a</span>’s in
`'banana'`.

A string slice can take a third index that specifies the “step size”;
that is, the number of spaces between successive characters. A step size
of 2 means every other character; 3 means every third, etc.

    >>> fruit = 'banana'
    >>> fruit[0:5:2]
    'bnn'

A step size of -1 goes through the word backwards, so the slice `[::-1]`
generates a reversed string.

Use this idiom to write a one-line version of `is_palindrome` from
Exercise [palindrome].

The following functions are all <span>*intended*</span> to check whether
a string contains any lowercase letters, but at least some of them are
wrong. For each function, describe what the function actually does
(assuming that the parameter is a string).

    def any_lowercase1(s):
        for c in s:
            if c.islower():
                return True
            else:
                return False

    def any_lowercase2(s):
        for c in s:
            if 'c'.islower():
                return 'True'
            else:
                return 'False'

    def any_lowercase3(s):
        for c in s:
            flag = c.islower()
        return flag

    def any_lowercase4(s):
        flag = False
        for c in s:
            flag = flag or c.islower()
        return flag

    def any_lowercase5(s):
        for c in s:
            if not c.islower():
                return False
        return True

[exrotate] A Caesar cypher is a weak form of encryption that involves
“rotating” each letter by a fixed number of places. To rotate a letter
means to shift it through the alphabet, wrapping around to the beginning
if necessary, so ’A’ rotated by 3 is ’D’ and ’Z’ rotated by 1 is ’A’.

To rotate a word, rotate each letter by the same amount. For example,
“cheer” rotated by 7 is “jolly” and “melon” rotated by -10 is “cubed”.
In the movie <span>*2001: A Space Odyssey*</span>, the ship computer is
called HAL, which is IBM rotated by -1.

Write a function called `rotate_word` that takes a string and an integer
as parameters, and returns a new string that contains the letters from
the original string rotated by the given amount.

You might want to use the built-in function <span>ord</span>, which
converts a character to a numeric code, and <span>chr</span>, which
converts numeric codes to characters. Letters of the alphabet are
encoded in alphabetical order, so for example:

    >>> ord('c') - ord('a')
    2

Because `'c'` is the two-eth letter of the alphabet. But beware: the
numeric codes for upper case letters are different.

Potentially offensive jokes on the Internet are sometimes encoded in
ROT13, which is a Caesar cypher with rotation 13. If you are not easily
offended, find and decode some of them. Solution:
<http://thinkpython2.com/code/rotate.py>.