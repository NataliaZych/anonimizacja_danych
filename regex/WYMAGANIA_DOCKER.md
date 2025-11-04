# 🐳 Wymagania Docker - Przewodnik

## Co to jest Docker?

Docker to narzędzie, które pozwala uruchamiać aplikacje w "kontenerach" - izolowanych środowiskach, które działają tak samo na każdym komputerze. Dzięki Docker możesz uruchomić nasze rozwiązanie do anonimizacji dokumentów bez potrzeby instalowania wielu skomplikowanych programów.

## 📋 Wymagania systemowe

### Minimalne wymagania:
- **System operacyjny**: Windows 10/11, macOS 10.15+, lub Linux
- **RAM**: Minimum 4 GB (zalecane 8 GB)
- **Dysk**: 10 GB wolnego miejsca
- **Procesor**: Dowolny nowoczesny procesor (Intel/AMD/Apple Silicon)

### Wymagane oprogramowanie:

#### 1. Docker Desktop
Docker Desktop to główne narzędzie potrzebne do uruchomienia rozwiązania.

**Windows:**
- Pobierz z: https://www.docker.com/products/docker-desktop/
- Wymaga Windows 10/11 (64-bit) z funkcją WSL 2
- Minimalna wersja: Docker Desktop 4.0+

**macOS:**
- Pobierz z: https://www.docker.com/products/docker-desktop/
- Działa na Intel i Apple Silicon (M1/M2/M3)
- Minimalna wersja: Docker Desktop 4.0+

**Linux:**
- Docker Engine można zainstalować przez menedżera pakietów:
  ```bash
  # Ubuntu/Debian
  sudo apt-get update
  sudo apt-get install docker.io docker-compose
  ```

#### 2. Docker Compose
Docker Compose jest zwykle dołączony do Docker Desktop. Jeśli nie, można go zainstalować osobno.

## 🚀 Instalacja krok po kroku

### Krok 1: Zainstaluj Docker Desktop

1. **Pobierz Docker Desktop:**
   - Wejdź na stronę: https://www.docker.com/products/docker-desktop/
   - Wybierz wersję odpowiednią dla swojego systemu
   - Pobierz i uruchom instalator

2. **Zainstaluj Docker Desktop:**
   - Uruchom pobrany instalator
   - Postępuj zgodnie z instrukcjami instalatora
   - Na Windows: upewnij się, że WSL 2 jest włączone
   - Zrestartuj komputer, jeśli zostaniesz o to poproszony

3. **Uruchom Docker Desktop:**
   - Znajdź ikonę Docker w menu Start (Windows) lub Applications (macOS)
   - Uruchom aplikację
   - Poczekaj, aż Docker się uruchomi (ikona w zasobniku systemowym przestanie migać)

4. **Zweryfikuj instalację:**
   - Otwórz terminal/powershell
   - Wpisz: `docker --version`
   - Powinno wyświetlić się coś w stylu: `Docker version 24.0.x`

### Krok 2: Sprawdź czy Docker działa

Otwórz terminal i wykonaj:
```bash
docker ps
```

Jeśli zobaczysz listę kontenerów (nawet pustą), oznacza to, że Docker działa poprawnie.

### Krok 3: Skonfiguruj rozwiązanie

1. **Skonfiguruj zmienne środowiskowe:**
   - Skopiuj plik `.env.example` do `.env`
   - Otwórz plik `.env` w edytorze tekstu
   - Zmień `N8N_PASSWORD` na bezpieczne hasło
   - Zapisz plik

2. **Przygotuj katalog na workflow:**
   - Utwórz katalog `workflows` w tym samym miejscu, co `docker-compose.yml`
   - Skopiuj plik `Anonimizacja Umów PL.json` do katalogu `workflows`

### Krok 4: Uruchom rozwiązanie

W terminalu, w katalogu z plikiem `docker-compose.yml`, wykonaj:

```bash
docker-compose up -d
```

