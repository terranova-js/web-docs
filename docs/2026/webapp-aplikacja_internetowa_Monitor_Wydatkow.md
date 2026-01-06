Wykonaj aplikację internetową służącą do ewidencji i wizualizacji wydatków osobistych. Wykorzystaj do tego celu pakiet XAMPP oraz biblioteki Bootstrap i Chart.js.

## 1. Operacje na bazie danych

Baza danych o nazwie `portfel` powinna zawierać tabelę `wydatki` o następujących polach:

- `id`: klucz główny, z autoinkrementacją, typ całkowity.
- `nazwa`: tekst o długości do 100 znaków, pole nie może być puste.
- `kwota`: typ zmiennoprzecinkowy (np. `DECIMAL(10,2)`), pole nie może być puste.
- `kategoria`: typ tekstowy (`Jedzenie`, `Transport`, `Rozrywka`, `Rachunki`, `Inne`).
- `data_wydatku`: typ `DATE`.

## 2. Architektura plików aplikacji

W folderze głównym serwera powinny znaleźć się następujące pliki:

- `db.php`: skrypt odpowiedzialny za połączenie z bazą danych przy użyciu obiektu `PDO` lub funkcji `mysqli`.
- `index.php`: główny plik witryny zawierający strukturę HTML, formularz oraz skrypty JS.
- `dodaj.php`: skrypt PHP odbierający dane z formularza metodą `POST` i zapisujący je w bazie danych.
- `dane_wykresu.php`: skrypt PHP pobierający z bazy sumy wydatków pogrupowane według kategorii i zwracający je w formacie **JSON**.

## 3. Wygląd i funkcjonalność (Frontend)

- **Stylistyka**: Wykorzystaj framework **Bootstrap** do przygotowania responsywnego układu strony (grid system).
- **Formularz**: Powinien zawierać pola edycyjne dla nazwy, kwoty (typ `number` z krokiem `0.01`), daty oraz listę rozwijaną (`select`) dla kategorii.
- **Wykres**: Na stronie głównej powinien znajdować się element `<canvas>`, na którym za pomocą biblioteki **Chart.js** zostanie wygenerowany wykres kołowy (`pie`).
- **Komunikacja**: Dane do wykresu powinny zostać pobrane asynchronicznie za pomocą funkcji `fetch()` lub metody `$.getJSON()` z biblioteki jQuery.

## 4. Logika biznesowa (Backend)

- Skrypt `dane_wykresu.php` musi realizować zapytanie SQL wykorzystujące funkcję agregującą oraz grupowanie:
`SELECT kategoria, SUM(kwota) AS suma FROM wydatki GROUP BY kategoria`.
- Skrypt `dodaj.php` po poprawnym zapisaniu rekordu do bazy powinien przekierować użytkownika z powrotem do strony `index.php` za pomocą nagłówka `header`.
- Należy zastosować podstawowe zabezpieczenia przed atakami typu SQL Injection (np. poprzez bindowanie parametrów w PDO).

## 5. Dokumentacja (Kryteria oceniania)

- Prawidłowe utworzenie struktury bazy danych.
- Połączenie z bazą i obsługa błędów.
- Prawidłowe przesyłanie danych z formularza do bazy.
- Implementacja zapytania grupującego dane.
- Poprawna generacja formatu JSON na potrzeby wykresu.
- Integracja zewnętrznej biblioteki JavaScript (Chart.js) i poprawne wyświetlenie danych pobranych z API.
