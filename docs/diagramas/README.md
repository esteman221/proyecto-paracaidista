# Diagramas Nassi-Schneiderman

Los diagramas representan el diseño común de las versiones
en C, Pascal y Fortran.

## Programa principal

![Diagrama de main](01-main.svg)

`main` establece la configuración, solicita los datos,
coordina su validación y ejecuta la búsqueda de resistencia.

Si la búsqueda tiene éxito, muestra los resultados.
En caso contrario, informa el motivo del error.

El diagrama representa el programa completo previsto.
La primera implementación en C probará inicialmente
la simulación con una resistencia fija.

## Diagramas pendientes

- Lectura y validación de datos.
- Cálculo de derivadas.
- Paso de Runge-Kutta de cuarto orden.
- Simulación de la caída.
- Refinamiento del impacto.
- Búsqueda de beta.
- Presentación de resultados.