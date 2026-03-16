# 📡 FitBoard - Dokumentacja API

**Wersja:** 1.0  
**Base URL:** `http://localhost:8000/api`  
**Format:** JSON

---

## 🔐 Autentykacja

Wszystkie endpointy poza `/register` i `/login` wymagają tokena Bearer w nagłówku:

```
Authorization: Bearer {token}
```

Token jest zwracany po zalogowaniu lub rejestracji.

---

## 1. Autentykacja

### 1.1 Rejestracja

**POST** `/register`

Tworzy nowe konto użytkownika i automatycznie loguje.

#### Request Body
```json
{
  "name": "Jan Kowalski",
  "email": "jan@example.com",
  "password": "tajnehaslo123",
  "password_confirmation": "tajnehaslo123"
}
```

#### Validation Rules
| Pole | Reguły |
|------|--------|
| name | required, string, max:255 |
| email | required, email, unique:users |
| password | required, min:8, confirmed |

#### Success Response (201 Created)
```json
{
  "user": {
    "id": 1,
    "name": "Jan Kowalski",
    "email": "jan@example.com",
    "created_at": "2026-03-16T12:00:00.000000Z"
  },
  "token": "1|laravel_sanctum_token_here..."
}
```

#### Error Response (422 Unprocessable Entity)
```json
{
  "message": "The email field must be a valid email address.",
  "errors": {
    "email": ["The email field must be a valid email address."]
  }
}
```

---

### 1.2 Logowanie

**POST** `/login`

Loguje użytkownika i zwraca token API.

#### Request Body
```json
{
  "email": "jan@example.com",
  "password": "tajnehaslo123"
}
```

#### Validation Rules
| Pole | Reguły |
|------|--------|
| email | required, email |
| password | required |

#### Success Response (200 OK)
```json
{
  "user": {
    "id": 1,
    "name": "Jan Kowalski",
    "email": "jan@example.com",
    "created_at": "2026-03-16T12:00:00.000000Z"
  },
  "token": "1|laravel_sanctum_token_here..."
}
```

#### Error Response (401 Unauthorized)
```json
{
  "message": "Invalid credentials"
}
```

---

### 1.3 Wylogowanie

**POST** `/logout`

Unieważnia aktualny token.

#### Headers
```
Authorization: Bearer {token}
```

#### Success Response (200 OK)
```json
{
  "message": "Logged out successfully"
}
```

---

### 1.4 Dane Użytkownika

**GET** `/user`

Zwraca dane zalogowanego użytkownika.

#### Headers
```
Authorization: Bearer {token}
```

#### Success Response (200 OK)
```json
{
  "id": 1,
  "name": "Jan Kowalski",
  "email": "jan@example.com",
  "created_at": "2026-03-16T12:00:00.000000Z"
}
```

---

## 2. Treningi (Workouts)

### 2.1 Lista Treningów

**GET** `/workouts`

Zwraca paginowaną listę treningów zalogowanego użytkownika.

#### Headers
```
Authorization: Bearer {token}
```

#### Query Parameters
| Parametr | Typ | Domyślnie | Opis |
|----------|-----|-----------|------|
| page | int | 1 | Numer strony |
| per_page | int | 10 | Ilość na stronę (max 50) |
| month | int | - | Filtrowanie po miesiącu (1-12) |
| year | int | - | Filtrowanie po roku |

#### Success Response (200 OK)
```json
{
  "data": [
    {
      "id": 1,
      "date": "2026-03-16",
      "notes": "Dobry trening, czuć progres",
      "duration_minutes": 60,
      "exercises_count": 4,
      "created_at": "2026-03-16T14:30:00.000000Z"
    },
    {
      "id": 2,
      "date": "2026-03-14",
      "notes": null,
      "duration_minutes": 45,
      "exercises_count": 3,
      "created_at": "2026-03-14T10:00:00.000000Z"
    }
  ],
  "links": {
    "first": "http://localhost:8000/api/workouts?page=1",
    "last": "http://localhost:8000/api/workouts?page=3",
    "prev": null,
    "next": "http://localhost:8000/api/workouts?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 3,
    "per_page": 10,
    "to": 10,
    "total": 25
  }
}
```

