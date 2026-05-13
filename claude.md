# Jak budować skuteczne prompty do Claude?

## Zadanie 1: Formuła P.K.Z.O. — Prompting dla każdego

Dobry prompt to klucz do uzyskania precyzyjnych i użytecznych odpowiedzi od AI. Formuła **P.K.Z.O.** to prosta metoda, która pomoże Ci konstruować skuteczne zapytania.

### 📋 Schemat P.K.Z.O.

#### 1. **P** — Persona (Rola)
Nadaj Claude konkretną rolę eksperta, który ma Ci pomóc.
- **Przykład:** "Jesteś ekspertem finansowym...", "Jesteś doświadczonym nauczycielem...", "Jesteś specjalistą HR..."

#### 2. **K** — Kontekst (Sytuacja)
Opisz sytuację, w której się znajdujesz. Podaj tło i ważne szczegóły.
- **Przykład:** "Pracujemy nad budżetem na 2025 rok...", "Organizuję szkolenie dla 20 osób...", "Przygotowuję się do rozmowy kwalifikacyjnej..."

#### 3. **Z** — Zadanie (Cel)
Określ konkretny cel — co dokładnie ma zrobić Claude?
- **Przykład:** "Stwórz tabelę...", "Napisz email...", "Przygotuj listę kroków...", "Wygeneruj pomysły..."

#### 4. **O** — Ograniczenia (Ramy i format)
Określ format odpowiedzi, długość, styl, język lub inne wymagania.
- **Przykład:** "Użyj PLN, max 10 pozycji, format tabeli", "W punktach, maksymalnie 5 punktów", "Ton formalny, do 200 słów"

---

## 💡 Przykład 1: Przygotowanie ogłoszenia o pracę

### ❌ Słaby prompt:
```
Napisz ogłoszenie o pracę dla sekretarki.
```

### ✅ Dobry prompt (P.K.Z.O.):
```
PERSONA: Jesteś specjalistą HR z 10-letnim doświadczeniem w rekrutacji.

KONTEKST: Nasza szkoła poszukuje osoby na stanowisko sekretarki/sekretarza 
do obsługi sekretariatu szkolnego. Praca na pełen etat, wymagana obsługa 
programu Vulcan, kontakt z rodzicami i nauczycielami.

ZADANIE: Stwórz profesjonalne ogłoszenie o pracę, które przyciągnie 
odpowiednich kandydatów.

OGRANICZENIA:
- Długość: do 300 słów
- Struktura: stanowisko, opis, wymagania, oferujemy, kontakt
- Ton: profesjonalny ale przyjazny
- Wynagrodzenie: 4500-5500 zł brutto (podaj widełki)
```

---

## 💡 Przykład 2: Planowanie budżetu firmowego

### ❌ Słaby prompt:
```
Pomóż mi z budżetem firmy.
```

### ✅ Dobry prompt (P.K.Z.O.):
```
PERSONA: Jesteś doświadczonym doradcą finansowym dla małych firm.

KONTEKST: Prowadzę małą firmę usługową (5 pracowników). Planuję budżet 
na 2025 rok. Roczny przychód to około 600 000 zł. Chcę lepiej kontrolować 
wydatki i zaplanować oszczędności na rozwój firmy.

ZADANIE: Stwórz szablon prostego budżetu rocznego z podstawowymi kategoriami 
kosztów, które powinienem monitorować.

OGRANICZENIA:
- Format: tabela w markdown
- Waluta: PLN
- Maksymalnie 12 kategorii kosztów
- Podziel na: koszty stałe i zmienne
- Dodaj kolumnę z procentowym udziałem w budżecie
- Dodaj krótki komentarz (2-3 zdania) do każdej kategorii
```

## Zadanie 2: Logowanie kontem Gmail do portalu Claude.ai

Cel: założyć konto lub zalogować się do Claude przy użyciu konta Google (Gmail).

### Krok po kroku

1. Wejdź na stronę: **https://claude.ai**
2. Kliknij przycisk **Continue with Google**.
3. Wybierz swoje konto Gmail z listy.
4. Jeśli pojawi się okno zgody Google, kliknij **Allow / Zezwól**.
5. Po poprawnym logowaniu zostaniesz przeniesiony do panelu Claude.
6. Przy pierwszym wejściu możesz zostać poproszony o:
   - potwierdzenie danych,
   - akceptację regulaminu,
   - wybór planu (darmowy lub płatny).

### Zadanie praktyczne dla uczestnika

- Zaloguj się do Claude kontem Gmail.
- Wyślij pierwszy prompt: **"wg zasady (P.K.Z.O.)"**
- Skopiuj odpowiedź i zapisz ją jako notatkę.

Oczekiwany efekt: uczestnik ma aktywne konto Claude i potrafi rozpocząć pierwszą rozmowę.

## Zadanie 3: Jak używać skilli w Claude.ai (import i praktyka)

Cel: nauczyć uczestnika korzystania z gotowych skilli zapisanych w folderze `zasoby/skills`.

