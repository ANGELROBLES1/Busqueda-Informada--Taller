## 9. Actividad 9 — Experimento comparativo

<p align="center">
  <img
    <img width="977" height="247" alt="image" src="https://github.com/user-attachments/assets/0f6dd1cf-f17c-457a-bace-1571660bb052" />

    width="700"
    alt="Comparación de estrategias en tinyCorners"
  >
</p>

### Configuración de la prueba

**Laberinto:** `tinyCorners`

```bash
python pacman.py -l tinyCorners -p SearchAgent -a fn=aStarSearch,prob=CornersProblem,heuristic=cornersHeuristicPropuesta
```

### Resultados

| Método | Costo | Expandidos | Tiempo | Óptimo |
|---|---:|---:|---:|:---:|
| UCS | 22 | 296 | 0.00199 s | ✓ |
| A* + `h = 0` | 22 | 296 | 0.00132 s | ✓ |
| A* + heurística básica | 22 | 115 | 0.00083 s | ✓ |
| A* + heurística propuesta | 22 | **61** | 0.00048 s | ✓ |

### Factor de reducción de expansiones

$$
R = \frac{N_{UCS}}{N_{A*}}
$$

| Heurística | R |
|---|---:|
| `h = 0` | 296 / 296 = 1.0 |
| Básica | 296 / 115 ≈ 2.6 |
| **Propuesta** | 296 / 61 ≈ **4.9** |

La heurística propuesta expande **casi 5 veces menos nodos** que UCS, manteniendo el costo óptimo (22).

### Heurística propuesta

Combina la distancia a la esquina pendiente más cercana con el costo del árbol de expansión mínima (MST) entre las esquinas que faltan:

$$
h(n) = d_M(n, \text{esquina más cercana}) + MST(C_p)
$$

```python
def mstManhattan(points):
  if len(points) <= 1:
    return 0
  points = list(points)
  inTree = {points[0]}
  remaining = set(points[1:])
  total = 0
  while remaining:
    best = min(((util.manhattanDistance(a, b), b) for a in inTree for b in remaining))
    total += best[0]
    inTree.add(best[1])
    remaining.remove(best[1])
  return total

def cornersHeuristicPropuesta(state, problem):
  corners = problem.corners
  position, visited = state
  pending = [c for c, v in zip(corners, visited) if not v]
  if not pending:
    return 0
  nearest = min(util.manhattanDistance(position, c) for c in pending)
  return nearest + mstManhattan(pending)
```

**Por qué es admisible:** cualquier recorrido real que visite todas las esquinas pendientes forma, sobre esas esquinas, un árbol de conexión de costo mínimo al menos `MST(Cp)`; y antes de llegar a la primera esquina, el costo real es al menos la distancia a la más cercana. Ambos términos usan Manhattan, que nunca sobreestima la distancia real. Suma de dos cotas inferiores = cota inferior.

**Verificación empírica** (sobre los 408 estados alcanzables de `tinyCorners`, comparando contra el costo real vía BFS exhaustivo): la heurística propuesta es admisible y consistente en el 100% de los estados, y domina (≥) a la heurística básica en todos ellos.

### Conclusión

A mayor información en la heurística, menor exploración: pasar de `h=0` a la básica cortó los nodos a menos de la mitad, y pasar de la básica a la propuesta los volvió a cortar casi a la mitad otra vez — sin perder optimalidad en ningún caso.

> Recordatorio: la tabla no incluye `mediumCorners` porque ese layout viene desconectado en este repo (ver nota en la Actividad 8).
