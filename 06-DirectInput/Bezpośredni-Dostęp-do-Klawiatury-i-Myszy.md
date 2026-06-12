# Bezpośredni Dostęp do Klawiatury i Myszy

W poprzednim rozdziale omówiliśmy zdarzenia związane z naciśnięciem klawiszy WASD oraz strzałek kierunkowych. Niestety podczas przytrzymywania klawisza zdarzenie wykonywane jest tylko raz. W niektórych sytuacjach, na przykład podczas poruszania postacią, chcielibyśmy, aby postać poruszała się tak długo, jak długo klawisz pozostaje wciśnięty.

Aby to zrealizować, możemy bezpośrednio sprawdzić, czy dany klawisz jest aktualnie wciśnięty. Warto zapamiętać, że zdarzenie nie jest tym samym co bezpośredni dostęp. Zdarzenia informują program o tym, że wystąpiła określona akcja, na przykład naciśnięcie lub zwolnienie klawisza. Bezpośredni dostęp pozwala natomiast w dowolnym momencie sprawdzić aktualny stan klawiatury lub myszy.

Poniższy kod pozwala sprawdzić, czy dany klawisz W,A,S lub D jest aktualnie wciśnięty.

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
            std::cout << "Klawisz W jest naciśnięty" << std::endl;
        }

        if(sf::Keyboard::isKeyPressed(sf::Keyboard::Key::A)) {
            std::cout << "Klawisz A jest naciśnięty" << std::endl;
        }

        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::S)) {
            std::cout << "Klawisz S jest naciśnięty" << std::endl;
        }

        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::D)) {
            std::cout << "Klawisz D jest naciśnięty" << std::endl;
        }

        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        // cokolwiek do rysowania tutaj
        window.display();	// wyświetl zawartość okna
    }
}
```

Podobnie jak w przypadku klawiatury, SFML umożliwia bezpośrednie sprawdzanie stanu przycisków myszy. Dzięki temu możemy w dowolnym momencie sprawdzić, czy lewy, prawy lub środkowy przycisk jest aktualnie wciśnięty, bez konieczności oczekiwania na zdarzenie. Jest to szczególnie przydatne podczas tworzenia edytorów, gier strategicznych lub programów wymagających przeciągania obiektów za pomocą myszy. Dodatkowo możemy odczytać aktualną pozycję kursora względem okna aplikacji, co pozwala określić miejsce, nad którym znajduje się użytkownik. Poniższy przykład wyświetla informację o wciśniętym przycisku myszy, a gdy żaden przycisk nie jest naciśnięty, wypisuje pozycję kursora względem okna.

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
            std::cout << "Lewy przycisk myszy jest naciśnięty" << std::endl;
        }
        else if (sf::Mouse::isButtonPressed(sf::Mouse::Button::Right)) {
            std::cout << "Prawy przycisk myszy jest naciśnięty" << std::endl;
        }
        else if (sf::Mouse::isButtonPressed(sf::Mouse::Button::Middle)) {
            std::cout << "Środkowy przycisk myszy jest naciśnięty" << std::endl;
        }
        else {
            sf::Vector2i mousePosition = sf::Mouse::getPosition(window); // pobranie pozycji myszy względem okna
            std::cout << "Pozycja myszy: (" << mousePosition.x << ", " << mousePosition.y << ")" << std::endl;
        }


        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        // cokolwiek do rysowania tutaj
        window.display();	// wyświetl zawartość okna
    }
}
```
