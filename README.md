[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=zulematirado/MSFP4)

# Práctica 4: Modelado y control del sistema endócrino

## Información de la estudiante

Tirado Manzo Zulema Guadalupe [23210724]; L23210724@tectijuana.edu.mx

Modelado de Sistemas Fisiológicos

Ingeniería Biomédica

## Docente

Dr. Paul Antonio Valle Trujillo; paul.valle@tectijuana.edu.mx

Departamento de Ingeniería Eléctrica y Electrónica, Tecnológico Nacional de México/IT Tijuana, Blvd. Alberto Limón Padilla s/n, Tijuana, C.P. 22454, B.C., México.

## Descripción de la asignatura

El modelado de sistemas fisiológicos permite representar mediante ecuaciones y simulaciones el comportamiento de distintos procesos del cuerpo humano. En Ingeniería Biomédica se utilizan analogías físicas y eléctricas para estudiar la dinámica de estos procesos y analizar su respuesta ante diferentes estímulos.

En esta práctica se emplean las leyes de Kirchhoff, ecuaciones diferenciales y funciones de transferencia para describir un modelo simplificado del sistema endócrino. Se estudian una condición de referencia, llamada control, y una condición alterada, llamada caso. La guía corresponde al apartado 5.6 del programa de la asignatura.

El desarrollo comprende el análisis matemático, la simulación en MATLAB, Simulink, Simscape Electrical y Python, y el diseño de un controlador PID. También se contempla la construcción del circuito y del controlador analógico para contrastar las simulaciones con mediciones de laboratorio.

## Objetivos

1. Interpretar los procesos de secreción, transporte, almacenamiento y respuesta hormonal mediante una analogía eléctrica.
2. Identificar la entrada, la salida, las corrientes, los nodos y la referencia del circuito equivalente.
3. Aplicar las leyes de Kirchhoff para obtener las ecuaciones integro-diferenciales del sistema.
4. Obtener las funciones de transferencia del control y del caso mediante la transformada de Laplace.
5. Determinar el valor final y el error estacionario ante una entrada escalón unitario.
6. Calcular los polos, establecer la estabilidad y analizar el efecto de las modificaciones de C y R2.
7. Implementar las ecuaciones y las funciones de transferencia en MATLAB/Simulink y verificar el circuito.
8. Sintonizar un controlador PID que permita al caso seguir la respuesta del control.
9. Reproducir las respuestas en Python y comparar las diferentes representaciones.

## Descripción detallada del sistema

El sistema endócrino regula funciones del organismo mediante hormonas liberadas por glándulas y transportadas hasta órganos o tejidos diana. Su efecto depende de procesos de producción, transporte, acumulación, utilización y eliminación.

El circuito de esta práctica representa de forma simplificada algunas de esas propiedades. No describe un eje hormonal específico ni establece una equivalencia directa entre voltajes y concentraciones hormonales reales. Su propósito es estudiar almacenamiento, disipación e inercia mediante un sistema dinámico de segundo orden.

### Diagrama biológico en BioRender


**Figura 1.** Analogía entre el estímulo para la secreción hormonal, su transporte, el almacenamiento temporal y la respuesta del tejido diana. El diagrama deberá relacionar estos procesos con Ve(t), Fe(t), R1, C, L, R2, Fs(t) y Vs(t).

### Analogía entre el sistema fisiológico y el eléctrico

| Proceso o variable fisiológica | Elemento eléctrico | Interpretación |
| --- | --- | --- |
| Estímulo para la secreción hormonal | Ve(t) | Señal de entrada |
| Secreción y transporte hormonal | Fe(t) | Corriente que atraviesa R1 |
| Pérdidas y oposición durante el transporte | R1 | Resistencia de entrada |
| Acumulación temporal de la señal hormonal | C | Almacenamiento de energía |
| Inercia de la respuesta del tejido diana | L | Oposición a cambios rápidos de corriente |
| Utilización o eliminación de la señal | R2 | Disipación en la rama de respuesta |
| Flujo asociado con la respuesta del órgano diana | Fs(t) | Corriente por L y R2 |
| Nivel hormonal efectivo normalizado | Vs(t) | Voltaje del nodo de salida respecto a tierra |

### Construcción del circuito equivalente

