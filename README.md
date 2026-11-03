# Projekt zaliczeniowy na przedmiot Języki obiektowe

## 1. Opis architektury i funkcjonalności
Program realizuje dwuwymiarową grę platformową z interfejsem graficznym (WinAPI GDI) oraz diagnostycznym interfejsem konsolowym. Projekt został podzielony na moduły nagłówkowe (`.h`) oraz implementacyjne (`.cpp`):

* **`grafika.h` / `grafika.cpp`**:
  * `Plat` – klasa bazowa reprezentująca obiekt platformy na mapie (tekstury i ich położenie `.bmp`).
  * Platforma podstawowa `Plat_pods` – klasa pochodna reprezentująca statyczną platformę o standardowym odbiciu (`x1.0`).
  * Platforma specjalna `Plat_spec_` – klasa pochodna reprezentująca ruchomą trampolinę poruszającą się w poziomie i zwiększającą siłę skoku gracza (`x1.35`).
  * Swiat gry `Sw_gry` – klasa zarządzająca warstwami renderowania (stałe tło, platformy, gracz, UI) oraz operacjami CRUD (dodawanie, usuwanie, wyszukiwanie, modyfikacja platform).
* **`ruch.h` / `ruch.cpp`**:
  * `Gracz` – klasa odpowiadająca za fizykę postaci (grawitacja, skok, ruch poziomy), detekcję kolizji AABB z platformami, odradzanie na pierwszej platformie oraz system żyć.
* **`main.cpp`**:
  * Inicjalizacja okna, główna pętla czasu rzeczywistego oraz obsługa wyjątków (`try-catch`).

## 2. Zastosowane mechanizmy obiektowe
* **Enkapsulacja:** Wszystkie pola klas są `private` lub `protected`, a dostęp do nich odbywa się przez metody `get`/`set` z walidacją danych.
* **Dziedziczenie i polimorfizm:** Klasy `PlatformaZwykla` oraz `PlatformaSprezysta` dziedziczą po klasie `Platforma` i przesłaniają metody wirtualne (`rysuj`, `aktualizuj`, `pobierzMnoznikSkoku`, `pobierzTyp`) z użyciem słowa kluczowego `override`.
* **Przeciążanie operatorów:**
  * `operator==` – porównywanie platform (zapobieganie dodaniu duplikatu).
  * `operator<` – porównywanie wysokości platform (wyszukiwanie najwyższej platformy przez `std::min_element`).
  * `operator-=` oraz `operator+=` – modyfikacja liczby żyć gracza po upadku poza mapę.
  * `operator<<` – wypisywanie pełnego stanu obiektów `Platforma` oraz `Gracz` na strumień `std::cout`.
* **Obsługa wyjątków:** Walidacja argumentów konstruktorów i setterów (`std::invalid_argument`, `std::out_of_range`, `std::runtime_error`).

## 3. Instrukcja kompilacji i uruchomienia
Projekt korzysta ze standardowego kompilatora C++ (MinGW `g++` ver.10.3.0) oraz wbudowanego modułu `gdi32`.

**Kompilacja (terminal):**
```bash
g++ main.cpp grafika.cpp ruch.cpp -o gra.exe -lgdi32

## Autor kodu oraz grafik Marcin Szawerda