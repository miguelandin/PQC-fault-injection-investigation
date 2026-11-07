## Introducción
En este documento se analizarán varías charlas sobre el tema que he decidido investigar: **Como protegerse a la era post cuántica** para ser más exactos, a tu organización o empresa.

---
## Exploración
### Ficha 1
- **Título:** Post Quantum Panic: When Will the Cracking Begin, & Can We Detect it?
- **Autores:** Konstantinos Karagiannis
- **Evento:** DEF CON 33
- **Año:** 2025
- **Enlace:** [https://www.youtube.com/watch?v=OkVYJx1iLNs](https://www.youtube.com/watch?v=OkVYJx1iLNs)
- **Resumen:** Analiza los plazos reales para la llegada de ordenadores cuánticos (CRQCs) capaces de romper la criptografía actual y revisa los hitos de empresas como IBM o IonQ. Además, aborda cómo monitorizar patrones de circuitos en la nube para detectar intentos de ejecución de estos algoritmos.
- **Relación con la temática:** Es fundamental para "proteger a tu empresa" porque define la ventana de tiempo (5-10 años) que tienen las organizaciones para migrar y propone métodos de detección temprana.
### Ficha 2
- **Título:** Redefining the Origin of Secrecy in a Post-Quantum World
- **Autores:** Frey Wilson
- **Evento:**  Black Hat 2025
- **Año:** 2025
- **Enlace:** [https://www.youtube.com/watch?v=dxJfjcQPZo8](https://www.youtube.com/watch?v=dxJfjcQPZo8)
- **Resumen:** Explora métodos de generación y distribución de claves simétricas capaces de resistir ataques cuánticos, comparando enfoques matemáticos con fenómenos cuánticos. Presenta una estrategia alternativa que aprovecha la aleatoriedad finita para establecer un origen de secreto mutuo resiliente y escalable.
- **Relación con la temática:** Contribuye a la protección empresarial post-cuántica al abordar uno de los mayores retos técnicos: cómo generar y distribuir claves de forma segura y eficiente antes de que los adversarios puedan romperlas.
### Ficha 3
- **Título:** Post-Quantum Cryptography: A Realistic Guide to Manage the Transition
- **Autores:** Jean-Philippe Aumasson
- **Evento:** Black Hat Asia 2026
- **Año:** 2026
- **Enlace:** [https://www.youtube.com/watch?v=_M0https://www.youtube.com/watch?v=_M0of-6aLpkof-6aLpk](https://www.youtube.com/watch?v=_M0of-6aLpk)
- **Resumen:** El CSO de Taurus ofrece una hoja de ruta estratégica y técnica para la integración de criptografía post-cuántica (PQC), revisando estándares globales (NIST, etc.) y analizando modos de implementación híbridos. Aborda la creación de planes de migración interna, evaluación de riesgos y adaptación de software corporativo.
- **Relación con la temática:** Impacta directamente en el núcleo de la investigación, proporcionando una guía aplicable al entorno corporativo (desde VPNs hasta normativas) para gestionar el riesgo del "cosecha ahora, descifra después".
### Ficha 4
- **Título:** Bypassing PQC Signature Verification with Fault Injection
- **Autores:** Fikret Garipay
- **Evento:** Black Hat USA 2025
- **Año:** 2026 (Año de publicación)
- **Enlace:** [https://www.youtube.com/watch?v=JS30uepSuMo](https://www.youtube.com/watch?v=JS30uepSuMo)
- **Resumen:** Demuestra cómo las implementaciones modernas de algoritmos PQC (Dilithium, XMSS, SPHINCS+) en sistemas embebidos siguen siendo vulnerables a técnicas clásicas de inyección de fallos por voltaje (Fault Injection). Explica cómo falsificar firmas válidas en hardware real (microcontroladores) operando sobre metal desnudo.
- **Relación con la temática:** Muestra que elegir un algoritmo seguro para la empresa no es suficiente; la implementación física también debe protegerse contra ataques de hardware que eluden la teoría cuántica.
### Ficha 5
- **Título:** QRAMM: The Cryptographic Migration to a Post Quantum World
- **Autores:** Emily Fane, Abdel Sy Fane
- **Evento:** DEF CON 33
- **Año:** 2025
- **Enlace:** [https://www.youtube.com/watch?v=SfWOqqDZEWI](https://www.youtube.com/watch?v=SfWOqqDZEWI)
- **Resumen:** Presenta QRAMM (Quantum Readiness Assurance Maturity Model), un framework de código abierto diseñado para evaluar la madurez y preparación de las organizaciones ante la migración criptográfica. Se enfoca en cuatro dimensiones: visibilidad, protección de datos, implementación técnica y gobernanza.
- **Relación con la temática:** Es directamente aplicable a cómo proteger a una empresa, proporcionando a la investigación un modelo de madurez auditable para planificar la agilidad criptográfica a nivel corporativo.
### Ficha 6
- **Título:** The Hacker's Guide To PQC
- **Autores:** Konstantinos Karagiannis
- **Evento:** DEF CON 32
- **Año:** 2024
- **Enlace:** [https://www.youtube.com/watch?v=l2G38ECvJF8](https://www.youtube.com/watch?v=l2G38ECvJF8)
- **Resumen:** Analiza la urgencia de adoptar la agilidad criptográfica frente a la estandarización del NIST. A través de un enfoque práctico, explora las matemáticas detrás de los criptosistemas basados en retículos y disecciona ataques reales ejecutados contra los algoritmos finalistas del NIST.
- **Relación con la temática:** Aporta un enfoque práctico indispensable para la protección corporativa, mostrando no solo la teoría defensiva, sino las vulnerabilidades reales (ataques) que las empresas deben mitigar al implementar el nuevo estándar.

---
## Selección
Se han seleccionado las charlas _"Post-Quantum Cryptography: A Realistic Guide to Manage the Transition"_ y _"Bypassing PQC Signature Verification with Fault Injection"_ (charlas 3 y 4) por los siguientes motivos:

- **Relevancia para la investigación personal:** Ambas cubren de forma integral el objetivo de "cómo proteger a la empresa". La charla 3 aporta la visión de gestión estratégica (gobernanza, planes de migración) y la charla 4 aporta la visión táctica defensiva (implementación física segura en hardware).

- **Calidad técnica:** Ambas provienen de Black Hat (Asia y USA), uno de los congresos de mayor prestigio técnico a nivel mundial.

- **Replicabilidad:** La investigación de la charla 4 es altamente replicable, ya que utiliza hardware real (microcontroladores) y librerías PQC de código abierto operando en _bare metal_. La charla 3, por su parte, ofrece hojas de ruta corporativas basadas en normativas públicas (NIST, FIPS 205) que pueden aplicarse sistemáticamente a cualquier organización.

- **Aportación metodológica:** La combinación de ambas charlas permite construir una metodología híbrida propia para la investigación: una auditoría de alto nivel basada en estándares (charla 3) complementada con pruebas de resistencia en la implementación de los algoritmos (charla 4).

---

## Análisis en detalle: Post-Quantum Cryptography: A Realistic Guide to Manage the Transition

### 1. Introducción
- **Título:** Post-Quantum Cryptography: A Realistic Guide to Manage the Transition.
- **Autor:** Jean-Philippe (JP) Aumasson (Criptógrafo con más de 20 años de experiencia, ponente en Black Hat durante 13 años, coautor de BLAKE2/3, SipHash y del estándar post-cuántico SLH-DSA).
- **Evento y Año:** Black Hat Asia 2026.
- **Enlace:** [https://www.youtube.com/watch?v=_M0of-6aLpk](https://www.youtube.com/watch?v=_M0of-6aLpk)
- **Motivo de elección:** Esta charla es la base estratégica perfecta para la investigación, ya que aborda directamente cómo proteger a tu empresa en la era post-cuántica, ofreciendo pautas normativas y de gestión de riesgos corporativos reales.

### 2. Ámbito
Es una charla de ámbito estrictamente **Defensivo** y de **Gobernanza/Gestión de riesgos**. Se centra en la preparación corporativa y la arquitectura de sistemas seguros.

### 3. Temática
Criptografía, Gestión de Riesgos Corporativos, Estándares de Seguridad (NIST, IETF) y Arquitectura de Sistemas.

### 4. Origen de la investigación
Se basa en la **experiencia profesional** del ponente. JP Aumasson lleva manejando riesgos cuánticos desde 2018 y es coautor de algoritmos (SLH-DSA) que actualmente se están estandarizando.

### 5. Permiso del objetivo y gestión
**No aplica (N/A).** Al ser una charla de naturaleza defensiva y estratégica sobre estándares criptográficos y planes de migración, no se realizan ataques contra infraestructuras de terceros que requieran autorización.

### 6. Demostraciones
El autor no realiza demostraciones de _hacking_ en vivo ni pruebas de concepto de código. Las "demostraciones" consisten en la exposición de marcos de trabajo (KMS de AWS/Google, implementación en iOS 26 y Android 17). La metodología de auditoría propuesta es altamente **replicable** a nivel organizativo para cualquier empresa.

### 7. Descripción general de la temática analizada
La charla desmiente mitos comunes: los ordenadores cuánticos no son más rápidos en ciclos de reloj ni prueban todas las contraseñas a la vez. Simplemente, resuelven problemas matemáticos muy específicos (factorización y logaritmo discreto). Esto significa que algoritmos de clave pública como RSA o curvas elípticas (ECC) están rotos, comprometiendo infraestructuras críticas como PKI (certificados), autenticación (SSH), blockchain y protocolos de intercambio de claves (TLS, PGP). El principal peligro actual es el "Cosecha ahora, descifra después" (guardar tráfico cifrado hoy para revelarlo en 20-30 años). La criptografía simétrica, por otro lado, no se ve gravemente afectada.

### 8. Análisis técnico de la investigación
Técnicamente, los ordenadores cuánticos actuales son "juguetes" limitados a unos 100 qubits ruidosos, y el gran reto es la corrección de errores. El autor estima el _Q-Day_ (fecha en que la computación cuántica romperá la criptografía actual) alrededor del año 2040.

Para mitigar esto, el NIST ha creado competiciones que han resultado en estándares como ML-KEM y ML-DSA (cuyas velocidades son comparables a SLH-DSA, aunque mucho más lentas que ECDSA). El autor destaca la tendencia actual del mercado: adopción de encriptación híbrida en APIs de AWS/Google, preparación en Apple (iMessage, VPN) y soporte futuro en Cloudflare.

### 9. Metodología de investigación propuesta por el alumno
A partir de la hoja de ruta expuesta por Aumasson, la metodología propia a aplicar en la investigación para proteger a las empresas se estructurará en las siguientes cuatro fases continuas e iterativas:

- **Fase 1 - Observación (Inventario pasivo):** Mapear dónde y cómo se utiliza la criptografía en la empresa, evaluando su nivel de criticidad. En esta fase no se realizan cambios, pero se auditan estrictamente las dependencias de terceros (_third parties_).

- **Fase 2 - Evaluación (Triaje de riesgos):** Priorizar los sistemas que presentan mayor riesgo y coste de adaptación. Se utilizará una matriz de riesgo basada en: sensibilidad de los datos, tiempo de vida útil de la información (_lifetime_) y nivel de exposición fuera de la organización.

- **Fase 3 - Decisión (Diseño de la estrategia):** Documentar formalmente los requisitos y los cambios técnicos a implementar (p. ej., pasar a esquemas PQC híbridos). Es fundamental comunicar las políticas de migración a los proveedores externos.

- **Fase 4 - Acción y Monitoreo (Implementación):** Integrar librerías de código abierto verificadas (preferiblemente en Rust) o APIs corporativas que soporten PQC híbrido, vigilando continuamente las actualizaciones de las guías de migración regionales y del NIST/IETF.

### 10. Uso de IA en la investigación analizada y posibles aplicaciones
El autor no menciona el uso de herramientas de IA en su investigación. Sin embargo, para aplicarlo al entorno corporativo, la IA generativa y los LLMs podrían utilizarse para el **descubrimiento automatizado de criptografía heredada (Legacy Crypto Discovery)**: analizando repositorios de código fuente estático para detectar llamadas a funciones RSA o ECC desactualizadas y sugiriendo la refactorización automática hacia librerías PQC modernas soportadas por el NIST.

### 11. Aspectos de mejora
- **Organización y presentación:** Aunque la estructura es sólida, la cantidad de estándares e iniciativas globales mencionadas (NIST, iniciativas de China, directrices regionales) podría condensarse en una tabla visual durante la presentación para facilitar la comprensión.

- **Técnicos:** La charla cubre muy bien el qué y el porqué, pero el plan de migración ("Decidir y Actuar") adolece de falta de ejemplos técnicos sobre cómo refactorizar sistemas heredados (_legacy systems_) monolíticos, donde simplemente actualizar una librería o migrar a algoritmos con firmas de mayor tamaño (como ML-DSA) podría romper la disponibilidad del servicio.

---
## Análisis en detalle: Bypassing PQC Signature Verification with Fault Injection
### 1. Introducción
- **Título:** Bypassing PQC Signature Verification with Fault Injection.
- **Autor:** Fikret Garipay (Ingeniero de seguridad en Keysight Device Security Testing, especializado en explotación de software y ataques de hardware).
- **Evento y Año:** Black Hat USA 2025.
- **Enlace:** [https://www.youtube.com/watch?v=JS30uepSuMo](https://www.youtube.com/watch?v=JS30uepSuMo)
- **Motivo de elección:** Esta investigación resulta fundamental para abordar la protección empresarial post-cuántica, ya que demuestra que la adopción de estándares criptográficos robustos no es suficiente si la implementación física en el hardware corporativo sigue siendo vulnerable.

### 2. Ámbito 
Se sitúa en un ámbito puramente **Ofensivo** (Hardware Hacking / Fault Injection). Se centra en vulnerar los procesos de verificación de firmas en dispositivos físicos.

### 3.Temática
Seguridad Hardware, Criptografía Post-Cuántica (PQC), Sistemas Embebidos (_Secure Boot_) y Ataques de Inyección de Fallos.

### 4.Origen de la investigación
La investigación tiene su origen en la experimentación y el descubrimiento técnico, apoyándose en la literatura académica previa (como el _paper_ "Fault Attacks Sensitivity of Public Parameters in the Dilithium Verification" de CARDIS 2023).

### 5. Permiso del objetivo y cómo se gestionó
**Sí, autorizado.** El investigador no atacó infraestructuras de terceros. Se diseñó un entorno de laboratorio controlado importando librerías PQC de código abierto (pqm4, XMSS y SPHINCS+) a _firmware bare-metal_ en placas de desarrollo comerciales propias (ARM Cortex-M y STM32F417).

### 6. Demostraciones
Las demostraciones son pruebas de concepto de inyección de fallos por voltaje (_Voltage Fault Injection_) sobre hardware real. Son **altamente replicables** en un entorno de laboratorio que cuente con equipamiento adecuado para manipular los niveles de voltaje de los microcontroladores durante la ejecución de ciclos de reloj específicos.

### 7. Descripción general de la temática analizada
Aunque los ordenadores cuánticos aún no están rompiendo la criptografía actual, la industria ya está en transición hacia el estándar PQC para reemplazar esquemas como RSA o ECC en funciones críticas como el _Secure Boot_ (arranque seguro) en chips como OpenTitan o Caliptra 2.0. Esta transición genera una nueva superficie de ataque. La charla demuestra que algoritmos inherentemente resistentes a los ataques cuánticos siguen siendo susceptibles a ataques físicos clásicos si el _hardware_ que los ejecuta puede ser manipulado.

### 8. Análisis técnico de la investigación
El ataque principal se basa en la Inyección de Fallos por Voltaje (_Voltage Fault Injection_), que consiste en reducir el suministro eléctrico en el microsegundo exacto para provocar que la CPU omita instrucciones o cometa errores computacionales.

En el caso del algoritmo Dilithium, el ataque busca saltarse el fragmento de código responsable de un cálculo crítico durante la verificación (la ecuación de `Az - ct_1 2^d`). Al omitir mediante _glitching_ la instrucción que calcula `ct_1 2^d`, el valor se convierte en cero, permitiendo al atacante eludir el componente de clave privada y validar una firma falsificada. Técnicas similares se aplicaron contra firmas basadas en hashes (WOTS+ usadas en XMSS y SPHINCS+), saltando pasos críticos del _checksum_.

### 9. Metodología de investigación propuesta en base a lo aprendido
A partir de los hallazgos técnicos expuestos, la metodología propia a desarrollar para la investigación sobre protección corporativa incorpora un enfoque híbrido que evalúa tanto la teoría matemática como la ejecución física. Se propone la siguiente metodología estructurada en tres fases:

- **Fase 1 - Despliegue de Entornos de Control:** Implementación de algoritmos PQC seleccionados sobre _hardware bare-metal_ que replique la arquitectura de los dispositivos IoT o de borde (_Edge_) empleados en la empresa.

- **Fase 2 - Auditoría de Superficie Física (Stress Testing):** Sometimiento del hardware a pruebas de _Fault Injection_ (glitching de voltaje y reloj) durante operaciones criptográficas críticas (como el _Secure Boot_) para medir la sensibilidad de los parámetros públicos de la verificación.

- **Fase 3 - Diseño de Contramedidas (_Hardening_):** Desarrollo de estrategias de mitigación en el código, tales como la redundancia de cálculos críticos o la validación aleatoria en la ejecución de instrucciones, para asegurar que un salto de instrucción forzado por hardware sea detectado antes de validar la firma.

### 10. Uso de IA en la investigación analizada y posibles aplicaciones
No se indica el uso explícito de Inteligencia Artificial por parte del autor. Sin embargo, en posibles aplicaciones futuras, la IA y los modelos de _Machine Learning_ podrían integrarse para analizar las trazas de energía (_Power Analysis_) en tiempo real e identificar de manera automatizada los ciclos de reloj exactos en los que el microcontrolador realiza el cálculo de `ct_1 2^d`, reduciendo significativamente el tiempo de perfilado necesario para ejecutar el ataque de inyección de fallos con éxito.

### 11. Aspectos de mejora
- **Técnicos:** Tal y como reconoce la propia investigación, el comportamiento de los ataques por inyección de fallos varía significativamente dependiendo de la implementación específica del fabricante del chip y el compilador utilizado. Esto dificulta enormemente la generalización del ataque o el desarrollo de una herramienta de explotación universal.

- **Presentación:** Sería beneficioso incluir esquemas visuales de la arquitectura del microcontrolador analizado o fragmentos en lenguaje ensamblador (_Assembly_) durante la presentación para ilustrar con mayor claridad el momento exacto en el que la instrucción es omitida por el fallo de voltaje.

---
## Uso de IA
- **Herramientas empleadas:** LLM (Asistente de IA generativa), Gemini.

- **Tareas delegadas a la IA:** Redacción formal de las fichas de los vídeos y estructuración del análisis detallado en base a los apuntes técnicos extraídos manualmente y la rúbrica académica exigida.

- **Tareas realizadas sin IA (Humanas):** Búsqueda de conferencias en plataformas de vídeo, filtrado de charlas por congresos relevantes, visionado de los vídeos (Fikret Garipay y JP Aumasson) y extracción de las notas técnicas, métricas y conclusiones de los ponentes.

- **Método de verificación:** Toda la información técnica redactada por la IA fue cotejada directamente contra los resúmenes oficiales de Black Hat y DEF CON y de los apuntes del investigador, asegurando que no se han introducido alucinaciones algorítmicas ni referencias inventadas.