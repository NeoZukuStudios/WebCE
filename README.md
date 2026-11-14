<div align="center">
  <img src="logo.png" alt="WebCE Engine Logo" width="220" />
  <h1>WebCE Engine</h1>
  <p><strong>Motor nativo de ciencia ficción en primera persona compilado para navegadores web mediante WebAssembly.</strong></p>

  <p>
    <img src="https://shields.io" alt="Plataforma Web" />
    <img src="https://shields.io" alt="Tecnología" />
    <img src="https://shields.io" alt="Licencia AGPL-3.0" />
  </p>
</div>

---

## 📖 Descripción General

**WebCE Engine** es una implementación web de código abierto basada en la descompilación comunitaria del motor clásico de consola (build 2342) y en el proyecto [OpenCommunityEdition/OpenCE](https://github.com). 

A diferencia de un emulador tradicional, **WebCE Engine** compila el código fuente nativo del motor directamente a **WebAssembly (Wasm)** utilizando **Emscripten** y **SDL3**, permitiendo ejecutar el juego de manera fluida en cualquier navegador web moderno sin necesidad de instalar programas externos ni complementos.

### ✨ Características Principales

* **Ejecución 100% en el Navegador:** Renderizado mediante **WebGL 2** y audio/controles gestionados a través de **SDL3**.
* **Privacidad y Almacenamiento Local:** Todo el procesamiento ocurre en el dispositivo del usuario. Los archivos de datos proporcionados por el jugador se almacenan localmente en el navegador (*IndexedDB / Origin Private File System*) y **nunca se suben a ningún servidor**.
* **Multijugador P2P en Tiempo Real:** Soporte para salas multijugador a través de **WebRTC** con código de red moderno y predicción de movimiento del lado del cliente.
* **Soporte de Mapas Personalizados:** Capacidad para cargar escenarios creados por la comunidad y compartir mapas automáticamente entre jugadores dentro de una misma sala.
* **Mejoras Visuales y de Rendimiento:**
  * Interpolación de fotogramas (*Smooth Motion*) para pantallas de 60Hz, 120Hz, 144Hz o superiores manteniendo la simulación física original a 30 ticks.
  * Apuntado instantáneo (*Instant Aim*) sin retraso de entrada del ratón.
  * Ajuste dinámico de campo de visión (*FOV* y *Viewmodel FOV* desde 80° hasta 150°).
  * Escalado de resolución nativo, filtrado anisotrópico, sombras de alta resolución y HUD reescalado en alta definición.
* **Modos de Juego Extendidos:** Incluye variantes clásicas y modos adicionales como *SWAT*, *Infection*, *Gun Game* y reglas personalizables.

---

## 🎮 Requisitos para Jugar

**WebCE Engine es únicamente el motor de ejecución y no incluye ningún archivo de juego, mapa, textura ni sonido protegido por derechos de autor.**

Para utilizar el motor necesitas:
1. Un navegador web moderno compatible con WebAssembly y WebGL 2 (Chrome, Edge, Firefox, Brave, Opera o Safari).
2. Una imagen de disco legal en formato **`.iso`** o **`.xiso`** de tu propia copia original de consola.

### ¿Cómo cargar tus datos?
1. Abre la aplicación web en tu navegador.
2. En la pantalla inicial, selecciona tu archivo `.iso` o `.xiso` mediante el selector de archivos.
3. El motor extraerá automáticamente la carpeta `maps/` necesaria dentro de la memoria local de tu navegador.
4. Una vez completada la extracción, el motor iniciará de inmediato. En futuras visitas no necesitarás volver a seleccionar el archivo.

---

## 🛠️ Guía de Compilación (Desarrollo Local)

Si deseas compilar **WebCE Engine** por tu cuenta o realizar modificaciones en el código fuente, sigue estos pasos en Windows, Linux o macOS.

### 1. Prerrequisitos
Asegúrate de tener instalados en tu sistema:
* **Python 3.10+**
* **Ninja Build** (`ninja`)
* **Emscripten SDK (`emsdk`)** configurado y actualizado.
* **Git**

### 2. Clonar el Repositorio
```bash
git clone https://github.com
cd WebCE-Engine
```

### 3. Activar el Entorno de Emscripten
* **En Windows (PowerShell):**
  ```powershell
  C:\emsdk\emsdk_env.ps1
  ```
* **En Linux / macOS:**
  ```bash
  source ~/emsdk/emsdk_env.sh
  ```

### 4. Configurar y Compilar para Web
Genera los archivos de construcción en modo optimizado (`--release`) y compila el objetivo web:

```bash
python configure.py --release
ninja web
```

Una vez finalizado el proceso, todos los archivos estáticos listos para producción (`.html`, `.js`, `.wasm`, `.json`) se generarán dentro de la carpeta:
```text
build/web/site/
```

### 5. Ejecutar Servidor Local de Pruebas
Los navegadores modernos bloquean la ejecución directa de WebAssembly por seguridad si abres el archivo HTML mediante `file://`. Inicia el servidor local incluido en el proyecto:

```bash
python tools/web_serve.py
```
Luego abre tu navegador y visita la dirección local indicada en la consola (generalmente `http://localhost:8000`).

---

## 📂 Estructura del Proyecto

```text
WebCE-Engine/
├── .github/workflows/   # Flujos de integración continua y compilación automática
├── deploy/              # Claves públicas y scripts de despliegue
├── docs/                # Documentación técnica sobre código de red, mods y cachés
├── port/                # Capa de abstracción de plataforma (Web, SDL3, OpenGL/WebGL)
├── source/              # Código fuente principal del motor del juego en C
├── tools/               # Scripts auxiliares (servidor web local, pruebas de red, firmas)
├── configure.py         # Script de configuración de compilación y detección de herramientas
├── logo.png             # Identidad visual principal del proyecto
├── LICENSE              # Texto completo de la licencia AGPL-3.0
└── README.md            # Documentación principal del repositorio
```

---

## 🌐 Despliegue en Producción

El contenido generado en `build/web/site/` es completamente estático y puede alojarse en cualquier servicio de hosting web (GitHub Pages, Cloudflare Pages, Vercel, Netlify o un servidor Nginx/Apache propio).

> **Nota Importante sobre Cabeceras HTTP:** Para que el navegador permita el uso de hilos múltiples (*SharedArrayBuffer* / *WebAssembly Threads*), tu servidor web debe enviar las siguientes cabeceras de aislamiento de origen cruzado:
> ```http
> Cross-Origin-Opener-Policy: same-origin
> Cross-Origin-Embedder-Policy: require-corp
> ```

---

## 📜 Licencia y Código Abierto

Este proyecto se distribuye bajo los términos de la **GNU Affero General Public License v3.0 (AGPL-3.0-or-later)**.

* **Libertad de uso y modificación:** Puedes usar, estudiar, modificar y distribuir este código fuente libremente.
* **Obligación de compartir el código en red:** De acuerdo con la **Sección 13 de la licencia AGPL-3.0**, si modificas este motor y lo alojas en un sitio web o servidor para que otras personas jueguen a través de la red, **debes ofrecer a los usuarios acceso al código fuente completo de tu versión modificada**.
* Consulta el archivo `LICENSE` en este repositorio para leer los términos legales completos.

---

## ⚖️ Aviso Legal (Disclaimer)

**WebCE Engine** es un proyecto independiente, sin fines de lucro y de código abierto creado con fines educativos y de preservación tecnológica.

* Este proyecto **no está afiliado, patrocinado, respaldado ni asociado** de ninguna manera con Microsoft Corporation, Xbox Game Studios, Halo Studios ni Bungie.
* Este repositorio **no contiene, distribuye ni enlaza** a archivos binarios comerciales, imágenes de disco, mapas originales ni activos protegidos por derechos de autor.
* Todos los derechos sobre los archivos de datos originales pertenecen a sus respectivos propietarios. El usuario es el único responsable de utilizar una copia legal de su propiedad.
