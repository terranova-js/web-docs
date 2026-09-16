# Tworzenie aplikacji webowej (Katalog Gier)

## Opis zadania

Wykonaj aplikację internetową opartą na architekturze MVC z wykorzystaniem frameworka CodeIgniter 4.

Aplikacja ma pełnić funkcję prostego katalogu gier i realizować pełen cykl operacji CRUD (Create, Read, Update, Delete). Interfejs użytkownika należy oprzeć na systemie szablonów (Layouts) oraz bibliotece stylów Pico.css.

## 1. Wytyczne dla bazy danych i modelu
   Wykorzystaj bazę danych MySQL/MariaDB.

Utwórz migrację dla tabeli o nazwie games zawierającą następujące pola:

 * id – klucz główny (Primary Key), Auto Increment, typ całkowity.
 * title – typ tekstowy (np. VARCHAR 100), pole wymagane.
 * genre – typ tekstowy (np. VARCHAR 50), pole wymagane.
 * release_year – typ całkowity (np. INT), pole wymagane.

Utwórz model o nazwie GameModel obsługujący tabelę games. Skonfiguruj w nim dozwolone pola ($allowedFields), aby umożliwić zapis i aktualizację danych.

## 2. Wytyczne dla rutingu (Routes)
   W pliku konfiguracyjnym `app/Config/Routes.php` wyłącz domyślny auto-routing i jawnie zdefiniuj następujące ścieżki:

  `GET /game` – wyświetlanie listy wszystkich gier.

  `GET /game/info(:num)` – wyświetlanie informacji o grze.

  `GET /game/add` – wyświetlanie formularza dodawania nowej gry.

  `POST /game/save` – odbiór danych z formularza i zapis do bazy.

  `GET /game/edit/(:num)` – wyświetlanie formularza edycji wybranej gry (gdzie (:num) to ID gry).

  `POST /game/update/(:num)` – odbiór danych z formularza edycji i aktualizacja wpisu w bazie.

  `DELETE /game/delete/(:num)` – usunięcie wpisu o podanym ID z bazy danych i przekierowanie z powrotem do listy.

## 3. Wytyczne dla kontrolera (GamesController)
   Utwórz kontroler GamesController zawierający metody obsługujące zdefiniowane ścieżki. Kontroler musi komunikować się z modelem GameModel.

 * Metoda index – pobiera wszystkie gry z bazy i przekazuje je do widoku.
 * Metoda info – wyświetla widok wyglądu gry.
 * Metoda create – ładuje widok pustego formularza.
 * Metoda store – pobiera dane z żądania POST i wstawia nowy rekord do bazy, a następnie przekierowuje użytkownika na ścieżkę /game.
 * Metoda edit – pobiera z bazy grę o przekazanym ID. Jeśli gra istnieje, przekazuje jej dane do widoku formularza.
 * Metoda update – pobiera zmienione dane z żądania POST i aktualizuje rekord o podanym ID, po czym wykonuje przekierowanie na /game.
 * Metoda delete – usuwa rekord o podanym ID z bazy danych i wykonuje przekierowanie na /game.

## 4. Wytyczne dla interfejsu użytkownika (Widoki)

   Zastosuj mechanizm dziedziczenia widoków dostępny w CodeIgniter 4.

### A. Główny szablon (app/Views/layouts/main.php)

 * Zdefiniuj strukturę dokumentu HTML5.
 * Dołącz bibliotekę Pico.css korzystając z adresu CDN.
 * Utwórz nawigację (tag <nav>) z linkami "Lista Gier" (/game) oraz "Dodaj grę" (/game/add).
 * Zdefiniuj sekcję o nazwie content, w której będą wyświetlane poszczególne podstrony.

### B. Widok listy gier (app/Views/games/index.php)

 * Dziedziczy z szablonu main.php.
 * Wyświetla dane przekazane z kontrolera w formie tabeli HTML.
 * Tabela ma zawierać kolumny: ID, Tytuł, Gatunek, Rok wydania oraz Akcje.
 * W kolumnie Akcje dla każdego wiersza umieść dwa przyciski/linki:
 * Edytuj (kierujący na /game/edit/ID)
 * Usuń (kierujący na /game/delete/ID)

### C. Widok dodawania/edycji gry (app/Views/games/create.php oraz edit.php)

 * Dopuszczalne jest stworzenie jednego wspólnego widoku formularza (np. form.php) lub dwóch osobnych.
 * Dziedziczy z szablonu main.php.
 * Formularz zawiera pola tekstowe dla: Tytułu, Gatunku (może być element <select>) oraz Roku wydania.
 * Wszystkie pola muszą być opisane znacznikami <label>.
 * W przypadku formularza edycji, pola muszą być wstępnie wypełnione danymi pobranymi z bazy.
 * Formularz zawiera przycisk typu submit zapisujący zmiany.
