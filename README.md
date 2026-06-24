# Portal STB - Guía del Archivo de Configuración (`config.json`)

Este repositorio aloja de forma remota el archivo `config.json` que controla dinámicamente el comportamiento, la carga de canales, la seguridad del reproductor y las herramientas de diagnóstico del portal HTML ejecutado localmente en decodificadores STB (como el Hybroad A350 con procesador HiSilicon).

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
