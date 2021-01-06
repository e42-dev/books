<link rel="stylesheet" type="text/css" media="all" href="./assets/css/book.css" />

# Case study: interface design

This chapter presents a case study that demonstrates a process for
designing functions that work together.

It introduces the <span>turtle</span> module, which allows you to create
images using turtle graphics. The <span>turtle</span> module is included
in most Python installations, but if you are running Python using
PythonAnywhere, you won’t be able to run the turtle examples (at least
you couldn’t when I wrote this).

If you have already installed Python on your computer, you should be
able to run the examples. Otherwise, now is a good time to install. I
have posted instructions at <http://tinyurl.com/thinkpython2e>.

Code examples from this chapter are available from
<http://thinkpython2.com/code/polygon.py>.

## 4.1 The turtle module

To check whether you have the <span>turtle</span> module, open the
Python interpreter and type

    >>> import turtle
    >>> bob = turtle.Turtle()

When you run this code, it should create a new window with small arrow
that represents the turtle. Close the window.

Create a file named <span>mypolygon.py</span> and type in the following
code:

    import turtle
    bob = turtle.Turtle()
    print(bob)
    turtle.mainloop()

The <span>turtle</span> module (with a lowercase ’t’) provides a
function called <span>Turtle</span> (with an uppercase ’T’) that creates
a Turtle object, which we assign to a variable named <span>bob</span>.
Printing <span>bob</span> displays something like:

    <turtle.Turtle object at 0xb7bfbf4c>

This means that <span>bob</span> refers to an object with type
<span>Turtle</span> as defined in module <span>turtle</span>.

`mainloop` tells the window to wait for the user to do something,
although in this case there’s not much for the user to do except close
the window.

Once you create a Turtle, you can call a <span>**method**</span> to move
it around the window. A method is similar to a function, but it uses
slightly different syntax. For example, to move the turtle forward:

    bob.fd(100)

The method, <span>fd</span>, is associated with the turtle object we’re
calling <span>bob</span>. Calling a method is like making a request: you
are asking <span>bob</span> to move forward.

The argument of <span>fd</span> is a distance in pixels, so the actual
size depends on your display.

Other methods you can call on a Turtle are <span>bk</span> to move
backward, <span>lt</span> for left turn, and <span>rt</span> right turn.
The argument for <span>lt</span> and <span>rt</span> is an angle in
degrees.

Also, each Turtle is holding a pen, which is either down or up; if the
pen is down, the Turtle leaves a trail when it moves. The methods
<span>pu</span> and <span>pd</span> stand for “pen up” and “pen down”.

To draw a right angle, add these lines to the program (after creating
<span>bob</span> and before calling `mainloop`):

    bob.fd(100)
    bob.lt(90)
    bob.fd(100)

When you run this program, you should see <span>bob</span> move east and
then north, leaving two line segments behind.

Now modify the program to draw a square. Don’t go on until you’ve got it
working!

## 4.2 Simple repetition

Chances are you wrote something like this:

    bob.fd(100)
    bob.lt(90)

    bob.fd(100)
    bob.lt(90)

    bob.fd(100)
    bob.lt(90)

    bob.fd(100)

We can do the same thing more concisely with a <span>for</span>
statement. Add this example to <span>mypolygon.py</span> and run it
again:

    for i in range(4):
        print('Hello!')

You should see something like this:

    Hello!
    Hello!
    Hello!
    Hello!

This is the simplest use of the <span>for</span> statement; we will see
more later. But that should be enough to let you rewrite your
square-drawing program. Don’t go on until you do.

Here is a <span>for</span> statement that draws a square:

    for i in range(4):
        bob.fd(100)
        bob.lt(90)

The syntax of a <span>for</span> statement is similar to a function
definition. It has a header that ends with a colon and an indented body.
The body can contain any number of statements.

A <span>for</span> statement is also called a <span>**loop**</span>
because the flow of execution runs through the body and then loops back
to the top. In this case, it runs the body four times.

This version is actually a little different from the previous
square-drawing code because it makes another turn after drawing the last
side of the square. The extra turn takes more time, but it simplifies
the code if we do the same thing every time through the loop. This
version also has the effect of leaving the turtle back in the starting
position, facing in the starting direction.

## 4.3 Exercises

The following is a series of exercises using the <span>turtle</span>
module. They are meant to be fun, but they have a point, too. While you
are working on them, think about what the point is.

The following sections have solutions to the exercises, so don’t look
until you have finished (or at least tried).

