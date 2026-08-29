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

## 5. Actividad 5 A* con Manhattan
- <img width="640" height="328" alt="image" src="https://github.com/user-attachments/assets/e76838da-58c9-4f1a-a15c-f4344a66fbac" />


| Algoritmo | Costo | Expandidos | Tiempo |
|---|---:|---:|---:|
| UCS | 30 | 32 | 0.001415 s |
| A* + Manhattan | 30 | 32 | 0.001551 s |

## ¿Por qué la distancia Manhattan puede orientar correctamente a Pac-Man aunque la heurística no considere directamente las paredes del laberinto?

La distancia Manhattan se calcula como:

$$
h_M(n) = |x_n - x_g| + |y_n - y_g|
$$

Esta fórmula mide cuántos pasos sobre la cuadrícula separan la posición actual de la meta, **sin tener en cuenta si existen paredes en medio**.

A pesar de esto, sigue siendo una heurística **admisible**, porque nunca sobreestima el costo real. El camino verdadero, considerando las paredes, siempre requiere una cantidad de pasos **igual o mayor** que la distancia Manhattan, pero nunca menor.

Esta propiedad permite que A* continúe garantizando una solución óptima, aunque la heurística no tenga información sobre los obstáculos.

Además, aunque ignore las paredes, la heurística sí logra **orientar la dirección general** de la búsqueda. En cada paso, A* prioriza los nodos que parecen estar más cerca de la meta, evitando explorar primero direcciones que se alejan claramente de ella.

Esto permite guiar eficazmente la búsqueda en laberintos con pocos obstáculos grandes, aunque Pac-Man todavía debe rodear las paredes cuando estas bloquean el camino directo.

En este caso particular, utilizando `mediumMaze`, el laberinto tiene una estructura similar a un corredor y posee pocas rutas alternativas. Por esta razón, la distancia Manhattan no logró reducir el número de nodos expandidos frente a UCS: ambos algoritmos expandieron **32 nodos**.

Además, el tiempo de A* fue ligeramente mayor debido al costo adicional de calcular la heurística en cada expansión. Sin embargo, esto no invalida su utilidad, ya que la heurística:

- Mantiene la optimalidad de la solución.
- Orienta la búsqueda hacia la meta.
- Puede reducir significativamente los nodos expandidos en laberintos con más ramificaciones o rutas alternativas.


En esta ejecución, el tiempo de A* (`0.001078 s`) fue ligeramente menor que el de UCS (`0.001290 s`). Esto confirma que la diferencia de tiempo entre ambos algoritmos no es estructural, sino producto de pequeñas variaciones normales del sistema operativo al medir intervalos tan cortos, del orden de milisegundos.
Esto refuerza la conclusión de que, **algorítmicamente, UCS es un caso particular de A* cuando `h(n) = 0`**.

## 6. Actividad 6 Comparacion de heuristicas
- <img width="886" height="650" alt="image" src="https://github.com/user-attachments/assets/f22eae48-9305-4c2a-bae6-f5a9c1268d31" />


| Heurística | Longitud | Costo | Expandidos | Tiempo |
|---|---:|---:|---:|---:|
| h(n) = 0 | 30 | 30 | 32 | 0.001078 s |
| Manhattan | 30 | 30 | 32 | 0.001551 s |
| Euclidiana | 30 | 30 | 32 | 0.000981 s |

## Pac-Man se desplaza únicamente horizontal o verticalmente. ¿Cuál de las dos heurísticas representa mejor este tipo de movimiento? Justifique con los resultados experimentales.

La **distancia Manhattan** representa mejor el movimiento de Pac-Man, ya que se calcula como:

$$
h_M(n) = |x_n-x_g| + |y_n-y_g|
$$

Esta fórmula suma los desplazamientos horizontales y verticales, que corresponden exactamente a los movimientos permitidos para Pac-Man: `North`, `South`, `East` y `West`. Pac-Man nunca puede desplazarse diagonalmente.

En cambio, la **distancia Euclidiana** se calcula como:

$$
h_E(n) = \sqrt{(x_n-x_g)^2+(y_n-y_g)^2}
$$

Esta heurística mide la distancia en línea recta entre la posición actual y la meta, incluyendo una dirección diagonal que Pac-Man **no puede recorrer directamente**.

En una cuadrícula se cumple que:

$$
h_E(n) \leq h_M(n)
$$

Por esta razón, la distancia Euclidiana generalmente subestima más el costo real que la distancia Manhattan. Aunque ambas son heurísticas admisibles, Manhattan es **más informativa** para este problema porque sus valores suelen estar más cerca del costo real sin sobreestimarlo.

### Resultados experimentales

| Heurística | Longitud | Costo | Nodos expandidos | Tiempo |
|---|---:|---:|---:|---:|
| `h(n) = 0` | 30 | 30 | 32 | 0.001078 s |
| Manhattan | 30 | 30 | 32 | 0.001551 s |
| Euclidiana | 30 | 30 | 32 | 0.000981 s |

En `mediumMaze`, las tres heurísticas expandieron **32 nodos** y encontraron una solución óptima con costo y longitud de **30**. Esto sucede porque el laberinto tiene pocas rutas alternativas, por lo que las heurísticas no tuvieron suficientes oportunidades para evitar exploraciones innecesarias.

Las diferencias de tiempo son muy pequeñas y se deben principalmente a variaciones normales del sistema al medir intervalos de milisegundos. Por lo tanto, no demuestran que la heurística Euclidiana sea más eficiente que Manhattan.

En conclusión, **Manhattan es la heurística más adecuada para Pac-Man**, porque representa directamente sus movimientos horizontales y verticales. Aunque en este experimento no redujo el número de nodos expandidos, en laberintos con más ramificaciones se esperaría que proporcionara una mejor orientación y expandiera menos nodos que la distancia Euclidiana.

## 7. Actividad 7 Diseñando el estado

-<img width="957" height="572" alt="image" src="https://github.com/user-attachments/assets/e2802c12-dc1a-49ff-985c-8bc0de92310f" /> 

- <img width="451" height="382" alt="image" src="https://github.com/user-attachments/assets/9187de3e-d8b1-4123-a572-c446d0a9a35e" />

**Laberinto:** `tinyCorners`

**Comando:**

```bash
python pacman.py -l tinyCorners -p SearchAgent -a fn=ucs,prob=CornersProblem
```

| Métrica | Resultado |
|---|---|
| Costo del camino | 22 |
| Nodos expandidos | 296 |
| Tiempo | 0.000805 s |
| Resultado del juego | Victoria — Score 518 |

### Representación del estado utilizada

```python
s = (posición, esquinas_visitadas)
```

Donde posición es una tupla (x, y) y esquinas_visitadas es una tupla de 4 booleanos (uno por cada esquina en self.corners), que indica si esa esquina ya fue visitada. Esto resuelve el problema planteado en la guía: dos estados con la misma posición (5,4) ahora son distinguibles según cuántas esquinas se hayan visitado previamente, evitando que UCS/A* los confunda como el mismo nodo.

Funciones implementadas:

getStartState: posición inicial + (False, False, False, False).
isGoalState: es meta cuando all(visited) es True, es decir, las 4 esquinas fueron visitadas.
getSuccessors: por cada movimiento válido, genera un nuevo estado; si la nueva posición coincide con una esquina, la marca como visitada en la tupla, sin alterar las demás.
