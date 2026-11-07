## Ámbito

La investigación combina un ámbito **ofensivo** (inyección de fallos / _fault injection_) para provocar la vulneración, y un ámbito **defensivo** (_hardening_ de software) para mitigarla. A diferencia del planteamiento inicial, el ataque se ejecuta en un **entorno de emulación determinista** (QEMU + GDB) en lugar de sobre hardware físico, dejando la validación física como objetivo opcional.

---
## Descripción de la investigación

### Pregunta de investigación
¿Es la verificación de firmas de algoritmos post-cuánticos (PQC) sobre arquitecturas embebidas (ARM Cortex-M4) vulnerable a inyección de fallos durante el cálculo crítico, y qué contramedidas a nivel de código C/Rust neutralizan ese riesgo con un coste de rendimiento asumible?

### Hipótesis de partida
La robustez _algorítmica_ de un esquema PQC no garantiza la robustez de su _implementación_: un único fallo (salto de instrucción o alteración de un registro) en el punto de verificación puede llevar a aceptar una firma inválida, con independencia de la fortaleza matemática del esquema.

### Objetivos
- **O1.** Desplegar una implementación PQC de referencia (Dilithium / ML-DSA) sobre firmware _bare-metal_ que se ejecute en un Cortex-M4 emulado.
- **O2.** Construir un _harness_ de inyección de fallos **determinista y reproducible** (QEMU + script de GDB) capaz de forzar un fallo en una instrucción concreta de la verificación.
- **O3.** Caracterizar el modelo de fallo explotable: identificar qué instrucciones / registros, al alterarse, provocan la aceptación de una firma inválida, y con qué frecuencia.
- **O4.** Diseñar e implementar contramedidas de software (_hardening_) y **medir su eficacia** comparando la tasa de éxito del ataque antes y después.
- **O5.** Cuantificar el coste de rendimiento (ciclos y tamaño de código) de cada contramedida.
- **O6 (opcional).** Validar uno de los fallos encontrados sobre hardware físico real mediante inyección por voltaje, si el calendario lo permite.

### Por qué la emulación
El ataque sobre hardware casero con crowbar y orquestador de bajo coste tiene un _jitter_ elevado y una ventana explotable de nanosegundos: produce muchos _resets_ y pocos fallos útiles, con alto riesgo de agotar el calendario sin resultados. La emulación resuelve esto:

- **Determinismo:** se inyecta el fallo exactamente en la instrucción deseada, sin lotería de temporización.
- **Reproducibilidad:** cualquier revisor puede re-ejecutar el experimento.
- **Coste cero y velocidad:** permite barrer sistemáticamente todas las instrucciones del bloque de verificación.
- **El valor del proyecto (la Fase 3 de hardening) no queda supeditado** a que un ataque físico casero funcione.

---
## Motivación
El estándar PQC está reemplazando a RSA y ECC en funciones críticas como el _Secure Boot_ en chips como OpenTitan o Caliptra 2.0, lo que genera una nueva superficie de ataque. Dotar de resistencia a fallos a estas implementaciones es el siguiente gran reto defensivo de la era post-cuántica.

---
## Alcance
### Qué se va a analizar
Implementación en **C** del esquema **Dilithium (ML-DSA)** de la librería `pqm4`, y su resistencia a fallos inyectados en las operaciones críticas de la verificación: la comprobación `w'1 = HighBits(Az - c·t1·2^d)` y la comparación final que decide aceptar o rechazar la firma, línea documentada matemáticamente por CARDIS 2023.

### Qué queda fuera
- Vulnerabilidades teóricas o matemáticas del algoritmo.
- Despliegue corporativo a gran escala.
- Ataques de canal lateral (SPA/DPA, EM) y ataques por láser.
- Un segundo algoritmo (SPHINCS+/XMSS): se descarta para mantener el proyecto abarcable; se centra todo el esfuerzo en Dilithium.

---
## Estado del arte
- **Bypassing PQC Signature Verification with Fault Injection** (Black Hat USA 2025, Fikret Garipay). Demuestra la falsificación de firmas válidas sobre _bare-metal_ manipulando niveles de voltaje.

- **Fault Attacks Sensitivity of Public Parameters in the Dilithium Verification** (CARDIS 2023). Documenta la sensibilidad de la verificación de Dilithium frente a fallos.

### Aportación de esta investigación
Frente al estado del arte, que ejecuta el ataque sobre hardware especializado, esta investigación:

1. **Reproduce el fallo en un entorno de emulación determinista**, accesible y reproducible por cualquiera, caracterizando el modelo de fallo completo del bloque de verificación.

2. Pone el peso en la **Fase 3 (hardening)**: diseña, implementa y **evalúa cuantitativamente** las contramedidas, que es el aporte defensivo diferencial.

---
## Sistemas objeto de la investigación
- **Arquitectura objetivo:** ARM Cortex-M4, firmware _bare-metal_.

- **Entorno de ejecución:** **QEMU** emulando una máquina compatible Cortex-M4 (p. ej. `mps2-an386` / `netduinoplus2`), con **GDB** (`arm-none-eabi-gdb`) como orquestador de fallos.

- **Acceso y laboratorio:** pleno acceso, entorno 100% software y reproducible.

- **Cobertura legal:** total. Todo el trabajo se realiza en emulación sobre código abierto y, en su caso, sobre hardware propiedad del investigador, sin tocar infraestructuras de terceros.

---
## Material hardware / software necesario

### Software (núcleo del proyecto)
- **`pqm4`** (Dilithium / ML-DSA sobre Cortex-M4) y _toolchain_ `arm-none-eabi-gcc`.

