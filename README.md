# w1mner-skille

Marketplace wtyczek Claude Code. Wtyczki: **ultra-plan**, **shut-up**, **just-do-it**.

## Instalacja

```
claude plugin marketplace add W1mner/w1mner-skille
claude plugin install ultra-plan@w1mner-skille
claude plugin install shut-up@w1mner-skille
claude plugin install just-do-it@w1mner-skille
```

(albo w sesji: `/plugin marketplace add W1mner/w1mner-skille`, potem `/plugin install <wtyczka>@w1mner-skille`)

`just-do-it` korzysta też z wtyczki `superpowers@claude-plugins-official`. Instalacja just-do-it ciągnie shut-up, ultra-plan i superpowers jako zależności; brakujące skill i tak doinstaluje sam.

## Użycie

- `/ultra-plan:ultra-plan <zadanie>`: research, plan i krytyka aż do 0 blokerów (bezpiecznik 6 rund)
- `/ultra-plan:ultra-plan-1 <zadanie>`, `-3`, `-5`: to samo z limitem rund krytyki
- `/shut-up:shut-up`: Claude pracuje w ciszy i odzywa się raz, na końcu. Wyłączasz słowem „gadaj”.
- `/just-do-it:just-do-it <zadanie>`: shut-up + superpowers, a ultra-plan tylko dla tych części zadania, które naprawdę go potrzebują (nie dla zmiany koloru przycisku)

## Aktualizacja

```
claude plugin marketplace update w1mner-skille
```
