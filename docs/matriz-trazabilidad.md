# Matriz de trazabilidad

Vincula necesidades, reglas de negocio, historias de usuario, requisitos funcionales y no funcionales, prioridad, incremento y evidencia de verificación. Las definiciones de cada código están en `docs/catalogo-requisitos.md`.

| Historia | Necesidad / regla | RF / RNF | Prioridad | Incremento | Módulo | Evidencia de verificación | Estado |
|---|---|---|---|---|---|---|---|
| HU-01 | ST-01, ST-05 / RN-01, RN-12, RN-13 | RF-01; RF-03; RNF-02; RNF-04; RNF-06 | Alta | I1 | Propiedades | Prueba de alta, edición y consulta; revisión de esquema | Modalidad sujeta a PD-02 |
| HU-02 | ST-01 / RN-01 | RF-02; RNF-08 | Alta | I1 | Propiedades | Prueba de filtro por modalidad | Validada por caso |
| HU-03 | ST-04, ST-05 / RN-03, RN-04, RN-13 | RF-04; RF-05; RF-06; RF-07; RNF-01; RNF-02 | Alta | I1 | Contratos | Prueba de carga PDF, rechazo de formato e inspección de volumen | Validada por caso |
| HU-04 | ST-01, ST-02 / RN-03 | RF-09; RF-10; RNF-06 | Media | I1 | Contratos | Prueba con contratos vigentes y vencidos | Alerta sujeta a PD-04 |
| HU-05 | ST-05 / RN-05, RN-13 | RF-11; RF-12; RNF-02 | Alta | I1 | Inventario | Prueba unitaria de valor por ítem | Validada por caso |
| HU-06 | ST-01, ST-03 / RN-06 | RF-13; RNF-04 | Media | I1 | Inventario | Prueba de valorización activa | Sujeta a PD-03 |
| HU-07 | ST-02, ST-05 / RN-07, RN-08, RN-13 | RF-14; RF-15; RNF-02; RNF-03 | Alta | I1 | Finanzas | Prueba de registro y revisión de FK | Validada por caso |
| HU-08 | ST-01, ST-02 / RN-07, RN-08 | RF-16; RF-17; RNF-08 | Media | I1 | Finanzas | Prueba de consulta por propiedad y periodo | Validada por caso |
| HU-09 | ST-02, ST-05 / RN-04 | RF-08; RNF-01; RNF-10 | Alta | I1 | Contratos | Prueba de acceso anónimo, por rol e inspección de Git | Validada por caso |
| HU-10 | ST-05 / RN-13 | RNF-05; RNF-10 | Media | I1 | Plataforma | Ejecución limpia de `docker compose up --build` | Validada por caso |
| HU-11 | ST-03 / RN-11 | RF-18; RF-19; RNF-04 | Alta | I2 | Comisiones | Prueba de alta de informe y reservas | Validada por caso |
| HU-12 | ST-02, ST-03 / RN-02, RN-13 | RF-20; RF-21; RNF-02; RNF-07 | Alta | I2 | Comisiones | Prueba con 20 %, 25 % y 30 % | Base sujeta a PD-01 |
| HU-13 | ST-01, ST-02 / RN-09, RN-13 | RF-22; RNF-02 | Alta | I2 | Reportes | Prueba de cálculo de utilidad neta | Validada por caso |
| HU-14 | ST-01, ST-02 / RN-09 | RF-24; RNF-09 | Alta | I2 | Reportes | Revisión de PDF y CSV/XLSX generados | Validada por caso |
| HU-15 | ST-01 / RN-09 | RF-23; RNF-08 | Media | I2 | Reportes | Prueba de consolidado con 10 propiedades | Validada por caso |
| HU-16 | ST-02 / RN-10 | RF-25; RF-26; RNF-07; RNF-04 | Alta | I2 | Reportes | Prueba negativa tras cierre y revisión de auditoría | Autoridad sujeta a PD-05 |
| HU-17 | ST-01 / RN-09 | RF-27; RNF-08 | Media | I2 | Reportes | Revisión de gráficos comparativos | Validada por caso |
| HU-18 | ST-02, ST-03 / RN-02 | RF-21; RNF-04 | Media | I2 | Comisiones | Prueba de consulta por administrador y mes | Base sujeta a PD-01 |

## Cobertura

- 18 historias: 10 en I1 y 8 en I2.
- 27 requisitos funcionales: RF-01 a RF-17 en I1; RF-18 a RF-27 en I2.
- 10 requisitos no funcionales: RNF-01 a RNF-06, RNF-08 y RNF-10 en I1; RNF-07, RNF-08 y RNF-09 en I2.
- Los seis módulos del caso están representados: propiedades, comisiones, contratos, inventario, finanzas y reportes. Además se incluye `Plataforma` como módulo de soporte del Incremento 1.
- Todas las historias tienen al menos un RF o RNF; ningún RF/RNF queda sin historia asociada.

## Decisiones pendientes reflejadas

| ID | Decisión | Afecta |
|---|---|---|
| PD-01 | Base exacta de cálculo de la comisión 20/25/30 % | RN-02, RF-20, RF-21, HU-12, HU-18 |
| PD-03 | Criterio de equipamiento activo frente a ítems dañados | RN-06, RF-13, HU-06 |
| PD-04 | Días de anticipación de alertas de vencimiento | RN-03, RF-10, HU-04 |
| PD-05 | Responsable autorizado del cierre de mes | RN-10, RF-25, RF-26, HU-16 |
| PD-02 | Regla para cambio de modalidad de propiedad | RN-12, RF-03, HU-01 |

La versión de trabajo debe identificarse como **SRS v1.0 — para revisión** hasta la validación formal de la semana 2.
