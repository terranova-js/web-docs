# Rozszerzenie aplikacji Monitor Wydatków:

## System logowania i rejestracji użytkowników

To rozszerzenie wprowadza do aplikacji obsługę wielu użytkowników, logowanie, sesje oraz podstawową autoryzację. Dzięki temu każdy użytkownik będzie miał dostęp wyłącznie do swoich danych, a administrator uzyska możliwość zarządzania kontami.

#### Wymagania techniczne

1. Nowe tabele w bazie danych
Należy dodać tabelę:

`users`
Przykładowe pola:

* `id` – klucz główny
* `username` – unikalna nazwa użytkownika
* `password` – hasło zapisane w postaci hasha (password_hash())
* `role` – rola użytkownika (user lub admin)
* `created_at` – data utworzenia konta

2. Modyfikacja istniejącej tabeli expenses

Należy dodać pole:

* `user_id` – klucz obcy wskazujący na tabelę users

Wszystkie zapytania dotyczące wydatków muszą być filtrowane po user_id.

3. Obsługa sesji

Aplikacja powinna wykorzystywać:

* `session_start()` na stronach wymagających logowania,
* zmienne sesyjne do przechowywania informacji o zalogowanym użytkowniku (user_id, username, role),
* mechanizm wylogowania niszczący sesję.

4. Bezpieczeństwo

* hasła muszą być przechowywane wyłącznie jako hashe (`password_hash()`),
* przy logowaniu należy używać `password_verify()`,
* dostęp do stron powinien być zabezpieczony przed użytkownikami niezalogowanymi,
* panel administratora powinien być dostępny tylko dla roli admin.

#### Wymagania funkcjonalne

1. Rejestracja użytkownika

Aplikacja powinna umożliwiać:

* utworzenie nowego konta użytkownika,
* walidację danych (unikalna nazwa użytkownika, minimalna długość hasła),
* zapisanie użytkownika w bazie z hasłem w formie hasha,
* rejestracja dostępna tylko dla administratora.

2. Logowanie użytkownika

Użytkownik powinien mieć możliwość:

* podania loginu i hasła,
* weryfikacji danych w bazie,
* zalogowania się i zapisania danych w sesji.

W przypadku błędnych danych należy wyświetlić komunikat o błędzie.

3. Wylogowanie

  Aplikacja powinna umożliwiać:

* wylogowanie użytkownika,
* usunięcie danych z sesji,
* przekierowanie na stronę logowania.

4. Ochrona dostępu

* wszystkie podstrony związane z wydatkami muszą być dostępne tylko dla zalogowanych użytkowników,
* użytkownik widzi wyłącznie swoje wydatki (filtrowanie po user_id),
* panel administratora dostępny tylko dla roli admin.

5. Panel administratora (opcjonalnie)

  Administrator powinien mieć możliwość:

* przeglądania listy użytkowników,
* usuwania kont,
* resetowania haseł,
* podglądu statystyk całego systemu.
