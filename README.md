# Drex Auth

Autenticador de dos pasos de Drex, en la línea de Google Authenticator:
tarjetas de cristal estilo Códigos de iOS sobre fondo de color, con el
código grande y un anillo que se vacía en tiempo real.

**En vivo:** https://floreseternasoporte-creator.github.io/Drex-Auth/

## Qué hace
- **Códigos TOTP/HOTP reales** — RFC 4226 / RFC 6238 (SHA-1/256/512,
  6/7/8 dígitos, periodo 30/60 s), con SHA-1 propio de respaldo si el
  navegador no ofrece Web Crypto. Verificado contra los vectores
  oficiales de ambos RFC.
- **Añadir cuentas**: escaneo QR con la cámara (jsQR), elegir / arrastrar
  / pegar una imagen del QR, clave Base32 manual, o enlace `otpauth://`
  (uno o varios por línea, con duplicados detectados).
- **Migración desde Google Authenticator**: entiende los enlaces
  `otpauth-migration://` de exportación y trae todas las cuentas de una vez.
- Tocar una tarjeta copia el código (con confirmación visual en la
  tarjeta); pulsación larga o clic derecho edita. Botón de ojo para
  ocultar los códigos, buscador a partir de 4 cuentas, modo Editar.
- **Respaldo** en JSON desde el menú «···» (exportar / importar).
- **Sin servidores**: las claves viven solo en el navegador del
  dispositivo (`localStorage`). Exporta un respaldo antes de borrar los
  datos del sitio.
- Claro/oscuro según el sistema, diseño responsive (rejilla en pantallas
  anchas) y `prefers-reduced-motion` respetado.

## Estructura
Un solo `index.html`, sin build, más `manifest.webmanifest` e `icon.svg`
para instalarla como app. `push_drexauth.py` publica en este repo vía la
API de GitHub (Git Data API). La versión anterior del agente quedó
guardada en `_v1-boone/` (no se publica).

## Arreglos 2026-10-23 (segunda publicación)
- El escaneo QR ya no depende del CDN de jsQR: usa primero el
  `BarcodeDetector` nativo del navegador (cámara e imagen) y jsQR queda
  de respaldo. Antes, si el CDN fallaba, la cámara ni siquiera abría.
- Al editar una cuenta importada con un periodo distinto de 30/60 s, el
  periodo se reseteaba a 30 y los códigos salían mal. Ahora el editor
  conserva el periodo real (opciones 15/30/60/90 + valor importado).
- La importación JSON normaliza algoritmo, dígitos, periodo y contador
  (antes un `algo` tipo «SHA-256» o dígitos fuera de rango entraban tal
  cual y rompían la generación).
- Instalable: `theme-color` claro/oscuro, manifiesto e icono enlazados.
