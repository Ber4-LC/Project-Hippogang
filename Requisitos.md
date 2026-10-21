# Requisitos

## 1. Contexto y alcance

### 1.1. Objetivo del sistema 

TurbineH es una empresa que ofrece servicios de su inteligencia artificial "Brain" mediante suscripciones. Esta se enfrenta a un problema relacionado con la gestión económica y el aumento del número de clientes. Este crecimiento dificulta la administración de facturas y cobros, puesto que, a medida que aumenta el número de clientes, también aumenta la cantidad de datos, movimientos, facturas y suscripciones que es necesario gestionar.

Por lo tanto, el objetivo del sistema, para solucionar el problema presentado, sería crear una aplicación que permita:

- *Gestionar clientes y suscripciones.* Poder modificar los precios de sus servicios de forma cómoda y sencilla, así como mantener un registro de los clientes y sus suscripciones.

- *Generar facturas y hacer seguimiento de los cobros.* Poder generar facturas con facilidad, así como consultar los cobros de los clientes, permitiendo comprobar los importes facturados, cobrados y pendientes de pago.

- *Mejorar la gestión a mayor escala.* Poder tener una mayor cantidad de clientes registrados y gestionarlos de forma cómoda, sin que el aumento de su número dificulte la administración.

- *Reducir el trabajo manual.* Poder automatizar procesos y mejorar la organización de los datos, facilitando así el trabajo a los empleados de TurbineH.

*Es importante considerar que esta es una aplicación diseñada únicamente para el uso interno de TurbineH. La aplicación no se encarga de proporcionar los servicios ofrecidos por la empresa, sino de gestionar los aspectos administrativos y económicos relacionados con su facturación y cobro.*

### 1.2. Alcance

El alcance de nuestra aplicación aborda diversos frentes, aun así hay cosas fuera de dicho alcance. Dentro de lo que sí entra en el alcance, tenemos:

- *Registrar y gestionar clientes.* No ofrecemos un portal de gestión a los clientes, únicamente a la empresa.
- *Gestionar subscripciones y tarifas.* Los empleados pueden consultar y administrar la información de las subscripciones y sus condiciones.
- *Generar y enviar facturas.* La aplicación permite generar facturas de forma automática y manual, así como enviarlas a los clientes correspondientes. 
- *Consultar pagos pendientes.* La empresa tendrá a su disposición los pagos pendientes de sus clientes.
- *Gestión de usuarios y permisos.* Permite administrar los usuarios de la aplicación y establecer diferentes permisos según las funciones que puedan realizar los empleados.
- *Consulta de información de facturación.* Permite visualizar datos generales sobre facturas, cobros y deudas pendientes.

