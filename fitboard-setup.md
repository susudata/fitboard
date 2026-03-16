# 🛠️ FitBoard - Instrukcja Instalacji

Kompletny przewodnik krok po kroku do uruchomienia backendu FitBoard.

---

## 📋 Wymagania Wstępne

| Wymaganie | Wersja | Link |
|-----------|--------|------|
| PHP | 8.2+ | https://www.php.net/downloads |
| Composer | 2.x | https://getcomposer.org/download/ |
| SQLite | 3.x | (wbudowane w PHP) |
| Node.js | 18+ | https://nodejs.org/ |
| NPM | 9+ | (instalowane z Node.js) |

### Sprawdzenie wersji
```bash
php -v          # PHP 8.2+
composer -v     # Composer 2.x
node -v         # v18+
npm -v          # 9+
```

---

## 🚀 Instalacja Backendu (Laravel)

### Krok 1: Stworzenie Projektu

```bash
# Przejdź do katalogu głównego projektu
cd fitboard-project

# Stwórz nowy projekt Laravel
cd .. && composer create-project laravel/laravel backend

# Lub jeśli już masz katalog fitboard-project:
composer create-project laravel/laravel backend
```

### Krok 2: Konfiguracja Środowiska

```bash
cd backend

# Skopiuj plik środowiska
cp .env.example .env

# Wygeneruj klucz aplikacji
php artisan key:generate
```

Edytuj plik `.env`:

```env
APP_NAME=FitBoard
APP_ENV=local
APP_KEY=base64:...  # wygenerowane automatycznie
APP_DEBUG=true
APP_URL=http://localhost:8000

# Baza danych SQLite
DB_CONNECTION=sqlite
# DB_DATABASE=laravel  # zakomentowane - SQLite używa pliku

# Sanctum
SANCTUM_STATEFUL_DOMAINS=localhost:5173
SESSION_DOMAIN=localhost

# Frontend URL (dla CORS)
FRONTEND_URL=http://localhost:5173
```

### Krok 3: Konfiguracja SQLite

```bash
# Stwórz katalog bazy danych (jeśli nie istnieje)
mkdir -p database

# Stwórz pusty plik SQLite
touch database/database.sqlite
```

### Krok 4: Instalacja Sanctum

```bash
# Zainstaluj Sanctum
composer require laravel/sanctum

# Opublikuj konfigurację
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"

# Uruchom migracje Sanctum
php artisan migrate
```

### Krok 5: Konfiguracja CORS

Edytuj `config/cors.php`:

```php
<?php

return [
    'paths' => ['api/*', 'sanctum/csrf-cookie'],
    'allowed_methods' => ['*'],
    'allowed_origins' => [env('FRONTEND_URL', 'http://localhost:5173')],
    'allowed_origins_patterns' => [],
    'allowed_headers' => ['*'],
    'exposed_headers' => [],
    'max_age' => 0,
    'supports_credentials' => true,
];
```

### Krok 6: Migracje i Seedery

```bash
# Uruchom wszystkie migracje
php artisan migrate

# Uruchom seedery (predefiniowane ćwiczenia)
php artisan db:seed --class=ExerciseSeeder

# Lub wszystko na raz (jeśli skonfigurujesz DatabaseSeeder)
php artisan migrate --seed
```

### Krok 7: Uruchomienie Serwera

```bash
# Uruchom serwer deweloperski Laravel
php artisan serve

# Backend będzie dostępny pod:
# http://localhost:8000
```

---

## 💻 Instalacja Frontendu (Vue 3)

### Krok 1: Stworzenie Projektu Vue

```bash
# W katalogu głównym projektu (obok backend/)
npm create vue@latest frontend

# Wybierz opcje:
# ✔ Project name: frontend
# ✔ Add TypeScript? No
# ✔ Add JSX Support? No
# ✔ Add Vue Router? Yes
# ✔ Add Pinia? Yes
# ✔ Add Vitest? No (można dodać później)
# ✔ Add Cypress? No
# ✔ Add ESLint? Yes
# ✔ Add Prettier? Yes
```

### Krok 2: Instalacja Zależności

```bash
cd frontend

# Podstawowe zależności
npm install

# Axios (HTTP client)
npm install axios

# ApexCharts (wykresy)
npm install apexcharts vue3-apexcharts

# Vuetify 3 (UI components)
npm install vuetify@next

# Data formatter
npm install date-fns
```

### Krok 3: Konfiguracja Axios

Stwórz plik `src/services/api.js`:

