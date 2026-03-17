# 🏋️ FitBoard

> Personalny dashboard do śledzenia treningów siłowych z wizualizacją postępów.

**Stack:** Vue 3 + Laravel 11 + SQLite  
**Poziom:** Projekt studencki (4/10)  
**Funkcjonalności:** CRUD treningów, wykresy postępów, heatmapa aktywności

---

## 📸 Zrzuty Ekranu

### Dashboard
```
┌──────────────────────────────────────────┐
│  FitBoard                    [Avatar] ▼  │
├──────────────────────────────────────────┤
│                                          │
│  📊 Twoje statystyki                     │
│  ┌──────┐ ┌──────┐ ┌──────┐             │
│  │  12  │ │  3   │ │ 45min│             │
│  │treningów│ │ten tydz│ │śr. czas│        │
│  └──────┘ └──────┘ └──────┘             │
│                                          │
│  🔥 Heatmapa aktywności                  │
│  ┌─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┐            │
│  │░│░│█│░│ │█│░│ │█│█│░│ │            │
│  │ │█│░│ │█│░│░│█│░│ │█│░│            │
│  └─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘            │
│                                          │
│  📈 Progres: Wyciskanie na ławce         │
│  ┌──────────────────────────┐            │
│  │    ╱‾‾‾╲                 │            │
│  │   ╱     ╲___╱‾‾          │            │
│  │  ╱                       │            │
│  └──────────────────────────┘            │
│                                          │
│  [+ Dodaj trening]                       │
└──────────────────────────────────────────┘
```

---

## 🚀 Funkcjonalności

### ✅ MVP (Must Have)
- [x] **Autentykacja** - rejestracja, logowanie, wylogowanie
- [x] **Dashboard** - statystyki, heatmapa aktywności, wykres progresu
- [x] **Dodawanie treningu** - formularz z ćwiczeniami, seriami, powtórzeniami
- [x] **Historia treningów** - lista z paginacją i filtrowaniem
- [x] **Wykresy postępów** - liniowy wykres ciężaru w czasie

### 🟡 Should Have
- [ ] Heatmapa aktywności (GitHub-style)
- [ ] Edycja i usuwanie treningów
- [ ] Filtrowanie po grupie mięśniowej
- [ ] Responsywny design

### 🔵 Could Have
- [ ] Streak counter (dni z rzędu z treningiem)
- [ ] Porównanie tygodni
- [ ] AI podsumowanie treningowe

---

## 🛠️ Stack Technologiczny

### Backend
| Technologia | Wersja | Uzasadnienie |
|-------------|--------|--------------|
| **PHP** | 8.2+ | Wymaganie Laravel 11 |
| **Laravel** | 11.x | Najnowsza wersja, mniej boilerplate |
| **SQLite** | 3.x | Zero konfiguracji na dev |
| **Sanctum** | ^4.0 | Prosta autentykacja API |
| **Eloquent** | - | ORM z relacjami |

### Frontend
| Technologia | Wersja | Uzasadnienie |
|-------------|--------|--------------|
| **Vue** | 3.4+ | Composition API, reactivity |
| **Vue Router** | 4.x | Routing SPA |
| **Pinia** | 2.x | State management |
| **Axios** | 1.x | HTTP client |
| **ApexCharts** | 3.x | Wykresy (prostsze niż Chart.js) |
| **Vuetify** | 3.x | Gotowe komponenty UI |

---

## 📁 Struktura Projektu

```
fitboard-project/
├── backend/                 # Laravel 11 API
│   ├── app/
│   │   ├── Http/
│   │   │   ├── Controllers/Api/   # Kontrolery API
│   │   │   ├── Requests/          # Walidacja formularzy
│   │   │   └── Resources/         # Formatowanie JSON
│   │   └── Models/                # Eloquent Models
│   ├── database/
│   │   ├── migrations/            # Migracje
│   │   └── seeders/               # Predefiniowane ćwiczenia
│   └── routes/
│       └── api.php                # Trasy API
│
├── frontend/                # Vue 3 SPA
│   ├── src/
│   │   ├── components/            # Reużywalne komponenty
│   │   ├── views/                 # Widoki stron
│   │   ├── services/              # API client
│   │   ├── stores/                # Pinia stores
│   │   └── router/                # Konfiguracja routingu
│   └── package.json
│
└── docs/                    # Dokumentacja
    ├── fitboard-architecture.md   # Architektura systemu
    ├── fitboard-api.md            # Dokumentacja API
    └── fitboard-setup.md          # Instrukcja instalacji
```

---

## 🔌 API Endpoints

