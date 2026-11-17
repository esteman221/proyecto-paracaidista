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

La función `paso_rk4` recibe la distancia y velocidad actuales,
la masa total, beta y el paso temporal h. Devuelve la distancia
y velocidad al finalizar el intervalo.

Se utiliza la convención en que los valores k representan
pendientes y no incluyen el factor h.

### Evaluaciones

Primera evaluación:

$$
k_{1x}=v,\qquad
k_{1v}=g-\frac{\beta}{m}v
$$

Segunda evaluación:

$$
v_2=v+\frac{h}{2}k_{1v}
$$

$$
k_{2x}=v_2,\qquad
k_{2v}=g-\frac{\beta}{m}v_2
$$

Tercera evaluación:

$$
v_3=v+\frac{h}{2}k_{2v}
$$

$$
k_{3x}=v_3,\qquad
k_{3v}=g-\frac{\beta}{m}v_3
$$

Cuarta evaluación:

$$
v_4=v+hk_{3v}
$$

$$
k_{4x}=v_4,\qquad
k_{4v}=g-\frac{\beta}{m}v_4
$$

### Actualización del estado

$$
x_{\text{nuevo}}
=x+\frac{h}{6}(k_{1x}+2k_{2x}+2k_{3x}+k_{4x})
$$

$$
v_{\text{nuevo}}
=v+\frac{h}{6}(k_{1v}+2k_{2v}+2k_{3v}+k_{4v})
$$

Las derivadas de este modelo no dependen explícitamente
de la posición ni del tiempo. Por eso, para evaluar las
pendientes solo se necesitan las velocidades intermedias.

La distancia y velocidad originales se conservan durante
las cuatro evaluaciones. Los valores nuevos se calculan
al finalizar el paso.

### Pseudocódigo

```text
FUNCION paso_rk4(x, v, masa, beta, h)

    (k1x, k1v) ← calcular_derivadas(v, masa, beta)

    v2 ← v + (h / 2) * k1v
    (k2x, k2v) ← calcular_derivadas(v2, masa, beta)

    v3 ← v + (h / 2) * k2v
    (k3x, k3v) ← calcular_derivadas(v3, masa, beta)

    v4 ← v + h * k3v
    (k4x, k4v) ← calcular_derivadas(v4, masa, beta)

    x_nuevo ← x + (h / 6) * (k1x + 2*k2x + 2*k3x + k4x)
    v_nuevo ← v + (h / 6) * (k1v + 2*k2v + 2*k3v + k4v)

    DEVOLVER (x_nuevo, v_nuevo)

FIN FUNCION
```

La función `simular_caida` será responsable de repetir
los pasos y actualizar el tiempo.

El tamaño del paso deberá comprobarse mediante pruebas
de precisión y estabilidad; no cualquier valor positivo
de h produce una aproximación adecuada.

## 9. Simulación de una caída

### 9.1. Responsabilidad

La función `simular_caida` calcula el tiempo y la velocidad
de llegada al suelo para una masa total, altura y beta dados.

Beta permanece fijo durante toda la simulación.

La función utiliza `paso_rk4` para avanzar y `refinar_impacto`
para localizar la llegada dentro del último intervalo.

### 9.2. Parámetros numéricos iniciales

| Parámetro | Valor propuesto | Propósito |
|---|---:|---|
| Paso máximo | 0.02 s | Limitar el avance temporal |
| Factor de escala temporal | 0.2 | Reducir el paso cuando la resistencia es elevada |
| Tolerancia temporal del impacto | 1e-8 s | Finalizar el refinamiento |
| Máximo de pasos | 10000000 | Limitar el trabajo de una simulación |
| Máximo de refinamientos | 80 | Limitar la búsqueda del impacto |

Estos valores son decisiones de implementación y deberán
validarse mediante pruebas.

La tolerancia temporal controla el ancho del intervalo
numérico del impacto. No garantiza por sí sola que el
error total de la simulación sea de ese tamaño.

### 9.3. Selección del paso

Para beta positivo:

$$
h=\min\left(h_{\max},\,0.2\frac{m}{\beta}\right)
$$

Para beta igual a cero:

$$
h=h_{\max}
$$

El paso se mantiene fijo dentro de cada simulación,
excepto al localizar el impacto.

### 9.4. Condiciones iniciales

- Tiempo: 0.
- Distancia recorrida: 0.
- Velocidad: 0.

Las entradas deben ser finitas, con masa y altura positivas,
beta no negativo y configuración numérica válida.

### 9.5. Detección del impacto

