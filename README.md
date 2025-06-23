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
### 🐾 Navegación por Campo Potencial
### 💪 Gradiente del Campo Potencial
### 📡 Simulación en CoppeliaSim
### 📚 Conclusiones
