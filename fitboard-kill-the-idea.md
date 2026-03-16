# 💀 WF_Kill_The_Idea – FitBoard

**Cel:** Znaleźć wszystkie pułapki i ryzyka projektu FitBoard ZANIM zaczniesz kodować. Jeśli pomysł przetrwa tę analizę – jest warty Twojego czasu.

**Kontekst:** Projekt studencki z Projektowania Aplikacji Internetowych | Vue + Laravel/Django | Poziom 4/10 | Frontend + mockupy

---

## 🔴 ATAK 1: "To już istnieje i jest za darmo"

### Zarzut:
FitBoard to kolejny tracker treningowy. Istnieją dziesiątki darmowych aplikacji – Strong, JEFIT, Hevy, FitNotes, nawet Apple Health i Google Fit. Dlaczego ktokolwiek miałby używać Twojej wersji?

### Werdykt: **NIE DOTYCZY – to projekt studencki, nie komercyjny**
Ale jest tu ukryta pułapka: **jeśli prowadzący zobaczy kolejny generyczny tracker, ocena może być przeciętna**. Musisz mieć wyróżnik.

### ✅ Mitygacja:
- **Dodaj unikalny element wizualny:** np. heatmapa aktywności w stylu GitHuba, animowane wykresy progresji, albo gamifikacja z odznaczeniami
- **Skoncentruj się na jednej konkretnej niszy:** zamiast "ogólny tracker", zrób "tracker dla biegaczy z analizą tempa" lub "tracker siłowy z wykresem 1RM"
- **Opcjonalnie AI:** Nawet prosta integracja z AI (np. automatyczne podsumowanie tygodnia treningowego) wyróżni Cię na tle klasy

---

## 🔴 ATAK 2: "Scope Creep – chcesz za dużo, a masz za mało czasu"

### Zarzut:
Dashboard, wykresy, formularze, historia, profil, heatmapa, streak-counter, 4 pełne ekrany... Przy poziomie 4/10 i ograniczonym czasie semestru, istnieje duże ryzyko, że projekt będzie w 80% niedokończony.

### Werdykt: **REALNE RYZYKO – najczęstszy zabójca projektów studenckich** ⚠️

### ✅ Mitygacja:
- **Zdefiniuj "Must Have" vs "Nice to Have":**
  - **Must Have:** Dashboard z 1 wykresem, formularz dodawania treningu, lista historii
  - **Nice to Have:** Heatmapa, streak, odznaczenia, profil użytkownika, AI
- **Zasada 3 ekranów:** Skup się na 3 kluczowych widokach, a nie 6. Lepiej mieć 3 dopracowane niż 6 niedokończonych
- **Użyj gotowych komponentów:** Vuetify lub PrimeVue na UI, Chart.js na wykresy – nie buduj od zera

---

## 🔴 ATAK 3: "Wykresy wyglądają łatwo, ale nie są"

### Zarzut:
Chart.js to potężna biblioteka, ale konfiguracja responsywnych, ładnych wykresów z dynamicznymi danymi jest trudniejsza, niż się wydaje. Przy 4/10 umiejętności możesz spędzić kilka dni na walce z konfiguracją jednego wykresu.

### Werdykt: **UMIARKOWANE RYZYKO** ⚠️

### ✅ Mitygacja:
- **Zacznij od statycznych danych:** Najpierw zrób wykres z hardcodowanymi danymi, dopiero potem podłączaj dynamiczne
- **Użyj ApexCharts zamiast Chart.js:** ApexCharts ma prostsze API i lepszą dokumentację dla Vue, komponent `vue3-apexcharts` jest plug-and-play
- **Ogranicz się do 2 typów wykresów:** np. liniowy (progres w czasie) + słupkowy (porównanie tygodni). Nie próbuj robić 5 różnych rodzajów

---

## 🔴 ATAK 4: "Brak backendu = brak trwałości danych"

### Zarzut:
Jeśli projekt jest frontend-only, dane treningowe znikną po odświeżeniu strony. To podważa sens "trackera" – co tracking, jeśli nic się nie zapisuje?

