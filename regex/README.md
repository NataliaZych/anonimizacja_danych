# 📄 Rozwiązanie Anonimizacji Umów PL

System automatycznej anonimizacji polskich dokumentów prawnych (umów) z wykorzystaniem n8n i zaawansowanego wykrywania danych wrażliwych.

## 🎯 Co to jest?

To rozwiązanie pozwala na automatyczną anonimizację dokumentów PDF zawierających umowy, zastępując wszystkie dane osobowe i wrażliwe placeholderami. System przygotowuje dokumenty w formatach gotowych do użycia z językami AI (LLM - Large Language Models), zachowując przy tym pełną strukturę dokumentu i generując szczegółowe raporty.

## 🤖 Co to jest n8n?

**n8n** to open-source'owa platforma do automatyzacji workflow (przepływów pracy). Działa podobnie do narzędzi takich jak Zapier czy Microsoft Power Automate, ale jest bezpłatna i możesz uruchomić ją na własnym serwerze.

### Główne zalety n8n:
- 📊 **Wizualny interfejs** - tworzysz workflow przeciągając i upuszczając węzły
- 🔗 **Integracje** - ponad 400 gotowych integracji z różnymi serwisami
- 💻 **Kod** - możliwość użycia własnego kodu (Python, JavaScript)
- 🔒 **Prywatność** - dane pozostają na Twoim serwerze
- 🆓 **Open Source** - całkowicie darmowe dla własnego użytku

## 📋 Opis workflow "Anonimizacja Umów PL"

Workflow składa się z 5 głównych kroków (węzłów), które działają sekwencyjnie:

### 1️⃣ **On form submission** (Form Trigger)
**Co robi:** 
- Tworzy formularz webowy, przez który użytkownicy mogą przesyłać pliki PDF
- Odbiera przesłany plik PDF z umową
- Uruchamia workflow automatycznie po przesłaniu pliku

**Szczegóły techniczne:**
- Typ: `n8n-nodes-base.formTrigger`
- Metoda: Formularz HTTP z możliwością przesłania pliku
- Output: Przesłany plik PDF jako dane binarne

---

### 2️⃣ **Extract from File** (Ekstrakcja tekstu)
**Co robi:**
- Otwiera przesłany plik PDF
- Wyciąga z niego cały tekst
- Łączy wszystkie strony w jeden ciąg tekstowy
- Przygotowuje tekst do dalszego przetwarzania

**Szczegóły techniczne:**
- Typ: `n8n-nodes-base.extractFromFile`
- Operacja: `pdf`
- Opcja: `joinPages: true` - łączy wszystkie strony w jeden tekst
- Output: Tekst z dokumentu

---

### 3️⃣ **Data anonymization** (Anonimizacja danych) 🔍
**Co robi:**
Najbardziej zaawansowany krok - wykrywa i anonimizuje wszystkie dane wrażliwe w tekście.

#### Wykrywane typy danych:

1. **📇 Numery identyfikacyjne:**
   - **PESEL** - 11-cyfrowy numer PESEL
   - **NIP** - Numer Identyfikacji Podatkowej (10 cyfr, różne formaty)
   - **REGON** - Numer REGON (9 lub 14 cyfr)
   - **KRS** - Numer KRS (9-10 cyfr)
   - **DOWÓD** - Numer dowodu osobistego (format ABC123456)

2. **👤 Dane osobowe:**
   - **Imiona i nazwiska** - rozpoznaje polskie imiona i nazwiska na podstawie:
     - Bazy 100+ popularnych polskich imion
     - Wzorców końcówek nazwisk (ski, cki, ak, ek, etc.)
     - Kontekstu dokumentu prawnego
   - **Pełne nazwy osób** - wzorce typu "Imię Nazwisko"

3. **📞 Dane kontaktowe:**
   - **Email** - adresy email w standardowym formacie
   - **Telefon** - numery telefonów (polskie formaty +48, 0048, lokalne)

4. **🏠 Dane adresowe:**
   - **Adresy** - ulice z numerami domów
   - **Kod pocztowy** - format XX-XXX Miasto

5. **💰 Dane finansowe:**
   - **Kwoty** - kwoty w złotych (zł, PLN)
   - **Konta bankowe** - numery kont w formacie PL XX XXXX...

