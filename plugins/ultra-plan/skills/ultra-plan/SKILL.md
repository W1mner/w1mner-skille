---
name: ultra-plan
description: Use only when the user explicitly asks for ultra-plan by name — "/ultra-plan", "ultra plan", "zrób ultra-plan", "użyj ultra-plan" (also via /ultra-plan-1, -3, -5) — i.e. a deep, current research + plan + critique loop before answering or implementing. Do not use for ordinary "make a plan" requests.
argument-hint: "[maks. rund] <zadanie>"
---

# Ultra-plan: research → plan → krytyka z wagami kątów

Plan powstaje po świeżym researchu i przechodzi rundy krytyki, aż krytyka NAJNOWSZEJ wersji nie znajdzie
blokerów. Ty (sesja główna) jesteś orkiestratorem: oceniasz, co w tym zadaniu jest ważne, rozdzielasz pracę,
syntetyzujesz, poprawiasz plan. Agenci badają i krytykują — nie edytują plików projektu i nie uruchamiają
własnych podagentów. Katalog kątów i szablony briefów: `szablony.md` w tym samym katalogu co ten plik.

## Parametry
- **Rundy krytyki: tyle, ile trzeba do celu — 0 dużych, 0 bardzo poważnych, 0 krytycznych.** Bez cyfry =
  wersja najmocniejsza: bez limitu rund (bezpiecznik: 6). Cyfra = **ogranicznik** R, czyli maks. liczba rund;
  rund może być mniej, gdy cel osiągnięty wcześniej. Jeśli pierwsze słowo argumentów to sama cyfra, to jest R,
  a reszta to zadanie. Skróty `/ultra-plan-1/-3/-5` podają R same. Zadanie puste → ostatnia prośba z rozmowy.
- **Budżet agentów na fazę = 4 jednostki Sonnet.** 1 Sonnet możesz wymienić na 2–3 Haiku (np. 4 S, albo
  2 S + 6 H, albo 1 S + 9 H). Pełnego budżetu nie musisz zużyć — małe zadanie = mniej agentów. Model zawsze
  jawnie: `model: "sonnet"` / `model: "haiku"`. Agenci jednej fazy idą RÓWNOLEGLE w jednej wiadomości.
- **Kto czym:** Sonnet — ocena i wnioski (najważniejsze kąty krytyki, porównanie rozwiązań, architektura, prawo).
  Haiku — przeszukiwanie konkretnego źródła (GitHub, YouTube, docs, changelogi, cenniki) i krytyka w kątach
  średniej wagi. Znalezisko Haiku „duże” i wyżej sprawdzasz sam (dowód, cytat) zanim uznasz je za bloker.
- **Data:** ustal dzisiejszą (z kontekstu albo `date`) i wpisuj ją w KAŻDY brief.
- Na starcie jedna linijka dla użytkownika: „ultra-plan do 0 blokerów (maks. R rund / bez limitu), research: … S + … H, krytyka: … S + … H na rundę,
  top kąty: …”. Nie pytaj o zgodę — wywołanie skilla nią jest.
- Pliki robocze: `<scratchpad sesji>/ultra-plan-<temat>/` (brak scratchpada → `<projekt>/.ultra-plan/<temat>/`),
  w briefach ścieżki BEZWZGLĘDNE. **Plan końcowy zapisz trwale w projekcie** (np. `docs/plany/`).
- **Stan:** `stan.md` w katalogu roboczym: wagi kątów, runda, wersja planu, budżet słów B i słowa każdej wersji,
  ID krytyków z ich kątami, rejestr znalezisk (ID · zdanie · kąt · ważność · status), zmienione fragmenty. Aktualizuj po każdej fazie; po kompakcji czytaj go zamiast zgadywać.
- Agent bez wyniku po ~15 min → pomiń go i zapisz to w wyniku.

## Krok 0 — rozpoznanie i wagi (bez agentów)
1. Przeczytaj, co już jest: pliki projektu, notatki, ustalenia z rozmowy, pamięć.
2. Wypisz: cel, ograniczenia użytkownika (dosłownie), czego nie wiesz, co może być nieaktualne.
3. **Oceń każdy kąt z katalogu (`szablony.md`) w skali 1–10 dla TEGO zadania** i dopisz własne, jeśli brakuje.
   „Zgodność z poleceniem” = zawsze 10. Wagi zapisz w `stan.md`:
   - **1–3:** pomijasz całkiem (nie trafia do żadnego agenta).
   - **4–5:** sprawdzany, ale liczą się tylko rażące błędy (krytyczne / bardzo poważne).
   - **6–7:** blokerem jest duże i wyżej.
   - **8–10:** blokerem jest duże i wyżej; średnie naprawiasz, gdy tanie.
   Przykład: animacja → muzyka, SFX, spójność dźwięku, historia, styl = 8–10; architektura kodu, niepotrzebny
   kod = 1–3. Moduł aplikacji → architektura, błędy logiczne, dane = 9–10; dźwięk, grafiki = 1.
