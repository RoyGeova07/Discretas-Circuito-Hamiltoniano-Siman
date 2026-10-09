# Matematicas Discretas - Circuitos Hamiltonianos — Farmacias Simán SPS

Resumen de qué hace `farmacias_siman_hamiltoniano.ipynb` con el archivo
`Datos_SucursalesSimán.xlsx`.

## El problema de la empresa

**NetGuard Solutions** brinda servicio de monitoreo y mantenimiento de
sistemas IoT (sensores de temperatura, cámaras, contadores de aforo y
controladores de refrigeración) instalados en las sucursales de Farmacias
Simán en San Pedro Sula.

La empresa necesita que sus técnicos revisen periódicamente cada sucursal
**sin repetir ninguna** y **regresando al punto de partida**, para no
desperdiciar tiempo ni combustible. Además, la gerencia evalúa abrir una
**sede técnica** desde la cual despachar a los técnicos, y quiere elegir su
ubicación con base en datos reales de conectividad — no por intuición.

En concreto, hacía falta responder:

1. ¿Cómo dividir la red completa de sucursales en bloques manejables?
2. ¿Existe una ruta (circuito Hamiltoniano) que visite cada sucursal de un
   bloque exactamente una vez y regrese al inicio? ¿Cuánto mide esa ruta?
3. De las 3 ubicaciones candidatas para la sede técnica, ¿cuál conviene más
   según la distancia real que tendría que recorrer un técnico saliendo de
   ahí?

## Cómo se resolvió

Se registraron en campo (Google Maps) las sucursales, se agruparon en
**5 bloques** geográficos, y para cada uno de los **3 candidatos de sede**
(A, B y C) se trazó a mano el recorrido que sale del candidato, visita cada
sucursal del bloque una vez, y vuelve — con su distancia de ida y de vuelta
en km. Todo esto quedó registrado en la hoja `Candidatos Bloques` del Excel.

<img width="573" height="459" alt="image" src="https://github.com/user-attachments/assets/16c0b405-7c4a-4d52-834e-ba7d14f3ab65" />


El notebook toma esos datos y:

- **Verifica matemáticamente** que cada uno de esos 15 recorridos
  (3 candidatos × 5 bloques) sea en efecto un circuito Hamiltoniano válido
  (aplica los teoremas de Dirac y Ore, y lo confirma por backtracking).
- **Calcula la distancia total** de cada circuito.
- **Compara los 3 candidatos** sumando la distancia de sus 5 circuitos junto
  con el costo y área de alquiler de cada sede, y **recomienda cuál conviene
  más** — respondiendo así la pregunta de negocio original.

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