---

### 2.2 Szczegóły Treningu

**GET** `/workouts/{id}`

Zwraca szczegóły treningu wraz z listą ćwiczeń.

#### Headers
```
Authorization: Bearer {token}
```

#### Success Response (200 OK)
```json
{
  "id": 1,
  "date": "2026-03-16",
  "notes": "Dobry trening, czuć progres",
  "duration_minutes": 60,
  "created_at": "2026-03-16T14:30:00.000000Z",
  "exercises": [
    {
      "id": 1,
      "exercise": {
        "id": 5,
        "name": "Wyciskanie na ławce",
        "category": "Siłowe",
        "muscle_group": "Klatka piersiowa"
      },
      "sets": 4,
      "reps": 8,
      "weight_kg": 80.0
    },
    {
      "id": 2,
      "exercise": {
        "id": 12,
        "name": "Przysiady ze sztangą",
        "category": "Siłowe",
        "muscle_group": "Nogi"
      },
      "sets": 3,
      "reps": 10,
      "weight_kg": 100.0
    }
  ]
}
```

#### Error Response (404 Not Found)
```json
{
  "message": "Workout not found"
}
```

#### Error Response (403 Forbidden)
```json
{
  "message": "Unauthorized"
}
```

---

### 2.3 Tworzenie Treningu

**POST** `/workouts`

Tworzy nowy trening z ćwiczeniami.

#### Headers
```
Authorization: Bearer {token}
Content-Type: application/json
```

#### Request Body
```json
{
  "date": "2026-03-16",
  "notes": "Dobry trening",
  "duration_minutes": 60,
  "exercises": [
    {
      "exercise_id": 5,
      "sets": 4,
      "reps": 8,
      "weight_kg": 80
    },
    {
      "exercise_id": 12,
      "sets": 3,
      "reps": 10,
      "weight_kg": 100
    }
  ]
}
```

#### Validation Rules
| Pole | Reguły |
|------|--------|
| date | required, date |
| notes | nullable, string, max:500 |
| duration_minutes | nullable, integer, min:1, max:300 |
| exercises | required, array, min:1 |
| exercises.*.exercise_id | required, exists:exercises,id |
| exercises.*.sets | required, integer, min:1, max:20 |
| exercises.*.reps | required, integer, min:1, max:100 |
| exercises.*.weight_kg | required, numeric, min:0, max:500 |

#### Success Response (201 Created)
```json
{
  "id": 3,
  "date": "2026-03-16",
  "notes": "Dobry trening",
  "duration_minutes": 60,
  "created_at": "2026-03-16T15:00:00.000000Z",
  "exercises": [
    {
      "id": 5,
      "exercise": {
        "id": 5,
        "name": "Wyciskanie na ławce",
        "category": "Siłowe",
        "muscle_group": "Klatka piersiowa"
      },
      "sets": 4,
      "reps": 8,
      "weight_kg": 80
    }
  ]
}
```

#### Error Response (422 Unprocessable Entity)
```json
{
  "message": "The exercises.0.exercise_id field must exist in exercises table.",
  "errors": {
    "exercises.0.exercise_id": ["The exercises.0.exercise_id field must exist in exercises table."]
  }
}
```

---

### 2.4 Aktualizacja Treningu

**PUT** `/workouts/{id}`

Aktualizuje istniejący trening.

#### Headers
```
Authorization: Bearer {token}
Content-Type: application/json
```

#### Request Body
```json
{
  "date": "2026-03-17",
  "notes": "Zaktualizowane notatki",
  "duration_minutes": 65,
  "exercises": [
    {
      "exercise_id": 5,
      "sets": 4,
      "reps": 8,
      "weight_kg": 82.5
    }
  ]
}
```

#### Validation Rules
Takie same jak przy tworzeniu.