6. **📅 Daty:**
   - Format numeryczny (DD.MM.YYYY, DD-MM-YYYY)
   - Format słowny polski (np. "15 października 2025 r.")

7. **🏢 Dane firmowe:**
   - **Nazwy firm** - rozpoznaje typowe formy prawne (Sp. z o.o., S.A., Sp.J., Sp.K.)
   - **Numery polis** - formaty EU/2025/XXXXXX

**Jak działa anonimizacja:**
- Używa własnej implementacji opartej na zaawansowanych wyrażeniach regularnych (regex) do wykrywania danych
- to własna implementacja zaprojektowana specjalnie dla polskich dokumentów prawnych
- Sortuje wykryte elementy od końca, aby zachować prawidłowe pozycje po zastąpieniu
- Zastępuje dane odpowiednimi placeholderami:
  - `[IMIĘ NAZWISKO]` - dla danych osobowych
  - `[PESEL]` - dla numeru PESEL
  - `[EMAIL]` - dla adresów email
  - `[TELEFON]` - dla numerów telefonów
  - `[ADRES]` - dla adresów
  - `[NIP]`, `[REGON]`, `[KRS]` - dla numerów identyfikacyjnych
  - `[KWOTA]` - dla kwot
  - `[DATA]` - dla dat
  - itd.

**Szczegóły techniczne:**
- Typ: `n8n-nodes-base.code`
- Język: Python 3
- Metoda: Własna implementacja regex-based (nie używa biblioteki Presidio)
- Biblioteki: `re` (wyrażenia regularne)
- Output: JSON z oryginalnym tekstem, zanonimizowanym tekstem, listą wykrytych encji i statystykami

---

### 4️⃣ **Generate LLM-Ready Files** (Generowanie plików dla LLM) 📦
**Co robi:**
Przygotowuje dokument w 5 różnych formatach, gotowych do użycia z językami AI:

#### Generowane pliki:

1. **📄 `umowa_dla_llm_[timestamp].txt`**
   - Czysty tekst zanonimizowanej umowy
   - Gotowy do bezpośredniego użycia w promptach dla AI
   - Bez dodatkowych metadanych

2. **📝 `umowa_structured_[timestamp].md`**
   - Format Markdown z metadanymi w YAML frontmatter
   - Zawiera:
     - Typ dokumentu
     - Status anonimizacji
     - ID umowy
     - Datę przetworzenia
     - Statystyki wykrytych encji
     - Pełny tekst umowy

3. **📊 `umowa_data_[timestamp].json`**
   - Strukturalne dane JSON
   - Zawiera:
     - Metadane dokumentu
     - Statystyki anonimizacji
     - Oryginalny i zanonimizowany tekst
     - Listę wykrytych encji z pozycjami
     - Instrukcje dla LLM (rekomendowane zadania, kontekst)

4. **📋 `raport_pelny_[timestamp].md`**
   - Szczegółowy raport z anonimizacji
   - Zawiera:
     - Statystyki (liczba wykrytych encji, długość tekstu)
     - Tabelę wykrytych typów danych
     - Szczegółową listę wszystkich zmian
     - Opis metody anonimizacji
     - Instrukcje użycia dla LLM

5. **📝 `prompt_template_[timestamp].md`**
   - Gotowy szablon promptu dla LLM
   - Zawiera:
     - Kontekst dokumentu
     - Statystyki
     - Pełny tekst umowy
     - Instrukcje zadań do wykonania (analiza typu umowy, ekstrakcja klauzul, ocena ryzyka)
     - Format oczekiwanej odpowiedzi

**Wszystkie pliki są pakowane w archiwum ZIP:**
- `anonimizacja_llm_ready_[timestamp].zip`

**Szczegóły techniczne:**
- Typ: `n8n-nodes-base.code`
- Język: Python 3
- Output: JSON z metadanymi + plik binarny (ZIP)

---

### 5️⃣ **Display Report Form** (Wyświetlenie raportu) ✅
**Co robi:**
- Wyświetla użytkownikowi formularz z wynikami
- Pokazuje komunikat sukcesu
- Udostępnia do pobrania archiwum ZIP z wszystkimi plikami
- Zawiera niestandardowe style CSS dla czytelnego wyświetlania

**Szczegóły techniczne:**
- Typ: `n8n-nodes-base.form`
- Operacja: `completion` - formularz sukcesu
- Respond with: `returnBinary` - zwraca plik ZIP do pobrania
- Output: Formularz HTML z możliwością pobrania pliku

