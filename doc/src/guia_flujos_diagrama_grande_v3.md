# Guía rápida de flujos del sistema a partir del diagrama de clases

Esta guía está pensada para usarla junto al **diagrama de clases grande
actual** y ayudar a sacar los diagramas de secuencia.

No intenta explicar clase por clase ni definir cada método. La idea es
responder a:

> **Cuando el sistema tiene que hacer X, ¿qué objetos participan y en
> qué orden?**

En cada apartado aparecen también los **casos de uso (CU)** y
**requisitos funcionales (FR)** que conviene mirar para hacer la
secuencia.

Cuando una parte todavía no está completamente cerrada en el diagrama,
se indica como **Pendiente / a revisar** para que no inventemos
comportamiento sin darnos cuenta.

------------------------------------------------------------------------

# 0. Antes de empezar: el "camino principal" del sistema

Si solo queréis entender primero el núcleo del sistema, empezad por este
recorrido:

``` text
EmpresaCliente
    ↓
Suscripcion
    ↓
SuscripcionManager.generarFacturasMensuales()
    ↓
FacturaManager.crearFacturaRecurrente(...)
    ↓
Factura
    ↓
MensajeManager.crearMensaje(...)
    ↓
Mensaje.enviarMensaje()
    ↓
cliente recibe la factura

más adelante...

ServicioPagos
    ↓
PagoManager
    ↓
Pago
    ↓
Factura pasa a PAGADA
    ↓
MensajeManager
    ↓
confirmación de pago
```

Ese es el flujo central. El resto del sistema se va conectando alrededor
de él: usuarios y permisos, incidencias, aprobaciones, auditoría,
informes, periodos económicos, etc.

------------------------------------------------------------------------

# 1. Inicio de sesión

**Relacionado:** CU-01 · FR-14 · FR-42

``` text
Empleado de TurbineH
    ↓
AutenticacionManager.getInstancia()
    ↓
AutenticacionManager.iniciarSesion(email, password)
    ↓
AutenticacionManager.validarDominioCorporativo(email)
    ↓
se localiza Usuario por email
    ↓
se comprueba:
    - dominio @turbineh.com
    - credenciales correctas
    - Usuario.activo = true
    ↓
se crea Sesion
    ↓
Sesion queda asociada a Usuario
```

Después del login, `Usuario.rol` determina qué operaciones puede
realizar. La autenticación responde a quién es el usuario; el rol se usa
después para autorización.

## Cierre de sesión

``` text
Empleado
    ↓
AutenticacionManager.getInstancia()
    ↓
AutenticacionManager.cerrarSesion(sesion)
    ↓
Sesion.cerrar()
```

## Desactivar un usuario

``` text
Administrador
    ↓
Usuario.desactivar()
    ↓
AutenticacionManager.getInstancia()
    ↓
AutenticacionManager.invalidarSesiones(usuario)
    ↓
se cierran/inactivan sus Sesion activas
```

Esto cubre FR-42.

### Participantes útiles para una secuencia

``` text
Empleado de TurbineH
AutenticacionManager
Usuario
Sesion
```

# 2. Dar o quitar acceso de un empleado a empresas

**Relacionado:** CU-02 · FR-07 · FR-40

Un administrador puede modificar qué empresas puede consultar un
empleado.

``` text
Administrador
    ↓
selecciona un Usuario
    ↓
Usuario.darAcceso(empresa)
        o
Usuario.quitarAcceso(empresa)
    ↓
se actualiza la relación Usuario ↔ EmpresaCliente
    ↓
AuditoriaManager.registrarAccion(...)
```

Después de esto, cuando ese empleado consulte facturas o datos, solo
debería recibir información de las empresas que tiene asignadas.

### Nota

`Usuario.darAccesoTemporal(...)` sigue apareciendo en el diagrama, pero
no es una parte central de los casos de uso actuales. No hace falta
usarlo en las secuencias salvo que decidamos mantener esa funcionalidad.

------------------------------------------------------------------------

# 3. Registrar una nueva empresa

**Relacionado:** CU-04 · FR-06 · FR-39 · FR-46 · FR-65

Este es uno de los flujos importantes porque conecta alta de empresa,
suscripción y primera factura.

