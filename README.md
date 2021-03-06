eventus
=======
A simple yet powerful event system.

Requirements
------------
* C++11

Using eventus
-------------
Eventus is a header-only library, so the simplest way to include it is to add
`eventus.hpp` to your project.

Here is a simple example of using the API:
```c++
struct point {
    int x;
    int y;
}

auto eq = eventus::event_queue<std::string>();

eq.add_handler<point>("moved", [](point p) {
    std::cout << "x: " << p.x << ", y: " << p.y << std::endl;
});

point new_location { 3, 6 };

eq.fire("moved", new_location);
```
*Prints:* `x: 3, y: 6`

`auto eq = eventus::event_queue<std::string>()`: Creates an `event_queue`
instance which uses strings to name events.  Almost any type can be used here,
such as c-style strings, `int`s, and unscoped enums.

`eq.add_handler<point>("moved", /*lambda*/)`: Adds an event listener (the
lambda or other function) to the "moved" event.

`eq.fire("moved", new_location)`: Fires the "moved" event, passing on the
point-type `new_location` to any listening event handlers.

Other Useful Info
-----------------
* Event handlers cannot be removed once added.  There are challenges in
implementing this well while maintaining the dead-simple interface.

* Nullary events and handlers use the non-templated `add_handler` function
and the unary `fire` function.

* Eventus uses runtime type information (RTTI).  Mismatched types between
firing an event and handling an event throw `std::bad_cast`.

Known Bugs
----------
* scoped enums (enum class) cannot be used as an event type

