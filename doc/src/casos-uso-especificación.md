Importante: Se entiende que si en actor se pone un usuario, todos los que están por encima de ese rol también pueden llevar a cabo ese trabajo.

# Especificación de los casos de uso
## Especificación del diagrama 1 (Autenticación y permisos de los roles de la base de datos):

### CU-01: Iniciar sesión.
```
• Nombre del caso de uso: Iniciar sesión.  
• Identificador: CU-01.  
• Objetivo: Autenticar a un empleado interno de Turbineh y habilitarle las funcionalidades correspondientes a su rol.  
• Actor o actores participantes:  
                    1. Empleado/Resposable.   
                    2. Administrador.  
                    3. Superadministrador.  
• Precondiciones:  
                    1. El usuario debe tener una cuenta dada de alta en el sistema con un correo corporativo.  
• Flujo principal:  
                    1. El usuario inicia sesión con sus credenciales.  
                    2. El sistema comprueba que tiene el dominio @turbineh.com y posteriormente la validez de las credenciales.  
                    3. Se realiza el inicio de sesión del usuario con su vista y privilegios asociados a dicho rol.  
• Flujos alternativos. (No existen en este caso)  
• Flujos de excepción:    
                    2a. Correo que no pertenece al dominio @turbineh.com.  
                    2b. Fallo en inicio de sesión a consecuencia de credenciales incorrectas.  
• Postcondiciones:  
                    1. El usuario queda autenticados con sus vistas y privilegios dependiendo de su rol.
```
  
  
### CU-02: Gestionar permisos de empleados.  
```
• Nombre del caso de uso: Gestionar permisos de empleados.  
• Identificador: CU-02.  
• Objetivo: Asignar o modificar la lista de empresas clientes que un empleado con rol lector puede visualizar.  
• Actor o actores participantes:   
                    1. Administrador que se encarga de gestionar los permisos del empleado.  
• Precondiciones:  
                    1. El administrador ha iniciado sesión en el sistema (CU-01) y existen tanto el empleado como las empresas clientes registradas.  
• Flujo principal:  
                    1. El administrador accede a la sección de gestión de permisos de empleados y selecciona un empleado lector o responsable.  
                    2. El sistema muestra el listado actual de empresas clientes asignadas a dicho empleado.  
                    3. El administrador añade nuevas empresas clientes o retira el acceso a clientes existentes y confirma los cambios.  
                    4. El sistema actualiza los permisos de acceso del empleado y registra la modificación en el historial de cambios.  
• Flujos alternativos:  
                    3. El administrador cancela la operación antes de guardar y el sistema mantiene las asignaciones previas.  
• Flujos de excepción:  
                    4. El sistema notifica que no se pudieron actualizar los permisos y revierte el cambio.  
• Postcondiciones:  
                    1. El empleado seleccionado únicamente puede consultar la información y facturas de la nueva lista de clientes asignados.
```
  
  
### CU-03: Consultar información mediante la API.  
```
• Nombre del caso de uso: Consultar información mediante la API.  
• Identificador: CU-03.  
• Objetivo: Acceso a la API por parte de los empleados con el fin de consultar información.  
• Actor o actores participantes:  
                    1. Empleado que solicita la consulta de información.  
• Precondiciones:  
                    1. El empleado dispone de los permisos requeridos para la consulta y la empresa cliente sobre la cual se hace la consulta tiene datos válidos.  
• Flujo principal:  
                    1. El empleado realiza una petición a la API solicitando los datos.  
                    2. El sistema valida los permisos del empleado y manda los datos de la petición.  
                    3. El sistema registra la consulta.  
• Flujos alternativos (No existen en este caso).  
• Flujos de excepción, cuando corresponda:  
                    2. El sistema deniega la consulta debido a la inexistencia de los permisos por parte del empleado.  
• Postcondiciones:  
                    1. El empleado solicitante de la información recibe dichos datos.
```  


