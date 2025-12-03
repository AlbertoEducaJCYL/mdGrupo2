# Desglose de Interfaz: Playlist de Spotify

Este documento explica la arquitectura visual y lógica de una playlist estándar en la interfaz de escritorio/web de Spotify.

## 1. La Cabecera (The Header)
Es la sección estática superior que presenta la identidad de la playlist.

* **Imagen de Portada (Cover Art):** Suele ser un collage de 4 carátulas o una imagen personalizada.
* **Metadatos:** Título (editable), Descripción, Autor/Creador.
* **Estadísticas:** Conteo de likes (guardados), número de canciones y duración total.
* **Fondo Dinámico:** El fondo del header extrae el color dominante de la imagen de portada y aplica un gradiente (o degradado) que se desvanece hacia el negro en la lista de canciones.

[Image of Spotify playlist header UI components]

## 2. Barra de Acciones (Sticky Control Bar)
Esta barra suele volverse "pegajosa" (sticky) en la parte superior cuando haces scroll hacia abajo.

* **Botón Play (Global):** El círculo verde icónico. Controla el estado `PLAY/PAUSE` de la lista entera.
* **Herramientas:** Corazón (Guardar en biblioteca), Descarga (Offline) y el menú de "Más opciones" (`...`).
* **Buscador/Filtro:** Permite filtrar canciones *dentro* de la playlist actual sin hacer una nueva petición al servidor.
* **Ordenación (Sort):** Dropdown para ordenar por fecha, título, artista, etc.

## 3. La Lista de Canciones (The Tracklist)
Es el núcleo de la interfaz. Funciona como una tabla de datos interactiva.

### Componentes Visuales de la Fila (Row)
1.  **Índice/Estado:** Muestra el número de canción. Al pasar el mouse (hover), cambia al botón de "Play". Si suena esa canción, muestra un ecualizador animado.
2.  **Info Principal:** Título de la canción y nombre del artista (enlace).
3.  **Álbum:** Enlace al álbum de origen.
4.  **Fecha:** Cuándo se añadió a la playlist.
5.  **Duración:** Tiempo en `mm:ss`.

### Lógica Técnica Clave
* **Virtualización (Virtual Scrolling):** Para playlists con 5,000 canciones, Spotify **NO** renderiza todas a la vez. Solo renderiza las que son visibles en pantalla (más un pequeño buffer arriba y abajo) para ahorrar memoria RAM y mejorar el rendimiento.
* **Drag & Drop:** Si eres el dueño, puedes arrastrar filas para reordenar la lista. Esto actualiza el índice en el array local y envía la petición al backend.
* **Menú Contextual:** Click derecho en una fila abre opciones específicas (Añadir a cola, Ir al artista, etc.).

---

## 4. Ejemplo de Estructura (Pseudo-código React)

Para entenderlo a nivel de código, así se vería la estructura de componentes simplificada:

```jsx
// Contenedor Principal
const PlaylistView = ({ playlistData }) => {
  // Extraer color dominante para el fondo
  const dominantColor = extractColor(playlistData.coverImage);

  return (
    <div className="playlist-container" style={{ background: `linear-gradient(${dominantColor}, #121212)` }}>
      
      {/* 1. Cabecera con info */}
      <PlaylistHeader 
        title={playlistData.title} 
        image={playlistData.coverImage} 
        author={playlistData.owner}
      />

      {/* 2. Barra de Control (Play, Like, Download) */}
      <ActionBar 
        onPlay={() => playContext(playlistData.uri)} 
        isSaved={playlistData.isLiked}
      />

      {/* 3. Tabla de Canciones */}
      <div className="tracklist-header">
        <span>#</span> <span>Título</span> <span>Álbum</span> <span>Reloj</span>
      </div>

      <TrackList items={playlistData.tracks}>
        {/* Renderizado virtualizado suele ir aquí */}
        {playlistData.tracks.map((track, index) => (
          <TrackRow 
            key={track.id} 
            index={index + 1} 
            track={track} 
            isActive={currentTrack.id === track.id}
          />
        ))}
      </TrackList>

    </div>
  );
};