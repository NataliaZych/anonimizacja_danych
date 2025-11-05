# 🔐 Anonimizacja Danych - Projekt

System automatycznej anonimizacji polskich dokumentów prawnych (umów) z wykorzystaniem zaawansowanych technik wykrywania danych wrażliwych. Projekt oferuje dwa komplementarne podejścia do anonimizacji: rozwiązanie oparte na wyrażeniach regularnych oraz wykorzystanie lokalnych modeli językowych.

## 📋 O Projekcie

Projekt **Anonimizacja Danych** to kompleksowe rozwiązanie służące do automatycznej identyfikacji i anonimizacji danych osobowych, identyfikacyjnych oraz innych wrażliwych informacji w polskich dokumentach prawnych. Głównym celem jest przygotowanie dokumentów do bezpiecznego użycia z językami AI (LLM) przy zachowaniu pełnej struktury dokumentu.

### ✨ Główne cechy

- 🔍 **Zaawansowane wykrywanie danych** - rozpoznaje PESEL, NIP, REGON, KRS, numery telefonów, email, adresy, imiona i nazwiska oraz inne dane wrażliwe
- 🛡️ **Pełna prywatność** - przetwarzanie lokalne, bez wysyłania danych do zewnętrznych serwisów
- 📄 **Wieloformatowe wyjście** - generuje dokumenty w formatach TXT, Markdown, JSON gotowe do użycia z LLM
- 📊 **Szczegółowe raporty** - tworzy kompleksowe raporty z procesu anonimizacji
- 🚀 **Gotowe do użycia** - prosty interfejs webowy do przesyłania dokumentów
- 🇵🇱 **Optymalizacja dla języka polskiego** - specjalnie zaprojektowane wzorce dla polskich dokumentów prawnych

## 🏗️ Struktura Projektu

```
anonimizacja_danych-1/
├── regex/                          # Rozwiązanie oparte na n8n i wyrażeniach regularnych
│   ├── Anonimizacja Umów PL.json  # Workflow n8n do anonimizacji
│   ├── docker-compose.yml          # Konfiguracja Docker
│   ├── README.md                   # Dokumentacja rozwiązania regex
│   └── WYMAGANIA_DOCKER.md         # Przewodnik instalacji Docker
├── lmstudio/                       # Rozwiązanie z lokalnymi modelami językowymi
│   └── README.md                   # Dokumentacja LM Studio
├── LICENSE                         # Licencja MIT
└── README.md                       # Ten plik
```

## 🔄 Dwa Podejścia do Anonimizacji

### 1️⃣ Rozwiązanie Regex-based (n8n)

**Lokalizacja:** `regex/`

Rozwiązanie oparte na platformie **n8n** wykorzystujące zaawansowane wyrażenia regularne do wykrywania danych wrażliwych. Własna implementacja zaprojektowana specjalnie dla polskich dokumentów prawnych.

#### Zalety:
- ⚡ **Szybkie działanie** - błyskawiczne przetwarzanie dokumentów
- 🎯 **Wysoka precyzja** - specjalistyczne wzorce dla polskich danych
- 💰 **Brak kosztów** - całkowicie darmowe, bez użycia zewnętrznych API
- 🔧 **Pełna kontrola** - możliwość modyfikacji wzorców wykrywania
- 🌐 **Interfejs webowy** - łatwy w użyciu formularz do przesyłania dokumentów

#### Kiedy używać:
- Szybka anonimizacja dużej liczby dokumentów
- Środowiska wymagające pełnej kontroli nad danymi
- Przetwarzanie dokumentów o przewidywalnej strukturze
- Ograniczenia budżetowe

#### ⚠️ Ograniczenia i Ostrzeżenia:
- **Rozwiązania mogą nie zawsze w pełni realizować cele anonimizacji danych** - systemy zostały przetestowane na różnych umowach, jednak inne, specyficzne umowy mogą zawierać dane wrażliwe, które nie zostaną wykryte
- **Szczególnie dotyczy rozwiązania regex-based** - wyrażenia regularne działają na podstawie predefiniowanych wzorców i mogą nie rozpoznać wszystkich wariantów danych, nietypowych formatów lub danych zapisanych w niestandardowy sposób
- **Zalecane jest ręczne sprawdzenie wyników** - zwłaszcza w przypadku dokumentów o nietypowej strukturze lub zawierających niestandardowe formaty danych
- **Nie gwarantujemy 100% skuteczności** - zawsze sprawdź zanonimizowane dokumenty przed ich użyciem, szczególnie w kontekście zgodności z RODO

