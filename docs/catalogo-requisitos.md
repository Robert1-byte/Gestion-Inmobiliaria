# Catálogo de trazabilidad: stakeholders, reglas, historias y requisitos

Documento de apoyo para la Actividad 1. Los códigos se usan en `docs/matriz-trazabilidad.md` y `docs/matriz-trazabilidad.csv`. Las decisiones marcadas como pendientes no se inventan: se registran como `PD-XX` para la entrevista con el cliente.

## 1. Partes interesadas

| ID | Stakeholder | Necesidad principal | Fuente |
|---|---|---|---|
| ST-01 | Propietario / Product Owner | Conocer utilidad neta por propiedad y estado del portafolio | Caso §1 y §2.6 |
| ST-02 | Contador | Consolidar ingresos, egresos, comisiones y cierres mensuales | Caso §1 y §2.5 |
| ST-03 | Administrador externo de renta temporal | Registrar informes de reservas y recibir liquidación de comisión | Caso §2.1 y §2.2 |
| ST-04 | Inquilino de renta tradicional | Tener contrato vigente y comprobantes de su arrendamiento | Caso §2.1 y §2.3 |
| ST-05 | Operador interno | Cargar propiedades, contratos, inventario y movimientos desde cero | Caso §1 y guía §4 |

## 2. Reglas de negocio

| ID | Regla | Fuente | Estado |
|---|---|---|---|
| RN-01 | El portafolio opera bajo dos modalidades: renta tradicional y renta temporal. | Caso §2.1 | Confirmada |
| RN-02 | La comisión del administrador externo es 20 %, 25 % o 30 % según lo pactado. | Caso §2.2 | Porcentaje confirmado; base pendiente PD-01 |
| RN-03 | Los contratos notarizados se registran en PDF con firma, inicio, vencimiento, garantía y estado. | Caso §2.3 | Confirmada; alertas pendientes PD-04 |
| RN-04 | Los PDF se almacenan fuera de la base de datos y el acceso es autenticado. | Caso §2.3 y §3 | Confirmada |
| RN-05 | El inventario valorizado aplica a propiedades de renta temporal. | Caso §2.4 | Confirmada |
| RN-06 | El valor activo del inventario excluye los estados Dañado y Dado de baja. | Caso §2.4 | Pendiente PD-03 |
| RN-07 | Los ingresos incluyen cánones de renta tradicional e ingresos por reservas reportadas. | Caso §2.5 | Confirmada |
| RN-08 | Los egresos incluyen luz, agua, alícuotas, internet, mantenimiento y reposición. | Caso §2.5 | Confirmada |
| RN-09 | Utilidad neta = ingresos brutos − comisión del administrador − egresos operativos. | Caso §2.6 | Confirmada |
| RN-10 | El cierre de mes impide alteraciones posteriores a la liquidación. | Caso §2.6 | Confirmada; autoridad pendiente PD-05 |
| RN-11 | No se integra vía API con Airbnb/Booking; se registran informes mensuales. | Caso §2.1 y guía §3 | Confirmada |
| RN-12 | No hay regla definida para cambiar una propiedad de modalidad. | Guía §4, paso 2 | Pendiente PD-02 |
| RN-13 | Los importes usan tipos numéricos exactos. | Caso §3 | Confirmada |

## 3. Historias de usuario

