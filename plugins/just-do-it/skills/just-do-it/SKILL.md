---
name: just-do-it
description: 'Use only when the user types /just-do-it (or says "just do it" as a command to work this way). Zestaw: superpowers + shut-up + ultra-plan tylko tam, gdzie się opłaca.'
argument-hint: "<zadanie>"
disable-model-invocation: true
---

# just-do-it

Opakowanie trzech rzeczy: metodyki **superpowers**, trybu **shut-up** i **ultra-plan** — ten ostatni tylko w tych częściach zadania, które naprawdę tego potrzebują. Wywołanie `/just-do-it` jest zgodą na pracę bez pytań: robisz zadanie do końca.

Zadanie: $ARGUMENTS (puste → ostatnia prośba z rozmowy).

## Krok 0 — prawdziwe skille, nie zamienniki

Ten skill nie zastępuje tamtych trzech. Każdy z nich wczytujesz w całości i stosujesz dokładnie tak, jak jest napisany. Nie streszczasz ich, nie odtwarzasz z pamięci i nie robisz „wersji uproszczonej”. Wymagane:

| Skill | Skąd |
|---|---|
| `shut-up` | marketplace `W1mner/w1mner-skille`, wtyczka `shut-up` (albo lokalnie `~/.claude/skills/shut-up`) |
| `ultra-plan` | marketplace `W1mner/w1mner-skille`, wtyczka `ultra-plan` (albo lokalnie `~/.claude/skills/ultra-plan`) |
| `superpowers:*` | marketplace `anthropics/claude-plugins-official`, wtyczka `superpowers` |

Sprawdź na liście dostępnych skilli, czy każdy jest. **Brakuje któregoś → zainstaluj go**, zanim zaczniesz zadanie:

```
claude plugin marketplace add W1mner/w1mner-skille
claude plugin install shut-up@w1mner-skille
claude plugin install ultra-plan@w1mner-skille
claude plugin marketplace add anthropics/claude-plugins-official
claude plugin install superpowers@claude-plugins-official
```

(Instalujesz tylko brakujące; „already added/installed” = OK.) Świeżo zainstalowana wtyczka może być niewidoczna dla narzędzia Skill do restartu sesji. Wtedy czytasz jej plik `SKILL.md` z `~/.claude/plugins/cache/<marketplace>/<wtyczka>/<wersja>/skills/<skill>/SKILL.md` (narzędziem Read, w całości) i wykonujesz go tak samo, jakby wczytało go narzędzie Skill. Nie da się zainstalować (brak sieci, odmowa) → to jest prawdziwa blokada: jedno zdanie do człowieka, czego brakuje i jaką komendą to doinstalować. Nie jedziesz dalej bez tego skilla.

## Krok 1 — włącz shut-up i superpowers

1. Załaduj `shut-up` narzędziem Skill (nazwa `shut-up` albo `shut-up:shut-up`). Od tej chwili obowiązują jego zasady: cisza w trakcie, pytanie tylko przy prawdziwej blokadzie, na końcu jedno krótkie podsumowanie.
2. Załaduj `superpowers:using-superpowers` narzędziem Skill i trzymaj się jego reguły: przed każdym typem pracy wczytujesz pasujący skill superpowers (krok 4).

## Krok 2 — oceń, czy i gdzie potrzebny jest ultra-plan (bez agentów, w głowie)

Rozbij zadanie na części (np. „architektura sieciowa”, „UI lobby”, „kolor przycisku”). Dla **każdej części** sprawdź pytania poniżej. Ultra-plan dostaje tylko część, która ma **co najmniej dwa „tak”**:

1. Czy to nowy system albo przebudowa od zera (kilka modułów, nowe API, nowy model danych)?
2. Czy trzeba wybrać technologię, bibliotekę, usługę albo cennik, który mógł się zmienić od mojej wiedzy (wersje, limity, ceny, prawo)?
3. Czy zły wybór będzie drogi do odkręcenia (migracja danych, prod, pieniądze, bezpieczeństwo, umowa z klientem)?
4. Czy wymagania są szerokie albo niejasne, a zgadnięcie źle oznacza przerobienie dużej części pracy?
5. Czy ta część zajmie więcej niż ok. 2 godziny pracy?

