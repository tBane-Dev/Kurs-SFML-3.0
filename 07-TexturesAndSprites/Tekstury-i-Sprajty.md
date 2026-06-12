# Textures and Sprites

Żeby narysować obraz w SFML potrzebne są dwie rzeczy - tekstura i sprite.

Tekstura (sf::Texture) jest obiektem przechowującym dane obrazu w pamięci karty graficznej. Można ją wczytać z pliku, np. PNG lub JPG. Sama tekstura nie jest jednak widoczna na ekranie.

Sprite (sf::Sprite) jest obiektem służącym do wyświetlania tekstury w oknie. To właśnie sprite posiada pozycję, obrót, skalę oraz inne właściwości związane z renderowaniem. Do jednego obiektu tekstury można przypisać wiele sprite'ów, dzięki czemu ten sam obraz może być wyświetlany w różnych miejscach na ekranie bez potrzeby wielokrotnego wczytywania pliku (w ten sposób powstają managery tekstur).

Najpierw należy wczytać teksturę, następnie przypisać ją do sprajta, a na końcu narysować sprite w oknie za pomocą funkcji draw(). Poniższy kod właśnie to przedstawia.

Tekstura, którą użyto w programie: <br />

![Texture](bies.png)

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Texture and Sprite");
	
    sf::Texture texture; // utworzenie obiektu tekstury

    // Ładowanie tekstury z pliku "bies.png"
    if (!texture.loadFromFile("bies.png")) {
        std::cout << "Failed to load texture" << std::endl; // w przypadku niepowodzenia wyświetl komunikat o błędzie
        return 0; // i przerwij program
    }

    sf::Sprite sprite(texture); // utworzenie sprajta i przypisanie do niego tekstury
    sprite.setOrigin(sf::Vector2f(texture.getSize() / 2u)); // ustawienie punktu odniesienia na środek tekstury
    sprite.setPosition(sf::Vector2f(window.getSize() / 2u)); // ustawienie sprajta na środku okna
    sprite.setScale(sf::Vector2f(2.f, 2.f)); // dwukrotne powiększenie sprajta w poziomie i w pionie
    
    while (window.isOpen()) {

        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();
        }

        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        window.draw(sprite); // narysowanie sprajta
        window.display(); // wyświetlenie zawartości okna
    }

    return 0;
}
```

![Textures and Sprites](program.png)
