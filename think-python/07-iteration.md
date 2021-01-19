<link rel="stylesheet" type="text/css" media="all" href="./assets/css/book.css" />

Iteration
=========

This chapter is about iteration, which is the ability to run a block of
statements repeatedly. We saw a kind of iteration, using recursion, in
Section [recursion]. We saw another kind, using a <span>for</span> loop,
in Section [repetition]. In this chapter we’ll see yet another kind,
using a <span>while</span> statement. But first I want to say a little
more about variable assignment.

7.1 Reassignment
------------

As you may have discovered, it is legal to make more than one assignment
to the same variable. A new assignment makes an existing variable refer
to a new value (and stop referring to the old value).

    >>> x = 5
    >>> x
    5
    >>> x = 7
    >>> x
    7

The first time we display <span>x</span>, its value is 5; the second
time, its value is 7.

Figure [fig.assign2] shows what <span>**reassignment**</span> looks like
in a state diagram.

At this point I want to address a common source of confusion. Because
Python uses the equal sign (<span>=</span>) for assignment, it is
tempting to interpret a statement like <span>a = b</span> as a
mathematical proposition of equality; that is, the claim that
<span>a</span> and <span>b</span> are equal. But this interpretation is
wrong.

First, equality is a symmetric relationship and assignment is not. For
example, in mathematics, if $a=7$ then $7=a$. But in Python, the
statement <span>a = 7</span> is legal and <span>7 = a</span> is not.

Also, in mathematics, a proposition of equality is either true or false
for all time. If $a=b$ now, then $a$ will always equal $b$. In Python,
an assignment statement can make two variables equal, but they don’t
have to stay that way:

    >>> a = 5
    >>> b = a    # a and b are now equal
    >>> a = 3    # a and b are no longer equal
    >>> b
    5

The third line changes the value of <span>a</span> but does not change
the value of <span>b</span>, so they are no longer equal.

Reassigning variables is often useful, but you should use it with
caution. If the values of variables change frequently, it can make the
code difficult to read and debug.

![image](figs/assign2.pdf)

[fig.assign2]

7.2 Updating variables
------------------

A common kind of reassignment is an <span>**update**</span>, where the
new value of the variable depends on the old.

    >>> x = x + 1

This means “get the current value of <span>x</span>, add one, and then
update <span>x</span> with the new value.”

If you try to update a variable that doesn’t exist, you get an error,
because Python evaluates the right side before it assigns a value to
<span>x</span>:

    >>> x = x + 1
    NameError: name 'x' is not defined

Before you can update a variable, you have to
<span>**initialize**</span> it, usually with a simple assignment:

    >>> x = 0
    >>> x = x + 1

Updating a variable by adding 1 is called an <span>**increment**</span>;
subtracting 1 is called a <span>**decrement**</span>.

7.3 The <span>while</span> statement
--------------------------------

Computers are often used to automate repetitive tasks. Repeating
identical or similar tasks without making errors is something that
computers do well and people do poorly. In a computer program,
repetition is also called <span>**iteration**</span>.

We have already seen two functions, <span>countdown</span> and
`print_n`, that iterate using recursion. Because iteration is so common,
Python provides language features to make it easier. One is the
<span>for</span> statement we saw in Section [repetition]. We’ll get
back to that later.

Another is the <span>while</span> statement. Here is a version of
<span>countdown</span> that uses a <span>while</span> statement:

    def countdown(n):
        while n > 0:
            print(n)
            n = n - 1
        print('Blastoff!')

You can almost read the <span>while</span> statement as if it were
English. It means, “While <span>n</span> is greater than 0, display the
value of <span>n</span> and then decrement <span>n</span>. When you get
to 0, display the word <span>Blastoff!</span>”

More formally, here is the flow of execution for a <span>while</span>
statement:

1.  Determine whether the condition is true or false.

2.  If false, exit the <span>while</span> statement and continue
    execution at the next statement.

3.  If the condition is true, run the body and then go back to step 1.

This type of flow is called a loop because the third step loops back
around to the top.

The body of the loop should change the value of one or more variables so
that the condition becomes false eventually and the loop terminates.
Otherwise the loop will repeat forever, which is called an
<span>**infinite loop**</span>. An endless source of amusement for
computer scientists is the observation that the directions on shampoo,
“Lather, rinse, repeat”, are an infinite loop.

In the case of <span>countdown</span>, we can prove that the loop
terminates: if <span>n</span> is zero or negative, the loop never runs.
Otherwise, <span>n</span> gets smaller each time through the loop, so
eventually we have to get to 0.

For some other loops, it is not so easy to tell. For example:

    def sequence(n):
        while n != 1:
            print(n)
            if n % 2 == 0:        # n is even
                n = n / 2
            else:                 # n is odd
                n = n*3 + 1

