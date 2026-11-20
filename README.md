# EscanerECO

**Lector universal de gafetes** para el Ecosistema ECO de Embotelladora de Colima. Prototipo funcional pendiente de validación con gafetes reales en iPhone y Android.

## Funciones

- Abre la cámara del teléfono o computadora desde una página HTTPS.
- Lee códigos **Code 128, Code 39, Code 93, Codabar, ITF, EAN-13, EAN-8, UPC-A, UPC-E y QR**.
- Identifica el texto de la nómina; permite captura manual y copia.
- Puede devolver la nómina a la aplicación web que abrió el escáner, mediante `window.postMessage` y un canal por sesión.
- Funciona también en modo independiente (sin registrar asistencia ni acceder a ninguna base).
- Utiliza el logotipo oficial proporcionado para ECO, incluido en `index.html`.

## Publicar en GitHub Pages

En el repositorio: **Settings → Pages → Build and deployment → Deploy from a branch → `main` → `/(root)` → Save**.
La URL prevista para la página, una vez habilitada y publicada, es:

`https://arqlal-byte.github.io/EscanerECO/`

El archivo `index.html` solo no activa automáticamente GitHub Pages. **No se considera publicada hasta comprobar que la URL responde correctamente.**

## Integrar con Inducción ECO v0.1.4

1. Publica y abre primero el escáner con HTTPS y verifica el permiso de cámara.
2. En las propiedades del proyecto Apps Script de Inducción ECO establece `ECO_LECTOR_URL` con la URL HTTPS real de GitHub Pages.
3. La versión 0.1.4 de `Code.gs` e `Index.html` debe estar instalada y desplegada por el propietario del proyecto. **Este repositorio no despliega Apps Script ni modifica su configuración actual.**
4. En **Administración → Asistencia**, abre una sesión y pulsa **Abrir lector**. Se abre una pestaña vinculada que devuelve lecturas al panel.
5. La aplicación valida inscripción, sesión y duplicados, y escribe en su Sheet. El lector por sí solo no escribe registros.

## Contrato de integración para otras aplicaciones

La app origen abre la página añadiendo un fragmento URL:

`#channel=<IDENTIFICADOR_ALEATORIO>&returnOrigin=<ORIGEN_HTTPS_DE_LA_APP>`

Debe crear un canal aleatorio nuevo por apertura y conservar una referencia a la ventana abierta (`window.open`). La página lectora envía a `window.opener` este mensaje:

```json
{"type":"ECO_LECTURA_GAFETE","channel":"<IDENTIFICADOR_ALEATORIO>","nomina":"001234","source":"EscanerECO","version":"1.0.0","method":"CAMARA"}
```

La aplicación debe comprobar **`event.origin` (el origen exacto de GitHub Pages), `event.source` (ventana abierta) y `channel`**. No debe registrar automáticamente sin verificar permisos, sesión, inscripción y duplicados. Tras registrar o rechazar, puede responder por `postMessage`:

```json
{"type":"ECO_RESPUESTA_ASISTENCIA","channel":"<IDENTIFICADOR_ALEATORIO>","nomina":"001234","ok":true,"texto":"Registro confirmado"}
```

Por defecto, el lector admite como aplicación origen únicamente URLs HTTPS de `script.google.com` y `*.googleusercontent.com`. Otros orígenes deben autorizarse explícitamente en el arreglo `ADDITIONAL_APPROVED_ORIGINS` de `index.html`, tras evaluación de seguridad. El parámetro `returnOrigin` **no concede autorización de acceso** a ninguna base.

### Limitaciones

- La transmisión entre pestañas es una integración a verificar en iPhone y Android; si el navegador rompe `window.opener`, usar copia/captura manual.
- No envía imágenes ni datos a GitHub. La biblioteca `html5-qrcode` v2.3.8 se carga desde jsDelivr, así que el acceso a Internet es necesario.
- Un código de barras identifica la nómina, **no acredita por sí mismo la identidad física** de la persona.
- El sitio público no lleva contraseñas, credenciales, tokens ni IDs de bases de datos.
- La configuración visual (`doGet`, viewport, barra de Google) de las otras aplicaciones no cambia.

## Versiones y recuperación

- **1.0.0 (2026-11-19)**: primer lector compartido (separado del prototipo de asistencia de Inducción ECO 0.1.4).
- Para revertir, utilizar el historial de commits de GitHub. No se modifica ningún Apps Script operativo al actualizar este repositorio.

La validación final corresponde al usuario después de probar la cámara y el flujo de lectura con un gafete real.
