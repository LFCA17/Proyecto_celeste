# Instrucciones de Copilot para Proyecto Celeste

Este repositorio se enfoca en simulaciones del problema de N-cuerpos en mecanica celeste.

## Contexto tecnico

- Curso base: enfoque de Jorge Zuluaga.
- Herramientas: numpy, REBOUND y pymcel cuando aplique.
- Objetivos de calidad del codigo:
  - Fisicamente correcto.
  - Numericamente estable o con diagnostico de error.
  - Modular, claro y extensible.

## Reglas de modelado

- Representar sistemas manuales como:
  - system = [dict(m=..., r=np.array([x,y,z]), v=np.array([vx,vy,vz])), ...]
- Alternativa con REBOUND:
  - sim = rebound.Simulation(); sim.add(...)
- Usar unidades consistentes; preferir unidades canonicas con G = 1 cuando sea posible.

## Ecuaciones de movimiento

Implementar la aceleracion gravitacional:

r_ddot_i = - sum_{j != i} G * m_j * (r_i - r_j) / |r_i - r_j|^3

Requisitos:

- Doble loop o vectorizacion.
- Usar np.linalg.norm.
- Evitar division por cero en encuentros cercanos.
- Recordar que es un sistema acoplado de 3N EDOs.

## Integracion temporal

Incluir al menos:

- Euler (referencia basica).
- Leapfrog/Verlet (recomendado por estabilidad energetica).

Opcional:

- RK4 para precision local.
- En REBOUND, preferir ias15 o whfast segun el caso.

## Energia y diagnostico fisico

- Energia cinetica: K = sum(1/2 m_i |v_i|^2)
- Energia potencial: U = -sum_{i<j} G m_i m_j / r_ij
- Energia total: E = K + U

Reglas obligatorias:

- No duplicar pares (i,j) y (j,i).
- Reportar error relativo de energia en el tiempo.
- Interpretar estado del sistema con E:
  - E < 0 ligado
  - E = 0 escape critico
  - E > 0 no ligado

## Validaciones minimas en respuestas

Siempre que se entregue codigo de simulacion:

1. Explicar brevemente la fisica relevante.
2. Entregar codigo limpio y modular.
3. Interpretar resultados.

Siempre incluir:

- Trayectorias (2D o 3D).
- Energia vs tiempo.
- Conservacion de energia (error relativo).

Opcional:

- Momento de inercia.
- Virial.
- Animaciones.

## Buenas practicas de implementacion

Preferir funciones separadas como:

- compute_accelerations(system)
- compute_energy(system)
- step(system, dt)

Evitar codigo monolitico.
Documentar supuestos, unidades y limitaciones numericas.

## Deteccion de errores obligatoria

Detectar y corregir activamente:

- Doble conteo en energia potencial.
- Inestabilidad numerica.
- Paso dt demasiado grande.
- Inconsistencias de unidades.

## Extension avanzada

Cuando sea util, habilitar o sugerir:

- Centro de masa.
- Reescalado y unidades canonicas.
- Coordenadas de Jacobi.
- Sistemas jerarquicos.
- Comparaciones: implementacion propia vs REBOUND vs pymcel.

## Graficos y simulaciones

### Contexto

Las visualizaciones deben ayudar a:

- Entender la dinamica del sistema.
- Validar resultados numericos.
- Analizar estabilidad, energia y comportamiento a largo plazo.

Herramientas esperadas:

- numpy
- matplotlib.pyplot
- matplotlib.animation
- plotly (clave para interactividad)
- REBOUND
- pymcel

Objetivo:

- Generar graficos claros, fisicos e interpretables.
- Priorizar analisis fisico por encima de estetica.

### Matplotlib (pylot)

Importar siempre:

- import matplotlib.pyplot as plt

Usar segun aplique:

- plt.plot()
- plt.scatter()
- plt.xlabel(), plt.ylabel()
- plt.title()
- plt.legend()
- plt.axis('equal')
- plt.grid()
- plt.show()

Preferir estructura con:

- fig, ax = plt.subplots()

### Plotly (interactivo)

Importar:

- import plotly.graph_objects as go

Ventajas esperadas:

- Zoom interactivo.
- Rotacion en 3D.
- Mejor visualizacion de trayectorias complejas.
- Ideal para notebooks.

### Tipos de graficos a priorizar

1. Trayectorias 2D.
  - Matplotlib: plt.plot(x, y)
  - Plotly: go.Scatter(x=x, y=y, mode='lines')
  - Opcional: marcar posicion actual.

2. Trayectorias 3D (muy importante).
  - Usar plotly con go.Scatter3d(x=..., y=..., z=..., mode='lines', name='body_i').
  - Permitir rotacion interactiva.
  - Usar colores distintos por cuerpo.

3. Energia vs tiempo.
  - Matplotlib: plt.plot(t, E)
  - Plotly: go.Scatter(x=t, y=E)
  - Mostrar K(t), U(t), E(t).
  - Verificar conservacion.

4. Momento de inercia.
  - Graficar I(t) e interpretar evolucion.

5. Virial (avanzado).
  - Graficar G(t) y analizar estabilidad.

6. Animaciones.
  - Matplotlib: FuncAnimation.
  - Plotly: frames y go.Figure(frames=frames).
  - Incluir reproduccion temporal y control de velocidad cuando sea posible.

7. Comparacion de metodos.
  - Euler vs Leapfrog.
  - Implementacion propia vs REBOUND.
  - Usar multiples curvas en la misma grafica.

8. Espacio de fase.
  - x vs vx y y vs vy para estabilidad.

### Uso de REBOUND en graficos

- Extraer posiciones con sim.particles[i].x, y, z.
- Graficar con matplotlib o plotly segun necesidad.
- Energia del sistema con sim.calculate_energy().

### Buenas practicas de visualizacion

- Usar funciones modulares:
  - plot_trajectories()
  - plot_energy()
  - animate_system()
- Mantener escalas fisicas correctas.
- Usar axis('equal') en matplotlib.
- En plotly, usar scene=dict(aspectmode='data').

### Errores a detectar

- Energia no conservada.
- Trayectorias no fisicas.
- Escalas incorrectas.
- dt demasiado grande.
- Visualizaciones enganiosas.

### Estilo de respuesta para visualizaciones

Siempre:

1. Explicar que se va a graficar y por que.
2. Mostrar codigo (matplotlib o plotly).
3. Explicar como interpretar el resultado.

### Bonus avanzado

- Visualizacion 3D interactiva completa.
- Animaciones con energia en tiempo real.
- Comparacion entre sistemas ligados y no ligados.
- Integracion con notebooks tipo pymcel.
- from scipy.integrate import odeint, solve_ivp cuando aplique.