#### Success Response (200 OK)
```json
{
  "id": 1,
  "date": "2026-03-17",
  "notes": "Zaktualizowane notatki",
  "duration_minutes": 65,
  "created_at": "2026-03-16T14:30:00.000000Z",
  "exercises": [...]
}
```

---

### 2.5 Usuwanie Treningu

**DELETE** `/workouts/{id}`

Usuwa trening i wszystkie powiązane exercise_logs.

#### Headers
```
Authorization: Bearer {token}
```

#### Success Response (200 OK)
```json
{
  "message": "Workout deleted successfully"
}
```

#### Error Response (403 Forbidden)
```json
{
  "message": "Unauthorized"
}
```

---

## 3. Ćwiczenia (Exercises)

### 3.1 Lista Ćwiczeń

**GET** `/exercises`

Zwraca listę dostępnych ćwiczeń (predefiniowane w bazie).

#### Headers
```
Authorization: Bearer {token}
```

#### Query Parameters
| Parametr | Typ | Opis |
|----------|-----|------|
| category | string | Filtrowanie po kategorii |
| muscle_group | string | Filtrowanie po grupie mięśniowej |
| search | string | Wyszukiwanie po nazwie |

#### Success Response (200 OK)
```json
{
  "data": [
    {
      "id": 1,
      "name": "Martwy ciąg",
      "category": "Siłowe",
      "muscle_group": "Plecy"
    },
    {
      "id": 5,
      "name": "Wyciskanie na ławce",
      "category": "Siłowe",
      "muscle_group": "Klatka piersiowa"
    },
    {
      "id": 12,
      "name": "Przysiady ze sztangą",
      "category": "Siłowe",
      "muscle_group": "Nogi"
    }
  ]
}
```

---

## 4. Statystyki (Stats)

### 4.1 Dashboard Stats

**GET** `/stats/dashboard`

Zwraca agregowane statystyki do wyświetlenia na dashboardzie.

#### Headers
```
Authorization: Bearer {token}
```

#### Success Response (200 OK)
```json
{
  "total_workouts": 25,
  "workouts_this_week": 3,
  "workouts_this_month": 12,
  "avg_duration_minutes": 55,
  "total_volume_kg": 15400,
  "most_frequent_exercise": {
    "id": 5,
    "name": "Wyciskanie na ławce",
    "count": 15
  }
}
```

---

### 4.2 Progres Ćwiczenia

**GET** `/stats/progress/{exercise_id}`

Zwraca historię ciężaru dla konkretnego ćwiczenia (do wykresu).

#### Headers
```
Authorization: Bearer {token}
```

#### Query Parameters
| Parametr | Typ | Domyślnie | Opis |
|----------|-----|-----------|------|
| months | int | 6 | Ile miesięcy wstecz |

#### Success Response (200 OK)
```json
{
  "exercise": {
    "id": 5,
    "name": "Wyciskanie na ławce",
    "category": "Siłowe",
    "muscle_group": "Klatka piersiowa"
  },
  "progress": [
    {
      "date": "2026-01-15",
      "max_weight": 75.0,
      "avg_weight": 72.5,
      "total_volume": 1740
    },
    {
      "date": "2026-02-01",
      "max_weight": 77.5,
      "avg_weight": 75.0,
      "total_volume": 1800
    },
    {
      "date": "2026-03-16",
      "max_weight": 80.0,
      "avg_weight": 78.5,
      "total_volume": 1880
    }
  ]
}
```

#### Error Response (404 Not Found)
```json
{
  "message": "Exercise not found"
}
```

---

### 4.3 Aktywność (Heatmap)

**GET** `/stats/activity`

Zwraca listę dat z treningami do wygenerowania heatmapy aktywności.

#### Headers
```
Authorization: Bearer {token}
```

#### Query Parameters
| Parametr | Typ | Domyślnie | Opis |
|----------|-----|-----------|------|
| months | int | 6 | Ile miesięcy wstecz |