``` text
Administrador
    ↓
introduce datos de la empresa
    ↓
EmpresaManager.buscarDuplicados(empresa)
    ↓
se comprueban los datos fiscales
    ↓
EmpresaManager.registrarEmpresa(empresa)
    ↓
se guarda EmpresaCliente con sus datos principales
(nombre comercial, razón social, NIF, país, moneda de trabajo, sector, etc.)
    ↓
se crea/asocia su Suscripcion
    ↓
EmpresaCliente.datosFiscalesCompletos()
    ↓
FacturaManager.crearFacturaInicialProrrateada(...)
    ↓
se crea la primera Factura
```

Si el alta ocurre una vez empezado el mes, la primera factura no debería
cobrar el mes completo, sino solo la parte proporcional restante.

Ejemplo conceptual:

``` text
empresa dada de alta el día 20
    ↓
FacturaManager.crearFacturaInicialProrrateada(...)
    ↓
factura proporcional a los días restantes
```

También hay que evitar crear una empresa duplicada. Si existía
anteriormente y estaba dada de baja, el caso de uso permite ofrecer su
reactivación:

``` text
EmpresaManager.buscarDuplicados(...)
    ↓
se encuentra empresa inactiva anterior
    ↓
EmpresaManager.reactivarEmpresa(empresa)
```

### Pendiente / a revisar

El diagrama representa bien el flujo general, pero la creación exacta de
la `Suscripcion` al registrar la empresa sigue sin tener un método
específico.

Además, `EmpresaManager` tiene la dependencia `genera primera factura`
hacia `FacturaManager`, así que en la secuencia del alta tiene sentido
que sea `EmpresaManager` quien termine llamando a
`FacturaManager.crearFacturaInicialProrrateada(...)`.

No inventéis demasiada lógica interna: basta con mostrar que el alta
deja a la empresa preparada con su suscripción y genera la primera
factura prorrateada.

------------------------------------------------------------------------

# 4. Importar empresas mediante CSV

**Relacionado:** CU-05 · FR-10 · FR-39

El CSV es otra forma de llegar al mismo resultado que el alta manual.

``` text
Administrador
    ↓
ImportadorCSV.validarArchivo(archivo)
    ↓
ImportadorCSV.generarVistaPrevia(archivo)
    ↓
el administrador revisa los datos
    ↓
ImportadorCSV.importarEmpresas(archivo)
    ↓
por cada empresa válida:
EmpresaManager.registrarEmpresa(...)
    ↓
se crean las EmpresaCliente válidas
```

La vista previa es importante porque puede haber filas corruptas o
errores de formato.

### Para la secuencia

Tiene sentido usar un `loop` para cada fila/empresa válida del CSV.

### Pendiente / a revisar

El diagrama no representa todavía con mucho detalle la decisión de
"mantener/corregir/ignorar" datos erróneos de la vista previa. Para una
primera secuencia puede tratarse como un `alt` sin crear nuevas clases.

------------------------------------------------------------------------

# 5. Editar los contactos de una empresa

**Relacionado:** CU-06 · FR-18 · FR-54

Los contactos son las personas externas que reciben las comunicaciones.

Añadir:

``` text
Responsable
    ↓
EmpresaCliente.añadirPersonaDeContacto(...)
    ↓
se crea/asocia Contacto
```

Editar:

``` text
Responsable
    ↓
EmpresaCliente.editarPersonaDeContacto(...)
    ↓
se actualiza Contacto
```

Eliminar:

``` text
Responsable
    ↓
EmpresaCliente.eliminarPersonaDeContacto(...)
```

El sistema no debería permitir dejar a la empresa sin un contacto
válido.

Además, `Contacto.admiteComunicacionesAdministrativas` permite saber si
ese canal admite este tipo de comunicaciones.

------------------------------------------------------------------------

# 6. Editar las condiciones de facturación de una empresa

**Relacionado:** CU-07 · FR-09 · FR-46

El administrador puede cambiar principalmente:

-   importe recurrente;
-   importes adicionales;
-   frecuencia de recordatorios.

Para el importe recurrente:

``` text
Administrador
    ↓
selecciona EmpresaCliente
    ↓
Suscripcion.modificarImporte(nuevoImporte)
```

Para los recordatorios:

``` text
Administrador
    ↓
EmpresaCliente.editarFrecuenciaRecordatorios()
    ↓
se modifica ConfiguracionRecordatorios
```

Antes de activar o aplicar determinadas condiciones de facturación, hay
que comprobar que los datos fiscales necesarios existan:

``` text
EmpresaCliente.datosFiscalesCompletos()
```

Si el cambio económico es grande, entra el sistema de aprobaciones,
explicado más abajo.

------------------------------------------------------------------------

