# Direct Mouse and Keyboard Input

In the previous chapter, we discussed events related to pressing the WASD keys and the arrow keys. Unfortunately, when a key is held down, the event is triggered only once. In some situations, such as character movement, we would like the character to continue moving for as long as the key remains pressed.

To achieve this, we can directly check whether a specific key is currently pressed. It is important to remember that an event is not the same as direct input. Events inform the program that a specific action has occurred, such as pressing or releasing a key. Direct input, on the other hand, allows us to check the current state of the keyboard or mouse at any time.

The following code demonstrates how to check whether the W, A, S, or D key is currently being pressed.

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Hello SFML 3.0");	

    while (window.isOpen()) {
		
        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();
        }
            
        if(sf::Keyboard::isKeyPressed(sf::Keyboard::Key::W)) {
            std::cout << "Key W is pressed" << std::endl;
        }

        if(sf::Keyboard::isKeyPressed(sf::Keyboard::Key::A)) {
            std::cout << "Key A is pressed" << std::endl;
        }

        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::S)) {
            std::cout << "Key S is pressed" << std::endl;
        }

        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::D)) {
            std::cout << "Key D is pressed" << std::endl;
        }

        window.clear(sf::Color::Black);
        // something to draw here
        window.display();
    }
}
```

Just like with the keyboard, SFML allows you to directly check the state of mouse buttons. This makes it possible to determine at any moment whether the left, right, or middle mouse button is currently pressed, without having to wait for an event. This is particularly useful when creating editors, strategy games, or applications that require dragging objects with the mouse. Additionally, we can read the current position of the cursor relative to the application window, making it possible to determine where the user is pointing. The following example displays information about the currently pressed mouse button, and when no button is pressed, it prints the cursor position relative to the window.

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Hello SFML 3.0");

    while (window.isOpen()) {

        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();
        }

        if (sf::Mouse::isButtonPressed(sf::Mouse::Button::Left)) {
            std::cout << "Lewy Mouse Button is Pressed" << std::endl;
        }
        else if (sf::Mouse::isButtonPressed(sf::Mouse::Button::Right)) {
            std::cout << "Right Mouse Button is Pressed" << std::endl;
        }
        else if (sf::Mouse::isButtonPressed(sf::Mouse::Button::Middle)) {
            std::cout << "Middle Mouse Button is Pressed" << std::endl;
        }
        else {
            sf::Vector2i mousePosition = sf::Mouse::getPosition(window);
            std::cout << "Cursor position: (" << mousePosition.x << ", " << mousePosition.y << ")" << std::endl;
        }


        window.clear(sf::Color::Black);
        // something to draw
        window.display();
    }
}
```