- **QEMU** (`qemu-system-arm`) y **`arm-none-eabi-gdb`** con _scripting_ en Python para automatizar la inyección y el barrido.

- Scripts propios de análisis y registro en Python (resultados a CSV).
### Hardware (opcional, solo si se aborda O6)
- **Se posee**: Placas orquestadoras (Arduino / Raspberry Pi Pico), voltímetro, _breadboard_ y cableado.

- **Se necesitaría**: Placa víctima **STM32F4 Discovery (F407)** con unidad de repuesto, MOSFET-N rápido, resistencias de puerta y _shunt_. _Nota: se usa F407 y no F417 porque su acelerador cripto hardware no interviene en PQC por software y la Discovery F407 es la plataforma de referencia de `pqm4`._

---
## Metodología prevista
Enfoque de auditoría técnica por fases.

- **Fase 0 — _Harness_ de inyección.** Montar QEMU + GDB, arrancar un programa trivial _bare-metal_ y validar que el script puede detener la ejecución en una instrucción concreta, **alterar un registro / saltar una instrucción** y continuar. Demuestra el mecanismo antes de atacar Dilithium.

- **Fase 1 — Despliegue del objetivo.** Compilar Dilithium (`pqm4`) para Cortex-M4 y ejecutar su verificación en QEMU, con un caso de firma **válida** y uno **inválida** como referencia.

- **Fase 2 — Campaña de inyección.** Recorrer sistemáticamente las instrucciones del bloque de verificación; en cada una, inyectar el fallo (_instruction skip_ y _bit-flip_ de registro) sobre el caso de firma **inválida** y registrar si pasa a ser aceptada. Salida: mapa de instrucciones/registros explotables.

- **Fase 3 — Contramedidas / _hardening_.** Implementar las defensas, recompilar y **repetir la Fase 2** sobre el código endurecido.

- **Fase 4 (opcional) — Validación física (O6).** Reproducir uno de los fallos explotables sobre la STM32F407 con inyección por voltaje.
### Modelo de fallo
- _Instruction skip_: avanzar el PC saltando una instrucción (equivale al efecto típico de un glitch).
- _Bit-flip_ / corrupción de registro: alterar el valor de un registro (p. ej. el que porta el resultado de la comparación) justo antes de usarse.
### Contramedidas a evaluar (Fase 3)
- Doble verificación con **comparación redundante** del resultado.
- Eliminación del **punto único de retorno** (`return 0`); uso de valores centinela difíciles de falsear con un solo _bit-flip_.
- Verificación de **integridad del flujo de control** y del contador de los bucles críticos.
- **Redundancia temporal** / repetición de la comprobación crítica.
- (Opcional) _infective computation_: propagar el fallo en lugar de solo detectarlo.

### Métrica de evaluación

| Métrica                   | Descripción                                                                          |
| ------------------------- | ------------------------------------------------------------------------------------ |
| Instrucciones explotables | nº de puntos del bloque de verificación cuyo fallo acepta una firma inválida         |
| Tasa de éxito             | % de inyecciones que logran la aceptación indebida                                   |
| Reducción tras hardening  | caída de la tasa de éxito / de los puntos explotables tras aplicar cada contramedida |
| Coste de rendimiento      | ciclos y tamaño de código añadidos por cada contramedida                             |

El éxito del _hardening_ se mide como la reducción (idealmente a cero) de los puntos explotables, ponderada por su coste de rendimiento.

### Evidencias y criterios
Se considera cumplido el criterio al documentar, de forma reproducible, qué fallos hacen que la verificación acepte una firma inválida, y al demostrar que el código endurecido (Fase 3) detecta o neutraliza esos fallos. Un modelo de fallo bien caracterizado, aun sin la validación física opcional, satisface el objetivo.

---
## Planteamiento de hitos por entrega y fechas

| Semanas | Fechas          | Hito                                                                                                                                                        |
| ------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1–2     | 13 oct – 27 oct | _Toolchain_ + QEMU + GDB operativos. **Harness de inyección (Fase 0)** sobre programa trivial. Compilación de Dilithium (`pqm4`).                           |
| 3–4     | 28 oct – 10 nov | Ejecución de la verificación de Dilithium en QEMU (casos válido/ inválido) y automatización del _script_ de inyección.                                      |
| 5–6     | 11 nov – 24 nov | Campaña de inyección (Fase 2): barrido del bloque de verificación y construcción del mapa de puntos explotables.                                            |
| 7–8     | 25 nov – 8 dic  | Implementación de contramedidas (Fase 3) y repetición de la campaña sobre el código endurecido. _(Si hay margen, inicio de la validación física opcional.)_ |
| 9       | 9 dic – 15 dic  | Conclusiones, evaluación del coste de rendimiento de las mitigaciones y entrega definitiva. **Límite: 15/12/26 23:59.**                                     |

---
## Plan de uso de IA
- **Herramientas empleadas:** LLM (asistentes de IA generativa: Gemini y Claude).
- **Tareas delegadas:** estructuración del análisis, redacción formal de la memoria y asistencia en los scripts de Python (automatización de GDB, barrido de inyección y análisis de resultados a CSV).
- **Tareas reservadas (humanas):** decisiones arquitectónicas, interpretación de resultados, diseño de las contramedidas, y —en la fase opcional— montaje del circuito, _flasheo_ y calibración del orquestador físico.
- **Método de verificación:** toda la teoría criptográfica y las directrices técnicas documentadas por la IA se cotejarán con los resúmenes oficiales de Black Hat y con papers como CARDIS 2023, para garantizar el rigor y evitar alucinaciones.