**Nigdy ultra-plan** dla: zmiany wyglądu (kolor, odstęp, tekst), poprawki jednego pliku, błędu z jasną przyczyną, zmiany konfiguracji, zadania na mniej niż ok. 30 minut. Nawet gdy całe zadanie jest duże, te jego części robisz od razu, bez planowania.

Przykłady:
- „zmień kolor przycisku” → 0 × tak → bez ultra-plan, po prostu zmieniasz.
- „napraw błąd logowania, tu jest log” → bez ultra-plan; superpowers:systematic-debugging.
- „zrób dwuosobową strzelankę w przeglądarce z łączeniem graczy” → ultra-plan dla: sieci/synchronizacji graczy (1, 2, 3, 5) i wyboru silnika/hostingu (2, 3); bez ultra-plan: menu, grafika, dźwięki, ekran wyników.

Wynik oceny zapisz jedną linią w notatkach roboczych (np. `stan.md` w scratchpadzie), nie w wiadomości do człowieka.

## Krok 3 — ultra-plan tylko dla wybranych części

Dla każdej wybranej części (albo jednej grupy, gdy części są ze sobą mocno związane) uruchom skill `ultra-plan` narzędziem Skill (nazwa `ultra-plan`, z wtyczki: `ultra-plan:ultra-plan`), a w argumentach podaj **tylko tę część** i granice: „planujesz X; Y i Z są poza zakresem, robione bez planu”.

Limit rund krytyki (pierwsze słowo argumentów):
- `1` — część średnia (2 „tak”);
- `3` — część duża (3–4 „tak”);
- bez cyfry — tylko gdy błąd jest bardzo drogi (prod z danymi klientów, pieniądze, bezpieczeństwo).

W trybie shut-up: pomiń linijkę startową ultra-planu dla człowieka i jego pytania (założenia wpisz w plan jako „Założenia”). Po planie od razu realizujesz — nie czekasz na zgodę, bo `/just-do-it` nią jest.

## Krok 4 — realizacja według superpowers

Każdy z tych skilli ładujesz narzędziem Skill i wykonujesz jego procedurę, a nie jej przybliżenie:
- błąd / test nie przechodzi → `superpowers:systematic-debugging`;
- nowy kod albo zmiana zachowania → `superpowers:test-driven-development`;
- plan z kilkoma niezależnymi zadaniami → `superpowers:writing-plans`, potem `superpowers:subagent-driven-development`;
- przed ogłoszeniem „gotowe” → `superpowers:verification-before-completion` (zawsze).

**Zmiana względem superpowers:** `superpowers:brainstorming` i inne bramki „człowiek zatwierdza projekt” traktujesz jako zatwierdzone przez `/just-do-it`. Projekt przemyśl i zapisz, jak każe skill, ale nie czekaj na zgodę. Wyjątek: akcja nieodwracalna lub na zewnątrz (deploy na prod, kasowanie cudzych danych, wysyłka do klienta, płatność), na którą zgoda nie padła — wtedy pytasz, zgodnie z shut-up.

Małe zadanie nie potrzebuje ciężkich procedur: zmiana koloru to edycja + szybkie sprawdzenie, bez planów i podagentów.

## Krok 5 — koniec

Jedna wiadomość według shut-up (do 5 linii): co zrobione i jak to sprawdzono; jednym zdaniem, czy i dla czego użyto ultra-planu (np. „ultra-plan: sieć gry, 3 rundy; reszta bez planu”); ścieżka do planu, jeśli powstał; ewentualnie jedna rzecz do zrobienia przez człowieka.

## Pierwszeństwo

Instrukcje użytkownika i CLAUDE.md projektu (np. „deploy tylko po tak”, „nie kasować bez zgody”) są ważniejsze od tego skilla. `/just-do-it` nie zwalnia z nich.