**📖 Szczegółowa dokumentacja:** [regex/README.md](regex/README.md)

---

### 2️⃣ Rozwiązanie z Lokalnymi Modelami Językowymi (LM Studio)

**Lokalizacja:** `lmstudio/`

Rozwiązanie wykorzystujące lokalne modele językowe (LLM), w szczególności model **Bielik-11B-v2.6-Instruct** uruchamiany w **LM Studio**.

#### Zalety:
- 🧠 **Inteligentne rozumienie kontekstu** - lepsze rozpoznawanie w złożonych przypadkach
- 📝 **Zachowanie kontekstu prawnego** - zrozumienie terminologii prawnej
- 🔒 **Pełna prywatność** - wszystko działa lokalnie, offline
- 🎨 **Kreatywne zamienniki** - bardziej naturalne zastąpienia danych

#### Kiedy używać:
- Dokumenty o złożonej strukturze
- Potrzeba zrozumienia kontekstu prawnego
- Środowiska wymagające najwyższej prywatności
- Dokumenty z nietypowymi formatami danych

**📖 Szczegółowa dokumentacja:** [lmstudio/README.md](lmstudio/README.md)

---

## 🚀 Szybki Start

### Opcja 1: Rozwiązanie Regex-based (Rekomendowane dla początkujących)

1. **Zainstaluj Docker Desktop** (zobacz [regex/WYMAGANIA_DOCKER.md](regex/WYMAGANIA_DOCKER.md))

2. **Skonfiguruj zmienne środowiskowe:**
   ```bash
   cd regex
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
   - Prześlij plik PDF z umową przez formularz
   - Pobierz archiwum ZIP z wynikami

**📖 Pełna instrukcja:** [regex/README.md](regex/README.md)

---

### Opcja 2: Rozwiązanie z LM Studio

1. **Pobierz i zainstaluj LM Studio:**
   - Wejdź na: https://lmstudio.ai/
   - Pobierz wersję dla swojego systemu
   - Zainstaluj aplikację

2. **Pobierz model Bielik-11B-v2.6-Instruct:**
   - W LM Studio przejdź do zakładki "Search"
   - Wyszukaj: `Bielik-11B-v2.6-Instruct`
   - Wybierz odpowiedni format dla swojego systemu (GGUF, MLX, etc.)
   - Pobierz model

3. **Uruchom serwer lokalny:**
   - W LM Studio przejdź do zakładki "Local Server"
   - Kliknij "Start Server"
   - Zapiszesz adres API (domyślnie: `http://localhost:1234`)

4. **Zintegruj z workflow:**
   - Użyj API LM Studio w swoim workflow
   - Wysyłaj teksty do modelu w celu anonimizacji

**📖 Pełna instrukcja:** [lmstudio/README.md](lmstudio/README.md)

---

## 📊 Wykrywane Typy Danych (Rozwiązanie Regex-based)

System rozpoznaje szeroką gamę danych wrażliwych, w tym:

- **Numery identyfikacyjne:** PESEL, NIP, REGON, KRS, numery dowodów osobistych
- **Dane osobowe:** Imiona i nazwiska (baza 100+ polskich imion, rozpoznawanie końcówek nazwisk)
- **Dane kontaktowe:** Adresy email, numery telefonów (polskie formaty)
- **Dane adresowe:** Ulice z numerami, kody pocztowe z miastami
- **Dane finansowe:** Kwoty w złotych, numery kont bankowych
- **Daty:** Format numeryczny i słowny polski
- **Dane firmowe:** Nazwy firm z formami prawnymi, numery polis

📖 **Szczegółowa lista z opisami:** Zobacz sekcję "Wykrywane typy danych" w [regex/README.md](regex/README.md)

## 📦 Format Wyjściowy (Rozwiązanie Regex-based)

Po przetworzeniu dokumentu otrzymujesz archiwum ZIP z 5 plikami gotowymi do użycia z LLM:

- **TXT** - Czysty tekst zanonimizowanej umowy
- **Markdown** - Format z metadanymi w YAML frontmatter
- **JSON** - Strukturalne dane z metadanymi i instrukcjami dla LLM
- **Raport** - Szczegółowy raport z anonimizacji i listą zmian
- **Prompt Template** - Gotowy szablon promptu dla AI

📖 **Szczegółowy opis formatów:** Zobacz sekcję "Format Wyjściowy" w [regex/README.md](regex/README.md)

## 🛠️ Wymagania Techniczne

