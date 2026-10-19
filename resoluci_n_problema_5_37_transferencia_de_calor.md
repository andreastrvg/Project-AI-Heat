# Solución de Ingeniería: Problema 5-37
**Curso:** INQU 4001 – Transferencia de Calor  
**Análisis:** Conducción Unidimensional de Calor en Estado Estacionario con Generación Interna  
**Método:** Diferencias Finitas (FDM) y Verificación Analítica  

---

### 1. Suposiciones Físicas

1. **Transferencia de calor unidimensional:** Se asume que el gradiente de temperatura solo existe a lo largo del espesor de la placa (eje $x$). En los ejes $y$ y $z$ la variación es despreciable ($T = T(x)$).
2. **Estado estacionario:** La temperatura en cada punto y los flujos de calor no cambian con respecto al tiempo ($\partial T / \partial t = 0$).
3. **Propiedades termofísicas constantes:** La conductividad térmica del aluminio ($k = 34\text{ W/m}\cdot\text{K}$) es uniforme e independiente de la temperatura.
4. **Generación uniforme de calor:** La tasa de generación volumétrica de energía dentro de la placa es constante ($\dot{e}_g = 6 \times 10^5\text{ W/m}^3$).
5. **Condiciones de contorno uniformes:** La superficie en $x = 0$ está perfectamente aislada ($q'' = 0$) y la superficie en $x = L$ está expuesta a convección uniforme ($h = 60\text{ W/m}^2\cdot\text{K}$, $T_\infty = 30^\circ\text{C}$).

---

### 2. Derivación de la Ecuación Diferencial Gobernante

Partiendo de la conservación de la energía en un volumen de control diferencial de área $A$ y grosor $dx$:

$$\dot{E}_{\text{entra}} - \dot{E}_{\text{sale}} + \dot{E}_{\text{gen}} = \dot{E}_{\text{almacenada}}$$

Para estado estacionario ($\dot{E}_{\text{almacenada}} = 0$):

$$q_x - q_{x+dx} + \dot{e}_g (A \, dx) = 0$$

Aplicando la expansión en serie de Taylor $q_{x+dx} = q_x + \frac{dq_x}{dx} dx$ y la Ley de Fourier ($q_x = -k A \frac{dT}{dx}$):

$$-\frac{d}{dx}\left(-k A \frac{dT}{dx}\right) dx + \dot{e}_g A \, dx = 0 \implies \frac{d}{dx}\left(k \frac{dT}{dx}\right) + \dot{e}_g = 0$$

Dado que $k$ es constante, dividimos por $k$ obteniendo la Ecuación de Poisson en 1D:

$$\frac{d^2 T}{dx^2} + \frac{\dot{e}_g}{k} = 0$$

---

### 3. Condiciones de Contorno

1. **Superficie aislada en $x = 0$ (Nodo 0):** Flujo de calor nulo (Simetría / Adiabático)
   $$\left. \frac{dT}{dx} \right|_{x=0} = 0$$

2. **Superficie convectiva en $x = L = 0.05\text{ m}$ (Nodo 5):** Balances de conducción y convección
   $$-\left. k \frac{dT}{dx} \right|_{x=L} = h (T(L) - T_\infty)$$

---

### 4. Discretización Nodal y Balance de Energía

* **Parámetros del dominio:**
  * Espesor total: $L = 0.05\text{ m}$
  * Intervalos: $5$ segmentos iguales $\implies \Delta x = \frac{0.05\text{ m}}{5} = 0.01\text{ m} = 1\text{ cm}$
  * Nodos: $6$ nodos ($T_0, T_1, T_2, T_3, T_4, T_5$) en $x = 0, 1, 2, 3, 4, 5\text{ cm}$.

Aplicando balance de energía en volumen de control para un área unitaria $A = 1\text{ m}^2$:
$$\sum \dot{Q}_{\text{hacia el nodo}} + \dot{E}_{\text{gen}} = 0$$

#### Nodo 0 (Frontera Aislada en $x = 0$)
Volumen de control de espesor $\Delta x / 2$:

$$k A \frac{T_1 - T_0}{\Delta x} + \dot{e}_g A \left(\frac{\Delta x}{2}\right) = 0$$

Multiplicando por $\frac{\Delta x}{k A}$:
$$-T_0 + T_1 = -\frac{\dot{e}_g (\Delta x)^2}{2 k}$$

#### Nodos Interiores $m = 1, 2, 3, 4$
Volumen de control de espesor $\Delta x$:

$$k A \frac{T_{m-1} - T_m}{\Delta x} + k A \frac{T_{m+1} - T_m}{\Delta x} + \dot{e}_g A \Delta x = 0$$

Multiplicando por $\frac{\Delta x}{k A}$:
$$T_{m-1} - 2 T_m + T_{m+1} = -\frac{\dot{e}_g (\Delta x)^2}{k} \quad (\text{para } m = 1, 2, 3, 4)$$

#### Nodo 5 (Frontera Convectiva en $x = L$)
Volumen de control de espesor $\Delta x / 2$:

$$k A \frac{T_4 - T_5}{\Delta x} + \dot{e}_g A \left(\frac{\Delta x}{2}\right) = h A (T_5 - T_\infty)$$

Reorganizando:
$$T_4 - \left(1 + \frac{h \Delta x}{k}\right) T_5 = -\frac{h \Delta x}{k} T_\infty - \frac{\dot{e}_g (\Delta x)^2}{2 k}$$

---

### 5. Formulación del Sistema Numérico

#### Cálculo de Constantes:
* $\frac{\dot{e}_g (\Delta x)^2}{k} = \frac{(6 \times 10^5)(0.01)^2}{34} = \mathbf{1.764706^\circ\text{C}}$
* $\frac{\dot{e}_g (\Delta x)^2}{2 k} = \mathbf{0.882353^\circ\text{C}}$
* $\frac{h \Delta x}{k} = \frac{(60)(0.01)}{34} = \mathbf{0.017647}$

#### Sistema Completo de Ecuaciones:

1. **Nodo 0:** $-T_0 + T_1 = -0.882353$
2. **Nodo 1:** $T_0 - 2T_1 + T_2 = -1.764706$
3. **Nodo 2:** $T_1 - 2T_2 + T_3 = -1.764706$
4. **Nodo 3:** $T_2 - 2T_3 + T_4 = -1.764706$
5. **Nodo 4:** $T_3 - 2T_4 + T_5 = -1.764706$
6. **Nodo 5:** $T_4 - 1.017647 T_5 = -1.411765$

Representación Matricial $\mathbf{A} \mathbf{T} = \mathbf{C}$:

$$\begin{bmatrix}
-1 & 1 & 0 & 0 & 0 & 0 \\
1 & -2 & 1 & 0 & 0 & 0 \\
0 & 1 & -2 & 1 & 0 & 0 \\
0 & 0 & 1 & -2 & 1 & 0 \\
0 & 0 & 0 & 1 & -2 & 1 \\
0 & 0 & 0 & 0 & 1 & -1.017647
\end{bmatrix}
\begin{bmatrix}
T_0 \\ T_1 \\ T_2 \\ T_3 \\ T_4 \\ T_5
\end{bmatrix}
=
\begin{bmatrix}
-0.882353 \\ -1.764706 \\ -1.764706 \\ -1.764706 \\ -1.764706 \\ -1.411765
\end{bmatrix}$$

---

### 6 y 8. Resultados Numéricos y Verificación Analítica

#### Solución Analítica Exacta:
Integrando $\frac{d^2 T}{dx^2} = -\frac{\dot{e}_g}{k}$ dos veces:

$$T(x) = -\frac{\dot{e}_g}{2 k} x^2 + C_1 x + C_2$$

Aplicando condiciones de contorno:
1. En $x = 0$, $\frac{dT}{dx} = 0 \implies C_1 = 0$.
2. En $x = L$, $\dot{e}_g L = h (T(L) - T_\infty) \implies T(L) = 30 + \frac{(6 \times 10^5)(0.05)}{60} = \mathbf{530.00^\circ\text{C}}$.

