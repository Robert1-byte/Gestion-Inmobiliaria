# Catálogo de trazabilidad: stakeholders, reglas, historias y requisitos

Documento de apoyo para la Actividad 1. Las historias de usuario son las definidas por el equipo. Los códigos se usan en `docs/matriz-trazabilidad.md` y `docs/matriz-trazabilidad.csv`. Las decisiones no resueltas se registran como `PD-XX` para la entrevista con el cliente.

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
| RN-01 | El portafolio opera bajo dos modalidades: renta tradicional y renta temporal. | Caso §2.1; HU-01 | Confirmada |
| RN-02 | La comisión del administrador externo es 20 %, 25 % o 30 % según lo pactado. | Caso §2.2; HU-08 | Porcentaje confirmado; base pendiente PD-01 |
| RN-03 | Los contratos notarizados se registran en PDF con firma, inicio, vencimiento, garantía y estado Vigente, Vencido o Terminado. | Caso §2.3; HU-03 | Confirmada; días de alerta pendientes PD-04 |
| RN-04 | Los PDF se almacenan fuera de la base de datos y el acceso es autenticado. | Caso §2.3 y §3; HU-03, HU-06 | Confirmada |
| RN-05 | El inventario valorizado aplica a propiedades de renta temporal. | Caso §2.4; HU-05 | Confirmada |
| RN-06 | El valor activo del inventario excluye los estados Dañado y Dado de baja. | Caso §2.4; HU-05 | Pendiente PD-03 |
| RN-07 | Los ingresos incluyen cánones de renta tradicional e ingresos por reservas reportadas. | Caso §2.5; HU-06, HU-07 | Confirmada |
| RN-08 | Los egresos incluyen luz, agua, alícuotas, internet, mantenimiento y reposición. | Caso §2.5; HU-06 | Confirmada |
| RN-09 | Utilidad neta = ingresos brutos − comisión del administrador − egresos operativos. | Caso §2.6; HU-09 | Confirmada |
| RN-10 | El cierre de mes impide alteraciones posteriores a la liquidación. | Caso §2.6; HU-10 | Confirmada; autoridad pendiente PD-05 |
| RN-11 | No se integra vía API con Airbnb/Booking; se registran informes mensuales. | Caso §2.1; HU-07 | Confirmada |
| RN-12 | La modalidad de una propiedad puede cambiar y debe conservar contratos y movimientos previos sin alteración. | HU-02 | Confirmada por el equipo |
| RN-13 | Los importes usan tipos numéricos exactos. | Caso §3; HU-03, HU-06, HU-08 | Confirmada |

## 3. Historias de usuario

