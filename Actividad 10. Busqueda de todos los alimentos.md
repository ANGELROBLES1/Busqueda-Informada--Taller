## 10. Actividad 10 — Búsqueda de todos los alimentos

<p align="center">
  <img
    <img width="890" height="992" alt="image" src="https://github.com/user-attachments/assets/a351d6dc-2712-41e9-994f-8ce088466795" />


    width="700"
    alt="Ejecución de FoodSearchProblem en testClassic"
  >
</p>

`FoodSearchProblem` ya viene implementado en el proyecto (no hay código que completar en esta actividad). El estado es `(pacmanPosition, foodGrid)`, y la meta se alcanza cuando `state[1].count() == 0` (ya no queda comida).

### Espacio de estados

Cada uno de los `F` alimentos puede estar en 2 condiciones (presente/consumido), así que el número de configuraciones de comida crece como `2^F`, multiplicado además por cada posición posible de Pac-Man. Esto es mucho más grande que en `CornersProblem` (que solo tenía `2^4 = 16` combinaciones de esquinas).

### Bug encontrado: `util.PriorityQueue`

Al correr UCS/A* con `FoodSearchProblem` sobre `testClassic`, la cola de prioridad falla:

```
TypeError: '<' not supported between instances of 'Grid' and 'Grid'
```

Motivo: cuando dos estados empatan en prioridad, `heapq` compara el siguiente elemento de la tupla — el estado completo, que aquí incluye un `Grid` (el `foodGrid`), y `Grid` no define `<`. No pasaba antes porque los estados de `PositionSearchProblem`/`CornersProblem` sí eran comparables (posiciones y booleanos).

**Fix en `util.py`** — agregar un contador como criterio de desempate en `push`/`pop`, para que `heapq` nunca necesite comparar el `item`:

```python
def __init__(self):
  self.heap = []
  self.count = 0

def push(self, item, priority):
  entry = (priority, self.count, item)
  heapq.heappush(self.heap, entry)
  self.count += 1

def pop(self):
  (priority, count, item) = heapq.heappop(self.heap)
  return item
```

No cambia qué algoritmo encuentra (sigue siendo óptimo), pero sí puede cambiar **cuántos nodos se expanden** cuando hay empates de prioridad — por eso los conteos de las Actividades 8 y 9 se ajustaron un poco (el costo óptimo, 22, no cambió en ningún caso).

### Configuración de la prueba

**Laberinto:** `testClassic` (8 alimentos)

```bash
python pacman.py -l testClassic -p SearchAgent -a fn=uniformCostSearch,prob=FoodSearchProblem
```

### Resultados

| Algoritmo | Costo | Expandidos | Tiempo |
|---|---:|---:|---:|
| UCS | 16 | 2598 | 0.102 s |
| A* + `h = 0` | 16 | 2598 | 0.072 s |

Sin heurística, A* se comporta igual que UCS (como en la Actividad 4) — la diferencia real vendrá con `foodHeuristic` en la Actividad 11.

### Conclusión

`FoodSearchProblem` ya funciona correctamente (una vez corregido `util.PriorityQueue`). El espacio de estados es considerablemente mayor que el de las esquinas, así que en la Actividad 11 una heurística informativa va a importar todavía más.
