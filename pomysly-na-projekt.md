# 🎓 Pomysły na Projekt Studencki – Projektowanie Aplikacji Internetowych

**Profil studenta:** Vue + Laravel/Django | Umiejętności: 4/10 | Zainteresowania: zdrowie, sport | Zakres: frontend + mockupy (opcjonalnie AI)

---

## Pomysł 1: FitBoard – Interaktywny Dashboard Treningowy

| Kategoria | Opis |
|---|---|
| **One-liner** | Personalny dashboard do logowania treningów, śledzenia postępów i wizualizacji statystyk sportowych |
| **The "Why Now"** | Boom na fitness tracking + rosnące zmęczenie skomplikowanymi aplikacjami typu MyFitnessPal. Studenci i amatorzy chcą czegoś prostego, wizualnego i szybkiego |
| **Target Audience** | Studenci, amatorzy siłowni, biegacze – osoby trenujące 2-4x w tygodniu, które chcą widzieć swój progres bez konieczności nauki skomplikowanej apki |
| **Dlaczego to dobry projekt studencki** | Pokazuje umiejętność budowy komponentów Vue, pracy z danymi, wizualizacji danych za pomocą wykresów, responsywnego designu |
| **Opcjonalne AI** | GPT generuje krótkie porady treningowe na podstawie zalogowanych danych, np. - Twoje wyniki na ławce rosną – czas zwiększyć obciążenie |

### Co pokażesz na obronie:
- **Vue components:** reużywalne karty, wykresy, formularze do logowania ćwiczeń
- **Wizualizacja danych:** Chart.js lub ApexCharts – wykresy postępów, heatmapa aktywności w stylu GitHuba
- **Stan aplikacji:** Vuex/Pinia do zarządzania danymi treningowymi
- **Responsywność:** mobile-first design, bo ludzie logują trening na telefonie

### Kluczowe ekrany:
1. Dashboard z podsumowaniem tygodnia, wykresami, streak-counter
2. Formularz dodawania treningu z wyborem ćwiczeń, serii, powtórzeń
3. Historia treningów z filtrami i sortowaniem
4. Profil użytkownika z celami i statystykami

### Ocena Solo-Dev:
- ✅ **Low Support** – brak potrzeby obsługi, użytkownik sam loguje dane
- ✅ **High Pain** – ludzie tracą motywację bez wizualnego feedbacku postępów
- ✅ **No Viral Dependency** – wartość już dla pierwszego użytkownika
- ✅ **Scope dla 4/10** – CRUD + wykresy, nie wymaga zaawansowanej logiki

---

## Pomysł 2: NutriPlan – Planer Żywieniowy dla Aktywnych Ludzi

| Kategoria | Opis |
|---|---|
| **One-liner** | Aplikacja do planowania posiłków i liczenia makroskładników, dopasowana do celów treningowych – masa, redukcja, utrzymanie |
| **The "Why Now"** | Trend na świadome odżywianie wśród młodych ludzi + brak prostych narzędzi w języku polskim dopasowanych do polskich produktów |
| **Target Audience** | Studenci i młodzi dorośli trenujący na siłowni, którzy chcą ogarnąć dietę bez płacenia za dietetyka |
| **Dlaczego to dobry projekt studencki** | Złożone formularze wieloetapowe, drag-and-drop, kalkulator makro, ciekawy UX – dużo elementów do pokazania na obronie |
| **Opcjonalne AI** | AI generuje propozycje posiłków na podstawie preferencji, alergii i celów kalorycznych użytkownika |

### Co pokażesz na obronie:
- **Formularz wieloetapowy:** wizard do onboardingu – cel, waga, wzrost, aktywność fizyczna, preferencje żywieniowe
- **Kalkulator makro:** automatyczne wyliczanie kalorii, białka, tłuszczu i węglowodanów na podstawie danych użytkownika
- **Planer tygodniowy:** drag-and-drop posiłków na dni tygodnia, widok kalendarza
- **Baza produktów:** wyszukiwarka z filtrami, dodawanie własnych produktów

### Kluczowe ekrany:
1. Onboarding wizard – 4-5 kroków zbierania danych o użytkowniku
2. Dashboard z dziennym podsumowaniem makro, paskami postępu, kalorycznym gauge
3. Planer tygodniowy z widokiem drag-and-drop
4. Wyszukiwarka produktów i przepisów z filtrami
5. Lista zakupów generowana automatycznie z planu posiłków

### Ocena Solo-Dev:
- ✅ **Low Support** – użytkownik sam konfiguruje swój plan
- ✅ **High Pain** – ludzie nie wiedzą, ile jeść, żeby osiągnąć cele sportowe
- ✅ **No Viral Dependency** – wartość natychmiastowa po wypełnieniu profilu
- ⚠️ **Scope dla 4/10** – drag-and-drop i kalkulator mogą wymagać trochę nauki, ale istnieją gotowe biblioteki Vue, np. vuedraggable

