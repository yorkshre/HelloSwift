# Scenariusz Lekcji: Pierwsze kroki w świecie Swift i SwiftUI

* **Grupa docelowa:** Uczniowie szkół średnich / początkujący programiści
* **Wymagania wstępne:** Podstawowa logika programowania (zmienne, pętle)
* **Cele operacyjne:**
  * Zrozumienie podstawowych typów danych oraz różnicy między `let` a `var`.
  * Tworzenie i modyfikowanie struktur danych (tablice – `Array`).
  * Wykorzystanie instrukcji warunkowych (`if-else`) w logice aplikacji.
  * Pisanie i wywoływanie funkcji w Swifcie.
  * Zbudowanie pierwszego interaktywnego interfejsu w SwiftUI.

---

## CZĘŚĆ I: Fundamenty języka Swift

### 1. Zmienne i stałe (`let` vs `var`)
W Swifcie dbamy o bezpieczeństwo kodu. Domyślnie używamy stałych:
* `let` – stała (wartość, która nie ulegnie zmianie).
* `var` – zmienna (wartość, którą można modyfikować).

```swift
let nazwaSzkoly = "Technikum Informatyczne"
var poziomUcznia = 1

poziomUcznia += 1 // Działa poprawnie!
// nazwaSzkoly = "Liceum" // BŁĄD! Nie można modyfikować wartości 'let'
```

---

### 2. Tablice (`Array`) – Kolekcje danych
Gdy chcemy przechowywać listę elementów tego samego typu (np. ocen, zadań czy imion), używamy tablic.

```swift
// Jawna deklaracja tablicy z elementami typu String
var przedmiotySzkolne: [String] = ["Matematyka", "Informatyka", "Fizyka"]

// Dodawanie nowego elementu do tablicy
przedmiotySzkolne.append("Biologia")

// Odczytywanie elementu (indeksowanie rozpoczynamy od 0)
let pierwszyPrzedmiot = przedmiotySzkolne[0] // "Matematyka"
print("Liczba przedmiotów: \(przedmiotySzkolne.count)")
```

---

### 3. Instrukcje warunkowe (`if-else`)
Podejmowanie decyzji w kodzie realizujemy za pomocą instrukcji warunkowych. Nawiasy okrągłe wokół warunku są opcjonalne.

```swift
let sredniaOcen = 4.75

if sredniaOcen >= 4.75 {
    print("Gratulacje! Otrzymujesz stypendium naukowe.")
} else if sredniaOcen >= 3.0 {
    print("Rok zaliczony pozytywnie.")
} else {
    print("Musisz poprawić oceny.")
}
```

---

### 4. Funkcje (`func`) – Reużywalne bloki kodu
Funkcja to wydzielony fragment kodu, który wykonuje określone zadanie. Może przyjmować parametry i zwracać wynik.

```swift
// Funkcja przyjmuje liczbę punktów i zwraca sformatowany komunikat
func generujKomunikat(punkty: Int) -> String {
    if punkty > 50 {
        return "Świetny wynik! Poziom 2 odblokowany."
    } else {
        return "Zbieraj punkty dalej!"
    }
}

// Wywołanie funkcji
let wynikKomunikatu = generujKomunikat(punkty: 65)
print(wynikKomunikatu)
```

---

## CZĘŚĆ II: Projekt Praktyczny w SwiftUI

Łączymy całą zdobywczą wiedzę w praktyce! Tworzymy aplikację **Menedżer Zadań Ucznia**, w której użytkownik dodaje zadania do tablicy, a interfejs SwiftUI automatycznie odświeża się na ekranie.

```swift
import SwiftUI

struct ContentView: View {
    // Stan aplikacji: tablica zadań oraz aktualnie wpisywany tekst
    @State private var listaZadań: [String] = ["Zrobić projekt z Swifta", "Przygotować się do fizyki"]
    @State private var noweZadanie: String = ""
    
    // Funkcja dodająca nowe zadanie do tablicy
    func dodajZadanie() {
        // Oczyszczamy tekst z niepotrzebnych spacji
        let oczyszczonyTekst = noweZadanie.trimmingCharacters(in: .whitespaces)
        
        // Instrukcja warunkowa sprawdzająca, czy pole nie jest puste
        if !oczyszczonyTekst.isEmpty {
            listaZadań.append(oczyszczonyTekst)
            noweZadanie = "" // Reset pola tekstowego po dodaniu
        }
    }

    var body: some View {
        VStack(spacing: 20) {
            
            // Nagłówek
            Text("Mój Plan Działania")
                .font(.largeTitle)
                .fontWeight(.bold)
                .foregroundColor(.indigo)

            // Sekcja wprowadzania danych (Pole tekstowe + Przycisk w HStack)
            HStack {
                TextField("Wpisz nowe zadanie...", text: $noweZadanie)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                
                Button(action: {
                    dodajZadanie() // Wywołanie przygotowanej funkcji
                }) {
                    Text("Dodaj")
                        .fontWeight(.semibold)
                        .padding(.horizontal, 16)
                        .padding(.vertical, 8)
                        .background(Color.blue)
                        .foregroundColor(.white)
                        .cornerRadius(8)
                }
            }
            .padding(.horizontal)

            // Lista wyświetlająca elementy tablicy
            List {
                if listaZadań.isEmpty {
                    Text("Brak zadań na liście. Czas na odpoczynek!")
                        .foregroundColor(.secondary)
                } else {
                    ForEach(listaZadań, id: \.self) { zadanie in
                        Text("📌 \(zadanie)")
                            .font(.body)
                    }
                }
            }
            .listStyle(InsetGroupedListStyle())
            
        }
        .padding(.vertical)
    }
}
```

---

## Podsumowanie i Zadania dla Uczniów

### Kluczowe pojęcia lekcji:
1. **`let` / `var`**: Rozróżnianie stałych i zmiennych.
2. **`[String]`**: Zastosowanie tablic do przechowywania zestawów danych.
3. **`if-else`**: Sterowanie logiką wykonania programu.
4. **`func`**: Pisanie powtarzalnego kodu z parametrami i zwracaniem wartości.
5. **`@State` / SwiftUI**: Budowanie interaktywnych widoków reagujących na zmiany w danych.

---

### Zadanie Utrwalające (Praca domowa / Klasowa)

1. **Zadanie 1 (Podstawowe):** Dodaj przycisk „Usuń ostatnie”, który usunie ostatni element z tablicy `listaZadań` używając metody `.removeLast()`. *Pamiętaj o dodaniu warunku `if !listaZadań.isEmpty`, aby uniknąć błędu!*
2. **Zadanie 2 (Rozszerzone):** Dodaj funkcję `policzZadania() -> String`, która zwraca napis z aktualną liczbą zadań na liście (np. `"Pozostało zadań: 3"`) i wyświetl ten tekst pod nagłówkiem.