## Especificación del diagrama 2 (Gestión de empresas cliente):  
  
### CU-04: Registrar empresa cliente.  
```
• Nombre del caso de uso: Registrar empresa cliente.  
• Identificador: CU-04.  
• Objetivo: Dar de alta de forma manual una nueva empresa cliente con sus datos fiscales, operativos y de contacto.  
• Actor o actores participantes:  
                    1. Administrador.  
• Precondiciones:  
                    1. El Administrador ha iniciado sesión en el sistema.  
• Flujo principal:  
                    1. El Administrador selecciona la opción de registrar un nuevo cliente.  
                    2. El Administrador introduce sus datos (Nombre, Razón Social, id, domicilio fiscal, país, etc.).  
                    3. El sistema valida que estén completos los campos obligatorios y los clientes no existen previamente.
                    4. El sistema almacena al nuevo cliente y registra la acción.    
• Flujos alternativos:
                    3a. El cliente existía anteriormente y fue eliminado pero como la base de datos conserva sus datos aún, se ofrece la posibilidad de reactivar su registro.
• Flujos de excepción, cuando corresponda:  
                    3a. Datos fiscales incompletos o inválidos.
                    3b. El cliente existe y está activo por lo que se registra un error.  
• Postcondiciones:  
                    1. La empresa queda registrada y lista para asignarle suscripciones y facturas.
```


### CU-05: Importar clientes mediante CSV.  
```
• Nombre del caso de uso: Importar clientes mediante CSV.  
• Identificador: CU-05.  
• Objetivo: Dar de alta empresas mediante ficheros CSV para eliminar trabajo manual.  
• Actor o actores participantes:  
                    1. Administrador.  
• Precondiciones:  
                    1. El Administrador ha iniciado sesión en el sistema y dispone del .csv para realizar el alta de la empresa.  
• Flujo principal:  
                    1. El Administrador selecciona la opción de registrar un nuevo cliente mediante fichero CSV.  
                    2. El sistema analiza las filas del fichero, comprueba que sean válidas y crea la ficha para la empresa (o fichas en caso de ser varias empresas) y registra la operación.      
• Flujos alternativos (No existen en este caso).      
• Flujos de excepción, cuando corresponda:
                    1. Archivo con formato diferente a CSV.  
                    2. Detección de datos erróneos o errores de formato. El sistema señala la fila del error.  
• Postcondiciones:  
                    1. La empresa queda registrada y lista para asignarle suscripciones y facturas.
```


