# Requisitos

## 1. Contexto y alcance

### 1.1. Objetivo del sistema

Breve descripción del problema que se pretende resolver y del objetivo general del sistema.

### 1.2. Alcance

Descripción de qué responsabilidades corresponden al sistema y, cuando sea relevante,
qué elementos quedan explícitamente fuera de su alcance.

### 1.3. Actores y partes interesadas

Identificar las personas, organizaciones o sistemas que interactúan con el sistema
o tienen algún interés relevante en su funcionamiento.

<!--
EJEMPLO:

| Actor / stakeholder | Descripción | Intereses principales |
|---|---|---|
| Personal administrativo | Personal de Turbine encargado de gestionar clientes, facturas y cobros | Realizar las tareas administrativas de forma rápida y disponer de información actualizada |
| Administrador | Usuario con permisos sobre la gestión del sistema y sus usuarios | Gestionar usuarios, permisos y configuración |
| Cliente de Turbine | Empresa a la que Turbine presta sus servicios y factura periódicamente | Recibir correctamente sus facturas y comunicaciones relacionadas con los pagos |
| Sistema de pagos | Sistema externo en el que se reflejan los cobros | Proporcionar información que permita determinar si una factura ha sido pagada |

Los elementos anteriores son únicamente un ejemplo de cómo documentar esta sección.
El equipo debe identificar y justificar los actores y stakeholders que considere
adecuados a partir de la información del caso.
-->

| Actor / stakeholder | Descripción | Intereses principales |
|---|---|---|
| ... | ... | ... |


## 2. Requisitos de negocio

Los requisitos de negocio describen los objetivos, necesidades o resultados que
la organización pretende conseguir mediante el desarrollo del sistema. No describen
todavía funcionalidades concretas del software.

<!--
EJEMPLO:

| ID | Requisito | Fuente |
|---|---|---|
| BR-01 | Reducir el trabajo manual asociado a la generación de facturas y al seguimiento de los cobros | Cliente / entrevista |
| BR-02 | Hacer más eficiente la gestión administrativa a medida que aumente el número de clientes de Turbine | Cliente / entrevista |

Observa que estos requisitos expresan POR QUÉ se necesita el sistema y qué pretende
conseguir la organización. No indican todavía cómo debe hacerlo el software.
-->

| ID | Requisito | Fuente |
|---|---|---|
| BR-01 | ... | Cliente / entrevista / decisión del equipo / otra |
| BR-02 | ... | ... |


## 3. Requisitos de usuario

Los requisitos de usuario describen las necesidades, objetivos o servicios que los
usuarios esperan obtener del sistema. Deben expresarse desde el punto de vista del
usuario, evitando entrar todavía en detalles internos de implementación.

<!--
EJEMPLO:

| ID | Actor | Requisito | Fuente |
|---|---|---|---|
| UR-01 | Personal administrativo | Poder generar y gestionar facturas de los clientes sin tener que realizar manualmente cada facturación mensual | Cliente / entrevista |
| UR-02 | Personal administrativo | Poder conocer qué facturas están pagadas y cuáles siguen pendientes | Cliente / entrevista |
| UR-03 | Administrador | Poder gestionar los usuarios del sistema y determinar a qué clientes puede acceder cada uno | Cliente / entrevista |

Un requisito de usuario expresa QUÉ necesita conseguir un usuario con el sistema,
pero normalmente no especifica todavía todos los comportamientos detallados que
serán necesarios para proporcionarle ese servicio.
-->

| ID | Actor | Requisito | Fuente |
|---|---|---|---|
| UR-01 | El usuario debe poder ver y modificar la información operativa de los clientes. | ... | ... |
| UR-01 | El usuario debe poder consultar facturas y descargarlas. | ... | ... |
| UR-01 | El usuario debe poder crear facturas si su nivel de permiso lo permite. | ... | ... |
| UR-01 | El rol "responsable" debe poder ver y modificar la información operativa de los clientes. | ... | ... |
| UR-01 | El rol "responsable" debe poder ver y modificar la información operativa de los clientes. | ... | ... |




## 4. Requisitos del sistema

### 4.1. Requisitos funcionales

Los requisitos funcionales describen comportamientos, servicios u operaciones que
debe proporcionar el sistema. Siempre que sea posible, deben formularse de manera
clara, precisa y verificable.

