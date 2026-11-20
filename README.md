# EscanerECO

**Escáner de gafetes y códigos de barras reutilizable** para las aplicaciones del Ecosistema ECO de Embotelladora de Colima.

**Versión:** 1.1.0 · **Fecha:** 20 de noviembre de 2026 · **Etapa:** prototipo funcional pendiente de validación operativa.

## Acceso

La dirección de GitHub Pages para este repositorio, cuando esté publicada y disponible, es:

`https://arqlal-byte.github.io/EscanerECO/`

Desde una aplicación integrada, el personal entra con el botón **Abrir EscanerECO**, no mediante un enlace copiado manualmente. El lector muestra un botón **Regresar a la aplicación**.

## Capacidades

- Escanea códigos **Code 128, Code 39, Code 93, Codabar, ITF, EAN, UPC y QR** con la cámara del dispositivo.
- Captura o copia manualmente una nómina. Conserva ceros iniciales.
- Comparte **solo el contenido del código leído** con la ventana que abrió el lector, por `window.postMessage` y un canal aleatorio por apertura.
- Recibe del sistema destino la **confirmación de registro**, incluyendo los duplicados.
- Muestra un botón para regresar a la aplicación de origen. Para **Inducción ECO**, el enlace de recuperación incluye solo los identificadores de convocatoria y sesión, nunca números de nómina.
- En iPhone o Android, si el navegador impide la comunicación entre pestañas, conserva temporalmente en pantalla las **lecturas sin confirmar**, permite copiarlas y regresar a la sesión. La aplicación destino decide si las registra.
- En modo independiente, permite leer y copiar, sin conectarse a RH o a Google Sheets.
- Utiliza el logotipo oficial proporcionado; no lo genera ni lo sustituye.

## Integración con Inducción ECO

Preparada en **Inducción ECO 0.1.5** (Code.gs e Index.html completos disponibles en el expediente de la aplicación).

1. Instalar **ambos archivos 0.1.5** en el proyecto Apps Script de Inducción ECO; no basta con actualizar GitHub. Requiere guardar y publicar una nueva versión del despliegue web.
2. No se necesita una nueva propiedad: si `ECO_LECTOR_URL` no está definida, la versión 0.1.5 utiliza automáticamente la URL anterior. Si existe, solo se admite la ruta oficial del repositorio EscanerECO.
3. En **Administración → Asistencia**, seleccionar convocatoria, crear y **abrir la sesión**, luego elegir **Ver asistencia → Abrir EscanerECO**.
4. Escanear. La lectura solo se marca como registrada cuando la aplicación confirma que Google Apps Script la guardó en la pestaña `Asistencias`. La misma nómina no se duplica dentro de una sesión.
5. Utilizar **Regresar a Inducción ECO** para volver al mismo control. Si el navegador no retuvo la ventana, el lector abre un enlace de regreso con la convocatoria y sesión identificadas.
6. Si aparecen lecturas sin confirmar, presionar **Copiar nóminas pendientes** y, al regresar, elegir **Pegar lecturas pendientes**. Esas recuperaciones se registran como `MANUAL` y vuelven a validarse contra los inscritos en la sesión.

### Contrato genérico del lector

El sistema de origen abre la URL del escáner con parámetros en el *fragmento* `#`:

- `channel`: identificador aleatorio por apertura, no una contraseña.
- `returnOrigin`: origen HTTPS exacto desde el que la aplicación recibirá `postMessage`.
- `returnUrl`: enlace HTTPS opcional para regresar a un **despliegue Apps Script**; está validado con dominio y ruta permitidos.
- `app`: nombre corto que se muestra en el botón de regreso.

El lector transmite `{ type: 'ECO_LECTURA_GAFETE', channel, nomina, source: 'EscanerECO', method }`. La aplicación debe confirmar mediante `{ type: 'ECO_RESPUESTA_ASISTENCIA', channel, nomina, ok, texto }` y verificar el origen de los mensajes.

**Cada aplicación es responsable** de autenticar al operador, validar permisos y contexto, verificar la nómina y registrar la operación. Este repositorio **no tiene acceso a Apps Script, ni a la base de trabajadores, ni a hojas de cálculo**.

## Seguridad y privacidad

- Sin API keys, contraseñas, tokens de servicio ni identificadores internos de bases de datos en GitHub.
- La imagen de cámara se procesa localmente en el navegador; el repositorio no recibe fotografías ni almacena videos o nóminas en un servidor.
- El intercambio entre ventanas usa un canal por apertura y origen explícito. No aceptar mensajes de orígenes no verificados.
- El retorno a la aplicación solo acepta enlaces `https://script.google.com/.../exec` con sesión y convocatoria de formato válido. No hay redirección a otros dominios.
- El número de nómina **no equivale a autenticación de identidad física**; el encargado debe verificar el gafete y controlar el acceso administrativo.
- Si la app pierde contacto, **una lectura no confirmada no cuenta como asistencia**. Debe revisarse la lista antes de regresar.
- La biblioteca `html5-qrcode` versión 2.3.8 se importa desde jsDelivr: se requiere Internet; la compatibilidad de cámara y permisos depende del navegador.

## Alcance de verificación

- **Comprobado:** sintaxis del JavaScript; comportamiento simulado de mensaje y confirmación; restricción de enlaces de regreso; captura de pendientes; aspecto de la interfaz en emulación de teléfono; lógica local de registro en lote y recuperación de sesión.
- **No comprobado aún:** lectura física de gafete, comunicación de ventanas reales en iPhone/Android, implementación operativa de Apps Script ni escritura real desde el despliegue.

## Versiones y recuperación

- **v1.0.0:** escáner universal inicial. Recuperable en el historial de commits de GitHub y el respaldo documental en Drive.
- **v1.1.0:** regreso a aplicación de origen, contexto de sesión, lecturas pendientes, integración de Inducción ECO 0.1.5.

No cambiar el código de otras aplicaciones por actualizar el escáner. Integrarlas por separado y con pruebas de compatibilidad.


## Actualización 1.1.1 (2026-11-20)

- Para Inducción ECO 0.1.6 la dirección de regreso conserva únicamente `eco_conv`.
- `eco_sesion` sigue siendo aceptado opcionalmente para integraciones anteriores si tiene identificador válido.
- Sin cambios en cámara, formatos de lectura, privacidad o comprobación de origen.
