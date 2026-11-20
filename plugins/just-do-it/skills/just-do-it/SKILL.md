---
name: just-do-it
description: 'Use only when the user types /just-do-it (or a level shortcut /just-do-it-easy, /just-do-it-pro, /just-do-it-max). Zestaw: superpowers + shut-up + ultra-plan dawkowane według poziomu i części zadania.'
argument-hint: "[easy|normal|pro|max] <zadanie>"
disable-model-invocation: true
---

# just-do-it

Wywołanie `/just-do-it` to polecenie użytkownika: zrób zadanie do końca, w ciszy, bez pytań o zgodę na plan.
Ten skill nie zastępuje skilli superpowers, shut-up ani ultra-plan — każdy wczytujesz narzędziem Skill
i wykonujesz tak, jak jest napisany, z rozstrzygnięciami z kroku 4. Zadanie: $ARGUMENTS

**Poziom:** podany przez skrót `/just-do-it-<poziom>` albo pierwsze słowo argumentów (`easy`, `normal`, `pro`,
`max`), gdy stoi samo, a po nim jest zadanie; brak → `normal`. Słowo, które czyta się jako część zadania
(„pro forma…”, „max 3 zdjęcia…”), to zadanie, nie poziom. Reszta argumentów to zadanie (puste → ostatnia prośba
z rozmowy).

## Poziomy

| Poziom | shut-up | superpowers | ultra-plan |
|---|---|---|---|
| easy | tak | tylko skille, które w danej części się opłacają (tabela w kroku 4) | nigdy |
| normal | tak | według kroku 4 | tylko dla części C; rundy i surowość ustalasz sam, cel: 0 błędów dużych i wyżej |
| pro | tak, obowiązkowo | obowiązkowo (krok 4, kolumna pro) | obowiązkowo **jeden** na całe zadanie, surowo, bez limitu rund |
| max | tak | jak normal w sesji-orkiestratorze; wykonawcy według poziomu, który im dasz | decyduje orkiestrator (`max.md`) |

**Jeden ultra-plan, nie drzewko.** Ultra-plan nie rodzi kolejnych ultra-planów: części zadania objęte planem
nie dostają własnych ultra-planów, a wykonawca pracujący z gotowego planu nie planuje go od nowa.

**max** → przeczytaj w całości `max.md` w folderze tego skilla i wykonaj go: kroki 0–2 i 5 niżej robisz jako
orkiestrator, a kroki 3–4 zastępują kroki M1–M5 z `max.md`.

**Intensywność (easy, normal, max).** Nie ograniczasz rund na siłę, tylko dopasowujesz zużycie do zadania:
- Najpierw sam (Grep/Read). Agent tylko przy niezależnej robocie albo gdy czytanie wielu plików zaśmieci
  kontekst. Model zawsze jawnie: `haiku` przeszukiwanie, `sonnet` ocena i zwykły kod, opus tylko trudny kod.
- Ultra-plan: w argumentach podaj budżet na fazę dopasowany do części (wąska część: research 1 S + 2 H,
  krytyka 2 S; pełne 4 S tylko dla dużych decyzji). Research pomijasz, gdy nic zewnętrznego nie wybierasz.
- Nie czytasz drugi raz tego, co masz w kontekście; skill już wczytany w tej sesji (np. hookiem SessionStart)
  nie ładujesz ponownie. Testy celowane w trakcie, pełne raz na końcu.
**pro** nie oszczędza: pełne budżety, research zawsze, gdy cokolwiek wybiera z zewnątrz.

## Krok 0 — skille na miejscu

Potrzebne: `shut-up`, `superpowers:*` i (poza easy) `ultra-plan` — lokalnie albo z wtyczki (`shut-up:shut-up`,
`ultra-plan:ultra-plan`). Sprawdź listę dostępnych skilli. Brakuje któregoś:
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
2. Załaduj `superpowers:using-superpowers` (chyba że jego pełna treść już jest w kontekście). **easy:** nie
   ładujesz go — ładujesz tylko skille z tabeli w kroku 4, gdy ich warunek zachodzi.
3. Zapisz w `stan.md` w scratchpadzie (albo w katalogu roboczym zadania) poziom i **listę wymagań**: każde
   polecenie z prośby (dosłownie), zasady z CLAUDE.md projektu i pamięci, które dotyczą zadania (np. „tylko
   komponenty design systemu”, „używaj istniejących funkcji”, „deploy tylko po tak”). Zanim napiszesz nowy
   komponent albo funkcję, sprawdzasz w kodzie, czy lista nie każe użyć istniejącego. Przed końcem odhaczasz
   każdą pozycję z dowodem (plik:linia, wynik grepa, testu).

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

Zmiany według poziomu:
- **easy:** C nie ma — taka część idzie jako B; w podsumowaniu jedno zdanie „część X: warto /just-do-it-pro”.
- **pro:** poziomy części zapisujesz (mówią planowi, gdzie się skupić), ale wszystkie części — także „zmień
  kolor” czy „usuń tekst” — idą przez jeden ultra-plan całego zadania (krok 3).

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