<!--
EJEMPLO:

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-01 | El sistema deberá permitir crear y almacenar una factura asociada a un cliente | UR-01 |
| FR-02 | El sistema deberá permitir indicar si la facturación de un cliente es puntual o recurrente | UR-01 |
| FR-03 | El sistema deberá generar automáticamente las nuevas facturas correspondientes a una facturación recurrente | UR-01 |
| FR-04 | El sistema deberá registrar si una factura se encuentra pagada o pendiente de pago | UR-02 |
| FR-05 | El sistema deberá permitir al administrador asignar clientes a los usuarios del sistema | UR-03 |

Evita requisitos como "El sistema gestionará las facturas correctamente", porque
no permiten saber con precisión qué comportamiento se espera.

También deben evitarse decisiones de diseño innecesarias, por ejemplo:
"El sistema utilizará una tabla SQL denominada FACTURAS".
Eso corresponde al diseño, no a los requisitos.
-->

| ID | Requisito | Relacionado con |
|---|---|---|
| FR-01 | El rol admin debe poder registrar nuevas empresas clientes con sus respectivas personas de contacto, nombre comercial, razón social, NIF o identificador fiscal, domicilio fiscal, país y moneda de trabajo.  | ... |
| FR-02 | El rol responsable debe poder consultar las facturas de los clientes a los que esté asignado, así como descargarlas. | ... |
| FR-03 | El rol admin debe poder añadir y editar a qué clientes tiene acceso un empleado lector. | ... |
| FR-04 | El rol admin debe poder eliminar un cliente. | ... |
| FR-05 | El rol admin debe poder editar varios parámetros para cada clientes: importe recurrente, importes adicionales, frecuencia de recordatorios de pago. Ciertos cambios de gran tamaño deben pasar por el superadmin. | ... |
| FR-06 | Los usuarios deben poder iniciar sesión con su email @turbineh.com y su contraseña, entrando con los privilegios correspondientes a su rol. | ... |
| FR-07 | Los usuarios deben poder crear facturas adicionales a las mensuales. | ... |
| FR-08 | El rol superadmin debe poder consultar el historial de cambios y facturas y tener acceso a las copias de seguridad. | ... |
| FR-09 | Los usuarios deben poder filtrar las facturas (pagadas/impagadas, por fecha, recurrente/adicional) | ... |
| FR-10 | Se debe poder acceder a la información del sistema a través de una API. | ... |
| FR-11 | El rol admin debe poder importar datos de clientes nuevos mediante CSV, lo cual debe mostrar una vista previa en la cual el admin pueda decidir que datos mantener en caso de haber datos corruptos u otros errores de formato. | ... |
| FR-12 | El sistema debe producir facturas mensuales a cada cliente el primer día del mes, así como las facturas adicionales. | ... |
| FR-13 | EL sistema debe enviar un flujo automático de recordatorios de pago, en un principio 3 días y 5 días después de la factura, y luego cada 2 días. | ... |
| FR-14 | EL sistema debe hacer un seguimiento del estado de pagos de todas las facturas automáticamente mediante conexión con Stripe. | ... |
| FR-15 | El sistema enviará una confirmación automática al cliente cuando las facturas sean pagadas con éxito. | ... |
| FR-16 | El sistema debe enviar a los roles admin un seguimiento mensual/trimestral/anual de las finanzas de la empresa. | ... |
| FR-17 | Cuando el sistema envía una factura, debe quedar registrado a qué destinatario se envío en ese momento | ... |
| FR-18 | Anualmente el sistema debe hacer una revisión de los rangos en los que se encuentra cada empresa y reajustar sus precios recurrentes, con validación humana. | ... |
| FR-19 | El rol superadmin debe poder cancelar una deuda, cambiando una factura de "impagada" a "pagada". | ... |
| FR-20 | El rol usuario debe poder marcar una factura en periodo de impago como "en pausa" para detener los recordatorios periodicos. | ... |
| FR-21 | Los usuarios deben poder editar las personas de contacto de una empresa. | ... |
| FR-22 | El sistema debe enviar una notificación de eventos relevantes a los encargados de cada empresa. | 23 |
| FR-23 | El usuario debe poder marcar las notificaciones como leídas o resueltas. | 22 |
| FR-24 | El sistema debe mostrar facturas que vencen pronto, facturas vencidas, pagos que necesitan conciliación, clientes con problemas de entrega y aprobaciones pendientes. | ... |
| FR-25 | El superadmin debe poder editar la tabla con los rangos para los precios recurrentes. | ... |
| FR-26 | El usuario debe poder dar descuentos o pruebas gratuitas a empresas. | 5 |
| FR-27 | El rol admin debe poder filtrar los informes por periodo, cliente, responsable interno, país, sector, estado de factura y tipo (recurrente/adicional) | 16 |
| FR-XX | ... | ... |
| FR-XX | ... | ... |
| FR-XX | ... | ... |
| FR-XX | ... | ... |
| FR-XX | ... | ... |
| FR-XX | ... | ... |



