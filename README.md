# 🏋️ FitBoard

Personalny dashboard do śledzenia treningów siłowych z wizualizacją postępów.

**Stack:** Vue 3 + Laravel 11 + SQLite  
**Status:** Projekt studencki

---

## ✨ Funkcje

- **Dashboard** – statystyki, heatmapa aktywności, wykres progresu
- **Zarządzanie treningami** – dodawanie, edycja, usuwanie treningów
- **Ćwiczenia** – baza ćwiczeń z podziałem na grupy mięśniowe
- **Śledzenie postępów** – wizualizacja wyników w czasie
- **Heatmapa aktywności** – kalendarz treningowy

---

## 🚀 Uruchomienie

```bash
# Backend (Laravel)
cd backend
composer install
cp .env.example .env
php artisan migrate
php artisan serve

# Frontend (Vue)
cd frontend
npm install
npm run dev
```

---

## 📁 Struktura

```
docs/              # Dokumentacja projektu
├── fitboard-api.md           # Dokumentacja API
├── fitboard-architecture.md  # Architektura
├── fitboard-setup.md         # Instrukcja instalacji
└── fitboard-testing.md       # Testy
```

---

## 🛠️ Technologie

- **Frontend:** Vue 3 (Composition API), Pinia, Chart.js
- **Backend:** Laravel 11, SQLite
- **Styling:** Custom CSS

---

## 📄 Licencja

MIT