La fuente Ve(t) alimenta al resistor R1. Después de R1 se encuentra el nodo N2, donde se mide Vs(t). Desde este nodo parten dos ramas hacia tierra: una contiene el capacitor C y la otra contiene el inductor L y el resistor R2 conectados en serie.

| Nodo o rama | Conexión |
| --- | --- |
| N1 | Terminal positivo de Ve(t) y entrada de R1 |
| N2 | Unión de R1, C y L; nodo donde se mide Vs(t) |
| N3 | Unión entre L y R2 |
| Tierra | Terminal negativo de la fuente, terminal inferior de C y terminal inferior de R2 |
| Rama de entrada | Ve(t) y R1, con corriente Fe(t) hacia N2 |
| Rama capacitiva | C entre N2 y tierra, con corriente Fc(t) |
| Rama de respuesta | L y R2 en serie, con corriente Fs(t) hacia tierra |

La salida Vs(t) se mide sobre el capacitor y, por tanto, sobre toda la rama L–R2. No corresponde únicamente al voltaje sobre R2. En las hojas del análisis aparece la igualdad Vs = R2 I2 en algunas líneas; para conservar la salida definida por la guía, esa igualdad se sustituye por Vs = L dI2/dt + R2 I2. La función de transferencia final del manuscrito sí corresponde a la salida del nodo N2.

### Parámetros del circuito equivalente

| Parámetro | Control | Caso |
| --- | ---: | ---: |
| R1 | 1 kΩ | 1 kΩ |
| L | 100 mH | 100 mH |
| C | 1 µF | 1000 µF |
| R2 | 100 kΩ | 1 kΩ |
| Entrada | Escalón unitario | Escalón unitario |
| Intervalo de simulación | 0 a 10 s | 0 a 10 s |

Para los cálculos se utilizan unidades del Sistema Internacional:

$$
R_1=1000\ \Omega,\qquad L=0.1\ \mathrm{H}.
$$

$$
C_{control}=10^{-6}\ \mathrm{F},\qquad R_{2,control}=100000\ \Omega.
$$

$$
C_{caso}=10^{-3}\ \mathrm{F},\qquad R_{2,caso}=1000\ \Omega.
$$

## Análisis matemático completo

### 1. Definición de las variables y condiciones iniciales

Se considera un circuito lineal con componentes ideales y constantes. Las condiciones iniciales son nulas:

$$
V_s(0^-)=0,\qquad F_s(0^-)=0.
$$

La corriente Fe(t) corresponde a i1(t) en el manuscrito y Fs(t) corresponde a i2(t). Estas variables representan corrientes eléctricas dentro de la analogía.

### 2. Ley de corrientes de Kirchhoff en N2

La corriente de entrada se divide entre el capacitor y la rama L–R2:

$$
F_e(t)=F_c(t)+F_s(t).
$$

Para el capacitor:

$$
F_c(t)=C\frac{dV_s(t)}{dt}.
$$

Por lo tanto:

$$
\boxed{F_e(t)=C\frac{dV_s(t)}{dt}+F_s(t)}.
$$

Despejando la derivada de la salida:

$$
\frac{dV_s(t)}{dt}=\frac{F_e(t)-F_s(t)}{C}.
$$

Integrando con condición inicial nula:

$$
\boxed{V_s(t)=\frac1C\int_0^t[F_e(\lambda)-F_s(\lambda)]\,d\lambda}.
$$

### 3. Ley de voltajes de Kirchhoff en la entrada

La entrada es igual a la suma del voltaje sobre R1 y el voltaje del nodo de salida:

$$
V_e(t)=R_1F_e(t)+V_s(t).
$$

Sustituyendo la expresión del capacitor:

$$
\boxed{V_e(t)=R_1F_e(t)+\frac1C\int_0^t[F_e(\lambda)-F_s(\lambda)]\,d\lambda}.
$$

### 4. Ley de voltajes de Kirchhoff en la rama L–R2

El voltaje de salida es la suma de los voltajes del inductor y del resistor:

$$
\boxed{V_s(t)=L\frac{dF_s(t)}{dt}+R_2F_s(t)}.
$$

Combinando con el voltaje del capacitor:

$$
\boxed{\frac1C\int_0^t[F_e(\lambda)-F_s(\lambda)]\,d\lambda
=L\frac{dF_s(t)}{dt}+R_2F_s(t)}.
$$

Estas ecuaciones integro-diferenciales describen las dos ramas y los dos elementos independientes de almacenamiento de energía.

