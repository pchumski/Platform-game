# 🎮 Not Tetris — gra platformowa 2D (C++ / SFML)

Gra platformowa 2D stworzona w C++ z użyciem biblioteki **SFML**, jako projekt zaliczeniowy z przedmiotu **Programowanie Strukturalne i Obiektowe** (Politechnika Poznańska).

Celem gracza jest uratowanie przyjaciela i zdobycie jak najwyższego wyniku, omijając przeciwników i przeszkody oraz wykorzystując przedmioty znalezione na mapie do znalezienia właściwej drogi.

---

## 🕹️ Rozgrywka

- **Sterowanie:** `A` / `D` — ruch w lewo/prawo, `Spacja` — skok, `Lewy Shift` — bieg
- **Cel:** dotrzeć do końca poziomu, zbierając monety i unikając wrogów oraz niebezpiecznych obiektów
- **Menu główne:** obsługa myszką lub klawiaturą (`↑` / `↓` / `Enter`)

## ✨ Funkcjonalności

- Animowana postać gracza (bieg, skok, zmiana kierunku)
- Wrogowie z własną logiką ruchu i kolizji
- System kolizji AABB (własna implementacja w `Collider`)
- Mapy poziomów wczytywane z plików tekstowych (patrz [Format map](#-format-map))
- Monety, skrzynie, przedmioty specjalne i obiekty niebezpieczne jako osobne warstwy mapy
- Dźwięki i muzyka w tle (SFML Audio)
- Menu główne i ekran opcji

## 🛠️ Technologie

- **C++17**
- **[SFML 2.5](https://www.sfml-dev.org/)** — grafika, audio, obsługa okna i zdarzeń
- **CMake ≥ 3.16** — budowanie (Windows / Linux / macOS)

---

## 🗂️ Struktura repozytorium

```
.
├── CMakeLists.txt          # konfiguracja budowania (zastępuje stare .sln)
├── restructure.sh          # skrypt, który wygenerował tę strukturę (do wglądu/historii)
├── src/                    # cały kod źródłowy (.cpp/.h)
├── assets/
│   ├── textures/           # grafiki (.png)
│   ├── audio/               # dźwięki i muzyka (.wav/.ogg)
│   ├── fonts/                # czcionka (.ttf)
│   └── levels/                # mapy poziomów (.txt)
└── external/SFML/          # (nieśledzone w git) lokalna kopia biblioteki SFML
```

### Klasy — krótki przewodnik

| Plik | Odpowiedzialność |
|---|---|
| `main.cpp` | Punkt wejścia, okno menu głównego, pętla zdarzeń, ekran opcji |
| `Game.h/.cpp` | Główna klasa gry — pętla `Update()`/`Render()`, ładowanie poziomu, logika rozgrywki |
| `Player.h/.cpp`, `NewCharacter.h/.cpp` | Postać gracza — ruch, skok, animacja, kolizje |
| `Enemies.h/.cpp` | Logika i ruch przeciwników |
| `Animation.h/.cpp` | Odtwarzanie animacji sprite'ów (klatki z arkusza tekstur) |
| `Collider.h/.cpp` | Wykrywanie kolizji AABB między obiektami |
| `Level.h/.cpp`, `LevelView.h/.cpp` | Wczytywanie i renderowanie warstwy terenu/tła poziomu |
| `Platform.h/.cpp` | Platformy statyczne |
| `Coin.h/.cpp`, `Items.h/.cpp`, `NewBox.h/.cpp`, `SpecialObjects.h/.cpp`, `DangerousItems.h/.cpp` | Kolejne warstwy obiektów na mapie |
| `Menu.h/.cpp` | Menu główne (nawigacja klawiaturą/myszką) |

### 📍 Format map

Poziomy są opisane jako **siatki znaków w plikach `.txt`** w `assets/levels/` (`location.txt`, `textures.txt`, `coins.txt`, `enemies.txt`, `dangerousitems.txt`, `newbox.txt`, `specialobjects.txt`, `items.txt`) — każdy plik to osobna warstwa mapy, gdzie znak w danej komórce siatki wskazuje, jaki obiekt (i z jakiej tekstury) ma się tam pojawić.

---

## ▶️ Budowanie i uruchomienie

Projekt korzysta z **CMake**, dzięki czemu otwiera się i buduje zarówno w Visual Studio (2019+, przez "Open Folder"), jak i z linii poleceń na Windows/Linux/macOS.

### 1. Pobierz SFML 2.5

Pobierz [SFML 2.5.1](https://www.sfml-dev.org/download/sfml/2.5.1/) dla swojego kompilatora i rozpakuj tak, aby powstała struktura:

```
external/SFML/
├── include/SFML/...
├── lib/...
└── bin/...   (tylko Windows — pliki .dll)
```

> Alternatywnie: zainstaluj SFML systemowo (np. `apt install libsfml-dev` na Linuksie, `vcpkg install sfml` na Windows) — jeśli katalog `external/SFML/` nie istnieje, `CMakeLists.txt` automatycznie spróbuje znaleźć SFML zainstalowane w systemie.

### 2. Zbuduj

```bash
cmake -S . -B build
cmake --build build
```

Plik wykonywalny oraz katalog `assets/` (a na Windows też `.dll` SFML) zostaną automatycznie skopiowane do katalogu builda — grę uruchamiasz stamtąd bez żadnej dodatkowej konfiguracji.

---

## 📄 Licencja

Projekt udostępniony na licencji [MIT](LICENSE).

Last update 07.2020
