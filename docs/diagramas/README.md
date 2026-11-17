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

## Cálculo de derivadas

![Diagrama de calcular_derivadas](02-calcular-derivadas.svg)

Evalúa las tasas de cambio de distancia y velocidad
para una velocidad, masa y resistencia dadas.
No modifica el estado ni avanza el tiempo.

## Paso de Runge-Kutta de cuarto orden

![Diagrama de paso_rk4](03-paso-rk4.svg)

Calcula cuatro evaluaciones de las derivadas y las combina
para obtener la distancia y velocidad siguientes.

Los valores originales se conservan durante todas las
evaluaciones. Los valores k representan pendientes
y no incluyen el factor h.

## Simulación de la caída

![Diagrama de simular_caida](04-simular-caida.svg)

Inicializa el estado, selecciona el paso temporal y repite
RK4 hasta detectar el suelo, un error o el límite de pasos.

Cuando un paso alcanza o supera la altura, utiliza
`refinar_impacto` desde el último estado anterior al suelo.

El resultado solo representa una llegada válida cuando
el indicador de éxito es verdadero.

## Diagramas pendientes

- Lectura y validación de datos.
- Refinamiento del impacto.
- Búsqueda de beta.
- Presentación de resultados.