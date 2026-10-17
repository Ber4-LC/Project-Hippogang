# Especificación de los casos de uso
## Especificación del diagrama 1 (Autenticación y permisos de los roles de la base de datos):

### CU-01: Iniciar sesión
• Nombre del caso de uso: Iniciar sesión
• Identificador: CU-01
• Objetivo: Autenticar a un empleado interno de Turbine y habilitarle las funcionalidades correspondientes a su rol.
• Actor o actores participantes: 
                    1. Empleado 
                    2. Empleado (lector)
                    3. Administrador
                    4. Superadministrador.
• Precondiciones:   
                    1. El usuario debe tener una cuenta dada de alta en el sistema con un correo corporativo.
• Flujo principal: 
                    1. El usuario inicia sesión con sus credenciales.
                    2. El sistema comprueba que tiene el dominio @turbineh.com y posteriormente la validez de las credenciales.
                    3. Se realiza el inicio de sesión del usuario con su vista y privilegios asociados a dicho rol.
• Flujos alternativos. (No existen en este caso)
• Flujos de excepción, cuando corresponda.
                    2a. Correo que no pertenece al dominio @turbineh.com.
                    2b. Fallo en inicio de sesión a consecuencia de credenciales incorrectas.
• Postcondiciones.
                    1. El usuario queda autenticados con sus vistas y privilegios dependiendo de su rol.


### CU-02: Gestionar permisos de empleados
• Nombre del caso de uso: Gestionar permisos de empleados
• Identificador: CU-02
• Objetivo: Asignar o modificar la lista de empresas clientes que un empleado con rol lector puede visualizar.
• Actor o actores participantes: 
                    1. Empleado sobre el que se hace la asignación/modificación.
                    2. Administrador o superadministrador que se encarga de gestionar los permisos de dichi empleado.
• Precondiciones: 
                    1. El administrador ha iniciado sesión en el sistema (CU-01) y existen tanto el empleado como las empresas clientes registradas.
• Flujo principal: 
                    1. El administrador accede a la sección de gestión de permisos de empleados y selecciona un empleado lector o responsable.
                    2. El sistema muestra el listado actual de empresas clientes asignadas a dicho empleado.
                    3. El administrador añade nuevas empresas clientes o retira el acceso a clientes existentes y confirma los cambios.
                    4. El sistema actualiza los permisos de acceso del empleado y registra la modificación en el historial de cambios.
• Flujos alternativos.
                    3. El administrador cancela la operación antes de guardar y el sistema mantiene las asignaciones previas.
• Flujos de excepción, cuando corresponda.
                    4. El sistema notifica que no se pudieron actualizar los permisos y revierte el cambio.
• Postcondiciones.
                    1. El empleado seleccionado únicamente puede consultar la información y facturas de la nueva lista de clientes asignados.


### CU-03: Consultar información mediante la API
• Nombre del caso de uso: Consultar información mediante la API
• Identificador: CU-03
• Objetivo: Acceso a la API por parte de los empleados con el fin de consultar información.
• Actor o actores participantes: 
                    1. Empleado que solicita la consulta de información.
• Precondiciones: 
                    1. El empleado dispone de los permisos requeridos para la consulta y la empresa cliente sobre la cual se hace la consulta tiene datos válidos.
• Flujo principal: 
                    1. El empleado realiza una petición a la API solicitando los datos.
                    2. El sistema valida los permisos del empleado y manda los datos de la petición.
                    3. El sistema registra la consulta.
• Flujos alternativos. (No existen en este caso)
• Flujos de excepción, cuando corresponda.
                    2. El sistema deniega la consulta debido a la inexistencia de los permisos por parte del empleado.
• Postcondiciones.
                    1. El empleado solicitante de la información recibe dichos datos.                    
