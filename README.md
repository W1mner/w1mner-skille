# w1mner-skille

Marketplace wtyczek Claude Code. Na razie jedna wtyczka: **ultra-plan**.

## Instalacja

```
claude plugin marketplace add W1mner/w1mner-skille
claude plugin install ultra-plan@w1mner-skille
```

(albo w sesji: `/plugin marketplace add W1mner/w1mner-skille`, potem `/plugin install ultra-plan@w1mner-skille`)

## Użycie

- `/ultra-plan:ultra-plan <zadanie>` — research → plan → krytyka, aż do 0 blokerów (bezpiecznik 6 rund)
- `/ultra-plan:ultra-plan-1 <zadanie>`, `-3`, `-5` — to samo z limitem rund krytyki

## Aktualizacja

```
claude plugin marketplace update w1mner-skille
```
