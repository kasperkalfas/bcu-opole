# Warsztaty AI w n8n (30 minut, za darmo, poziom podstawowy)

Materiał dla grupy: studenci, przedsiębiorcy, pracownicy sekretariatu szkoły, urzędnicy.

## Cel warsztatu

W 30 minut pokazać 2 proste automatyzacje w n8n z darmowym modelem Gemini:
1. FAQ z jednego dokumentu (wklejony tekst).
2. Klasyfikacja wiadomości: pilne / normalne / do archiwum.

## Wymagania (bezpłatnie)

- Konto Google.
- Klucz API Gemini z Google AI Studio.
- n8n uruchomione lokalnie lub w darmowej wersji testowej.

## Krok 1: Darmowy klucz Gemini

1. Wejdź na: https://aistudio.google.com
2. Zaloguj się kontem Google.
3. Wejdź w sekcję API keys.
4. Utwórz nowy klucz i skopiuj go.
5. Zachowaj klucz do wklejenia w n8n.

## Krok 2: Wspólna konfiguracja HTTP Request w n8n

W obu ćwiczeniach użyjesz tego samego node `HTTP Request`. Ustaw go raz, a potem tylko podmieniaj treść promptu.

### Dokładna konfiguracja node

1. Kliknij **+ Add node** i wybierz `HTTP Request`.
2. W zakładce parametrów ustaw:
   - **Method**: `POST`
   - **URL**: `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent`
3. W sekcji **Query Parameters** dodaj:
   - **Name**: `key`
   - **Value**: `TWÓJ_KLUCZ_GEMINI`
4. W sekcji **Send Body** wybierz tryb `JSON`.
5. W sekcji **Headers** dodaj:
   - **Content-Type**: `application/json`
6. W polu **Body** wklej JSON z promptem (oddzielny dla każdego ćwiczenia).

### Co sprawdzić przed uruchomieniem

- Czy klucz API jest w Query Parameter `key`, a nie w body.
- Czy JSON zaczyna się od `{` i kończy `}`.
- Czy zmienna z poprzedniego noda jest wpisana jako expression, np. `{{$json.tekst_zrodlowy}}`.

### Gdzie jest wynik z Gemini

Po wykonaniu noda wejdź w **Output**. Tekst modelu znajdziesz pod ścieżką:
- `candidates[0].content.parts[0].text`

To tę wartość przekazujemy dalej do kolejnych nodów `Set` lub `Switch`.

## Ćwiczenie 1 (ok. 15 min): FAQ z jednego dokumentu

### Co pokazujesz
Z wklejonego regulaminu, ogłoszenia albo pisma AI tworzy 5 pytań i odpowiedzi FAQ.

### Dla kogo
Sekretariat, urząd, przedsiębiorcy, studenci.

### Nody
`Manual Trigger` → `Set` → `HTTP Request (Gemini)` → `Set (Wynik FAQ)`

### Krok po kroku

1. Dodaj `Manual Trigger`.
   - Ten node pozwala uruchamiać ćwiczenie ręcznie podczas warsztatu.

2. Dodaj node `Set` i nazwij go np. `Dane wejściowe FAQ`.
   - Włącz tryb **Manual Mapping**.
   - Dodaj pole typu string: `tekst_zrodlowy`.
   - Wklej dłuższy tekst, np. regulamin, komunikat lub fragment procedury.
   - Upewnij się, że tekst ma minimum kilka zdań, żeby FAQ było sensowne.

3. Dodaj `HTTP Request (Gemini)` i ustaw konfigurację wspólną z sekcji wyżej.
   - Najważniejsze: URL modelu, Query Param `key`, JSON body.

4. W Body JSON wstaw:

```json
{
  "contents": [
    {
      "parts": [
        {
          "text": "Na podstawie tekstu przygotuj dokładnie 5 pytań i odpowiedzi FAQ w języku polskim. Pisz prosto i zrozumiale dla poziomu podstawowego. Odpowiedź zwróć w formacie: Pytanie 1: ... Odpowiedź 1: ... Tekst źródłowy: {{$json.tekst_zrodlowy}}"
        }
      ]
    }
  ]
}
```

5. Uruchom sam node `HTTP Request` przyciskiem **Execute step**.
   - Dzięki temu od razu sprawdzisz, czy API odpowiada poprawnie.
   - Jeśli jest błąd, popraw najpierw klucz lub JSON, zanim przejdziesz dalej.

6. Dodaj kolejny `Set` o nazwie `Wynik FAQ`.
   - Dodaj pole `faq` i wpisz expression:
   - `{{$json.candidates[0].content.parts[0].text}}`
   - To pole wyciąga samą treść odpowiedzi modelu bez dodatkowych danych technicznych.

7. Kliknij `Execute Workflow` i przejdź do output noda `Wynik FAQ`.
   - Skopiuj gotowy tekst FAQ i pokaż uczestnikom.
   - Jeśli odpowiedź jest za długa lub za trudna, zmień prompt i uruchom ponownie.

8. Wariant szybki na żywo (opcjonalnie):
   - Zmień tylko `tekst_zrodlowy` na inny temat.
   - Pokaż, że ten sam workflow działa dla różnych dokumentów bez przebudowy nodów.

### Efekt
Uczestnik widzi, jak z jednego tekstu szybko przygotować gotowe pytania i odpowiedzi dla odbiorców.

## Ćwiczenie 2 (ok. 15 min): Klasyfikacja wiadomości

### Co pokazujesz
AI przypisuje kategorię wiadomości i proponuje kolejne działanie.

### Dla kogo
Sekretariat i administracja (także studenci i przedsiębiorcy).

