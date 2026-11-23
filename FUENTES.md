# Fuentes, librerías y recursos de terceros

Este archivo lista todo lo que el proyecto toma de otras personas o servicios:
código base, librerías, servicios en la nube, documentación técnica y herramientas
de IA. Si algo no aparece aquí, es trabajo propio del equipo.

---

## 1. Código y proyecto de origen

| Recurso | Autor | Qué se tomó | Enlace |
|---|---|---|---|
| Guía *"ESP32 conectado a una base de datos en la nube — Aiven (PostgreSQL) + Railway + GitHub"* (curso TEI201) | Fernando Méndez | Arquitectura ESP32 → Railway → Aiven, estructura del servidor Express (`server.js`), tabla `lecturas`, uso de `x-api-key`, página con Chart.js y código base del envío HTTPS desde el ESP32 | PDF entregado en el curso |
| Repositorio de ejemplo de la guía | Fernando Méndez | Estructura de archivos (`server.js`, `package.json`, `.env.example`, `.gitignore`, `public/`, `esp32/`) | https://github.com/ferjomendez/esp32-railway-demo |
| Repositorio de ejemplo TEI201 (proyecto RockU) | Fernando Méndez | Estructura de carpetas del repositorio y tabla de rúbrica | https://github.com/ferjomendez/Repo-Ejemplo-TEI201 |

**Qué se cambió respecto de la guía** (detalle en [`software/README.md`](software/README.md)):

- La lógica de control de la bomba (máquina de estados con retardos y descanso del motor) es propia del equipo.
- El envío a internet corre en una **tarea aparte del ESP32 (núcleo 0)** con cola y reintentos, para no frenar el control de la bomba.
- El ESP32 guarda **varias redes WiFi** (WiFiMulti) y tiene además una **página web local** propia.
- El servidor valida valores no finitos y permite filtrar por sensor (`?sensor=nivel`).
- La página web del servidor se rediseñó para mostrar el estado del estanque, la bomba y si el ESP32 está sin señal.

---

## 2. Librerías de software

### Servidor (Node.js, versiones de `package.json`)

| Librería | Versión | Dónde se usa | Licencia | Sitio |
|---|---|---|---|---|
| express | ^4.21.0 | `server.js`: API y entrega de la página | MIT | https://expressjs.com |
| pg (node-postgres) | ^8.13.0 | `server.js`: conexión a PostgreSQL con SSL | MIT | https://node-postgres.com |
| dotenv | ^16.4.5 | `server.js`: leer `.env` al probar en el computador | BSD-2-Clause | https://github.com/motdotla/dotenv |
| Node.js | 18.20.8 (en Railway) | Ejecuta el servidor | MIT | https://nodejs.org |

### Página web

| Librería | Versión | Dónde se usa | Licencia | Sitio |
|---|---|---|---|---|
| Chart.js | 4.4.1 (desde jsDelivr) | `public/index.html`: gráfico del historial | MIT | https://www.chartjs.org |

### Firmware del ESP32 (Arduino)

| Librería | Versión | Dónde se usa | Licencia | Sitio |
|---|---|---|---|---|
| Arduino core para ESP32 (Espressif) | 3.3.12 | Base del firmware. Incluye `WiFi`, `WiFiMulti`, `WiFiClientSecure`, `HTTPClient`, `WebServer`, `ESPmDNS` | LGPL-2.1 | https://github.com/espressif/arduino-esp32 |
| ArduinoJson (Benoit Blanchon) | 7.4.3 | Armar el JSON que se envía a Railway | MIT | https://arduinojson.org |
| FreeRTOS (incluido en el core) | — | Tarea de envío en el núcleo 0 y cola de lecturas | MIT | https://www.freertos.org |

Herramientas: Arduino IDE 2.3.10, Autodesk Fusion 360 (diseño 3D y PCB), Git y GitHub.

---

## 3. Servicios en la nube

| Servicio | Uso | Plan | Enlace |
|---|---|---|---|
| Railway | Ejecuta el servidor Express y publica la página | Trial (30 días o US$5) | https://railway.com |
| Aiven | Base de datos PostgreSQL 18 | Free (Free-1-1gb) | https://aiven.io |
| GitHub | Código fuente y deploy automático hacia Railway | Gratuito | https://github.com |
| jsDelivr | CDN desde donde se carga Chart.js | Gratuito | https://www.jsdelivr.com |

---

## 4. Datasheets y documentación técnica

| Componente | Documento | Enlace |
|---|---|---|
| ESP32 (Espressif) | ESP32 Series Datasheet | https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf |
| ESP32 (Espressif) | Documentación del core Arduino-ESP32 | https://docs.espressif.com/projects/arduino-esp32/ |
| Relé SRA-05VDC-CL (Ningbo Songle), usado en la PCB | Datasheet del relé (LCSC C99666). SPDT, bobina de 5 V y 42 Ω | https://datasheet.lcsc.com/szlcsc/Ningbo-Songle-Relay-SRA-05VDC-CL_C99666.pdf · ficha: https://www.lcsc.com/product-detail/C99666.html |
| Flotador P35 | Ficha técnica del flotador | **POR COMPLETAR** (enlace del vendedor o ficha) |
| Bomba de agua 220 V ($6.000 CLP) | Ficha técnica (potencia, caudal) | **POR COMPLETAR** (foto de la etiqueta o enlace de la tienda) |
| Sensor de luz TEMT6000 (en la PCB) | Datasheet Vishay TEMT6000 | **POR COMPLETAR** (si se usa) |

---

## 5. Modelos 3D de terceros

| Modelo | Autor | Uso | Enlace |
|---|---|---|---|
| Modelo 3D de la PCB y sus componentes (diseño `ESP32_PCB`, cabeceras de pines exportadas por *Open CASCADE STEP translator*) | **POR COMPLETAR**: indicar si la PCB la diseñó el equipo o se importó un STEP de terceros, y de dónde | Referencia de volúmenes reales dentro del encapsulado | — |

El encapsulado (caja, techo y tubos) fue diseñado por el equipo (Diego Poupin) en Fusion 360
con ayuda de IA (ver sección 6), y lo imprimió la universidad.

---

## 6. Uso de inteligencia artificial

Se usó **Claude (Anthropic), mediante Claude Code**, como asistente durante el proyecto.

| Parte del proyecto | Para qué se usó la IA | Qué hizo el equipo |
|---|---|---|
| Diseño 3D | Modelar en Fusion 360 el encapsulado (caja, techo con riel, tubo telescópico de 3 tramos, roscas) y dividirlo en piezas de 10 cm o menos para la impresora | Definió los requisitos (estanque de 2000 L, tubo de 3 tramos de 10 cm, techo para lluvia, entradas por relé, rosca del sensor), revisó y pidió cada cambio |
| Firmware ESP32 | Escribir y mejorar el código: página local, envío a Railway en una tarea aparte, varias redes WiFi | Escribió la primera versión del control con flotador y relé, probó en la placa real, ajustó WiFi y conexiones |
| Servidor y página | Adaptar el código de la guía al proyecto (sensores, validaciones, página del estanque) | Creó las cuentas de GitHub, Aiven y Railway, configuró las variables y verificó la página publicada |
| Documentación | Redactar borradores de README, FUENTES, protocolo de pruebas e iteraciones a partir de los datos reales del proyecto | Completa los datos que faltan (equipo, ODS, precios, fotos, pruebas) y revisa los textos |

Todo el código generado con IA se probó en el hardware real y en la página publicada.
Las cifras de este repositorio salen de mediciones o datos reales.
