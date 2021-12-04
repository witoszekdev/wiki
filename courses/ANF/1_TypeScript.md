# TypeScript

## Podstawy

### Dlaczego bezpieczeństwo typów?

- Zapewnia że aplikacja sama w sobie się nie wywali (błędne dane od usera = 500, jest ok)
- beneficjent: my albo ktoś inny za 0.5 roku
- jeśli kod ma zostać przejęty przez kogoś warto zainwestować w typy
- nie ma nic za darmo: trzeba rozumieć komunikaty błędów
- TS nie jest w stanie wszystkieg wyłapać

TS jest najbardziej popularny, lepsze wsparcie od bibliotek, itd.

- Zastępuje unit testy na badanie struktury danych - skupiamy się na funkcjonalnościach
- Typ to podpowiedź dla innego programisty (przekazujemy nasze intencje)
- Mamy zaufanie do kompilatora (wychwyci nasze błędy)

Ograniczenia TS'a

- nie wszystkie typy da się wychwycić (póki nie uruchomimy, nie znamy danych)
  - nie sprawdzimy poprawności JSONa z serwera
  - jeśli TS miałby to robić, to musiałby dorzucić kod do runtime'u
- race condition
- koercja jest przepuszczona (rzutowanie na stringi działa w TSie)
- dzielenie przez 0 xD (dostajemy Infinity) - JS to przepuszcza

TS nie respektuje semver!

- podnośmy wersję TS'a regularnie (dzięki temu zmiany będą łatwiejsze)

### PROTIPy

`declare` - zadeklarować zmienną / funkcję ale bez inicjowania jej początkowej wartości

w TS playground możemy importować moduły z npm'a! (dokładniej importujemy tylko typyp z `@types`), React (z JSX) też działa
