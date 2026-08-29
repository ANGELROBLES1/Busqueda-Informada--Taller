## 7. Actividad 7 — Diseñando el estado

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/e2802c12-dc1a-49ff-985c-8bc0de92310f"
    width="720"
    alt="Implementación del estado para CornersProblem"
  >
</p>

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/9187de3e-d8b1-4123-a572-c446d0a9a35e"
    width="450"
    alt="Ejecución del laberinto tinyCorners"
  >
</p>

La imagen corresponde al estado inicial de la ejecución de CornersProblem sobre el laberinto tinyCorners, antes de iniciar el recorrido calculado por UCS. Se observan cuatro puntos de comida ubicados exactamente en las cuatro esquinas del laberinto (definidas en self.corners), que constituyen el objetivo del problema: Pac-Man debe visitarlas todas para alcanzar el estado meta (isGoalState). El marcador SCORE: 0 confirma que la partida apenas está comenzando, antes de que se ejecuten las 22 acciones del camino óptimo encontrado por el algoritmo

### Configuración de la prueba

**Laberinto:** `tinyCorners`

**Comando ejecutado:**

```bash
python pacman.py -l tinyCorners -p SearchAgent -a fn=ucs,prob=CornersProblem
```

### Resultados

| Métrica | Resultado |
|---|---:|
| Costo del camino | 22 |
| Nodos expandidos | 296 |
| Tiempo | 0.000805 s |
| Resultado del juego | Victoria — *Score* 518 |

### Representación del estado

```python
s = (posición, esquinas_visitadas)
```

El estado está compuesto por dos elementos:

- **`posición`:** tupla `(x, y)` que representa la ubicación actual de Pac-Man.
- **`esquinas_visitadas`:** tupla de cuatro valores booleanos, uno por cada esquina definida en `self.corners`. Cada valor indica si la esquina correspondiente ya fue visitada.

Esta representación soluciona el problema planteado en la guía. Dos estados con la misma posición, por ejemplo `(5, 4)`, pueden representar situaciones diferentes dependiendo de las esquinas visitadas previamente.

De esta manera, UCS y A* no confunden como un mismo nodo dos estados que comparten la misma posición, pero tienen un progreso diferente.

### Funciones implementadas

#### `getStartState`

Devuelve la posición inicial de Pac-Man y una tupla que indica que ninguna esquina ha sido visitada:

```python
(posición_inicial, (False, False, False, False))
```

#### `isGoalState`

Determina si el estado actual es una meta. El objetivo se cumple cuando las cuatro esquinas han sido visitadas:

```python
all(esquinas_visitadas)
```

Cuando esta expresión devuelve `True`, Pac-Man ha visitado todas las esquinas.

#### `getSuccessors`

Por cada movimiento válido, la función:

1. Calcula la nueva posición de Pac-Man.
2. Conserva el registro de las esquinas visitadas.
3. Comprueba si la nueva posición coincide con una esquina.
4. Marca esa esquina como visitada.
5. Genera el nuevo estado sin modificar las demás posiciones de la tupla.

### Conclusión

Para resolver `CornersProblem`, no es suficiente representar el estado solamente mediante la posición de Pac-Man. También es necesario almacenar las esquinas que ya fueron visitadas.

La representación:

```python
s = (posición, esquinas_visitadas)
```

permite que el algoritmo diferencie correctamente los estados y encuentre una ruta que visite las cuatro esquinas del laberinto.
