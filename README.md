# Simulación de caída amortiguada

Proyecto de programación estructurada para simular la caída vertical
de un paracaidista mediante el método de Runge-Kutta de cuarto orden.

El mismo algoritmo se implementará en C, Pascal y Fortran.

## Objetivo

Calcular la velocidad y la distancia recorrida durante la caída,
y determinar automáticamente la constante de resistencia del
paracaídas necesaria para alcanzar una velocidad de aterrizaje
aproximada de 8 km/h.

## Entradas

- Masa del paracaidista, en kilogramos.
- Masa del equipo, en kilogramos.
- Altura del salto, en metros.

## Caso de estudio

- Masa del paracaidista: 80 kg.
- Masa del equipo: 50 kg.
- Altura del salto: 1500 m.
- Velocidad de aterrizaje objetivo: 8 km/h.

## Organización

- `src/c/`: implementación en C.
- `src/pascal/`: implementación en Pascal.
- `src/fortran/`: implementación en Fortran.
- `docs/`: modelo matemático y diseño.
- `docs/diagramas/`: diagramas Nassi-Schneiderman.
- `pruebas/`: casos de prueba y comparación de resultados.

## Estado

Estructura inicial del proyecto. Pendiente desarrollar el diseño
del algoritmo y las implementaciones.