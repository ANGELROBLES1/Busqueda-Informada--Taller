# BusquedaInformada
## 1. Actividad 1. Exploración del entorno
- <img width="570" height="387" alt="image" src="https://github.com/user-attachments/assets/5676ab36-256a-43ff-ae93-f250c2783a83" />


| Elemento | Descripción | Código |
|---|---|---|
| Estado | La configuración completa del juego: posición de Pac-Man, posición de fantasmas, comida y cápsulas restantes, y *score*. | Clase `GameState`, archivo `pacman.py`, línea 41. |
| Estado inicial | Estado con el que arranca el juego: Pac-Man en su posición de *spawn*, toda la comida presente, fantasmas en su posición inicial. | Se genera en `Game/GameState` a partir del *layout* leído; es visible al inicio de la ejecución de `python pacman.py`. |
| Acciones | Los cuatro movimientos posibles: North, South, East, West (y Stop). | Clase `Directions`, archivo `game.py`, líneas 33–38. Las acciones legales en cada estado se obtienen con `getLegalActions()`, `pacman.py`, línea 60. |
| Función sucesora | Dado un estado y una acción válida, genera el nuevo estado: mueve a Pac-Man, actualiza comida comida, mueve fantasmas. | Método `generateSuccessor(agentIndex, action)`, `pacman.py`, línea 71. |
| Objetivo | Que no quede comida en el tablero (`isWin`) sin haber sido atrapado por un fantasma (`isLose`). | Métodos `isWin()` en línea 187 e `isLose()` en línea 184 de `pacman.py`. La comida restante se consulta con `getNumFood()` (línea 151) y `getFood()` (línea 154). |
| Costo | Cada movimiento cuesta 1 punto (`TIME_PENALTY`); ganar suma 500 y ser atrapado resta 500. | Constante `TIME_PENALTY = 1` en línea 240; penalización aplicada en línea 90 (`scoreChange += -TIME_PENALTY`); bono de victoria `+500` en línea 349; penalización de derrota `-500` en línea 424, todo en `pacman.py`. |


## 2. Actividad 2. Búsqueda de costo uniforme
- <img width="791" height="375" alt="image" src="https://github.com/user-attachments/assets/53cf2c12-ab4c-4e1b-bbb8-87c4c6265368" />


| Métrica | Resultado | Razón |
|---|---:|---|
| Costo del camino | 30 | UCS garantiza encontrar el camino de **menor costo posible** y, como cada movimiento en Pac-Man cuesta 1 (por `TIME_PENALTY = 1`), el costo total equivale al número mínimo de pasos necesarios para llegar del inicio a la meta en `mediumMaze`. |
| Longitud del camino | 30 | Coincide exactamente con el costo porque cada acción individual (North/South/East/West) tiene `stepCost = 1`; no hay acciones con costo distinto en este problema, así que “número de pasos” y “costo acumulado” tienen el mismo valor. |
| Nodos expandidos | 32 | UCS expande el nodo de menor `g(n)` en cada iteración, explorando primero los estados más cercanos al inicio en términos de costo. Como no usa ninguna heurística que oriente la búsqueda hacia la meta, expande algunos nodos “de más” (32 en vez de 30) antes de confirmar cuál es el camino óptimo; esto es normal en una búsqueda no informada. |
| Tiempo | 0.0 s | El programa redondea el tiempo a un decimal (`%.1f` segundos en `searchAgents.py`). Como el laberinto es pequeño (solo 32 nodos expandidos), la búsqueda tomó menos de 0.05 segundos reales —del orden de milisegundos—, por lo que al redondear se muestra como 0.0. No significa que la búsqueda fuera instantánea, sino que fue demasiado rápida para la precisión del formato de impresión. |


- <img width="1102" height="680" alt="image" src="https://github.com/user-attachments/assets/85ffe3b6-b86d-4205-ba2b-ddcbbe3dac98" />
- <img width="976" height="147" alt="image" src="https://github.com/user-attachments/assets/bbd417e6-189d-4c66-9ae3-65775a6aebf2" />

