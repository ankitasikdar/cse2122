---
title: Splitting code into several files
layout: default
---

You have probably been noticing that your source code files have grown somewhat
large and include a diverse range of features: class definitions, class method
implementations, other functions, and a `main()` function. In future
assignments you will define several classes, some of which may have nothing to
do with each other. At some point, an organization scheme will be needed to
reduce complexity.

C++ offers a very simplistic technique for splitting code into files (sometimes
called "modules" or "libraries" in other languages). The idea is simply to
write code in several files, then "include" them all into the same file, giving
the *appearance* (to the compiler) that all the code was written in one file.

We have been doing this for some time, using `#include <xyz>`. Except for one
minor change, this is the same technique we'll use to split our programs into
multiple files.

**A word of advice: Do not write your code in one file, then try to
split it into several files later; students often try this and find it
frustrating or impossible. Begin your project by creating several
files...**

## Understanding `#include`

First, it's important to see how simple `#include` really is. When the compiler
is reading a file (e.g. `myprogram.cpp`) and it sees `#include <xyz>` (which
may appear anywhere, though it must be found on a line all by itself), the
compiler literally reads the file `xyz` (wherever that file is on your
computer) and pastes its contents exactly where `#include <xyz>` appeared. So
it's a simple substitution: substitute `#include <xyz>` with the actual contents
of the file `xyz`.

The form of `#include` we will be using for our own files is `#include "abc.h"`
where `abc.h` is a file we created. The format `#include <abc.h>` (with angle
brackets) means the file `abc.h` will be found in some system directory, known
to the compiler. The quoted format means the file will be found somewhere
locally, probably in the same directory as our program code `myprogram.cpp`.

## Common practice for splitting code

Since `#include` does a simple substitution, we *could* (but won't) split a
large program `myprogram.cpp` into two smaller files `myprogramA.cpp` and
`myprogramB.cpp`, with `myprogramB.cpp` including `myprogramA.cpp` by placing
the statement `#include "myprogramA.cpp"` at the top of the file
`myprogramB.cpp`. I don't see anything particularly bad about this approach,
but it is virtually unseen in real C++ programming.

Instead, we will include only "header files" into our "source files." Header
files usually have the ending `.h` and source files usually have the ending
`.cpp`.  A header file contains only class and function declarations (a
declaration is a statement that a class exists and has certain properties and
methods, or that a function exists; neither the class methods nor the functions
will be defined; that is, their implementations will not be provided in the
header file).

Some included files may include other files, and may attempt to include files
*already included*; we need to prevent repeated-includes because the compiler
is not happy when a class or function or variable of the same name is declared
twice. Thus, in every header file (named `blah.h` in this example), we write
the following at the top and bottom:

{% highlight cpp %}
#ifndef BLAH_H
#define BLAH_H

...

#endif
{% endhighlight %}

This means "if the name `BLAH_H` is not already defined, then define it." If a
file is included twice, then `BLAH_H` *will* be defined (by the first
inclusion) so the entire "if--endif" will be skipped (which is the whole file,
because the whole file is between the "if" and "endif"). Of course, `BLAH_H`
can be anything; it could be `FOO`; we usually write `FILE_H` for the file
named `file.h` so that we don't reuse names.


## Example

Here is the Shape class and subclasses split into multiple files, plus a main
file.

`shape.h`:

{% highlight cpp %}
#ifndef SHAPE_H
#define SHAPE_H

class Shape
{
    public:
    double x;
    double y;

    virtual double area() = 0;
};

#endif
{% endhighlight %}

`rectangle.h`:

{% highlight cpp %}
#ifndef RECTANGLE_H
#define RECTANGLE_H

#include "shape.h"

class Rectangle : public Shape
{
    public:
    double width;
    double height;

    double area();
};

#endif
{% endhighlight %}

`rectangle.cpp`:

{% highlight cpp %}
#include "rectangle.h"

double Rectangle::area()
{
    return width * height;
}
{% endhighlight %}

`ellipse.h`:

{% highlight cpp %}
#ifndef ELLIPSE_H
#define ELLIPSE_H

#include "shape.h"

class Ellipse : public Shape
{
    public:
    double major_axis;
    double minor_axis;
 
    double area();
};

#endif
{% endhighlight %}

`ellipse.cpp`:

{% highlight cpp %}
#include "ellipse.h"
 
double Ellipse::area()
{
    return 3.1415926 * major_axis * minor_axis;
}
{% endhighlight %}

`main.cpp`:

{% highlight cpp %}
#include <iostream>
#include "rectangle.h"
#include "ellipse.h"
using namespace std;

int main()
{
    Rectangle r;
    r.width = 10;
    r.height = 15;
    r.x = 3;
    r.y = 2;

    cout << r.area() << endl;

    Ellipse e;
    e.major_axis = 3;
    e.minor_axis = 5;
    e.x = 14;
    e.y = 68;

    cout << e.area() << endl;

    return 0;
}
{% endhighlight %}

## Compiling a project that has multiple files

The `.h` files don't need to be compiled; they will be included by the `.cpp`
files. But the `.cpp` files do need to be compiled, each separately, and then
"linked" together into a grand final program.

How this is done depends on which tools you are using. If you are on the
"command line" and using g++, you can do this:

<pre>
g++ -c rectangle.cpp
g++ -c ellipse.cpp
g++ -c main.cpp
g++ -o myprogram rectangle.o ellipse.o main.o
</pre>

The first three lines compile each `.cpp` file separately (producing a
corresponding `.o` file). The fourth line links all the `.o` files together to
create the final program.
## Using CMAKE

Compiling using<a href="https://dl.dropbox.com/u/2989703/example.zip">Cmake</a>

* Unzip contents into a folder.
* Go to the folder in your terminal.
* cmake . 
* make 
* run the program by using ./shape

Here is a sample of what it looks in my terminal 

arindam:Week-7\:Wed Feb 27: cmake .
-- The C compiler identification is GNU
-- The CXX compiler identification is GNU
-- Check for working C compiler: /usr/bin/gcc
-- Check for working C compiler: /usr/bin/gcc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/arindam/Dropbox/cse2122/src/Week-7


arindam:Week-7\:Wed Feb 27: make
Scanning dependencies of target shape
[ 33%] Building CXX object CMakeFiles/shape.dir/main.cpp.o
[ 66%] Building CXX object CMakeFiles/shape.dir/shape.cpp.o
[100%] Building CXX object CMakeFiles/shape.dir/Rectangle.cpp.o
Linking CXX executable shape
[100%] Built target shape


arindam:Week-7\:Wed Feb 27: ./shape 
 class example 
****************************************
Calling the SHAPE constructor. Setting x to 10 and y to 15.
6
5,4
****************************************
Calling the SHAPE constructor. Setting x to 10 and y to 15.
This is a RECTANGLE  default constructor
r.x 9.3
r.y 2.3
****************************************
Calling the SHAPE constructor. Setting x to 10 and y to 15.
This is a RECTANGLE  default constructor
r.x 10
r.y 15
****************************************
Calling the SHAPE constructor. Setting x to 10 and y to 15.
This is a RECTANGLE  default constructor
2.07495e-317
6.95326e-310
prints garbage because the constructor does not initialize anything
arindam:Week-7\:Wed Feb 27: 

## Visual Studio

<iframe src="http://player.vimeo.com/video/24224470?title=0&byline=0&portrait=0" width="600" height="450" frameborder="0"></iframe>

## CodeBlocks

<iframe src="http://player.vimeo.com/video/24224759?title=0&byline=0&portrait=0" width="600" height="450" frameborder="0"></iframe>


