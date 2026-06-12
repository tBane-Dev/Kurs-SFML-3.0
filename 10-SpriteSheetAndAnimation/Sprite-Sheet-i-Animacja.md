# Sprite Sheet i Animacja

W poprzednich rozdziałach poznaliśmy tekstury, sprajty, tekst oraz odmierzanie czasu. Teraz połączymy te elementy, aby stworzyć prostą animację postaci na podstawie jednego pliku graficznego, czyli tak zwanego sprite sheeta.

Sprite sheet to obraz zawierający kilka klatek animacji umieszczonych obok siebie. Zamiast wczytywać każdą klatkę jako osobny plik, możemy wczytać jeden obraz, a następnie wyświetlać tylko jego wybrany fragment. W SFML służy do tego funkcja ``setTextureRect()``, która pozwala określić prostokątny obszar tekstury widoczny na sprajcie.

Funkcja ``setTextureRect()`` przyjmuje obiekt typu ``sf::IntRect``. Obiekt ten zawiera pozycję oraz rozmiar fragmentu tekstury, który ma zostać wyświetlony. Dzięki temu możemy zmieniać aktualnie widoczną klatkę animacji, przesuwając prostokąt po sprite sheecie.

W tym przykładzie użyjemy również wcześniej poznanych klas ``sf::Clock`` oraz ``sf::Time``, aby zmieniać klatkę animacji co określony czas. Dodatkowo wykorzystamy tekst do wyświetlania numeru aktualnej klatki.

W programie najpierw wczytujemy teksturę sprite_sheet.png, która zawiera wszystkie klatki animacji. Następnie tworzymy sprajta i ustawiamy mu początkowy fragment tekstury za pomocą funkcji ``setTextureRect()``.

Zmienna ``frameSize`` określa rozmiar pojedynczej klatki animacji. W naszym przykładzie każda klatka ma rozmiar ``128x128`` pikseli. Zmienna ``framesCount`` przechowuje liczbę wszystkich klatek znajdujących się w sprite sheecie.

Numer aktualnej klatki przechowujemy w zmiennej ``currentFrame``. Aby przejść do następnej klatki, zwiększamy jej wartość o jeden. Operator modulo % sprawia, że po osiągnięciu ostatniej klatki animacja wraca z powrotem do klatki pierwszej.

Po obliczeniu numeru aktualnej klatki tworzymy obiekt ``sf::IntRect``, który określa fragment tekstury do wyświetlenia. Pozycja x prostokąta zależy od numeru aktualnej klatki oraz szerokości pojedynczej klatki.
Jeżeli ``currentFrame`` wynosi 0, wyświetlany jest fragment tekstury od pozycji x = 0. Jeżeli ``currentFrame`` wynosi 1, wyświetlany jest fragment od pozycji x = 128. Dla kolejnych klatek pozycja przesuwa się dalej o szerokość jednej klatki.

Do kontrolowania szybkości animacji używamy zegara. Program sprawdza, czy od ostatniej zmiany klatki minęło więcej niż 0.2 sekundy. Jeśli tak, animacja przechodzi do następnej klatki.

Na końcu aktualizujemy również tekst wyświetlany w oknie, aby pokazywał numer aktualnej klatki animacji.

W ten sposób możemy stworzyć prostą animację na podstawie jednego pliku graficznego. Jest to bardzo często używana technika w grach 2D, ponieważ pozwala przechowywać wiele klatek animacji w jednej teksturze i wygodnie wybierać, która z nich ma być aktualnie wyświetlana.

Sprite Sheet użyty w programie: <br />
![Sprite Sheet](sprite_sheet.png)

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Sprite Sheet and Animation");
    
    // utworzenie obiektu czcionki
    sf::Font font;
   
    // wczytanie czcionki z katalogu czcionek systemu Windows
    if(!font.openFromFile("C:\\Windows\\Fonts\\arial.ttf")) {
        std::cout << "Failed to load font!" << std::endl;
        return 0;
    }

    sf::Texture spriteSheetTexture; // utworzenie tekstury przechowującej sprite sheet
    if(!spriteSheetTexture.loadFromFile("sprite_sheet.png")) { // wczytanie sprite sheeta z pliku
        std::cout << "Failed to load texture!" << std::endl;
        return 0;
    }

    // utworzenie napisu wyświetlającego numer aktualnej klatki
    sf::Text frameText(font, "Frame: 0", 47u);
    frameText.setFillColor(sf::Color::White);
    frameText.setOutlineThickness(4.f);
    frameText.setOutlineColor(sf::Color(127, 127, 127));
    frameText.setPosition(sf::Vector2f(47.f, 47.f));

    // zmienne animacji
    sf::Vector2i frameSize(128, 128);
    int framesCount = 4;

    // zmienne do odmierzania czasu animacji
    sf::Clock clock;
    sf::Time prevTime = clock.getElapsedTime(); // przypisanie prevTime aktualnego czasu
    int currentFrame = 0;

    sf::Sprite sprite(spriteSheetTexture);
    sprite.setTextureRect(sf::IntRect(sf::Vector2i(0, 0), frameSize)); // ustaw pierwszą klatkę
    sprite.setScale(sf::Vector2f(2.f, 2.f));
    sprite.setPosition(sf::Vector2f(256.f, 160.f));

    while (window.isOpen()) {

        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();
        }

        sf::Time currentTime = clock.getElapsedTime(); // zapisanie aktualnego czasu

        if ((currentTime - prevTime).asSeconds() > 0.2f) {

            currentFrame = (currentFrame + 1) % framesCount;

            sf::IntRect textureRect;
            textureRect.position = sf::Vector2i(currentFrame * frameSize.x, 0);
            textureRect.size = frameSize;
            sprite.setTextureRect(textureRect); // ustawienie fragmentu tekstury odpowiadającego bieżącej klatce

            frameText.setString("Frame: " + std::to_string(currentFrame)); // aktualizacja tekstu z aktualną klatką

            prevTime = currentTime;
        }

        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        window.draw(sprite); // narysuj sprite
        window.draw(frameText); // narysuj tekst
        window.display(); // wyświetl zawartość okna
    }
}
```

![Animacja](program.png)