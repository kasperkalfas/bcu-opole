# 3 zadania praktyczne z NotebookLM

## Zadanie 1: Wczytanie dokumentu PDF

### Cel
Dodać źródło do NotebookLM i przygotować bazę do dalszej pracy.

### Instrukcja
1. Wejdź do NotebookLM i utwórz nowy notatnik.
2. Kliknij **Add source / Dodaj źródło**.
3. Wgraj jeden z plików:
   - `zasoby/pdf/BCU.Opole.pdf` (prezentacja z warsztatów) **lub**
   - `zasoby/pdf/historia_opole.pdf`.
4. Poczekaj, aż dokument zostanie przetworzony.
5. Sprawdź, czy dokument jest widoczny na liście źródeł.

### Oczekiwany efekt
Uczestnik ma aktywny notatnik z poprawnie załadowanym PDF-em.

---

## Zadanie 2: Quiz i rozmowa z dokumentem

### Cel
Nauczyć się zadawać pytania i testować wiedzę na podstawie źródła.

### Instrukcja
1. W tym samym notatniku wpisz prompt:

```text
Na podstawie załadowanego dokumentu przygotuj quiz:
- 10 pytań (6 zamkniętych, 4 otwarte)
- poziom podstawowy
- na końcu dodaj klucz odpowiedzi.
```

2. Następnie wpisz prompt do rozmowy:

```text
Wyjaśnij mi ten materiał prostym językiem, jak dla osoby początkującej.
Najpierw podaj 5 najważniejszych wniosków, a potem zapytaj mnie, czy chcesz żebym rozwinął któryś punkt.
```

3. Zadaj 2-3 pytania uzupełniające, np.:
   - „Który fragment jest najważniejszy do zapamiętania?”
   - „Podaj przykład praktycznego zastosowania.”

### Oczekiwany efekt
Uczestnik potrafi wygenerować quiz i prowadzić rozmowę z dokumentem.

---

## Zadanie 3: Wygenerowanie podcastu z materiału

### Cel
Automatycznie stworzyć audio-podsumowanie dokumentu.

### Instrukcja
1. W notatniku z tym samym PDF-em wybierz opcję generowania audio/podcastu (np. **Audio Overview**).
2. Ustaw język polski (jeśli dostępny) i poziom: podstawowy.
3. Wygeneruj podcast.
4. Odsłuchaj całość i zapisz 3 najważniejsze informacje, które zapamiętałeś.

### Prompt pomocniczy (opcjonalnie)
```text
Przygotuj wersję podcastu dla początkujących.
Mów prostym językiem, używaj krótkich zdań, unikaj żargonu.
Czas: około 3-5 minut.
```

### Oczekiwany efekt
Uczestnik potrafi wygenerować i wykorzystać podcast jako formę powtórki materiału.