---

## Pomysł 3: SportConnect – Łącznik Amatorskich Sportowców

| Kategoria | Opis |
|---|---|
| **One-liner** | Platforma, na której amatorzy mogą tworzyć i dołączać do lokalnych wydarzeń sportowych – pickup basketball, bieganie grupowe, siatkówka plażowa |
| **The "Why Now"** | Post-pandemiczny trend na sport w grupach + brak dedykowanej platformy do tego typu wydarzeń – ludzie korzystają z grup na Facebooku, co jest chaotyczne i nieefektywne |
| **Target Audience** | Studenci i młodzi dorośli, którzy chcą uprawiać sport, ale nie mają stałej grupy – nowi w mieście, szukający partnerów treningowych |
| **Dlaczego to dobry projekt studencki** | Integracja z mapą, zaawansowane filtrowanie i wyszukiwanie, system RSVP, ciekawa architektura danych – rozbudowany projekt z wieloma modułami |
| **Opcjonalne AI** | AI-matching: automatyczne dopasowywanie wydarzeń do profilu użytkownika na podstawie lokalizacji, poziomu i preferencji sportowych |

### Co pokażesz na obronie:
- **Mapa interaktywna:** integracja z Leaflet.js lub Mapbox – pinezki z wydarzeniami, geolokalizacja
- **System filtrów:** sport, data, lokalizacja, poziom zaawansowania, dystans
- **CRUD wydarzeń:** tworzenie, edycja, usuwanie, dołączanie do wydarzeń
- **Profile użytkowników:** sport, poziom, historia aktywności

### Kluczowe ekrany:
1. Strona główna z mapą wydarzeń w okolicy i listą nadchodzących eventów
2. Formularz tworzenia wydarzenia – sport, lokalizacja na mapie, data, max uczestników, poziom
3. Widok szczegółów wydarzenia z listą uczestników, chatem, przyciskiem dołącz/opuść
4. Profil użytkownika z historią wydarzeń, statystykami, odznaczeniami
5. Wyszukiwarka z zaawansowanymi filtrami

### Ocena Solo-Dev:
- ✅ **Low Support** – community samo zarządza wydarzeniami
- ✅ **High Pain** – znalezienie ludzi do wspólnego sportu jest realnie frustrujące
- ⚠️ **Viral Dependency** – wartość rośnie z liczbą użytkowników, ALE jako projekt studencki z mockupami to nie problem
- ⚠️ **Scope dla 4/10** – integracja z mapą wymaga trochę nauki, ale Leaflet.js jest dość prosty. Projekt jest bardziej rozbudowany, co może być plusem na ocenę

---

## Porównanie Pomysłów

| Kryterium | FitBoard | NutriPlan | SportConnect |
|---|---|---|---|
| **Trudność implementacji** | ⭐⭐ Niska | ⭐⭐⭐ Średnia | ⭐⭐⭐ Średnia |
| **Wow-factor na obronie** | ⭐⭐⭐ Wykresy i dashboard | ⭐⭐⭐⭐ Drag-and-drop, wizard | ⭐⭐⭐⭐ Mapa, real-time |
| **Potencjał AI** | ⭐⭐ Porady treningowe | ⭐⭐⭐⭐ Generator posiłków | ⭐⭐⭐ Matching wydarzeń |
| **Dopasowanie do zainteresowań** | ✅ Sport/Fitness | ✅ Zdrowie/Sport | ✅ Sport/Społeczność |
| **Ryzyko scope-creep** | Niskie | Średnie | Wysokie |

---

## Co dalej?

**Wybierz jeden pomysł, który najbardziej Cię intryguje.** Mogę wtedy:
1. Uruchomić `WF_Kill_The_Idea` – sprawdzimy, gdzie kryją się pułapki
2. Przejść do `WF_MVP_Scoping` – zdefiniujemy dokładny zakres MVP na projekt studencki
3. Zaprojektować `WF_User_Journey_Map` – rozrysujemy ścieżkę użytkownika

💡 **Moja rekomendacja:** Dla Twojego poziomu (4/10) i zakresu projektu (frontend + mockupy), **FitBoard** jest najbezpieczniejszym wyborem – niski próg wejścia, a wykresy i dashboard wyglądają efektownie na prezentacji. **NutriPlan** jest ambitniejszy, ale bardziej imponujący. **SportConnect** jest najbardziej rozbudowany – świetny, jeśli chcesz się wykazać, ale ryzykowny czasowo.