### Nody
`Manual Trigger` → `Set (Wiadomość)` → `HTTP Request (Gemini)` → `Set (Kategoria)` → `Switch`

### Krok po kroku

1. Dodaj `Manual Trigger`.
   - To punkt startowy demonstracji, uruchamiany ręcznie przez prowadzącego.

2. Dodaj `Set` i nazwij node np. `Wiadomość wejściowa`.
   - Włącz **Manual Mapping**.
   - Dodaj pole string `wiadomosc`.
   - Wklej jedną wiadomość testową od interesanta/klienta/studenta.

3. Dodaj `HTTP Request (Gemini)` i użyj tej samej konfiguracji co wcześniej.
   - Zmieniasz tylko prompt w body JSON.

4. W Body JSON wstaw:

```json
{
  "contents": [
    {
      "parts": [
        {
          "text": "Sklasyfikuj wiadomość do jednej z kategorii: pilne, normalne, do_archiwum. Następnie zaproponuj jedno krótkie kolejne działanie. Zwróć odpowiedź dokładnie w 2 liniach: Kategoria: <wartość> Działanie: <wartość>. Wiadomość: {{$json.wiadomosc}}"
        }
      ]
    }
  ]
}
```

5. Uruchom `HTTP Request` przez **Execute step** i sprawdź odpowiedź modelu.
   - Upewnij się, że model zwrócił linię z `Kategoria:`.

6. Dodaj `Set` o nazwie `Kategoria`.
   - Pole `model_output`:
   - `{{$json.candidates[0].content.parts[0].text}}`
   - Pole `kategoria`:
   - `{{ $json.model_output.toLowerCase().includes('pilne') ? 'pilne' : ($json.model_output.toLowerCase().includes('normalne') ? 'normalne' : 'do_archiwum') }}`

7. Dodaj `Switch` i skonfiguruj 3 warunki dla pola `kategoria`:
   - równe `pilne`
   - równe `normalne`
   - równe `do_archiwum`

8. Do każdej gałęzi podłącz prosty `Set` z decyzją operacyjną:
   - `pilne` → „Przekaż do natychmiastowej obsługi”.
   - `normalne` → „Obsłuż w standardowej kolejce”.
   - `do_archiwum` → „Zapisz i zamknij bez dalszych działań”.

9. Uruchom cały workflow `Execute Workflow`.
   - Pokaż, do której gałęzi trafiła wiadomość.
   - Zmień treść `wiadomosc` i uruchom ponownie, aby pokazać zmianę klasyfikacji.

10. Wersja demonstracyjna dla różnych grup (opcjonalnie):
   - student: wiadomość o terminie zaliczenia,
   - przedsiębiorca: zapytanie o ofertę,
   - sekretariat/urząd: prośba o pilne zaświadczenie.
   Dzięki temu uczestnicy widzą uniwersalność tego samego workflow.

### Efekt
Uczestnik widzi prosty mechanizm triage wiadomości, który można później rozbudować.

## Plan prowadzenia (30 min)

- 0–5 min: logowanie, klucz Gemini, szybkie pokazanie n8n.
- 5–20 min: ćwiczenie FAQ.
- 20–30 min: ćwiczenie klasyfikacji i Switch.

## Gotowe teksty testowe do pokazania

### Tekst do FAQ (wklej do `tekst_zrodlowy`)
Sekretariat szkoły informuje, że od poniedziałku do piątku pracuje w godzinach 8:00–15:00. Wnioski o legitymację należy składać do godziny 13:00. Zaświadczenia są wydawane w ciągu 2 dni roboczych. W pilnych sprawach można kontaktować się mailowo. W okresie wakacyjnym godziny pracy mogą ulec zmianie.

### Wiadomość do klasyfikacji (wklej do `wiadomosc`)
Dzień dobry, jutro mam termin złożenia dokumentów i brakuje mi jednego podpisu. Czy mogę pilnie umówić wizytę jeszcze dziś?

## Najczęstsze problemy i szybkie rozwiązania

- Brak odpowiedzi z API: sprawdź poprawność klucza Gemini i czy klucz jest aktywny.
- Błąd 400: sprawdź, czy Body JSON jest poprawny i nie ma brakujących nawiasów.
- Błąd 403: klucz nie ma uprawnień lub został błędnie skopiowany.
- Pusty wynik: sprawdź ścieżkę `candidates[0].content.parts[0].text`.
- Switch nie działa: sprawdź, czy pole `kategoria` ma dokładnie jedną z 3 wartości.

## Szybka checklista prowadzącego przed zajęciami

- Mam aktywny klucz Gemini i zapisany w bezpiecznym miejscu.
- W n8n działa `Manual Trigger` i mogę uruchomić workflow.
- Node `HTTP Request` ma poprawny URL i Query Param `key`.
- W node `Set` używam poprawnych nazw pól: `tekst_zrodlowy`, `wiadomosc`.
- Potrafię odczytać wynik z `candidates[0].content.parts[0].text`.
- Mam przygotowane 2-3 teksty zapasowe do demo.

## Co mówić uczestnikom podczas pokazu (wersja bardzo prosta)

- „Najpierw dodajemy dane wejściowe w `Set`.”
- „Potem wysyłamy je do Gemini przez `HTTP Request`.”
- „Na końcu odbieramy wynik i podejmujemy decyzję w `Switch`.”
- „Nie budujemy nic skomplikowanego — celem jest zrozumienie schematu działania.”

## Co uczestnik umie po warsztacie

- Uruchomić prosty workflow w n8n.
- Połączyć n8n z darmowym modelem Gemini.
- Przetworzyć tekst przez AI i uzyskać praktyczny wynik.
- Zrobić prostą logikę decyzyjną przez `Switch`.