# 7. Descuentos y pruebas gratuitas

**Relacionado:** CU-08 · FR-20

El descuento pertenece a la suscripción.

``` text
Responsable
    ↓
selecciona EmpresaCliente
    ↓
Suscripcion.aplicarDescuento(descuento, diasPeriodo)
    ↓
se guarda descuento + fecha de finalización
```

Mientras el descuento esté vigente, las facturas recurrentes deberían
utilizar el importe resultante de esa suscripción.

------------------------------------------------------------------------

# 8. Cambios económicos grandes y aprobaciones

**Relacionado:** CU-09 · FR-09 · FR-47 · FR-48 · FR-49

Para determinados cambios no debería bastar con que una sola persona los
ejecute directamente.

El flujo general es:

``` text
Empleado interno autorizado intenta hacer cambio económico
    ↓
AprobacionManager.requiereAprobacion(importe)
    ↓
si NO requiere aprobación
    → se aplica normalmente

si SÍ requiere aprobación
    ↓
AprobacionManager.crearSolicitud(...)
    ↓
SolicitudAprobacion queda PENDIENTE
    ↓
otro usuario autorizado la revisa
    ↓
SolicitudAprobacion.aprobar(...)
        o
SolicitudAprobacion.rechazar(...)
```

La persona que crea la solicitud no debería aprobar su propia operación.

### Para la secuencia

Aquí encaja muy bien un bloque:

``` text
alt requiere aprobación
...
else no requiere aprobación
...
end
```

### Pendiente / a revisar

El diagrama tiene la estructura principal, pero no representa todas las
posibles reglas configurables de umbrales y separación de funciones.
Para los diagramas de secuencia no hace falta entrar tanto en detalle.

------------------------------------------------------------------------

# 9. Dar de baja o reactivar una empresa

**Relacionado:** CU-10 · FR-08

Dar de baja no significa borrar todo el histórico.

``` text
Administrador
    ↓
EmpresaManager.darDeBaja(empresa)
    ↓
se comprueba que pueda darse de baja
    ↓
EmpresaCliente.darDeBaja()
    ↓
la empresa queda inactiva
    ↓
no se generan nuevas facturas recurrentes
    ↓
se conserva el histórico necesario
```

Si hay impagos pendientes, el caso de uso permite bloquear la baja.

Una empresa que ya existía y estaba inactiva puede volver:

``` text
EmpresaManager.reactivarEmpresa(empresa)
    ↓
EmpresaCliente.reactivar()
```

------------------------------------------------------------------------

# 10. Gestión general de una empresa

**Relacionado:** CU-11 · UR-03 · FR-39 · FR-45

Cuando un responsable entra en la ficha de una empresa puede consultar y
editar sus datos operativos.

El flujo general sería:

``` text
Responsable
    ↓
abre EmpresaCliente
    ↓
consulta datos
    ↓
activa edición
    ↓
EmpresaCliente.editarDatosFiscales()
        / editar contactos
        / editar configuración permitida
    ↓
se validan los nuevos datos
    ↓
AuditoriaManager.registrarAccion(...)
```

### Pendiente / a revisar

Los favoritos, elementos recientes y algunas "etiquetas" de estado
aparecen en requisitos, pero no están desarrollados como parte principal
del diagrama. No hace falta introducirlos a la fuerza en las primeras
secuencias.

------------------------------------------------------------------------

# 11. Generación automática de facturas mensuales

**Relacionado:** FR-27 · FR-44 · FR-50\
**Proceso automático; no tiene un CU dedicado**

Este es el flujo principal de facturación automática.

``` text
Proceso automático mensual
    ↓
SuscripcionManager.generarFacturasMensuales()
    ↓
recorre suscripciones activas
    ↓
por cada una:
FacturaManager.crearFacturaRecurrente(empresa, suscripcion)
    ↓
FacturaManager.validarFactura(factura)
    ↓
se crea Factura RECURRENTE con sus ConceptoFactura
    ↓
MensajeManager.crearMensaje(...)
    ↓
Mensaje.enviarMensaje()
```

En el diagrama de secuencia debería haber un `loop` por las
suscripciones activas.

Hay que evitar crear dos veces la misma factura si el proceso se lanza
varias veces. El requisito exige idempotencia, aunque en el diagrama no
hemos añadido una clase específica para esto.

### También puede haber facturas adicionales programadas

Durante el ciclo mensual:

``` text
FacturaManager.buscarFacturasProgramadas(empresa)
```

