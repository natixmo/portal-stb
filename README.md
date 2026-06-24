# Portal STB - Guía del Archivo de Configuración (`config.json`)

Este repositorio aloja de forma remota el archivo `config.json` que controla dinámicamente el comportamiento, la carga de canales, la seguridad del reproductor y las herramientas de diagnóstico del portal HTML ejecutado localmente en decodificadores STB Hybroad A350 con procesador HiSilicon.

El uso de este archivo permite modificar el comportamiento del portal desde internet sin necesidad de editar ni flashear constantemente el archivo `portal.html` dentro de la memoria interna del dispositivo.

---

## 📄 Estructura Completa del JSON

```json
{
  "channels": {
    "url": "[http://iptv-org.github.io/iptv/index.m3u](http://iptv-org.github.io/iptv/index.m3u)",
    "max": 20
  },
  "epg": {
    "url": "[http://www.tdtchannels.com/epg/TV.json](http://www.tdtchannels.com/epg/TV.json)",
    "enabled": false
  },
  "player": {
    "method": "vod",
    "timeout_ms": 15000
  },
  "ui": {
    "logos": false,
    "weather": false
  },
  "system": {
    "force_http_all": true
  },
  "debug": {
    "show_overlay": true,
    "print_console": true,
    "log_keys": false,
    "log_memory_ms": 30000
  }
}
```

---

## 🛠️ Explicación Detallada de los Campos

### 📺 1. Bloque `channels` (Gestión de Canales)
Controla la procedencia y el volumen de la lista de reproducción de televisión.
* **`url`** *(String)*: Dirección URL donde se encuentra el listado de canales. Puede apuntar a un archivo estructurado en formato `.json` o a una lista de reproducción estándar en formato `.m3u`.
* **`max`** *(Number)*: Límite máximo de canales que el portal procesará y pintará en la pantalla. Limitar este número es crítico en dispositivos STB antiguos con recursos limitados para evitar que el navegador del sistema colapse.

### 📅 2. Bloque `epg` (Guía Electrónica de Programación)
Administra los datos de la programación en directo (qué están emitiendo ahora y qué viene después).
* **`url`** *(String)*: Dirección URL del archivo JSON que contiene los eventos de programación de los canales.
* **`enabled`** *(Boolean)*: `true` activa la descarga y procesamiento de la guía; `false` la desactiva por completo para ahorrar ancho de banda y ciclos de CPU en decodificadores lentos.

### 🎬 3. Bloque `player` (Parámetros del Reproductor Nativo)
Ajusta el comportamiento del núcleo de reproducción de video (`AVPlayer` / `HiPlayer`).
* **`method`** *(String)*: Especifica el método de llamada interna para el streaming nativo (por defecto `vod`, el cual fuerza el modo Video On Demand, siendo el protocolo más estable descubierto para el SDK de HiSilicon).
* **`timeout_ms`** *(Number)*: Tiempo de espera máximo en milisegundos (ej. `15000` = 15 segundos) antes de que el reproductor considere que un enlace ha fallado y salte automáticamente a la siguiente URL alternativa del canal.

### 🎨 4. Bloque `ui` (Interfaz de Usuario)
Modifica elementos visuales y componentes estéticos de la pantalla principal.
* **`logos`** *(Boolean)*: `true` habilita la carga de imágenes/iconos externos para cada canal; `false` oculta los logos y dibuja un marcador de posición plano para acelerar el renderizado de la guía.
* **`weather`** *(Boolean)*: `true` activa el widget climatológico en la barra superior; `false` elimina el widget de la interfaz para reducir las consultas de red externas.

### 🔒 5. Bloque `system` (Reglas del Sistema de Red)
Controla las políticas de bajo nivel de las peticiones de red y compatibilidad de protocolos.
* **`force_http_all`** *(Boolean)*: **Campo Crítico de Compatibilidad.** Cuando está en `true`, activa el motor de reescritura dinámica del portal, de modo que cualquier URL de listados, EPG, Clima o flujos multimedia que comience con `https://` será interceptada y modificada a `http://` plano. Esto evita que las librerías criptográficas obsoletas del firmware del STB provoquen un fallo de segmentación (*Segmentation Fault*) e invaliden el descifrado SSL/TLS de servidores modernos.

### 🐛 6. Bloque `debug` (Herramientas de Diagnóstico y Logs)
Permite realizar tareas de mantenimiento, mapeo y telemetría en tiempo real sobre el televisor.
* **`show_overlay`** *(Boolean)*: `true` despliega un recuadro de texto translúcido verde en la parte superior de la pantalla que muestra las últimas acciones del portal (Logs de inicialización, errores de carga, buffering del reproductor, etc.). `false` lo oculta para una visualización limpia.
* **`print_console`** *(Boolean)*: `true` habilita los comandos `console.log()` internos del código hacia la consola nativa del sistema operativo. Cambiarlo a `false` reduce la carga de procesamiento del navegador del STB.
* **`log_keys`** *(Boolean)*: `true` intercepta las pulsaciones del mando a distancia e imprime el código numérico de la tecla (`keyCode`) directamente en el recuadro verde de la pantalla. Es la herramienta ideal para mapear nuevos mandos o solucionar problemas con botones desconfigurados.
* **`log_memory_ms`** *(Number)*: Intervalo de tiempo en milisegundos (ej. `30000` = 30 segundos) en el cual el portal llamará al método nativo `Ag_system_getMemoryInfo()` e imprimirá el estado actual de consumo de memoria RAM en el registro de depuración para controlar fugas de memoria.

---

## 🔄 Notas sobre el despliegue

1. Las actualizaciones realizadas en este archivo `config.json` se verán reflejadas de manera automática la próxima vez que el decodificador STB sea encendido o cuando el usuario pulse el botón **Verde (Recargar)** en su mando a distancia.
2. Si el servidor donde se aloja este archivo fuerza de manera estricta el uso de conexiones cifradas `https` (como ocurre con el CDN de `jsdelivr`), asegúrate de que el portal local tenga la instrucción adecuada en el arranque o aloja una copia de este archivo en un servidor de almacenamiento local bajo `http` plano si el dispositivo no consigue leer la configuración de internet.