| ID | Título | Enunciado | Criterio de aceptación | Prioridad | Incremento | Módulo |
|---|---|---|---|---|---|---|
| HU-01 | Registro y administración del portafolio de propiedades | Como propietario o administrador, quiero registrar y actualizar las propiedades con su tipo, dirección, modalidad de renta (Tradicional o Temporal) y estado operativo, para centralizar el control de los 10 inmuebles y futuras adquisiciones. | Dado que un usuario autorizado accede al módulo de propiedades; cuando ingresa los datos obligatorios de un inmueble y guarda; entonces el sistema almacena la propiedad en PostgreSQL y la muestra activa en el portafolio. | Alta | I1 | Propiedades |
| HU-02 | Cambio de modalidad y conservación de historial | Como propietario, quiero cambiar la modalidad de arrendamiento de una propiedad (de tradicional a temporal o viceversa) conservando intactos sus contratos y movimientos previos, para mantener la trazabilidad histórica completa del inmueble. | Dado que una propiedad tiene historial registrado bajo una modalidad; cuando se actualiza su modalidad de renta; entonces el sistema conserva los registros anteriores sin alteración y habilita la nueva modalidad. | Alta | I1 | Propiedades |
| HU-03 | Carga y control de vigencia de contratos notarizados en PDF | Como propietario o administrador autorizado, quiero subir contratos notarizados en formato PDF registrando sus fechas de firma, inicio, vencimiento, garantías y porcentaje de comisión pactado, para digitalizar el respaldo legal y controlar su vigencia. | Dado que existe una propiedad registrada; cuando se carga un archivo .pdf válido junto con sus metadatos de vigencia; entonces el sistema guarda el archivo en almacenamiento persistente fuera de la BD, registra su ruta y calcula su estado (Vigente, Vencido o Terminado). | Alta | I1 | Contratos |
| HU-04 | Alertas tempranas de vencimiento de contratos | Como propietario o administrador, quiero visualizar alertas tempranas cuando un contrato esté próximo a vencer, para gestionar oportunamente su renovación o terminación. | Dado que un contrato vigente alcanza el umbral de días previo a su fecha de vencimiento; cuando el usuario ingresa al panel principal; entonces el sistema despliega una alerta visible destacando el contrato próximo a expirar. | Media | I1 | Contratos |
| HU-05 | Gestión de inventario valorizado de equipamiento y mobiliario | Como administrador o propietario, quiero registrar y actualizar el inventario valorizado de muebles y enseres por propiedad (cantidad, precio unitario y estado de conservación) y calcular su valor total, para respaldar las actas de entrega en renta temporal y controlar reposiciones ante daños. | Dado que una propiedad está destinada a renta temporal; cuando se registran o actualizan ítems con su precio unitario y estado (Nuevo, Bueno, Regular, Dañado, Dado de baja); entonces el sistema recalcula automáticamente el valor total del equipamiento activo de la propiedad. | Media | I1 | Inventario |
| HU-06 | Registro operativo de ingresos y egresos por propiedad | Como administrador o propietario, quiero registrar mensualmente los ingresos (cánones e ingresos de reservas) y los egresos operativos por propiedad (luz, agua, alícuotas, internet, mantenimiento y reposiciones) adjuntando comprobantes, para llevar un control ordenado del flujo de caja por inmueble. | Dado que selecciono una propiedad y un período mensual abierto; cuando registro un ingreso o egreso con su categoría, monto exacto y respaldo; entonces el sistema persiste el movimiento asociado exclusivamente a esa propiedad. | Alta | I1 | Finanzas |
| HU-07 | Registro de informes mensuales de reservas de renta temporal | Como administrador externo, quiero registrar en el sistema el informe mensual detallado de reservas concretadas (fechas, plataforma de origen como Airbnb/Booking/directo y monto generado), para reportar los ingresos del mes sin requerir integración API externa. | Dado que una propiedad opera en renta temporal; cuando el administrador registra las reservas del mes con sus fechas, canal y monto; entonces el sistema consolida el ingreso bruto mensual de reservas para dicha propiedad. | Alta | I2 | Comisiones |
| HU-08 | Cálculo y liquidación automática de comisiones de administradores | Como propietario, quiero que el sistema calcule automáticamente la comisión mensual del administrador externo aplicando el 20 %, 25 % o 30 % pactado en su contrato sobre los ingresos reportados, para agilizar y auditar las liquidaciones mensuales. | Dado que existe un informe mensual de ingresos registrado y un porcentaje de comisión contractual (20 %, 25 % o 30 %); cuando se genera la liquidación del mes; entonces el sistema calcula el monto exacto de comisión utilizando precisión decimal exacta. | Alta | I2 | Comisiones |
| HU-09 | Reportes financieros mensuales y rentabilidad neta con exportación | Como propietario, quiero visualizar en pantalla (con gráficos comparativos) y exportar en PDF y Excel/CSV los reportes mensuales por propiedad y consolidados aplicando la fórmula de utilidad neta (IngresosBrutos-Comisión-EgresosOperativos), para conocer la rentabilidad real de cada inmueble. | Dado que existen movimientos y comisiones registrados en un período; cuando solicito el reporte mensual individual o consolidado; entonces el sistema presenta el desglose exacto de utilidad neta y permite descargarlo en PDF y Excel/CSV. | Alta | I2 | Reportes |
| HU-10 | Cierre de mes para bloqueo de auditoría | Como propietario, quiero ejecutar el cierre de mes una vez validados los ingresos, egresos y comisiones del período, para impedir modificaciones posteriores y garantizar la integridad contable. | Dado que un período mensual está consolidado; cuando el propietario confirma el Cierre de Mes; entonces el sistema marca el período como cerrado y bloquea cualquier edición, inserción o eliminación en dicho mes. | Alta | I2 | Reportes |

## 4. Requisitos funcionales

