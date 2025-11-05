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

1. **Skonfiguruj zmienne środowiskowe (opcjonalne, ale zalecane):**
   
   **Uwaga:** Plik `.env` nie jest wymagany, ponieważ `docker-compose.yml` ma wartości domyślne. Jednak dla bezpieczeństwa zalecane jest utworzenie pliku `.env` z silniejszym hasłem.
   
   Utwórz plik `.env` w katalogu z `docker-compose.yml`:
   ```bash
   # Utwórz plik .env i dodaj:
   N8N_USER=admin
   N8N_PASSWORD=TwojeBezpieczneHaslo123!
   ```
   
   Jeśli nie utworzysz pliku `.env`, zostaną użyte wartości domyślne:
   - Użytkownik Basic Auth: `admin`
   - Hasło Basic Auth: `admin123` ⚠️ **To hasło jest słabe - zmień je w środowisku produkcyjnym!**

2. **Przygotuj katalog na workflow:**
   - Utwórz katalog `workflows` w tym samym miejscu, co `docker-compose.yml`
   - **Opcjonalnie:** Skopiuj plik `Anonimizacja Umów PL.json` do katalogu `workflows` (możesz też zaimportować go później przez interfejs n8n)

### Krok 4: Uruchom rozwiązanie

W terminalu, w katalogu z plikiem `docker-compose.yml`, wykonaj:

```bash
docker-compose up -d
```

To uruchomi rozwiązanie w tle. Możesz zamknąć terminal - aplikacja będzie działać.

## 🔍 Sprawdzanie czy wszystko działa

### 📚 Wyjaśnienie: Dwie warstwy uwierzytelniania w n8n

Przed rozpoczęciem logowania, ważne jest zrozumienie, że n8n używa **dwóch niezależnych systemów uwierzytelniania**:

#### 🏢 **Analogia do budynku:**

Wyobraź sobie budynek biurowy:
- **Basic Auth** = **Dozorca na parterze** - sprawdza czy w ogóle masz prawo wejść do budynku
- **Konto właściciela n8n** = **Twoje biuro na piętrze** - po wejściu do budynku musisz jeszcze otworzyć drzwi do swojego biura

#### 🔐 **Warstwa 1: Basic Auth (HTTP Authentication)**

**Co to jest:**
- To jest **standardowy protokół HTTP** używany przez serwery WWW
- Działa **na poziomie serwera HTTP** (przed n8n)
- To jest **zewnętrzna brama** - chroni dostęp do całego serwera n8n
- Ustawiana przez zmienne środowiskowe w Docker

**💡 Czy Basic Auth jest potrzebny do testów lokalnych?**
- **NIE** - dla testów lokalnych na `localhost` możesz go wyłączyć
- **TAK** - zalecany w środowisku produkcyjnym lub gdy serwer jest dostępny z sieci
- **Dla testów lokalnych:** Możesz pominąć Basic Auth i przejść od razu do tworzenia konta właściciela n8n

**Jak wyłączyć Basic Auth dla testów:**
W pliku `docker-compose.yml` zmień linię 11:
```yaml
# Z:
N8N_BASIC_AUTH_ACTIVE=true

# Na:
N8N_BASIC_AUTH_ACTIVE=false
```

Po wyłączeniu Basic Auth:
- Nie zobaczysz okna Basic Auth
- Przejdziesz od razu do formularza "Set up owner account" (przy pierwszym uruchomieniu)
- Lub do formularza logowania n8n (przy kolejnych uruchomieniach)
- ⚠️ **Uwaga:** Tylko dla testów lokalnych! W środowisku produkcyjnym zawsze używaj Basic Auth.

**Kiedy jest używane:**
- **Tylko gdy włączony** (`N8N_BASIC_AUTH_ACTIVE=true`) - przy każdym żądaniu do serwera n8n
- Przed załadowaniem jakiejkolwiek strony n8n (jeśli włączony)
- Chroni przed dostępem nieautoryzowanych osób do interfejsu
- Można wyłączyć dla testów lokalnych (`N8N_BASIC_AUTH_ACTIVE=false`)

**Jak działa:**
```
Przeglądarka → "Daj mi stronę n8n" 
    ↓
Serwer HTTP: "Najpierw pokaż mi login i hasło Basic Auth!"
    ↓
Przeglądarka pokazuje okno logowania Basic Auth
    ↓
Użytkownik wpisuje: admin / admin123
    ↓
Serwer HTTP: "OK, możesz wejść do n8n"
```