1.  Write a function called <span>square</span> that takes a parameter
    named <span>t</span>, which is a turtle. It should use the turtle to
    draw a square.

    Write a function call that passes <span>bob</span> as an argument to
    <span>square</span>, and then run the program again.

2.  Add another parameter, named <span>length</span>, to
    <span>square</span>. Modify the body so length of the sides is
    <span>length</span>, and then modify the function call to provide a
    second argument. Run the program again. Test your program with a
    range of values for <span>length</span>.

3.  Make a copy of <span>square</span> and change the name to
    <span>polygon</span>. Add another parameter named <span>n</span> and
    modify the body so it draws an n-sided regular polygon. Hint: The
    exterior angles of an n-sided regular polygon are $360/n$ degrees.

4.  Write a function called <span>circle</span> that takes a turtle,
    <span>t</span>, and radius, <span>r</span>, as parameters and that
    draws an approximate circle by calling <span>polygon</span> with an
    appropriate length and number of sides. Test your function with a
    range of values of <span>r</span>.

    Hint: figure out the circumference of the circle and make sure that
    <span>length \* n = circumference</span>.

5.  Make a more general version of <span>circle</span> called
    <span>arc</span> that takes an additional parameter
    <span>angle</span>, which determines what fraction of a circle to
    draw. <span>angle</span> is in units of degrees, so when
    <span>angle=360</span>, <span>arc</span> should draw a complete
    circle.

## 4.4 Encapsulation

The first exercise asks you to put your square-drawing code into a
function definition and then call the function, passing the turtle as a
parameter. Here is a solution:

    def square(t):
        for i in range(4):
            t.fd(100)
            t.lt(90)

    square(bob)

The innermost statements, <span>fd</span> and <span>lt</span> are
indented twice to show that they are inside the <span>for</span> loop,
which is inside the function definition. The next line,
<span>square(bob)</span>, is flush with the left margin, which indicates
the end of both the <span>for</span> loop and the function definition.

Inside the function, <span>t</span> refers to the same turtle
<span>bob</span>, so <span>t.lt(90)</span> has the same effect as
<span>bob.lt(90)</span>. In that case, why not call the parameter
<span>bob</span>? The idea is that <span>t</span> can be any turtle, not
just <span>bob</span>, so you could create a second turtle and pass it
as an argument to <span>square</span>:

    alice = turtle.Turtle()
    square(alice)

Wrapping a piece of code up in a function is called
<span>**encapsulation**</span>. One of the benefits of encapsulation is
that it attaches a name to the code, which serves as a kind of
documentation. Another advantage is that if you re-use the code, it is
more concise to call a function twice than to copy and paste the body!

## 4.5 Generalization

The next step is to add a <span>length</span> parameter to
<span>square</span>. Here is a solution:

    def square(t, length):
        for i in range(4):
            t.fd(length)
            t.lt(90)

    square(bob, 100)

Adding a parameter to a function is called
<span>**generalization**</span> because it makes the function more
general: in the previous version, the square is always the same size; in
this version it can be any size.

The next step is also a generalization. Instead of drawing squares,
<span>polygon</span> draws regular polygons with any number of sides.
Here is a solution:

    def polygon(t, n, length):
        angle = 360 / n
        for i in range(n):
            t.fd(length)
            t.lt(angle)

    polygon(bob, 7, 70)

This example draws a 7-sided polygon with side length 70.

If you are using Python 2, the value of <span>angle</span> might be off
because of integer division. A simple solution is to compute <span>angle
= 360.0 / n</span>. Because the numerator is a floating-point number,
the result is floating point.

When a function has more than a few numeric arguments, it is easy to
forget what they are, or what order they should be in. In that case it
is often a good idea to include the names of the parameters in the
argument list:

    polygon(bob, n=7, length=70)

These are called <span>**keyword arguments**</span> because they include
the parameter names as “keywords” (not to be confused with Python
keywords like <span>while</span> and <span>def</span>).

This syntax makes the program more readable. It is also a reminder about
how arguments and parameters work: when you call a function, the
arguments are assigned to the parameters.

## 4.6 Interface design

The next step is to write <span>circle</span>, which takes a radius,
<span>r</span>, as a parameter. Here is a simple solution that uses
<span>polygon</span> to draw a 50-sided polygon:

    import math

    def circle(t, r):
        circumference = 2 * math.pi * r
        n = 50
        length = circumference / n
        polygon(t, n, length)

The first line computes the circumference of a circle with radius
<span>r</span> using the formula $2 \pi r$. Since we use
<span>math.pi</span>, we have to import <span>math</span>. By
convention, <span>import</span> statements are usually at the beginning
of the script.

