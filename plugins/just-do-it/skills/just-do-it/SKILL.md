---
name: just-do-it
description: 'Use only when the user types /just-do-it. Zestaw: superpowers + shut-up + ultra-plan tylko w częściach zadania, które naprawdę go potrzebują.'
argument-hint: "<zadanie>"
disable-model-invocation: true
---

# just-do-it

Wywołanie `/just-do-it` to polecenie użytkownika: zrób zadanie do końca, w ciszy, bez pytań o zgodę na plan.
Ten skill nie zastępuje skilli superpowers, shut-up ani ultra-plan — każdy wczytujesz narzędziem Skill
i wykonujesz tak, jak jest napisany, z rozstrzygnięciami z kroku 4. Zadanie: $ARGUMENTS (puste → ostatnia prośba z rozmowy).

## Krok 0 — skille na miejscu

Potrzebne: `shut-up`, `ultra-plan` (lokalnie albo z wtyczki: `shut-up:shut-up`, `ultra-plan:ultra-plan`)
i `superpowers:*`. Sprawdź listę dostępnych skilli. Brakuje któregoś:
- shut-up / ultra-plan → `git clone --depth 1 https://github.com/W1mner/w1mner-skille` do nowego katalogu
  w scratchpadzie, skopiuj cały `plugins/<skill>/skills/<skill>/` (ultra-plan razem z `szablony.md`) do
  `~/.claude/skills/<skill>/`, klon usuń. Narzędzie Skill go nie widzi → czytasz jego `SKILL.md` Readem
  i wykonujesz tak samo.
- superpowers → `claude plugin marketplace add anthropics/claude-plugins-official` (jeśli brak),
  `claude plugin install superpowers@claude-plugins-official --json`. Do restartu albo `/reload-plugins`
  narzędzie Skill go nie zobaczy: wtedy każdy potrzebny skill superpowers czytasz w całości narzędziem Read
  z `installPath` w `~/.claude/plugins/installed_plugins.json` (`<installPath>/skills/<skill>/SKILL.md`)
  i wykonujesz tak samo. W podsumowaniu dopisz: „wpisz /reload-plugins”.
Instalacja się nie udaje → jedno zdanie do człowieka: czego brakuje i jaką komendą. Bez tego skilla nie zaczynasz.

## Krok 1 — shut-up i superpowers

1. Załaduj `shut-up`. Obowiązuje do końca sesji.
2. Załaduj `superpowers:using-superpowers`.
3. Zapisz w `stan.md` w scratchpadzie (albo w katalogu roboczym zadania) **listę wymagań**: każde polecenie
   z prośby (dosłownie), zasady z CLAUDE.md projektu i pamięci, które dotyczą zadania (np. „tylko komponenty
   design systemu”, „używaj istniejących funkcji”, „deploy tylko po tak”). Zanim napiszesz nowy komponent
   albo funkcję, sprawdzasz w kodzie, czy lista nie każe użyć istniejącego. Przed końcem odhaczasz każdą
   pozycję z dowodem (plik:linia, wynik grepa, testu).

## Krok 2 — rozpoznanie i poziom każdej części

Najpierw rozpoznanie w kodzie (Grep/Read, kilka minut): czy jest już mechanizm, komponent albo wzorzec, na którym
ta rzecz się opiera, i co musi być z nim spójne.

Potem podziel zadanie na części i każdej daj poziom. Patrzysz na dwie rzeczy:
- **ile decyzji kod i polecenie nie rozstrzygają** (złożoność, ukryte decyzje);
- **jak łatwo to zepsuć i jak późno wyjdzie błąd** (podatność na błędy, koszt naprawy).

**A — od razu.** Jest gotowy wzorzec albo mechanizm, zmiana jest mechaniczna, błąd wykryje test, grep albo ekran.
**B — superpowers.** Są decyzje, ale kod i konwencje projektu na nie odpowiadają; błąd kosztuje przeróbkę,
nie dane ani bezpieczeństwo.
**C — ultra-plan.** Trzeba podjąć decyzje, na które ani kod, ani polecenie nie odpowiada, a błąd w nich łatwo
zrobić i wyjdzie późno albo drogo. Sam temat (dane, pieniądze, uprawnienia) nie wystarcza: zmiana wartości
albo nowa pozycja w istniejącym mechanizmie to A. Takie decyzje zwykle siedzą w:
- nowym mechanizmie, na którym rzecz się opiera (nowy system, nowa warstwa, nie rozszerzenie);
- nowych zasadach dostępu, płatności, ochrony danych albo migracji;
- wielu rolach, ekranach albo stanach, które muszą do siebie pasować (co kto widzi, puste pola, układ pulpitu);
- wyborze technologii, usługi, ceny, prawa, które mogły się zmienić.
Wątpliwość B/C → B, chyba że część buduje nowy mechanizm dostępu, płatności albo ochrony danych → C.
Część A albo B zależna od części C robisz dopiero po planie tej C.