| ID | Historia | Criterio de aceptación (Dado / Cuando / Entonces) | Prioridad | Incremento | Módulo |
|---|---|---|---|---|---|
| HU-01 | Como operador interno, quiero registrar una propiedad con sus datos principales, para iniciar el portafolio. | Dado que no existe la propiedad; cuando registro código, dirección, tipo y modalidad; entonces queda disponible en el portafolio. | Alta | I1 | Propiedades |
| HU-02 | Como propietario, quiero filtrar propiedades por modalidad, para revisar renta tradicional y temporal por separado. | Dado que existen propiedades; cuando filtro por modalidad; entonces solo se muestran las de esa modalidad. | Alta | I1 | Propiedades |
| HU-03 | Como operador interno, quiero registrar un contrato y adjuntar su PDF notarizado, para digitalizar la documentación. | Dado un contrato vigente; cuando adjunto un PDF válido; entonces el sistema guarda el archivo fuera de la BD y sus metadatos. | Alta | I1 | Contratos |
| HU-04 | Como contador, quiero consultar vigencias y alertas de contratos, para anticipar vencimientos. | Dado que existen contratos con fechas; cuando consulto alertas; entonces se listan los próximos a vencimiento. | Media | I1 | Contratos |
| HU-05 | Como operador interno, quiero registrar bienes del inventario de una propiedad temporal, para valorizar el equipamiento. | Dada una propiedad temporal; cuando registro descripción, cantidad y precio; entonces el sistema calcula el valor del ítem. | Alta | I1 | Inventario |
| HU-06 | Como propietario, quiero consultar el valor del equipamiento activo por propiedad, para respaldar actas de entrega. | Dado el inventario registrado; cuando consulto la valorización; entonces se muestra el total de ítems activos. | Media | I1 | Inventario |
| HU-07 | Como operador interno, quiero registrar ingresos y egresos por propiedad, para ordenar el flujo de caja mensual. | Dada una propiedad y un mes; cuando registro un movimiento con categoría y monto; entonces queda trazable. | Alta | I1 | Finanzas |
| HU-08 | Como contador, quiero consultar el flujo mensual por propiedad, para revisar ingresos y egresos antes del cierre. | Dado que existen movimientos; cuando consulto propiedad y periodo; entonces se muestran ingresos, egresos y saldo operativo. | Media | I1 | Finanzas |
| HU-09 | Como contador, quiero acceder a los contratos PDF con mi usuario, para revisarlos sin exponerlos públicamente. | Dado un usuario sin autenticar; cuando intenta abrir un PDF; entonces el sistema deniega el acceso. | Alta | I1 | Contratos |
| HU-10 | Como equipo de desarrollo, quiero un entorno Docker reproducible, para demostrar el Incremento 1 con PostgreSQL. | Dado un equipo limpio; cuando ejecuto `docker compose up`; entonces levantan backend, BD y volumen de contratos. | Media | I1 | Plataforma |
| HU-11 | Como administrador externo, quiero registrar mi informe mensual de reservas, para reportar renta temporal. | Dado un mes; cuando registro reservas con plataforma y monto; entonces el informe queda listo para validación. | Alta | I2 | Comisiones |
| HU-12 | Como contador, quiero calcular automáticamente la comisión pactada, para liquidar a cada administrador. | Dado un informe validado; cuando se calcula; entonces se aplica 20 %, 25 % o 30 % sobre la base acordada. | Alta | I2 | Comisiones |
| HU-13 | Como propietario, quiero ver la utilidad neta mensual por propiedad, para evaluar rentabilidad real. | Dados ingresos, comisión y egresos de un mes; cuando consulto la propiedad; entonces se muestra la utilidad neta. | Alta | I2 | Reportes |
| HU-14 | Como contador, quiero exportar reportes a PDF y Excel/CSV, para entregar la liquidación formal. | Dado un reporte generado; cuando selecciono formato; entonces se descarga el archivo con totales. | Alta | I2 | Reportes |
| HU-15 | Como propietario, quiero un consolidado general del portafolio, para comparar el desempeño de todas las propiedades. | Dado que existen reportes mensuales; cuando consulto el consolidado; entonces se muestra el total del portafolio. | Media | I2 | Reportes |
| HU-16 | Como contador, quiero cerrar un mes por propiedad, para evitar cambios posteriores a la liquidación. | Dado un mes liquidado; cuando registro el cierre; entonces los movimientos e informes de ese periodo quedan bloqueados. | Alta | I2 | Reportes |
| HU-17 | Como propietario, quiero gráficos comparativos por propiedad y mes, para visualizar tendencias. | Dado que existen datos de varios meses; cuando abro el panel; entonces se muestran comparativas. | Media | I2 | Reportes |
| HU-18 | Como administrador externo, quiero consultar mis liquidaciones por mes, para verificar el pago de mi comisión. | Dado que existe una liquidación aprobada; cuando consulto mi administrador y mes; entonces veo base, porcentaje y monto. | Media | I2 | Comisiones |

## 4. Requisitos funcionales