<span>n</span> is the number of line segments in our approximation of a
circle, so <span>length</span> is the length of each segment. Thus,
<span>polygon</span> draws a 50-sided polygon that approximates a circle
with radius <span>r</span>.

One limitation of this solution is that <span>n</span> is a constant,
which means that for very big circles, the line segments are too long,
and for small circles, we waste time drawing very small segments. One
solution would be to generalize the function by taking <span>n</span> as
a parameter. This would give the user (whoever calls
<span>circle</span>) more control, but the interface would be less
clean.

The <span>**interface**</span> of a function is a summary of how it is
used: what are the parameters? What does the function do? And what is
the return value? An interface is “clean” if it allows the caller to do
what they want without dealing with unnecessary details.

In this example, <span>r</span> belongs in the interface because it
specifies the circle to be drawn. <span>n</span> is less appropriate
because it pertains to the details of <span>*how*</span> the circle
should be rendered.

Rather than clutter up the interface, it is better to choose an
appropriate value of <span>n</span> depending on
<span>circumference</span>:

    def circle(t, r):
        circumference = 2 * math.pi * r
        n = int(circumference / 3) + 3
        length = circumference / n
        polygon(t, n, length)

Now the number of segments is an integer near
<span>circumference/3</span>, so the length of each segment is
approximately 3, which is small enough that the circles look good, but
big enough to be efficient, and acceptable for any size circle.

Adding 3 to <span>n</span> guarantees that the polygon has at least 3
sides.

## 4.7 Refactoring

When I wrote <span>circle</span>, I was able to re-use
<span>polygon</span> because a many-sided polygon is a good
approximation of a circle. But <span>arc</span> is not as cooperative;
we can’t use <span>polygon</span> or <span>circle</span> to draw an arc.

One alternative is to start with a copy of <span>polygon</span> and
transform it into <span>arc</span>. The result might look like this:

    def arc(t, r, angle):
        arc_length = 2 * math.pi * r * angle / 360
        n = int(arc_length / 3) + 1
        step_length = arc_length / n
        step_angle = angle / n
        
        for i in range(n):
            t.fd(step_length)
            t.lt(step_angle)

The second half of this function looks like <span>polygon</span>, but we
can’t re-use <span>polygon</span> without changing the interface. We
could generalize <span>polygon</span> to take an angle as a third
argument, but then <span>polygon</span> would no longer be an
appropriate name! Instead, let’s call the more general function
<span>polyline</span>:

    def polyline(t, n, length, angle):
        for i in range(n):
            t.fd(length)
            t.lt(angle)

Now we can rewrite <span>polygon</span> and <span>arc</span> to use
<span>polyline</span>:

    def polygon(t, n, length):
        angle = 360.0 / n
        polyline(t, n, length, angle)

    def arc(t, r, angle):
        arc_length = 2 * math.pi * r * angle / 360
        n = int(arc_length / 3) + 1
        step_length = arc_length / n
        step_angle = float(angle) / n
        polyline(t, n, step_length, step_angle)

Finally, we can rewrite <span>circle</span> to use <span>arc</span>:

    def circle(t, r):
        arc(t, r, 360)

This process—rearranging a program to improve interfaces and facilitate
code re-use—is called <span>**refactoring**</span>. In this case, we
noticed that there was similar code in <span>arc</span> and
<span>polygon</span>, so we “factored it out” into
<span>polyline</span>.

If we had planned ahead, we might have written <span>polyline</span>
first and avoided refactoring, but often you don’t know enough at the
beginning of a project to design all the interfaces. Once you start
coding, you understand the problem better. Sometimes refactoring is a
sign that you have learned something.

## 4.8 A development plan

A <span>**development plan**</span> is a process for writing programs.
The process we used in this case study is “encapsulation and
generalization”. The steps of this process are:

1.  Start by writing a small program with no function definitions.

2.  Once you get the program working, identify a coherent piece of it,
    encapsulate the piece in a function and give it a name.

3.  Generalize the function by adding appropriate parameters.

4.  Repeat steps 1–3 until you have a set of working functions. Copy and
    paste working code to avoid retyping (and re-debugging).

5.  Look for opportunities to improve the program by refactoring. For
    example, if you have similar code in several places, consider
    factoring it into an appropriately general function.

This process has some drawbacks—we will see alternatives later—but it
can be useful if you don’t know ahead of time how to divide the program
into functions. This approach lets you design as you go along.

## 4.9 docstring