### CU-06: Editar personas de contacto de cliente.  
```
• Nombre del caso de uso: Editar personas de contacto de cliente.  
• Identificador: CU-06.  
• Objetivo: Actualizar la información de los contactos y destinatarios de facturación asociados a una empresa cliente.    
• Actor o actores participantes:  
                    1. Responsable.  
• Precondiciones:  
                    1. El Responsable responsable ha iniciado sesión en el sistema y tiene permisos para editar la ficha de la empresa cliente.  
• Flujo principal:  
                    1. El Responsable accede a la ficha del cliente.   
                    2. El Responsable añade un nuevo contacto, modifica los datos existentes o elimina un contacto.
                    3. El sistema valida la operación y la registra.  
• Flujos alternativos (No existen en este caso).      
• Flujos de excepción, cuando corresponda:
                    2. El Responsable deja sin contacto a la empresa y salta un error.    
• Postcondiciones:  
                    1. Los contactos de la empresa quedan actualizados.
```
  
  
### CU-07: Editar parámetros de facturación de cliente.  
```
• Nombre del caso de uso: Editar parámetros de facturación de cliente.   
• Identificador: CU-07.  
• Objetivo: Modificar las condiciones económicas y de seguimiento de un cliente.    
• Actor o actores participantes:  
                    1. Administrador.  
• Precondiciones:  
                    1. El Administrador ha iniciado sesión en el sistema y existe el cliente en el sistema.  
• Flujo principal:  
                    1. El administrador selecciona un cliente y accede a su configuración de parámetros de suscripción y facturación.   
                    2. El administrador modifica el importe recurrente, los importes adicionales o la frecuencia de los recordatorios de pago.
                    3. El sistema aplica los nuevos parámetros, actualiza la suscripción y registra las modificaciones.    
• Flujos alternativos (No existen en este caso).      
• Flujos de excepción, cuando corresponda:
                    2a. El formato del importe o el mismo importe no son válidos.
                    2b. Los datos no son suficientes, el sistema bloquea la acción.    
• Postcondiciones:  
                    1. Los parámetros de facturación de la empresa quedan actualizados.
```
  
  
### CU-08: Aplicar descuentos o pruebas gratuitas.  
```
• Nombre del caso de uso: Aplicar descuentos o pruebas gratuitas.   
• Identificador: CU-08.  
• Objetivo: Asignar un descuento promocional o un periodo de prueba gratuita a la suscripción de una empresa cliente.    
• Actor o actores participantes:  
                    1. Responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión en el sistema y la empresa cliente está registrada.  
• Flujo principal:  
                    1. El Responsable selecciona la opción de añadir una promoción sobre la empresa cliente.   
                    2. El Responsable introduce el porcentaje de descuento y su fecha de caducidad.  
                    3. El sistema aplica los nuevos parámetros y registra las modificaciones.    
• Flujos alternativos (No existen en este caso).      
• Flujos de excepción, cuando corresponda:
                    2. Error en la fecha o el porcentaje de descuento.     
• Postcondiciones:  
                    1. El descuento o la prueba gratuita queda aplicada a la empresa cliente hasta la fecha asignada.  
```


### CU-09: Validar cambios de facturación de gran tamaño.  
```
• Nombre del caso de uso: Validar cambios de facturación de gran tamaño.   
• Identificador: CU-09.  
• Objetivo: Revisar la facturación para un posible cambio de plan para la empresa cliente.    
• Actor o actores participantes:  
                    1. Superadministrador.  
• Precondiciones:  
                    1. El Superadministrador ha iniciado sesión en el sistema, la empresa cliente está registrada y tiene una notificación de revisión de cambio de factura.  
• Flujo principal:  
                    1. El Superadministrador accede a la ficha de la empresa cliente a revisar.   
                    2. El sistema muestra la diferencia de facturación.  
                    3. El Superadministrador realiza la modificación del cambio de plan.
                    4. El sistema aplica el cambio y registra las modificaciones.      
• Flujos alternativos (No existen en este caso).
                    3. El Superadministrador no realiza la modificación.    
• Flujos de excepción, cuando corresponda: (ninguno en este caso)       
• Postcondiciones:  
                    1. La notificación de revisión de cambio de factura desaparece al revisar el plan actual de la empresa cliente.  
```


### CU-10: Eliminar una empresa cliente.  
```
• Nombre del caso de uso: Eliminar una empresa cliente.   
• Identificador: CU-10.  
• Objetivo: Dar de baja una empresa cliente en la operatoria activa conservando sus datos históricos y fiscales.    
• Actor o actores participantes:  
                    1. Administrador.  
• Precondiciones:  
                    1. El Administrador ha iniciado sesión y la empresa cliente está registrada como activa.  
• Flujo principal:  
                    1. El Administrador elimina la empresa cliente.   
                    2. El sistema manda una advertencia.  
                    3. El Administrador confirma y elimina finalmente la empresa.
                    4. El sistema borra los datos aunque mantiene las facturas en la base de datos y registra las modificaciones.      
• Flujos alternativos:
                    2. El sistema impide la eliminación por impagos pendientes.
                    3. El Administrador decide no realizar la modificación.    
• Flujos de excepción, cuando corresponda: (ninguno en este caso)       
• Postcondiciones:  
                    1. El cliente deja de estar activo para nuevas facturaciones automáticas, aunque sus datos de facturas siguen almacenados en la base de datos.    
```