```javascript
import axios from 'axios'

const api = axios.create({
  baseURL: 'http://localhost:8000/api',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  }
})

// Interceptor - dodaje token do każdego żądania
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token')
  if (token) {
    config.headers.Authorization = `Bearer ${token}`
  }
  return config
})

// Interceptor - obsługa błędów
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      localStorage.removeItem('token')
      window.location.href = '/login'
    }
    return Promise.reject(error)
  }
)

export default api
```

### Krok 4: Konfiguracja Vuetify

Edytuj `src/main.js`:

```javascript
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'
import router from './router'

// Vuetify
import 'vuetify/styles'
import { createVuetify } from 'vuetify'
import * as components from 'vuetify/components'
import * as directives from 'vuetify/directives'

const vuetify = createVuetify({
  components,
  directives,
  theme: {
    defaultTheme: 'light'
  }
})

const app = createApp(App)

app.use(createPinia())
app.use(router)
app.use(vuetify)

app.mount('#app')
```

### Krok 5: Uruchomienie Frontendu

```bash
# W katalogu frontend/
npm run dev

# Frontend będzie dostępny pod:
# http://localhost:5173
```

---

## 📁 Struktura Katalogów po Instalacji

```
fitboard-project/
├── backend/                          # Laravel 11 API
│   ├── app/
│   ├── bootstrap/
│   ├── config/
│   ├── database/
│   │   ├── database.sqlite          # Baza danych
│   │   └── seeders/
│   ├── routes/
│   │   └── api.php                  # Definicje tras API
│   ├── .env                         # Konfiguracja środowiska
│   └── ...
│
├── frontend/                         # Vue 3 SPA
│   ├── src/
│   │   ├── components/
│   │   ├── views/
│   │   ├── services/
│   │   │   └── api.js              # Konfiguracja Axios
│   │   ├── stores/
│   │   │   └── auth.js             # Pinia store
│   │   ├── router/
│   │   │   └── index.js            # Vue Router
│   │   ├── App.vue
│   │   └── main.js
│   ├── package.json
│   └── ...
│
└── docs/                             # Dokumentacja
    ├── architecture.md
    ├── api.md
    └── setup.md
```

---

## 🧪 Testowanie API

### Używając cURL

```bash
# Test działania API
curl http://localhost:8000/api/health

# Rejestracja
curl -X POST http://localhost:8000/api/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test","email":"test@example.com","password":"password","password_confirmation":"password"}'
```

### Używając Postman/Insomnia

1. Stwórz nową kolekcję "FitBoard API"
2. Dodaj zmienną środowiskową `base_url` = `http://localhost:8000/api`
3. Dodaj zmienną `token` (pusta na start)
4. W endpointach autoryzowanych dodaj header: `Authorization: Bearer {{token}}`

---

## ⚠️ Rozwiązywanie Problemów

### Problem: CORS errors w przeglądarce

**Rozwiązanie:**
Sprawdź `config/cors.php` - `supports_credentials` musi być `true`.

### Problem: "Database file not found"

**Rozwiązanie:**
```bash
cd backend
touch database/database.sqlite
php artisan migrate
```

### Problem: "Personal access client not found"

**Rozwiązanie:**
```bash
php artisan passport:install
# lub dla Sanctum:
php artisan migrate
```

### Problem: Port 8000 jest zajęty

**Rozwiązanie:**
```bash
# Użyj innego portu
php artisan serve --port=8080
```

---

## 🔒 Przygotowanie do Produkcji (opcjonalnie)

### Zmiana bazy na MySQL

Edytuj `.env`:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=fitboard
DB_USERNAME=root
DB_PASSWORD=secret
```

### Optymalizacja Laravel

```bash
# Cache konfiguracji
php artisan config:cache

# Cache tras
php artisan route:cache

# Cache widoków
php artisan view:cache

# Optymalizacja autoloadera
composer install --optimize-autoloader --no-dev
```

---

## ✅ Checklist Po Instalacji

- [ ] Backend działa pod `http://localhost:8000`
- [ ] Frontend działa pod `http://localhost:5173`
- [ ] Migracje zostały uruchomione bez błędów
- [ ] Seeder ćwiczeń dodał dane do bazy
- [ ] Rejestracja przez API zwraca token
- [ ] Logowanie przez API działa
- [ ] CORS pozwala na komunikację frontend <-> backend

---

## 📞 Wsparcie

Jeśli napotkasz problemy:

1. Sprawdź logi: `backend/storage/logs/laravel.log`
2. Upewnij się, że wszystkie wymagania są zainstalowane
3. Sprawdź czy porty 8000 i 5173 są wolne
4. Skonsultuj się z dokumentacją Laravel: https://laravel.com/docs/11.x

---

**Gotowe!** Teraz możesz przejść do implementacji funkcjonalności. Zobacz [`ARCHITECTURE.md`](./fitboard-architecture.md) dla szczegółów technicznych.
