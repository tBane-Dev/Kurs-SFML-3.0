# Zdarzenia

Zdarzenia (ang. events) są to komunikaty informujące program o tym, że użytkownik wykonał jakąś akcję lub że w oknie aplikacji zaszła określona zmiana. Przykładem zdarzenia może być naciśnięcie klawisza, kliknięcie przyciskiem myszy lub zamknięcie okna.

Poniżej przedstawiam kod programu, który reaguje na zamknięcie okna. Jak zapewne zauważyłeś w poprzednich przykładach, okno można było zamknąć jedynie poprzez zatrzymanie programu w środowisku programistycznym. Teraz dodajemy obsługę zdarzenia zamknięcia okna, dzięki czemu po kliknięciu przycisku X aplikacja zostanie poprawnie zamknięta. Dodatkowo system operacyjny rozpoznaje, że okno prawidłowo obsługuje komunikaty, dlatego kursor myszy nie zmienia się już na ikonę informującą o braku odpowiedzi programu.

```cpp
#include <SFML/Graphics.hpp>

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Hello SFML 3.0");	
	
    while (window.isOpen()) {
		
        while (const std::optional event = window.pollEvent()) {
            if (event->is<sf::Event::Closed>()) 
                window.close();
        }
        
        // rysowanie
        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        // cokolwiek do narysowania
        window.display();	// wyświetl
    }

    return 0;
}
```

W bibliotece SFML wszystkie zdarzenia są przechowywane w kolejce zdarzeń. Funkcja ``window.pollEvent()`` pobiera kolejne zdarzenia z tej kolejki aż do momentu, gdy będzie ona pusta. Następnie możemy sprawdzić typ zdarzenia i odpowiednio na nie zareagować.

Poniżej przedstawiam kilka podstawowych zdarzeń związanych z obsługą myszy. Program będzie wypisywał informacje o naciśnięciu przycisku myszy, zwolnieniu przycisku oraz ruchu kursora.

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
                std::cout << "Kliknięto Lewy Przycisk Myszy na pozycji: " << mp->position.x << ", " << mp->position.y << std::endl;
            }

            if (const auto* mr = event->getIf<sf::Event::MouseButtonReleased>(); mr && mr->button == sf::Mouse::Button::Left) {
                std::cout << "Zwolniono Lewy Przycisk Myszy na pozycji" << mr->position.x << ", " << mr->position.y << std::endl;
            }

            if (const auto* mm = event->getIf<sf::Event::MouseMoved>(); mm) {
                std::cout << "Przesunięto Kursor na pozycję: " << mm->position.x << ", " << mm->position.y << std::endl;
            }

            if (const auto* mws = event->getIf < sf::Event::MouseWheelScrolled>(); mws) {
                std::cout << "Obrócono kółko myszy o  " << mws->delta << std::endl;
            }
        }
        
        // rysowanie
        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        // cokolwiek do narysowania
        window.display();	// wyświetl
    }

    return 0;
}
```

W podobny sposób można obsługiwać zdarzenia związane z klawiaturą. SFML udostępnia zdarzenie ``sf::Event::KeyPressed``, które jest generowane w momencie naciśnięcia klawisza. Po pobraniu zdarzenia za pomocą funkcji ``getIf<sf::Event::KeyPressed>()`` możemy sprawdzić, który klawisz został naciśnięty, porównując jego kod z wartościami z wyliczenia ``sf::Keyboard::Key``.

W poniższym przykładzie program reaguje na naciśnięcie klawiszy W, A, S, D oraz klawiszy kierunkowych. Po wykryciu odpowiedniego zdarzenia informacja jest wypisywana do konsoli. W praktycznych zastosowaniach w miejscu funkcji ``std::cout`` można umieścić dowolną logikę programu, na przykład sterowanie postacią, przewijanie kamery lub wywoływanie określonych funkcji.

Warto pamiętać, że zdarzenie KeyPressed jest wywoływane tylko w momencie naciśnięcia klawisza. Jeżeli chcemy sprawdzać, czy klawisz jest aktualnie przytrzymywany, należy skorzystać z mechanizmu odpytywania stanu klawiatury, który poznamy w dalszej części poradnika.

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
                std::cout << "Naciśnięto klawisz W" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::A) {
                std::cout << "Naciśnięto klawisz A" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::S) {
                std::cout << "Naciśnięto klawisz S" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::D) {
                std::cout << "Naciśnięto klawisz D" << std::endl;
            }

            // Klawisze kierunkowe
            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::Up) {
                std::cout << "Naciśnięto klawisz ze strzałką w górę" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::Left) {
                std::cout << "Naciśnięto klawisz ze strzałką w lewo" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::Down) {
                std::cout << "Naciśnięto klawisz ze strzałką w dół" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::Right) {
            std::cout << "Naciśnięto klawisz ze strzałką w prawo" << std::endl;
            }

        }

        // rysowanie
        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        // cokolwiek do narysowania
        window.display();	// wyświetl
    }

    return 0;
}
```

Można również sprawdzać moment zwolnienia klawisza za pomocą zdarzenia ``sf::Event::KeyReleased``. Jest to przydatne w sytuacjach, gdy akcja powinna zostać wykonana dopiero po puszczeniu klawisza, a nie w chwili jego naciśnięcia.

Obsługa zdarzenia ``KeyPressed`` jest wykorzystywana na przykład podczas implementacji obsługi skrótów klawiszowych, takich jak ``Ctrl+C, Ctrl+X, Ctrl+V``. Program może sprawdzić, czy w momencie naciśnięcia klawisza C, X lub V wciśnięty jest również klawisz Ctrl, a następnie wykonać odpowiednią operację kopiowania, wycinania lub wklejania. Dzięki temu użytkownik może wygodnie korzystać ze standardowych skrótów klawiaturowych znanych z większości aplikacji.

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
                std::cout << "Wykonano akcję Wytnij" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::C && sf::Keyboard::isKeyPressed(sf::Keyboard::Key::LControl)) {
                std::cout << "Wykonano akcję Kopiuj" << std::endl;
            }

            if (const auto* kp = event->getIf<sf::Event::KeyPressed>(); kp && kp->code == sf::Keyboard::Key::V && sf::Keyboard::isKeyPressed(sf::Keyboard::Key::LControl)) {
                std::cout << "Wykonano akcję Wklej" << std::endl;
            }
        }

        // rysowanie
        window.clear(sf::Color::Black); // wyczyść ekran i wypełnij czarnym kolorem
        // cokolwiek do narysowania
        window.display();	// wyświetl
    }

    return 0;
}
```

Oczywiście, podobnie jak w przypadku zdarzeń związanych z myszą, również dla klawiatury można obsługiwać zdarzenie zwolnienia klawisza za pomocą funkcji ``event->getIf<sf::Event::KeyReleased>()``. Dzięki temu program może reagować nie tylko na moment naciśnięcia klawisza, ale również na jego puszczenie.