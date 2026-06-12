# Button With Sprite

Textures: <br />
![Buttons Texture](button.png) <br />
![Buttons Hover Texture](button_hover.png) <br />
![Buttons Press Texture](button_press.png) <br />

![Buttons With Sprite](buttons_with_sprites.png)

```cpp
#include <SFML/Graphics.hpp>
#include <iostream>
#include <functional>
#include <memory>

sf::Vector2f mousePosition;

enum class ButtonState { Idle, Hovered, Pressed };

class Button;

std::shared_ptr<Button> hoveredButton = nullptr;
std::shared_ptr<Button> pressedButton = nullptr;

class Button : public std::enable_shared_from_this<Button>{
public:
    sf::FloatRect _rect;
    sf::Texture _texture;
    sf::Texture _hoverTexture;
    sf::Texture _pressTexture;
    std::function<void()> _onClickFunction;

    ButtonState _state;

    Button(sf::Vector2f position, sf::Texture texture, sf::Texture hoverTexture, sf::Texture pressTexture);
    ~Button();

    void cursorHover();
    void handleEvent(const sf::Event& event);
    void update();
    void draw(sf::RenderWindow& window);
};

Button::Button(sf::Vector2f position, sf::Texture texture, sf::Texture hoverTexture, sf::Texture pressTexture) {
    _rect = sf::FloatRect(position, sf::Vector2f(texture.getSize()));

    _texture = texture;
    _hoverTexture = hoverTexture;
    _pressTexture = pressTexture;

    _state = ButtonState::Idle;
}

Button::~Button() {

}

void Button::cursorHover() {
    if (_rect.contains(mousePosition)) {
        hoveredButton = this->shared_from_this();
    }
}

void Button::handleEvent(const sf::Event& event) {
    if (const auto* mbp = event.getIf<sf::Event::MouseButtonPressed>(); mbp && mbp->button == sf::Mouse::Button::Left) {
        if (hoveredButton.get() == this) {
            pressedButton = this->shared_from_this();
            _state = ButtonState::Pressed;
        }
    }
}

void Button::update() {
    if (_state == ButtonState::Pressed) {
        if (_onClickFunction) {
            _onClickFunction();
        }

        if(pressedButton.get() == this) {
            pressedButton = nullptr;
        }

        _state = ButtonState::Idle;
    }
    else if (hoveredButton.get() == this) {
        _state = ButtonState::Hovered;
    }
    else {
        _state = ButtonState::Idle;
    }
}

void Button::draw(sf::RenderWindow& window) {

    sf::Texture currentTexture;

    switch (_state) {
        case ButtonState::Hovered:
            currentTexture = _hoverTexture;
            break;
        case ButtonState::Pressed:
            currentTexture = _pressTexture;
            break;
        default:
            currentTexture = _texture;
            break;
    }

    sf::Sprite sprite(currentTexture);
    sprite.setPosition(_rect.position);
    window.draw(sprite);
}

int main() {
    sf::RenderWindow window = sf::RenderWindow(sf::VideoMode(sf::Vector2u(800u, 600u)), "Buttons With Sprite");
	
    sf::Texture buttonTexture;
    sf::Texture buttonHoverTexture;
    sf::Texture buttonPressTexture;

    buttonTexture.loadFromFile("button.png");
    buttonHoverTexture.loadFromFile("button_hover.png");
    buttonPressTexture.loadFromFile("button_press.png");

    std::shared_ptr<Button> button1 = std::make_shared<Button>(
        sf::Vector2f(150.f, 150.f), 
        buttonTexture,
        buttonHoverTexture,
        buttonPressTexture
    );

    button1->_onClickFunction = []() {
        std::cout << "Button 1 clicked!" << std::endl;
    };
    
    std::shared_ptr<Button> button2 = std::make_shared<Button>(
        sf::Vector2f(450.f, 150.f), 
        buttonTexture,
        buttonHoverTexture,
        buttonPressTexture
    );

    button2->_onClickFunction = []() {
        std::cout << "Button 2 clicked!" << std::endl;
    };
     
    std::shared_ptr<Button> button3 = std::make_shared<Button>(
        sf::Vector2f(300.f, 350.f),
        buttonTexture,
        buttonHoverTexture,
        buttonPressTexture
    );

    button3->_onClickFunction = []() {
        std::cout << "Button 3 clicked!" << std::endl;
    };

    while (window.isOpen()) {

        mousePosition = window.mapPixelToCoords(sf::Mouse::getPosition(window));

        hoveredButton = nullptr;
        button1->cursorHover();
        button2->cursorHover();
        button3->cursorHover();


        while (const std::optional event = window.pollEvent()) {

            if (event->is<sf::Event::Closed>())
                window.close();

            button1->handleEvent(*event);
            button2->handleEvent(*event);
            button3->handleEvent(*event);
        }
        
        button1->update();
        button2->update();
        button3->update();

        window.clear(sf::Color::Black);
        button1->draw(window);
        button2->draw(window);
        button3->draw(window);
        window.display();
    }
}
```