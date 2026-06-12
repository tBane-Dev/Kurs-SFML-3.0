# Events

Events are messages that inform a program that the user has performed an action or that a specific change has occurred in the application window. Examples of events include pressing a key, clicking a mouse button, or closing a window.

Below is a program that reacts to the window close event. As you may have noticed in the previous examples, the window could only be closed by stopping the program from the development environment. We now add support for the window close event, so clicking the **X** button will properly close the application.

Additionally, the operating system will recognize that the window is correctly processing messages, so the mouse cursor will no longer change to the icon indicating that the application is not responding.

```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Hello SFML 3.0");	
	
    while (window.isOpen()) {
		
        while (const std::optional event = window.pollEvent()) {
            if (event->is<sf::Event::Closed>()) 
                window.close();
        }
        
        // render
        window.clear(sf::Color::Black);
        // something to draw here
        window.display();
    }

    return 0;
}
```

In SFML, all events are stored in an event queue. The `window.pollEvent()` function retrieves events from this queue until it becomes empty. We can then check the event type and react accordingly.

Below are a few basic mouse-related events. The program prints information about mouse button presses, mouse button releases, and mouse movement.

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Hello SFML 3.0");	

    while (window.isOpen()) {
		
        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();

            if (const auto* mp = event->getIf<sf::Event::MouseButtonPressed>(); mp && mp->button == sf::Mouse::Button::Left) {
                std::cout << "Left Mouse Button Pressed at: " << mp->position.x << ", " << mp->position.y << std::endl;
            }

            if (const auto* mr = event->getIf<sf::Event::MouseButtonReleased>(); mr && mr->button == sf::Mouse::Button::Left) {
                std::cout << "Left Mouse Button Released at: " << mr->position.x << ", " << mr->position.y << std::endl;
            }

            if (const auto* mm = event->getIf<sf::Event::MouseMoved>(); mm) {
                std::cout << "Mouse Moved to: " << mm->position.x << ", " << mm->position.y << std::endl;
            }

            if (const auto* mws = event->getIf < sf::Event::MouseWheelScrolled>(); mws) {
                std::cout << "Mouse Wheel Scrolled with " << mws->delta << " value" << std::endl;
            }
        }
        
        // render
        window.clear(sf::Color::Black);
        // something to draw here
        window.display();
    }

    return 0;
}
```

Keyboard events can be handled in a similar way. SFML provides the ``sf::Event::KeyPressed`` event, which is generated whenever a key is pressed. After retrieving the event using the ``getIf<sf::Event::KeyPressed>()`` function, we can determine which key was pressed by comparing its code with the values defined in the ``sf::Keyboard::Key`` enumeration.

In the following example, the program reacts to the W, A, S, and D keys, as well as the arrow keys. When the corresponding event is detected, a message is printed to the console. In real-world applications, the std::cout statements can be replaced with any program logic, such as character movement, camera scrolling, or calling specific functions.

It is important to remember that the KeyPressed event is triggered only when a key is pressed. If we want to check whether a key is currently being held down, we need to use direct keyboard input, which will be covered in the next chapter.

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Hello SFML 3.0");	

    while (window.isOpen()) {
		
        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();

            // Klwisze WASD
            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::W) {
                std::cout << "Pressed key W" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::A) {
                std::cout << "Pressed key A" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::S) {
                std::cout << "Pressed key S" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::D) {
                std::cout << "Pressed key D" << std::endl;
            }

            // Klawisze kierunkowe
            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::Up) {
                std::cout << "Pressed key Up" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::Left) {
                std::cout << "Pressed key Left" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::Down) {
                std::cout << "Pressed key Down" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::Right) {
            std::cout << "Pressed key Right" << std::endl;
            }

        }

        // render
        window.clear(sf::Color::Black);
        // something to draw here
        window.display();
    }

    return 0;
}
```

It is also possible to detect when a key is released using the ``sf::Event::KeyReleased`` event. This is useful in situations where an action should be performed only after the key has been released, rather than when it is initially pressed.

The ``KeyPressed`` event is commonly used when implementing keyboard shortcuts such as ``Ctrl+C, Ctrl+X, and Ctrl+V``. The program can check whether the Ctrl key is being held down when the C, X, or V key is pressed, and then perform the appropriate copy, cut, or paste operation. This allows users to conveniently use the standard keyboard shortcuts found in most applications.

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Hello SFML 3.0");	

    while (window.isOpen()) {
		
        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::X && sf::Keyboard::isKeyPressed(sf::Keyboard::Key::LControl)) {
                std::cout << "Cut" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::C && sf::Keyboard::isKeyPressed(sf::Keyboard::Key::LControl)) {
                std::cout << "Copy" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::V && sf::Keyboard::isKeyPressed(sf::Keyboard::Key::LControl)) {
                std::cout << "Paste" << std::endl;
            }
        }

        // render
        window.clear(sf::Color::Black);
        // something to draw here
        window.display();
    }

    return 0;
}
```

Of course, just like with mouse events, keyboard key release events can also be handled using the ``event->getIf<sf::Event::KeyReleased>()`` function. This allows the program to react not only when a key is pressed, but also when it is released.
