<link rel="stylesheet" type="text/css" media="all" href="./assets/css/book.css" />

Dictionaries
============

This chapter presents another built-in type called a dictionary.
Dictionaries are one of Python’s best features; they are the building
blocks of many efficient and elegant algorithms.

A dictionary is a mapping
-------------------------

A <span>**dictionary**</span> is like a list, but more general. In a
list, the indices have to be integers; in a dictionary they can be
(almost) any type.

A dictionary contains a collection of indices, which are called
<span>**keys**</span>, and a collection of values. Each key is
associated with a single value. The association of a key and a value is
called a <span>**key-value pair**</span> or sometimes an
<span>**item**</span>.

In mathematical language, a dictionary represents a
<span>**mapping**</span> from keys to values, so you can also say that
each key “maps to” a value. As an example, we’ll build a dictionary that
maps from English to Spanish words, so the keys and the values are all
strings.

The function <span>dict</span> creates a new dictionary with no items.
Because <span>dict</span> is the name of a built-in function, you should
avoid using it as a variable name.

    >>> eng2sp = dict()
    >>> eng2sp
    {}

The squiggly-brackets, `{}`, represent an empty dictionary. To add items
to the dictionary, you can use square brackets:

    >>> eng2sp['one'] = 'uno'

This line creates an item that maps from the key `'one'` to the value
`'uno'`. If we print the dictionary again, we see a key-value pair with
a colon between the key and value:

    >>> eng2sp
    {'one': 'uno'}

This output format is also an input format. For example, you can create
a new dictionary with three items:

    >>> eng2sp = {'one': 'uno', 'two': 'dos', 'three': 'tres'}

But if you print <span>eng2sp</span>, you might be surprised:

    >>> eng2sp
    {'one': 'uno', 'three': 'tres', 'two': 'dos'}

The order of the key-value pairs might not be the same. If you type the
same example on your computer, you might get a different result. In
general, the order of items in a dictionary is unpredictable.

But that’s not a problem because the elements of a dictionary are never
indexed with integer indices. Instead, you use the keys to look up the
corresponding values:

    >>> eng2sp['two']
    'dos'

The key `'two'` always maps to the value `'dos'` so the order of the
items doesn’t matter.

If the key isn’t in the dictionary, you get an exception:

    >>> eng2sp['four']
    KeyError: 'four'

The <span>len</span> function works on dictionaries; it returns the
number of key-value pairs:

    >>> len(eng2sp)
    3

The <span>in</span> operator works on dictionaries, too; it tells you
whether something appears as a <span>*key*</span> in the dictionary
(appearing as a value is not good enough).

    >>> 'one' in eng2sp
    True
    >>> 'uno' in eng2sp
    False

To see whether something appears as a value in a dictionary, you can use
the method <span>values</span>, which returns a collection of values,
and then use the <span>in</span> operator:

    >>> vals = eng2sp.values()
    >>> 'uno' in vals
    True

The <span>in</span> operator uses different algorithms for lists and
dictionaries. For lists, it searches the elements of the list in order,
as in Section [find]. As the list gets longer, the search time gets
longer in direct proportion.

Python dictionaries use a data structure called a
<span>**hashtable**</span> that has a remarkable property: the
<span>in</span> operator takes about the same amount of time no matter
how many items are in the dictionary. I explain how that’s possible in
Section [hashtable], but the explanation might not make sense until
you’ve read a few more chapters.