### 5. Transformada de Laplace

Aplicando Laplace con condiciones iniciales nulas:

$$
V_e(s)=R_1F_e(s)+\frac{F_e(s)-F_s(s)}{Cs},
$$

$$
\frac{F_e(s)-F_s(s)}{Cs}=(Ls+R_2)F_s(s),
$$

$$
V_s(s)=(Ls+R_2)F_s(s).
$$

De la segunda ecuación:

$$
F_e(s)-F_s(s)=Cs(Ls+R_2)F_s(s).
$$

Despejando Fe(s):

$$
F_e(s)=[1+Cs(Ls+R_2)]F_s(s).
$$

Sustituyendo en la ecuación de entrada:

$$
V_e(s)=R_1[1+Cs(Ls+R_2)]F_s(s)+(Ls+R_2)F_s(s).
$$

Desarrollando:

$$
V_e(s)=[R_1+R_1LCs^2+R_1R_2Cs+Ls+R_2]F_s(s).
$$

Agrupando por potencias de s:

$$
\boxed{V_e(s)=[R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)]F_s(s)}.
$$

La corriente de la rama de respuesta es:

$$
F_s(s)=\frac{V_e(s)}{R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)}.
$$

### 6. Función de transferencia general

Como Vs(s) = (Ls + R2)Fs(s):

$$
V_s(s)=\frac{Ls+R_2}{R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)}V_e(s).
$$

Entonces:

$$
\boxed{G(s)=\frac{V_s(s)}{V_e(s)}=
\frac{Ls+R_2}{R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)}}.
$$

El denominador es de segundo orden por la presencia del capacitor y del inductor. El numerador contiene un cero real en:

$$
s_z=-\frac{R_2}{L}.
$$

### 7. Función de transferencia del control

Sustituyendo R1 = 1000 Ω, L = 0.1 H, C = 0.000001 F y R2 = 100000 Ω:

$$
R_1LC=(1000)(0.1)(10^{-6})=0.0001,
$$

$$
L+R_1R_2C=0.1+(1000)(100000)(10^{-6})=100.1,
$$

$$
R_1+R_2=1000+100000=101000.
$$

Por tanto:

$$
\boxed{G_{control}(s)=\frac{0.1s+100000}{0.0001s^2+100.1s+101000}}.
$$

### 8. Función de transferencia del caso

Sustituyendo R1 = 1000 Ω, L = 0.1 H, C = 0.001 F y R2 = 1000 Ω:

$$
R_1LC=(1000)(0.1)(0.001)=0.1,
$$

$$
L+R_1R_2C=0.1+(1000)(1000)(0.001)=1000.1,
$$

$$
R_1+R_2=1000+1000=2000.
$$

Entonces:

$$
\boxed{G_{caso}(s)=\frac{0.1s+1000}{0.1s^2+1000.1s+2000}}.
$$

### 9. Modelo de ecuaciones diferenciales ordinarias

La corriente de entrada también puede expresarse como:

$$
F_e(t)=\frac{V_e(t)-V_s(t)}{R_1}.
$$

Al sustituirla en la ley de corrientes:

$$
\boxed{\frac{dV_s(t)}{dt}=\frac{V_e(t)-V_s(t)}{R_1C}-\frac{F_s(t)}{C}}.
$$

De la rama L–R2:

$$
\boxed{\frac{dF_s(t)}{dt}=\frac{V_s(t)-R_2F_s(t)}{L}}.
$$

Los estados se seleccionan como z1 = Vs y z2 = Fs:

$$
\dot{\mathbf z}=
\begin{bmatrix}
-1/(R_1C)&-1/C\\
1/L&-R_2/L
\end{bmatrix}\mathbf z+
\begin{bmatrix}1/(R_1C)\\0\end{bmatrix}V_e,
$$

$$
V_s=\begin{bmatrix}1&0\end{bmatrix}\mathbf z,\qquad D=0.
$$

### 10. Ecuación diferencial de segundo orden

De la relación entre entrada y salida en Laplace:

$$
[R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)]V_s(s)=(Ls+R_2)V_e(s).
$$

Aplicando la transformada inversa:

$$
\boxed{R_1LC\ddot V_s(t)+(L+R_1R_2C)\dot V_s(t)+(R_1+R_2)V_s(t)
=L\dot V_e(t)+R_2V_e(t)}.
$$