Calculando la constante $C_2$:
$$C_2 = 530 + \frac{(6 \times 10^5)(0.05)^2}{2(34)} = \mathbf{552.05882^\circ\text{C}}$$

Ecuación Analítica:
$$T(x) = 552.05882 - 8823.5294 x^2 \quad [{^\circ}\text{C}]$$

---

### 7. Identificación de Temperaturas Extremas

* **Temperatura Máxima ($T_{\max}$):** **$552.06^\circ\text{C}$** en el **Nodo 0 ($x = 0$)**. Debido a que $x=0$ está aislado, el calor debe viajar la distancia máxima para ser disipado.
* **Temperatura Mínima ($T_{\min}$):** **$530.00^\circ\text{C}$** en el **Nodo 5 ($x = L = 5\text{ cm}$)**, donde el calor se disipa activamente al fluido.

---

### 9. Conservación de Energía y Unidades

#### Balance de Energía Global:
$$\dot{Q}_{\text{generado}} = \dot{e}_g \cdot (A \cdot L) = (6 \times 10^5\text{ W/m}^3) \cdot (1\text{ m}^2) \cdot (0.05\text{ m}) = \mathbf{30,000\text{ W/m}^2}$$

$$\dot{Q}_{\text{convectivo}} = h \cdot A \cdot (T_5 - T_\infty) = (60\text{ W/m}^2\cdot\text{K}) \cdot (1\text{ m}^2) \cdot (530.00 - 30) = \mathbf{30,000\text{ W/m}^2}$$

$$\dot{Q}_{\text{generado}} - \dot{Q}_{\text{convectivo}} = 0 \quad \text{(Energía perfectamente conservada)}$$

---

### 10. Interpretación Física del Perfil de Temperatura

1. **Perfil Parabólico:** La dependencia cuadrática de $x$ se debe a la generación de calor uniforme. A medida que nos acercamos a $x = L$, la cantidad acumulada de calor que fluye aumenta, incrementando la pendiente del perfil.
2. **Resistencia Térmica Dominante:** La resistencia por convección ($R_{\text{conv}} = \frac{1}{h} = 0.0167\text{ m}^2\cdot\text{K/W}$) es significativamente mayor que la resistencia por conducción ($R_{\text{cond}} = \frac{L}{k} = 0.00147\text{ m}^2\cdot\text{K/W}$). Por esta razón, la mayor caída de temperatura ocurre entre la superficie y el fluido ($500^\circ\text{C}$) en comparación con la caída interna dentro de la placa ($22.06^\circ\text{C}$).

---

### 11. Posibles Fuentes de Error Numérico

1. **Error de Truncamiento:** Derivado de la aproximación de diferencias finitas (orden $O(\Delta x^2)$).
2. **Error de Redondeo:** Pérdida de precisión al resolver el sistema matricial.
3. **Malla Muy Gruesa:** Seleccionar un $\Delta x$ demasiado grande cuando las propiedades térmicas dependen de la temperatura o la generación no es uniforme.

---

### 12. Tabla Resumen de Resultados

| Nodo ($m$) | Posición $x$ (cm) | Temp. FDM ($^\circ\text{C}$) | Temp. Analítica ($^\circ\text{C}$) | Diferencia ($^\circ\text{C}$) | Descripción Física |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | $0.0$ | **552.06** | **552.06** | 0.00 | Superficie aislada ($T_{\max}$, flujo nulo) |
| **1** | $1.0$ | **551.18** | **551.18** | 0.00 | Punto interior |
| **2** | $2.0$ | **548.53** | **548.53** | 0.00 | Punto interior |
| **3** | $3.0$ | **544.12** | **544.12** | 0.00 | Punto interior |
| **4** | $4.0$ | **537.94** | **537.94** | 0.00 | Punto interior |
| **5** | $5.0$ | **530.00** | **530.00** | 0.00 | Superficie convectiva ($T_{\min}$) |