### Werdykt: **REALNE RYZYKO – wymaga rozwiązania** ⚠️

### ✅ Mitygacja:
- **Opcja A: LocalStorage/IndexedDB** – najprostsze rozwiązanie, dane żyją w przeglądarce. Wystarczające na demo
- **Opcja B: JSON Server** – fake REST API, uruchamiasz jedną komendą, dane w pliku JSON. Idealny do prototypowania z Vue
- **Opcja C: Firebase/Supabase** – jeśli chcesz mieć prawdziwy backend bez pisania go od zera. Supabase ma darmowy tier i gotowe SDK
- **Opcja D: Prosty Laravel/Django backend** – jeśli prowadzący wymaga backendu, minimum 2-3 endpointy CRUD

---

## 🔴 ATAK 5: "Gdzie jest wartość edukacyjna?"

### Zarzut:
Prowadzący ocenia nie tylko efekt końcowy, ale też proces i zastosowane techniki. Prosty CRUD z wykresem może być za mało, żeby dostać dobrą ocenę.

### Werdykt: **ZALEŻY OD WYMAGAŃ PRZEDMIOTU** ⚠️

### ✅ Mitygacja:
- **Dodaj warstwę techniczną, która pokaże naukę:**
  - Vue Router – routing między widokami
  - Pinia/Vuex – zarządzanie stanem
  - Composables – reużywalne logiki Vue 3
  - Responsywny design – media queries lub CSS Grid/Flexbox
- **Dokumentacja:** Przygotuj README z diagramem architektury, opisem technologii i uzasadnieniem wyborów
- **Testy:** Nawet 2-3 unit testy komponentów Vue pokażą dojrzałość

---

## 🟢 CO PRZETRWAŁO ATAK – Mocne Strony FitBoard

| Siła | Dlaczego to działa |
|---|---|
| **Prosty model danych** | Trening -> Ćwiczenia -> Serie. Łatwy do zmapowania na CRUD |
| **Wizualny efekt wow** | Wykresy i dashboard wyglądają profesjonalnie z minimum kodu |
| **Osobiste zaangażowanie** | Możesz testować na własnych treningach – motywacja do dokończenia |
| **Elastyczny scope** | Łatwo dodać lub usunąć funkcje bez przebudowy całości |
| **Dobra dokumentacja bibliotek** | Chart.js, ApexCharts, Vuetify – wszystko dobrze udokumentowane |

---

## 📊 WERDYKT KOŃCOWY

| Kategoria | Ocena | Komentarz |
|---|---|---|
| **Wykonalność dla 4/10** | 🟢 8/10 | Bardzo realistyczny, jeśli ograniczysz scope |
| **Ryzyko scope-creep** | 🟡 6/10 | Musisz bezwzględnie ciąć funkcje do minimum |
| **Efekt wizualny** | 🟢 9/10 | Dashboard z wykresami robi wrażenie na obronie |
| **Wartość edukacyjna** | 🟢 7/10 | Pokrywa routing, stan, komponenty, wizualizację |
| **Oryginalność** | 🟡 5/10 | Tracker to znany koncept – potrzebujesz wyróżnika |

### **DECYZJA: ✅ FitBoard PRZETRWAŁ – z zastrzeżeniami**

Projekt jest warty realizacji, ale pod warunkiem, że:

1. **Drastycznie ograniczysz scope** do 3 ekranów max
2. **Dodasz 1 wyróżnik** – heatmapa GitHubowa LUB integracja AI LUB gamifikacja
3. **Rozwiążesz problem trwałości danych** – LocalStorage minimum, Supabase idealnie
4. **Nie będziesz budować backendu od zera**, jeśli nie jest wymagany

---

## ❓ Kluczowe pytanie przed startem

Zanim przejdziesz do budowy, musisz odpowiedzieć na jedno pytanie: **Jakie są dokładne wymagania prowadzącego?** Czy wymagany jest backend? Dokumentacja? Testy? Prezentacja? To determinuje, ile czasu zostanie na sam kod.