Para simular entradas con saltos, la formulación de dos estados evita calcular numéricamente la derivada del escalón.

### 11. Análisis del error en estado estacionario

Se aplica un escalón unitario:

$$
V_e(t)=u(t),\qquad V_e(s)=\frac1s.
$$

Para cada planta en lazo abierto se define el error entre entrada y salida:

$$
e(t)=V_e(t)-V_s(t).
$$

En Laplace:

$$
E(s)=V_e(s)-V_s(s),\qquad V_s(s)=G(s)V_e(s).
$$

Entonces:

$$
E(s)=\frac{1-G(s)}{s}.
$$

Aplicando el teorema del valor final, válido para las plantas estables analizadas:

$$
e_{ss}=\lim_{s\to0}sE(s)=\lim_{s\to0}[1-G(s)]=1-G(0).
$$

La ganancia estática es:

$$
G(0)=\frac{L(0)+R_2}{R_1LC(0)^2+(L+R_1R_2C)(0)+(R_1+R_2)}
=\frac{R_2}{R_1+R_2}.
$$

Por tanto:

$$
e_{ss}=1-\frac{R_2}{R_1+R_2}
=\frac{R_1+R_2-R_2}{R_1+R_2}
=\boxed{\frac{R_1}{R_1+R_2}}.
$$

**Control:**

$$
V_{s,control}(\infty)=\frac{100000}{101000}=\frac{100}{101}\approx0.990099\ \mathrm V,
$$

$$
e_{ss,control}=\frac{1000}{101000}=\frac1{101}\approx0.00990099.
$$

Para una entrada de 1 V, el error equivale a 0.00990099 V, aproximadamente 0.990099 % de la amplitud de entrada.

**Caso:**

$$
V_{s,caso}(\infty)=\frac{1000}{2000}=\frac12=0.5\ \mathrm V,
$$

$$
e_{ss,caso}=\frac{1000}{2000}=\frac12=0.5.
$$

Para una entrada de 1 V, el error equivale a 0.5 V, es decir, 50 %.

**Diferencia entre control y caso:**

La referencia para el controlador es la respuesta del control. La diferencia de lazo abierto entre ambas plantas es:

$$
e_{ref}(t)=V_{s,control}(t)-V_{s,caso}(t).
$$

Su valor estacionario es:

$$
\boxed{e_{ref}(\infty)=\frac{100}{101}-\frac12=\frac{99}{202}\approx0.490099\ \mathrm V}.
$$

Esta diferencia no debe confundirse con el error de cada planta respecto a la entrada unitaria. Tampoco se utiliza aquí la expresión 1/[1+G(0)], que corresponde a otra estructura: una planta dentro de un lazo de realimentación unitaria.

### 12. Análisis de estabilidad

Los polos satisfacen:

$$
R_1LCs^2+(L+R_1R_2C)s+(R_1+R_2)=0.
$$

Definiendo:

$$
a=R_1LC,\qquad b=L+R_1R_2C,\qquad c=R_1+R_2,
$$

se aplica la fórmula general:

$$
\boxed{p_{1,2}=\frac{-b\pm\sqrt{b^2-4ac}}{2a}}.
$$

**Control:**

$$
p_{1,2}=\frac{-100.1\pm\sqrt{(100.1)^2-4(0.0001)(101000)}}{2(0.0001)}.
$$

$$
\boxed{p_1\approx-1010.010110,\qquad p_2\approx-999989.989890}\quad\mathrm{s}^{-1}.
$$

**Caso:**

$$
p_{1,2}=\frac{-1000.1\pm\sqrt{(1000.1)^2-4(0.1)(2000)}}{2(0.1)}.
$$

$$
\boxed{p_1\approx-2.000200,\qquad p_2\approx-9998.999800}\quad\mathrm{s}^{-1}.
$$

Los cuatro polos son reales y negativos. Ambas plantas son asintóticamente estables y sus denominadores corresponden a sistemas sobreamortiguados. En el plano complejo, todos los polos se ubican sobre el eje real negativo y tienen parte imaginaria igual a cero.