### CU-11: Gestión de empresas clientes.  
```
• Nombre del caso de uso: Gestión de clientes y usuarios.   
• Identificador: CU-11.  
• Objetivo: Gestionar las empresas clientes.    
• Actor o actores participantes:  
                    1. Responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión y tiene acceso a la empresa cliente.  
• Flujo principal:  
                    1. El responsable accede al menú de la empresa cliente.   
                    2. El sistema muestra los datos.  
                    3. El responsable activa la edición y cambia datos como los datos fiscales.
                    4. El sistema actualiza los datos de la empresa cliente.      
• Flujos alternativos (No existen en este caso).    
• Flujos de excepción, cuando corresponda:
                    4. El sistema rechaza los datos por invalidez de los mismos o error de formato.
• Postcondiciones:  
                    1. Los datos del cliente cambian y se modifica la base de datos.    
```

## Especificación del diagrama 3 (Gestión de facturas e impagos):

### CU-12: Crear factura adicional.
```
• Nombre del caso de uso: Crear factura adicional.  
• Identificador: CU-12.  
• Objetivo: Emitir una factura puntual para un cliente al margen de su facturación mensual recurrente.  
• Actor o actores participantes:  
                    1. Responsable responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión y tiene acceso asignado al menos a un cliente activo.  
• Flujo principal:  
                    1. El Responsable selecciona un cliente y crea una factura adicional. Adicionalmente, puede adjuntar archivos a la factura.   
                    2. El Responsable introduce los conceptos e importes adicionales.  
                    3. El sistema verifica que la factura adicional no tiene errores.  
                    4. El sistema genera y envía la factura al cliente y la añade a la base de datos como pendiente de pago.  
• Flujos alternativos.:
                    4. El sistema guarda la factura para que se emita junto a la del mes próximo.  
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. La nueva factura queda creada y almacenada.
```

  
### CU-13: Consultar y descargar facturas.
```
• Nombre del caso de uso: Consultar y descargar facturas.  
• Identificador: CU-13.  
• Objetivo: Visualizar el listado de las facturas de los clientes asignados y poder descargarlas.  
• Actor o actores participantes:  
                    1. Responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión y tiene acceso asignado al menos a un cliente.
• Flujo principal:  
                    1. El Responsable accede a la sección de facturación.  
                    2. El sistema muestra las facturas correspondientes a los clientes a los que el responsable tiene acceso.  
                    3. El Responsable selecciona una factura para ver su detalle y pulsa la opción de descarga.
                    4. El sistema genera y descarga el documento de la factura y notifica a la base de datos.  
• Flujos alternativos. (No existen en este caso)  
• Flujos de excepción:    
                    4. Error al generar el archivo de descarga. 
• Postcondiciones:  
                      1. El Responsable visualiza y descarga el archivo.  
```


### CU-14: Filtrar facturas.
```
• Nombre del caso de uso: Filtrar facturas.  
• Identificador: CU-14.  
• Objetivo: Acotar el listado de facturas según su estado de cobro o rango de fechas.  
• Actor o actores participantes:  
                    1. Responsable responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión y tiene acceso asignado al menos a un cliente
• Flujo principal:  
                    1. El Responsable selecciona uno o más criterios de filtrado.  
                    2. El sistema muestra las facturas correspondientes a los criterios seleccionados.  
• Flujos alternativos.   
                    2. No existen facturas para los criterios seleccionados.
• Flujos de excepción (no hay).
• Postcondiciones:  
                      1. Se muestra el listado de facturas en pantalla.  
```    