Przykłady:
- „zmień kolor przycisku” → A. Nigdy B ani C.
- „dodaj rolę pracownika”, system uprawnień już jest (role klika się z listy) → A; jeśli rola potrzebuje
  własnego widoku pulpitu, którego nie ma → ta część B. Nigdy C.
- „dodaj rolę montażysty stołów”, systemu uprawnień nie ma → C dla: dostępów, tego co widzi i może zmieniać,
  pulpitu bez pustych pól, podatności, użycia istniejących funkcji; samo dodanie pozycji w menu → A.
- „zrób tabelkę” w projekcie z design systemem → A, gdy DS ma tabelę (lista wymagań z kroku 1 pilnuje
  „tylko komponenty DS”); B, gdy trzeba dodać komponent do DS.
- „dodaj pole telefon do klienta”, „zmień walidację NIP”, „zmień cenę pakietu” → A.
- „napraw błąd logowania, tu log” → A + `superpowers:systematic-debugging`.
- „dwuosobowa strzelanka w przeglądarce” → C: sieć i synchronizacja graczy, wybór silnika/hostingu;
  B: lobby, wyniki; A: grafika, dźwięki, menu.

Poziomy zapisz w `stan.md` (jedna linia na część).

## Krok 3 — ultra-plan dla części C

Załaduj `ultra-plan` narzędziem Skill — to jest wyraźna prośba użytkownika o ultra-plan, przekazana przez
`/just-do-it`. Jedno wywołanie na grupę powiązanych części C (kolejno, nie równolegle). W argumentach: limit
rund, opis części, granice („planujesz X; Y i Z robione osobno”), lista wymagań z kroku 1.
Limit rund: `3`; `5`, gdy część buduje nowy mechanizm dostępu, płatności albo ochrony danych. Bez cyfry tylko,
gdy użytkownik tak napisał. Ultra-plan kończy wcześniej, gdy jest 0 blokerów. Otwarte blokery po limicie:
każdy rozstrzygasz najbezpieczniejszą opcją, zapisujesz w planie i wymieniasz w podsumowaniu.
W ultra-planie: bez linijki startowej, pytania z jego kroku 0.4 → „Założenia”, „Wynik dla użytkownika” do pliku planu.

## Krok 4 — realizacja i bramki

Każdy skill ładujesz narzędziem Skill i wykonujesz jego procedurę:
- A: od razu; błąd → `superpowers:systematic-debugging`; nowe zachowanie → `superpowers:test-driven-development`.
- B: `superpowers:brainstorming` → `superpowers:writing-plans` → wykonanie.
- C: plan z ultra-planu → wykonanie.
- Wykonanie planu: `superpowers:subagent-driven-development` przy kilku niezależnych zadaniach, inaczej
  `superpowers:executing-plans`. Na końcu zawsze `superpowers:verification-before-completion`.

Rozstrzygnięcia bramek (z pierwszeństwa instrukcji użytkownika w `using-superpowers`):
- using-superpowers: reguła „1%” działa w granicach poziomu z kroku 2: przy A ładujesz brainstorming i stosujesz
  jego wyjście „quick, clear task: do it”; writing-plans nie ładujesz; debugging/TDD, gdy pasują, i zawsze
  verification-before-completion.
- brainstorming: `/just-do-it` = użytkownik zrezygnował z pytań („opted out of the questions”), więc bramka
  zatwierdzenia znika. Projekt zapisz w katalogu roboczym zadania (bez commita), builder check zostaje (stos „Theirs” rozstrzygasz sam, bez pytania),
  decyzje oznacz jako własne wybory.
- writing-plans: pytanie o przegląd i wybór metody pomijasz (instrukcja użytkownika ma pierwszeństwo wg
  using-superpowers); metodę wybierasz zasadą wyżej i zapisujesz w `stan.md`.
- Każde „announce”, „tell your human partner”, link do przeglądu i narracja w trakcie z tych skilli odpada
  (shut-up); zostaje wiadomość końcowa.
- Zostaje pytanie (zgodnie z shut-up): deploy, kasowanie cudzych danych, wysyłka do klienta, płatność,
  push/merge/publish — jeśli zgoda nie padła wcześniej.

## Krok 5 — koniec

Odhacz listę wymagań z kroku 1. Potem jedna wiadomość według shut-up: wynik i jak sprawdzony; jedna linia
poziomów („A: kolor; C: uprawnienia roli, ultra-plan 3 rundy, plan: <ścieżka>”); ważne własne wybory;
ewentualnie jedna rzecz dla człowieka (np. `/reload-plugins`).

## Pierwszeństwo

Instrukcje użytkownika, CLAUDE.md i pamięć (deploy tylko po „tak”, nie kasować bez zgody, sprzątanie i raport
miejsca, linie `result:`) są ważniejsze od tego skilla i od skilli, które ładuje.