| Propiedad | Control | Caso |
| --- | ---: | ---: |
| Polo dominante | −1010.010110 s⁻¹ | −2.000200 s⁻¹ |
| Polo rápido | −999989.989890 s⁻¹ | −9998.999800 s⁻¹ |
| Cero | −1000000 s⁻¹ | −10000 s⁻¹ |
| Constante de tiempo dominante aproximada | 0.000990089 s | 0.499950 s |
| Valor final ante escalón de 1 V | 0.990099 V | 0.5 V |
| Error respecto a la entrada | 0.00990099 V | 0.5 V |

Las constantes de tiempo dominantes se estiman mediante tau = 1/|p1|. El caso es considerablemente más lento. El polo rápido y el cero están próximos, pero no se cancelan exactamente; para la implementación se conserva el modelo completo de segundo orden.

### 13. Efecto de C y R2 sobre la dinámica

El capacitor aumenta de 1 µF a 1000 µF y R2 disminuye de 100 kΩ a 1 kΩ. En conjunto, estas modificaciones producen una respuesta más lenta en el caso. R2 también modifica el valor final, mientras que C afecta la respuesta transitoria.

### 14. Respuesta temporal al escalón

Ante el escalón unitario, el control presenta una subida rápida hacia 0.990099 V y el caso una subida más lenta hacia 0.5 V. Ambas respuestas son no oscilatorias con estos parámetros.

## Controlador PID

Se considera un controlador PID para reducir la diferencia entre la respuesta del caso y la del control, utilizando esta última como referencia. Su efecto se evalúa comparando las respuestas del control, del caso sin controlador y del caso con PID.

## Simulaciones y resultados

Las simulaciones deben utilizar la misma entrada escalón de 1 V, condiciones iniciales nulas y el intervalo de 0 a 10 s.

Las figuras deben identificar Ve(t), Vs,control(t), Vs,caso(t) y Vs,PID(t), según la comparación realizada. El eje horizontal corresponde al tiempo en segundos y el vertical al voltaje en volts.

Los valores finales, errores y polos indicados en este README son resultados analíticos. Los documentos proporcionados no incluyen las gráficas finales, ganancias sintonizadas ni mediciones del montaje; por ello, no se afirma una concordancia experimental ni una mejora cuantificada del PID.

## Comparación con el montaje experimental

En laboratorio se construyen los circuitos del control y del caso y se registran simultáneamente entrada y salida con el osciloscopio. Una onda cuadrada puede aproximar una sucesión de escalones si su semiperiodo permite observar el establecimiento de la respuesta y se registran amplitud y nivel de offset.

La comparación debe considerar las tolerancias de resistores, capacitores e inductores, la resistencia interna del inductor, la resistencia serie equivalente del capacitor, la impedancia del generador y la carga de las sondas. En el controlador también influyen los límites de alimentación, la saturación y el ancho de banda de los amplificadores operacionales.

Para comparar con el modelo ideal es necesario medir o registrar los componentes realmente utilizados y las condiciones de excitación. Las fotografías y capturas del osciloscopio se integran como evidencia del montaje y no sustituyen el registro de esos valores.

## Conclusión

El análisis permitió obtener un modelo de segundo orden del sistema endócrino. Ambos circuitos son estables, pero el caso responde más lentamente y alcanza un voltaje final menor. La analogía eléctrica permite comprender estas diferencias y plantear el uso de un PID para aproximar la respuesta del caso a la del control.

**Palabras clave:** Sistema endócrino; Analogía eléctrica; Leyes de Kirchhoff; Función de transferencia; Error estacionario; Estabilidad; Controlador PID; Simscape Electrical; MATLAB; Python.

## Lista de archivos del repositorio

La documentación de la práctica se organiza con los siguientes elementos, conforme se completa cada actividad:

1. README.md con la interpretación fisiológica y el análisis matemático.
2. Imagen del diagrama biológico elaborado en BioRender.
3. Cuaderno computacional de MATLAB [.mlx] y su exportación [.pdf].
4. Modelos de Simulink [.slx].
5. Código fuente de Python [.py].
6. Gráficas de las respuestas.
7. Cálculo de componentes y esquema del controlador analógico.
8. Capturas del osciloscopio.
9. Archivo CITATION.cff con la información de la estudiante y del repositorio.

## Referencias

[1] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2026. Apartado 5.6: Modelado y control del sistema endócrino.

[2] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018.

[3] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.

[4] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.

[5] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.

[6] N. S. Nise, *Control Systems Engineering*, 8th ed., John Wiley & Sons, 2020.
