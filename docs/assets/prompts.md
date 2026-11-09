REGISTRO DE USO DE IA — Investigación PQC Fault Injection
Asignatura: Técnicas y Metodologías de Investigación en Ciberseguridad
Herramienta de IA: Claude (Anthropic)
(Todas las horas en hora de Madrid)

== 2026-11-07 ==
- Actualización de las fuentes del proyecto desde el repositorio GitHub (carpeta docs/).
  Guardadas en el proyecto las 2 fuentes (analisis de charlas + propuesta de investigacion).
- Definicion de estrategia para la entrega intermedia (10/11): priorizar la memoria escrita;
  objetivo tecnico realista para 3 dias = Fase 0 (harness de inyeccion sobre programa trivial).

== 2026-11-09 ==
- 13:18  Validado el borrador de la memoria (indice + secciones 1, 2, 6, 7, 8).
         Confirmado por el alumno: la primera entrega fue unicamente el markdown de analisis
         de charlas (sin emulacion); por tanto el giro a emulacion QEMU+GDB es un cambio
         introducido en esta entrega (seccion 1.5).
- 13:18  Inicio de la Fase 0: montaje de toolchain arm-none-eabi + QEMU + GDB y construccion
         del harness de inyeccion de fallos sobre un programa trivial bare-metal.
- 13:40  Fase 0 COMPLETADA en entorno de prueba. Toolchain operativo (QEMU 8.2.2,
         arm-none-eabi-gcc 13.2.1, gdb 15.1). Compilado fase0.elf para mps2-an386 (Cortex-M4).
         Ejecucion de referencia: firma invalida -> RECHAZADA (correcto).
         Harness inject.py, 2 tecnicas de fallo sobre el punto de decision de verify():
           * salto de instruccion (omitir el 'str' del veredicto de rechazo) -> ACEPTADA
           * corrupcion de registro (forzar verify() = 0)                     -> ACEPTADA
         Resultado: 2 de 2 tecnicas hacen que una firma invalida sea aceptada.
         Evidencias: results.csv, injection_log.txt.
         Tareas con IA: generacion del programa trivial, linker/startup y el harness en Python.
         Verificacion: ejecutado y comprobado el efecto real en el emulador.
- 14:50  Correccion del harness. En la maquina del alumno (QEMU/gdb de Arch) la fila 'baseline'
         salia '??': el metodo original capturaba la salida de texto de QEMU, fragil cuando hay
         un depurador conectado. Solucion: el veredicto se lee del registro r0 via GDB
         (printf $r0), determinista e independiente de la version de QEMU.
         Verificado en el sandbox: 8/8 ejecuciones estables (baseline RECHAZADA; ambas
         inyecciones ACEPTADA). Reempaquetado fase0.tar.gz.
- 14:55  Corregido tambien el modo manual (Makefile): nuevo target 'inject1' que se conecta a un
         'make gdbserver' ya lanzado (antes 'inject' chocaba en el puerto 1234). Probado: la
         Terminal 1 (QEMU) imprime 'FIRMA ACEPTADA' tras forzar la aceptacion. Reenviado el paquete.
- 18:00  El alumno ejecuta el harness corregido en su maquina y aporta 4 capturas de evidencia
         (tabla, results.csv, injection_log.txt, terminal con la ejecucion de referencia).
         Ensamblada la memoria intermedia completa: docs/memoria-entrega-intermedia.md
         (secciones 1-8 + anexos), con las 4 capturas en docs/assets/ (fig1-fig4).
         Tareas con IA: redaccion y maquetado del documento. Verificacion: contenido
         contrastado con la evidencia real de la Fase 0 aportada por el alumno.
