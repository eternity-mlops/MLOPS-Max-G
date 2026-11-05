# Pracownia 4 - Środowisko K8s (k3s)

Zadanie polegało na uruchomieniu modelu językowego w lokalnym klastrze k3s.

## 1. Uruchomienie klastra i manifesty
Pliki manifestów zostały umieszczone w katalogu `k8s/`:
- `configmap.yaml` – ustawienie THREADS: "2"
- `deployment.yaml` – uruchomienie kontenera bez roota (runAsNonRoot: true), zmiana HOME na /tmp, limity RAM/CPU oraz startupProbe
- `service.yaml` – wystawienie usługi przez NodePort

Polecenie uruchamiające:
`kubectl apply -f k8s/`

## 2. Testy REST API
Test działania usługi:
`curl -s http://localhost:32061/v1/models`

Odpowiedź z modelu `1.png`.

## 3. Aktualizacja i wycofanie (Rollout)
Test nieudanej aktualizacji do wersji `llm:1.1`:
`kubectl set image deployment/llm llm=llm:1.1`

Wycofanie do stabilnej wersji:
`kubectl rollout undo deploy/llm`

Wynik wycofania zmian `2.png`.

## 4. Porównanie: Docker Compose vs k3s

| Metryka | Docker Compose | Kubernetes k3s |
|---|---|---|
| Zużycie RAM przez model | ~1.9 GB | ~1.9 GB |
| Zużycie RAM przez system | ~200 MB | ~600 MB |
| Czas do 1. odpowiedzi | ~15 s | ~25 s |
| Liczba kroków uruchomienia | 2 kroki | 4 kroki |