| ID | Requisito | Verificación | Incremento |
|---|---|---|---|
| RF-01 | El sistema deberá registrar una propiedad con código, dirección, tipo, modalidad y estado operativo. | Prueba de alta y consulta | I1 |
| RF-02 | El sistema deberá listar propiedades filtrando por modalidad de renta. | Prueba de filtro | I1 |
| RF-03 | El sistema deberá actualizar los datos de una propiedad existente. | Prueba de edición | I1 |
| RF-04 | El sistema deberá registrar un contrato con fechas de firma, inicio y vencimiento, garantía y porcentaje de comisión cuando corresponda. | Prueba de registro con metadatos completos | I1 |
| RF-05 | El sistema deberá adjuntar un archivo PDF a un contrato. | Prueba de carga multipart | I1 |
| RF-06 | El sistema deberá rechazar archivos que no sean PDF o que excedan el tamaño máximo configurado. | Prueba negativa de carga | I1 |
| RF-07 | El sistema deberá almacenar el PDF fuera de PostgreSQL y registrar ruta relativa y SHA-256. | Inspección de volumen y BD | I1 |
| RF-08 | El sistema deberá permitir visualizar o descargar un PDF solo a usuarios autenticados y autorizados. | Prueba de control de acceso | I1 |
| RF-09 | El sistema deberá calcular el estado de un contrato como Vigente, Vencido o Terminado a partir de sus fechas y terminación. | Prueba con tres estados | I1 |
| RF-10 | El sistema deberá listar contratos próximos a vencimiento según el umbral de días configurado. | Prueba de alerta en panel | I1 |
| RF-11 | El sistema deberá registrar ítems de inventario con descripción, categoría, cantidad, precio unitario y estado de conservación. | Prueba de alta | I1 |
| RF-12 | El sistema deberá calcular el valor total de cada ítem como cantidad por precio unitario. | Prueba unitaria de cálculo | I1 |
| RF-13 | El sistema deberá recalcular el valor del equipamiento activo por propiedad al registrar o actualizar ítems. | Prueba de valorización | I1 |
| RF-14 | El sistema deberá registrar movimientos de ingreso por propiedad y periodo mensual. | Prueba de registro | I1 |
| RF-15 | El sistema deberá registrar movimientos de egreso operativo por propiedad y periodo mensual. | Prueba de registro | I1 |
| RF-16 | El sistema deberá consultar movimientos filtrando por propiedad, mes, año y tipo. | Prueba de filtro | I1 |
| RF-17 | El sistema deberá mostrar el flujo mensual por propiedad como ingresos menos egresos. | Prueba de consulta | I1 |
| RF-18 | El sistema deberá registrar informes mensuales de administradores externos. | Prueba de alta de informe | I2 |
| RF-19 | El sistema deberá registrar reservas con plataforma, fechas y monto dentro de un informe. | Prueba de alta de reserva | I2 |
| RF-20 | El sistema deberá calcular la comisión aplicando el porcentaje pactado de 20 %, 25 % o 30 % sobre los ingresos reportados validados. | Prueba unitaria con tres porcentajes | I2 |
| RF-21 | El sistema deberá registrar la liquidación de comisión por administrador y mes. | Prueba de consulta de liquidación | I2 |
| RF-22 | El sistema deberá calcular la utilidad neta mensual por propiedad según la fórmula definida. | Prueba de cálculo | I2 |
| RF-23 | El sistema deberá generar un reporte mensual consolidado del portafolio. | Prueba de reporte consolidado | I2 |
| RF-24 | El sistema deberá exportar reportes en formato PDF y Excel/CSV. | Verificación de archivos generados | I2 |
| RF-25 | El sistema deberá registrar el cierre de mes por propiedad y periodo. | Prueba de cierre | I2 |
| RF-26 | El sistema deberá bloquear edición, inserción y eliminación de movimientos e informes después del cierre de mes. | Prueba negativa tras cierre | I2 |
| RF-27 | El sistema deberá mostrar gráficos comparativos por propiedad y mes. | Revisión de pantalla | I2 |
| RF-28 | El sistema deberá registrar el cambio de modalidad de una propiedad conservando contratos y movimientos previos sin alteración. | Prueba de cambio con historial | I1 |
| RF-29 | El sistema deberá permitir adjuntar un comprobante PDF a cada movimiento de ingreso o egreso. | Prueba de carga y consulta de comprobante | I1 |

## 5. Requisitos no funcionales

| ID | Categoría | Condición medible | Verificación | Incremento |
|---|---|---|---|---|
| RNF-01 | Seguridad | Todo PDF de contrato o comprobante requiere sesión autenticada y autorización vigente. | Prueba de acceso anónimo y con rol inválido | I1 |
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
| PD-01 | Definir la base exacta de cálculo de la comisión 20/25/30 %. | Docente / propietario | RN-02, RF-20, HU-08 |
| PD-03 | Confirmar si un ítem Dañado se excluye del equipamiento activo. | Docente / propietario | RN-06, RF-13, HU-05 |
| PD-04 | Definir días de anticipación para alertas de vencimiento. | Docente / contador | RN-03, RF-10, HU-04 |
| PD-05 | Definir quién autoriza el cierre de mes. | Docente / propietario | RN-10, RF-25, RF-26, HU-10 |

Nota: la decisión anterior PD-02 quedó resuelta por HU-02, que define que el cambio de modalidad se permite conservando el historial.