---

## 🔄 Przepływ danych (Flow)

```
Użytkownik przesyła PDF
         ↓
[On form submission] → odbiera plik
         ↓
[Extract from File] → wyciąga tekst z PDF
         ↓
[Data anonymization] → wykrywa i anonimizuje dane
         ↓
[Generate LLM-Ready Files] → tworzy pliki i ZIP
         ↓
[Display Report Form] → pokazuje wyniki i plik do pobrania
```

## 🚀 Jak używać?

### Uruchomienie rozwiązania:

1. **Zainstaluj Docker** (zobacz [WYMAGANIA_DOCKER.md](WYMAGANIA_DOCKER.md))

2. **Skonfiguruj zmienne środowiskowe:**
   ```bash
   cp .env.example .env
   # Edytuj .env i zmień hasło
   ```

3. **Przygotuj katalog workflows:**
   ```bash
   mkdir workflows
   cp "Anonimizacja Umów PL.json" workflows/
   ```

4. **Uruchom rozwiązanie:**
   ```bash
   docker-compose up -d
   ```

5. **Otwórz przeglądarkę:**
   - Wejdź na: http://localhost:5678
   - Zaloguj się (domyślnie: admin / hasło z .env)
   - Zaimportuj workflow z pliku `Anonimizacja Umów PL.json`

6. **Użyj workflow:**
   - Kliknij na workflow "Anonimizacja Umów PL"
   - Kliknij "Execute Workflow" lub użyj URL formularza
   - Prześlij plik PDF z umową
   - Poczekaj na przetworzenie
   - Pobierz archiwum ZIP z wynikami

## 📊 Przykładowe wyniki

Po przetworzeniu umowy otrzymasz:

- ✅ Statystyki anonimizacji (liczba wykrytych encji)
- ✅ Szczegółowy raport z listą wszystkich zmian
- ✅ 5 formatów plików gotowych do użycia z AI
- ✅ Gotowy szablon promptu dla LLM

## 🔒 Bezpieczeństwo

- Wszystkie dane są przetwarzane lokalnie (na Twoim serwerze)
- Brak wysyłania danych do zewnętrznych serwisów
- Możliwość ustawienia uwierzytelniania w n8n
- W środowisku produkcyjnym zalecane użycie HTTPS

## 🛠️ Wymagania techniczne

- Docker Desktop 4.0+ lub Docker Engine z Docker Compose
- Minimum 4 GB RAM (zalecane 8 GB)
- 10 GB wolnego miejsca na dysku
- Port 5678 dostępny (lub zmień w docker-compose.yml)

Szczegółowe informacje w [WYMAGANIA_DOCKER.md](WYMAGANIA_DOCKER.md)

## 📚 Dodatkowe informacje

### Struktura projektu:
```
anonimizacja_danych/
├── docker-compose.yml          # Konfiguracja Docker
├── .env.example                # Przykładowe zmienne środowiskowe
├── WYMAGANIA_DOCKER.md         # Szczegółowy przewodnik Docker
├── README.md                   # Ten plik
├── Anonimizacja Umów PL.json   # Workflow n8n
└── workflows/                  # Katalog na workflow (utwórz ręcznie)
```

### Wspierane formaty wejściowe:
- ✅ PDF (tekstowy, nie skan)

### Wspierane typy dokumentów:
- Umowy o pracę
- Umowy zlecenia
- Umowy o dzieło
- Umowy sprzedaży
- Umowy najmu
- Inne dokumenty prawne z polskimi danymi

## 🤝 Wsparcie

W przypadku problemów:
1. Sprawdź logi: `docker-compose logs -f`
2. Sprawdź [WYMAGANIA_DOCKER.md](WYMAGANIA_DOCKER.md) - sekcja "Rozwiązywanie problemów"
3. Upewnij się, że workflow jest aktywne w n8n
4. Sprawdź czy plik PDF zawiera tekst (nie jest to skan)

## 📝 Licencja

To rozwiązanie wykorzystuje:
- **n8n** - Apache License 2.0
- **Docker** - Apache License 2.0

Workflow i kod anonimizacji są udostępnione jako open source.

---

**Wersja:** 1.0  
**Ostatnia aktualizacja:** 2025

