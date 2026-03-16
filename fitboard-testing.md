# 🧪 FitBoard - Plan Testów

Kompletna strategia testowania backendu FitBoard.

---

## 1. Filozofia Testowania

> **Zasada:** Testujemy zachowanie, nie implementację.

### Poziomy Testów

```
┌─────────────────────────────────────┐
│  E2E Tests (opcjonalnie)            │  ← Cały flow użytkownika
├─────────────────────────────────────┤
│  Feature Tests ⭐ FOCUS              │  ← Endpointy API
├─────────────────────────────────────┤
│  Unit Tests (model/mniejsze)        │  ← Logika biznesowa
└─────────────────────────────────────┘
```

**Dla projektu studenckiego skupiamy się na Feature Tests.**

---

## 2. Konfiguracja Testów

### 2.1 Baza Testowa

Laravel domyślnie używa osobnej bazy dla testów.

```php
// phpunit.xml
<env name="DB_CONNECTION" value="sqlite"/>
<env name="DB_DATABASE" value=":memory:"/>
```

### 2.2 Refresh Database

```php
<?php

namespace Tests\Feature;

use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

abstract class FeatureTestCase extends TestCase
{
    use RefreshDatabase;
    
    // Każdy test zaczyna się z czystą bazą
}
```

---

## 3. Scenariusze Testowe

### 3.1 Autentykacja (`AuthTest.php`)

#### Test: Rejestracja - sukces
```php
public function test_user_can_register()
{
    $response = $this->postJson('/api/register', [
        'name' => 'Jan Kowalski',
        'email' => 'jan@example.com',
        'password' => 'password123',
        'password_confirmation' => 'password123',
    ]);

    $response->assertStatus(201)
        ->assertJsonStructure([
            'user' => ['id', 'name', 'email', 'created_at'],
            'token'
        ]);
    
    $this->assertDatabaseHas('users', [
        'email' => 'jan@example.com'
    ]);
}
```

#### Test: Rejestracja - walidacja email
```php
public function test_registration_requires_valid_email()
{
    $response = $this->postJson('/api/register', [
        'name' => 'Jan',
        'email' => 'niepoprawny-email',
        'password' => 'password123',
        'password_confirmation' => 'password123',
    ]);

    $response->assertStatus(422)
        ->assertJsonValidationErrors(['email']);
}
```

#### Test: Rejestracja - unikalny email
```php
public function test_registration_requires_unique_email()
{
    User::factory()->create(['email' => 'jan@example.com']);

    $response = $this->postJson('/api/register', [
        'name' => 'Jan',
        'email' => 'jan@example.com',
        'password' => 'password123',
        'password_confirmation' => 'password123',
    ]);

    $response->assertStatus(422)
        ->assertJsonValidationErrors(['email']);
}
```

#### Test: Logowanie - sukces
```php
public function test_user_can_login()
{
    $user = User::factory()->create([
        'email' => 'jan@example.com',
        'password' => bcrypt('password123')
    ]);

    $response = $this->postJson('/api/login', [
        'email' => 'jan@example.com',
        'password' => 'password123',
    ]);

    $response->assertStatus(200)
        ->assertJsonStructure(['user', 'token']);
}
```

#### Test: Logowanie - niepoprawne hasło
```php
public function test_login_fails_with_wrong_password()
{
    $user = User::factory()->create([
        'email' => 'jan@example.com',
        'password' => bcrypt('password123')
    ]);

    $response = $this->postJson('/api/login', [
        'email' => 'jan@example.com',
        'password' => 'zlehaslo',
    ]);

    $response->assertStatus(401);
}
```

#### Test: Dostęp do /user - autoryzowany
```php
public function test_authenticated_user_can_get_profile()
{
    $user = User::factory()->create();
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson('/api/user');

    $response->assertStatus(200)
        ->assertJson(['email' => $user->email]);
}
```

#### Test: Dostęp do /user - nieautoryzowany
```php
public function test_unauthenticated_user_cannot_get_profile()
{
    $response = $this->getJson('/api/user');

    $response->assertStatus(401);
}
```

---

### 3.2 Treningi (`WorkoutTest.php`)

#### Test: Lista treningów - autoryzowany użytkownik
```php
public function test_user_can_get_own_workouts()
{
    $user = User::factory()->create();
    Workout::factory()->count(3)->create(['user_id' => $user->id]);
    
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson('/api/workouts');

    $response->assertStatus(200)
        ->assertJsonCount(3, 'data');
}
```

#### Test: Lista treningów - tylko własne
```php
public function test_user_sees_only_own_workouts()
{
    $user1 = User::factory()->create();
    $user2 = User::factory()->create();
    
    Workout::factory()->create(['user_id' => $user1->id]);
    Workout::factory()->create(['user_id' => $user2->id]);
    
    $token = $user1->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson('/api/workouts');

    $response->assertStatus(200)
        ->assertJsonCount(1, 'data');
}
```

