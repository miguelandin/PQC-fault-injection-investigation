# Memoria — Entrega intermedia
**Asignatura:** Técnicas y Metodologías de Investigación en Ciberseguridad
**Autor:** Miguel Cabrera Landín
**Fecha:** 10/11/2026
**Título de la investigación:** Inyección de fallos sobre la verificación de firmas PQC en emulación y diseño de contramedidas de software

---

## Índice
1. Enfoque final de la investigación
   1.1 Pregunta de investigación
   1.2 Hipótesis de partida
   1.3 Objetivos
   1.4 Alcance
   1.5 Cambios respecto a la primera entrega y su motivo
2. Proceso de obtención de información y fuentes
   2.1 Método de búsqueda y selección
   2.2 Fuentes seleccionadas
   2.3 Fuentes descartadas y motivo
3. Entorno de trabajo montado
4. Acciones realizadas
5. Resultados obtenidos y evidencias
6. Problemas encontrados y decisiones tomadas
7. Grado de cumplimiento de hitos y planificación actualizada
8. Uso de IA
   Anexos

---

## 1. Enfoque final de la investigación

### 1.1 Pregunta de investigación
¿Es la verificación de firmas de algoritmos post-cuánticos (PQC) sobre
arquitecturas embebidas (ARM Cortex-M4) vulnerable a inyección de fallos
durante el cálculo crítico, y qué contramedidas a nivel de código (C)
neutralizan ese riesgo con un coste de rendimiento asumible?

### 1.2 Hipótesis de partida
La robustez *algorítmica* de un esquema PQC no garantiza la robustez de su
*implementación*: un único fallo (salto de instrucción o alteración de un
registro) en el punto de verificación puede llevar a aceptar una firma
inválida, con independencia de la fortaleza matemática del esquema.

### 1.3 Objetivos
- **O1.** Desplegar una implementación PQC de referencia (Dilithium / ML-DSA)
  sobre firmware *bare-metal* ejecutándose en un Cortex-M4 emulado.
- **O2.** Construir un *harness* de inyección de fallos determinista y
  reproducible (QEMU + script de GDB) capaz de forzar un fallo en una
  instrucción concreta de la verificación.
- **O3.** Caracterizar el modelo de fallo explotable: qué instrucciones o
  registros, al alterarse, provocan la aceptación de una firma inválida, y con
  qué frecuencia.
- **O4.** Diseñar e implementar contramedidas de software (*hardening*) y medir
  su eficacia comparando la tasa de éxito antes y después.
- **O5.** Cuantificar el coste de rendimiento (ciclos y tamaño de código) de
  cada contramedida.
- **O6 (opcional).** Validar uno de los fallos sobre hardware físico real
  (STM32F407) mediante inyección por voltaje, si el calendario lo permite.

### 1.4 Alcance
**Dentro:** implementación en C de Dilithium (ML-DSA) de la librería `pqm4` y
su resistencia a fallos inyectados en las operaciones críticas de la
verificación (el bloque que decide aceptar o rechazar la firma).

**Fuera:** vulnerabilidades teóricas/matemáticas del algoritmo; despliegue
corporativo a gran escala; ataques de canal lateral (SPA/DPA, EM) y por láser;
un segundo algoritmo (SPHINCS+/XMSS), descartado para mantener el proyecto
abarcable.

### 1.5 Cambios respecto a la primera entrega y su motivo
1. **De ataque físico a emulación determinista.** El planteamiento inicial
   ejecutaba la inyección por voltaje sobre hardware real con un montaje casero
   (crowbar + Raspberry Pi Pico). Se reorienta a un entorno de emulación
   **QEMU + GDB**. *Motivo:* el ataque físico casero tiene un *jitter* elevado
   y una ventana explotable de nanosegundos, lo que produce muchos *resets* y
   pocos fallos útiles, con alto riesgo de agotar el calendario sin resultados.
   La emulación aporta determinismo (se inyecta el fallo en la instrucción
   exacta), reproducibilidad por terceros y coste cero, y desacopla el valor
   del proyecto —la Fase 3 de *hardening*— de que un ataque físico casero
   llegue a funcionar.
2. **Acotación a Dilithium.** Se descartan SPHINCS+/XMSS para concentrar el
   esfuerzo en un único esquema y garantizar profundidad en el plazo.
3. **La validación física pasa a ser objetivo opcional (O6)**, no el núcleo.

---

## 2. Proceso de obtención de información y fuentes

### 2.1 Método de búsqueda y selección
Búsqueda de charlas técnicas en congresos de referencia (Black Hat, DEF CON) a
través de plataformas de vídeo, filtrando por relevancia respecto al objetivo
("cómo proteger a una organización en la era post-cuántica"). Se fichó un
conjunto inicial de 6 charlas y se seleccionaron 2 por su complementariedad:
una de enfoque estratégico/gobernanza y otra de enfoque ofensivo/hardware. A
partir de la segunda se incorporó su base académica (paper de CARDIS 2023) y la
librería de referencia de implementación.

### 2.2 Fuentes seleccionadas
- Aumasson, J.-P. *Post-Quantum Cryptography: A Realistic Guide to Manage the
  Transition.* Black Hat Asia 2026. — Base estratégica (gobernanza, migración).