| Metoda | Endpoint | Opis | Auth |
|--------|----------|------|------|
| POST | `/api/register` | Rejestracja | ❌ |
| POST | `/api/login` | Logowanie | ❌ |
| POST | `/api/logout` | Wylogowanie | ✅ |
| GET | `/api/workouts` | Lista treningów | ✅ |
| POST | `/api/workouts` | Nowy trening | ✅ |
| GET | `/api/workouts/{id}` | Szczegóły treningu | ✅ |
| PUT | `/api/workouts/{id}` | Aktualizacja | ✅ |
| DELETE | `/api/workouts/{id}` | Usunięcie | ✅ |
| GET | `/api/exercises` | Lista ćwiczeń | ✅ |
| GET | `/api/stats/dashboard` | Statystyki | ✅ |
| GET | `/api/stats/progress/{id}` | Progres ćwiczenia | ✅ |
| GET | `/api/stats/activity` | Heatmapa | ✅ |

Pełna dokumentacja API: [`fitboard-api.md`](./fitboard-api.md)

---

## 🗄️ Model Danych

### Tabele

| Tabela | Opis | Rekordy |
|--------|------|---------|
| `users` | Użytkownicy aplikacji | ~N |
| `exercises` | Predefiniowane ćwiczenia | 27 |
| `workouts` | Treningi użytkowników | ~N * 50 |
| `exercise_logs` | Szczegóły serii | ~N * 50 * 5 |

### Relacje
```
User 1---* Workout 1---* ExerciseLog *---1 Exercise
```

---

## 🚀 Instalacja

### Szybki Start

```bash
# 1. Backend
cd backend
composer install
cp .env.example .env
php artisan key:generate
touch database/database.sqlite
php artisan migrate --seed
php artisan serve

# 2. Frontend (w nowym terminalu)
cd frontend
npm install
npm run dev
```

Szczegółowe instrukcje: [`fitboard-setup.md`](./fitboard-setup.md)

---

## 🧪 Testy

```bash
# Backend tests
cd backend
php artisan test

# Testy konkretnego pliku
php artisan test --filter=WorkoutTest
```

Plan testów: [`fitboard-testing.md`](./fitboard-testing.md)

---

## 📊 Statystyki Projektu

| Metryka | Wartość |
|---------|---------|
| Endpointy API | 12 |
| Tabele w bazie | 4 |
| Modele Eloquent | 4 |
| Kontrolery | 4 |
| Widoki Vue | 4 |
| Komponenty Vue | ~10 |
| Linie kodu (est.) | ~3000 |

---

## 🎓 Wartość Edukacyjna

Ten projekt demonstruje:

| Umiejętność | Implementacja |
|-------------|---------------|
| **REST API** | Laravel - pełny CRUD |
| **Autentykacja** | Sanctum tokens |
| **Walidacja** | Form Requests |
| **Relacje DB** | Eloquent ORM |
| **Frontend** | Vue 3 Composition API |
| **State Management** | Pinia |
| **Routing** | Vue Router |
| **HTTP Client** | Axios + interceptors |
| **Wizualizacja** | ApexCharts |
| **Komponenty UI** | Vuetify |

---

## 📝 TODO

### Faza 1: Setup
- [ ] Inicjalizacja Laravel 11
- [ ] Konfiguracja Sanctum
- [ ] Migracje i seedery

### Faza 2: Auth API
- [ ] Register endpoint
- [ ] Login endpoint
- [ ] Logout endpoint

### Faza 3: Core API
- [ ] CRUD Workouts
- [ ] Lista ćwiczeń
- [ ] Stats endpoints

### Faza 4: Frontend
- [ ] Setup Vue 3
- [ ] Auth views
- [ ] Dashboard
- [ ] Workout form
- [ ] History list

### Faza 5: Polish
- [ ] Wykresy
- [ ] Heatmapa
- [ ] Testy
- [ ] Dokumentacja

---

## 📄 Licencja

Projekt studencki - Projektowanie Aplikacji Internetowych.

---

## 🤝 Współtwórcy

Projekt wykonany w ramach zajęć z PAI.

---

## 📚 Dokumentacja

- [`fitboard-architecture.md`](./fitboard-architecture.md) - Architektura systemu, ERD, uzasadnienie wyborów
- [`fitboard-api.md`](./fitboard-api.md) - Dokumentacja wszystkich endpointów
- [`fitboard-setup.md`](./fitboard-setup.md) - Instrukcja instalacji krok po kroku
- [`fitboard-testing.md`](./fitboard-testing.md) - Plan testów

---

**Status:** 🚧 Dokumentacja gotowa, kod w trakcie implementacji
