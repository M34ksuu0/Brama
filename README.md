# Sterownik Bramy na Arduino (Serwa + IR + PIR)

Projekt automatycznej bramy dwuskrzydłowej sterowanej pilotem na podczerwień (IR), napędzanej dwoma serwomechanizmami, wyposażonej w funkcję bezpieczeństwa z czujnikiem ruchu (PIR) oraz sygnalizację LED.

## 📌 Funkcjonalności
* **Sterowanie pilotem IR:** Otwieranie, zamykanie oraz zatrzymywanie ruchu (funkcja STOP).
* **Zabezpieczenie PIR:** Jeśli podczas **zamykania** bramy czujnik PIR wykryje ruch, brama natychmiast automatycznie zawróci i się **otworzy**.
* **Sygnalizacja LED:** Dioda miga w trakcie pracy mechanizmu.
* **Non-blocking code:** Płynna praca oparta na funkcji `millis()`, co eliminuje blokujące opóźnienia `delay()`.

---

## 🔌 Podłączenie pinów (Wiring)

| Element | Pin w Arduino | Opis |
| :--- | :---: | :--- |
| **Odbiornik IR** | Pin `10` | Odbiór sygnałów z pilota |
| **Prawe Serwo** | Pin `3` | Sygnał sterujący prawego skrzydła |
| **Lewe Serwo** | Pin `5` | Sygnał sterujący lewego skrzydła |
| **Czujnik PIR** | Pin `9` | Wykrywanie przeszkód / ruchu |
| **Dioda LED** | Pin `12` | Sygnalizacja ruchu bramy |

---

## 📊 Schemat działania systemu (Architektura)

Poniższy diagram przedstawia przepływ danych i interakcje między komponentami systemu:

```mermaid
graph TD
    subgraph Wejscia [Sensory i Sterowanie]
        IR[Odbiornik IR - Pin 10]
        PIR[Czujnik PIR - Pin 9]
    end

    subgraph Mikrokomputer [Arduino]
        Loop[Pętla główna loop]
    end

    subgraph Wyjscia [Aktuatorki i Sygnalizacja]
        ServoP[Prawe Serwo - Pin 3]
        ServoL[Lewe Serwo - Pin 5]
        LED[Dioda LED - Pin 12]
    end

    IR -->|Komenda z pilota: Power/Stop| Loop
    PIR -->|Wykrycie ruchu podczas zamykania| Loop
    Loop -->|Aktualizacja pozycji skrzydła| ServoP
    Loop -->|Aktualizacja pozycji skrzydła| ServoL
    Loop -->|Miganie ostrzegawcze| LED