### Rozwiązanie Regex-based:
- Docker Desktop 4.0+ lub Docker Engine z Docker Compose
- Minimum 4 GB RAM (zalecane 8 GB)
- 10 GB wolnego miejsca na dysku
- Port 5678 dostępny

### Rozwiązanie LM Studio:
- System operacyjny: Windows, macOS lub Linux
- RAM: Minimum 16GB (model 11B wymaga około 8-12GB)
- Dysk: ~8-10GB wolnego miejsca na model
- Procesor: Nowoczesny procesor z obsługą AVX2 (zalecane)

## 🔒 Bezpieczeństwo

- ✅ Wszystkie dane są przetwarzane lokalnie (na Twoim serwerze/komputerze)
- ✅ Brak wysyłania danych do zewnętrznych serwisów (wersja regex)
- ✅ Możliwość ustawienia uwierzytelniania w n8n
- ✅ W środowisku produkcyjnym zalecane użycie HTTPS
- ⚠️ **WAŻNE:** Zawsze zmień domyślne hasło w pliku `.env`
- ⚠️ **WAŻNE:** Rozwiązania mogą nie zawsze w pełni realizować cele anonimizacji danych. Systemy zostały przetestowane na różnych umowach, jednak inne, specyficzne umowy mogą zawierać dane wrażliwe, które nie zostaną wykryte. Szczególnie dotyczy to rozwiązania regex-based. **Zawsze sprawdź ręcznie wyniki anonimizacji przed użyciem dokumentów.**

## 📚 Wspierane Formaty i Dokumenty

- **Format wejściowy:** PDF (tekstowy, nie skan)
- **Typy dokumentów:** Umowy o pracę, zlecenia, dzieło, sprzedaży, najmu oraz inne dokumenty prawne z polskimi danymi

📖 **Szczegóły:** Zobacz sekcję "Wspierane formaty i dokumenty" w [regex/README.md](regex/README.md)

## 🤝 Wsparcie i Rozwiązywanie Problemów

### Rozwiązanie Regex-based:
📖 **Szczegółowa pomoc:** Zobacz sekcję "Wsparcie" w [regex/README.md](regex/README.md) oraz [regex/WYMAGANIA_DOCKER.md](regex/WYMAGANIA_DOCKER.md)

### Rozwiązanie LM Studio:
📖 **Szczegółowa pomoc:** Zobacz sekcję "Pomoc i wsparcie" w [lmstudio/README.md](lmstudio/README.md)

## 📝 Licencja

Projekt jest udostępniony na licencji **MIT License**.

Zobacz plik [LICENSE](LICENSE) dla szczegółów.

### Użyte komponenty:
- **n8n** - Apache License 2.0
- **Docker** - Apache License 2.0
- **LM Studio** - Zgodnie z licencją producenta
- **Bielik-11B-v2.6-Instruct** - Zgodnie z licencją modelu

## 🔄 Porównanie Podejść

| Cecha | Regex-based | LM Studio |
|-------|-------------|-----------|
| **Szybkość** | ⚡⚡⚡⚡⚡ Bardzo szybkie | ⚡⚡⚡ Średnie |
| **Koszty** | 💰 Darmowe | 💰 Darmowe (lokalnie) |
| **Precyzja** | 🎯🎯🎯🎯 Wysoka | 🎯🎯🎯🎯🎯 Bardzo wysoka |
| **Rozumienie kontekstu** | ⭐⭐ Ograniczone | ⭐⭐⭐⭐⭐ Doskonałe |
| **Wymagania sprzętowe** | 💻💻 Niskie | 💻💻💻💻 Wysokie |
| **Konfiguracja** | ⚙️ Prosta | ⚙️⚙️ Średnia |
| **Offline** | ✅ Tak | ✅ Tak |
| **Wymagania sieci** | ❌ Brak | ❌ Brak (po pobraniu modelu) |

## 🎯 Które Rozwiązanie Wybrać?

**Wybierz Regex-based jeśli:**
- Potrzebujesz szybkiego rozwiązania do produkcji
- Masz ograniczone zasoby sprzętowe
- Dokumenty mają przewidywalną strukturę
- Chcesz pełną kontrolę nad wzorcami wykrywania

**Wybierz LM Studio jeśli:**
- Masz wydajny komputer z wystarczającą ilością RAM
- Dokumenty mają złożoną strukturę
- Potrzebujesz zrozumienia kontekstu prawnego
- Potrzebujesz bardziej naturalnych zamienników danych


## 👥 Autorzy

Copyright (c) 2025 PlainLegalZych

---

**Wersja:** 1.0  
**Ostatnia aktualizacja:** 2025