### 4.2. Requisitos no funcionales

Los requisitos no funcionales describen propiedades de calidad, restricciones o
condiciones que debe satisfacer el sistema. Cuando sea posible, deberán incluir
algún criterio que permita determinar si el requisito se cumple.

<!--
EJEMPLO:

| ID | Categoría | Requisito | Criterio verificable |
|---|---|---|---|
| NFR-01 | Usabilidad | Las operaciones administrativas habituales deberán poder realizarse de forma rápida y sencilla | Una operación habitual como generar o consultar una factura debería poder completarse aproximadamente en menos de un minuto |
| NFR-02 | Portabilidad / Usabilidad | La aplicación deberá poder utilizarse desde distintos tipos de dispositivo | Las principales funcionalidades deberán ser utilizables desde ordenador, tableta y teléfono móvil |
| NFR-03 | Escalabilidad | El sistema deberá poder gestionar un crecimiento significativo del número de clientes | El diseño no deberá asumir un número pequeño o fijo de clientes y deberá permitir gestionar potencialmente miles de ellos |

Evita expresiones difíciles de comprobar como:
"La aplicación deberá ser moderna", "muy rápida" o "fácil de usar".

Cuando el cliente emplee expresiones de este tipo, el equipo deberá intentar
transformarlas en criterios más concretos y verificables o documentar la
interpretación realizada.
-->

| ID | Categoría | Requisito | Criterio verificable |
|---|---|---|---|
| NFR-01 | ... | El sistema debe ser responsive, una aplicación web que funcione en dispositivos móviles correctamente | ... |
| NFR-02 | ... | El sistema debe comprobar cuando un usuario se loguee que el correa sea @turbineh.com | ... |
| NFR-03 | ... | Cuando un cliente sea eliminado por un admin, sus datos deben permanecer en la base de datos tanto tiempo como la ley permita. | ... |
| NFR-04 | ... | Las facturas deben ser estándar y válidas fiscalmente. | ... |
| NFR-05 | ... | Los datos de las facturas están bien cifrados para no tener problemas legales. | ... |


## 5. Reglas de negocio

Reglas del dominio o de la organización que condicionan el comportamiento del sistema
pero que no representan por sí mismas una funcionalidad.

| ID | Regla |
|---|---|
| RB-01 | ... |
| RB-02 | ... |


## 6. Suposiciones y decisiones de análisis

En esta sección se documentarán las decisiones tomadas por el equipo cuando la
información proporcionada por el cliente no sea suficiente para determinar unívocamente
un requisito.

| ID | Suposición o decisión | Justificación | Requisitos afectados |
|---|---|---|---|
| A-01 | ... | ... | FR-... |
| A-02 | ... | ... | ... |


## 7. Dudas, ambigüedades y cuestiones pendientes

Se incluirán aquí aspectos de la información proporcionada por el cliente que sean
ambiguos, incompletos, contradictorios o que requieran aclaración.

| ID | Cuestión | Origen | Impacto | Estado / resolución |
|---|---|---|---|---|
| Q-01 | ... | Entrevista | ... | Pendiente |
| Q-02 | ... | ... | ... | Resuelta: ... |


## 8. Trazabilidad

La tabla permitirá comprobar la relación entre las necesidades de negocio,
los requisitos de usuario y los requisitos del sistema.

| Requisito de negocio | Requisito de usuario | Requisito funcional / no funcional |
|---|---|---|
| BR-01 | UR-01 | FR-01, FR-02 |
| BR-02 | UR-03 | FR-07, NFR-02 |


## 9. Glosario

| Término | Definición |
|---|---|
| ... | ... |

