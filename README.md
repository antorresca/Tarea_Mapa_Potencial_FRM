# 🗺️ Tarea Mapa Potencial FRM
## 🪶 Autores
* Juan Camilo Gomez Robayo
* Andres Camilo Torres Cajamarca
## 🔢 Procedimiento
### 🚗 Robot asigando 

<div align="center">
  <img src="https://github.com/user-attachments/assets/dbb591f7-67b8-4d72-ad7f-67075c5ce733" width="400">
</div>

### 🤖 Modelo del robot
Para el modelo cinemático, se tiene que el robot tiene 4 ruedas fijas motorizadas:

<div align="center">
  <img src="https://github.com/user-attachments/assets/c6d7a804-b02e-4e3d-bd9f-eddef953f2ac" width="400">
</div>

Con lo cual, cada rueda puede girar a una velocidad diferente ($\phi_1,\phi_2,\phi_3,\phi_4$), útil para terrenos irregulares; sin embargo, en este caso, el terreno es plano, por ello, se puede simplificar el modelo haciendo que ($\phi_1=\phi_4=\phi_l$ y $\phi_2=\phi_3=\phi_r$) haciendo que el modelo cinematico se simplifique a:

<div align="center">
  <img src="https://github.com/user-attachments/assets/a1009f12-ae63-4995-8720-1b5a8729ff0a" width="400">
</div>

*Nota: La rueda esferica se colocó simplemente para mantener la condición de estabilidad.*

Tomando esto, el robot *Summit XL* de *Robornik* puede actuar como un robot diferencial y se pueden emplear lo siguiente:

$$ \begin{bmatrix}
\frac{r}{2} & \frac{r}{2} \\
0 & 0 \\
\frac{r}{l} & \frac{r}{l} \\
\end{bmatrix} \cdot  \begin{bmatrix}
\phi_r \\
\phi_l \\
\end{bmatrix} =  \begin{bmatrix}
\dot{x} \\
\dot{y} \\
\omega \\
\end{bmatrix}$$

### 🌎 Mapas

Para calcular el radio ($R$) del circulo circunscrito en el robot, se midió la distancia en el CoppeliaSim colocando un cilindro de tal forma que se lograra la imagen de referencia dada

<div align='center'>
  <img src="https://github.com/user-attachments/assets/55f97d9d-f0ae-4311-8983-f7b5e15032dd" width="300" style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/a00c83cf-f364-40c5-987c-b3a1ff893876" width="300" style="display: inline-block;">
</div>

