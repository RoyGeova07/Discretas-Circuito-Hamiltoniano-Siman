# Matematicas Discretas - Circuitos Hamiltonianos — Farmacias Simán SPS

Resumen de qué hace `farmacias_siman_hamiltoniano.ipynb` con el archivo
`Datos_SucursalesSimán.xlsx`.

## ¿Qué problema resuelve?

Para cada una de las **3 ubicaciones candidatas** (A, B y C) donde NetGuard
Solutions podría instalar su sede técnica, el notebook verifica y mide el
recorrido que tendría que hacer un técnico para revisar, en **cada uno de los
5 bloques**, todas las sucursales de ese bloque una sola vez y regresar al
punto de partida (el candidato). En total evalúa **15 circuitos**
(3 candidatos × 5 bloques).

## Fuente de datos: hoja "Candidatos Bloques"

El notebook lee **una sola hoja** del Excel: `Candidatos Bloques`. Esa hoja
ya trae, para cada combinación Candidato + Bloque, la secuencia ordenada de
tramos del recorrido (columna "Recorrido", con el formato
`Origen → Destino`) junto con su **Distancia Ida (km)** y **Distancia Vuelta
(km)**. El resto de hojas del libro (`Lista de Sucursales`,
`Bloques de Sucursales`, `Candidatos`, `Distancias de Farmacias Vecinas`,
`Distancias de Candidatos`) ya no se usan en este notebook.

## Qué hace el código, paso a paso

1. **Carga los recorridos** — Recorre la hoja `Candidatos Bloques` y arma,
   por candidato y por bloque, la lista de tramos con sus distancias.
   Normaliza los nombres de sucursal (ignora tildes) para que una misma
   sucursal escrita de dos formas no se trate como dos nodos distintos.
2. **Construye el subgrafo de cada circuito** — Por cada combinación
   Candidato + Bloque arma un grafo con esos tramos (el candidato incluido
   como punto de partida/llegada).
3. **Verifica Dirac y Ore** — Aplica las dos condiciones matemáticas
   clásicas de existencia de circuitos Hamiltonianos sobre cada subgrafo
   (a modo de análisis teórico; no son necesarias para que el circuito
   exista, solo suficientes).
4. **Busca el circuito por backtracking** — Confirma, probando
   combinaciones, que existe una ruta que visita cada sucursal del bloque
   exactamente una vez y regresa al candidato.
5. **Calcula la distancia del circuito** — Suma la distancia de ida, de
   vuelta y el promedio de cada uno de los 15 circuitos.
6. **Compara los 3 candidatos** — Suma la distancia total (los 5 bloques)
   de cada candidato y, junto con el precio y área de alquiler de cada uno,
   **recomienda cuál conviene más** por ser el que exige recorrer menos
   kilómetros en total.

## Qué resultado final entrega

- Una tabla con los 15 circuitos: si cumplen Dirac, si cumplen Ore, si el
  circuito Hamiltoniano es válido y su distancia promedio.
- Para cada candidato: el detalle de sus 5 circuitos (ordenados de más
  corto a más largo) y la suma total de kilómetros.
- Una recomendación final de cuál candidato conviene, combinando la
  distancia total recorrida con el costo/área de alquiler de cada sede.

## Cómo correrlo

1. Coloca `Datos_SucursalesSimán.xlsx` en la **misma carpeta** que el
   notebook.
2. Abre `farmacias_siman_hamiltoniano.ipynb`.
3. Ejecuta todas las celdas en orden (Restart & Run All).
