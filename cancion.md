# 🎵 Documentación de Interfaz: Reproductor Web de Spotify

Este documento detalla la estructura y los componentes de la interfaz de usuario (UI) para la reproducción de una canción en **Spotify Web**. La interfaz se divide principalmente en tres áreas: la Barra de Reproducción (Footer), el Área Principal de Contenido y la Barra Lateral de Navegación.

---

## 1. Barra de Reproducción Inferior (The Player)
Esta es la sección más crítica, persistente en la parte inferior de la pantalla. Contiene los controles activos de la canción.

### A. Sección Izquierda (Información de la Pista)
Elementos visuales que identifican lo que se está escuchando actualmente.

* **Carátula del Álbum (Thumbnail):**
    * Imagen pequeña cuadrada a la izquierda.
    * *Interacción:* Al hacer clic, expande la vista de la carátula o navega al álbum.
    * *Hover:* Muestra un icono de flecha hacia arriba para expandir.
* **Información de Texto:**
    * **Título de la Canción:** Texto en blanco (o color primario). Enlace clicable al álbum/single.
    * **Nombre del Artista:** Texto más pequeño en gris. Enlace clicable al perfil del artista.
* **Botón "Añadir a la Biblioteca" (+ / ❤):**
    * Icono circular.
    * *Estado inactivo:* Círculo con un más (+) o corazón vacío.
    * *Estado activo:* Check verde o corazón lleno (indica "Guardado").

### B. Sección Central (Controles de Reproducción)
El núcleo funcional de la interfaz.

1.  **Botones de Control (Fila Superior):**
    * **Aleatorio (Shuffle):** Icono de flechas cruzadas. Verde si está activo.
    * **Anterior (Previous):** Icono de salto atrás `|<`.
    * **Reproducir/Pausa (Play/Pause):** Botón circular central, más grande que el resto. Alterna entre un triángulo (play) y dos barras (pause).
    * **Siguiente (Next):** Icono de salto adelante `>|`.
    * **Repetir (Loop):** Icono de flechas en ciclo. Estados: *Desactivado (Gris)* -> *Repetir Todo (Verde)* -> *Repetir Una (Verde con un '1')*.

2.  **Barra de Progreso (Fila Inferior):**
    * **Tiempo transcurrido:** (Ej. `1:20`) a la izquierda.
    * **Slider (Scrubber):** Línea horizontal. La parte reproducida es blanca/verde al hacer hover; la restante es gris oscuro.
    * **Duración total:** (Ej. `3:45`) a la derecha.
    * *UX:* Al arrastrar el punto del slider, se cambia la posición de la canción.

### C. Sección Derecha (Herramientas Adicionales)
Funcionalidades secundarias y gestión de salida de audio.

* **Vista de "Ahora Suena" (Now Playing View):** Abre un panel lateral derecho con detalles.
* **Letras (Lyrics):** Icono de micrófono. Despliega las letras sincronizadas en la pantalla principal.
* **Cola (Queue):** Icono de tres líneas horizontales. Muestra la lista de reproducción siguiente.
* **Dispositivos (Connect):** Icono de monitor/altavoz. Permite cambiar la salida de audio (Cast, AirPlay, etc.).
* **Volumen:** Icono de altavoz + Barra deslizante horizontal.
* **Pantalla Completa:** Icono de flecha diagonal.

---

## 2. Área Principal (Main View)
Cuando una canción se reproduce desde una Playlist o Álbum, esta es la vista central.

### Cabecera (Hero Section)

* **Imagen Grande:** Carátula en alta resolución con sombra dinámica (basada en el color dominante).
* **Metadatos:**
    * Tipo (Ej. "CANCIÓN", "ÁLBUM").
    * Título (Tipografía grande y negrita, `font-weight: 700+`).
    * Detalles: Foto del artista (circular) • Nombre Artista • Año • Duración.

### Lista de Reproducción (Tracklist)
Diseño de tabla (`grid` o `flexbox`) con las siguientes columnas habituales:
1.  **#:** Número de pista (cambia a un ecualizador verde animado cuando suena esa canción).
2.  **Título:** Título (blanco) + Artista (gris).
3.  **Álbum:** Nombre del álbum al que pertenece.
4.  **Fecha de adición:** (Visible en playlists).
5.  **Duración:** Tiempo en minutos/segundos.

---

## 3. Paleta de Colores y Estilos (Design System)

La interfaz utiliza un tema oscuro (*Dark Mode*) por defecto para reducir la fatiga visual y resaltar el contenido (las carátulas).

| Elemento | Color Hex | Uso |
| :--- | :--- | :--- |
| **Fondo Principal** | `#121212` | Fondo de la aplicación. |
| **Fondo Base** | `#000000` | Barra inferior y lateral. |
| **Verde Spotify** | `#1DB954` | Acentos, Play, Activo, Hover links. |
| **Texto Principal** | `#FFFFFF` | Títulos, nombres de canción activos. |
| **Texto Secundario**| `#B3B3B3` | Artistas, duración, iconos inactivos. |

> **Nota de UX:** Los elementos interactivos suelen tener una transición de opacidad o color al pasar el ratón por encima (`hover state`), generalmente iluminándose a blanco o verde.

---

## 4. Ejemplo de Código (Implementación)

A continuación, se presentan ejemplos prácticos para replicar la barra de reproducción ("The Footer").

### A. Estructura Visual (HTML y CSS)
Este código crea el esqueleto visual de la barra inferior utilizando *Flexbox* para alinear las tres secciones (Info, Controles, Volumen).

**HTML:**

```html
<footer class="spotify-player">
  <div class="player-left">
    <img src="album-art.jpg" alt="Cover" class="album-cover" />
    <div class="track-info">
      <span class="track-name">Blinding Lights</span>
      <span class="artist-name">The Weeknd</span>
    </div>
    <button class="like-btn">♡</button>
  </div>

  <div class="player-center">
    <div class="player-controls">
      <button class="control-btn shuffle">🔀</button>
      <button class="control-btn prev">⏮</button>
      <button class="control-btn play-pause">▶</button>
      <button class="control-btn next">⏭</button>
      <button class="control-btn loop">🔁</button>
    </div>
    <div class="playback-bar">
      <span class="time current">0:00</span>
      <div class="progress-bar"><div class="progress-fill"></div></div>
      <span class="time total">3:20</span>
    </div>
  </div>

  <div class="player-right">
    <button class="extra-btn">🎤</button>
    <button class="extra-btn">Devices</button>
    <div class="volume-bar">
      <span>🔊</span>
      <div class="progress-bar volume"><div class="progress-fill"></div></div>
    </div>
  </div>
</footer>