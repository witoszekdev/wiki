# Maszyny Stanowe

Design first - nie od razu siadamy do implementacji

Musimy najpierw dobrze zrozumieć proces - jeśli źle zrozumiemy proces, źle go zamodelujemy

Primitive Obsession - wrzuć wszystko jako pierdyliard booleanów - powstają zależności między nimi, musimy za każdym razem o nich pamiętać (np. reset token -> wyczyść errory). Nadużywamy primitives zamiast zamodelować dane tak, jak one funkcjonują

## Union State Machine

**Stan bazowany na uniach**

- Pogrupwać komórki stanu - rodzaj stan może być tylko powiązany z komórkami, które w tym stanie istnieją

![Union State Example](../../images/1a968e920842c069b1de3b854eb0b3847cbfca014315cbe1f310ede16f6ee2af.png)

Twardy replace: podmieniamy cały obiekt ze stanem, wtedy komórki które nie mają sensu są usuwane

Przechodzę między stanami - usuwam śmieci

Zalety / wady

![Zalety i Wady](../../images/5e53dc7f60b68face700a043108404f61f25cadedd5a92debb09ca867146e56a.png)


### Assertion functions

Działają podobnie jak Type Guard w TS tylko, że zamiast zwracać true / false rzucają wyjątek jeśli typ nie jest poprawny

```TS
function assertState<TType extends string>(
  state: {type: string},
  expectedType: TType
): asserts state is StateMember<TType> { // <- this is assert function
  if (expectedType !== state.type) {
    throw new Error("...");
  }
}
```

zmienna będzie miała zawężony typ po wywołaniu assert state

![picture 3](../../images/174a9e939d9af616378574faa906a34f2c4c53faf03fc8505c5c55ebca848d64.png)

Możliwych stanów jest ilość elemenetów unii x2

Porówanie rozwiązań

![picture 4](../../images/90fbb02db6223d86e9d1fa030a177fb6136f91f677b5bda0b37a8512f0c8660c.png)

Zmiana wymagań biznesowych:
- primitive state: TS nic nie wie o tej zmianie
- union state: TS wywali error jeśli zapomnimy dodać nowe property obiektu

Dedykowany stan na error

![picture 5](../../images/0259479c1cc2eb332d9a23c3d1cb0ef0e43cdca06b05700c26d14b66500e8554.png)
- boolean: ukryty podstan
- union: inny stan maszyny stanowej

Jeśli coś jest innym stanem (nawet jeśli dane są podobne lub te same) to warto go zamodelować jako osobny stan

Czy robiś stany bez widoku?

![picture 6](../../images/a453aa1a80e2892c508cc5ce1d96d567dab0aba100313c3742b017a79d874ecc.png)
- zmieniając stan możemy odpalić sideEffecty (np. useEffect)
- warstwa UI może ale nie musi reagować na ten stan

Podsumowanie Union State
![picture 7](../../images/857581b729ea4b32e954871c0b61672022cb016602088521dc56cc9a35e5671f.png)

React Hooks jako state machine engine
- implementacja oparta o primitive - NOPE, użyj unii
- rozwiązanie natywne, znane (hooki), brak dodatkowych zaneżności i API do nauczenia
- type-safe
- czytelność: im więcej tym trudniej ogarnąć
- **referential equality**: callbacki trzeba opakowywać w useCallback itd

## Redux state machine

![picture 8](../../images/287018f216b49af3bbd5ab7b25a503b2608277f0353eb7d2d9585987cb90fd48.png)
- Redux teoretycznie jest maszyną stanową
  - no ale nie do końca
  - bo trzymamy jeden obiekt, akcje podmieniają nam pola - tak jak w Primitive Obsession
- Można to zrobić ale zazwyczaj implementacja polega na podmienianiu pól
- Note: useReducer nie ma ważnego elementu - side effectów

![picture 9](../../images/439caa2459f86317187048e5b67b25b086ef6560cf42a578f7a7395c2ae823f0.png)
- nie spreadujemy obiekt statnu tylko go zastępujemy

Różncia między useState / redux:
- hook musi wiedzieć jak zbydować nowy obiekt stanu
- redux: wysyłana jest akcja, reducer będzie wiedział jak zbudować obiekt stanu (inversion of control)