puede recuperar facturas adicionales que se habían dejado para el mes
siguiente.

### Pendiente / a revisar

El `MensajeManager` actual recibe una sola `Factura` en
`crearMensaje(...)`, aunque `Mensaje` contiene
`facturas : List<Factura>` y la relación permite `0..*` facturas por
mensaje. Por tanto, el modelo ya permite conceptualmente que un mensaje
lleve varias facturas, pero la firma del método todavía no refleja del
todo ese caso. No deis por cerrado ese detalle en una secuencia sin
revisarlo.

------------------------------------------------------------------------

# 12. Crear una factura adicional

**Relacionado:** CU-12 · FR-15 · FR-25 · FR-44 · FR-50 · FR-62

Flujo normal:

``` text
Responsable
    ↓
introduce conceptos e importes
    ↓
FacturaManager.crearFacturaAdicional(empresa, importe, false)
    ↓
se crea Factura ADICIONAL
    ↓
se añaden ConceptoFactura
    ↓
opcionalmente ArchivoAdjunto
    ↓
FacturaManager.validarFactura(factura)
    ↓
MensajeManager.crearMensaje(...)
    ↓
Mensaje.enviarMensaje()
```

La factura queda pendiente de pago.

Si se quiere enviar el mes siguiente:

``` text
FacturaManager.crearFacturaAdicional(empresa, importe, true)
    ↓
Factura.programadaProximoMes = true
    ↓
no se envía todavía
```

Después será recogida durante la facturación mensual mediante:

``` text
FacturaManager.buscarFacturasProgramadas(empresa)
```

También existe `Factura.guardarBorrador()` para poder conservar una
factura que todavía no se ha terminado.

------------------------------------------------------------------------

# 13. Consultar, filtrar y descargar facturas

**Relacionado:** CU-13 · CU-14 · FR-13 · FR-16 · FR-56

Filtrar:

``` text
Responsable
    ↓
selecciona filtros
    ↓
FacturaManager.filtrarFacturas(...)
    ↓
List<Factura>
```

Después puede elegir una factura y descargarla:

``` text
Responsable
    ↓
selecciona Factura
    ↓
Factura.descargarFactura()
```

El empleado solo debe ver facturas de empresas a las que tenga acceso.

### Pendiente / a revisar

La conservación/regeneración de documentos contables y la diferencia
entre archivos temporales y documentos definitivos está en requisitos,
pero no está modelada completamente en el diagrama. No hace falta
resolverla para una secuencia básica de consulta/descarga.

------------------------------------------------------------------------

# 14. Recordatorios automáticos de impago

**Relacionado:** FR-28 · FR-57

La empresa tiene una `ConfiguracionRecordatorios` propia y existe además
una configuración por defecto en `ConfiguracionSistema`.

La lógica esperada es:

``` text
Proceso automático
    ↓
MensajeManager.generarRecordatoriosPendientes()
    ↓
busca facturas IMPAGADAS
    ↓
comprueba Factura.recordatoriosPausados
    ↓
consulta ConfiguracionRecordatorios
    ↓
comprueba si hoy toca recordatorio
    ↓
MensajeManager.crearMensaje(... RECORDATORIO ...)
    ↓
Mensaje.enviarMensaje()
```

La configuración permite:

``` text
diasIniciales
+
frecuenciaPosteriorDias
```

por ejemplo:

``` text
día 3
día 5
después cada día
```

### Pendiente / a revisar

Esta parte sigue siendo de las menos cerradas del diseño.

Falta decidir exactamente:

-   cómo se calcula/guarda el próximo envío;
-   qué ocurre al reanudar;
-   cómo se ajusta un recordatorio que cae en fin de semana/festivo.

Para la secuencia se puede representar el flujo general, pero no
inventéis todavía un algoritmo detallado de calendario.

------------------------------------------------------------------------

# 15. Pausar o reanudar recordatorios de una factura

**Relacionado:** CU-15 · FR-17

Pausar:

``` text
Responsable
    ↓
selecciona factura IMPAGADA
    ↓
Factura.pausarRecordatorios()
    ↓
recordatoriosPausados = true
```

Reanudar:

``` text
Responsable
    ↓
Factura.reanudarRecordatorios()
    ↓
recordatoriosPausados = false
```

La pausa afecta a esa factura, no a toda la empresa.

------------------------------------------------------------------------

# 16. Condonar una deuda

**Relacionado:** CU-16 · FR-03

