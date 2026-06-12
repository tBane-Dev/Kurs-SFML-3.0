# Instalacja

Bibliotekę SFML 3.0 można pobrać z oficjalnej strony projektu SFML:
(https://www.sfml-dev.org)

W chwili pisania tego poradnika najnowszą wersją biblioteki jest SFML 3.1, dlatego odnośnik do pobrania nie znajduje się bezpośrednio na stronie głównej.

SFML 3.0 można pobrać bezpośrednio z archiwum wydań:
(https://www.sfml-dev.org/download/sfml/3.0.0/)

![SFML](download.png)

Jeżeli korzystasz z 64-bitowej wersji Visual Studio 2022, pobierz paczkę oznaczoną jako:
Visual C++ 17 (2022) - 64-bit

W przypadku innych wersji środowiska Visual Studio należy wybrać paczkę odpowiadającą używanemu kompilatorowi.

Po pobraniu archiwum rozpakuj je w wybranym katalogu, na przykład:

``C:\SFML-3.0``

Rozpakowany katalog powinien zawierać następującą strukturę:
```
-bin
-doc
-examples
-include
-lib
-changelog.md
-license.md
-readme.md
```
W dalszej części poradnika będziemy korzystać właśnie z tej lokalizacji podczas konfiguracji projektu.