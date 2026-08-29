## 5. Actividad 5 — A* con Manhattan

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/e76838da-58c9-4f1a-a15c-f4344a66fbac"
    width="620"
    alt="Ejecución de A* con la heurística Manhattan"
  >
</p>

### Resultados

| Algoritmo | Costo | Nodos expandidos | Tiempo |
|---|---:|---:|---:|
| UCS | 30 | 32 | 0.001415 s |
| A* + Manhattan | 30 | 32 | 0.001551 s |

### ¿Por qué la distancia Manhattan puede orientar correctamente a Pac-Man aunque no considere las paredes?

La distancia Manhattan se calcula como:

$$
h_M(n) = |x_n-x_g| + |y_n-y_g|
$$

Esta fórmula mide la cantidad mínima de pasos horizontales y verticales que separan la posición actual de la meta, **sin considerar si existen paredes en medio**.

A pesar de esta limitación, Manhattan es una heurística **admisible**, porque nunca sobreestima el costo real. Al considerar los obstáculos, el camino verdadero siempre necesitará una cantidad de pasos igual o mayor que la distancia Manhattan, pero nunca menor.

Esta propiedad permite que A* continúe garantizando una solución óptima, aunque la heurística no conozca la ubicación de las paredes.

### Orientación de la búsqueda

Aunque ignore los obstáculos, Manhattan puede orientar la dirección general de la búsqueda. A* prioriza los nodos que parecen estar más cerca de la meta y evita explorar primero direcciones que se alejan claramente de ella.

Cuando una pared bloquea el recorrido directo, Pac-Man debe rodearla. Sin embargo, la heurística sigue proporcionando una estimación útil de la dirección en la que se encuentra la meta.

### Interpretación de los resultados

En `mediumMaze`, UCS y A* con Manhattan obtuvieron:

- Un costo óptimo de **30**.
- Un total de **32 nodos expandidos**.
- Tiempos de ejecución muy cercanos.

Manhattan no redujo el número de nodos expandidos porque `mediumMaze` tiene una estructura similar a un corredor y presenta pocas rutas alternativas. En este tipo de laberinto, la heurística tiene pocas oportunidades para evitar exploraciones innecesarias.

A* con Manhattan tardó `0.001551 s`, mientras que UCS tardó `0.001415 s`. Esta pequeña diferencia puede deberse tanto al cálculo adicional de la heurística como a variaciones normales del sistema al medir tiempos del orden de milisegundos.

### Ventajas de Manhattan

La heurística Manhattan:

- Mantiene la optimalidad de la solución.
- Orienta la búsqueda hacia la meta.
- Representa los movimientos horizontales y verticales de Pac-Man.
- Puede reducir los nodos expandidos en laberintos con más ramificaciones o rutas alternativas.

### Conclusión

Aunque en `mediumMaze` Manhattan no redujo los nodos expandidos frente a UCS, sigue siendo una heurística adecuada porque orienta la búsqueda sin sobreestimar el costo real. Su ventaja puede observarse con mayor claridad en laberintos que tengan más rutas alternativas y ramificaciones.