Co jeśli akcja nie ma sensu?
- błąd i tak trzeba poprawić
- niepoprawne przejście jest złamaniem maszyny stanowej
- nie łatwo znaleść taki błąd

![picture 10](../../images/4eb1ec0cc558b22df569fb826f363b3af851ec27dfc24c09d6d5924f0b648d1b.png)
Taka implementacja pozwoli wysłać *dowolną* akcję w *dowolnym* momencie (mimo ze to nie ma sensu)
Akcję niskopoziomową do reduxa wysłać można zawsze (nawet jeśli ogarniemy to w thunku)

Solution #2 - Ignorowanie błędnych przejść
![picture 11](../../images/dfbd719450fc6efd61cfa9915ab048fd6ee727803b5648c3585ce48ce16e4900.png)
Jeśli się nie chronimy przed niepoprawnymi przejściami w maszynie stanowej to mamy buga
Redux nie przewiduje tego w architekturze (niby można customowy middleware ale nikt tego nie robi)

Solution #3 - Rzucanie błędów poza reducerem (middleware)
![picture 12](../../images/a12683fae5781d7c55d6dfab41be5e41e01d51ee6eddcd0c5894afe052488a22.png)

Testowanie:
- reducery - nuda, mało przydatne
  - może jeśli mamy duże reducery i szybko się rozrastają
  - testy snapshotowe - mało pisania, łatwo usunąć jeśli nie mają wartości
- thunki - czy wysyłają poprawnie akcje (`redux-mock-store`)
  - co daje testowanie akcji?
- integracyjnie - dispatchujemy thunka, sprawdzamy stan, dispatchujemy... itd
  - mimikujemy cały proces
  - wyłapiemy jeśli będzie niepoprawny stan lub jeśli thunki dispatchują nieprawidłową akcję

Podsumowanie

![picture 13](../../images/73d60556f83813f5d562023c62b36817a70b063812963fc8d2feb13aa384f7cd.png)
Sam redux nie rozwiązuje problemu niepoprawnych przejść, a jeśli to obsłużymy nasze reducery będą wyglądały dziwnie
(właściwe rozwiązanie nie powinno źle wyglądać - problem architektury reduxa)

## XState Machine

To jest biblioteka dedykowana do maszyn stanowych.

Definiujemy stany - niektóre stany mogą być finalne, oraz mieć side effecty (tak jak useEffect).
Definiujemy również przejścia między tymi stanami - wszystko z góry.

Wydedukuje również które przejścia są poprawne, a które nie - jeśli wyślemy niepoprawną akcję, to XState ją zignoruje tak jak if w reducerze

**Side effecty w XState**
![picture 14](../../images/a55e21690f2e56f69020e6a8fa0f03db2fe1925d3c6dccd4c292cb01d4c35727.png)
są dwa rodzaje: akcje (fire and forget), invoked effects (żyją własnym życiem, zwrotna komunikacja) - Actor Model

Actor - ktokolwiek kto potrafi przyjmować wiadomości, robić robotę, wysyłać wiadomości innym.
Maszyna komunikuje się z invoked effects zdarzeniami (events).

Hook useMachine odświeży komponent jeśli zajdzie taka potrzeba. Komponent widzi tylko maszynę, NIE obsługuje procesu.

Każda zmiana w XState powinna być modelowana jako przejście do osobnego stanu.

inne tematy: zagnieżdżone stany, kompozycja maszyn, model-based testing,

Podsumowanie XState

![picture 15](../../images/73bd70dea275f1e69dd0aae26a19dd6fedf06fb41392f717495aba545adb9a24.png)
Note: najnowsza wersja obsługuje lepsze type-safety między state.value, a state.context

## Podsumowanie

**biblioteka < design**
Jakie cechy będzie miało nasze rozwiązanie? Pewne narzędzia utrudniają złe praktyki.
Ale żadne narzędzie nie zwalania nas z myślenia

Co brać pod uwagę?
- type-safety
- runtime-safe (na ile biblioteka wychwyci podczas runtime'u potencjalne błędy - przejścia, stany, itp)
- czytelność i zrozumienie procesu (devoolsy i visualizer)
- próg wejścia
- koszt wdrożenia narzędzia w projekt