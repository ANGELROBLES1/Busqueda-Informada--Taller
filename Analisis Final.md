## Análisis final

### 1. ¿Cuál es la diferencia fundamental entre UCS y A*?

UCS ordena la frontera solo por el costo acumulado `g(n)`; no sabe nada sobre qué tan cerca está la meta. A* ordena por `f(n) = g(n) + h(n)`, sumando una estimación del costo restante. Esa información extra es lo que le permite a A* orientar la búsqueda hacia la meta en vez de explorar "a ciegas" en todas direcciones por igual.

### 2. ¿Qué función cumple g(n)?

Es el costo real ya acumulado desde el estado inicial hasta `n`. Es un valor exacto, no una estimación — se conoce con certeza en cuanto se llega a `n`.

### 3. ¿Qué función cumple h(n)?

Es una estimación del costo que falta desde `n` hasta la meta. A diferencia de `g(n)`, es una proyección, no un valor exacto — y su calidad (qué tan cerca está de la distancia real) es lo que determina qué tan bien guía la búsqueda.

### 4. ¿Qué ocurre cuando h(n) = 0?

`f(n) = g(n) + 0 = g(n)`, así que A* se reduce exactamente a UCS. Lo comprobamos empíricamente en las Actividades 8 y 11: con `h=0`, A* expande el mismo número de nodos que UCS (295 en `tinyCorners`, 2598 en `testClassic`).

### 5. ¿Cuál presentó mejores resultados: Manhattan o Euclidiana?

En `mediumMaze` (Actividad 6) ambas llegaron al mismo costo óptimo (30) expandiendo el mismo número de nodos (32) — el laberinto tiene poca ramificación y no les dio oportunidad de diferenciarse en la práctica.

Aun así, **Manhattan es la más adecuada para Pac-Man**: como los movimientos son solo horizontales o verticales (nunca diagonales), `h_M(n) = |x_n-x_g| + |y_n-y_g|` describe exactamente ese tipo de desplazamiento. La Euclidiana mide línea recta (`h_E(n) ≤ h_M(n)` siempre en una cuadrícula), así que subestima más el costo real — sigue siendo admisible, pero menos informativa. En un laberinto con más rutas alternativas, Manhattan expandiría menos nodos que Euclidiana.

### 6. ¿Una heurística más grande siempre es mejor? Justifique.

Solo si se mantiene **admisible** (`h(n) ≤ h*(n)`). Entre dos heurísticas admisibles, la más grande (más informativa) expande menos nodos — lo vimos directamente: en `CornersProblem`, la propuesta (que domina a la básica en todos los estados) expandió 77 nodos contra 147 de la básica. Pero si la heurística crece más allá de `h*(n)` deja de ser admisible, y A* puede devolver una solución subóptima. Más grande solo ayuda mientras siga siendo una cota inferior real.

### 7. ¿Por qué una heurística que sobreestima puede ser problemática?

Porque puede hacer que A* descarte un camino que en realidad es óptimo, si su `f(n)` parece (falsamente) peor que el de un camino subóptimo. A* deja de garantizar la solución óptima en cuanto `h(n) > h*(n)` para algún estado.

### 8. ¿Por qué el estado de CornersProblem necesita almacenar más información que únicamente (x, y)?

Porque `(x, y)` sola no distingue el progreso del agente: dos visitas a la misma posición pueden representar situaciones distintas si en una ya se visitaron esquinas y en la otra no. Sin el registro de esquinas visitadas, el algoritmo trataría como "el mismo estado" (y por lo tanto como ya visitado) situaciones que en realidad tienen objetivos pendientes distintos, y nunca completaría el problema correctamente.

### 9. ¿Por qué FoodSearchProblem presenta un espacio de estados considerablemente mayor?

Porque en vez de 4 esquinas con `2^4 = 16` combinaciones posibles de visitadas/pendientes, hay `F` alimentos con `2^F` combinaciones posibles de presente/consumido, multiplicado por cada posición de Pac-Man. Con solo 8 alimentos (`testClassic`) ya son hasta 256 configuraciones de comida por posición — muchas más que las 16 de `CornersProblem`.

### 10. ¿Qué relación encontró entre la calidad de la heurística y el número de nodos expandidos?

Una relación inversa y monótona, confirmada en ambos problemas:

| Heurística | `CornersProblem` (tinyCorners) | `FoodSearchProblem` (testClassic) |
|---|---:|---:|
| `h = 0` (= UCS) | 295 | 2598 |
| Básica | 147 | 702 |
| Propuesta (más informativa) | 77 | 111 |

Mientras más informativa la heurística (más cerca de `h*(n)` sin sobreestimar), menos nodos expande A* para encontrar el mismo costo óptimo. La propuesta, al dominar a la básica en todos los estados, siempre expande igual o menos nodos que ella.

---

## Resultados generales

| Experimento | Costo | Expandidos | Tiempo | Óptimo |
|---|---:|---:|---:|:---:|
| UCS (`mediumMaze`) | 30 | 32 | 0.00086 s | ✓ |
| A* + heurística nula (`mediumMaze`) | 30 | 32 | 0.00108 s | ✓ |
| A* + Manhattan (`mediumMaze`) | 30 | 32 | 0.00155 s | ✓ |
| A* + Euclidiana (`mediumMaze`) | 30 | 32 | 0.00098 s | ✓ |
| A* + Corners Heuristic (`tinyCorners`) | 22 | 147 | 0.00092 s | ✓ |
| A* + Food Heuristic (`testClassic`) | 16 | 702 | 0.023 s | ✓ |

Las primeras cuatro filas son de `PositionSearchProblem` sobre `mediumMaze` (Actividades 2–6); las dos últimas son `CornersProblem` y `FoodSearchProblem` (Actividades 8–11). En `mediumMaze` las heurísticas no lograron reducir los nodos expandidos porque el laberinto tiene poca ramificación — a diferencia de `tinyCorners` y `testClassic`, donde sí se ve una reducción clara.