### CU-15: Pausar recordatorios de factura impagada.
```
• Nombre del caso de uso: Pausar recordatorios de factura impagada.  
• Identificador: CU-15.  
• Objetivo: Detener temporalmente el envío de correos automáticos de reclamación de pago para una factura pendiente.  
• Actor o actores participantes:  
                    1. Responsable responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión existe una factura sin pagar.
• Flujo principal:  
                    1. El Responsable selecciona una factura sin pagar y la pone en estado de "pausa".  
                    2. El sistema cambia el estado a "pausa".    
                    3. El sistema deja de enviar recordatorios de impago.    
• Flujos alternativos.:
                    1. El Responsable selecciona una factura en "pausa" y la reanuda. 
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. La factura para a estar marcada como "en pausa".
```


### CU-16: Cancelar deuda de factura.
```
• Nombre del caso de uso: Cancelar deuda de factura.  
• Identificador: CU-16.  
• Objetivo: Dar por saldada manualmente una deuda cambiando el estado de una factura de "impagada" a "pagada".  
• Actor o actores participantes:  
                    1. Superadministrador.  
• Precondiciones:  
                    1. El Superadministrador ha iniciado sesión existe una factura sin pagar.
• Flujo principal:  
                    1. El Superadministrador selecciona una factura sin pagar y la pone en estado "saldada".  
                    2. El sistema solicita confirmación de seguridad y justificación.     
                    3. El Superadministrador confirma la operación.
                    4. El sistema actualiza la factura al estado "pagada" y detiene los recordatorios, modificando la base de datos así.
• Flujos alternativos.:
                    3. Operación cancelada por el superadministrador por lo que la factura mantiene su estado de impago. 
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. La deuda pasa a estar saldada y la factura a pagada.
```


### CU-17: Configurar plantilla de email de facturación.
```
• Nombre del caso de uso: Configurar plantilla de email de facturación.  
• Identificador: CU-17.  
• Objetivo: Definir y personalizar la plantilla de correo electrónico (incluyendo idioma español o inglés) que se utilizará al enviar facturas a los clientes.  
• Actor o actores participantes:  
                    1. Responsable responsable.  
• Precondiciones:  
                    1. El Responsable responsable ha iniciado sesión y tiene permisos sobre el cliente.
• Flujo principal:  
                    1. El Responsable accede a la configuración de comunicaciones del cliente.  
                    2. El Responsable selecciona o edita la plantilla de correo de facturación y define el idioma (español o inglés).    
                    3. El sistema actualiza la base de datos cambiando la configuración del cliente.
• Flujos alternativos (no hay).
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. La nueva plantilla queda activa para los siguientes envíos.  
```
    
  
### CU-18: Configurar objetivos de recaudación.
```
• Nombre del caso de uso: Configurar cobros y objetivos de recaudación.  
• Identificador: CU-18.  
• Objetivo: Establecer metas de cobro y visualizar su progreso en el panel de control.  
• Actor o actores participantes:  
                    1. Administrador.  
• Precondiciones:  
                    1. El Administrador ha iniciado sesión en el sistema.  
• Flujo principal:  
                    1. El Administrador accede a la configuración de objetivos y establece uno nuevo.  
                    2. El sistema guarda el objetivo actual.  
                    3. El sistema actualiza el panel de control para mostrar el progreso del objetivo nuevo.    
• Flujos alternativos (no hay).  
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. El nuevo objetivo queda configurado y su progreso es visible.  
```


### CU-19: Sincronizar estado de pagos con Stripe y confirmar cobro.
```
• Nombre del caso de uso: Sincronizar estado de pagos con Stripe y confirmar cobro.  
• Identificador: CU-19.  
• Objetivo: Registrar los pagos con Stripe y enviar una confirmación de pago al cliente.  
• Actor o actores participantes:  
                    1. El Sistema de pagos.  
• Precondiciones:  
                    1. Existen facturas emitidas pendientes y existe una conexión registrada con Stripe.  
• Flujo principal:  
                    1. El sistema consulta o recibe eventos de pago desde Stripe para almacenarlos en la base de datos.  
                    2. El sistema recibe la confirmación de pago de Stripe y cambia el estado de la factura a "pagada".  
                    3. El sistema envía una confirmación de pago al cliente.
                    4. El sistema registra la operación en el historial de procesos.    
• Flujos alternativos (no hay)  
• Flujos de excepción:
                    1. La conexión con Stripe se cae. 
• Postcondiciones:  
                      1. Todas las facturas abonadas pasan al estado "pagada" y la empresa cliente recibe su justificante de pago.
```
    
