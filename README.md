# EscanerECO

**Escáner de códigos de barras y QR reutilizable** del Ecosistema ECO de Embotelladora de Colima.

**Versión:** 1.1.2 — 20 de noviembre de 2026. **Estado:** prototipo funcional, pendiente de validación con gafetes reales en celulares.

## Acceso

Una vez publicado GitHub Pages, el lector usa esta dirección:

https://arqlal-byte.github.io/EscanerECO/

La aplicación de origen (por ejemplo, Inducción ECO) presenta el botón **Abrir EscanerECO**. Cuando se proporciona una URL de retorno validada, el lector muestra **Regresar a Inducción ECO**, conservando la convocatoria.

## Funciones

- Escaneo continuo por cámara de códigos **Code 128, Code 39, Code 93, Codabar, ITF, EAN-13, EAN-8, UPC-A, UPC-E y QR**.
- Área de lectura rectangular y horizontal para gafetes con barras; zona adaptativa al ancho de la pantalla.
- Solicitud opcional de enfoque continuo y resolución mejorada cuando las capacidades del navegador lo permiten. Si el dispositivo rechaza estas capacidades, la lectura básica continúa.
- Control de zoom visible únicamente cuando la cámara ofrece zoom programable.
- Botón **Leer código desde una foto**, para elegir o tomar una imagen; la cámara activa se detiene antes de procesarla y se puede iniciar de nuevo después. La imagen no se envía a Apps Script ni a un servidor de GitHub.
- Captura manual de nómina, copia local y lista de lecturas sin confirmar.
- Comunicación entre ventanas mediante `window.postMessage` con canal y origen exacto, sin guardar códigos de nómina en GitHub.
- Preserva ceros iniciales. Un código leído no acredita por sí mismo la identidad de la persona.

## Integración con Inducción ECO

La integración actual está preparada en **Inducción ECO 0.1.6**. Este repositorio solo realiza la lectura, **no consulta RH ni escribe en Google Sheets**. Inducción ECO debe validar que la nómina esté inscrita en la convocatoria activa, impedir duplicados y confirmar el registro.

El flujo es **Administración → Asistencia → Seleccionar convocatoria → Abrir EscanerECO → Leer gafete → Regresar a Inducción ECO**. Una convocatoria equivale al curso completo y genera una sola asistencia por trabajador; no se crean sesiones adicionales.

La aplicación abre el lector con parámetros en el fragmento de URL (`#`): `channel`, `returnOrigin`, `returnUrl`, `app`.

El lector envía un mensaje:

```js
{ type: 'ECO_LECTURA_GAFETE', channel, nomina, source: 'EscanerECO', version: '1.1.2', method }
```

El método de lectura puede ser `CAMARA`, `MANUAL` o `FOTO`. **La aplicación que registra la asistencia define y valida su propio método de almacenamiento**. Inducción ECO 0.1.6 trata las lecturas recibidas del escáner como `CAMARA` y las valida en servidor. No se modificó Apps Script para esta versión.

La aplicación de origen confirma mediante:

```js
{ type: 'ECO_RESPUESTA_ASISTENCIA', channel, nomina, ok, texto }
```

Si se pierde la comunicación entre pestañas, el lector conserva **en pantalla** los números pendientes, para copiar y revisar en la aplicación. No significa que la asistencia esté guardada.

## Seguridad y privacidad

- No almacenar credenciales, tokens ni identificadores de bases de datos en este repositorio público.
- Respetar los orígenes autorizados; el enlace de regreso solo acepta despliegues `script.google.com` con estructura validada y convocatoria permitida.
- No guardar imágenes, videos ni nóminas en servidores de GitHub. La biblioteca `html5-qrcode` versión **2.3.8** se obtiene de jsDelivr.
- El uso de la cámara exige HTTPS y autorización del navegador.
- No mostrar al público datos personales adicionales a la nómina leída; cada aplicación debe limitar los permisos administrativos.

## Validaciones realizadas

- Sintaxis JavaScript mediante Node.js: correcta.
- Prueba local con biblioteca de cámara simulada: arranque, recuadro horizontal, control de zoom, carga de imagen y conservación de ceros iniciales.
- Comprobación de la interfaz en emulación de teléfono: sin desbordamiento horizontal ni errores JS.
- Se conservó el logotipo corporativo oficial de la versión anterior, sin alteraciones.
- **Pendiente:** lectura física de un gafete con Code 39/Code 128 en iPhone/Android y confirmación real de asistencia contra Apps Script.

Las pruebas simuladas no son prueba de compatibilidad de hardware ni de reconocimiento óptico real.

## Versiones y recuperación

- **1.0.0:** lector universal inicial.
- **1.1.0:** retorno y lecturas sin confirmar.
- **1.1.1:** regreso por convocatoria sin exigir sesión.
- **1.1.2:** encuadre horizontal, enfoque/zoom opcionales, diagnóstico de lectura y fotografía.

Para revertir, consultar historial de commits o la rama de respaldo de GitHub. **No se alteran Inducción ECO, su viewport ni los Sheets** por actualizar este lector.