### Co to jest skill

Skill to gotowy zestaw instrukcji i promptów do konkretnej roli. Dzięki temu szybciej uzyskasz dobrą odpowiedź od Claude.

Dostępne skille w tym projekcie:
- `zasoby/skills/student/SKILL.md`
- `zasoby/skills/przedsiebiorca/SKILL.md`
- `zasoby/skills/sekretariat/SKILL.md`
- `zasoby/skills/urzednik/SKILL.md`

### Jak „zaimportować” skill do Claude.ai

Claude.ai nie ma klasycznego przycisku „Import skill” z pliku lokalnego. Najprostsza metoda to wklejenie treści skilla do rozmowy.

#### Metoda wgrania skilla: Kopiuj-wklej (najprostsza)
1. Otwórz wybrany plik `SKILL.md`.
2. Skopiuj całą treść.
3. Wejdź do rozmowy w Claude.ai.
4. Wklej treść i dopisz polecenie na pocztku:
   - /skill-creator [tresc skilla]
5. Następnie postepuj zgodnie z poleceniami


### Przykłady użycia (na bazie naszych skilli)

#### 1) Sekretariat szkoły
Prompt:
```text
Używaj zasad ze skilla: sekretariat.
Przerób ten skomplikowany opis na listę 5 prostych kroków dla rodzica.
Język: prosty, bez żargonu urzędowego.
Tekst: [wklej procedurę]
```

#### 2) Urzędnik
Prompt:
```text
Używaj zasad ze skilla: urzednik.
Streszcz procedurę do wersji "Najważniejsze informacje".
Format: 5 punktów + sekcja "Terminy" + sekcja "Wymagane dokumenty".
Tekst: [wklej procedurę]
```

#### 3) Student
Prompt:
```text
Używaj zasad ze skilla: student.
Stwórz streszczenie poniższego tekstu, wyciągając tylko tezy i wnioski.
Format: 5 punktów, max 150 słów.
Tekst: [wklej materiał]
```

#### 4) Przedsiębiorca
Prompt:
```text
Używaj zasad ze skilla: przedsiebiorca.
Napisz krótką ofertę dla klienta.
Usługa: [wklej].
Klient: [wklej branżę].
Format: 4 sekcje (problem, rozwiązanie, korzyści, cena), max 180 słów.
Ton: profesjonalny i konkretny.
```

Oczekiwany efekt: uczestnik potrafi użyć gotowego skilla, uruchomić zadanie i poprawić odpowiedź Claude.

## Zadanie 4: Projekty w Claude — jeden temat, wiele dokumentów (skill: student)

Cel: nauczyć uczestników pracy w Claude Projects, gdzie jeden projekt przechowuje kontekst wielu materiałów i rozmów.

Materiały do użycia:
- `zasoby/pdf/BCU.Opole.pdf` (prezentacja z warsztatów)
- skill: `zasoby/skills/student/SKILL.md`

### Jak wykonać zadanie w Claude Projects

1. Wejdź do Claude i utwórz nowy projekt: **Sprawy studenckie**.
2. W projekcie dodaj plik `BCU.Opole.pdf` (ikona spinacza/plus).
3. W pierwszej wiadomości w projekcie wklej treść skilla `student` i poproś Claude o stosowanie tych zasad w całym projekcie.
4. Załóż w projekcie osobny czat: **Analiza prezentacji BCU**.
5. W tym czacie wykonaj analizę materiału.
6. Załóż drugi czat w tym samym projekcie: **Powtórka do zajęć** i poproś o pytania kontrolne/fiszki. Claude powinien utrzymać kontekst dokumentu projektu.

### Prompt startowy do czatu „Analiza prezentacji BCU”

```text
Używaj zasad ze skilla: student.
Pracujemy w projekcie „Sprawy studenckie” na pliku BCU.Opole.pdf.
Przygotuj:
1) Streszczenie prezentacji w 6 punktach.
2) Listę 8 kluczowych pojęć z prostym wyjaśnieniem.
3) Sekcję „Co warto zapamiętać na egzamin/zaliczenie?” (max 5 punktów).
Język: prosty, dla początkujących.
```

### Prompt do drugiego czatu „Powtórka do zajęć”

```text
Na podstawie wiedzy z tego projektu i pliku BCU.Opole.pdf:
1) Ułóż plan nauki na 7 dni (15-20 minut dziennie).
2) Przygotuj 12 pytań kontrolnych (od łatwych do trudniejszych).
3) Dodaj 6 fiszek pytanie-odpowiedź.
Format: czytelne sekcje, krótkie odpowiedzi.
```

Wskazówka dla uczestników: twórz osobne projekty dla różnych obszarów (np. projekty unijne, sprawy studenckie), a w każdym projekcie osobne czaty do konkretnych zadań.

Oczekiwany efekt: uczestnik potrafi użyć Claude Projects do pracy na wielu dokumentach i rozmowach, z zachowaniem wspólnego kontekstu oraz ze skillem student.