Dictionary as a collection of counters {#histogram}
--------------------------------------

Suppose you are given a string and you want to count how many times each
letter appears. There are several ways you could do it:

1.  You could create 26 variables, one for each letter of the alphabet.
    Then you could traverse the string and, for each character,
    increment the corresponding counter, probably using a chained
    conditional.

2.  You could create a list with 26 elements. Then you could convert
    each character to a number (using the built-in function
    <span>ord</span>), use the number as an index into the list, and
    increment the appropriate counter.

3.  You could create a dictionary with characters as keys and counters
    as the corresponding values. The first time you see a character, you
    would add an item to the dictionary. After that you would increment
    the value of an existing item.

Each of these options performs the same computation, but each of them
implements that computation in a different way.

An <span>**implementation**</span> is a way of performing a computation;
some implementations are better than others. For example, an advantage
of the dictionary implementation is that we don’t have to know ahead of
time which letters appear in the string and we only have to make room
for the letters that do appear.

Here is what the code might look like:

    def histogram(s):
        d = dict()
        for c in s:
            if c not in d:
                d[c] = 1
            else:
                d[c] += 1
        return d

The name of the function is <span>histogram</span>, which is a
statistical term for a collection of counters (or frequencies).

The first line of the function creates an empty dictionary. The
<span>for</span> loop traverses the string. Each time through the loop,
if the character <span>c</span> is not in the dictionary, we create a
new item with key <span>c</span> and the initial value 1 (since we have
seen this letter once). If <span>c</span> is already in the dictionary
we increment <span>d[c]</span>.

Here’s how it works:

    >>> h = histogram('brontosaurus')
    >>> h
    {'a': 1, 'b': 1, 'o': 2, 'n': 1, 's': 2, 'r': 2, 'u': 2, 't': 1}

The histogram indicates that the letters `'a'` and `'b'` appear once;
`'o'` appears twice, and so on.

Dictionaries have a method called <span>get</span> that takes a key and
a default value. If the key appears in the dictionary, <span>get</span>
returns the corresponding value; otherwise it returns the default value.
For example:

    >>> h = histogram('a')
    >>> h
    {'a': 1}
    >>> h.get('a', 0)
    1
    >>> h.get('c', 0)
    0

As an exercise, use <span>get</span> to write <span>histogram</span>
more concisely. You should be able to eliminate the <span>if</span>
statement.

Looping and dictionaries
------------------------

If you use a dictionary in a <span>for</span> statement, it traverses
the keys of the dictionary. For example, `print_hist` prints each key
and the corresponding value:

    def print_hist(h):
        for c in h:
            print(c, h[c])

Here’s what the output looks like:

    >>> h = histogram('parrot')
    >>> print_hist(h)
    a 1
    p 1
    r 2
    t 1
    o 1

Again, the keys are in no particular order. To traverse the keys in
sorted order, you can use the built-in function <span>sorted</span>:

    >>> for key in sorted(h):
    ...     print(key, h[key])
    a 1
    o 1
    p 1
    r 2
    t 1

Reverse lookup {#raise}
--------------

Given a dictionary <span>d</span> and a key <span>k</span>, it is easy
to find the corresponding value <span>v = d[k]</span>. This operation is
called a <span>**lookup**</span>.

But what if you have <span>v</span> and you want to find <span>k</span>?
You have two problems: first, there might be more than one key that maps
to the value <span>v</span>. Depending on the application, you might be
able to pick one, or you might have to make a list that contains all of
them. Second, there is no simple syntax to do a <span>**reverse
lookup**</span>; you have to search.

Here is a function that takes a value and returns the first key that
maps to that value:

    def reverse_lookup(d, v):
        for k in d:
            if d[k] == v:
                return k
        raise LookupError()

This function is yet another example of the search pattern, but it uses
a feature we haven’t seen before, <span>raise</span>. The <span>**raise
statement**</span> causes an exception; in this case it causes a
<span>LookupError</span>, which is a built-in exception used to indicate
that a lookup operation failed.

If we get to the end of the loop, that means <span>v</span> doesn’t
appear in the dictionary as a value, so we raise an exception.

Here is an example of a successful reverse lookup:

    >>> h = histogram('parrot')
    >>> key = reverse_lookup(h, 2)
    >>> key
    'r'

And an unsuccessful one:

    >>> key = reverse_lookup(h, 3)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "<stdin>", line 5, in reverse_lookup
    LookupError

The effect when you raise an exception is the same as when Python raises
one: it prints a traceback and an error message.

When you raise an exception, you can provide a detailed error message as
an optional argument. For example:

    >>> raise LookupError('value does not appear in the dictionary')
    Traceback (most recent call last):
      File "<stdin>", line 1, in ?
    LookupError: value does not appear in the dictionary

A reverse lookup is much slower than a forward lookup; if you have to do
it often, or if the dictionary gets big, the performance of your program
will suffer.

Dictionaries and lists {#invert}
----------------------

Lists can appear as values in a dictionary. For example, if you are
given a dictionary that maps from letters to frequencies, you might want
to invert it; that is, create a dictionary that maps from frequencies to
letters. Since there might be several letters with the same frequency,
each value in the inverted dictionary should be a list of letters.

Here is a function that inverts a dictionary:

    def invert_dict(d):
        inverse = dict()
        for key in d:
            val = d[key]
            if val not in inverse:
                inverse[val] = [key]
            else:
                inverse[val].append(key)
        return inverse

Each time through the loop, <span>key</span> gets a key from
<span>d</span> and <span>val</span> gets the corresponding value. If
<span>val</span> is not in <span>inverse</span>, that means we haven’t
seen it before, so we create a new item and initialize it with a
<span>**singleton**</span> (a list that contains a single element).
Otherwise we have seen this value before, so we append the corresponding
key to the list.

Here is an example:

    >>> hist = histogram('parrot')
    >>> hist
    {'a': 1, 'p': 1, 'r': 2, 't': 1, 'o': 1}
    >>> inverse = invert_dict(hist)
    >>> inverse
    {1: ['a', 'p', 't', 'o'], 2: ['r']}

![image](figs/dict1.pdf)

[fig.dict1]

Figure [fig.dict1] is a state diagram showing <span>hist</span> and
<span>inverse</span>. A dictionary is represented as a box with the type
<span>dict</span> above it and the key-value pairs inside. If the values
are integers, floats or strings, I draw them inside the box, but I
usually draw lists outside the box, just to keep the diagram simple.

Lists can be values in a dictionary, as this example shows, but they
cannot be keys. Here’s what happens if you try:

    >>> t = [1, 2, 3]
    >>> d = dict()
    >>> d[t] = 'oops'
    Traceback (most recent call last):
      File "<stdin>", line 1, in ?
    TypeError: list objects are unhashable

I mentioned earlier that a dictionary is implemented using a hashtable
and that means that the keys have to be <span>**hashable**</span>.

A <span>**hash**</span> is a function that takes a value (of any kind)
and returns an integer. Dictionaries use these integers, called hash
values, to store and look up key-value pairs.

This system works fine if the keys are immutable. But if the keys are
mutable, like lists, bad things happen. For example, when you create a
key-value pair, Python hashes the key and stores it in the corresponding
location. If you modify the key and then hash it again, it would go to a
different location. In that case you might have two entries for the same
key, or you might not be able to find a key. Either way, the dictionary
wouldn’t work correctly.

That’s why keys have to be hashable, and why mutable types like lists
aren’t. The simplest way to get around this limitation is to use tuples,
which we will see in the next chapter.

Since dictionaries are mutable, they can’t be used as keys, but they
<span>*can*</span> be used as values.

Memos {#memoize}
-----

If you played with the <span>fibonacci</span> function from
Section [one.more.example], you might have noticed that the bigger the
argument you provide, the longer the function takes to run. Furthermore,
the run time increases quickly.

To understand why, consider Figure [fig.fibonacci], which shows the
<span>**call graph**</span> for <span>fibonacci</span> with
<span>n=4</span>:

![image](figs/fibonacci.pdf)

[fig.fibonacci]

A call graph shows a set of function frames, with lines connecting each
frame to the frames of the functions it calls. At the top of the graph,
<span>fibonacci</span> with <span>n=4</span> calls
<span>fibonacci</span> with <span>n=3</span> and <span>n=2</span>. In
turn, <span>fibonacci</span> with <span>n=3</span> calls
<span>fibonacci</span> with <span>n=2</span> and <span>n=1</span>. And
so on.

Count how many times <span>fibonacci(0)</span> and
<span>fibonacci(1)</span> are called. This is an inefficient solution to
the problem, and it gets worse as the argument gets bigger.

One solution is to keep track of values that have already been computed
by storing them in a dictionary. A previously computed value that is
stored for later use is called a <span>**memo**</span>. Here is a
“memoized” version of <span>fibonacci</span>:

    known = {0:0, 1:1}

    def fibonacci(n):
        if n in known:
            return known[n]

        res = fibonacci(n-1) + fibonacci(n-2)
        known[n] = res
        return res

<span>known</span> is a dictionary that keeps track of the Fibonacci
numbers we already know. It starts with two items: 0 maps to 0 and 1
maps to 1.

Whenever <span>fibonacci</span> is called, it checks <span>known</span>.
If the result is already there, it can return immediately. Otherwise it
has to compute the new value, add it to the dictionary, and return it.

If you run this version of <span>fibonacci</span> and compare it with
the original, you will find that it is much faster.

Global variables
----------------

In the previous example, <span>known</span> is created outside the
function, so it belongs to the special frame called `__main__`.
Variables in `__main__` are sometimes called <span>**global**</span>
because they can be accessed from any function. Unlike local variables,
which disappear when their function ends, global variables persist from
one function call to the next.

It is common to use global variables for <span>**flags**</span>; that
is, boolean variables that indicate (“flag”) whether a condition is
true. For example, some programs use a flag named <span>verbose</span>
to control the level of detail in the output:

    verbose = True

    def example1():
        if verbose:
            print('Running example1')

If you try to reassign a global variable, you might be surprised. The
following example is supposed to keep track of whether the function has
been called:

    been_called = False

    def example2():
        been_called = True         # WRONG

But if you run it you will see that the value of `been_called` doesn’t
change. The problem is that <span>example2</span> creates a new local
variable named `been_called`. The local variable goes away when the
function ends, and has no effect on the global variable.

To reassign a global variable inside a function you have to
<span>**declare**</span> the global variable before you use it:

    been_called = False

    def example2():
        global been_called 
        been_called = True

The <span>**global statement**</span> tells the interpreter something
like, “In this function, when I say `been_called`, I mean the global
variable; don’t create a local one.”

Here’s an example that tries to update a global variable:

    count = 0

    def example3():
        count = count + 1          # WRONG

If you run it you get:

    UnboundLocalError: local variable 'count' referenced before assignment

Python assumes that <span>count</span> is local, and under that
assumption you are reading it before writing it. The solution, again, is
to declare <span>count</span> global.

    def example3():
        global count
        count += 1

If a global variable refers to a mutable value, you can modify the value
without declaring the variable:

    known = {0:0, 1:1}

    def example4():
        known[2] = 1

So you can add, remove and replace elements of a global list or
dictionary, but if you want to reassign the variable, you have to
declare it:

    def example5():
        global known
        known = dict()

Global variables can be useful, but if you have a lot of them, and you
modify them frequently, they can make programs hard to debug.

Debugging {#debugging-7}
---------

As you work with bigger datasets it can become unwieldy to debug by
printing and checking the output by hand. Here are some suggestions for
debugging large datasets:

Scale down the input:
:   If possible, reduce the size of the dataset. For example if the
    program reads a text file, start with just the first 10 lines, or
    with the smallest example you can find. You can either edit the
    files themselves, or (better) modify the program so it reads only
    the first <span>n</span> lines.

    If there is an error, you can reduce <span>n</span> to the smallest
    value that manifests the error, and then increase it gradually as
    you find and correct errors.

Check summaries and types:
:   Instead of printing and checking the entire dataset, consider
    printing summaries of the data: for example, the number of items in
    a dictionary or the total of a list of numbers.

    A common cause of runtime errors is a value that is not the right
    type. For debugging this kind of error, it is often enough to print
    the type of a value.

Write self-checks:
:   Sometimes you can write code to check for errors automatically. For
    example, if you are computing the average of a list of numbers, you
    could check that the result is not greater than the largest element
    in the list or less than the smallest. This is called a “sanity
    check” because it detects results that are “insane”.

    Another kind of check compares the results of two different
    computations to see if they are consistent. This is called a
    “consistency check”.

Format the output:
:   Formatting debugging output can make it easier to spot an error. We
    saw an example in Section [factdebug]. Another tool you might find
    useful is the <span>pprint</span> module, which provides a
    <span>pprint</span> function that displays built-in types in a more
    human-readable format (<span>pprint</span> stands for “pretty
    print”).

Again, time you spend building scaffolding can reduce the time you spend
debugging.

Glossary
--------

mapping:
:   A relationship in which each element of one set corresponds to an
    element of another set.

dictionary:
:   A mapping from keys to their corresponding values.

key-value pair:
:   The representation of the mapping from a key to a value.

item:
:   In a dictionary, another name for a key-value pair.

key:
:   An object that appears in a dictionary as the first part of a
    key-value pair.

value:
:   An object that appears in a dictionary as the second part of a
    key-value pair. This is more specific than our previous use of the
    word “value”.

implementation:
:   A way of performing a computation.

hashtable:
:   The algorithm used to implement Python dictionaries.

hash function:
:   A function used by a hashtable to compute the location for a key.

hashable:
:   A type that has a hash function. Immutable types like integers,
    floats and strings are hashable; mutable types like lists and
    dictionaries are not.

lookup:
:   A dictionary operation that takes a key and finds the corresponding
    value.

reverse lookup:
:   A dictionary operation that takes a value and finds one or more keys
    that map to it.

raise statement:
:   A statement that (deliberately) raises an exception.

singleton:
:   A list (or other sequence) with a single element.

call graph:
:   A diagram that shows every frame created during the execution of a
    program, with an arrow from each caller to each callee.

memo:
:   A computed value stored to avoid unnecessary future computation.

global variable:
:   A variable defined outside a function. Global variables can be
    accessed from any function.

global statement:
:   A statement that declares a variable name global.

flag:
:   A boolean variable used to indicate whether a condition is true.

declaration:
:   A statement like <span>global</span> that tells the interpreter
    something about a variable.

Exercises
---------

[wordlist2]

Write a function that reads the words in <span>words.txt</span> and
stores them as keys in a dictionary. It doesn’t matter what the values
are. Then you can use the <span>in</span> operator as a fast way to
check whether a string is in the dictionary.

If you did Exercise [wordlist1], you can compare the speed of this
implementation with the list <span>in</span> operator and the bisection
search.

[setdefault]

Read the documentation of the dictionary method <span>setdefault</span>
and use it to write a more concise version of `invert_dict`. Solution:
<http://thinkpython2.com/code/invert_dict.py>.

Memoize the Ackermann function from Exercise [ackermann] and see if
memoization makes it possible to evaluate the function with bigger
arguments. Hint: no. Solution:
<http://thinkpython2.com/code/ackermann_memo.py>.

If you did Exercise [duplicate], you already have a function named
`has_duplicates` that takes a list as a parameter and returns
<span>True</span> if there is any object that appears more than once in
the list.

Use a dictionary to write a faster, simpler version of `has_duplicates`.
Solution: <http://thinkpython2.com/code/has_duplicates.py>.

[exrotatepairs]

Two words are “rotate pairs” if you can rotate one of them and get the
other (see `rotate_word` in Exercise [exrotate]).

Write a program that reads a wordlist and finds all the rotate pairs.
Solution: <http://thinkpython2.com/code/rotate_pairs.py>.

Here’s another Puzzler from <span>*Car Talk*</span>
(<http://www.cartalk.com/content/puzzlers>):

> This was sent in by a fellow named Dan O’Leary. He came upon a common
> one-syllable, five-letter word recently that has the following unique
> property. When you remove the first letter, the remaining letters form
> a homophone of the original word, that is a word that sounds exactly
> the same. Replace the first letter, that is, put it back and remove
> the second letter and the result is yet another homophone of the
> original word. And the question is, what’s the word?
>
> Now I’m going to give you an example that doesn’t work. Let’s look at
> the five-letter word, ‘wrack.’ W-R-A-C-K, you know like to ‘wrack with
> pain.’ If I remove the first letter, I am left with a four-letter
> word, ’R-A-C-K.’ As in, ‘Holy cow, did you see the rack on that buck!
> It must have been a nine-pointer!’ It’s a perfect homophone. If you
> put the ‘w’ back, and remove the ‘r,’ instead, you’re left with the
> word, ‘wack,’ which is a real word, it’s just not a homophone of the
> other two words.
>
> But there is, however, at least one word that Dan and we know of,
> which will yield two homophones if you remove either of the first two
> letters to make two, new four-letter words. The question is, what’s
> the word?

You can use the dictionary from Exercise [wordlist2] to check whether a
string is in the word list.

To check whether two words are homophones, you can use the CMU
Pronouncing Dictionary. You can download it from
<http://www.speech.cs.cmu.edu/cgi-bin/cmudict> or from
<http://thinkpython2.com/code/c06d> and you can also download
<http://thinkpython2.com/code/pronounce.py>, which provides a function
named `read_dictionary` that reads the pronouncing dictionary and
returns a Python dictionary that maps from each word to a string that
describes its primary pronunciation.

Write a program that lists all the words that solve the Puzzler.
Solution: <http://thinkpython2.com/code/homophone.py>.

Tuples {#tuplechap}
======

This chapter presents one more built-in type, the tuple, and then shows
how lists, dictionaries, and tuples work together. I also present a
useful feature for variable-length argument lists, the gather and
scatter operators.

One note: there is no consensus on how to pronounce “tuple”. Some people
say “tuh-ple”, which rhymes with “supple”. But in the context of
programming, most people say “too-ple”, which rhymes with “quadruple”.

Tuples are immutable
--------------------

A tuple is a sequence of values. The values can be any type, and they
are indexed by integers, so in that respect tuples are a lot like lists.
The important difference is that tuples are immutable.

Syntactically, a tuple is a comma-separated list of values:

    >>> t = 'a', 'b', 'c', 'd', 'e'

Although it is not necessary, it is common to enclose tuples in
parentheses:

    >>> t = ('a', 'b', 'c', 'd', 'e')

To create a tuple with a single element, you have to include a final
comma:

    >>> t1 = 'a',
    >>> type(t1)
    <class 'tuple'>

A value in parentheses is not a tuple:

    >>> t2 = ('a')
    >>> type(t2)
    <class 'str'>

Another way to create a tuple is the built-in function
<span>tuple</span>. With no argument, it creates an empty tuple:

    >>> t = tuple()
    >>> t
    ()

If the argument is a sequence (string, list or tuple), the result is a
tuple with the elements of the sequence:

    >>> t = tuple('lupins')
    >>> t
    ('l', 'u', 'p', 'i', 'n', 's')

Because <span>tuple</span> is the name of a built-in function, you
should avoid using it as a variable name.

Most list operators also work on tuples. The bracket operator indexes an
element:

    >>> t = ('a', 'b', 'c', 'd', 'e')
    >>> t[0]
    'a'

And the slice operator selects a range of elements.

    >>> t[1:3]
    ('b', 'c')

But if you try to modify one of the elements of the tuple, you get an
error:

    >>> t[0] = 'A'
    TypeError: object doesn't support item assignment

Because tuples are immutable, you can’t modify the elements. But you can
replace one tuple with another:

    >>> t = ('A',) + t[1:]
    >>> t
    ('A', 'b', 'c', 'd', 'e')

This statement makes a new tuple and then makes <span>t</span> refer to
it.

The relational operators work with tuples and other sequences; Python
starts by comparing the first element from each sequence. If they are
equal, it goes on to the next elements, and so on, until it finds
elements that differ. Subsequent elements are not considered (even if
they are really big).

    >>> (0, 1, 2) < (0, 3, 4)
    True
    >>> (0, 1, 2000000) < (0, 3, 4)
    True

Tuple assignment {#tuple.assignment}
----------------

It is often useful to swap the values of two variables. With
conventional assignments, you have to use a temporary variable. For
example, to swap <span>a</span> and <span>b</span>:

    >>> temp = a
    >>> a = b
    >>> b = temp

This solution is cumbersome; <span>**tuple assignment**</span> is more
elegant:

    >>> a, b = b, a

The left side is a tuple of variables; the right side is a tuple of
expressions. Each value is assigned to its respective variable. All the
expressions on the right side are evaluated before any of the
assignments.

The number of variables on the left and the number of values on the
right have to be the same:

    >>> a, b = 1, 2, 3
    ValueError: too many values to unpack

More generally, the right side can be any kind of sequence (string, list
or tuple). For example, to split an email address into a user name and a
domain, you could write:

    >>> addr = 'monty@python.org'
    >>> uname, domain = addr.split('@')

The return value from <span>split</span> is a list with two elements;
the first element is assigned to <span>uname</span>, the second to
<span>domain</span>.

    >>> uname
    'monty'
    >>> domain
    'python.org'

Tuples as return values
-----------------------

Strictly speaking, a function can only return one value, but if the
value is a tuple, the effect is the same as returning multiple values.
For example, if you want to divide two integers and compute the quotient
and remainder, it is inefficient to compute <span>x//y</span> and then
<span>x%y</span>. It is better to compute them both at the same time.

The built-in function <span>divmod</span> takes two arguments and
returns a tuple of two values, the quotient and remainder. You can store
the result as a tuple:

    >>> t = divmod(7, 3)
    >>> t
    (2, 1)

Or use tuple assignment to store the elements separately:

    >>> quot, rem = divmod(7, 3)
    >>> quot
    2
    >>> rem
    1

Here is an example of a function that returns a tuple:

    def min_max(t):
        return min(t), max(t)

<span>max</span> and <span>min</span> are built-in functions that find
the largest and smallest elements of a sequence. `min_max` computes both
and returns a tuple of two values.

Variable-length argument tuples {#gather}
-------------------------------

Functions can take a variable number of arguments. A parameter name that
begins with <span>**gathers**</span> arguments into a tuple. For
example, <span>printall</span> takes any number of arguments and prints
them:

    def printall(*args):
        print(args)

The gather parameter can have any name you like, but <span>args</span>
is conventional. Here’s how the function works:

    >>> printall(1, 2.0, '3')
    (1, 2.0, '3')

The complement of gather is <span>**scatter**</span>. If you have a
sequence of values and you want to pass it to a function as multiple
arguments, you can use the operator. For example, <span>divmod</span>
takes exactly two arguments; it doesn’t work with a tuple:

    >>> t = (7, 3)
    >>> divmod(t)
    TypeError: divmod expected 2 arguments, got 1

But if you scatter the tuple, it works:

    >>> divmod(*t)
    (2, 1)

Many of the built-in functions use variable-length argument tuples. For
example, <span>max</span> and <span>min</span> can take any number of
arguments:

    >>> max(1, 2, 3)
    3

But <span>sum</span> does not.

    >>> sum(1, 2, 3)
    TypeError: sum expected at most 2 arguments, got 3

As an exercise, write a function called `sum_all` that takes any number
of arguments and returns their sum.

Lists and tuples
----------------

<span>zip</span> is a built-in function that takes two or more sequences
and interleaves them. The name of the function refers to a zipper, which
interleaves two rows of teeth.

This example zips a string and a list:

    >>> s = 'abc'
    >>> t = [0, 1, 2]
    >>> zip(s, t)
    <zip object at 0x7f7d0a9e7c48>

The result is a <span>**zip object**</span> that knows how to iterate
through the pairs. The most common use of <span>zip</span> is in a
<span>for</span> loop:

    >>> for pair in zip(s, t):
    ...     print(pair)
    ...
    ('a', 0)
    ('b', 1)
    ('c', 2)

A zip object is a kind of <span>**iterator**</span>, which is any object
that iterates through a sequence. Iterators are similar to lists in some
ways, but unlike lists, you can’t use an index to select an element from
an iterator.

If you want to use list operators and methods, you can use a zip object
to make a list:

    >>> list(zip(s, t))
    [('a', 0), ('b', 1), ('c', 2)]

The result is a list of tuples; in this example, each tuple contains a
character from the string and the corresponding element from the list.

If the sequences are not the same length, the result has the length of
the shorter one.

    >>> list(zip('Anne', 'Elk'))
    [('A', 'E'), ('n', 'l'), ('n', 'k')]

You can use tuple assignment in a <span>for</span> loop to traverse a
list of tuples:

    t = [('a', 0), ('b', 1), ('c', 2)]
    for letter, number in t:
        print(number, letter)

Each time through the loop, Python selects the next tuple in the list
and assigns the elements to <span>letter</span> and <span>number</span>.
The output of this loop is:

    0 a
    1 b
    2 c

If you combine <span>zip</span>, <span>for</span> and tuple assignment,
you get a useful idiom for traversing two (or more) sequences at the
same time. For example, `has_match` takes two sequences, <span>t1</span>
and <span>t2</span>, and returns <span>True</span> if there is an index
<span>i</span> such that <span>t1[i] == t2[i]</span>:

    def has_match(t1, t2):
        for x, y in zip(t1, t2):
            if x == y:
                return True
        return False

If you need to traverse the elements of a sequence and their indices,
you can use the built-in function <span>enumerate</span>:

    for index, element in enumerate('abc'):
        print(index, element)

The result from <span>enumerate</span> is an enumerate object, which
iterates a sequence of pairs; each pair contains an index (starting from
0) and an element from the given sequence. In this example, the output
is

    0 a
    1 b
    2 c

Again.

Dictionaries and tuples {#dictuple}
-----------------------

Dictionaries have a method called <span>items</span> that returns a
sequence of tuples, where each tuple is a key-value pair.

    >>> d = {'a':0, 'b':1, 'c':2}
    >>> t = d.items()
    >>> t
    dict_items([('c', 2), ('a', 0), ('b', 1)])

The result is a `dict_items` object, which is an iterator that iterates
the key-value pairs. You can use it in a <span>for</span> loop like
this:

    >>> for key, value in d.items():
    ...     print(key, value)
    ...
    c 2
    a 0
    b 1

As you should expect from a dictionary, the items are in no particular
order.

Going in the other direction, you can use a list of tuples to initialize
a new dictionary:

    >>> t = [('a', 0), ('c', 2), ('b', 1)]
    >>> d = dict(t)
    >>> d
    {'a': 0, 'c': 2, 'b': 1}

Combining <span>dict</span> with <span>zip</span> yields a concise way
to create a dictionary:

    >>> d = dict(zip('abc', range(3)))
    >>> d
    {'a': 0, 'c': 2, 'b': 1}

The dictionary method <span>update</span> also takes a list of tuples
and adds them, as key-value pairs, to an existing dictionary.

It is common to use tuples as keys in dictionaries (primarily because
you can’t use lists). For example, a telephone directory might map from
last-name, first-name pairs to telephone numbers. Assuming that we have
defined <span>last</span>, <span>first</span> and <span>number</span>,
we could write:

    directory[last, first] = number

The expression in brackets is a tuple. We could use tuple assignment to
traverse this dictionary.

    for last, first in directory:
        print(first, last, directory[last,first])

This loop traverses the keys in <span>directory</span>, which are
tuples. It assigns the elements of each tuple to <span>last</span> and
<span>first</span>, then prints the name and corresponding telephone
number.

There are two ways to represent tuples in a state diagram. The more
detailed version shows the indices and elements just as they appear in a
list. For example, the tuple `('Cleese', 'John')` would appear as in
Figure [fig.tuple1].

![image](figs/tuple1.pdf)

[fig.tuple1]

But in a larger diagram you might want to leave out the details. For
example, a diagram of the telephone directory might appear as in
Figure [fig.dict2].

![image](figs/dict2.pdf)

[fig.dict2]

Here the tuples are shown using Python syntax as a graphical shorthand.
The telephone number in the diagram is the complaints line for the BBC,
so please don’t call it.

Sequences of sequences
----------------------

I have focused on lists of tuples, but almost all of the examples in
this chapter also work with lists of lists, tuples of tuples, and tuples
of lists. To avoid enumerating the possible combinations, it is
sometimes easier to talk about sequences of sequences.

In many contexts, the different kinds of sequences (strings, lists and
tuples) can be used interchangeably. So how should you choose one over
the others?

To start with the obvious, strings are more limited than other sequences
because the elements have to be characters. They are also immutable. If
you need the ability to change the characters in a string (as opposed to
creating a new string), you might want to use a list of characters
instead.

Lists are more common than tuples, mostly because they are mutable. But
there are a few cases where you might prefer tuples:

1.  In some contexts, like a <span>return</span> statement, it is
    syntactically simpler to create a tuple than a list.

2.  If you want to use a sequence as a dictionary key, you have to use
    an immutable type like a tuple or string.

3.  If you are passing a sequence as an argument to a function, using
    tuples reduces the potential for unexpected behavior due to
    aliasing.

Because tuples are immutable, they don’t provide methods like
<span>sort</span> and <span>reverse</span>, which modify existing lists.
But Python provides the built-in function <span>sorted</span>, which
takes any sequence and returns a new list with the same elements in
sorted order, and <span>reversed</span>, which takes a sequence and
returns an iterator that traverses the list in reverse order.

Debugging {#debugging-8}
---------

Lists, dictionaries and tuples are examples of <span>**data
structures**</span>; in this chapter we are starting to see compound
data structures, like lists of tuples, or dictionaries that contain
tuples as keys and lists as values. Compound data structures are useful,
but they are prone to what I call <span>**shape errors**</span>; that
is, errors caused when a data structure has the wrong type, size, or
structure. For example, if you are expecting a list with one integer and
I give you a plain old integer (not in a list), it won’t work.

To help debug these kinds of errors, I have written a module called
<span>structshape</span> that provides a function, also called
<span>structshape</span>, that takes any kind of data structure as an
argument and returns a string that summarizes its shape. You can
download it from <http://thinkpython2.com/code/structshape.py>

Here’s the result for a simple list:

    >>> from structshape import structshape
    >>> t = [1, 2, 3]
    >>> structshape(t)
    'list of 3 int'

A fancier program might write “list of 3 int<span>*s*</span>”, but it
was easier not to deal with plurals. Here’s a list of lists:

    >>> t2 = [[1,2], [3,4], [5,6]]
    >>> structshape(t2)
    'list of 3 list of 2 int'

If the elements of the list are not the same type,
<span>structshape</span> groups them, in order, by type:

    >>> t3 = [1, 2, 3, 4.0, '5', '6', [7], [8], 9]
    >>> structshape(t3)
    'list of (3 int, float, 2 str, 2 list of int, int)'

Here’s a list of tuples:

    >>> s = 'abc'
    >>> lt = list(zip(t, s))
    >>> structshape(lt)
    'list of 3 tuple of (int, str)'

And here’s a dictionary with 3 items that map integers to strings.

    >>> d = dict(lt) 
    >>> structshape(d)
    'dict of 3 int->str'

If you are having trouble keeping track of your data structures,
<span>structshape</span> can help.

Glossary
--------

tuple:
:   An immutable sequence of elements.

tuple assignment:
:   An assignment with a sequence on the right side and a tuple of
    variables on the left. The right side is evaluated and then its
    elements are assigned to the variables on the left.

gather:
:   An operation that collects multiple arguments into a tuple.

scatter:
:   An operation that makes a sequence behave like multiple arguments.

zip object:
:   The result of calling a built-in function <span>zip</span>; an
    object that iterates through a sequence of tuples.

iterator:
:   An object that can iterate through a sequence, but which does not
    provide list operators and methods.

data structure:
:   A collection of related values, often organized in lists,
    dictionaries, tuples, etc.

shape error:
:   An error caused because a value has the wrong shape; that is, the
    wrong type or size.

Exercises
---------

Write a function called `most_frequent` that takes a string and prints
the letters in decreasing order of frequency. Find text samples from
several different languages and see how letter frequency varies between
languages. Compare your results with the tables at
<http://en.wikipedia.org/wiki/Letter_frequencies>. Solution:
<http://thinkpython2.com/code/most_frequent.py>.

[anagrams]

More anagrams!

1.  Write a program that reads a word list from a file (see
    Section [wordlist]) and prints all the sets of words that are
    anagrams.

    Here is an example of what the output might look like:

        ['deltas', 'desalt', 'lasted', 'salted', 'slated', 'staled']
        ['retainers', 'ternaries']
        ['generating', 'greatening']
        ['resmelts', 'smelters', 'termless']

    Hint: you might want to build a dictionary that maps from a
    collection of letters to a list of words that can be spelled with
    those letters. The question is, how can you represent the collection
    of letters in a way that can be used as a key?

2.  Modify the previous program so that it prints the longest list of
    anagrams first, followed by the second longest, and so on.

3.  In Scrabble a “bingo” is when you play all seven tiles in your rack,
    along with a letter on the board, to form an eight-letter word. What
    collection of 8 letters forms the most possible bingos?

    Solution: <http://thinkpython2.com/code/anagram_sets.py>.

Two words form a “metathesis pair” if you can transform one into the
other by swapping two letters; for example, “converse” and “conserve”.
Write a program that finds all of the metathesis pairs in the
dictionary. Hint: don’t test all pairs of words, and don’t test all
possible swaps. Solution: <http://thinkpython2.com/code/metathesis.py>.
Credit: This exercise is inspired by an example at
<http://puzzlers.org>.

Here’s another Car Talk Puzzler
(<http://www.cartalk.com/content/puzzlers>):

> What is the longest English word, that remains a valid English word,
> as you remove its letters one at a time?
>
> Now, letters can be removed from either end, or the middle, but you
> can’t rearrange any of the letters. Every time you drop a letter, you
> wind up with another English word. If you do that, you’re eventually
> going to wind up with one letter and that too is going to be an
> English word—one that’s found in the dictionary. I want to know what’s
> the longest word and how many letters does it have?
>
> I’m going to give you a little modest example: Sprite. Ok? You start
> off with sprite, you take a letter off, one from the interior of the
> word, take the r away, and we’re left with the word spite, then we
> take the e off the end, we’re left with spit, we take the s off, we’re
> left with pit, it, and I.

Write a program to find all words that can be reduced in this way, and
then find the longest one.

This exercise is a little more challenging than most, so here are some
suggestions:

1.  You might want to write a function that takes a word and computes a
    list of all the words that can be formed by removing one letter.
    These are the “children” of the word.

2.  Recursively, a word is reducible if any of its children are
    reducible. As a base case, you can consider the empty string
    reducible.

3.  The wordlist I provided, <span>words.txt</span>, doesn’t contain
    single letter words. So you might want to add “I”, “a”, and the
    empty string.

4.  To improve the performance of your program, you might want to
    memoize the words that are known to be reducible.

Solution: <http://thinkpython2.com/code/reducible.py>.