Con ello se obtuvo un valor de $0.46$ m. Con ello y empleando el codigo [arena2025.m](https://github.com/labsir-un/FRM_Lab_0_Intro_software/blob/main/Secciones/Matlab/scripts/Mapa_potencial/arena2025.m) se obtuvo el siguiente mapa de obstaculos

<div align='center'>
  <img src='https://github.com/user-attachments/assets/0f2db760-788d-4a87-9efe-67c38d9d1e57' width=400>
</div>

Para crear el campo de repulsión se empleará la función sigmoidal, específicamente la tangente hiperbólica (_tanh_), que permite suavizar el comportamiento de la fuerza de repulsión a medida que el robot se aproxima a los obstáculos. Esta función evita discontinuidades o repulsiones infinitas en el borde del obstáculo, generando un campo más estable y continuo, ideal para aplicaciones en entornos dinámicos o con múltiples obstáculos distribuidos.

### 🐾 Navegación por Campo Potencial

Para crear el campo potencial, se tuvo en cuenta que el campo potencial sigue la siguiente forma

$$U_{\text{total}}(\mathbf{x}) = U_{\text{att}}(\mathbf{x}) + \sum_j U_{\text{rep}}^{(j)}(\mathbf{x})$$

Para el campo de atracción se usó 

$$U_{att}(x)= \zeta |x−x_{meta}|^2$$

y en el caso de campo de repulsión

$$U_{\text{rep}}(\mathbf{x}) = 
\begin{cases}
\alpha \cdot \tanh\left(\beta \cdot (r_{inf} - d_{\text{borde}})\right), & \text{si } 0 < d_{\text{borde}} < r_{inf} \\
\alpha, & \text{si } d_{\text{borde}} \leq 0 \\
0, & \text{si } d_{\text{borde}} \geq r_{inf}
\end{cases}$$

donde $d_{\text{borde}} = \|\mathbf{x} - \mathbf{c}_j\| - R_j$ es la distancia al borde del obstaculo con centro en $c_j$ y radio $R_j$.

Teniendo en cuenta eso, se definieron los siguientes parametros para la creacion del campo pontencial 

<div align='center'>
  <table><thead><tr><th>Parámetro</th><th>Valor</th><th>Descripcion</th></tr></thead><tbody><tr><td>$\alpha$</td><td>10</td><td>Intensidad de campo de repulsion</td></tr><tr><td>$\beta$</td><td>1</td>  <td>Pendiente de función _sigmoidal_</td></tr><tr><td>$r_{inf}$</td><td>$k\cdot 0.1$</td><td>Zona de interaccion con obstaculos</td></tr><tr><td>$\zeta$</td><td>0.1</td><td>Intensidad de campo de atracción</td></tr></tbody></table>
</div>

Teniendo en cuenta eso se empleó el archivo [codigoCreacionMapas.mlx](Archivos/codigoCreacionMapas.mlx) en donde se crea una malla 100x100 (100 puntos en _x_ y 100 puntos en _y_) y se evalua punto a punto el campo potencial. Con este código se obtuvo el siguiente mapa con campo potencial

<div align='center'>
  <img src='https://github.com/user-attachments/assets/b961c9c1-d821-4e60-88e4-322736bdd60e' width=400>
</div>

**Nota:** Como se puede observar, en el interior de los obstáculos el valor del campo es constante. Esto se hace intencionalmente para evitar la aparición de mínimos locales dentro del obstáculo, lo cual podría atrapar al robot y dificultar su salida.

Una vez teniendo esto, se realizó el calculo de las trayectorias variando la orientacion inicial del robot, dando

<div align='center'>
  <img src='https://github.com/user-attachments/assets/5c7abf4c-bf0d-472b-96fb-e57a74cc40b1' width=400 style="display: inline-block;">
  <img src='https://github.com/user-attachments/assets/a865e7fe-4706-4b4f-88df-ee4370b22a52' width=400 style="display: inline-block;">
  <img src='https://github.com/user-attachments/assets/4e937b53-8224-49ab-bc66-86ec0494e588' width=400 style="display: inline-block;">
</div>

Como se puede observar, en los casos de $30^{o}$ y $60^{o}$ el robot logra llegar al objetivo porque desde el principio empieza a evadir el obstaculo. Sin embargo, en el caso de $45^{o}$ el robot quedaria justo de frente al obstaculo, generando un mínimo local, lo cual genera que el robot no pueda evadir este obstaculo. Para ello se investigaron formas de evitar estos casos, algunas opciones son:

1. **Uso de gradiante previo:** Se reutiliza la última dirección para evitar quedarse atrapado en mínimos locales.
2. **Uso de movimientos aleatorios:** Se añade una perturbación aleatoria para escapar de mínimos locales y continuar avanzando.
3. **Uso de campo potencial tangencial:** Se añade un campo tangencial alrededor de obstáculos, rotando el vector de repulsión, para evitar estancamientos por mínimos locales.

Despues de varias pruebas con estas alternativas, se decidió tomar la opción 3 (Uso de campo potencial tangencial), para ello, se calcula una fuerza de repulsion tangente de la siguiente manera

$$\vec{F_{tan}}= 2\cdot \frac{r_{inf}-d_{borde}}{r_{inf}} \cdot \frac{1}{|\vec{d}|}\cdot
\begin{bmatrix}
-d_y \\
d_x
\end{bmatrix}$$

Esta fuerza tangencial solo se usa cuando la fuerza total es $<6$ (este valor se halló despues de algunas iteraciones). Con esta modificacion, al calcular la trayectoria con una orientacion incial de $45^{o}$, se obtuvo 

<div align='center'>
  <img src='https://github.com/user-attachments/assets/3c50dbf5-cfd7-4d04-aaf4-662c447e73da' width=400>
</div>

### 💪 Gradiente del Campo Potencial

A continuación se muestra el gradiente del campo potencial

<div align='center'>

  <img src='https://github.com/user-attachments/assets/36832992-61cd-4be4-babe-4d078e10f786' width=400>
</div>

### 📡 Simulación en CoppeliaSim

En la siguiente imagen se muestra la escena creada en CoppeliaSim, resultante de las dimensiones del vehículo y la constante de radio de giro que para el caso fue de 0.45 metros.

Radio de giro para el vehículo móvil

<div align='center'>
  <img src='https://github.com/user-attachments/assets/58272737-a8a8-4620-8761-7ad3459174b3' width=400>
</div>

Escéna CoppeliaSim

<div align='center'>
  <img src='https://github.com/user-attachments/assets/3babf186-14db-4488-b0bb-18a1841ddafe' width=800>
</div>

En el siguiente video se presenta la simulación en CoppeliaSim de la navegación del robot Robotnik siguiendo la trayectoria generada por el campo potencial, para esta simulación se utilizó la conexión entre Matlab y CoppelliaSim utilizando el código [Conexion_coppelia.mlx](Archivos/Conexion_coppelia.mlx).

<div align="center">
  <video src="https://github.com/user-attachments/assets/b876411f-e2a4-4871-8e4a-a7683be97028">
</div>
    
### 📚 Conclusiones

La navegación por Campo Potencial Artificial resulta ser una herramienta bastante útil para la implementación de generación de trayectorias, además del bajo conste computaciónal debido al uso de funciones, permite una fácil implementacion en diferentes geometrías, Siempre y cuando se tenga el adecuado tratamiento para los mínimos locales, donde puede fallar la planeación de la trayectoria.