**Gdzie jest skonfigurowane:**
- W `docker-compose.yml` linia 11-13:
  ```yaml
  N8N_BASIC_AUTH_ACTIVE=true
  N8N_BASIC_AUTH_USER=${N8N_USER:-admin}
  N8N_BASIC_AUTH_PASSWORD=${N8N_PASSWORD:-admin123}
  ```

**Charakterystyka:**
- ✅ Proste - jeden login, jedno hasło dla wszystkich
- ✅ Szybkie - nie wymaga bazy danych
- ✅ Chroni cały interfejs n8n
- ❌ Nie ma indywidualnych kont użytkowników
- ❌ To NIE jest konto użytkownika w n8n

---

#### 👤 **Warstwa 2: Konto właściciela n8n (Owner Account)**

**Co to jest:**
- To jest **wewnętrzny system użytkowników** aplikacji n8n
- Działa **wewnątrz aplikacji n8n** (po przejściu Basic Auth)
- To jest **konto użytkownika** w bazie danych n8n
- Tworzone przez formularz w interfejsie n8n

**Kiedy jest używane:**
- **Po** przejściu przez Basic Auth
- Do logowania do interfejsu n8n
- Do zarządzania workflow, ustawieniami, użytkownikami
- n8n może mieć wiele kont użytkowników (owner, admin, member)

**Jak działa:**
```
Po przejściu Basic Auth → Zobaczysz stronę n8n
    ↓
n8n: "Witaj! Czy masz już konto?"
    ↓
Przy pierwszym uruchomieniu: "Nie? Stwórz konto właściciela!"
    ↓
Formularz: Email, Imię, Nazwisko, Hasło
    ↓
Konto zapisane w bazie danych n8n (w Docker volume)
    ↓
Od teraz logujesz się tym kontem
```

**Gdzie jest skonfigurowane:**
- **NIE** w `docker-compose.yml`!
- Tworzone przez formularz "Set up owner account"
- Przechowywane w Docker volume `n8n_data` (baza danych SQLite)

**Charakterystyka:**
- ✅ Indywidualne konta dla każdego użytkownika
- ✅ Możliwość dodawania wielu użytkowników
- ✅ Zarządzanie uprawnieniami
- ✅ Integracja z systemem n8n (workflow, ustawienia, historie)
- ⚠️ Wymaga przejścia przez Basic Auth najpierw (jeśli Basic Auth jest włączony)

---

#### 🔄 **Przepływ logowania - krok po kroku:**

**⚠️ Uwaga:** Poniższe przepływy zakładają, że Basic Auth jest włączony (`N8N_BASIC_AUTH_ACTIVE=true`). Jeśli wyłączyłeś Basic Auth dla testów, pomiń kroki dotyczące Basic Auth.

**Przy pierwszym uruchomieniu (z Basic Auth):**

```
1. Otwierasz http://localhost:5678
   ↓
2. PRZEGLĄDARKA: "Chcę zobaczyć n8n"
   ↓
3. SERWER HTTP: "Najpierw Basic Auth!"
   ↓
4. Okno Basic Auth (standardowe okno przeglądarki)
   Wpisujesz: admin / admin123
   ↓
5. SERWER HTTP: "OK, Basic Auth przeszedł!"
   ↓
6. n8n: "Witaj! Widzę, że to pierwszy raz..."
   ↓
7. Formularz "Set up owner account"
   Wpisujesz: email, imię, nazwisko, hasło (np. Admin123!)
   ↓
8. n8n: "Konto utworzone! Zapisane w bazie danych"
   ↓
9. ✅ Jesteś w interfejsie n8n!
```

**Przy pierwszym uruchomieniu (bez Basic Auth - tylko dla testów lokalnych):**

```
1. Otwierasz http://localhost:5678
   ↓
2. n8n: "Witaj! Widzę, że to pierwszy raz..."
   ↓
3. Formularz "Set up owner account"
   Wpisujesz: email, imię, nazwisko, hasło (np. Admin123!)
   ↓
4. n8n: "Konto utworzone! Zapisane w bazie danych"
   ↓
5. ✅ Jesteś w interfejsie n8n!
```

**Przy kolejnych uruchomieniach (z Basic Auth):**

```
1. Otwierasz http://localhost:5678
   ↓
2. Basic Auth: admin / admin123
   ↓
3. n8n: "Witaj! Zaloguj się"
   ↓
4. Formularz logowania n8n
   Wpisujesz: email i hasło z kroku 7 (z pierwszego uruchomienia)
   ↓
5. ✅ Jesteś w interfejsie n8n!
```

