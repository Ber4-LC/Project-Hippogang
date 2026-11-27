# Justifiación del diagrama de arquitectura del sistema
Como se puede observar en el diagrama de arquitectura, hemos tomado un enfoque muy simple con respecto a la estructura de la aplicación.
Teniendo en cuenta los requisitos especificados por el cliente durante la reunión: la aplicación es para uso interno dentro de la empresa, para
optimizar la gestión de facturas hacia los clientes. Actualmente la empresa cuenta con pocos empleados, pero el cliente quiere que la aplicación
sea escalable hasta una cantidad de 200 usuarios concurrentes para preveer el crecimiento de la empresa. 

Como la aplicación tiene que ser rápida y eficiente, y 200 usuarios concurrentes es bastante poco, optamos por el modelo cliente-servidor, y en este caso
solo haría falta 1 servidor, puesto que no habrá miles de usuarios accediendo a la vez, solamente los empleados de turbineH. Por tanto, no necesitamos
un balanceador de carga y varios servidores.

Es importante que los usuarios estén separados de la base de datos y no puedan acceder a información que no les corresponde, por lo que los usuarios
interactuarán con una interfaz frontend, luego las peticiones van al servidor donde se comprobarán permisos y se harán todas las operaciones de lógica de
negocio, y es solamente el servidor el que está conectado a la base de datos.

En cuanto a las empresas cliente, como hemos dicho, la aplicación es interna, por lo que la única interacción es que el sistema enviará los recordatorios
y las facturas a los contactos de las empresas según su medio de comunicación, y recibirá los pagos del serivicio externo que los maneja (en principio Stripe).
