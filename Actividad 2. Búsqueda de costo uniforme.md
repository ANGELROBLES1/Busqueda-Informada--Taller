## 2. Actividad 2 — Búsqueda de costo uniforme

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/53cf2c12-ab4c-4e1b-bbb8-87c4c6265368"
    width="570"
    alt="Ejecución de la búsqueda de costo uniforme"
  >
</p>

| Métrica | Resultado | Razón |
|---|---:|---|
| Costo del camino | 30 | UCS garantiza encontrar el camino de **menor costo posible**; como cada movimiento cuesta 1 (`TIME_PENALTY = 1`), el costo total equivale al mínimo de pasos necesarios entre inicio y meta. |
| Longitud del camino | 30 | Coincide con el costo porque cada acción tiene `stepCost = 1`; no hay acciones con costo distinto en este problema. |
| Nodos expandidos | 32 | UCS expande el nodo de menor `g(n)` en cada iteración, sin ninguna heurística que oriente la búsqueda hacia la meta. Por eso expande algunos nodos adicionales —32 en vez de 30— antes de confirmar el camino óptimo, algo típico de la búsqueda no informada. |
| Tiempo | 0.000861168 s | Tiempo real medido después de aumentar la precisión de impresión (`%.9f` en `searchAgents.py`). Confirma que la búsqueda fue muy rápida —menos de 1 milisegundo—, lo cual es consistente con un laberinto pequeño de solo 32 nodos expandidos. |

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/85ffe3b6-b86d-4205-ba2b-ddcbbe3dac98"
    width="750"
    alt="Código de la búsqueda de costo uniforme"
  >
</p>

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/2864eaef-79ec-46f6-99ce-e8cb633ed553"
    width="700"
    alt="Resultados obtenidos con la búsqueda de costo uniforme"
  >
</p>

La captura muestra la ejecución gráfica de UCS (fn=ucs) sobre el laberinto mediumMaze. Las celdas sombreadas en rojo/rosado representan los estados que el algoritmo ya expandió durante la búsqueda, mientras que Pac-Man (el círculo amarillo) se encuentra en su posición actual dentro de ese recorrido. Se observa que UCS explora de manera uniforme el espacio alcanzable desde el punto de partida, expandiendo los nodos en orden de menor costo acumulado g(n), sin ninguna preferencia por la dirección hacia la meta —por eso el área sombreada crece de forma compacta alrededor del punto de inicio, en vez de dirigirse directamente hacia el objetivo. El marcador SCORE: 54 en la parte inferior refleja los puntos acumulados por Pac-Man hasta ese instante de la partida (comida recogida menos la penalización por cada movimiento).