The condition for this loop is <span>n != 1</span>, so the loop will
continue until <span>n</span> is <span>1</span>, which makes the
condition false.

Each time through the loop, the program outputs the value of
<span>n</span> and then checks whether it is even or odd. If it is even,
<span>n</span> is divided by 2. If it is odd, the value of
<span>n</span> is replaced with <span>n\*3 + 1</span>. For example, if
the argument passed to <span>sequence</span> is 3, the resulting values
of <span>n</span> are 3, 10, 5, 16, 8, 4, 2, 1.

Since <span>n</span> sometimes increases and sometimes decreases, there
is no obvious proof that <span>n</span> will ever reach 1, or that the
program terminates. For some particular values of <span>n</span>, we can
prove termination. For example, if the starting value is a power of two,
<span>n</span> will be even every time through the loop until it reaches
1. The previous example ends with such a sequence, starting with 16.

The hard question is whether we can prove that this program terminates
for <span>*all*</span> positive values of <span>n</span>. So far, no one
has been able to prove it <span>*or*</span> disprove it! (See
<http://en.wikipedia.org/wiki/Collatz_conjecture>.)

As an exercise, rewrite the function `print_n` from Section [recursion]
using iteration instead of recursion.

7.4 <span>break</span>
------------------

Sometimes you don’t know it’s time to end a loop until you get half way
through the body. In that case you can use the <span>break</span>
statement to jump out of the loop.

For example, suppose you want to take input from the user until they
type <span>done</span>. You could write:

    while True:
        line = input('> ')
        if line == 'done':
            break
        print(line)

    print('Done!')

The loop condition is <span>True</span>, which is always true, so the
loop runs until it hits the break statement.

Each time through, it prompts the user with an angle bracket. If the
user types <span>done</span>, the <span>break</span> statement exits the
loop. Otherwise the program echoes whatever the user types and goes back
to the top of the loop. Here’s a sample run:

    > not done
    not done
    > done
    Done!

This way of writing <span>while</span> loops is common because you can
check the condition anywhere in the loop (not just at the top) and you
can express the stop condition affirmatively (“stop when this happens”)
rather than negatively (“keep going until that happens”).

7.5 Square roots
------------

Loops are often used in programs that compute numerical results by
starting with an approximate answer and iteratively improving it.

For example, one way of computing square roots is Newton’s method.
Suppose that you want to know the square root of $a$. If you start with
almost any estimate, $x$, you can compute a better estimate with the
following formula:

$$y = \frac{x + a/x}{2}$$ For example, if $a$ is 4 and $x$ is 3:

    >>> a = 4
    >>> x = 3
    >>> y = (x + a/x) / 2
    >>> y
    2.16666666667

The result is closer to the correct answer ($\sqrt{4} = 2$). If we
repeat the process with the new estimate, it gets even closer:

    >>> x = y
    >>> y = (x + a/x) / 2
    >>> y
    2.00641025641

After a few more updates, the estimate is almost exact:

    >>> x = y
    >>> y = (x + a/x) / 2
    >>> y
    2.00001024003
    >>> x = y
    >>> y = (x + a/x) / 2
    >>> y
    2.00000000003

In general we don’t know ahead of time how many steps it takes to get to
the right answer, but we know when we get there because the estimate
stops changing:

    >>> x = y
    >>> y = (x + a/x) / 2
    >>> y
    2.0
    >>> x = y
    >>> y = (x + a/x) / 2
    >>> y
    2.0

When <span>y == x</span>, we can stop. Here is a loop that starts with
an initial estimate, <span>x</span>, and improves it until it stops
changing:

    while True:
        print(x)
        y = (x + a/x) / 2
        if y == x:
            break
        x = y

For most values of <span>a</span> this works fine, but in general it is
dangerous to test <span>float</span> equality. Floating-point values are
only approximately right: most rational numbers, like $1/3$, and
irrational numbers, like $\sqrt{2}$, can’t be represented exactly with a
<span>float</span>.

Rather than checking whether <span>x</span> and <span>y</span> are
exactly equal, it is safer to use the built-in function <span>abs</span>
to compute the absolute value, or magnitude, of the difference between
them:

        if abs(y-x) < epsilon:
            break

Where `epsilon` has a value like <span>0.0000001</span> that determines
how close is close enough.

7.6 Algorithms
----------

Newton’s method is an example of an <span>**algorithm**</span>: it is a
mechanical process for solving a category of problems (in this case,
computing square roots).

To understand what an algorithm is, it might help to start with
something that is not an algorithm. When you learned to multiply
single-digit numbers, you probably memorized the multiplication table.
In effect, you memorized 100 specific solutions. That kind of knowledge
is not algorithmic.

But if you were “lazy”, you might have learned a few tricks. For
example, to find the product of $n$ and 9, you can write $n-1$ as the
first digit and $10-n$ as the second digit. This trick is a general
solution for multiplying any single-digit number by 9. That’s an
algorithm!

Similarly, the techniques you learned for addition with carrying,
subtraction with borrowing, and long division are all algorithms. One of
the characteristics of algorithms is that they do not require any
intelligence to carry out. They are mechanical processes where each step
follows from the last according to a simple set of rules.

Executing algorithms is boring, but designing them is interesting,
intellectually challenging, and a central part of computer science.

Some of the things that people do naturally, without difficulty or
conscious thought, are the hardest to express algorithmically.
Understanding natural language is a good example. We all do it, but so
far no one has been able to explain <span>*how*</span> we do it, at
least not in the form of an algorithm.

7.7 Debugging
---------

As you start writing bigger programs, you might find yourself spending
more time debugging. More code means more chances to make an error and
more places for bugs to hide.

One way to cut your debugging time is “debugging by bisection”. For
example, if there are 100 lines in your program and you check them one
at a time, it would take 100 steps.

Instead, try to break the problem in half. Look at the middle of the
program, or near it, for an intermediate value you can check. Add a
<span>print</span> statement (or something else that has a verifiable
effect) and run the program.

If the mid-point check is incorrect, there must be a problem in the
first half of the program. If it is correct, the problem is in the
second half.

Every time you perform a check like this, you halve the number of lines
you have to search. After six steps (which is fewer than 100), you would
be down to one or two lines of code, at least in theory.

In practice it is not always clear what the “middle of the program” is
and not always possible to check it. It doesn’t make sense to count
lines and find the exact midpoint. Instead, think about places in the
program where there might be errors and places where it is easy to put a
check. Then choose a spot where you think the chances are about the same
that the bug is before or after the check.

7.8 Vocabulary
--------

reassignment:
:   Assigning a new value to a variable that already exists.

update:
:   An assignment where the new value of the variable depends on the
    old.

initialization:
:   An assignment that gives an initial value to a variable that will be
    updated.

increment:
:   An update that increases the value of a variable (often by one).

decrement:
:   An update that decreases the value of a variable.

iteration:
:   Repeated execution of a set of statements using either a recursive
    function call or a loop.

infinite loop:
:   A loop in which the terminating condition is never satisfied.

algorithm:
:   A general process for solving a category of problems.

7.9 Exercises
---------

Copy the loop from Section [squareroot] and encapsulate it in a function
called `mysqrt` that takes <span>a</span> as a parameter, chooses a
reasonable value of <span>x</span>, and returns an estimate of the
square root of <span>a</span>.

To test it, write a function named `test_square_root` that prints a
table like this:

    a   mysqrt(a)     math.sqrt(a)  diff
    -   ---------     ------------  ----
    1.0 1.0           1.0           0.0
    2.0 1.41421356237 1.41421356237 2.22044604925e-16
    3.0 1.73205080757 1.73205080757 0.0
    4.0 2.0           2.0           0.0
    5.0 2.2360679775  2.2360679775  0.0
    6.0 2.44948974278 2.44948974278 0.0
    7.0 2.64575131106 2.64575131106 0.0
    8.0 2.82842712475 2.82842712475 4.4408920985e-16
    9.0 3.0           3.0           0.0

The first column is a number, $a$; the second column is the square root
of $a$ computed with `mysqrt`; the third column is the square root
computed by <span>math.sqrt</span>; the fourth column is the absolute
value of the difference between the two estimates.

The built-in function <span>eval</span> takes a string and evaluates it
using the Python interpreter. For example:

    >>> eval('1 + 2 * 3')
    7
    >>> import math
    >>> eval('math.sqrt(5)')
    2.2360679774997898
    >>> eval('type(math.pi)')
    <class 'float'>

Write a function called `eval_loop` that iteratively prompts the user,
takes the resulting input and evaluates it using <span>eval</span>, and
prints the result.

It should continue until the user enters `'done'`, and then return the
value of the last expression it evaluated.

The mathematician Srinivasa Ramanujan found an infinite series that can
be used to generate a numerical approximation of $1 / \pi$:

$$\frac{1}{\pi} = \frac{2\sqrt{2}}{9801} 
\sum^\infty_{k=0} \frac{(4k)!(1103+26390k)}{(k!)^4 396^{4k}}$$

Write a function called `estimate_pi` that uses this formula to compute
and return an estimate of $\pi$. It should use a <span>while</span> loop
to compute terms of the summation until the last term is smaller than
<span>1e-15</span> (which is Python notation for $10^{-15}$). You can
check the result by comparing it to <span>math.pi</span>.

Solution: <http://thinkpython2.com/code/pi.py>.