| ID | Requisito | Verificación | Incremento |
|---|---|---|---|
| RF-01 | El sistema deberá registrar una propiedad con código, dirección, tipo y modalidad. | Prueba de alta y consulta | I1 |
| RF-02 | El sistema deberá listar propiedades filtrando por modalidad de renta. | Prueba de filtro | I1 |
| RF-03 | El sistema deberá actualizar los datos de una propiedad existente. | Prueba de edición | I1 |
| RF-04 | El sistema deberá registrar un contrato de arrendamiento o de administración asociado a una propiedad. | Prueba de registro con FK | I1 |
| RF-05 | El sistema deberá adjuntar un archivo PDF a un contrato. | Prueba de carga multipart | I1 |
| RF-06 | El sistema deberá rechazar archivos que no sean PDF o que excedan el tamaño máximo configurado. | Prueba negativa de carga | I1 |
| RF-07 | El sistema deberá almacenar el PDF fuera de PostgreSQL y registrar ruta relativa y SHA-256. | Inspección de volumen y BD | I1 |
| RF-08 | El sistema deberá permitir visualizar o descargar un PDF solo a usuarios autenticados y autorizados. | Prueba de control de acceso | I1 |
| RF-09 | El sistema deberá calcular el estado de vigencia de un contrato a partir de sus fechas. | Prueba con fechas vigente y vencida | I1 |
| RF-10 | El sistema deberá listar contratos próximos a vencimiento según el rango configurado. | Prueba de alerta | I1 |
| RF-11 | El sistema deberá registrar ítems de inventario con descripción, categoría, cantidad y precio unitario. | Prueba de alta | I1 |
| RF-12 | El sistema deberá calcular el valor total de cada ítem como cantidad por precio unitario. | Prueba unitaria de cálculo | I1 |
| RF-13 | El sistema deberá calcular el valor del equipamiento activo por propiedad. | Prueba de valorización | I1 |
| RF-14 | El sistema deberá registrar movimientos de ingreso por propiedad. | Prueba de registro | I1 |
| RF-15 | El sistema deberá registrar movimientos de egreso operativo por propiedad. | Prueba de registro | I1 |
| RF-16 | El sistema deberá consultar movimientos filtrando por propiedad, mes, año y tipo. | Prueba de filtro | I1 |
| RF-17 | El sistema deberá mostrar el flujo mensual por propiedad como ingresos menos egresos. | Prueba de consulta | I1 |
| RF-18 | El sistema deberá registrar informes mensuales de administradores externos. | Prueba de alta de informe | I2 |
| RF-19 | El sistema deberá registrar reservas con plataforma, fechas y monto dentro de un informe. | Prueba de alta de reserva | I2 |
| RF-20 | El sistema deberá calcular la comisión aplicando el porcentaje pactado de 20 %, 25 % o 30 % sobre la base validada. | Prueba unitaria con tres porcentajes | I2 |
| RF-21 | El sistema deberá registrar la liquidación de comisión por administrador y mes. | Prueba de consulta de liquidación | I2 |
| RF-22 | El sistema deberá calcular la utilidad neta mensual por propiedad según la fórmula definida. | Prueba de cálculo | I2 |
| RF-23 | El sistema deberá generar un reporte mensual consolidado del portafolio. | Prueba de reporte consolidado | I2 |
| RF-24 | El sistema deberá exportar reportes en formato PDF y Excel/CSV. | Verificación de archivos generados | I2 |
| RF-25 | El sistema deberá registrar el cierre de mes por propiedad y periodo. | Prueba de cierre | I2 |
| RF-26 | El sistema deberá bloquear modificaciones de movimientos e informes después del cierre de mes. | Prueba negativa tras cierre | I2 |
| RF-27 | El sistema deberá mostrar gráficos comparativos por propiedad y mes. | Revisión de pantalla | I2 |

## 5. Requisitos no funcionales

| ID | Categoría | Condición medible | Verificación | Incremento |
|---|---|---|---|---|
| RNF-01 | Seguridad | Todo PDF requiere sesión autenticada y autorización vigente. | Prueba de acceso anónimo y con rol inválido | I1 |
| RNF-02 | Precisión | Los importes se almacenan en `NUMERIC(14,2)` y se procesan con `BigDecimal`. | Inspección de esquema y pruebas unitarias | I1 |
| RNF-03 | Integridad | Las tablas financieras usan foreign keys con `ON DELETE RESTRICT`. | Revisión de migraciones Flyway | I1 |
| RNF-04 | Auditabilidad | Las entidades de negocio registran `created_at`, `updated_at` y `created_by`. | Revisión de entidades y BD | I1 |
| RNF-05 | Despliegue | `docker compose up --build` levanta backend, PostgreSQL y volumen de contratos sin pasos manuales adicionales. | Prueba en entorno limpio | I1 |
| RNF-06 | Usabilidad | Los formularios muestran mensajes de validación en español y permiten carga desde cero sin hojas de cálculo previas. | Prueba con operador | I1 |
| RNF-07 | Auditabilidad | Tras el cierre de mes, cualquier modificación devuelve error y queda registrada en auditoría. | Prueba negativa y revisión de log | I2 |
| RNF-08 | Rendimiento | Las consultas de listado y reporte con 10 propiedades y 12 meses responden en menos de 2 segundos. | Prueba de tiempo local | I1-I2 |
| RNF-09 | Formato | Los reportes exportados incluyen encabezados, periodo y totales. | Revisión del archivo generado | I2 |
| RNF-10 | Seguridad | El repositorio contiene `.env.example`, nunca `.env`, secretos ni contraseñas reales. | Inspección de Git e historial | I1 |

## 6. Decisiones pendientes

| ID | Decisión requerida | Responsable | Relación |
|---|---|---|---|
| PD-01 | Definir la base exacta de cálculo de la comisión 20/25/30 %. | Docente / propietario | RN-02, RF-20 |
| PD-02 | Definir el efecto de cambiar una propiedad de modalidad. | Docente / propietario | RN-12, RF-03 |
| PD-03 | Confirmar si un ítem Dañado se excluye del equipamiento activo. | Docente / propietario | RN-06, RF-13 |
| PD-04 | Definir días de anticipación para alertas de vencimiento. | Docente / contador | RN-03, RF-10 |
| PD-05 | Definir quién autoriza el cierre de mes. | Docente / propietario | RN-10, RF-25 |
