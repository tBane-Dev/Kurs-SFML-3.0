# Zegar i Czas

Często zachodzi potrzeba odmierzania czasu w programach, a w szczególności w grach komputerowych. Pomiar czasu jest niezbędny między innymi do tworzenia animacji, obsługi opóźnień, odmierzania czasu rozgrywki, wyświetlania liczników czy wykonywania określonych akcji w regularnych odstępach czasu. 

Biblioteka SFML udostępnia do tego celu klasy ``sf::Clock`` oraz ``sf::Time``. Klasa ``sf::Clock`` działa jak stoper - pozwala mierzyć czas, jaki upłynął od momentu jej utworzenia lub ostatniego wyzerowania. Z kolei klasa ``sf::Time`` przechowuje zmierzoną wartość czasu i umożliwia jej wygodne odczytanie w sekundach, milisekundach lub mikrosekundach. Dzięki tym klasom możemy w prosty sposób kontrolować upływ czasu w naszych aplikacjach.

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Clock and Time");
    
    // stwórz obiekt czcionki
    sf::Font font;
   
    // załaduj czionkę z Windowsa
    if(!font.openFromFile("C:\\Windows\\Fonts\\arial.ttf")) {
        std::cerr << "Failed to load font!" << std::endl;
        return 0;
    }

    // stwórz tekst dla sekund
    sf::Text textSeconds(font, "0 seconds", 47u);
    textSeconds.setFillColor(sf::Color::White);
    textSeconds.setOutlineThickness(4.f);
    textSeconds.setOutlineColor(sf::Color(127, 127, 127));
    textSeconds.setPosition(sf::Vector2f(100.f, 100.f));

    // stwórz tekst dla milisekund
    sf::Text textMiliSeconds(font, "0 miliseconds", 47u);
    textMiliSeconds.setFillColor(sf::Color::White);
    textMiliSeconds.setOutlineThickness(4.f);
    textMiliSeconds.setOutlineColor(sf::Color(127, 127, 127));
    textMiliSeconds.setPosition(sf::Vector2f(100.f, 200.f));

    // stwórz tekst dla mikrosekund
    sf::Text textMicroSeconds(font, "0 microseconds", 47u);
    textMicroSeconds.setFillColor(sf::Color::White);
    textMicroSeconds.setOutlineThickness(4.f);
    textMicroSeconds.setOutlineColor(sf::Color(127, 127, 127));
    textMicroSeconds.setPosition(sf::Vector2f(100.f, 300.f));

    sf::Clock clock; // stwórz obiekt zegaru dla obliczania czasu

    while (window.isOpen()) {

        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();
        }

        sf::Time time = clock.getElapsedTime(); // pobierz czas

        std::stringstream seconds;
        seconds << std::fixed << std::setprecision(2) << time.asSeconds(); // ustawienie precyzji dla sekund - 2 miejsca po przecinku

        textSeconds.setString(seconds.str() + " seconds");
        textMiliSeconds.setString(std::to_string(time.asMilliseconds()) + " miliseconds");
        textMicroSeconds.setString(std::to_string(time.asMicroseconds()) + " microseconds");

        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        window.draw(textSeconds);	// wyświetl sekundy
        window.draw(textMiliSeconds);	// wyświetl milisekundy
        window.draw(textMicroSeconds);	// wyświetl mikrosekundy
        window.display(); // wyświelt zawartość okna
    }

    return 0;
}
```

![Zegar i Czas](program.png)