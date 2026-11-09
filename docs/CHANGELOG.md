# Changelog

Registro de avances de la investigación *Inyección de fallos sobre la
verificación de firmas PQC en emulación y diseño de contramedidas de software*.
Formato basado en [Keep a Changelog](https://keepachangelog.com/es-ES/).

## [2026-11-09]
### Añadido
- **Fase 0 completada:** *harness* de inyección de fallos determinista y
  reproducible (QEMU + GDB) sobre un objetivo de control *bare-metal*
  (Cortex-M4). Dos técnicas —salto de instrucción y corrupción de registro—
  consiguen que una firma inválida sea aceptada (2 de 2).
- Memoria de la entrega intermedia (secciones 1–8) con evidencias: capturas,
  `results.csv` e `injection_log.txt`, en `docs/` y `docs/assets/`.
- Paper de referencia **CARDIS 2023** (IACR ePrint 2023/1796) incorporado como
  fuente técnica.
- Registro de uso de IA (`prompts.md`).
### Cambiado
- El *harness* lee el veredicto directamente del registro `r0` vía GDB (en lugar
  de la salida de texto de QEMU), para resultados deterministas e independientes
  de la versión de QEMU.
### Eliminado
- Fichero de memoria duplicado (`Memoria intermedia.md`).

## [2026-11-07]
### Añadido
- Documento de análisis de conferencias (primera entrega).
- Documento de propuesta: inyección de fallos sobre la verificación de firmas
  PQC en emulación y diseño de contramedidas.
- Estructura inicial del repositorio (README, licencia).
### Cambiado
- Reorientación del enfoque: de inyección física por voltaje (montaje casero)
  a **emulación determinista QEMU + GDB**; alcance acotado a **Dilithium**
  (se descartan SPHINCS+/XMSS). Validación física relegada a objetivo opcional.
