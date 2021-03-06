---
title: Template class (part 2)
layout: default
---

## Templated class
Let's start with something simple 

consider the class 
{% highlight cpp %}

class point{
public:
double x;
double y;
void print ()
{
  cout <<" x "<<x<<" y "<<y<<endl;
}
};


int main ()
{
  point a;
  a.x=10.7;
  a.y=20.0;
  
  a.print();
return 0;
}
{% endhighlight %}

simple template class 
{% highlight cpp %}
template <typename T>
class point{
public:
T x;
T y;
void print ()
{
  cout <<" x "<<x<<" y "<<y<<endl;
}
};


int main ()
{
  point<int> a;
  a.x=10;
  a.y=20;
  
  a.print();
return 0;
}
{% endhighlight %}

Let's try something a little bit more complicated.

Consider the template class LinkedList:

{% highlight cpp %}
template <typename T> // T can be anything, e.g. foo
class LinkedList
{
    private:
    struct node
    {
        T value; // not double, T!
        node *pnext;
    };

    node *head;

    public:
    LinkedList();
    void insert_front(T value); // T!
    ...
    T nth(int n); // T!
    ...
};
...
{% endhighlight %}

We defined linked lists of different types as follows:

{% highlight cpp %}
    LinkedList<string> list1;     // Linked list of strings
    LinkedList<int> list2;        // Linked list of integers
    LinkedList<double> list3;     // Linked list of doubles
    LinkedList<bool> list4;       // Linked list of boolean values
{% endhighlight %}

We can also use a class type to define a linked list of more complex structures.
For instance, a point is defined by its x and y coordinates.
We can create a linked list of points as follows:

{% highlight cpp %}
class Point
{
  double x;
  double y;
};
...

  LinkedList<Point> point_list;    // Linked list of points
{% endhighlight %}

Note that the member functions receive and return variables
of type Point:
{% highlight cpp %}
template <typename T>
class LinkedList
{
    ...
    public:
    ...
    void insert_front(T value); // T!
    ...
    T nth(int n); // T!
    ...
};
...

  LinkedList<Point> point_list;
  Point p, q;

  for (int i = 0; i < 10; i++)
  {
    p.x = i;
    p.y = i*i;
    point_list.insert_front(p);
  }

  q = point_list.nth(2);
...
{% endhighlight %}

We have defined Point with coordinates of type double,
but maybe we also want points with coordinates of type int or float.
Define Point as a template to give this flexibility:

{% highlight cpp %}
template <typename COORD_TYPE>
class Point 
{
  COORD_TYPE x;
  COORD_TYPE y;  
}
...

  Point<double> p;      // Point whose coordinates have type double
  Point<int> q;         // Point whose coordinates have type int
  Point<float> r;       // Point whose coorindates have type r
{% endhighlight %}

We can use the template class Point to define different types
of linked lists of points:
{% highlight cpp %}
template <typename COORD_TYPE>
class Point 
{
  COORD_TYPE x;
  COORD_TYPE y;  
}
...
// Linked list of points whose coordinates have type double
  LinkedList<Point<double> > plist1;    // Note space between '>' and '>'

// Linked list of points whose coordinates have type int
  LinkedList<Point<int> > plist2;  

// Linked list of points whose coordinates have type float
  LinkedList<Point<int> > plist3;  
{% endhighlight %}

Note that there MUST be a space between '>' and '>'.  
A common error is to forget this space.
Unfortunately, the symbol '>>' has a separate meaning in C++.
If you forget the space, the compiler will interpret '>>' 
as the right shift operator
and generate a whole bunch of (unintelligible) error messages.

## Two or more template parameters

A template can have two or more type parameters.
For instance, a circle is defined by the x and y coordinates
of its center and its radius:
{% highlight cpp %}
class Circle
{
  public:
    double x;
    double y;
    double radius;
};
{% endhighlight %}

We may also want a circle whose x and y coordinates are integers
and its radius is type double.
Or perhaps we also want the x and y coordinates to have type double
and the radius to be an integer.
We can define a template class with separate types 
for the coordinates and the radius.
{% highlight cpp %}
template <typename COORD_TYPE, typename RADIUS_TYPE>
class Circle
{
  public:
    COORD_TYPE x;
    COORD_TYPE y;
    RADIUS_TYPE radius;
};
...

  Circle<double, double> c1;  // Coordinates type double, radius type double.
  Circle<int, double> c2;     // Coordinates type int, radius type double.
  Circle<double, int> c3;     // Coordinates type double, radius type int.
{% endhighlight %}

When we create the circle, the k'th type in the template parameter list
is set to be the k'th type in the template argument list.

