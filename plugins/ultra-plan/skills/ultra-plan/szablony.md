# Katalog kątów i szablony briefów dla ultra-plan

## Katalog kątów (oceniasz każdy 1–10 dla zadania; dopisz własne, jeśli brakuje)

**Ogólne:** zgodność z poleceniem (zawsze 10) · spójność · błędy logiczne · potencjalne braki / kompletność ·
wykonalność · kolejność i zależności kroków · założenia · koszty i limity (pieniądze, tokeny, czas) ·
ryzyka · bezpieczeństwo · prawo, licencje, RODO · aktualność wersji i możliwość aktualizacji · testowalność
i kryteria „gotowe” · utrzymanie po wdrożeniu.

**Kod i aplikacje:** architektura i struktura kodu · niepotrzebny kod · powtarzanie elementów i ponowne
użycie funkcji/obiektów · wydajność i optymalizacja · dane i migracje · integracje i API · obsługa błędów ·
testy · wdrożenie i wycofanie (rollback) · zgodność z istniejącymi konwencjami projektu.

**UI/UX:** spójność wizualna · UX i przepływy · responsywność/telefon · teksty w interfejsie · dostępność (a11y).

**Media (filmy, animacje, grafika):** historia i scenariusz · tempo i montaż · muzyka · efekty dźwiękowe ·
spójność dźwięku (głośność, miks) · głos/lektor · styl obrazu · spójność postaci · napisy · zgodność
z referencjami · dostępność materiałów (assety, modele, licencje) · wymagania platformy (format, długość).

**Treści i biznes:** grupa docelowa · ton i marka · oferta i cena · SEO · konkurencja.

