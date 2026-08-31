## 8. Actividad 8 — Heurística para las esquinas

<p align="center">
  <img
    src="<img width="870" height="417" alt="image" src="https://github.com/user-attachments/assets/83eb9ee7-6990-4ff7-af29-1d73f17dd88a" />
"
    width="700"
    alt="Implementación de cornersHeuristic"
  >
</p>

<p align="center">
  <img
    src="URL_DE_TU_CAPTURA_AQUI_2"
    width="450"
    alt="Ejecución de A* con cornersHeuristic sobre tinyCorners"
  >
</p>

> Reemplaza las URLs por tus propias capturas (arrástralas al editor de GitHub para generar el link `user-attachments`).

El mismo camino óptimo de 22 pasos de la Actividad 7, ahora encontrado expandiendo muchos menos nodos gracias a la heurística.

### Configuración de la prueba

**Laberinto:** `tinyCorners`

```bash
python pacman.py -l tinyCorners -p SearchAgent -a fn=astar,prob=CornersProblem,heuristic=cornersHeuristic
```

### Resultados

| Algoritmo | Costo | Nodos expandidos | Tiempo |
|---|---:|---:|---:|
| UCS | 22 | 296 | 0.000992 s |
| A* + `h(n) = 0` | 22 | 296 | 0.000962 s |
| A* + `cornersHeuristic` | 22 | **115** | 0.000532 s |

Mismo costo óptimo en los tres casos; la heurística reduce los nodos expandidos casi a la mitad (`R ≈ 2.6` vs. UCS).

### Heurística implementada

```python
def cornersHeuristic(state, problem):
  corners = problem.corners
  walls = problem.walls

  position, visited = state
  pending = [corner for corner, wasVisited in zip(corners, visited) if not wasVisited]

  if not pending:
    return 0

  return max(util.manhattanDistance(position, corner) for corner in pending)
```

$$
h(n) = \max_{c \in C_p} d_M(n, c)
$$

`Cp` = esquinas aún no visitadas.

### Admisibilidad

`d_M(n,c)` nunca sobreestima la distancia real (las paredes solo alargan el camino, nunca lo acortan). Pac-Man debe llegar tarde o temprano a la esquina pendiente más lejana, así que ningún camino real puede costar menos que esa distancia: `0 ≤ h(n) ≤ h*(n)`.

### Consistencia

Para un sucesor `n'` con costo 1:

- Si `n'` no visita esquina nueva: un paso cambia cada `d_M` en máximo 1 → `h(n) ≤ h(n') + 1`.
- Si `n'` visita la esquina `c₀`: `d_M(n, c₀) = 1` y el resto se comporta como el caso anterior → `h(n) = max(1, h(n')+1) = h(n')+1`.

En ambos casos `h(n) ≤ c(n,n') + h(n')`, y `h(goal) = 0`. **Es consistente** (y por tanto admisible).

*Nota:* una heurística más informativa que considere las 4 esquinas a la vez queda para la Actividad 9 (`heurística propuesta`).

### Nota sobre `mediumCorners`

El layout del repo solo tiene 36 celdas alcanzables desde el inicio (ninguna esquina incluida) — parece corrupto/editado, ya que el original de Berkeley expande casi 2000 nodos con BFS. Revisar `layouts/mediumCorners.lay` antes de armar la tabla de la Actividad 9.

### Conclusión

Con una heurística admisible y consistente, A* llega al mismo óptimo que UCS explorando menos de la mitad de los nodos: mejor información, menos búsqueda.
