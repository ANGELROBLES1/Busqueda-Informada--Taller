## 4. Actividad 4 — A* sin información

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/ae28fa6f-8fd0-45b4-8932-2541d18d79de"
    width="620"
    alt="Ejecución de A* sin información heurística"
  >
</p>

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/59f323de-790b-49c8-9f43-93f673e376a0"
    width="700"
    alt="Resultado de A* con heurística nula"
  >
</p>

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/9392210b-e43d-49ac-9d67-83141fb49242"
    width="750"
    alt="Comparación entre UCS y A* con heurística nula"
  >
</p>

### Resultados

| Algoritmo | Costo | Nodos expandidos | Tiempo |
|---|---:|---:|---:|
| UCS | 30 | 32 | 0.001290 s |
| A* con `h(n) = 0` | 30 | 32 | 0.001078 s |

### ¿Qué comportamiento se observa?

A* con `h(n) = 0` presenta un comportamiento equivalente o muy similar a la búsqueda de costo uniforme.

Cuando la heurística es nula, la fórmula de A* se reduce a:

$$
f(n) = g(n) + h(n) = g(n) + 0 = g(n)
$$

La prioridad utilizada por A* para ordenar la frontera de búsqueda queda determinada únicamente por el costo acumulado `g(n)`, exactamente igual que en UCS.

Por esta razón, ambos algoritmos expanden los nodos en el mismo orden y llegan al mismo resultado: un costo óptimo de **30** y **32 nodos expandidos** en ambos casos.

Las pequeñas diferencias en el tiempo de ejecución se deben a variaciones normales del sistema al medir intervalos tan cortos y no representan una diferencia algorítmica importante.

### Conclusión

**UCS es un caso particular de A* cuando `h(n) = 0`**, porque ambos algoritmos utilizan solamente el costo acumulado `g(n)` para decidir qué nodo expandir.
