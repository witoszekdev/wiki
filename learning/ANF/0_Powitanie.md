Nie można mieć wszystkiego

Tanio - Dobrze - Szybko

Tanio + Dobrze = Długo
Tanio + Szybko = Źle
Szybko + Dobrze = Drobo
Wszyskiego na raz się nie da 😅

W architekturzy nie ma cudów - są tradeoffy (jeśli coś jest cudem to znaczy że autor coś nam nie mówi, albo sam nie wie o czym mówi)

Co lepsze? To zależy
Jeśli umiesz obsługiwać jedynie młotek - wszystko wygląda jak gwóźdź

## Przykłady trade-offów

### Redux

Plusy:

- Stan przechowywany w obiekcie
- Możliwość podróżowania w czasie
- Serializacji
- Zapisywania na backendzie

Wady:

- Te obiekty trzeba tworzyć (boilerplate)
- Sporo pisania (akcje, reducery, selektory)
- "Technologiczne bizancjum" xD

### TypeScript

- Sporo czasu na nauce oraz utrzymywaniu
- Kod obsługiwający runtime podlega kompilajci = więcej roboty
- Uporządkowane struktury danych (długoterminowo łatiwjesze)
- Początkowo spowolni, potem przyśpieszy
- Trwale podnosi poziom skomplikowania projektu
- Każda osoba będzie musiała zrozumieć typy

## Drivery architektorniczne

Czynniki, które nadają kierunek naszym decyzjom

np.

- niezależne jednostki wdrożeniowe (np. w korpo rzadko, ludzie nie ufają nowej wersji)
  - rozwiązanie przykładowe: mikrofrontendy
- ujednolicenie tool stacka, konwencje
  - software house ma wiele projektów
  - ustalenie i trzymanie się konwencji
- szybkość na urządzeniach mobilnych
  - jak wygląda, jak szybko działa
  - to martość powierzchni reklamowej
  - stosowanie SPA to słaby pomysł 😬
  - SWR (stale-while-revalidate) wyswietl to co w cache, w między czasie pobierz update

## Architektura

Zestaw ograniczeń (biznesowych lub technicznych)
np. nasza strona musi reagować szybciej niż x inaczej odejdą do konkurencji, musimy dostarczyć software bo deadline

Decyzje, ich konsekwencje i koszt zmiany (ciężko to potem zmienić)
np. wybór frameworka oraz toolstacka jaki narzuca
Redux - jedyne źródło prawdy (ma korzyści jedynie jeśli jest single source of truth), np. odtworzenie błędu (jeśli mamy tylko 90% danych to nie odtworzymy w 100% czyli wcale)

Jeśli coś trudno potem zmienić ta decyzja musi być solidnie wyargumentowana

Zestaw uniwersalnych cech kodu: styl, elastyczność, łatwość utrzymywania, szybkość dowożenia ficzerów

- implicit vs explicit (np. typy)
- centralized vs distributed (np. stan)
- eager vs lazy (np. obliczenia)

## Dobre, złe

Każdy ma nieporównywalne dośiadczenie i inne projekty, dlatego trudno jest decydować czy coś jest lepsze czy gorsze (to jest ocena, nie fakt, argument)
Biznesu nie obchodzi niemutowalność, ani frameworki 😅

moje lepsze, twoje lepsze to nie do końca to samo

dychtonia myślenia - fałszywy podział na dwie możliwość (dobrą i złą)
rzeczywistość jest dużo bardziej skomplikowania

posługuj się konkretne:

- nie lepsze => mniej kodu
- nie lepsze => łatwiej testować
- nie lepsze => mniej skomplikowane
- nie lepsze => bardziej skomplikowane, ale czuję się mądrzejszy :DD

posługując się językiem konkretów szybciej dochodzimy do konsrtruktywnej decyzji

czasami tych słów używa się jako skrótu - dobre pod warunkiem że znamy argumentację za/przeciw

## Design first

Oddziel fazę koncepcyjną od implementacji (Design-first)
kodując myślimy o implementacji a nie o naturze rozwiązywanego problemu

Upewnij się że problem który rozwiązujesz na pewno istnieje
(biznes - kilka krótkich formularzy, a my generyczne formularze dla 5 formularzy)

Jak poświęcamy na coś czas, to nie możemy go poświęcić na coś ważniejszego
YAGNI - you ain't gonna need it

Szukaj więcej niż 1 sposobu na rozwiązanie problemu
=> potem mamy problem coś zaorać (czas w plecy, to rozwiązanie jest gorsze)
=> lepiej znaleść przynajmniej 2 rozwiązania (mamy co porównywać)

Bądź obiektywny wobec technologii. Jeśli jakieś narzędzie nie rozwiązuje naszego problemu, to go nie używajmy
Technologie mają robić swoją robotę:

- hurra optymizm => strona firmowa na Gatsby z GraphQLem zamiast Wordpressa / statycznej stornki
- negatywne emocje => może korzystałem z tego narzędzia źle? może to narzędzie nie rozwiązuje tego problemu? ludzie nie lubią w tym pracować 🤭

## Balans

Kod ma działać, aplikacja ma być stabilna
My chcemy się rozwijać i uczyć się nowych narzędzi (CV Driven Development)

Nie chcemy siedzieć w starych narzędziach jeśli istnieją lepsze.
Nauka wliczona w koszt projektu jest OK pod warunkiem że zachowamy umiar.

## Choose your battles

Refaktoruj nie wszystko na raz. KTóre dadzą największe korzyści - zakończ tematy, potem bierz kolejne

> Perfekcja nie jest wtedy nic więcej nie można dodać ale wtedy, kiedy nic więcej nie można usunąć