Importante: en los requisitos actuales una deuda condonada **NO se
registra como pagada**.

``` text
Superadmin
    ↓
selecciona factura IMPAGADA
    ↓
introduce confirmación + justificación
    ↓
Factura.condonarDeuda(justificacion)
    ↓
Factura.estado = CONDONADA
    ↓
se detienen recordatorios
    ↓
AuditoriaManager.registrarAccion(...)
```

No se crea un `Pago`, porque la factura no se ha cobrado.

------------------------------------------------------------------------

# 17. Recepción y validación de pagos

**Relacionado:** CU-20 · FR-29 · FR-30 · FR-64

``` text
PagoManager.sincronizarPagos()
    ↓
ServicioPagos.obtenerPagos()
    ↓
por cada pago recibido:
PagoManager.registrarPago(importe, referencia, origen)
    ↓
se crea Pago
    ↓
se intenta identificar la Factura correspondiente
    ↓
PagoManager.validarPago(pago)
```

## Si el pago es correcto

``` text
PagoManager.confirmarPago(pago)
    ↓
EstadoPago = CONFIRMADO
    ↓
Factura.estado = PAGADA
    ↓
MensajeManager
    ↓
Mensaje tipo CONFIRMACION_PAGO
    ↓
Contacto del cliente
```

## Si el pago no cuadra

Por ejemplo, importe/origen incorrecto o factura todavía no
identificada:

``` text
Pago.estado = REQUIERE_CONCILIACION
    ↓
NotificacionManager.crearNotificacion(...)
    ↓
Usuario responsable recibe aviso
```

La relación `Pago -> Factura` es `0..1`, así que el pago puede existir
temporalmente sin una factura identificada.

### Importante

`ServicioPagos` sigue siendo la abstracción de la integración externa,
aunque el requisito actual hable de Stripe.

# 18. Mensajes externos y plantillas

**Relacionado:** CU-18 · FR-12 · FR-21 · FR-30 · FR-32

`Mensaje` representa comunicaciones hacia contactos externos.

Ejemplos:

``` text
FACTURA_RECURRENTE
FACTURA_ADICIONAL
RECORDATORIO
CONFIRMACION_PAGO
AVISO_CAMBIO_PRECIOS
```

Cuando otro proceso necesita enviar algo:

``` text
FacturaManager / PagoManager / otro proceso
    ↓
MensajeManager.crearMensaje(...)
    ↓
se obtiene `EmpresaCliente.plantillaFacturacion`
    ↓
se obtienen los Contacto destinatarios
    ↓
se crea Mensaje
    ↓
Mensaje.enviarMensaje()
```

El mensaje pasa por estados como:

``` text
EN_COLA → ENVIADO → ENTREGADO
                      o
                   FALLIDO → REINTENTADO
```

El campo:

``` text
Mensaje.destinatariosEnvio
```

sirve para conservar quién recibió realmente el mensaje en ese momento,
aunque el contacto cambie más adelante.

### Pendiente / a revisar

La plantilla actual del diagrama es bastante simple. Los requisitos
exigen recordar qué plantilla se utilizó en cada envío; la relación
`Mensaje → Plantilla` ayuda, pero si una plantilla se modifica "en el
sitio" habría que asegurarse de conservar el histórico.

------------------------------------------------------------------------

# 19. Comentarios, menciones e incidencias

**Relacionado:** CU-21 · CU-22 · FR-23 · FR-24 · FR-35 · FR-59

## Abrir una incidencia

En la versión actual ya no hay un `IncidenciaManager`. La creación parte
directamente de la empresa:

``` text
Responsable
    ↓
EmpresaCliente.agregarIncidencia(autor, responsable, contenido, prioridad)
    ↓
se crea Incidencia
    ↓
queda asociada a EmpresaCliente
    ↓
se asignan autor + responsable + prioridad
    ↓
EstadoIncidencia = ABIERTA
```

Después la propia incidencia puede evolucionar:

``` text
Incidencia.cambiarEstado(...)
Incidencia.cambiarPrioridad(...)
```

y admite comentarios mediante:

``` text
Incidencia.agregarComentario(comentario)
```

## Añadir un comentario general a una empresa

``` text
Responsable/Admin
    ↓
EmpresaCliente.agregarComentario(usuario, contenido)
    ↓
se crea y guarda Comentario
```

Si el comentario menciona a empleados:

``` text
Comentario.obtenerMencionados()
    ↓
Comentario.notificarMencionados(...)
    ↓
NotificacionManager.crearNotificacion(...)
```

