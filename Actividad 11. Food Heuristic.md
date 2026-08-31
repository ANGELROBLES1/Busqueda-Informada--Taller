## 11. Actividad 11 — Heurística para alimentos

<p align="center">
  <img
    <img width="886" height="1006" alt="image" src="https://github.com/user-attachments/assets/43b90d6c-eb3f-48f8-a6ed-18597d985c81" />

    width="700"
    alt="Ejecución de A* con foodHeuristicPropuesta en testClassic"
  >
</p>

### Configuración de la prueba

**Laberinto:** `testClassic` (8 alimentos)

```bash
python pacman.py -l testClassic -p SearchAgent -a fn=aStarSearch,prob=FoodSearchProblem,heuristic=foodHeuristicPropuesta
```

### Resultados

| Heurística | Costo | Expandidos | Tiempo |
|---|---:|---:|---:|
| `h = 0` | 16 | 2598 | 0.063 s |
| Heurística 1 (básica) | 16 | 702 | 0.023 s |
| **Heurística 2 (propuesta)** | 16 | **111** | **0.004 s** |

`R` respecto a UCS (2598 expandidos): básica ≈ **3.7**, propuesta ≈ **23.4**.

### Heurística 1 — básica

Igual idea que `cornersHeuristic`: distancia Manhattan al alimento pendiente más lejano.

```python
position, foodGrid = state
foodList = foodGrid.asList()
if not foodList:
  return 0
return max(util.manhattanDistance(position, food) for food in foodList)
```

### Heurística 2 — propuesta

Generaliza `cornersHeuristicPropuesta`: distancia al alimento más cercano + costo del MST entre los alimentos pendientes.

```python
def foodHeuristicPropuesta(state, problem):
  position, foodGrid = state
  foodList = foodGrid.asList()
  if not foodList:
    return 0

  nearest = min(util.manhattanDistance(position, food) for food in foodList)

  if len(foodList) == 1:
    mst = 0
  else:
    inTree = {foodList[0]}
    remaining = set(foodList[1:])
    mst = 0
    while remaining:
      best = min((util.manhattanDistance(a, b), b) for a in inTree for b in remaining)
      mst += best[0]
      inTree.add(best[1])
      remaining.remove(best[1])

  return nearest + mst
```

**Admisible y consistente** por el mismo argumento que en `CornersProblem` (suma de dos cotas inferiores en Manhattan). Verificado sobre los **3892 estados alcanzables** de `testClassic`: admisible y consistente en el 100%, y domina a la Heurística 1 en todos los estados.

### Reto — caché con `problem.heuristicInfo`

El cálculo que se repite entre estados es la distancia entre pares de alimentos (los alimentos no se mueven). Se probó cacheándola en `problem.heuristicInfo['foodDistances']`.

**Resultado honesto: no ayudó — de hecho fue más lento.** En `testClassic` (8 alimentos) el tiempo con y sin caché fue el mismo (0.0047 s). En un experimento más exigente con 300 llamadas sobre subconjuntos de los 97 alimentos de `mediumClassic`: **sin caché 3.44 s vs. con caché 6.38 s**.

**Por qué:** `util.manhattanDistance` ya es una operación trivial (dos restas). El costo de armar la llave de la caché (tupla, comparación, búsqueda en diccionario) es *más caro* que simplemente recalcular la resta. Cachear solo vale la pena cuando lo que se guarda es costoso de calcular — por ejemplo, una distancia real de laberinto vía BFS, no una distancia Manhattan.

### Conclusión

M�s información en la heurística (Heurística 2) redujo los nodos expandidos casi 23 veces respecto a UCS sin perder optimalidad. Pero no toda optimización "obvia" ayuda: cachear un cálculo que ya es barato puede hacer las cosas más lentas, no más rápidas.
