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

W obu ćwiczeniach użyjesz tego samego podejścia:

1. Dodaj node `HTTP Request`.
2. Method: `POST`.
3. URL:
   `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent`
4. Query Parameters:
   - `key` = `TWÓJ_KLUCZ_GEMINI`
5. Send Body: `JSON`.
6. Header `Content-Type: application/json`.
7. W Body wysyłaj pole `contents` z promptem.

Minimalny format odpowiedzi Gemini, z którego korzystamy:
- wynik tekstowy będzie w: `candidates[0].content.parts[0].text`

## Ćwiczenie 1 (ok. 15 min): FAQ z jednego dokumentu

### Co pokazujesz
Z wklejonego regulaminu, ogłoszenia albo pisma AI tworzy 5 pytań i odpowiedzi FAQ.

### Dla kogo
Sekretariat, urząd, przedsiębiorcy, studenci.

### Nody
`Manual Trigger` → `Set` → `HTTP Request (Gemini)` → `Set (Wynik FAQ)`

### Krok po kroku

1. Dodaj `Manual Trigger`.
2. Dodaj `Set` i utwórz pole tekstowe `tekst_zrodlowy`.
   - Wklej dłuższy tekst (np. regulamin lub komunikat).
3. Dodaj `HTTP Request` i ustaw konfigurację wspólną z sekcji wyżej.
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

5. Dodaj kolejny `Set` o nazwie `Wynik FAQ`.
6. Utwórz pole `faq` i ustaw wartość:
   `{{$json.candidates[0].content.parts[0].text}}`
7. Kliknij `Execute Workflow`.
8. Pokaż uczestnikom gotowe FAQ.

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
2. Dodaj `Set` i utwórz pole `wiadomosc`.
   - Wklej przykładową wiadomość, np. prośbę o pilny termin lub zwykłe zapytanie.
3. Dodaj `HTTP Request` i użyj tej samej konfiguracji co wcześniej.
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

5. Dodaj `Set` o nazwie `Kategoria`.
6. Utwórz pole `model_output`:
   `{{$json.candidates[0].content.parts[0].text}}`
7. Utwórz pole `kategoria` z wyrażeniem:
   `{{ $json.model_output.toLowerCase().includes('pilne') ? 'pilne' : ($json.model_output.toLowerCase().includes('normalne') ? 'normalne' : 'do_archiwum') }}`
8. Dodaj `Switch` i ustaw reguły:
   - jeśli `kategoria` = `pilne`
   - jeśli `kategoria` = `normalne`
   - jeśli `kategoria` = `do_archiwum`
9. Do każdej gałęzi możesz dodać prosty `Set` z komunikatem:
   - pilne: „Przekaż do natychmiastowej obsługi”.
   - normalne: „Obsłuż w standardowej kolejce”.
   - do_archiwum: „Zapisz i zamknij bez dalszych działań”.

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

- Brak odpowiedzi z API: sprawdź poprawność klucza Gemini.
- Błąd 400: sprawdź, czy Body JSON jest poprawny.
- Pusty wynik: sprawdź ścieżkę `candidates[0].content.parts[0].text`.
- Switch nie działa: sprawdź, czy pole `kategoria` ma dokładnie jedną z 3 wartości.

## Co uczestnik umie po warsztacie

- Uruchomić prosty workflow w n8n.
- Połączyć n8n z darmowym modelem Gemini.
- Przetworzyć tekst przez AI i uzyskać praktyczny wynik.
- Zrobić prostą logikę decyzyjną przez `Switch`.