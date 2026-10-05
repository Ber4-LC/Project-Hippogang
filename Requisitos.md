# Requisitos 

## Requisitos de negocio 
Descripción de las necesidades y objetivos de negocio que motivan el desarrollo del sistema. 
El sistema busca facilitar la parte relacionada a la facturación del producto a los distintos clientes.
Será utilizada internamente dentro de la empresa y busca organizar las facturas, automatizar el envío de estas y el envío de recordatorios de pago a los clientes.

## Requisitos de usuario 
Los usuarios (empleados de la empresa) deberán poder acceder a las facturas relevantes a ellos, consultarlas y descargarlas.

## Requisitos del sistema 

### Requisitos Funcionales 
- FR1. El rol admin debe poder registrar nuevos clientes y asignarlos a empleados del rol lector.
- FR2. El rol lector debe poder consultar las facturas de los clientes a los que esté asignado, así como descargarlas.
- FR3. El rol admin debe poder editar a qué clientes tiene acceso un empleado lector.
- FR4. El rol admin debe poder eliminar un cliente.
- FR5. El rol superadmin debe poder editar varios parámetros para cada clientes: importe recurrente, importes adicionales, frecuencia de recordatorios de pago.
- FR6. Los usuarios deben poder iniciar sesión con su email @turbineh.com y su contraseña, entrando con los privilegios correspondientes a su rol.
- FR7. Los usuarios deben poder crear facturas adicionales a las mensuales.
- FR8. El rol superadmin debe poder consultar el historial de cambios y facturas y tener acceso a las copias de seguridad.
- FR9. Los usuarios deben poder filtrar las facturas (pagadas/impagadas, por fecha, recurrente/adicional)
- FR10. Se debe poder acceder a la información del sistema a través de una API.
- FR11. El rol admin debe poder importar datos mediante archivos csv. (Baja prioridad)

### Requisitos No-Funcionales 
- NFR1. El sistema debe producir facturas mensuales a cada cliente el primer día del mes, así como las facturas adicionales.
- NFR2. El sistema debe ser responsive, una aplicación web que funcione en dispositivos móviles correctamente.
- NFR3. El proceso de facturar debe poder realizarse en menos de un minuto.
- NFR4. EL sistema debe enviar un flujo automático de recordatorios de pago, en un principio 3 días y 5 días después de la factura, y luego cada 2 días.
- NFR5. EL sistema debe hacer un seguimiento del estado de pagos de todas las facturas automáticamente mediante conexión con Stripe.
- NFR6. El sistema enviará una confirmación automática al cliente cuando las facturas sean pagadas con éxito.
- NFR7. El sistema debe comprobar cuando un usuario se loguee que el correa sea @turbineh.com
- NFR8. Cuando un cliente sea eliminado por un admin, sus datos deben permanecer en la base de datos tanto tiempo como la ley permita.
- NFR9. El sistema debe enviar a los roles admin un seguimiento mensual/trimestral/anual de las finanzas de la empresa.
- NFR10. Las facturas deben ser estándar y válidas fiscalmente.
- NFR11. Los datos de las facturas están bien cifrados para no tener problemas legales.