**Źródła researchu:** sieć (wyszukiwarka/Exa) · GitHub (repo, issues, release'y) · YouTube · oficjalne docs
i changelogi · cenniki · Hugging Face / leaderboardy · fora, Reddit, X · marketplace'y skilli i wtyczek ·
lokalny kod i pliki.

Uzupełnij pola w <>. Brief ma być samowystarczalny: agent nie widzi rozmowy.

## Research — Sonnet (temat w głąb)

```
Dziś jest <RRRR-MM-DD>. Robisz research do planu: <zadanie w 1–3 zdaniach>.
Twoje tematy (waga 1–10, od najważniejszego): <lista>. Inni agenci robią: <krótko>.
Co już wiemy (nie powtarzaj): <krótko>.

Zasady aktualności:
- Każdy fakt z URL i datą publikacji/aktualizacji; brak daty → „data nieznana”.
- Preferuj źródła z ostatnich 3 miesięcy; starsze oznacz „STARE (data)”.
- Twoja wiedza z treningu jest nieaktualna o miesiące: wersje, ceny i „najlepsze” bierzesz WYŁĄCZNIE ze źródeł
  sprawdzonych dziś.
- Najpierw ODKRYWANIE nowości: „best <X> <rok>”, „new <X> released <miesiąc rok>”, „<X> vs”; GitHub po dacie;
  rankingi; listy „awesome-<X>”. Potem shortlista maks. 5 kandydatów i dla każdego: release'y, cennik na dziś —
  nowsza wersja? lepsza alternatywa? tańsza? (wynik także, gdy „nie ma”).
- Źródła pierwotne (docs, repo, cenniki) ponad blogi i farmy SEO. Budżet ~30 wyszukiwań.

Narzędzia: skill agent-reach, jeśli jest (najpierw `agent-reach doctor --json`), inaczej WebSearch/WebFetch;
pliki lokalne tylko do odczytu. Pliki tymczasowe wyłącznie w: <ścieżka bezwzględna>. Nie edytuj niczego
innego. Nie uruchamiaj własnych podagentów.

Wynik (max ~600 słów): 1) ustalenia z URL + datą, 2) tabela: narzędzie/usługa · wersja dziś · data źródła ·
nowsza? · lepsza alternatywa? · tańsza? + „Nowe w ostatnich 90 dniach:”, 3) rekomendacja, 4) ryzyka i niewiadome.
```

## Research — Haiku (przeszukanie źródła)

```
Dziś jest <RRRR-MM-DD>. Zbierasz fakty do planu: <zadanie w 1 zdaniu>.
Twoje źródło: <np. GitHub / YouTube / oficjalne docs X>. Tematy (od najważniejszego): <lista>.
Szukaj tylko w tym źródle. Każdy fakt z URL i datą (brak → „data nieznana”); nie podawaj niczego z pamięci.
Nie oceniaj i nie rekomenduj — tylko fakty: co istnieje, wersja, data, cena, liczby (gwiazdki, wyświetlenia).
Budżet ~15 wyszukiwań. Pliki tymczasowe tylko w: <ścieżka>. Nie edytuj niczego, nie uruchamiaj podagentów.
Wynik (max ~400 słów): lista faktów „temat · fakt · URL · data”, na końcu czego nie znalazłeś.
```

## Krytyka

```
Dziś jest <RRRR-MM-DD>. Krytykujesz plan. Twoim zadaniem jest ZNALEŹĆ problemy, nie chwalić.
Polecenie użytkownika (dosłownie): <cytat>
Plan: <ścieżka do plan-vK.md> (przeczytaj cały). Research: <ścieżka>.
Granice zakresu są na górze planu. Problem spoza nich, albo „a co, jeśli…” bez dowodu, że wystąpi przy TYM
zadaniu, to najwyżej „mała” — nie rozszerzaj planu o rzeczy, których polecenie nie wymaga.
Twoje kąty (waga · próg, od najważniejszego): <np. „muzyka 10 · duże+”, „napisy 6 · duże+”, „SEO 4 · tylko krytyczne/b. poważne”>.
Najwięcej czasu daj kątom o najwyższej wadze. W kątach z progiem „tylko krytyczne/b. poważne” zgłaszaj
wyłącznie rażące błędy.
Rejestr dotychczasowych znalezisk (ID · zdanie · status): <lista albo „brak”>. Nie zgłaszaj ponownie niczego
z rejestru, także innymi słowami, chyba że poprawka to zepsuła.

Skala ważności (używaj tylko tej):
- krytyczna: plan nie osiągnie celu, złamie polecenie/zakaz użytkownika, grozi utratą danych, pieniędzy lub bezpieczeństwa.
- bardzo poważna: kluczowy krok nie zadziała albo wymaga przebudowy dużej części planu.
- duża: istotna luka, sprzeczność lub błędne założenie; da się naprawić lokalnie, ale bez tego wynik będzie wyraźnie gorszy.
- średnia: niedoróbka, która kosztuje czas/pieniądze lub jakość, ale nie blokuje.
- mała: kosmetyka, nazwy, czytelność.

<Jeśli dotyczy: „Jesteś też adwokatem diabła: podaj najmocniejszy argument, że cały plan jest zły albo
rozwiązuje nie ten problem, i oceń jego ważność.”>

Każde znalezisko: ID · kąt · ważność · cytat z planu · problem · dowód (źródło/liczba/scenariusz porażki) · poprawka.
Poprawka = naprawa PRZYCZYNY w istniejącym tekście, zapisana jako jedna operacja: ZAMIEŃ „<cytat>” → „<nowy
tekst>” · PRZESUŃ krok X przed Y · USUŃ „<cytat>” · SCAL X+Y. DODAJ (1–2 zdania w istniejącej sekcji) tylko gdy
bez brakującego kroku celu nie da się osiągnąć. Nie proponuj nowych sekcji, wariantów „jeśli się nie uda, to…”
ani zabezpieczeń na wszelki wypadek — jeśli krok może zawieść, popraw sam krok.
Ważność „duża” i wyżej tylko z konkretnym dowodem; uwagi o stylu to „mała”.
Odrzucenia autora (jeśli są w briefie): dla każdego „potwierdzam” albo „podtrzymuję” z powodem.
Kąt bez znalezisk: napisz „nic” i wskaż sekcje/zdania planu, które sprawdziłeś.
Kąt „aktualność wersji”: sprawdź sam (maks. ~15 wyszukiwań), czy kluczowe narzędzia/modele/ceny są nadal najnowsze.
Tylko czytasz; nie edytuj plików, nie uruchamiaj podagentów. Wynik max <~600 słów; Haiku ~400>, od najpoważniejszych.
```

## Runda 2+ (SendMessage do TEGO SAMEGO krytyka, po jego ID)

```
Runda <K>. Poprawiłem plan: <ścieżka do plan-vK.md> (<słowa> słów; poprzednio <słowa>).
Nie czytaj całego planu ani researchu od nowa — masz je w kontekście. Zmiany (było → jest):
<lista fragmentów diff, tylko to, co się zmieniło — WSZYSTKIE zmiany wersji, także z cudzych znalezisk>
Twoje znaleziska:
- <ID>: naprawione → <gdzie, jaką operacją>
- <ID>: naprawione inaczej → <operacja>; twoja propozycja nie weszła, bo <jedno zdanie>
- <ID>: odrzucone → <jedno zdanie faktów>
Zadanie:
1) Dla każdego swojego znaleziska: „naprawione” / „nienaprawione — <dlaczego>” (inna operacja niż twoja
   propozycja to nie powód do „nienaprawione”, jeśli problem zniknął); dla odrzuconych
   „potwierdzam” albo „podtrzymuję — <powód>”.
2) Sprawdź tylko zmienione fragmenty: czy poprawka czegoś nie zepsuła albo nie wyszła poza Granice zakresu.
3) Nowe znaleziska w niezmienionych fragmentach zgłaszaj tylko jako krytyczne/bardzo poważne.
Ta sama skala ważności i ten sam format znaleziska co w rundzie 1 (poprawka = operacja ZAMIEŃ/PRZESUŃ/USUŃ/SCAL).
Wynik max ~300 słów. Brak uwag → napisz „nic” i które fragmenty sprawdziłeś.
```