A <span>**docstring**</span> is a string at the beginning of a function
that explains the interface (“doc” is short for “documentation”). Here
is an example:

    def polyline(t, n, length, angle):
        """Draws n line segments with the given length and
        angle (in degrees) between them.  t is a turtle.
        """    
        for i in range(n):
            t.fd(length)
            t.lt(angle)

By convention, all docstrings are triple-quoted strings, also known as
multiline strings because the triple quotes allow the string to span
more than one line.

It is terse, but it contains the essential information someone would
need to use this function. It explains concisely what the function does
(without getting into the details of how it does it). It explains what
effect each parameter has on the behavior of the function and what type
each parameter should be (if it is not obvious).

Writing this kind of documentation is an important part of interface
design. A well-designed interface should be simple to explain; if you
have a hard time explaining one of your functions, maybe the interface
could be improved.

## 4.10 Debugging

An interface is like a contract between a function and a caller. The
caller agrees to provide certain parameters and the function agrees to
do certain work.

For example, <span>polyline</span> requires four arguments:
<span>t</span> has to be a Turtle; <span>n</span> has to be an integer;
<span>length</span> should be a positive number; and <span>angle</span>
has to be a number, which is understood to be in degrees.

These requirements are called <span>**preconditions**</span> because
they are supposed to be true before the function starts executing.
Conversely, conditions at the end of the function are
<span>**postconditions**</span>. Postconditions include the intended
effect of the function (like drawing line segments) and any side effects
(like moving the Turtle or making other changes).

Preconditions are the responsibility of the caller. If the caller
violates a (properly documented!) precondition and the function doesn’t
work correctly, the bug is in the caller, not the function.

If the preconditions are satisfied and the postconditions are not, the
bug is in the function. If your pre- and postconditions are clear, they
can help with debugging.

## 4.11 Glossary

method:
:   A function that is associated with an object and called using dot
    notation.

loop:
:   A part of a program that can run repeatedly.

encapsulation:
:   The process of transforming a sequence of statements into a function
    definition.

generalization:
:   The process of replacing something unnecessarily specific (like a
    number) with something appropriately general (like a variable or
    parameter).

keyword argument:
:   An argument that includes the name of the parameter as a “keyword”.

interface:
:   A description of how to use a function, including the name and
    descriptions of the arguments and return value.

refactoring:
:   The process of modifying a working program to improve function
    interfaces and other qualities of the code.

development plan:
:   A process for writing programs.

docstring:
:   A string that appears at the top of a function definition to
    document the function’s interface.

precondition:
:   A requirement that should be satisfied by the caller before a
    function starts.

postcondition:
:   A requirement that should be satisfied by the function before it
    ends.

## 4.12 Exercises

Download the code in this chapter from
<http://thinkpython2.com/code/polygon.py>.

1.  Draw a stack diagram that shows the state of the program while
    executing <span>circle(bob, radius)</span>. You can do the
    arithmetic by hand or add <span>print</span> statements to the code.

2.  The version of <span>arc</span> in Section [refactoring] is not very
    accurate because the linear approximation of the circle is always
    outside the true circle. As a result, the Turtle ends up a few
    pixels away from the correct destination. My solution shows a way to
    reduce the effect of this error. Read the code and see if it makes
    sense to you. If you draw a diagram, you might see how it works.

![image](figs/flowers.pdf)

[fig.flowers]

Write an appropriately general set of functions that can draw flowers as
in Figure [fig.flowers].

Solution: <http://thinkpython2.com/code/flower.py>, also requires
<http://thinkpython2.com/code/polygon.py>.

![image](figs/pies.pdf)

[fig.pies]

Write an appropriately general set of functions that can draw shapes as
in Figure [fig.pies].

Solution: <http://thinkpython2.com/code/pie.py>.

The letters of the alphabet can be constructed from a moderate number of
basic elements, like vertical and horizontal lines and a few curves.
Design an alphabet that can be drawn with a minimal number of basic
elements and then write functions that draw the letters.

You should write one function for each letter, with names `draw_a`,
`draw_b`, etc., and put your functions in a file named
<span>letters.py</span>. You can download a “turtle typewriter” from
<http://thinkpython2.com/code/typewriter.py> to help you test your code.

You can get a solution from <http://thinkpython2.com/code/letters.py>;
it also requires <http://thinkpython2.com/code/polygon.py>.

Read about spirals at <http://en.wikipedia.org/wiki/Spiral>; then write
a program that draws an Archimedian spiral (or one of the other kinds).
Solution: <http://thinkpython2.com/code/spiral.py>.