
Arquitectura del Sistema 


1. Enfoque del diseño

Para la arquitectura de la aplicación hemos optado por un diseño sencillo y abstracto centrado en mostrar el flujo principal de datos y el control de acceso, sin entrar en detalles complejos de infraestructura.

La idea principal ha sido separar la lógica central de nuestra aplicación tanto de las comprobaciones previas de seguridad como de los servicios externos (base de datos y pasarela de pagos), dejando clara la responsabilidad de cada parte.

2. Componentes y flujo del sistema

El diagrama se divide en tres bloques principales según su función:

Control de acceso (Izquierda)

Hemos sacado las comprobaciones de seguridad fuera del bloque principal para filtrar las peticiones antes de que se ejecuten:

Usuario y Validar la Sesión: Lo primero que se comprueba al interactuar con el sistema es que el usuario tenga una sesión iniciada y válida.

Solicitud y Comprobación de permisos: Una vez validada la sesión, el usuario lanza su petición y se verifica si tiene permisos para realizarla:

No: Se deniega el paso y vuelve atrás, evitando accesos no autorizados.

Sí: La solicitud entra al bloque principal de la aplicación para ser procesada.

Núcleo de la Aplicación (Bloque central)

Aquí reside la lógica principal de nuestro proyecto. Dentro hemos destacado dos módulos clave que interactúan con el exterior:

Actualiza pagos realizados: Recibe la confirmación cuando se completa un cobro y actualiza el estado correspondiente dentro de la aplicación.

Notificar cliente: Se encarga de enviar avisos o confirmaciones al usuario cuando ocurre algún cambio relevante.

Servicios externos y persistencia (Bloques verdes)

Se diferencian del bloque principal porque son elementos externos al núcleo de la aplicación:

Pasarela de pagos: La mantenemos separada por seguridad, evitando gestionar datos bancarios directamente en nuestro sistema. Cuando un pago se completa, notifica al módulo de Actualiza pagos realizados.

Base de Datos: Almacena toda la información de forma persistente. Recibe las actualizaciones desde la aplicación y cuenta con una salida condicional (Si evento requiere) hacia Notificar cliente para disparar avisos al usuario cuando se registra un cambio importante.

3. Justificación de la estructura

Seguridad: Ninguna petición llega a la lógica principal sin haber pasado antes por la validación de sesión y permisos.

Mantenibilidad: Al tener la base de datos y la pasarela de pagos separadas del núcleo, podemos modificar cualquiera de los dos servicios en el futuro sin tener que rehacer toda la aplicación.