Las notas administrativas restringidas se distinguen mediante:

``` text
TipoComentario.NORMAL
TipoComentario.NOTA_ADMINISTRATIVA
```

### Pendiente / a revisar

La relación UML indica que cada `Incidencia` tiene un
`responsable : Usuario`, y `EmpresaCliente.agregarIncidencia(...)` lo
recibe, pero ese atributo no aparece escrito dentro de la clase
`Incidencia`. Conviene añadirlo para que atributo y asociación digan lo
mismo.

------------------------------------------------------------------------

# 20. Notificaciones internas

**Relacionado:** CU-23 · FR-19 · FR-35 · FR-36

Las `Notificacion` son internas para empleados. No son correos al
cliente.

Crear una notificación:

``` text
NotificacionManager.crearNotificacion(destinatario, mensaje)
```

Consultar/filtrar:

``` text
NotificacionManager.filtrarNotificaciones(leida, resuelta)
```

Resolver:

``` text
Notificacion.marcarLeida()
Notificacion.marcarResuelta()
```

Ejemplos de cosas que pueden crear notificaciones:

-   mención en comentario;
-   incidencia;
-   pago que necesita conciliación;
-   aprobación pendiente;
-   otros eventos relevantes.

------------------------------------------------------------------------

# 21. Revisión anual de precios

**Relacionado:** FR-33 · FR-04 · CU-26

Además de editar manualmente la tabla de precios, el sistema tiene un
proceso anual que revisa las suscripciones.

``` text
Proceso automático anual
    ↓
SuscripcionManager.revisarPreciosAnuales()
    ↓
recorre Suscripcion
    ↓
TablaPrecios.obtenerPrecio(ingresos)
    ↓
compara precio actual y nuevo
    ↓
si hay cambio relevante:
AprobacionManager
    ↓
validación humana
    ↓
Suscripcion.modificarImporte(nuevoImporte)
```

La revisión no debería cambiar precios sensibles automáticamente sin
validación.

------------------------------------------------------------------------

# 22. Editar la tabla de precios

**Relacionado:** CU-26 · FR-04

``` text
Superadmin
    ↓
ConfiguracionSistema.editarTablaDePrecios()
    ↓
se modifican RangoPrecio
    ↓
TablaPrecios.validarRangos()
    ↓
si todo es válido se guarda
```

Si los rangos se solapan o tienen formato incorrecto, no se aplican.

Después puede generarse un aviso de cambio de precios usando un
`Mensaje` de tipo:

``` text
AVISO_CAMBIO_PRECIOS
```

------------------------------------------------------------------------

# 23. Objetivos de recaudación

**Relacionado:** CU-19 · FR-43

El objetivo ya no se representa mediante una clase `ObjetivoCobro`:
forma parte del `PeriodoEconomico`.

``` text
Administrador
    ↓
PeriodoEconomicoManager.obtenerPeriodo(fechaInicio, tipo)
    ↓
PeriodoEconomico.configurarObjetivo(importe)
    ↓
se guarda objetivoRecaudacion del periodo
```

Para mostrar el avance:

``` text
PeriodoEconomico.calcularTotales()
    ↓
totalRecaudado
    ↓
PeriodoEconomico.calcularProgresoObjetivo()
```

El periodo conserva también `totalFacturado`, `totalRecaudado` y
`totalPendiente`.

Como cada periodo conserva su propio objetivo, los objetivos anteriores
no se sobrescriben al configurar uno nuevo para otro periodo.

# 24. Informes financieros

**Relacionado:** CU-24 · FR-11 · FR-31 · FR-58

Ya no existe una entidad `InformeFinanciero`. Los datos económicos se
obtienen mediante `PeriodoEconomico` y `PeriodoEconomicoManager`.

Para un periodo normal:

``` text
Administrador
    ↓
PeriodoEconomicoManager.obtenerPeriodo(fechaInicio, tipo)
    ↓
PeriodoEconomico.calcularTotales()
    ↓
totalFacturado
totalRecaudado
totalPendiente
```

Para fechas que no correspondan exactamente a un periodo económico:

``` text
Administrador
    ↓
PeriodoEconomicoManager.generarInformePeriodoPersonalizado(fechaInicio, fechaFin)
    ↓
se calculan los datos económicos del intervalo
```

Así no hace falta una clase `InformeFinanciero` solo para representar un
resultado calculado.

### Pendiente / a revisar