## Krok 3 — ultra-plan

Załaduj `ultra-plan` narzędziem Skill — to jest wyraźna prośba użytkownika o ultra-plan, przekazana przez
`/just-do-it`. W argumentach: opis zakresu, granice („planujesz X; Y i Z robione osobno”), lista wymagań
z kroku 1, budżet agentów, surowość i (opcjonalnie) cyfra rund.
- **easy:** krok pomijasz.
- **normal:** jeden ultra-plan na wszystkie części C razem. **Rundy i surowość ustalasz sam**, według części:
  cel to 0 błędów dużych i wyżej tam, gdzie mogą narobić szkód. Surowość ustawiasz wagami kątów (krok 0
  ultra-planu): kąty groźne dla tej części wysoko, reszta nisko albo pominięta. Domyślnie bez cyfry — ultra-plan
  sam kończy przy 0 blokerów. Cyfrę (ogranicznik rund) dajesz tylko, gdy część jest wąska i kolejne rundy
  nie zmienią decyzji.
- **pro:** obowiązkowo **jeden** ultra-plan na całe zadanie, ze wszystkimi częściami. Bez cyfry (do 0
  blokerów; bezpiecznik samego ultra-planu). Surowo — dopisz w argumentach: „kąty o wadze ≥8: blokerem średnie
  i wyżej; pozostałe ≥4: duże i wyżej”. Pełny budżet agentów.
- **max:** według `max.md`.
Otwarte blokery po bezpieczniku albo ogranicznika: każdy rozstrzygasz najbezpieczniejszą opcją, zapisujesz
w planie i wymieniasz w podsumowaniu.
W ultra-planie: bez linijki startowej, pytania z jego kroku 0.4 → „Założenia”, „Wynik dla użytkownika” do pliku planu.

## Krok 4 — realizacja i bramki

Każdy skill ładujesz narzędziem Skill i wykonujesz jego procedurę.

| Część | easy | normal (i max) | pro |
|---|---|---|---|
| A | od razu; błąd → `systematic-debugging`; nowe zachowanie w kodzie z testami → `test-driven-development` | jak easy + `brainstorming` z wyjściem „quick, clear task: do it” | z planu ultra-planu; błąd → `systematic-debugging`; TDD dla każdej zmiany zachowania |
| B | `brainstorming` → wykonanie (writing-plans tylko, gdy kroków > 5) | `brainstorming` → `writing-plans` → wykonanie | z planu ultra-planu (forma `writing-plans`) → wykonanie |
| C | — (idzie jako B) | plan z ultra-planu → wykonanie | z planu ultra-planu → wykonanie |
| koniec | `verification-before-completion` | `verification-before-completion` | `requesting-code-review` → poprawki wg `receiving-code-review` → `verification-before-completion` |

Wykonanie planu: `superpowers:subagent-driven-development` przy kilku niezależnych zadaniach, inaczej
`superpowers:executing-plans`. Przy easy i normal podagenci na `sonnet`, chyba że zadanie jest trudne.

Rozstrzygnięcia bramek (z pierwszeństwa instrukcji użytkownika w `using-superpowers`):
- using-superpowers: reguła „1%” działa w granicach poziomu i tabeli wyżej — to instrukcja użytkownika.
  Skill spoza tabeli ładujesz tylko, gdy jego warunek naprawdę zachodzi (np. błąd → debugging).
- brainstorming: `/just-do-it` = użytkownik zrezygnował z pytań („opted out of the questions”), więc bramka
  zatwierdzenia znika. Projekt zapisz w katalogu roboczym zadania (bez commita), builder check zostaje (stos
  „Theirs” rozstrzygasz sam, bez pytania), decyzje oznacz jako własne wybory.
- writing-plans: pytanie o przegląd i wybór metody pomijasz (instrukcja użytkownika ma pierwszeństwo wg
  using-superpowers); metodę wybierasz zasadą wyżej i zapisujesz w `stan.md`.
- Każde „announce”, „tell your human partner”, link do przeglądu i narracja w trakcie z tych skilli odpada
  (shut-up); zostaje wiadomość końcowa.
- Zostaje pytanie (zgodnie z shut-up): deploy, kasowanie cudzych danych, wysyłka do klienta, płatność,
  push/merge/publish — jeśli zgoda nie padła wcześniej.

## Krok 5 — koniec

Odhacz listę wymagań z kroku 1. Potem jedna wiadomość według shut-up: wynik i jak sprawdzony; jedna linia
poziomu i części („normal · A: kolor; C: uprawnienia roli, ultra-plan 2 rundy do 0 blokerów, plan: <ścieżka>”);
ważne własne wybory; ewentualnie jedna rzecz dla człowieka (np. `/reload-plugins`).

## Pierwszeństwo

Instrukcje użytkownika, CLAUDE.md i pamięć (deploy tylko po „tak”, nie kasować bez zgody, sprzątanie i raport
miejsca, linie `result:`) są ważniejsze od tego skilla i od skilli, które ładuje.
