## 6. Actividad 6 — Comparación de heurísticas

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/f22eae48-9305-4c2a-bae6-f5a9c1268d31"
    width="700"
    alt="Ejecución y comparación de las heurísticas"
  >
</p>

### Resultados experimentales

| Heurística | Longitud | Costo | Nodos expandidos | Tiempo |
|---|---:|---:|---:|---:|
| `h(n) = 0` | 30 | 30 | 32 | 0.001078 s |
| Manhattan | 30 | 30 | 32 | 0.001551 s |
| Euclidiana | 30 | 30 | 32 | 0.000981 s |

### ¿Cuál heurística representa mejor el movimiento de Pac-Man?

La **distancia Manhattan** representa mejor el movimiento de Pac-Man porque se calcula como:

$$
h_M(n) = |x_n-x_g| + |y_n-y_g|
$$

Esta fórmula suma los desplazamientos horizontales y verticales, que corresponden exactamente a los movimientos permitidos para Pac-Man:

- `North`
- `South`
- `East`
- `West`

Pac-Man nunca puede desplazarse diagonalmente.

### Distancia Euclidiana

La distancia Euclidiana se calcula como:

$$
h_E(n) = \sqrt{(x_n-x_g)^2+(y_n-y_g)^2}
$$

Esta heurística mide la distancia en línea recta entre la posición actual y la meta. Por ello, considera una dirección diagonal que Pac-Man **no puede recorrer directamente**.

En una cuadrícula se cumple que:

$$
h_E(n) \leq h_M(n)
$$

La distancia Euclidiana suele subestimar más el costo real que Manhattan. Aunque ambas son heurísticas admisibles, Manhattan es **más informativa** para este problema porque generalmente proporciona valores más cercanos al costo real sin sobreestimarlo.

### Análisis de los resultados

En `mediumMaze`, las tres configuraciones obtuvieron los mismos resultados principales:

- Longitud del camino: **30**.
- Costo de la solución: **30**.
- Nodos expandidos: **32**.

Esto ocurre porque el laberinto tiene pocas rutas alternativas. Por lo tanto, las heurísticas no tuvieron suficientes oportunidades para evitar exploraciones innecesarias.

Los tiempos obtenidos fueron:

- Heurística nula: `0.001078 s`.
- Manhattan: `0.001551 s`.
- Euclidiana: `0.000981 s`.

Las diferencias son muy pequeñas y pueden deberse a variaciones normales del sistema al medir intervalos del orden de milisegundos. Por esta razón, el menor tiempo de la heurística Euclidiana no demuestra que sea algorítmicamente más eficiente que Manhattan.

### Conclusión

**Manhattan es la heurística más adecuada para Pac-Man**, porque representa directamente sus movimientos horizontales y verticales.

Aunque en este experimento no redujo el número de nodos expandidos, en laberintos con más rutas alternativas y ramificaciones se esperaría que proporcionara una mejor orientación y expandiera menos nodos que la distancia Euclidiana.