4. Pytania (maks. 3) tylko gdy odpowiedź zmieni wagi albo kąty researchu ORAZ nic nie mówi, że użytkownika nie
   ma lub żeby nie pytać. W innym razie założenia oznacz w planie jako „Założenia”.
5. Research pomijasz TYLKO, gdy zadanie dotyczy wyłącznie lokalnego kodu/danych, które już przeczytałeś,
   i niczego zewnętrznego nie wybiera. Napisz wtedy: „research pominięty, bo …”.

## Rozdział kątów między agentów (research i każda runda krytyki)
Posortuj kąty o wadze ≥4 malejąco i rozdaj je **jak karty, wężykiem** (agent 1, 2, …, n, n, …, 2, 1, 1, 2…),
tak żeby każdy agent miał coś z góry listy i coraz mniej ważne. Każdy agent dostaje kilka kątów z wagami
i wie, który jest dla niego najważniejszy. Kąty 9–10 możesz dać dwóm agentom (w tym co najmniej jednemu Sonnet).
W rundach 2+ rozdanie się NIE zmienia: ci sami krytycy, te same kąty (patrz Faza 3).

## Faza 1 — research
- Te same wagi, ale rozdajesz **tematy × źródła**. Haiku: jedno-dwa źródła na agenta (GitHub, YouTube, oficjalne
  docs i changelogi, cenniki, Hugging Face, fora/Reddit, marketplace'y skilli/wtyczek, lokalne pliki) dla
  tematów o wadze ≥6 — zbiera fakty z URL i datą, bez rekomendacji. Sonnet: najważniejsze tematy w głąb
  i porównanie kandydatów. Brief = szablon „Research” (wersja S albo H).
- Jeśli jest skill `agent-reach`, agenci go używają; inaczej WebSearch/WebFetch.
- Raport bez dat przy faktach → odeślij do uzupełnienia (najwyżej raz). Sam zweryfikuj 2–3 fakty, na których
  plan najbardziej polega. Zapisz `research.md` (streszczenie ze źródłami i datami).

## Faza 2 — plan v1
`plan-v1.md`: **Cel · Założenia · Ustalenia z researchu (źródło + data) · Plan realizacji (kroki, kolejność,
kto/co) · Koszty · Ryzyka · Kryteria „gotowe” · Otwarte pytania.** Zadanie „wybierz / porównaj” → zamiast planu
realizacji: **Kryteria wyboru · Macierz kandydatów (z datami) · Rekomendacja · Plan B · Jak wdrożyć.**
**Na samej górze planu dwie rzeczy, które obowiązują we wszystkich wersjach:**
- **Granice zakresu** (3–6 punktów): co plan obejmuje i czego NIE obejmuje, wyprowadzone z polecenia
  i założeń. Np. „złożyć biurko z paczki, którą użytkownik ma/zamówi; NIE: wybór mebli, transport, inne meble”.
  Granic nie zmieniasz w trakcie rund — to może tylko użytkownik.
- **Budżet słów:** B = słowa całego planu v1 (razem z Granicami i załącznikami) × 1,3. B zapisujesz tylko
  w `stan.md`, nie w planie. Każda wersja ≤ B. Jedna wersja może
  urosnąć najwyżej o 10% względem poprzedniej; więcej tylko wtedy, gdy w tej samej wersji tniesz gdzie indziej.
Powyżej ~2000 słów — plan główny + załączniki (załączniki liczą się do B). Plan implementacji w repo: forma `superpowers:writing-plans`,
jeśli ten skill jest. Sprzeczne źródła → wygrywa nowsze i pierwotne (docs, release, cennik), nie blog.

## Faza 3 — krytyka (rundy)
- **Runda 1 — pełna krytyka, świeże agenty.** Wszystkie kąty ≥4 rozdane wężykiem. Jeden Sonnet gra też adwokata
  diabła (najmocniejszy argument, że cały plan jest zły). Krytyk dostaje: dosłowne polecenie + plan (z Granicami
  zakresu i załącznikami) + `research.md` + swoje kąty z wagami i progami + rejestr znalezisk.
  Brief = „Krytyka”. Agenci: `subagent_type` general-purpose (sieć do kąta „aktualność wersji”).
  **Zapisz ID każdego krytyka w `stan.md`** (z wyniku narzędzia Agent) — w kolejnych rundach do nich wracasz.
- **Rundy 2+ — ci sami krytycy, tylko różnica.** Nie odpalasz nowych agentów: każdemu krytykowi wysyłasz
  `SendMessage` (po jego ID) z briefem „Runda 2+” (`szablony.md`): diff plan-vK → plan-v(k+1) (tylko zmienione
  fragmenty: było → jest), co zrobiłeś z JEGO znaleziskami, odrzucenia z jednym zdaniem faktów. Krytyk ma już
  w kontekście plan, research i swoje znaleziska, więc nie czyta ich ponownie i nie powtarza wyszukiwań.
  Jego zadanie w rundzie 2+: (a) potwierdzić albo podważyć naprawę swoich znalezisk, (b) sprawdzić, czy diff
  czegoś nie zepsuł. Nowe znaleziska w niezmienionych sekcjach zgłasza tylko jako krytyczne/bardzo poważne.
  Dlaczego: świeży agent co rundę czyta cały plan od nowa i z definicji znajduje „nowe” problemy w miejscach,
  które już były sprawdzone — to napędza puchnięcie planu i niekończące się rundy. Ten sam agent zna kontekst,
  pracuje na różnicy i zużywa wyraźnie mniej tokenów (plan i research nie są wysyłane drugi raz).
- **Agent nie odpowiada na SendMessage** (zniknął, błąd, ~15 min ciszy) → zastąp go jednym świeżym agentem
  z briefem „Krytyka” dla tych samych kątów, z pełnym rejestrem i dopiskiem „runda K: sprawdzasz poprawki
  i diff; nowe znaleziska w niezmienionych sekcjach tylko krytyczne/bardzo poważne”.
- **Świeże oczy na końcu (opcjonalnie, 1 agent Sonnet):** gdy runda da 0 blokerów, a zadanie ma kąty 9–10,
  jeden NOWY agent czyta tylko plan końcowy i polecenie i zgłasza wyłącznie krytyczne/bardzo poważne. Jego
  znalezisko przechodzi Test trafności jak każde inne.

## Faza 4 — poprawki i pętla
0. **Test trafności (każde znalezisko, przed liczeniem):** (a) czy problem NAPRAWDĘ wystąpi przy TYM
   zadaniu i jego zakresie (dowód: plik, krok planu, cytat), a nie hipotetycznie albo gdzie indziej?
   (b) czy proponowana poprawka usuwa DOKŁADNIE ten problem i nie zmienia niczego obok? (c) czy problem
   i poprawka mieszczą się w **Granicach zakresu** i założeniach planu? Problem spoza granic („a co, jeśli
   sklep nie ma biurka”, „import z innego kraju”) to nie bloker: odrzucasz jako „poza zakresem” albo, jeśli
   naprawdę ważny, wpisujesz jedno zdanie do „Otwarte pytania”. Nie → odrzucasz (jedno zdanie powodu
   w rejestrze), do planu nie trafia.
1. Zbierz znaleziska, połącz duplikaty (bierz wyższą ważność), sprawdź znaleziska Haiku duże+.
   **Nie obniżasz ważności nadanej przez krytyka.** Bloker = ważność ≥ progu wagi jego kąta (Krok 0.3).
2. Popraw plan → `plan-v(k+1).md`. Bloker: **naprawiony** (gdzie) albo **odrzucony** z dowodem — odrzucony
   zostaje OTWARTY do „potwierdzam” krytyka; po drugim „podtrzymuję” naprawiasz albo oddajesz jako decyzję.
   **Naprawa u źródła.** Dla każdego blokera znajdź krok/zdanie planu, które jest PRZYCZYNĄ problemu,
   i zmień je tak, żeby problem nie powstał. Każda poprawka to jedna z operacji:
   - **ZAMIEŃ** — błędne zdanie/krok zastępujesz poprawnym (długość podobna, najwyżej ~1,5× starego zdania);
   - **PRZESUŃ** — zmieniasz kolejność kroków;
   - **USUŃ** — wyrzucasz krok/zdanie, które problem tworzy albo nic nie wnosi;
   - **SCAL** — dwa kroki/zdania w jedno;
   - **DODAJ** — tylko gdy celu nie da się osiągnąć bez brakującego kroku; nowy krok to 1–2 zdania w istniejącej
     sekcji, nigdy nowa sekcja.
   Przykład (biurko): krytyka „krok 1 czyta instrukcję, krok 2 zamawia biurko — instrukcja przychodzi w paczce”.
   Dobrze: PRZESUŃ (najpierw zamów, potem czytaj instrukcję z paczki). Źle: dopisać „jeśli instrukcji nie ma,
   poszukaj PDF u producenta, a jeśli go nie ma, …” — to łata objaw, plan rośnie, a błąd w kolejności zostaje.
   Wymóg nie wynika z polecenia ani celu? Nie trafia do planu, nawet gdy krytyk go proponuje — poprawka
   krytyka to sugestia, decydujesz ty według tej listy.
   **Budżet:** po poprawkach policz słowa (`wc -w`). Wersja > B albo +10% względem poprzedniej → przed
   krytyką tnij: najpierw uzasadnienia, powtórzenia, warunki „na wszelki wypadek”, potem kroki poza Granicami.
   Plan = decyzje, bez uzasadnień i powtórzeń. Zapisz w `stan.md` zmienione fragmenty (było → jest)
   i liczbę słów wersji — z tego robisz diff dla krytyków.
3. **Stop = cel**, gdy runda najnowszej wersji da 0 blokerów (nowych i otwartych). 0 już w rundzie 1 → koniec
   (ewentualnie „świeże oczy” z Fazy 3).
4. **Zbieżność:** licz tylko blokery (nowe · otwarte · zamknięte). Od rundy 2 nowe znalezisko w fragmencie
   niezmienionym od rundy 1 jest blokerem tylko jako krytyczne/bardzo poważne (reszta → „zostawione”). Jeśli
   blokerów nie ubyło o ~1/3 względem poprzedniej rundy, NIE przerywaj — znajdź przyczynę i ją usuń przed
   następną rundą: krytycy powtarzają stare znaleziska (popraw rejestr), plan puchnie (tnij), poprawka psuje
   co innego (wróć do prostszej wersji), bloker to w istocie wybór użytkownika (zapytaj go, jeśli jest; jeśli
   nie — oznacz jako decyzję i nie licz jako bloker).
5. **Limit:** R rund, a bez cyfry bezpiecznik 6. Po limicie z otwartymi blokerami nie ogłaszaj sukcesu:
   ostatnia SKRYTYKOWANA wersja, otwarte blokery jako decyzje, poprawki po ostatniej rundzie „niezweryfikowane”.
6. Nie-blokery naprawiaj, gdy tanie; reszta → lista „zostawione”.

## Wynik dla użytkownika
1. Werdykt w jednym zdaniu + plan końcowy (ścieżka i streszczenie).
2. Wagi kątów (top i pominięte) w jednej linijce.
3. Tabela rund: runda → agenci (S/H, nowi/ci sami) → blokery nowe / otwarte / zamknięte → słowa planu → co zmieniono.
4. Zostawione i odrzucone blokery z powodem; najważniejsze źródła z datami.
5. Dalej wg trybu rozmowy (zgoda użytkownika albo realizacja, jeśli kazał działać bez pytań).

## Typowe błędy
| Błąd | Zamiast tego |
|---|---|
| Każdy kąt traktowany tak samo | Wagi 1–10 z Kroku 0, progi, pomijanie 1–3 |
| Agent dostaje same mało ważne kąty | Rozdanie wężykiem od najważniejszych |
| Plan puchnie z rundy na rundę (800 → 3000 słów) | Naprawa u źródła (ZAMIEŃ/PRZESUŃ/USUŃ/SCAL), budżet B = v1 × 1,3, +10% na wersję |
| Plan zbacza z tematu (biurko → import choinek) | Granice zakresu w v1, test (c) „poza zakresem”, wymogi spoza celu nie wchodzą |
| Poprawka krytyka wklejona dosłownie jako nowy akapit | Poprawka krytyka to sugestia; ty wybierasz operację z listy |
| Nowi krytycy co rundę, każdy czyta wszystko od nowa | Runda 2+: ci sami krytycy przez SendMessage, tylko diff |
| Cyfra brana za liczbę agentów albo rund „do zrobienia” | Cyfra to tylko MAKSIMUM rund; bez cyfry pętla do 0 blokerów |
| Liczba znalezisk skacze, koniec na limicie | Rejestr znalezisk w briefie, liczyć blokery, usunąć przyczynę braku zbieżności |
| Za mało agentów na duży research | Wymień Sonnety na Haiku (1 → 2–3) i rozdaj źródła |
| Haiku ogłasza bloker bez dowodu | Duże+ od Haiku sprawdzasz sam |
| Research z pamięci | Data w briefie i przy faktach, odsyłanie raportów bez dat |
| Agenci edytują projekt / mnożą podagentów | Tylko czytają; pliki tymczasowe w katalogu roboczym |
