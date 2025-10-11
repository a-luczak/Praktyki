# Praktyki

**PT100_App.py** to aplikacja graficzna (GUI) w Pythonie służąca do zarządzania i monitorowania czujników temperatury **PT100** komunikujących się przez port szeregowy (np. z mikrokontrolerem typu Arduino).  
Program pozwala na konfigurację czujników, odczyt temperatur, logowanie danych do pliku CSV i wizualizację przebiegu temperatury w czasie.

---

## Funkcje

- Automatyczne wykrywanie i łączenie z portami szeregowymi  
- Odczyt i konfiguracja czujników PT100 przez komendy tekstowe (`LIST`, `READ`, `NEW`, `SET`, `DEL`)  
- Zapisywanie pomiarów do pliku CSV (ręcznie lub automatycznie)  
- Wykres temperatury w czasie (z możliwością ograniczenia okna czasowego)  
- Tabela z aktualnymi danymi czujników  
- Log tekstowy wszystkich komunikatów i poleceń  

---

## Wymagania

Aplikacja wymaga Pythona **3.8+** oraz następujących bibliotek:

```
bash
pip install pyserial matplotlib PySide6
```
Jeśli nie masz PySide6, aplikacja spróbuje automatycznie użyć PyQt6.

---

## Uruchomienie

1. Podłącz mikrokontroler z czujnikiem PT100 do komputera przez USB.

2. Uruchom aplikację:
```
python PT100_App.py
```

3. W oknie programu:

- kliknij Refresh Ports i wybierz odpowiedni port (np. COM3 lub /dev/ttyUSB0),

- naciśnij Connect,

- użyj przycisków LIST, READ, NEW, SET, DEL, aby komunikować się z urządzeniem,

- włącz opcję Auto save lub wybierz plik CSV, aby logować pomiary.

---

## Obsługiwane komendy (wysyłane do urządzenia)

| Komenda | Opis |
|------------|------------|
| LIST  | zwraca listę wszystkich zarejestrowanych czujników w formacie JSON  |
| READ id=X  | odczytuje temperaturę z czujnika o danym ID  |
| NEW id=X pin=A0 name=PT100  | tworzy nowy czujnik  |
| SET id=X name=NowyCzujnik interval=1000  | zmienia parametry czujnika  |
| DEL id=X  | usuwa czujnik  |

Urządzenie powinno odpowiadać w formacie JSON, np.:
```
{"id": 1, "name": "PT100", "pin": "A0", "t": 23.45}
```

lub (dla LIST):
```
{"s": [{"id": 1, "name": "PT100", "pin": "A0", "active": 1}]}
```
---

## Logowanie danych

Dane zapisywane są w formacie CSV:
```
timestamp_iso, epoch_ms, id, name, temp_c, source
```

Źródło (source) może przyjąć wartości:

- read – odczyt wykonany komendą READ

- interval – automatyczny pomiar cykliczny

- list_export – eksport z tabeli

---

## Struktura projektu

```
PT100_App.py        # główny plik programu
README.md           # opis projektu
```

## Architektura aplikacji

- SerialBackend – odpowiada za łączność szeregową, uruchamia wątek czytający dane i emituje sygnały do GUI.

- CsvLogger – zarządza zapisem pomiarów do pliku CSV.

- PT100App – główne okno aplikacji, w którym znajdują się:

a) wybór portu,

b) polecenia dla urządzenia,

c) tabela czujników,

d) wykres temperatur,

e) log komunikacji.


## Autorzy

Projekt edukacyjny stworzony przez Michała Dulka i Andrzeja Łuczaka w ramach praktyk na Politechnice Poznańskiej.

## Licencja

Ten projekt możesz dowolnie wykorzystywać do celów naukowych lub własnych testów.
Autorzy nie ponoszą odpowiedzialności za błędne odczyty lub uszkodzenia sprzętu.