Si la distancia obtenida después de un paso es menor
que la altura, se acepta el nuevo estado.

Si alcanza o supera la altura, se conserva el estado
anterior y se refina el intervalo temporal del impacto.

No se sustituye simplemente la distancia calculada por
la altura, porque el tiempo y la velocidad también deben
corresponder al instante de llegada.

### 9.6. Pseudocódigo de simulación

```text
FUNCION simular_caida(masa, altura, beta, configuracion)

    t ← 0
    x ← 0
    v ← 0
    pasos ← 0

    terminado ← FALSO
    exito ← FALSO
    motivo ← "Limite de pasos alcanzado"

    h ← configuracion.paso_maximo

    SI beta > 0 ENTONCES
        h ← MINIMO(h, 0.2 * masa / beta)
    FIN SI

    SI h no es finito O h <= 0 ENTONCES
        terminado ← VERDADERO
        motivo ← "Paso temporal invalido"
    FIN SI

    MIENTRAS NO terminado Y pasos < configuracion.max_pasos

        (x_nuevo, v_nuevo) ← paso_rk4(x, v, masa, beta, h)
        pasos ← pasos + 1

        SI x_nuevo o v_nuevo no son finitos ENTONCES

            terminado ← VERDADERO
            motivo ← "Resultado numerico no finito"

        SINO

            SI x_nuevo >= altura ENTONCES

                impacto ← refinar_impacto(
                    x, v, masa, beta, altura, h,
                    configuracion.tolerancia_tiempo,
                    configuracion.max_refinamientos
                )

                terminado ← VERDADERO
                exito ← impacto.exito
                motivo ← impacto.motivo

                SI exito ENTONCES
                    t ← t + impacto.dt
                    x ← impacto.distancia
                    v ← impacto.velocidad
                FIN SI

            SINO

                SI t + h <= t ENTONCES
                    terminado ← VERDADERO
                    motivo ← "El tiempo no puede avanzar"
                SINO
                    x ← x_nuevo
                    v ← v_nuevo
                    t ← t + h
                FIN SI

            FIN SI

        FIN SI

    FIN MIENTRAS

    DEVOLVER (exito, motivo, t, x, v)

FIN FUNCION
```

Los valores de tiempo, distancia y velocidad solo representan
un impacto válido cuando el indicador de éxito es verdadero.

### 9.7. Refinamiento del impacto

Precondiciones:

- La distancia inicial x es menor que la altura.
- Un paso RK4 de duración h alcanza o supera la altura.
- La tolerancia temporal es positiva.
- El máximo de refinamientos es un entero positivo.

Se busca un tiempo adicional dt entre 0 y h mediante
bisección. Todas las evaluaciones parten del mismo estado
anterior al impacto.

```text
FUNCION refinar_impacto(
    x, v, masa, beta, altura, h,
    tolerancia_tiempo, max_refinamientos
)

    a ← 0
    b ← h
    iteraciones ← 0
    fallo ← FALSO

    dt ← 0
    x_impacto ← x
    v_impacto ← v
    motivo ← "Refinamiento incompleto"

    MIENTRAS (b - a) > tolerancia_tiempo
             Y iteraciones < max_refinamientos
             Y NO fallo

        medio ← a + (b - a) / 2

        SI medio = a O medio = b ENTONCES

            fallo ← VERDADERO
            motivo ← "No se puede reducir mas el intervalo"

        SINO

            (x_prueba, v_prueba) ← paso_rk4(
                x, v, masa, beta, medio
            )

            SI x_prueba o v_prueba no son finitos ENTONCES

                fallo ← VERDADERO
                motivo ← "Resultado numerico no finito"

            SINO

                SI x_prueba < altura ENTONCES
                    a ← medio
                SINO
                    b ← medio
                FIN SI

            FIN SI

        FIN SI

        iteraciones ← iteraciones + 1

    FIN MIENTRAS

    exito ← NO fallo Y (b - a) <= tolerancia_tiempo

    SI exito ENTONCES

        dt ← a + (b - a) / 2

        (x_impacto, v_impacto) ← paso_rk4(
            x, v, masa, beta, dt
        )

        SI x_impacto o v_impacto no son finitos ENTONCES
            exito ← FALSO
            motivo ← "Resultado numerico no finito"
        SINO
            motivo ← "Impacto localizado"
        FIN SI

    FIN SI

    DEVOLVER (exito, motivo, dt, x_impacto, v_impacto)

FIN FUNCION
```

La distancia final será una aproximación a la altura.
La precisión completa también depende del error acumulado
por los pasos RK4 anteriores.

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