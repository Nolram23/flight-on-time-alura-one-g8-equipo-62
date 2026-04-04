# Data Science Service API

API FastAPI que expone el modelo de predicción de retrasos de vuelos.

## Endpoints principales

POST /predict
Recibe información de un vuelo y devuelve si probablemente estará puntual o retrasado.

Ejemplo de entrada (JSON)

{
  "airline": "AZ",
  "origin": "GIG",
  "destination": "GRU",
  "departure_time": "2025-11-10T14:30:00",
  "distance_miles": 350
}


Ejemplo de salida (JSON)

{
  "prediction": "delayed",
  "probability": 0.68
}


## Cómo probar la API

1. Activar el entorno virtual (.venv) y levantar la API:

    uvicorn main:app --reload

2. Abrir otra terminal y usar PowerShell:
    
* Ejemplo 1 — Request JSON (un vuelo)
{
  "airline": "AZ",
  "origin": "GIG",
  "destination": "GRU",
  "distance_miles": 350,
  "departure_time": "2025-11-10T14:30:00"
}

* Ejemplo 2 — Vuelo nocturno (más probabilidad de atraso)

{
  "airline": "AA",
  "origin": "JFK",
  "destination": "LAX",
  "distance_miles": 2475,
  "departure_time": "2025-11-11T23:45:00"
}

* Ejemplo 3 — Vuelo temprano y corto

{
  "airline": "LA",
  "origin": "SCL",
  "destination": "LIM",
  "distance_miles": 1530,
  "departure_time": "2025-11-12T06:10:00"
}


* Ejemplo 4 — PowerShell (varios vuelos)

$vuelos = @(
    @{ airline="AZ"; origin="GIG"; destination="GRU"; distance_miles=350; departure_time="2025-11-10T14:30:00" },
    @{ airline="AA"; origin="JFK"; destination="LAX"; distance_miles=2475; departure_time="2025-11-11T23:45:00" }
)

foreach ($vuelo in $vuelos) {
    $body = $vuelo | ConvertTo-Json
    Invoke-RestMethod `
        -Uri http://127.0.0.1:8000/predict `
        -Method Post `
        -Body $body `
        -ContentType "application/json"
}