## Especificación del diagrama 4 (Sistema de comunicación interno):

### CU-20: Consultar panel de control.
```
• Nombre del caso de uso: Consultar panel de control.  
• Identificador: CU-20.  
• Objetivo: Mostrar en un panel las incidencias pendientes de atender.  
• Actor o actores participantes:  
                    1. Responsable responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión.  
• Flujo principal:  
                    1. El Responsable accede al panel de seguimiento.  
                    2. El sistema muestra las incidencias pendientes de atender.  
                    3. El Responsable pulsa sobre cualquiera de las incidencias y se le muestra dicha incidencia para revisarla.    
• Flujos alternativos:
                    2. El sistema no tiene ninguna incidencia que mostrar.  
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. El Responsable visualiza las incidencias.  
```
  
  
### CU-21: Gestionar incidencias y comentarios.
```
• Nombre del caso de uso: Crear las incidencias y comentarios.  
• Identificador: CU-21.  
• Objetivo: Avisar de posibles errores o incidencias para su posterior resolución.  
• Actor o actores participantes:  
                    1. Responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión y está en la ficha de una empresa cliente.  
• Flujo principal:  
                    1. El Responsable selecciona la opción de abrir una incidencia o comentario.  
                    2. El sistema guarda la incidencia o el comentario y genera una notificación interna al empleado mencionado.  
• Flujos alternativos (no hay).  
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. La incidencia o comentario queda registrado en el historial y el destinatario recibe la notificación.  
```
  
  
### CU-22: Gestionar notificaciones internas.
```
• Nombre del caso de uso: Gestionar notificaciones internas.  
• Identificador: CU-22.  
• Objetivo: Consultar los avisos que puedan surgir y resolverlos.  
• Actor o actores participantes:  
                    1. Responsable responsable.  
• Precondiciones:  
                    1. El Responsable ha iniciado sesión y tiene notificaciones pendientes.  
• Flujo principal:  
                    1. El Responsable abre la bandeja de notificaciones y las revisa.  
                    2. El Responsable marca la notificación como resuelta.  
                    3. El sistema actualiza el estado de la notificación y modifica la base de datos, eliminándola de la lista de pendientes.    
• Flujos alternativos (no hay).  
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. El estado de la notificación pasa a leída o resuelta.  
```

  
## Especificación del diagrama 5 (Tareas del superadministrador):

### CU-23: Consultar y filtrar informes financieros.
```
• Nombre del caso de uso: Consultar y filtrar informes financieros.  
• Identificador: CU-23.  
• Objetivo: Obtener y filtrar reportes del estado financiero de Turbineh.  
• Actor o actores participantes:  
                    1. Administrador.  
• Precondiciones:  
                    1. El administrador ha iniciado sesión.  
• Flujo principal:  
                    1. El administrador accede a la pestaña de informes financieros.  
                    2. El sistema muestra los informes financieros mensuales, trimestrales y anuales.  
                    3. El administrador filtra por cliente/periodo/responsable responsable/país/estado de factura (pagada o no)/tipo de factura (recurrente o adicional).
                    4. El sistema muestra los informes con los filtros aplicados.      
• Flujos alternativos (no hay).  
• Flujos de excepción (no hay).  
• Postcondiciones:  
                      1. El administrador obtiene el informe según los filtros aplicados.  
```
  
  
### CU-24: Revisión de historiales y copias de seguridad.
```
• Nombre del caso de uso: Revisión de historiales y copias de seguridad.  
• Identificador: CU-24.  
• Objetivo: Supervisar los cambios manuales y el estado de las copias de seguridad.  
• Actor o actores participantes:  
                    1. Superadministrador.  
• Precondiciones:  
                    1. El superadministrador ha iniciado sesión.  
• Flujo principal:  
                    1. El superadministrador accede a la pestaña de revisión y copias de seguridad. 
                    2. El superadministrador filtra la vista a revisar.  
                    3. El sistema muestra los registros detallados con fecha y hora, responsable que lo modificó, destinatario y resultado de la operación.
                    4. El superadministrador puede generar una copia de seguridad o descargar los archivos.      
• Flujos alternativos (no hay).  
• Flujos de excepción:
                    2. Los filtros del superadministrador son erróneos.  
• Postcondiciones:  
                    1. El administrador verifica los informes y genera copia de seguridad.  
```
  

