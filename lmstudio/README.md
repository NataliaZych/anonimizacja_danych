# LM Studio - Lokalne modele językowe

## Co to jest LM Studio?

LM Studio to darmowa aplikacja desktopowa, która umożliwia uruchamianie lokalnych modeli językowych (LLM) bezpośrednio na Twoim komputerze. Aplikacja jest dostępna na Windows, macOS i Linux. Główne zalety LM Studio:

- **Prywatność** - wszystkie dane pozostają na Twoim urządzeniu, nie są przesyłane do zewnętrznych serwerów
- **Offline** - działanie bez połączenia z internetem po pobraniu modelu
- **Darmowe** - brak kosztów za użycie API
- **Zgodność** - wsparcie dla różnych formatów modeli (GGUF, MLX, itp.)
- **Prosty interfejs** - łatwa instalacja i uruchamianie modeli

LM Studio jest szczególnie przydatne do zadań wymagających:
- Przetwarzania danych wrażliwych (np. anonimizacja dokumentów)
- Eksperymentowania z różnymi modelami
- Pracy w środowiskach wymagających pełnej kontroli nad danymi

## Model Bielik-11B-v2.6-Instruct

### Informacje o modelu

Model **Bielik-11B-v2.6-Instruct** to polski model językowy oparty na architekturze Llama, zoptymalizowany specjalnie dla języka polskiego. Model dostępny jest w różnych formatach (GGUF, MLX, itp.), które można wybrać w zależności od posiadanego systemu operacyjnego.

**Dlaczego ten model?**
- Doskonałe wyniki w anonimizacji umów w języku polskim
- Zrozumienie kontekstu prawnego i terminologii polskiej
- Dostępność w różnych formatach dla różnych platform
- Format 8-bit zapewnia równowagę między wydajnością a jakością

### Instalacja LM Studio

1. **Pobierz LM Studio:**
   - Odwiedź stronę: https://lmstudio.ai/
   - Pobierz wersję odpowiednią dla Twojego systemu operacyjnego
   - Zainstaluj aplikację zgodnie z instrukcjami

2. **Uruchom LM Studio** i przejdź do zakładki "Search" lub "Models"

### Dodawanie modelu Bielik-11B-v2.6-Instruct

#### Metoda 1: Wyszukiwanie w LM Studio (zalecana)

1. **W LM Studio:**
   - Otwórz zakładkę "Search" lub "Models"
   - W polu wyszukiwania wpisz: `Bielik-11B-v2.6-Instruct`
   - Wybierz odpowiedni format dla swojego systemu (GGUF dla Windows/Linux, MLX dla macOS z Apple Silicon)
   - Wybierz model z listy wyników
   - Kliknij "Download" - model zostanie automatycznie pobrany i zainstalowany

#### Metoda 2: Pobranie z Hugging Face

Jeśli model nie jest dostępny w wyszukiwarce LM Studio:

1. **Otwórz Hugging Face:**
   - Przejdź na: https://huggingface.co/models
   - Wyszukaj: `Bielik-11B-v2.6-Instruct`
   - Wybierz odpowiedni format dla swojego systemu

2. **Pobierz model:**
   - Skopiuj ścieżkę do modelu (np. `username/model-name`)
   - W LM Studio przejdź do "Settings" → "Models"
   - Wybierz opcję "Open Models Folder" aby otworzyć folder z modelami
   - Pobierz pliki modelu do tego folderu

3. **Importuj w LM Studio:**
   - W LM Studio kliknij "Refresh" lub przeładuj listę modeli
   - Model powinien pojawić się na liście dostępnych modeli

### Uruchamianie modelu

1. **Wybierz model:**
   - W zakładce "Chat" lub "Local Server"
   - Z listy rozwijanej wybierz `Bielik-11B-v2.6-Instruct` (lub odpowiednią wersję formatu dla Twojego systemu)

2. **Uruchom serwer lokalny (jeśli potrzebny dla integracji):**
   - Przejdź do zakładki "Local Server"
   - Kliknij "Start Server"
   - Zapiszesz adres API (domyślnie: `http://localhost:1234`)

3. **Skonfiguruj parametry (opcjonalnie):**
   - Temperature: 0.7-0.9 dla bardziej kreatywnych odpowiedzi
   - Max tokens: dostosuj w zależności od długości tekstu
   - Context window: upewnij się, że jest wystarczająco duża dla długich dokumentów

## Wykorzystanie w anonimizacji umów

Model **Bielik-11B-v2.6-Instruct** sprawdził się bardzo dobrze w zadaniach anonimizacji umów w języku polskim dzięki:

- **Wysokiej jakości rozpoznawania** danych wrażliwych (imiona, nazwiska, adresy, numery PESEL, NIP, itp.)
- **Zachowaniu kontekstu prawnego** podczas zastępowania danych
- **Poprawności językowej** w generowanych zamiennikach
- **Wydajności** dzięki optymalizacji formatu dla różnych platform

### Przykładowe zastosowanie

Model może być wykorzystany jako:
- Alternatywa dla zewnętrznych API w procesie anonimizacji
- Lokalne rozwiązanie zapewniające pełną prywatność danych
- Narzędzie do weryfikacji jakości anonimizacji wykonanej innymi metodami

## Wymagania systemowe

- **System operacyjny:** Windows, macOS lub Linux
- **RAM:** Minimum 16GB (model 11B wymaga około 8-12GB)
- **Dysk:** ~8-10GB wolnego miejsca na model
- **Procesor:** Nowoczesny procesor z obsługą AVX2 (zalecane)
- **Format modelu:** Wybierz odpowiedni format dla swojego systemu:
  - **GGUF** - dla Windows i Linux
  - **MLX** - dla macOS z Apple Silicon (M1/M2/M3)
  - **Inne formaty** - zgodnie z obsługą Twojego systemu

## Pomoc i wsparcie

- Dokumentacja LM Studio: https://lmstudio.ai/docs
- Community: Discord LM Studio
- Hugging Face: https://huggingface.co/ - źródło modeli

---

**Uwaga:** Wybierz odpowiedni format modelu dla swojego systemu operacyjnego. Format MLX jest zoptymalizowany dla macOS z Apple Silicon, podczas gdy format GGUF działa najlepiej na Windows i Linux.

