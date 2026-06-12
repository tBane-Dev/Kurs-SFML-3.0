# Font and Texts


To draw text in SFML 3.0, we need a font and an object of type ``sf::Text``.

A font defines the appearance of characters such as letters, digits, and symbols. It is the font that allows text to be displayed on the screen using a specific typeface.

In SFML, fonts are represented by the ``sf::Font`` class. Before a font can be used, it must be loaded from a file using the ``openFromFile()`` function. Once the font has been successfully loaded, we can create an ``sf::Text`` object by providing the font, the text string, and the character size.

The ``sf::Text`` object provides many options for customizing the appearance of text. We can change its color, position, rotation, scale, outline thickness, and outline color. This makes it easy to adjust the appearance of text to fit the needs of our application or game.

The following example demonstrates how to load a font from a Windows system file, create a text object, and center it within the application window. The text is then drawn on a black background.

The ``getLocalBounds()`` function of the ``sf::Text`` class returns an object of type ``sf::FloatRect``, which contains information about the local dimensions of the text. Using the position and size fields, we can obtain the position as well as the width and height of the area occupied by the text. In the example presented above, we use the width of the text to calculate its position, allowing it to be centered within the application window.

The ``getLineSpacing(characterSize)`` function of the ``sf::Font`` class returns the height of a single line of text in pixels for the specified character size. This value includes the spacing between consecutive lines of text and is useful when arranging multiple text objects one below another. In our example, it is used to calculate the vertical position of the text when centering it within the window.

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Font and Texts");

    // create a Font object
    sf::Font font;

    // load the Font from Windows
    if (!font.openFromFile("C:\\Windows\\Fonts\\arial.ttf")) {
        std::cerr << "Failed to load font!" << std::endl;
        return 0;
    }

    unsigned int characterSize = 79u; // set the character size to 79 pixels

    // create a Text object with the font and character size
    sf::Text text(font, "Hello SFML 3.0", characterSize);
    text.setFillColor(sf::Color::Red);
    text.setOutlineThickness(6.f);
    text.setOutlineColor(sf::Color(127, 0, 0));
    text.setRotation(sf::degrees(5.f));

    // calculate the position of the text to center it in the window
    sf::Vector2u textPosition;
    textPosition.x = (window.getSize().x - (unsigned int)(text.getLocalBounds().size.x)) / 2u;
    textPosition.y = (window.getSize().y - (unsigned int)(font.getLineSpacing(characterSize))) / 2u;
    textPosition.y -= 47u;

    text.setPosition(sf::Vector2f(textPosition));

    while (window.isOpen()) {
        while (const std::optional event = window.pollEvent()) {
            if (event->is<sf::Event::Closed>())
                window.close();
        }

        window.clear(sf::Color::Black); // clear screen and fill it with black color
        window.draw(text);              // draw text
        window.display();               // display the contents of the window
    }

    return 0;
}
```

![Font and Text](program.png)