#### Test: Tworzenie treningu - sukces
```php
public function test_user_can_create_workout()
{
    $user = User::factory()->create();
    $exercise = Exercise::factory()->create();
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->postJson('/api/workouts', [
            'date' => '2026-03-16',
            'notes' => 'Dobry trening',
            'duration_minutes' => 60,
            'exercises' => [
                [
                    'exercise_id' => $exercise->id,
                    'sets' => 4,
                    'reps' => 8,
                    'weight_kg' => 80
                ]
            ]
        ]);

    $response->assertStatus(201)
        ->assertJsonStructure([
            'id', 'date', 'notes', 'duration_minutes', 'exercises'
        ]);
    
    $this->assertDatabaseHas('workouts', [
        'user_id' => $user->id,
        'notes' => 'Dobry trening'
    ]);
}
```

#### Test: Tworzenie treningu - walidacja
```php
public function test_workout_requires_date()
{
    $user = User::factory()->create();
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->postJson('/api/workouts', [
            'notes' => 'Test',
            'exercises' => []
        ]);

    $response->assertStatus(422)
        ->assertJsonValidationErrors(['date', 'exercises']);
}
```

#### Test: Tworzenie treningu - nieistniejące ćwiczenie
```php
public function test_workout_requires_valid_exercise_id()
{
    $user = User::factory()->create();
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->postJson('/api/workouts', [
            'date' => '2026-03-16',
            'exercises' => [
                [
                    'exercise_id' => 9999, // nieistniejące
                    'sets' => 4,
                    'reps' => 8,
                    'weight_kg' => 80
                ]
            ]
        ]);

    $response->assertStatus(422)
        ->assertJsonValidationErrors(['exercises.0.exercise_id']);
}
```

#### Test: Pobieranie szczegółów - sukces
```php
public function test_user_can_get_workout_details()
{
    $user = User::factory()->create();
    $workout = Workout::factory()->create(['user_id' => $user->id]);
    ExerciseLog::factory()->create(['workout_id' => $workout->id]);
    
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson("/api/workouts/{$workout->id}");

    $response->assertStatus(200)
        ->assertJsonStructure([
            'id', 'date', 'notes', 'exercises' => [
                '*' => ['id', 'sets', 'reps', 'weight_kg', 'exercise']
            ]
        ]);
}
```

#### Test: Pobieranie szczegółów - cudzy trening
```php
public function test_user_cannot_get_other_user_workout()
{
    $user1 = User::factory()->create();
    $user2 = User::factory()->create();
    $workout = Workout::factory()->create(['user_id' => $user2->id]);
    
    $token = $user1->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson("/api/workouts/{$workout->id}");

    $response->assertStatus(403);
}
```

#### Test: Aktualizacja treningu
```php
public function test_user_can_update_workout()
{
    $user = User::factory()->create();
    $workout = Workout::factory()->create(['user_id' => $user->id]);
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->putJson("/api/workouts/{$workout->id}", [
            'date' => '2026-03-17',
            'notes' => 'Zaktualizowane',
            'duration_minutes' => 65,
            'exercises' => []
        ]);

    $response->assertStatus(200);
    
    $this->assertDatabaseHas('workouts', [
        'id' => $workout->id,
        'notes' => 'Zaktualizowane'
    ]);
}
```

#### Test: Usuwanie treningu
```php
public function test_user_can_delete_workout()
{
    $user = User::factory()->create();
    $workout = Workout::factory()->create(['user_id' => $user->id]);
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->deleteJson("/api/workouts/{$workout->id}");

    $response->assertStatus(200);
    
    $this->assertDatabaseMissing('workouts', [
        'id' => $workout->id
    ]);
}
```

---

### 3.3 Statystyki (`StatsTest.php`)

#### Test: Dashboard stats
```php
public function test_user_can_get_dashboard_stats()
{
    $user = User::factory()->create();
    Workout::factory()->count(5)->create(['user_id' => $user->id]);
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson('/api/stats/dashboard');

    $response->assertStatus(200)
        ->assertJsonStructure([
            'total_workouts',
            'workouts_this_week',
            'workouts_this_month',
            'avg_duration_minutes',
            'most_frequent_exercise'
        ]);
}
```

#### Test: Progress ćwiczenia
```php
public function test_user_can_get_exercise_progress()
{
    $user = User::factory()->create();
    $exercise = Exercise::factory()->create();
    
    $workout = Workout::factory()->create(['user_id' => $user->id]);
    ExerciseLog::factory()->create([
        'workout_id' => $workout->id,
        'exercise_id' => $exercise->id,
        'weight_kg' => 80
    ]);
    
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson("/api/stats/progress/{$exercise->id}");

    $response->assertStatus(200)
        ->assertJsonStructure([
            'exercise' => ['id', 'name', 'category', 'muscle_group'],
            'progress'
        ]);
}
```

