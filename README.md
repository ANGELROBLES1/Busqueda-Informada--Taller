# Taller: Búsqueda Informada con Pac-Man — Algoritmo A*

**Curso:** Inteligencia Artificial
**Tema:** Búsqueda informada, algoritmo A*, heurísticas (Manhattan y Euclidiana)
**Entorno base:** Proyecto Pac-Man (inspirado en CS221 / Berkeley AI)

---

## 1. Descripción del proyecto

Este taller utiliza el entorno de Pac-Man como espacio experimental para implementar y
comparar algoritmos de búsqueda: **búsqueda de costo uniforme (UCS)** y **A\***, evaluando
el efecto de distintas funciones heurísticas sobre el número de nodos expandidos y el costo
de la solución encontrada.

## 2. Estructura del proyecto

| Archivo | Función |
|---|---|
| `pacman.py` | Control principal del entorno Pac-Man. |
| `search.py` | Implementación de los algoritmos de búsqueda (UCS, A*). |
| `searchAgents.py` | Definición de problemas de búsqueda, agentes y heurísticas. |
| `util.py` | Estructuras de datos auxiliares (`PriorityQueue`, pilas, colas). |
| `layout.py` | Lectura y representación de los laberintos. |
| `layouts/` | Archivos `.lay` con los distintos mapas (`mediumMaze`, `tinyCorners`, etc.). |

## 3. Componentes del problema de búsqueda (Actividad 1)

| Elemento | Descripción |
|---|---|
| Estado | Configuración del juego: posición de Pac-Man, comida restante, fantasmas, score. |
| Estado inicial | Posición de arranque de Pac-Man con toda la comida presente. |
| Acciones | `North`, `South`, `East`, `West` (según paredes). |
| Función sucesor | `generateSuccessor` / `getSuccessors`: genera el estado resultante de una acción válida. |
| Objetivo | Ausencia de comida restante (`isGoalState`), sin haber sido atrapado por un fantasma. |
| Costo | 1 punto por movimiento (`TIME_PENALTY`); +500 al ganar; −500 al perder. |

## 4. Algoritmos implementados

### 4.1 Búsqueda de costo uniforme — `uniformCostSearch` (`search.py`)

Expande siempre el nodo de menor costo acumulado `g(n)`, usando `util.PriorityQueue()`.

```python
def uniformCostSearch(problem):
  frontier = util.PriorityQueue()
  startState = problem.getStartState()
  frontier.push((startState, [], 0), 0)
  visited = set()

  while not frontier.isEmpty():
    state, actions, cost = frontier.pop()
    if state in visited:
      continue
    visited.add(state)

    if problem.isGoalState(state):
      return actions

    for successor, action, stepCost in problem.getSuccessors(state):
      if successor not in visited:
        newCost = cost + stepCost
        frontier.push((successor, actions + [action], newCost), newCost)

  return None
```

### 4.2 A\* — `aStarSearch` (`search.py`)

Igual que UCS, pero la prioridad de la cola es `f(n) = g(n) + h(n)`, donde `h(n)` es la
función heurística recibida como parámetro.

```python
def aStarSearch(problem, heuristic=nullHeuristic):
  frontier = util.PriorityQueue()
  startState = problem.getStartState()
  startPriority = 0 + heuristic(startState, problem)
  frontier.push((startState, [], 0), startPriority)
  visited = set()

  while not frontier.isEmpty():
    state, actions, cost = frontier.pop()
    if state in visited:
      continue
    visited.add(state)

    if problem.isGoalState(state):
      return actions

    for successor, action, stepCost in problem.getSuccessors(state):
      if successor not in visited:
        newCost = cost + stepCost
        priority = newCost + heuristic(successor, problem)
        frontier.push((successor, actions + [action], newCost), priority)

  return None
```

### 4.3 CornersProblem — representación del estado (`searchAgents.py`)

Para resolver el problema de las cuatro esquinas, el estado se redefine como
`(posición, esquinas_visitadas)`, donde `esquinas_visitadas` es una tupla de 4 booleanos.
Esto distingue estados que comparten la misma posición `(x, y)` pero difieren en cuántas
esquinas ya fueron visitadas.

