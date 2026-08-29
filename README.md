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
| Costo del camino | 30 | UCS garantiza encontrar el camino de **menor costo posible**; como cada movimiento cuesta 1 (`TIME_PENALTY = 1`), el costo total equivale al mínimo de pasos necesarios entre inicio y meta. |
| Longitud del camino | 30 | Coincide con el costo porque cada acción tiene `stepCost = 1`; no hay acciones con costo distinto en este problema. |
| Nodos expandidos | 32 | UCS expande el nodo de menor `g(n)` en cada iteración, sin ninguna heurística que oriente la búsqueda hacia la meta. Por eso expande algunos nodos “de más” (32 en vez de 30) antes de confirmar el camino óptimo, algo típico de la búsqueda no informada. |
| Tiempo | 0.000861168 s | Tiempo real medido tras aumentar la precisión de impresión (`%.9f` en `searchAgents.py`). Confirma que la búsqueda fue muy rápida (menos de 1 milisegundo), consistente con lo esperado para un laberinto pequeño de solo 32 nodos. |


- <img width="1102" height="680" alt="image" src="https://github.com/user-attachments/assets/85ffe3b6-b86d-4205-ba2b-ddcbbe3dac98" />
- <img width="876" height="135" alt="image" src="https://github.com/user-attachments/assets/2864eaef-79ec-46f6-99ce-e8cb633ed553" />


## 3. Actividad 3 Programando A*
- <img width="797" height="463" alt="image" src="https://github.com/user-attachments/assets/87bf657f-06fc-4fdf-8eec-b6877f4f5041" />
- <img width="1357" height="702" alt="image" src="https://github.com/user-attachments/assets/3298bbc4-b299-4c22-bd24-93aad442037a" />


## 4. Actividad 4 A* sin información
- <img width="633" height="366" alt="image" src="https://github.com/user-attachments/assets/ae28fa6f-8fd0-45b4-8932-2541d18d79de" />
- <img width="873" height="131" alt="image" src="https://github.com/user-attachments/assets/59f323de-790b-49c8-9f43-93f673e376a0" />
- <img width="1055" height="155" alt="image" src="https://github.com/user-attachments/assets/9392210b-e43d-49ac-9d67-83141fb49242" />


| Algoritmo | Costo | Expandidos | Tiempo |
|---|---:|---:|---:|
| UCS | 30 | 32 | 0.001290 s |
| A* con h(n) = 0 | 30 | 32 | 0.001078 s |

### ¿Qué comportamiento observa? Explique por qué A* con h(n) = 0 presenta un comportamiento equivalente o muy similar a búsqueda de costo uniforme
Cuando la heurística es nula, la fórmula de A* se reduce a:

$$
f(n) = g(n) + h(n) = g(n) + 0 = g(n)
$$

Es decir, la prioridad usada por A* para ordenar la frontera de búsqueda queda determinada únicamente por el costo acumulado `g(n)`, exactamente igual que en UCS. Por eso ambos algoritmos expanden los nodos en el mismo orden y llegan al mismo resultado: costo óptimo de **30** y **32 nodos expandidos** en ambos casos.

En esta ejecución, el tiempo de A* (`0.001078 s`) fue ligeramente menor que el de UCS (`0.001290 s`). Esto confirma que la diferencia de tiempo entre ambos algoritmos no es estructural, sino producto de pequeñas variaciones normales del sistema operativo al medir intervalos tan cortos, del orden de milisegundos.

Esto refuerza la conclusión de que, **algorítmicamente, UCS es un caso particular de A* cuando `h(n) = 0`**.
