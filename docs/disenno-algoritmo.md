# Diseño del algoritmo

## 1. Objetivo

Diseñar un algoritmo común para las implementaciones en C,
Pascal y Fortran.

El algoritmo simulará la caída mediante Runge-Kutta de
cuarto orden y buscará automáticamente la constante de
resistencia necesaria para alcanzar una velocidad de
aterrizaje aproximada de 8 km/h.

## 2. Entradas del usuario

- Masa del paracaidista, en kg.
- Masa del equipo, en kg.
- Altura del salto, en m.

Las entradas deben ser numéricas y finitas.

La masa del paracaidista y la altura deben ser positivas.
La masa del equipo puede ser cero, pero no negativa.

## 3. Resultados

- Constante de resistencia beta, en kg/s.
- Tiempo de llegada al suelo, en s.
- Velocidad de llegada, en m/s y km/h.

Si no se encuentra una solución, se debe informar
la causa en lugar de mostrar un resultado exitoso.

## 4. Datos internos

### Estado de la caída

- Tiempo transcurrido.
- Distancia recorrida hacia abajo.
- Velocidad hacia abajo.

### Parámetros físicos

- Masa total: masa de la persona + masa del equipo.
- Altura inicial.
- Constante de resistencia beta.
- Gravedad: 9.81 m/s².
- Velocidad objetivo: 8 / 3.6 m/s.

### Parámetros numéricos

- Paso temporal.
- Tolerancia de velocidad.
- Criterio de precisión para localizar el impacto.
- Límites de pasos e iteraciones.

Los valores y criterios numéricos se definirán al
detallar RK4, la detección del impacto y la búsqueda.

## 5. Funciones y responsabilidades

| Función | Entradas | Salidas |
|---|---|---|
| leer_datos | Entrada por consola | Masas, altura e indicador de lectura válida |
| validar_datos | Masas y altura | Indicador de validez y causa del error |
| calcular_derivadas | Velocidad, masa total y beta | Derivadas de distancia y velocidad |
| paso_rk4 | Distancia, velocidad, masa, beta y paso temporal | Distancia y velocidad siguientes |
| simular_caida | Masa total, altura, beta y configuración numérica | Tiempo y velocidad de impacto, indicador de éxito |
| buscar_beta | Masa total, altura, velocidad objetivo y configuración numérica | Beta encontrado, resultado de la caída e indicador de éxito |
| mostrar_resultados | Beta y resultado de la caída | Información en consola |
| main | Ninguna | Coordinación del programa |

La gravedad será una constante común del modelo.

## 6. Flujo principal

1. Leer los datos.
2. Comprobar que la lectura y los valores sean válidos.
3. Calcular la masa total.
4. Buscar la constante de resistencia.
5. Si la búsqueda tiene éxito, mostrar los resultados.
6. Si ocurre un error, informar su causa.

## 7. Evaluación de las derivadas

Se utilizarán las expresiones:

- Derivada de la distancia: v.
- Derivada de la velocidad: g - (beta / m) * v.

Esta operación evalúa tasas de cambio; no modifica
el estado ni avanza el tiempo.

## 8. Avance mediante RK4

A partir de la distancia y velocidad actuales:

1. Evaluar las derivadas al inicio del intervalo.
2. Evaluarlas en un primer estado intermedio.
3. Evaluarlas en un segundo estado intermedio.
4. Evaluarlas en un estado estimado al final.
5. Combinar las cuatro evaluaciones para obtener
   la distancia y velocidad siguientes.

Las dos variables se actualizarán conjuntamente.

## 9. Simulación de una caída

1. Inicializar tiempo, distancia y velocidad en cero.
2. Mantener beta fijo durante toda la simulación.
3. Avanzar mediante pasos RK4.
4. Detectar cuándo se alcanza o supera la altura.
5. Si se supera, refinar el último intervalo para
   aproximar el instante de llegada al suelo.
6. Devolver el tiempo y la velocidad de impacto.

La simulación tendrá un límite de pasos e informará
si no puede completarse.

## 10. Búsqueda de beta

Se propone utilizar bisección:

1. Verificar si la velocidad objetivo es alcanzable
   desde la altura indicada con beta no negativo.
2. Encontrar un intervalo de valores de beta que
   encierre la solución.
3. Simular con el punto medio del intervalo.
4. Comparar la velocidad de llegada con el objetivo.
5. Si llega demasiado rápido, aumentar el límite
   inferior de beta.
6. Si llega demasiado lento, disminuir el límite
   superior de beta.
7. Repetir hasta cumplir la tolerancia o alcanzar
   el límite de iteraciones.

Cada simulación comienza desde el reposo.

## 11. Organización estructurada

- Cada función tendrá una responsabilidad definida.
- Los datos se comunicarán mediante parámetros y resultados.
- Se evitarán variables globales modificables.
- Las funciones matemáticas no leerán ni imprimirán datos.
- Los tres lenguajes conservarán el mismo diseño lógico.