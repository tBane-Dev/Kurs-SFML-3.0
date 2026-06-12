# Primitives

SFML provides several basic shapes that can be rendered on the screen. These include rectangles, circles, and custom convex shapes.

In this chapter, we will learn how to create and draw these objects.

## Rectangle (`sf::RectangleShape`)

The first shape is a rectangle represented by the `sf::RectangleShape` class.

The following program creates a rectangle with a size of 400×300 pixels positioned at (200, 150). The rectangle is filled with a red color and has a dark red outline with a thickness of 10 pixels.

Pay attention to the notation:

```cpp
sf::Color(127, 0, 0)
```

In this case, the color is created by specifying the following components:
* Red
* Green
* Blue

Each value can range from 0 to 255.

It is also possible to provide a fourth parameter representing transparency (Alpha), but we will not use it for now.

```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "RectangleShape");

    sf::RectangleShape rect(sf::Vector2f(400.f, 300.f));
    rect.setFillColor(sf::Color::Red);
    rect.setOutlineThickness(10.0f);
    rect.setOutlineColor(sf::Color(127, 0, 0));
    rect.setPosition(sf::Vector2f(200.f, 150.f));

    while (window.isOpen()) {

        window.clear(sf::Color::Black);
        window.draw(rect);
        window.display();
    }

    return 0;
}
```

![RectangleShape](program_rectangle.png)

## Circle (`sf::CircleShape`)

The `sf::CircleShape` class is used to draw circles. Its constructor takes the circle radius as a parameter. In our example, the radius is 100 pixels.

The circle is filled with a green color and has a dark green outline with a thickness of 10 pixels.

We also use the `setScale` function, which scales the object by a factor of 2 horizontally and 1.5 vertically. This function works for all classes derived from `sf::Transformable`, including rectangles, circles, and sprites.

The example also demonstrates the use of the `setOrigin` function. By default, the origin of every shape is located at its upper-left corner. For a circle with a radius of 100 pixels, the center is located at (100, 100), so we set the origin to that position.

As a result, all transformations such as positioning, rotation, and scaling are performed relative to the center of the circle.

Finally, we set the position of the shape to (400, 300), which corresponds to the center of an 800×600 pixel window.

It is worth noting that after applying a scale of `(2.0, 1.5)`, the shape is no longer a perfect circle and becomes an ellipse.

```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "CircleShape");

    sf::CircleShape circle(100.f);
    circle.setFillColor(sf::Color::Green);
    circle.setOutlineThickness(10.0f);
    circle.setOutlineColor(sf::Color(0, 127, 0));
    circle.setOrigin(sf::Vector2f(100.f, 100.f));
    circle.setScale(sf::Vector2f(2.f, 1.5f));
    circle.setPosition(sf::Vector2f(400.f, 300.f));

    while (window.isOpen()) {

        window.clear(sf::Color::Black);
        window.draw(circle);
        window.display();
    }

    return 0;
}
```

![CircleShape](program_circle.png)

## Convex Shape (`sf::ConvexShape`)

The last shape available in SFML 3.0 is the convex polygon (`sf::ConvexShape`). Unlike rectangles or circles, its shape is defined manually by specifying individual points.

In the following example, we create a pentagon consisting of five vertices. First, we specify the number of points using the `setPointCount` function, and then we define the position of each point using `setPoint`.

After defining the shape, we set its origin (`setOrigin`) to the center of the polygon at position (150, 150). This ensures that all transformations, such as movement, rotation, and scaling, are performed relative to the center of the shape.

Next, we assign a yellow fill color and a dark yellow outline with a thickness of 10 pixels. To demonstrate transformations, we also use the `setScale` function, which stretches the shape three times horizontally and twice vertically.

Finally, we position the polygon at (400, 250) and display it in the window.

```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "ConvexShape");

    sf::ConvexShape convex;
    convex.setPointCount(5);
    convex.setPoint(0, sf::Vector2f(150.f, 100.f));
    convex.setPoint(1, sf::Vector2f(200.f, 150.f));
    convex.setPoint(2, sf::Vector2f(175.f, 200.f));
    convex.setPoint(3, sf::Vector2f(125.f, 200.f));
    convex.setPoint(4, sf::Vector2f(100.f, 150.f));

    convex.setOrigin(sf::Vector2f(150.f, 150.f));
    convex.setFillColor(sf::Color::Yellow);
    convex.setOutlineThickness(10.0f);
    convex.setOutlineColor(sf::Color(127, 127, 0));
    convex.setScale(sf::Vector2f(3.f, 2.f));
    convex.setPosition(sf::Vector2f(400.f, 250.f));

    while (window.isOpen()) {

        window.clear(sf::Color::Black);
        window.draw(convex);
        window.display();
    }

    return 0;
}
```

After running the program, a yellow pentagon with a dark outline will be displayed on the screen. Because we applied the `setScale` function, its proportions are modified, demonstrating that polygons support the same transformations as the other shapes available in SFML.

**Note:** The points of a convex shape should be specified in the order they appear around the shape (either clockwise or counterclockwise). Providing points in a random order may result in incorrect rendering.

![ConvexShape](program_convex.png)