Los filtros adicionales exigidos por FR-11 todavía no aparecen como
parámetros de `generarInformePeriodoPersonalizado(...)`.

La conversión entre monedas de FR-58 tampoco tiene todavía un servicio
específico.

# 25. Cerrar un periodo económico

**Relacionado:** CU-17 · FR-51 · FR-52 · FR-53

``` text
Administrador
    ↓
PeriodoEconomicoManager.obtenerPeriodo(fechaInicio, tipo)
    ↓
PeriodoEconomicoManager.validarCierre(periodo)
```

Antes del cierre:

``` text
PeriodoEconomico.calcularTotales()
    ↓
totalFacturado
totalRecaudado
totalPendiente
```

Si todo está correcto:

``` text
PeriodoEconomicoManager.cerrarPeriodo(periodo, formato)
    ↓
PeriodoEconomico.cerrar()
    ↓
estado = CERRADO
fechaCierre = ...
    ↓
se genera PaqueteGestoria
```

Después del cierre, las modificaciones económicas ordinarias quedan
bloqueadas.

## Modificación excepcional

``` text
Administrador autorizado
    ↓
PeriodoEconomicoManager.realizarModificacionExcepcional(periodo, justificacion)
    ↓
AuditoriaManager.registrarAccion(...)
```

Así se mantiene FR-52 de forma controlada y auditable.

# 26. Historial y auditoría

**Relacionado:** CU-25 · CU-28 · FR-01 · FR-02 · FR-40 · FR-63

Muchas operaciones importantes terminan registrándose en auditoría.

Operación manual:

``` text
AuditoriaManager.registrarAccion(...)
    ↓
RegistroAccion
```

Proceso automático:

``` text
AuditoriaManager.registrarProcesoAutomatico(...)
    ↓
RegistroAccion
```

Consultar historial:

``` text
AuditoriaManager.filtrarHistorial(...)
```

## Revertir una operación

``` text
Superadmin
    ↓
selecciona RegistroAccion
    ↓
AuditoriaManager.revertirAccion(registro)
    ↓
se comprueba registro.reversible
    ↓
si está permitido:
se recupera el estado anterior cuando sea posible
    ↓
se registra también la reversión
```

No todas las operaciones pueden revertirse: hay restricciones legales y
contables.

### Importante para las secuencias

No hace falta meter `AuditoriaManager` en absolutamente todas las
secuencias desde el principio.

Usadlo cuando el caso de uso/requisito diga expresamente que la acción
debe registrarse, o cuando sea una operación sensible.

------------------------------------------------------------------------

# 27. Copias de seguridad

**Relacionado:** CU-25 · FR-01 · FR-34

``` text
Superadmin
    ↓
CopiaSeguridadManager.generarCopia()
    ↓
se crea CopiaSeguridad
```

Consultar:

``` text
CopiaSeguridadManager.listarCopias()
```

Restaurar:

``` text
CopiaSeguridadManager.restaurarCopia(copia)
```

Una copia puede estar:

``` text
GENERANDO
DISPONIBLE
FALLIDA
```

------------------------------------------------------------------------

# 28. Consultar estado técnico del sistema

**Relacionado:** CU-27 · FR-37 · FR-38

Este caso de uso existe en los requisitos, pero el diagrama de clases
grande **no tiene un bloque específico de monitorización técnica**.

Conceptualmente:

``` text
Superadmin
    ↓
solicita estado del sistema
    ↓
se recopilan métricas técnicas
    ↓
se muestran estado y fallos
```

### Para los compañeros

No inventéis un `SistemaManager`, `HealthManager`, etc. en la secuencia
sin que antes decidamos añadirlo al diagrama.

Este es uno de los casos de uso que todavía está **poco representado en
el UML**.

------------------------------------------------------------------------

# 29. Consulta mediante API

**Relacionado:** CU-03 · FR-26

También está en los casos de uso pero no tiene una clase específica en
el diagrama.

Conceptualmente:

``` text
Empleado de TurbineH
    ↓
petición API
    ↓
se valida Usuario + acceso a EmpresaCliente
    ↓
se devuelven los datos permitidos
    ↓
AuditoriaManager.registrarAccion(...)
```

Para un diagrama de secuencia definitivo probablemente habría que añadir
una interfaz/controlador de API o decidir el nivel de abstracción que
queremos usar.

------------------------------------------------------------------------

# 30. Qué secuencias haría primero

Para no intentar hacer las 28 de golpe, yo empezaría por estas porque
construyen la historia principal del sistema:

1.  **CU-04 Registrar empresa cliente**
2.  **Facturación mensual automática (FR-27)**
3.  **CU-12 Crear factura adicional**
4.  **CU-20 Sincronizar pago y confirmar cobro**
5.  **CU-15 Pausar recordatorios**
6.  **CU-16 Condonar deuda**
7.  **CU-02 Gestionar permisos**
8.  **CU-22 Crear incidencia/comentario sobre una empresa**
9.  **CU-09 Validar cambio grande / aprobación**
10. **CU-17 Cerrar periodo económico**

Una vez estén esos, muchas de las demás secuencias serán variaciones o
flujos más pequeños.

------------------------------------------------------------------------

# 31. Regla práctica para convertir esta guía en un diagrama de secuencia

Para cada caso, intentad seguir siempre esta estructura:

``` text
1. ¿Quién inicia la acción?
        ↓
2. ¿Qué Manager recibe la operación?
        ↓
3. ¿Qué entidad consulta o modifica?
        ↓
4. ¿Se crea algún objeto nuevo?
        ↓
5. ¿Llama a otro Manager?
        ↓
6. ¿Hay un alt / loop?
        ↓
7. ¿Hay que registrar auditoría/notificar/enviar mensaje?
```

Ejemplo muy corto:

``` text
Responsable
    → FacturaManager.crearFacturaAdicional(...)
    → <<create>> Factura
    → FacturaManager.validarFactura(...)
    → MensajeManager.crearMensaje(...)
    → <<create>> Mensaje
    → Mensaje.enviarMensaje()
```

Si durante una secuencia os sale algo como:

``` text
XManager.metodoQueNoExiste()
```

no lo inventéis silenciosamente.

Apuntadlo y revisamos si:

-   realmente necesita añadirse al diagrama de clases;
-   ya existe otra operación que cubre esa responsabilidad;
-   o es un detalle demasiado bajo para aparecer en la secuencia.

------------------------------------------------------------------------

# 32. Partes del sistema que todavía conviene tratar con cuidado

Estas son las zonas donde el UML todavía no cierra todos los detalles:

-   **Recordatorios automáticos:** el flujo general existe, pero no está
    definido todo el calendario interno.
-   **Reanudar recordatorios:** falta decidir desde qué fecha vuelve a
    contar.
-   **Factura adicional programada:** hay que decidir definitivamente
    cómo se envía junto con la recurrente.
-   **Histórico exacto de plantillas:** la relación existe, pero hay que
    asegurar que editar una plantilla no destruya el histórico.
-   **Salud del sistema:** CU-27 no tiene todavía clases específicas.
-   **API:** CU-03 no tiene todavía componente/clase de entrada
    específico.
-   **Conversión de moneda:** informes/facturas tienen moneda, pero no
    hay todavía servicio de conversión.
-   **Documentos contables y retención:** aparece en requisitos, pero
    `ArchivoAdjunto` no cubre todo ese problema.
-   **Favoritos/recientes y algunas etiquetas de estado:** están en
    requisitos pero no son parte central del UML actual.
-   **Operaciones masivas:** están en requisitos pero no se han modelado
    como bloque propio.
-   **Idempotencia de facturas:** el requisito existe, pero se tratará
    como validación/lógica interna mientras no se modele otra cosa.

Para los diagramas de secuencia, estas partes se pueden dejar para más
adelante o representar únicamente al nivel que ya está decidido.

------------------------------------------------------------------------

# 33. Cambios de esta versión del diagrama que afectan a las secuencias

Respecto a la versión anterior de esta guía:

-   `AutenticacionManager` es ahora `<<Singleton>>`; se obtiene mediante
    `getInstancia()`.
-   `PagoManager.registrarPago(...)` recibe `importe`, `referencia` y
    `origen`, sin exigir una `Factura`.
-   `ObjetivoCobro` desaparece: el objetivo está en
    `PeriodoEconomico.objetivoRecaudacion`.
-   `PeriodoEconomico` incorpora `totalFacturado`, `totalRecaudado` y
    `totalPendiente`, además de `calcularTotales()` y
    `calcularProgresoObjetivo()`.
-   `PeriodoEconomicoManager` incorpora
    `generarInformePeriodoPersonalizado(fechaInicio, fechaFin)`.
-   Los informes ya no necesitan una entidad `InformeFinanciero`.

El resto de flujos descritos en esta guía se mantiene igual que en la
versión anterior.