#### Test: Activity heatmap
```php
public function test_user_can_get_activity_data()
{
    $user = User::factory()->create();
    Workout::factory()->create([
        'user_id' => $user->id,
        'date' => now()
    ]);
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson('/api/stats/activity');

    $response->assertStatus(200)
        ->assertJsonStructure([
            'activity',
            'total_days',
            'max_intensity'
        ]);
}
```

---

### 3.4 Ćwiczenia (`ExerciseTest.php`)

#### Test: Lista ćwiczeń
```php
public function test_user_can_get_exercises_list()
{
    $user = User::factory()->create();
    Exercise::factory()->count(10)->create();
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson('/api/exercises');

    $response->assertStatus(200)
        ->assertJsonCount(10, 'data');
}
```

#### Test: Filtrowanie po grupie mięśniowej
```php
public function test_exercises_can_be_filtered_by_muscle_group()
{
    $user = User::factory()->create();
    Exercise::factory()->create(['muscle_group' => 'Klatka piersiowa']);
    Exercise::factory()->create(['muscle_group' => 'Nogi']);
    $token = $user->createToken('test')->plainTextToken;

    $response = $this->withHeader('Authorization', "Bearer {$token}")
        ->getJson('/api/exercises?muscle_group=Klatka piersiowa');

    $response->assertStatus(200)
        ->assertJsonCount(1, 'data');
}
```

---

## 4. Uruchamianie Testów

### Wszystkie testy
```bash
cd backend
php artisan test
```

### Konkretny plik
```bash
php artisan test --filter=AuthTest
```

### Konkretny test
```bash
php artisan test --filter=test_user_can_register
```

### Z pokryciem kodu (opcjonalnie)
```bash
php artisan test --coverage
```

### W trybie verbose
```bash
php artisan test --filter=WorkoutTest -v
```

---

## 5. Test Factories

### UserFactory
```php
public function definition(): array
{
    return [
        'name' => fake()->name(),
        'email' => fake()->unique()->safeEmail(),
        'email_verified_at' => now(),
        'password' => bcrypt('password'),
    ];
}
```

### WorkoutFactory
```php
public function definition(): array
{
    return [
        'user_id' => User::factory(),
        'date' => fake()->date(),
        'notes' => fake()->optional()->sentence(),
        'duration_minutes' => fake()->numberBetween(30, 120),
    ];
}
```

### ExerciseLogFactory
```php
public function definition(): array
{
    return [
        'workout_id' => Workout::factory(),
        'exercise_id' => Exercise::factory(),
        'sets' => fake()->numberBetween(1, 5),
        'reps' => fake()->numberBetween(5, 15),
        'weight_kg' => fake()->randomFloat(1, 20, 200),
    ];
}
```

---

## 6. CI/CD (opcjonalnie)

### GitHub Actions - przykład
```yaml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup PHP
      uses: shivammathur/setup-php@v2
      with:
        php-version: '8.2'
    
    - name: Install Dependencies
      run: |
        cd backend
        composer install
    
    - name: Run Tests
      run: |
        cd backend
        php artisan test
```

---

## 7. Checklist Testów

### Auth
- [ ] Rejestracja - sukces
- [ ] Rejestracja - walidacja email
- [ ] Rejestracja - unikalny email
- [ ] Logowanie - sukces
- [ ] Logowanie - niepoprawne hasło
- [ ] Logowanie - nieistniejący user
- [ ] /user - autoryzowany
- [ ] /user - nieautoryzowany
- [ ] Logout - działa

### Workouts
- [ ] Lista - autoryzowany
- [ ] Lista - tylko własne
- [ ] Lista - paginacja
- [ ] Szczegóły - sukces
- [ ] Szczegóły - cudzy trening (403)
- [ ] Szczegóły - nieistniejący (404)
- [ ] Tworzenie - sukces
- [ ] Tworzenie - walidacja
- [ ] Tworzenie - nieistniejące ćwiczenie
- [ ] Aktualizacja - sukces
- [ ] Aktualizacja - cudzy trening (403)
- [ ] Usuwanie - sukces
- [ ] Usuwanie - cudzy trening (403)

### Exercises
- [ ] Lista - dostępna
- [ ] Lista - filtrowanie

### Stats
- [ ] Dashboard - poprawna struktura
- [ ] Progress - poprawna struktura
- [ ] Activity - poprawna struktura

---

**Łącznie:** ~30 testów feature

**Szacowany czas implementacji:** 2-3h

**Następny krok:** Implementacja kodu w tempie zajęć.
