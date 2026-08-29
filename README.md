# Taller: Búsqueda Informada con Pac-Man — Algoritmo A*

**Curso:** Inteligencia Artificial
**Tema:** Búsqueda informada, algoritmo A*, heurísticas (Manhattan y Euclidiana)


---

## 1. Descripción del proyecto

Este taller utiliza el entorno de Pac-Man como espacio experimental para implementar y comparar algoritmos de búsqueda: **búsqueda de costo uniforme (UCS)** y **A\***, 
evaluando distintas funciones heurísticas sobre el número de nodos expandidos y el costode la solución encontrada.

## 2. Estructura del proyecto

| Archivo | Función |
|---|---|
| `pacman.py` | Control principal del entorno Pac-Man. |
| `search.py` | Implementación de los algoritmos de búsqueda (UCS, A*). |
| `searchAgents.py` | Definición de problemas de búsqueda, agentes y heurísticas. |
| `util.py` | Estructuras de datos auxiliares (`PriorityQueue`, pilas, colas). |
| `layout.py` | Lectura y representación de los laberintos. |
| `layouts/` | Archivos `.lay` con los distintos mapas (`mediumMaze`, `tinyCorners`, etc.). |

## 3. Componentes del problema de búsqueda 

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

### 4.2 A\* — `aStarSearch` (`search.py`)

Igual que UCS, pero la prioridad de la cola es `f(n) = g(n) + h(n)`, donde `h(n)` es la
función heurística recibida como parámetro.

### 4.3 CornersProblem — representación del estado (`searchAgents.py`)

Para resolver el problema de las cuatro esquinas, el estado se redefine como
`(posición, esquinas_visitadas)`, donde `esquinas_visitadas` es una tupla de 4 booleanos.
Esto distingue estados que comparten la misma posición `(x, y)` pero difieren en cuántas
esquinas ya fueron visitadas.



**Referencia:** Stanford University, CS221: Artificial Intelligence — Pac-Man Assignment.
https://stanford-cs221.github.io/spring2023/assignments/pacman/index.html