Es importante anotar que nuestra aplicación no distribuye ni desarrolla la IA, sino que esta está destinada a facilitar la gestión administrativa de la empresa. De igual manera, la aplicación no se encarga de procesar pagos reales, únicamente de mantener registros de los cobros para que la empresa pueda tenerlas a su disposición.

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
| FR-XX | El rol superadmin debe poder consultar el historial de todos los procesos automáticos para poder var cuáles ocurrieron. | ... |
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
| FR-XX | Los usuarios pueden establecer la plantilla de los emails de factura a sus clientes, y debe quedar registrado la plantilla usada. | ... |
| FR-XX | El sistema debe generar copias de seguridad con una política que se pueda definir y que se pueda restaurar. | ... |
| FR-XX | El usuario debe poder realizar acciones a varios clientes a la vez pero debe pedir confirmación y mostrar a cuántos elementos está afectando y cómo. | ... |
| FR-XX | El usuario debe poder abrir incidencias hacia un cliente, especificando un responsable, estado, prioridad y comentarios. | ... |
| FR-XX | El usuario debe poder dejar comentarios asociados a elementos, mencionar a otro usuario le envía una notificación | ... |
| FR-XX | El usuario debe poder etiquetar clientes (partner, estratégico, beta, etc) | ... |
| FR-XX | El usuario debe poder adjuntar archivos a las facturas. | ... |
| FR-XX | El sistema debe tener una pantalla sencilla de la salud del sistema. | ... |
| FR-XX | ... | ... |
| FR-28 | El sistema debe mostrar con detalle los fallos que ocurran al administrador y mostrar al usuario solo un código de incidencia. | ... |
| FR-29 | El sistema debe buscar posibles clientes duplicados antes de crearlos, permitiendo que los campos no necesarios para la emisión de facturas no tengan datos. | 11 |
| FR-30 | El sistema debe registrar el historial de cada usuario y permitir al administrador acceder a ellos. | ... |
| FR-31 | El sistema debe permitir marcar a usuarios como favoritos y acceder a ellos, así como a los usuarios recientemente consultados. | ... |
| FR-32 | El sistema debe invalidar las sesiones de un usuario cuando un administrador desactiva a este usuario. | ... |
| FR-33 | Los datos fiscales del usuario deben poder configurarse. | 5 |
| FR-34 | El sistema debe permitir configurar un objetivo mensual o anual de cobros y poder ver su progreso por parte del administrador; si se cambia de objetivo, debe guardarse el anterior objetivo establecido. | ... |
| FR-35 | El sistema debe calcular correctamente los importes, reflejando exactamente lo que se guarda. | ... |
| FR-36 | El sistema debe permitir gestionar el estado de un usuario mediante diferentes etiquetas, pudiendo estar prospecto importado, pendiente de activación, activo, inactivo, etc. | ... |
| FR-37 | El sistema debe bloquear y explicar en caso de que se intente activar la recurrencia de un usuario sin tener los datos fiscales suficientes para poder activarla. | ... |
| FR-38 | El sistema debe tener una bandeja de aprobaciones que permita a los usuarios ver el estado de sus solicitudes y comentarios de los administradores asociados a ellas. | ... |
| FR-39 | El sistema debe impedir que el usuario apruebe sus propias solicitudes. | ... |
| FR-40 | El sistema debe permitir configurar los umbrales económicos y las operaciones que requieren separación de funciones para importes sensibles. | ... |
| FR-41 | El sistema debe impedir que el usuario genere varias facturas iguales si este lo solicita en varias ocasiones. Para estos casos se mostrara un mensaje informando que está en proceso. | ... |
| FR-42 | El sistema debe permitir que los administradores cierren periodos económicos mensuales o trimestrales. Una vez cerrado el periodo, no se permitirán modificaciones económicas ordinarias sobre los registros incluidos en él. | ... |
| FR-43 | El sistema debe permitir que los administradores realicen modificaciones excepcionales en los periodos económicos cerrados mediante un procedimiento controlado y auditado. | 42 |
| FR-44 | El sistema debe poder exportar un paquete mensual o trimestral para la gestoría en un formato configurable por el administrador, así como generar cuándo se generó y el período que cubría. | ... |
| FR-45 | El administrador debe poder ver el estado de los mensajes, mostrando si están en cola, enviados, entregados, fallidos o reintentados. | ... |
| FR-46 | El sistema debe permitir registrar y consultar si el canal de contacto de usuario admite comunicaciones administrativas. | ... |
| FR-47 | El sistema debe registrar las exportaciones realizadas con fecha/hora, usuario que lo realizó, filtros aplicados y volumen de datos exportados. Las exportaciones masivas de datos deben quedar registradas. | ... |
| FR-48 | El sistema debe poder conservar los documentos un tiempo configurable por el administrador, así como regenerarlos si se permiten. Se debe diferenciar entre un archivo temporal de un documento contable a conservar. | ... |
| FR-49 | El sistema debe trasladar las alertas programadas para los findes de semana o festivos al próximo día laborable y debe ser configurable por el administrador. | ... |
| FR-50 | El sistema deberá mostrar cada factura en la moneda de emisión y permitir la conversión de importes a otras monedas para la elaboración de informes, diferenciando los importes originales de los convertidos. | ... |
| FR-51 | El sistema permite realizar comentarios por parte de los usuarios y los administradores, diferenciando los comentarios de los usuarios como comentarios normales y los comentarios de los administradores como notas restringidas administrativas. | ... |
| FR-XX | Se deben anonimizar los datos de los usuarios siempre y cuando los datos no invaliden facturas historicas| ... |
| FR-XX | El sistema debe guardar métricas del tiempo empleado para realizar operaciones y clics necesarios| ... |
| FR-XX | El sistema debe hacer guardado de borradores en caso de cierre accidental | ... |
| FR-XX | El sistema debe permitir  | ... |
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
| NFR-05 | ... | Los datos de las facturas están bien cifrados para no tener problemas legales. Datos fiscales no deben estar en URLs públicas y las comunicaciones deben estar cifrados.| ... |
| NFR-XX | ... | No debe haber conflictos si varias personas editan una factura a la vez. | ... |
| NFR-XX | Escalabilidad | El sistema debe poder soportar centenas de usuarios internos. | ... |
| NFR-XX | Disponibilidad | Puede haber caídas breves del sistema, pero no deben causar la pérdida ni duplicación de ninguna factura. | ... |
| NFR-XX | ... | El sistema debe disponer de un entorno de prueba para poder testear los cambios de configuración antes de aplicarlos. | ... |
| NFR-XX | ... | El sistema interno puede estar en español, pero las comunicaciones al cliente deben poder elegir inglés. | ... |
| NFR-XX | ... | Asegurar que no haya conflictos con las zonas horarias, las fechas de facturas deben usar la zona horaria del receptor. | ... |
| NFR-XX | ... | Las tareas normales deben realizarse en menos de un minuto y de manera fluida. | ... |
| NFR-XX | ... | Las operaciones largas o pesadas deben realizarse en segundo plano. | ... |
| NFR-XX | ... | Debe haber buen UX (User experience) con autosugerencias y no hacer repetir formularios si falla un campo. | ... |
| NFR-XX | ... | Al adjuntar un archivo a una factura debe haber limites de tamaño y no ejecutar archivos peligrosos. | ... |
| NFR-XX | ... | El dashboard con los resúmenes de las finanzas debe poder explicar de dónde sale cada cifra. | ... |
| NFR-XX | ... | La API externa debe someterse a las mismas reglas de acceso que la interfaz. | ... |
| NFR-XX | ... | Las integraciones de servicios externos deben estar encapsuladas y deben poder cambiarse por otro servicio fácilmente. | ... |
| NFR-XX | ... | Los errores deben estar bien distinguidos para que cuando algo no funciona el usuario sepa qué parte del sistema está fallando. | ... |
| NFR-12 | Seguridad | El sistema debe cerrar la sesión activa del usuario tras un tiempo de inactividad para evitar el riesgo de acceso no autorizado. | ... |
| NFR-13 | Seguridad | El sistema debe tener una medida de protección ante varios intentos fallidos de inicio de sesión. | ... |
| NFR-14 | Mantenibilidad | El sistema debe disponer de pruebas automatizadas para verificar el funcionamiento de las reglas económicas importantes. | ... |
| NFR-15 | Verificabilidad | El sistema debe tener escenarios de aceptación claros para los administradores | ... |
| NFR-16 | Mantenibilidad | Debe haber una documentación de usuario corta y útil, instrucciones de despliegue y mantenimiento, modelo de datos y API, decisiones importantes y explicación de reglas de negocio. | ... |
| NFR-17 | Compatibilidad | Se debe usar software libre o accesibles para una aplicación web responsive que sea compatible para cualquier navegador y dispositivo. | ... |
| NFR-18 | Mantenibilidad | Debe tener un entorno de desarrollo y pruebas. | ... |
| NFR-19 | Seguridad | Se debe separar la configuración y secretos del código. | ... |
| NFR-20 | Mantenibilidad | Se debe evitar duplicar los datos del usuario en el código. | ... |
| NFR-21 | Fiabilidad | Se debe utilizar precisión decimal. | ... |
| NFR-22 | Fiabilidad | El sistema debe ser tolerante con tareas en segundo plano. | ... |
| NFR-23 | QoL | Se debe poder completar cualquier operación con pocos clics | Los clics no deben sentirse innecesarios |
| NFR-24 | QoL | Interfaz intuitiva para nuevos usuarios y menús con poca profundidad | Deben meterse opciones claras y genéricas para hacer menús con poca profundidad|
| NFR-25 | Usabilidad| Consistencia en la distribución de los menús y opciones| Boton de guardar siempre en la misma esquina, carga, descarga , edicion, cada cosa no debe cambiar segun el lugar o plataforma en la que estè 

| NFR-XX | ... | ... | ... |

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
| Q-01 | El usuario debe poder enviar presupuestos al clientes y en caso de ser aceptado, debe generar una factura. Esto se propone como una idea interesante pero rompe la idea de que la apliación es interna. Se podría enviar un correo pero ¿Cómo procesar automáticamente la respuesta del cliente desde fuera del sistema? | Entrevista | Requisito funcional pendiente | Pendiente |
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

