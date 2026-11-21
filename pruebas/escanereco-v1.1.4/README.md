# EscanerECO v1.1.4 — prueba aislada
Fecha: 20 de noviembre de 2026. Estado: prototipo experimental; pendiente de prueba con iPhone y gafetes reales.

## Alcance
- Prueba independiente del lector de credenciales Coca-Cola/ECO existente, sin modificar, etiquetar ni sustituir gafetes.
- Se preserva sin cambios la versión operativa v1.1.3 en la raíz `/index.html`.
- **Esta ruta de prueba NO registra asistencias en Google Apps Script ni en Google Sheets.**
- Se conserva la identidad gráfica oficial de la versión de origen.

## URL de prueba
https://arqlal-byte.github.io/EscanerECO/pruebas/escanereco-v1.1.4/

## Cambios técnicos
1. Modo predeterminado Code 39 para gafetes ECO (modo universal optativo).
2. ZXing-C++ WebAssembly mediante `zxing-wasm@3.1.6` para leer Code39 con `tryHarder` y `tryRotate`.
3. Preprocesamiento local de la imagen utilizando el canal **rojo** para mejorar el contraste de barras negras sobre fondo rojo.
4. Lectura del fotograma completo (sin la franja `qrbox`).
5. Sin zoom automático: se mantiene control manual cuando la cámara lo soporta.
6. Se conserva `html5-qrcode@2.3.8` como respaldo y la opción de fotografía.
7. Se mantiene la lógica de seguridad y confirmación del código original, pero el modo de prueba bloquea el vínculo operativo.

## Pruebas
- Verificado mediante lectura del código en GitHub: la raíz `index.html` conserva el SHA anterior `fd4125b84d10c5dc20bec4c1094be6800ef553c8`.
- Comprobación estática de los cambios y de la presencia del logotipo oficial, el motor anterior y las restricciones de prueba.
- Imagen de referencia del gafete proporcionada por el usuario: una prueba local con decodificador de barras leyó Code39 «5029» al utilizar el canal rojo, pero no con la foto original sin procesar. Esta prueba de imagen **no valida el JavaScript de esta nueva versión**, ni constituye una prueba física con cámara.
- **Pendiente:** comprobar la carga real de los scripts/CDN y WASM en Safari iPhone, permiso de cámara, lectura en tiempo real, velocidad, valores consecutivos, orientación, iluminación, reflejos y memoria.
- **Pendiente:** confirmar el despliegue GitHub Pages de esta ruta; la creación de archivo por API no es confirmación de publicación web.

## Protocolo de prueba
1. Abrir el enlace experimental desde Safari en iPhone.
2. Elegir «Gafete ECO (Code 39 reforzado)» e iniciar cámara.
3. Presentar un gafete real de frente, dejando todo el código dentro de la imagen.
4. Comparar la nómina leída en pantalla con la impresa; verificar lecturas consecutivas.
5. Si no reconoce, utilizar «Leer código desde una foto» y comparar.
6. Para regresión, elegir modo universal, detener y reiniciar cámara.
7. No registrar asistencia: el mensaje debe indicar «Modo independiente» / «Sin confirmar».

## Consideraciones
- Dependencia de dos bibliotecas externas CDN para la carga y de un archivo `.wasm`.
- En celulares modestos, el procesamiento adicional podría aumentar consumo de batería y temperatura: comprobar en prueba real.
- El código de barras no autentica por sí solo la identidad de una persona.
- La variante experimental no debe sustituir a la operativa sin consentimiento del usuario y validación.

## Recuperación
No se alteró la raíz de GitHub ni Apps Script, por lo que la recuperación consiste en dejar de usar esta carpeta de pruebas. No borrar respaldos sin autorización.
