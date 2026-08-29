## 2. Actividad 2. Búsqueda de costo uniforme
<p align="center">
  <img
    src="<img width="791" height="375" alt="image" src="https://github.com/user-attachments/assets/53cf2c12-ab4c-4e1b-bbb8-87c4c6265368" />"
    width="570"
    alt="Actividad 1 — Exploración del entorno"
  >
</p>

| Métrica | Resultado | Razón |
|---|---:|---|
| Costo del camino | 30 | UCS garantiza encontrar el camino de **menor costo posible**; como cada movimiento cuesta 1 (`TIME_PENALTY = 1`), el costo total equivale al mínimo de pasos necesarios entre inicio y meta. |
| Longitud del camino | 30 | Coincide con el costo porque cada acción tiene `stepCost = 1`; no hay acciones con costo distinto en este problema. |
| Nodos expandidos | 32 | UCS expande el nodo de menor `g(n)` en cada iteración, sin ninguna heurística que oriente la búsqueda hacia la meta. Por eso expande algunos nodos “de más” (32 en vez de 30) antes de confirmar el camino óptimo, algo típico de la búsqueda no informada. |
| Tiempo | 0.000861168 s | Tiempo real medido tras aumentar la precisión de impresión (`%.9f` en `searchAgents.py`). Confirma que la búsqueda fue muy rápida (menos de 1 milisegundo), consistente con lo esperado para un laberinto pequeño de solo 32 nodos. |

<p align="center">
  <img
    src="<img width="1102" height="680" alt="image" src="https://github.com/user-attachments/assets/85ffe3b6-b86d-4205-ba2b-ddcbbe3dac98" />"
    width="570"
    alt="Actividad 1 — Exploración del entorno"
  >
</p>

<p align="center">
  <img
    src="<img width="876" height="135" alt="image" src="https://github.com/user-attachments/assets/2864eaef-79ec-46f6-99ce-e8cb633ed553" />
"
    width="570"
    alt="Actividad 1 — Exploración del entorno"
  >
</p>

