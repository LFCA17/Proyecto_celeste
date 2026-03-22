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