- Garipay, F. *Bypassing PQC Signature Verification with Fault Injection.*
  Black Hat USA 2025. — Base táctica (resistencia de la implementación a fallos).
- Andersson Calle Viera, Alexandre Berzati y Karine Heydemann, [*Fault Attacks Sensitivity of Public Parameters in the Dilithium Verification.* CARDIS 2023](https://eprint.iacr.asia/2023/1796.pdf). — Fuente técnica que documenta la sensibilidad de la verificación de Dilithium frente a fallos.
- Librería **`pqm4`** — implementación de referencia de Dilithium/ML-DSA para
  Cortex-M4.

### 2.3 Fuentes descartadas y motivo
Las otras 4 charlas fichadas (plazos de llegada de CRQCs y detección temprana;
distribución de claves resistente a cuántico; modelo de madurez QRAMM; guía
práctica de agilidad criptográfica) se descartan del análisis en profundidad:
aportan contexto valioso sobre el panorama post-cuántico, pero no sostienen ni
la línea estratégica de migración ni la táctica de inyección de fallos sobre la
que se construye la investigación. Se citan como contexto, no se analizan.

---

## 3. Entorno de trabajo montado
*[Pendiente — se completa tras Fase 0: versiones de arm-none-eabi-gcc, QEMU,
GDB; máquina Cortex-M4 emulada (mps2-an386 / netduinoplus2); accesos. Hardware
O6: STM32F407 Discovery, pendiente de adquirir.]*

## 4. Acciones realizadas
*[Pendiente — se documenta la Fase 0 de forma reproducible: comandos de
compilación, arranque en QEMU, conexión GDB y guion de inyección.]*

## 5. Resultados obtenidos y evidencias
*[Pendiente — capturas, logs y scripts del harness; resultados parciales y
negativos incluidos.]*

---

## 6. Problemas encontrados y decisiones tomadas
- **P1. Baja eficiencia del ataque físico casero.** El montaje crowbar + Pico
  presentaba jitter alto y ventana de nanosegundos → muchos resets y pocos
  fallos útiles. **Decisión:** migrar a emulación determinista (QEMU + GDB) y
  relegar la validación física a O6 opcional.
- **P2. Alcance inicial inabarcable.** Abordar Dilithium y además SPHINCS+/XMSS
  excedía el plazo. **Decisión:** concentrar todo el esfuerzo en Dilithium.
- *[P3…: problemas técnicos del montaje de Fase 0 — se añaden según surjan
  (selección de máquina QEMU, símbolos de depuración, breakpoints, etc.).]*

---

## 7. Grado de cumplimiento de hitos y planificación actualizada

### Cumplimiento del plan inicial
| Semanas             | Plan original                                              | Estado real                  |
| ------------------- | ---------------------------------------------------------- | ---------------------------- |
| 1–2 (13–27 oct)     | Toolchain + QEMU + GDB; harness Fase 0; compilar Dilithium | No iniciado                  |
| 3–4 (28 oct–10 nov) | Verificación de Dilithium en QEMU; script de inyección     | En curso (arranque 7–10 nov) |

Se reconoce un retraso respecto al cronograma inicial. El trabajo técnico se
concentra a partir del 7 de noviembre, priorizando la Fase 0 por ser la base
reproducible de todo lo demás.

### Planificación actualizada hasta la entrega final (15/12)
| Semana | Fechas | Hito |
| --- | --- | --- |
| — | 7–10 nov | Fase 0 (harness sobre programa trivial) + memoria intermedia. *Stretch:* iniciar compilación de Dilithium |
| 1 | 11–17 nov | Fase 1: Dilithium en QEMU (casos válido/inválido) e inicio de la automatización de la inyección |
| 2 | 18–24 nov | Fase 2: campaña de inyección sobre el bloque de verificación; mapa de puntos explotables |
| 3 | 25 nov–1 dic | Fase 3: diseño e implementación de contramedidas (*hardening*) |
| 4 | 2–8 dic | Repetición de la campaña sobre el código endurecido; métricas de eficacia y coste |
| 5 | 9–15 dic | Conclusiones, memoria final y presentación. **Entrega: 15/12 23:59** |

---

## 8. Uso de IA

**Tareas realizadas con IA (hasta el momento):** estructuración del análisis y
de la memoria, redacción formal, propuesta de los scripts de GDB/Python del
*harness* y asistencia en el montaje del *toolchain*. Herramientas: Gemini y
Claude.

**Tareas realizadas sin IA (humanas):** búsqueda y visionado de las charlas,
extracción de notas técnicas, decisiones arquitectónicas, interpretación de
resultados y diseño de las contramedidas.

**Verificación aplicada:** la teoría criptográfica y las directrices técnicas
redactadas con IA se cotejan con los resúmenes oficiales de Black Hat/DEF CON y
con el paper de CARDIS 2023; cada script se valida ejecutándolo y comprobando
su efecto real en el emulador.

**Registro de uso:** se adjunta en anexo (sesiones y prompts relevantes).

---

## Anexos
- A1. Scripts del harness de inyección (GDB/Python).
- A2. Logs y capturas de ejecución.
- A3. Registro de uso de IA.