#### Success Response (200 OK)
```json
{
  "activity": [
    {
      "date": "2026-03-16",
      "count": 1,
      "intensity": 1
    },
    {
      "date": "2026-03-14",
      "count": 1,
      "intensity": 1
    },
    {
      "date": "2026-03-12",
      "count": 2,
      "intensity": 2
    },
    {
      "date": "2026-03-10",
      "count": 1,
      "intensity": 1
    }
  ],
  "total_days": 4,
  "max_intensity": 2
}
```

---

## 5. Kody Błędów

| Kod | Znaczenie | Opis |
|-----|-----------|------|
| 200 | OK | Sukces |
| 201 | Created | Utworzono zasób |
| 204 | No Content | Usunięto zasób |
| 400 | Bad Request | Błąd w żądaniu |
| 401 | Unauthorized | Brak tokena lub nieprawidłowy token |
| 403 | Forbidden | Brak dostępu do zasobu |
| 404 | Not Found | Zasób nie istnieje |
| 422 | Unprocessable Entity | Błąd walidacji |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Błąd serwera |

---

## 6. Przykłady użycia (cURL)

### Rejestracja
```bash
curl -X POST http://localhost:8000/api/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jan Kowalski",
    "email": "jan@example.com",
    "password": "tajnehaslo123",
    "password_confirmation": "tajnehaslo123"
  }'
```

### Logowanie
```bash
curl -X POST http://localhost:8000/api/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "jan@example.com",
    "password": "tajnehaslo123"
  }'
```

### Tworzenie treningu
```bash
curl -X POST http://localhost:8000/api/workouts \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer {token}" \
  -d '{
    "date": "2026-03-16",
    "notes": "Dobry trening",
    "duration_minutes": 60,
    "exercises": [
      {
        "exercise_id": 5,
        "sets": 4,
        "reps": 8,
        "weight_kg": 80
      }
    ]
  }'
```

---

## 7. Predefiniowane Ćwiczenia

Ćwiczenia dostępne po uruchomieniu seedera:

| ID | Nazwa | Kategoria | Grupa mięśniowa |
|----|-------|-----------|-----------------|
| 1 | Martwy ciąg | Siłowe | Plecy |
| 2 | Podciąganie na drążku | Siłowe | Plecy |
| 3 | Wiosłowanie sztangą | Siłowe | Plecy |
| 4 | Ściąganie drążka do klatki | Siłowe | Plecy |
| 5 | Wyciskanie na ławce | Siłowe | Klatka piersiowa |
| 6 | Wyciskanie hantli | Siłowe | Klatka piersiowa |
| 7 | Rozpiętki | Siłowe | Klatka piersiowa |
| 8 | Dip na poręczach | Siłowe | Klatka piersiowa |
| 9 | Wyciskanie żołnierskie | Siłowe | Barki |
| 10 | Unoszenie hantli bokiem | Siłowe | Barki |
| 11 | Arnoldki | Siłowe | Barki |
| 12 | Przysiady ze sztangą | Siłowe | Nogi |
| 13 | Wykroki | Siłowe | Nogi |
| 14 | Leg press | Siłowe | Nogi |
| 15 | Prostowanie nóg | Siłowe | Nogi |
| 16 | Uginanie nóg | Siłowe | Nogi |
| 17 | Wspięcia na palce | Siłowe | Łydki |
| 18 | Uginanie ramion ze sztangą | Siłowe | Biceps |
| 19 | Uginanie ramion z hantlami | Siłowe | Biceps |
| 20 | Wyciskanie francuskie | Siłowe | Triceps |
| 21 | Pompki na triceps | Siłowe | Triceps |
| 22 | Plank | Siłowe | Core |
| 23 | Crunches | Siłowe | Core |
| 24 | Russian twist | Siłowe | Core |
| 25 | Bieg na bieżni | Kardio | Nogi |
| 26 | Jazda na rowerku | Kardio | Nogi |
| 27 | Skakanka | Kardio | Całe ciało |

---

**Następny krok:** Zobacz [`SETUP.md`](./fitboard-setup.md) dla instrukcji instalacji.
