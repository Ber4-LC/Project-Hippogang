Importante: Se entiende que si en actor se pone un usuario, todos los que están por encima de ese rol también pueden llevar a cabo ese trabajo.

# Especificación de los casos de uso
## Especificación del diagrama 1 (Autenticación y permisos de los roles de la base de datos):

### CU-01: Iniciar sesión.
```
• Nombre del caso de uso: Iniciar sesión.  
• Identificador: CU-01.  
• Objetivo: Autenticar a un empleado interno de Turbine y habilitarle las funcionalidades correspondientes a su rol.  
• Actor o actores participantes:  
                    1. Empleado.  
                    2. Empleado (lector).  
                    3. Administrador.  
                    4. Superadministrador.  
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
                    1. Empleado sobre el que se hace la asignación/modificación.  
                    2. Administrador o superadministrador que se encarga de gestionar los permisos de dicho empleado.  
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
                    3. El sistema valida que estén completos los campos obligatorios.
                    4. El sistema almacena al nuevo cliente y registra la acción.    
• Flujos alternativos:
                    3. El cliente existía anteriormente y fue eliminado pero como la base de datos conserva sus datos aún, se ofrece la posibilidad de reactivar su registro.  
• Flujos de excepción, cuando corresponda:  
                    3. Datos fiscales incompletos o inválidos.  
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
                    1. Usuario responsable.  
• Precondiciones:  
                    1. El Usuario responsable ha iniciado sesión en el sistema y tiene permisos para editar la ficha de la empresa cliente.  
• Flujo principal:  
                    1. El Usuario responsable accede a la ficha del cliente.   
                    2. El Usuario responsable añade un nuevo contacto, modifica los datos existentes o elimina un contacto.
                    3. El sistema valida la operación y la registra.  
• Flujos alternativos (No existen en este caso).      
• Flujos de excepción, cuando corresponda:
                    2. El Usuario responsable deja sin contacto a la empresa y salta un error.    
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
                    2. El formato del importe o el mismo importe no son válidos.    
• Postcondiciones:  
                    1. Los parámetros de facturación de la empresa quedan actualizados.
```
  
  
### CU-08: Aplicar descuentos o pruebas gratuitas.  
```
• Nombre del caso de uso: Aplicar descuentos o pruebas gratuitas.   
• Identificador: CU-08.  
• Objetivo: Asignar un descuento promocional o un periodo de prueba gratuita a la suscripción de una empresa cliente.    
• Actor o actores participantes:  
                    1. Administrador.  
• Precondiciones:  
                    1. El Administrador ha iniciado sesión en el sistema y la empresa cliente está registrada.  
• Flujo principal:  
                    1. El administrador selecciona la opción de añadir una promoción sobre la empresa cliente.   
                    2. El administrador introduce el porcentaje de descuento y su fecha de caducidad.  
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
• Flujos alternativos (No existen en este caso).
                    2. El sistema impide la eliminación por impagos pendientes.
                    3. El Administrador decide no realizar la modificación.    
• Flujos de excepción, cuando corresponda: (ninguno en este caso)       
• Postcondiciones:  
                    1. El cliente deja de estar activo para nuevas facturaciones automáticas, aunque sus datos de facturas siguen almacenados en la base de datos.    
```     