### CU-25: Edición de la tabla de precios recurrentes.
```
• Nombre del caso de uso: Edición de la tabla de precios recurrentes.  
• Identificador: CU-25.  
• Objetivo: Actualizar las tarifas preestablecidas y reajustar precios.  
• Actor o actores participantes:  
                    1. Superadministrador.  
• Precondiciones:  
                    1. El superadministrador ha iniciado sesión.  
• Flujo principal:  
                    1. El superadministrador accede a la configuración de rangos de precios. 
                    2. El superadministrador modifica los rangos de precios.  
                    3. El sistema valida que no haya incoherencias (errores de formato o solapamientos entre rangos de precios).
                    4. El superadministrador confirma los cambios y el sistema actualiza la base de datos, cambiando la tabla de referencia.      
• Flujos alternativos (no hay).  
• Flujos de excepción:
                    3. Se produce un solapamiento entre los rangos de los precios o hay error de formato.
• Postcondiciones:  
                    1. La tabla de rangos de los precios queda actualizada y se envía una notificación a la empresas clientes.  
```  
  

### CU-26: Consultar estado del sistema.
```
• Nombre del caso de uso: Consultar estado del sistema.  
• Identificador: CU-26.  
• Objetivo: Visualizar el estado técnico del sistema y garantizar su estabilidad.  
• Actor o actores participantes:  
                    1. Superadministrador.  
• Precondiciones:  
                    1. El superadministrador ha iniciado sesión.  
• Flujo principal:  
                    1. El superadministrador accede a la pestaña de estabilidad del sistema. 
                    2. El sistema recopila las métricas técnicas y el estado del servidor.  
                    3. El sistema muestra la información al Superadministrador.
                    4. El superadministrador confirma los cambios y el sistema actualiza la base de datos, cambiando la tabla de referencia.      
• Flujos alternativos (no hay).  
• Flujos de excepción (no hay).
• Postcondiciones:  
                    1. El Superadministrador visualiza el estado técnico de la plataforma.  
```
  
  
### CU-27: Revertir acciones previas.
```
• Nombre del caso de uso: Revertir acciones previas.  
• Identificador: CU-27.
• Objetivo: Deshacer una modificación o acción reciente en el sistema.  
• Actor o actores participantes:  
                    1. Superadministrador.  
• Precondiciones:  
                    1. El superadministrador ha iniciado sesión.  
• Flujo principal:  
                    1. El superadministrador accede al historial de cambios. 
                    2. El superadministrador selecciona la opción de "Revertir".  
                    3. El sistema valida que la acción es legalmente reversible.
                    4. El sistema restaura el estado anterior de los datos y registra la revesión.  
• Flujos alternativos (no hay).  
• Flujos de excepción:
                    3. La acción no se puede revertir por motivos legales.
• Postcondiciones:  
                    1. El Superadministrador revierte la acción y el sistema recupera su estado previo.  
```
    