To uruchomi rozwiązanie w tle. Możesz zamknąć terminal - aplikacja będzie działać.

## 🔍 Sprawdzanie czy wszystko działa

1. **Otwórz przeglądarkę internetową**
2. **Wejdź na adres:** http://localhost:5678

3. **Przy pierwszym uruchomieniu - Utwórz konto właściciela:**
   - Zobaczysz formularz "Set up owner account"
   - Wypełnij wymagane pola:
     * **Email**: Dowolny adres email (np. `admin@localhost`)
     * **First Name**: Twoje imię (np. `Admin`)
     * **Last Name**: Twoje nazwisko (np. `User`)
     * **Password**: Minimum 8 znaków, 1 cyfra i 1 wielka litera (np. `Admin123!`)
   - Opcjonalnie: Odznacz checkbox "I want to receive security and product updates" jeśli nie chcesz otrzymywać aktualizacji
   - Kliknij przycisk **"Next"** aby utworzyć konto
   - Po utworzeniu konta zobaczysz główny interfejs n8n

4. **Przy kolejnych uruchomieniach - Logowanie:**
   - Zaloguj się używając utworzonego konta (email i hasło z formularza setup)

**Uwaga:** Basic Auth (użytkownik `admin` / hasło z `.env`) działa dla API/webhooków, ale przy pierwszym uruchomieniu n8n wymaga utworzenia konta właściciela przez formularz setup. To jednorazowa operacja.

Jeśli widzisz interfejs n8n, wszystko działa poprawnie!

## 🛠️ Podstawowe komendy

### Uruchomienie rozwiązania:
```bash
docker-compose up -d
```

### Zatrzymanie rozwiązania:
```bash
docker-compose down
```

### Wyświetlenie logów:
```bash
docker-compose logs -f
```

### Restart rozwiązania:
```bash
docker-compose restart
```

### Sprawdzenie statusu:
```bash
docker-compose ps
```

## ⚠️ Rozwiązywanie problemów

### Problem: "Docker nie uruchamia się"
**Rozwiązanie:**
- Sprawdź, czy Docker Desktop jest uruchomiony (ikona w zasobniku systemowym)
- Upewnij się, że masz włączoną wirtualizację w BIOS/UEFI (Windows/Linux)
- Na Windows: sprawdź czy WSL 2 jest zainstalowane i włączone

### Problem: "Port 5678 jest zajęty"
**Rozwiązanie:**
- Sprawdź, czy coś innego używa portu 5678
- Możesz zmienić port w `docker-compose.yml`:
  ```yaml
  ports:
    - "5679:5678"  # Zmień 5679 na dowolny wolny port
  ```

### Problem: "Brak dostępu do internetu w kontenerze"
**Rozwiązanie:**
- Sprawdź połączenie internetowe
- Sprawdź ustawienia firewall
- Sprawdź ustawienia proxy w Docker Desktop

### Problem: "Błąd przy uruchamianiu"
**Rozwiązanie:**
- Sprawdź logi: `docker-compose logs`
- Upewnij się, że wszystkie pliki są w odpowiednich miejscach
- Sprawdź uprawnienia do plików (Linux/macOS)

## 📚 Dodatkowe zasoby

- Dokumentacja Docker: https://docs.docker.com/
- Dokumentacja n8n: https://docs.n8n.io/
- Docker Desktop: https://www.docker.com/products/docker-desktop/

## 🔒 Bezpieczeństwo

⚠️ **WAŻNE:**
- Zawsze zmień domyślne hasło w pliku `.env`
- Nie udostępniaj pliku `.env` publicznie
- W środowisku produkcyjnym użyj HTTPS zamiast HTTP
- Regularnie aktualizuj Docker Desktop

## 💡 Wskazówki

- Docker Desktop używa sporo zasobów - jeśli nie używasz, możesz go wyłączyć
- Regularnie czyść nieużywane obrazy: `docker system prune -a`
- Backup danych: dane są przechowywane w wolumenie Docker - można je eksportować za pomocą `docker volume` commands