```python
def getStartState(self):
    visited = (False, False, False, False)
    return (self.startingPosition, visited)

def isGoalState(self, state):
    position, visited = state
    return all(visited)

def getSuccessors(self, state):
    successors = []
    for action in [Directions.NORTH, Directions.SOUTH, Directions.EAST, Directions.WEST]:
      position, visited = state
      x, y = position
      dx, dy = Actions.directionToVector(action)
      nextx, nexty = int(x + dx), int(y + dy)
      hitsWall = self.walls[nextx][nexty]

      if not hitsWall:
        nextPosition = (nextx, nexty)
        nextVisited = list(visited)
        if nextPosition in self.corners:
          cornerIndex = self.corners.index(nextPosition)
          nextVisited[cornerIndex] = True
        nextVisited = tuple(nextVisited)
        successors.append(((nextPosition, tuple(nextVisited)), action, 1))

    self._expanded += 1
    return successors
```

## 5. Comandos de ejecución

```bash
# Actividad 2 — UCS en mediumMaze
python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs

# Actividad 4 — A* con heurística nula
python pacman.py -l mediumMaze -p SearchAgent -a fn=astar,heuristic=nullHeuristic

# Actividad 5 — A* con Manhattan
python pacman.py -l mediumMaze -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic

# Actividad 6 — A* con Euclidiana
python pacman.py -l mediumMaze -p SearchAgent -a fn=astar,heuristic=euclideanHeuristic

# Actividad 7 — CornersProblem con UCS
python pacman.py -l tinyCorners -p SearchAgent -a fn=ucs,prob=CornersProblem
```

## 6. Resultados experimentales

**Laberinto:** `mediumMaze`

| Heurística | Longitud | Costo | Expandidos | Tiempo |
|---|---|---|---|---|
| UCS (línea base) | 30 | 30 | 32 | 0.001290 s |
| A* + h(n) = 0 | 30 | 30 | 32 | 0.001078 s |
| A* + Manhattan | 30 | 30 | 32 | 0.001551 s |
| A* + Euclidiana | 30 | 30 | 32 | 0.000981 s |

**Laberinto:** `tinyCorners` (CornersProblem)

| Algoritmo | Costo | Expandidos | Tiempo | Resultado |
|---|---|---|---|---|
| UCS | 22 | 296 | 0.000805 s | Victoria (Score 518) |

> **Nota sobre `bigMaze`:** se detectó que este laberinto tiene una región desconectada;
> la meta por defecto `(1,1)` es inalcanzable desde la posición inicial de Pac-Man (67 de
> 227 celdas abiertas no son alcanzables). Por eso UCS y A* reportan correctamente
> `Path found with total cost of 999999` (sin solución). No se usó para las comparaciones.

## 7. Conclusiones parciales

- En `mediumMaze`, al ser un laberinto tipo corredor con pocas rutas alternativas, UCS y
  A* (con cualquier heurística) expanden exactamente el mismo número de nodos (32) y
  encuentran el mismo costo óptimo (30). La ventaja de A* sobre UCS se hace evidente en
  laberintos con más ramificaciones, como se espera observar en las siguientes
  actividades (Corners, Food).
- La heurística Manhattan es más adecuada que la Euclidiana para Pac-Man, porque el
  agente solo se mueve horizontal o verticalmente (sin diagonales), coincidiendo
  exactamente con la definición matemática de la distancia Manhattan.
- Representar correctamente el espacio de estados (como en `CornersProblem`) es esencial:
  agregar información adicional al estado, además de la posición `(x, y)`, permite que el
  algoritmo distinga situaciones que de otro modo parecerían idénticas.

## 8. Estado del taller

| Actividad | Estado |
|---|---|
| 1. Exploración del entorno | ✅ Completa |
| 2. Búsqueda de costo uniforme | ✅ Completa |
| 3. Implementación de A* | ✅ Completa |
| 4. Heurística nula | ✅ Completa |
| 5. Distancia Manhattan | ✅ Completa |
| 6. Distancia Euclidiana | ✅ Completa |
| 7. Problema de las cuatro esquinas | ✅ Completa |
| 8. Heurística para las esquinas | ⏳ Pendiente |
| 9. Experimento comparativo (Corners) | ⏳ Pendiente |
| 10. Búsqueda de todos los alimentos | ⏳ Pendiente |
| 11. Diseñando foodHeuristic | ⏳ Pendiente |

---

**Referencia:** Stanford University, CS221: Artificial Intelligence — Pac-Man Assignment.
https://stanford-cs221.github.io/spring2023/assignments/pacman/index.html
