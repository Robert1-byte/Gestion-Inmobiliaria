# Matriz de trazabilidad

Vincula necesidades, reglas de negocio, historias de usuario, requisitos funcionales y no funcionales, prioridad, incremento y evidencia de verificación. Las definiciones de cada código están en `docs/catalogo-requisitos.md`. Las historias son las definidas por el equipo.

| Historia | Necesidad / regla | RF / RNF | Prioridad | Incremento | Módulo | Evidencia de verificación | Estado |
|---|---|---|---|---|---|---|---|
| HU-01 | ST-01, ST-05 / RN-01, RN-12, RN-13 | RF-01; RF-02; RF-03; RNF-02; RNF-04; RNF-05; RNF-06; RNF-08; RNF-10 | Alta | I1 | Propiedades / Plataforma | Prueba de alta, edición, filtro y despliegue Docker; revisión de esquema | Validada por caso y HU-01 |
| HU-02 | ST-01, ST-05 / RN-12, RN-13 | RF-28; RNF-04 | Alta | I1 | Propiedades | Prueba de cambio de modalidad conservando contratos y movimientos | Validada por HU-02 |
| HU-03 | ST-01, ST-04, ST-05 / RN-03, RN-04, RN-13 | RF-04; RF-05; RF-06; RF-07; RF-08; RF-09; RNF-01; RNF-02; RNF-03 | Alta | I1 | Contratos | Prueba de carga PDF, metadatos, rechazo de formato, acceso y estados | Validada por caso y HU-03 |
| HU-04 | ST-01, ST-02 / RN-03 | RF-10; RNF-06 | Media | I1 | Contratos | Prueba de alerta en panel con contrato próximo a vencer | Umbral sujeto a PD-04 |
| HU-05 | ST-01, ST-03, ST-05 / RN-05, RN-06, RN-13 | RF-11; RF-12; RF-13; RNF-02; RNF-04 | Media | I1 | Inventario | Prueba de registro, estados y recálculo de valor activo | Sujeta a PD-03 |
| HU-06 | ST-02, ST-05 / RN-07, RN-08, RN-13 | RF-14; RF-15; RF-16; RF-17; RF-29; RNF-01; RNF-02; RNF-03 | Alta | I1 | Finanzas | Prueba de movimientos con comprobante, filtros y flujo mensual | Validada por caso y HU-06 |
| HU-07 | ST-03 / RN-11, RN-13 | RF-18; RF-19; RNF-04 | Alta | I2 | Comisiones | Prueba de alta de informe y reservas sin API externa | Validada por HU-07 |
| HU-08 | ST-01, ST-02, ST-03 / RN-02, RN-13 | RF-20; RF-21; RNF-02; RNF-07 | Alta | I2 | Comisiones | Prueba con 20 %, 25 % y 30 % y revisión de liquidación | Base sujeta a PD-01 |
| HU-09 | ST-01, ST-02 / RN-09, RN-13 | RF-22; RF-23; RF-24; RF-27; RNF-08; RNF-09 | Alta | I2 | Reportes | Prueba de utilidad neta, gráficos y exportación PDF/CSV/XLSX | Validada por HU-09 |
| HU-10 | ST-01, ST-02 / RN-10 | RF-25; RF-26; RNF-04; RNF-07 | Alta | I2 | Reportes | Prueba negativa de edición, inserción y eliminación tras cierre | Autoridad sujeta a PD-05 |

## Cobertura

- 10 historias definidas por el equipo: 6 en I1 (HU-01 a HU-06) y 4 en I2 (HU-07 a HU-10).
- 29 requisitos funcionales: RF-01 a RF-17, RF-28 y RF-29 en I1; RF-18 a RF-27 en I2.
- 10 requisitos no funcionales: RNF-01 a RNF-06, RNF-08 y RNF-10 en I1; RNF-07, RNF-08 y RNF-09 en I2.
- Los seis módulos del caso están representados: propiedades, contratos, inventario, finanzas, comisiones y reportes. Además se incluye `Plataforma` como soporte del Incremento 1.
- Todas las historias tienen al menos un RF o RNF; ningún RF/RNF queda sin historia asociada.

## Decisiones pendientes reflejadas

| ID | Decisión | Afecta |
|---|---|---|
| PD-01 | Base exacta de cálculo de la comisión 20/25/30 % | RN-02, RF-20, RF-21, HU-08 |
| PD-03 | Criterio de equipamiento activo frente a ítems dañados | RN-06, RF-13, HU-05 |
| PD-04 | Días de anticipación de alertas de vencimiento | RN-03, RF-10, HU-04 |
| PD-05 | Responsable autorizado del cierre de mes | RN-10, RF-25, RF-26, HU-10 |

PD-02 quedó resuelta por HU-02: el cambio de modalidad está permitido y conserva el historial.

La versión de trabajo debe identificarse como **SRS v1.0 — para revisión** hasta la validación formal de la semana 2.
