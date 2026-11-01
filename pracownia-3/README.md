# Pracownia 3 - Konteneryzacja TinyLlama (Docker Compose)

## Opis
Projekt polegał na przeniesieniu usługi z modelem **TinyLlama-1.1B** z tradycyjnej usługi `systemd` (z Pracowni 2) do kontenera Docker zarządzanego przez Docker Compose.

### Co zostało zrobione:
- **Dwuetapowy Dockerfile (multi-stage)**: Pobieranie i weryfikacja sumy SHA-256 w etapie `builder`, dzięki czemu obraz końcowy bazujący na `debian:trixie-slim` jest czysty i lekki.
- **Bezpieczeństwo**: Utworzenie dedykowanego użytkownika `llamafile` bez uprawnień roota.
- **Konfiguracja Compose**: Wstawienie `healthcheck`, polityki `restart: always` oraz przypisanie portu do adresu Host-Only (`192.168.128.2:8080`).

---

## Porównanie: systemd vs Docker Compose

| Cecha | systemd (Pracownia 2) | Docker Compose (Pracownia 3) |
| :--- | :--- | :--- |
| **Izolacja** | Brak – proces działa bezpośrednio na maszynie VM. | Pełna izolacja kontenera (osobny system plików i sieć). |
| **Zależności** | Instalowane bezpośrednio w systemie operacyjnym. | Zamknięte wewnątrz obrazu Dockera. |
| **Przenośność** | Wymaga przygotowania konkretnej maszyny VM. | Wystarczy uruchomić `docker compose up` na dowolnym sprzęcie. |
| **Restart przy awarii** | Obsługiwany przez `Restart=always` w `.service`. | Obsługiwany przez `restart: always` w `compose.yaml`. |

---

## Screeny z weryfikacji

### 1. Status kontenera (docker compose ps)
![Screen 1](1.png)

### 2. Nasłuchiwanie portu 8080 (ss -tlnp)
![Screen 2](2.png)

### 3. Test API z poziomu macOS (curl)
![Screen 3](3.png)

### 4. Test uśmiercenia kontenera (docker kill)
![Screen 4](4.png)