**Przy kolejnych uruchomieniach (bez Basic Auth - tylko dla testów lokalnych):**

```
1. Otwierasz http://localhost:5678
   ↓
2. n8n: "Witaj! Zaloguj się"
   ↓
3. Formularz logowania n8n
   Wpisujesz: email i hasło z formularza setup
   ↓
4. ✅ Jesteś w interfejsie n8n!
```

---

#### ⚠️ **WAŻNE RÓŻNICE:**

| Cecha | Basic Auth | Konto właściciela n8n |
|-------|------------|----------------------|
| **Gdzie** | Poziom HTTP/serwera | Poziom aplikacji n8n |
| **Gdzie skonfigurowane** | `docker-compose.yml` / `.env` | Formularz w interfejsie n8n |
| **Gdzie przechowywane** | Zmienne środowiskowe | Baza danych n8n (Docker volume) |
| **Ile kont** | 1 (wspólne dla wszystkich) | Wielu (indywidualne) |
| **Kiedy używane** | PRZED każdą stroną n8n | PO przejściu Basic Auth |
| **Cel** | Chronić serwer przed dostępem | Zarządzać użytkownikami w n8n |
| **To samo?** | ❌ NIE | ❌ NIE |

---

#### 💡 **Dlaczego n8n używa obu?**

1. **Basic Auth** = Prosta ochrona "na wejściu" - szybka, nie wymaga bazy danych
2. **Konto n8n** = Właściwy system użytkowników z uprawnieniami, historią, workflow

To jest **bezpieczniejsza architektura** - nawet jeśli ktoś znajdzie hasło do Basic Auth, musi jeszcze znać konto użytkownika n8n.

---

#### ✅ **Podsumowanie - co jest wymagane, a co opcjonalne:**

| Element | Czy wymagany? | Uwagi |
|---------|---------------|-------|
| **Konto właściciela n8n** | ✅ **ZAWSZE** | n8n nie działa bez konta właściciela. To jest główny system użytkowników aplikacji. Musisz je utworzyć przy pierwszym uruchomieniu. |
| **Basic Auth** | ⚠️ **OPCJONALNY** | Można wyłączyć dla testów lokalnych (`N8N_BASIC_AUTH_ACTIVE=false`). **Zalecany w produkcji** dla dodatkowej warstwy bezpieczeństwa. |

**W skrócie:**
- **Konto n8n = ZAWSZE** - bez niego n8n nie zadziała
- **Basic Auth = OPCJONALNY** - możesz wyłączyć dla testów, ale warto mieć w produkcji

---

1. **Otwórz przeglądarkę internetową**
2. **Wejdź na adres:** http://localhost:5678

3. **Przy pierwszym uruchomieniu - Uwierzytelnianie:**
   
   **🔐 Ważne:** n8n używa **dwóch warstw** uwierzytelniania:
   
   **a) Basic Auth (HTTP) - pierwszy ekran:**
   - Zobaczysz okno logowania Basic Auth
   - Użytkownik: `admin` (lub wartość z pliku `.env` / zmiennej `N8N_USER`)
   - Hasło: `admin123` (lub wartość z pliku `.env` / zmiennej `N8N_PASSWORD`)
   - Wpisz dane i kliknij "Sign in"
   
   **b) Utworzenie konta właściciela n8n - drugi ekran:**
   - Po zalogowaniu Basic Auth zobaczysz formularz "Set up owner account"
   - **To jest osobne konto** - nie to samo co Basic Auth!
   - Wypełnij wymagane pola:
     * **Email**: Dowolny adres email (np. `admin@localhost`)
     * **First Name**: Twoje imię (np. `Admin`)
     * **Last Name**: Twoje nazwisko (np. `User`)
     * **Password**: Minimum 8 znaków, 1 cyfra i 1 wielka litera (np. `Admin123!`)
   - Opcjonalnie: Odznacz checkbox "I want to receive security and product updates" jeśli nie chcesz otrzymywać aktualizacji
   - Kliknij przycisk **"Next"** aby utworzyć konto
   - Po utworzeniu konta zobaczysz główny interfejs n8n

4. **Przy kolejnych uruchomieniach - Logowanie:**
   - **Krok 1:** Basic Auth - `admin` / hasło z `.env` (lub domyślne `admin123`)
   - **Krok 2:** Logowanie do n8n - email i hasło z formularza setup (konto właściciela)

**Uwaga:** Basic Auth chroni cały interfejs n8n, a konto właściciela to konto użytkownika w systemie n8n. To są dwa różne mechanizmy uwierzytelniania.

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

