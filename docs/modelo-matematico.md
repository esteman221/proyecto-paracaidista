# Modelo matemático de la caída amortiguada

## 1. Objetivo

Modelar la caída vertical de un paracaidista y determinar
automáticamente la constante de resistencia del paracaídas
necesaria para llegar al suelo a aproximadamente 8 km/h.

Se calcularán la velocidad y la distancia recorrida durante
la caída mediante el método de Runge-Kutta de cuarto orden.

## 2. Supuestos

- El paracaidista comienza desde el reposo.
- El paracaídas está abierto desde el inicio.
- El movimiento es vertical.
- La gravedad es constante.
- La masa total permanece constante.
- La resistencia del aire es proporcional a la velocidad.

Se toma como positiva la dirección hacia abajo.

## 3. Variables y constantes

| Símbolo | Descripción | Unidad |
|---|---|---|
| m_p | Masa del paracaidista | kg |
| m_e | Masa del equipo | kg |
| m | Masa total: m_p + m_e | kg |
| H | Altura inicial del salto | m |
| t | Tiempo desde el inicio del salto | s |
| x(t) | Distancia recorrida hacia abajo | m |
| v(t) | Velocidad hacia abajo | m/s |
| g | Aceleración de gravedad: 9.81 | m/s² |
| β | Constante de resistencia del paracaídas | kg/s |
| R(t) | Magnitud de la resistencia del aire | N |

Aunque el enunciado utiliza la palabra peso para valores en
kilogramos, estos se interpretan como masas. El peso como
fuerza se calcula mediante W = m g y se expresa en newtons.

La distancia recorrida x(t) comienza en cero.
La altura restante se obtiene como H - x(t).

## 4. Fuerzas que actúan

El peso actúa hacia abajo:

$$
W = mg
$$

La resistencia del aire actúa hacia arriba y su magnitud es:

$$
R = \beta v
$$

Por lo tanto, la fuerza neta hacia abajo es:

$$
F = mg - \beta v
$$

## 5. Ecuaciones diferenciales

Aplicando la segunda ley de Newton:

$$
m\frac{dv}{dt} = mg - \beta v
$$

Despejando la derivada de la velocidad:

$$
\frac{dv}{dt} = g - \frac{\beta}{m}v
$$

La velocidad también representa la tasa de cambio
de la distancia recorrida:

$$
\frac{dx}{dt} = v
$$

Estas dos ecuaciones forman el sistema que resolverá
el programa mediante Runge-Kutta de cuarto orden.

## 6. Condiciones iniciales y llegada al suelo

Al comenzar el salto:

$$
t = 0,\qquad x(0) = 0,\qquad v(0) = 0
$$

El impacto ocurre en el instante T en que:

$$
x(T) = H
$$

La velocidad de aterrizaje es v(T).

Si un paso numérico supera la altura H, se deberá
aproximar el instante de impacto dentro de ese intervalo.

## 7. Velocidad objetivo

La velocidad deseada al llegar al suelo es:

$$
v_{\text{objetivo}} = \frac{8}{3.6}
= \frac{20}{9}\ \text{m/s}
$$

El programa buscará un valor de β no negativo que cumpla:

$$
v(T) \approx v_{\text{objetivo}}
$$

Para ello, se simulará la caída con diferentes valores
de β hasta alcanzar una tolerancia definida en el
diseño del algoritmo.

## 8. Caso solicitado

| Dato | Valor |
|---|---:|
| Masa del paracaidista | 80 kg |
| Masa del equipo | 50 kg |
| Masa total | 130 kg |
| Altura del salto | 1500 m |
| Velocidad objetivo | 8 km/h |

El valor de β deberá ser encontrado por el algoritmo;
no será un dato ingresado por el usuario ni un resultado
fijo escrito en el código.

## 9. Solución analítica para comprobar resultados

Para β > 0, la velocidad exacta del modelo es:

$$
v(t)=\frac{mg}{\beta}
\left(1-e^{-\beta t/m}\right)
$$

La distancia recorrida es:

$$
x(t)=\frac{mg}{\beta}t
-\frac{m^2g}{\beta^2}
\left(1-e^{-\beta t/m}\right)
$$

Para β = 0, el movimiento corresponde a caída libre:

$$
v(t)=gt
$$

$$
x(t)=\frac{1}{2}gt^2
$$

Estas expresiones servirán para comprobar los resultados
numéricos. La simulación se implementará mediante RK4.

## 10. Velocidad límite y viabilidad

Para β > 0, la velocidad tiende a:

$$
v_{\text{límite}}=\frac{mg}{\beta}
$$

La velocidad límite no debe confundirse con la velocidad
de impacto: el paracaidista podría alcanzar el suelo
antes de aproximarse a ella.

Para una altura H, la máxima velocidad de impacto con
β no negativo corresponde a la caída libre:

$$
v_{\text{máxima}}=\sqrt{2gH}
$$

Si la velocidad objetivo supera ese valor, no existe
una solución con resistencia no negativa bajo este modelo.