We can create a linked list of circles as follows:
{% highlight cpp %}
template <typename COORD_TYPE, typename RADIUS_TYPE>
class Circle
{
  public:
    COORD_TYPE x;
    COORD_TYPE y;
    RADIUS_TYPE radius;
};
...

  // Linked list of circles: coordinates type int, radius type double.
  LinkedList<Circle<int, double> > circle_list1;

  // Linked list of circles: coordinates type double, radius type int.
  LinkedList<Circle<double, int> > circle_list2;
{% endhighlight %}

We can also use the Point class in defining the coordinates 
of the center of the circle:
{% highlight cpp %}
template <typename T>
class Point 
{
  T x;
  T y;  
};

template <typename COORD_TYPE, typename RADIUS_TYPE>
class CircleA
{
  public:
    Point<COORD_TYPE> p;  // Coordinates are p.x and p.y
    RADIUS_TYPE radius;
};
...

  // Circle with coordinates of type int, radius of type double
  CircleA<int, double> c;

  c.p.x = 3;          // Set x-coordinate
  c.p.y = 4;          // Set y-coordinate
  c.radius = 5.5;     // Set radius

  // Linked list of circles: coordinates type int, radius type double.
  LinkedList<CircleA<int, double> > circle_list1;

  // Linked list of circles: coordinates type double, radius type int.
  LinkedList<CircleA<double, int> > circle_list2;
{% endhighlight %}

A different way to use the class Point is as follows:
{% highlight cpp %}
template <typename T>
class Point 
{
  T x;
  T y;  
};

template <typename POINT_TYPE, typename RADIUS_TYPE>
class CircleB
{
  public:
    POINT_TYPE p;
    RADIUS_TYPE radius;
};
...

  // CircleB with coordinates of type int, radius of type double
  CircleB<Point<int>, double> c;

  c.p.x = 3;          // Set x-coordinate
  c.p.y = 4;          // Set y-coordinate
  c.radius = 5.5;     // Set radius

  // Linked list of circles: coordinates type int, radius type double.
  LinkedList<CircleB<Point<int>, double> > circle_list1;

  // Linked list of circles: coordinates type double, radius type int.
  LinkedList<CircleB<Point<double>, int> > circle_list2;
{% endhighlight %}

Template class CircleB gives more flexibility than template class CircleA.
For instance, CircleB could have different types for coordinates x and y
or could have other data stored with the point.
On the other hand, anyone writing a new class to be used as a POINT_TYPE 
must remember to have x and y fields in the class.
The flexibility of CircleB also creates the potential for more
things to go wrong.

Template classes can have more than two parameters.
For instance, in computer graphics the representation
of a circle may include an r,g,b color and an opacity.
{% highlight cpp %}
template <typename COORD_TYPE, typename RADIUS_TYPE,
          typename COLOR_TYPE, typename OPACITY_TYPE>
class Circle
{
  public:
    COORD_TYPE x;
    COORD_TYPE y;
    RADIUS_TYPE radius;
    COLOR_TYPE r,g,b;
    OPACITY_TYPE opacity;
};
...

  Circle<int, double, unsigned char, float> c;
...
{% endhighlight %}

## Templates and compiler errors

With all but the simplest templates, compiler errors can be extremely confusing.
Unless you are extremely good at template programming,
it is often better to first code a class without using templates
and then convert to a template class.

The following code generates a fairly simple compiler error:
{% highlight cpp %}
...
 6. template <typename COORD_TYPE>
 7. class Point
 8. {
 9. public:
10.  COORD_TYPE x;
11.  COORD_TYPE y;
12. };
13.
14. template <typename POINT_TYPE, typename RADIUS_TYPE>
15. class Circle
16. {
17. public:
18.   POINT_TYPE p;
19.   RADIUS_TYPE radius;
20. };
21. 
22. int main()
23. {
24.   Circle<Point,double> c1;
25.   c1.p.x = 3;
26.   c1.p.y = 4;
27.   c1.radius = 2.3;
28. 
29.   cout << "Radius: " << c1.radius << endl;
30.
31.   return 0;
32. }
{% endhighlight %}

Can you spot the error?
Compiling the function gives the following error message:
{% highlight cpp %}
> g++ circle3_error.cpp
circle3_error.cpp: In function 'int main()':
circle3_error.cpp:24: error: type/value mismatch at argument 1 
     in template parameter list for 
     'template<class POINT_TYPE, class RADIUS_TYPE> class Circle'
circle3_error.cpp:24: error:   expected a type, got 'Point'
circle3_error.cpp:24: error: invalid type in declaration before ';' token
circle3_error.cpp:26: error: request for member 'p' in 'c1', 
     which is of non-class type 'int'
circle3_error.cpp:27: error: request for member 'p' in 'c1', 
     which is of non-class type 'int'
circle3_error.cpp:28: error: request for member 'radius' in 'c1', 
     which is of non-class type 'int'
circle3_error.cpp:30: error: request for member 'radius' in 'c1', 
     which is of non-class type 'int'
{% endhighlight %}

Can you determine the error after reading the error message?


