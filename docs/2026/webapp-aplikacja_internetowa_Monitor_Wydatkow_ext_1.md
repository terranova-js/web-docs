# Rozszerzenia aplikacji Monitor Wydatków:

## 1. Różne źródła danych

Aplikacja powinna umożliwiać wybór źródła danych, z którego pobierane i zapisywane są informacje o wydatkach.
W ustawieniach aplikacji należy dodać sekcję „Źródło danych”, w której użytkownik może wybrać jedną z opcji:

Dostępne źródła danych
- Baza danych (MySQL) – domyślne źródło, zgodne z pierwotnym projektem.
- Plik JSON – tryb demonstracyjny, umożliwiający działanie aplikacji bez serwera bazodanowego.
- Plik CSV – alternatywne źródło danych, przydatne do testów i integracji.

#### Wymagania techniczne

W folderze projektu należy utworzyć plik `config.php`, zawierający ustawienie:

```php
$data_source = 'mysql'; // możliwe wartości: mysql, json, csv
```

__Należy przygotować warstwę abstrakcji danych__:

Plik `DataSourceInterface.php` – interfejs z metodami:
 - `getAll()`
 - `add($record)`
 - `delete($id)`
 - `update($id, $record)`
 - `getChartData()`

implementacje:
 - `MySQLDataSource.php`
 - `JsonDataSource.php`
 - `CsvDataSource.php`

W pliku `index.php` aplikacja powinna automatycznie wczytać odpowiednią klasę na podstawie ustawienia w `config.php`.

#### Wymagania funkcjonalne:

Użytkownik nie musi wiedzieć, z jakiego źródła korzysta aplikacja — interfejs pozostaje taki sam.

W trybie JSON/CSV dane są zapisywane lokalnie w plikach:
- data/data.json
- data/data.csv

W trybie JSON/CSV operacje dodawania, usuwania i edycji muszą działać analogicznie jak w MySQL.

Wykres Chart.js  musi poprawnie pobierać dane niezależnie od źródła.

## 2. Import i eksport danych

Aplikacja powinna umożliwiać użytkownikowi przenoszenie danych między różnymi instalacjami systemu oraz wykonywanie kopii zapasowych.

### 2.1. Eksport danych

W menu aplikacji należy dodać sekcję „Eksport danych”, umożliwiającą pobranie pliku z aktualnymi wydatkami.

Dostępne formaty eksportu:
 - CSV – kompatybilny z Excel, Google Sheets i innymi aplikacjami.
 - JSON – format strukturalny, przydatny do integracji z innymi systemami.

Wymagania techniczne:

Plik `export.php` powinien:
- pobierać dane z aktualnie wybranego źródła,
- generować plik w wybranym formacie,
- ustawiać nagłówki HTTP umożliwiające pobranie pliku.

Przykład nagłówka:

```php
header('Content-Type: text/csv');
header('Content-Disposition: attachment; filename="wydatki.csv"');
```

### 2.2. Import danych

W menu aplikacji należy dodać sekcję „Import danych”, umożliwiającą wczytanie pliku CSV lub JSON.

### Wymagania funkcjonalne:

Formularz importu powinien umożliwiać przesłanie pliku .csv lub .json.

Po przesłaniu pliku aplikacja powinna:
- zweryfikować poprawność struktury danych,
- wyświetlić podgląd danych przed zapisaniem,
- umożliwić użytkownikowi:
  * scalenie danych z istniejącymi,
  * nadpisanie istniejących danych.

#### Wymagania techniczne:

Plik `import.php` powinien:
- odczytać przesłany plik,
- przekształcić dane do formatu tablicy PHP,
- przekazać dane do aktualnie wybranego źródła danych.

Walidacja powinna obejmować:
- poprawność pól: nazwa, kwota, kategoria, data_wydatku,
- poprawność formatu daty,
- poprawność wartości liczbowych.


## 3. Zmiany w interfejsie użytkownika (Frontend)

W menu głównym należy dodać sekcję „Ustawienia”, zawierającą wybór źródła danych.

Należy dodać dwie nowe podstrony:
- `import.php` – formularz importu,
- `export.php` – wybór formatu eksportu.

W przypadku błędów importu aplikacja powinna wyświetlać czytelne komunikaty.

## 4. Logika biznesowa (Backend)

Wszystkie operacje na danych muszą być wykonywane przez warstwę abstrakcji danych.

Wykres Chart.js  musi korzystać z metody `getChartData()` niezależnie od źródła.

Import i eksport muszą działać poprawnie w każdym trybie (MySQL, JSON, CSV).

