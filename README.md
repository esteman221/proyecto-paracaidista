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

## Versionamiento

### v0.4.1 — Enunciado del Proyecto

- Se agrego el enunciado del Proyecto.
- Se modifico ligeramente el README principal.

### v0.4 — Respecto al uso de la IA

- Incorporación de USO-DE-LA-IA.md para documentar el apoyo
  de IA en la redacción y elaboración de diagramas SVG.

### v0.3.9 — Diagramas restantes realizados

- Diagramas restantes realizados. 

### v0.3.8 — Diagrama de busqueda de Beta

- Diagrama del proceso de Busqueda de Beta.
- Pendiente: diagramas de lectura, validación y presentación.

### v0.3.7 — Diagrama de refinar impacto

- Diagrama de refinamiento del impacto del paracaidista. 
- Pendiente: diagramas de búsqueda, entrada y salida.

### v0.3.6 — Diagrama de Simulacion de caida

- Diagrama de simulacion de caida.
- Pendiente: demas diagramas.

### v0.3.5 - Diagramas de RK4 y calcular derivada

- Diagramas de `calcular_derivadas` y `paso_rk4`.
- Pendiente: completar los diagramas restantes.

### v0.3.4 — Primer Diagrama

- Diagrama Nassi-Schneiderman de `main`.
- Pendiente: diagramas de las funciones auxiliares.

### v0.3.3 — Buscar_Beta (Disenno)

- Búsqueda automática de beta mediante bisección.
- Definición de tolerancias y límites de cálculo.
- Pendiente: diagramas Nassi-Schneiderman.

### v0.3.2 — Avance de Disenno

- Diseño de la simulación y refinamiento del impacto.
- Definición inicial del paso temporal y límites de cálculo.
- Pendiente: búsqueda de beta y diagramas Nassi-Schneiderman.

### v0.3.1.1 — Correcciones del README
- Agregar Readme

### v0.3.1 Correcciones a la funcion RK4

- Fórmulas y pseudocódigo de RK4 para distancia y velocidad.
- Pendiente: parámetros numéricos, detalle del impacto,
  búsqueda por bisección y diagramas Nassi-Schneiderman.

### v0.3 — Diseño del algoritmo (en desarrollo)

- Definición de las funciones y sus responsabilidades.
- Organización del flujo del programa desde `main`.
- Separación entre simulación de la caída y búsqueda de resistencia.
- Pendiente: detalle de RK4 y diagramas Nassi-Schneiderman.

### v0.2 — Modelo matemático

- Definición de supuestos, variables y unidades.
- Formulación de las ecuaciones de velocidad y distancia.
- Condiciones iniciales y criterio de llegada al suelo.
- Definición de la velocidad objetivo y del caso solicitado.
- Soluciones analíticas para verificar la simulación.
- Distinción entre velocidad límite y velocidad de impacto.

### v0.1.1 - Correccion del README
- Correccion del readme

### v0.1 — Estructura inicial

- Organización de carpetas para C, Pascal y Fortran.
- Directorios para documentación, diagramas y pruebas.
- Configuración de `.gitignore`.
- Descripción inicial del proyecto en el README.