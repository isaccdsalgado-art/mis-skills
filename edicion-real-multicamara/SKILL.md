---
name: edicion-real-multicamara
description: >-
  Proceso completo de Edición Real Multicámara en Adobe Premiere Pro: importación y
  clasificación de footage por carpetas, sincronización sin espacios en negro, anidado
  por cámara y creación de clip multicámara, detección de hablantes y silencios en las
  pistas de audio, cortes exclusivos en pausas/silencios (con 30% de plano general/Cámara C
  de mínimo 5s) y selección real del ángulo activo ([MC1], [MC2], [MC3], [MC4]) moviendo el
  playhead y seleccionando 1, 2, 3, 4 con el teclado sobre el clip multicámara nativo. Úsese
  siempre que el usuario pida armar, cortar o cambiar ángulos automáticamente en una edición
  multicámara o video podcast en Premiere Pro.
---

# Edición Real Multicámara (Premiere Pro)

Sigue este procedimiento paso a paso y respeta todas las reglas obligatorias.

## Reglas Obligatorias
1. **Orden en carpetas (Bins)**: Cada elemento creado (secuencias anidadas, clip multicámara, copias de línea de tiempo, audios y videos) debe clasificarse en sus debidas carpetas (`FOOTAGE`, `AUDIO`, `VIDEO`, `COMPS`, `SEQUENCES`) para mantener el proyecto ordenado.
2. **Protección de originales**: Toda línea de tiempo se duplica (`duplicate_sequence`) antes de editar, sin tocar jamás los originales.
3. **Vista Multicam**: Activar la ventana de vista multicámara en el Monitor de Programa.
4. **La selección sigue al cabezal de reproducción**: En la barra superior del panel de Línea de Tiempo (*Timeline*), mantener activo el icono **"La selección sigue al cabezal de reproducción"** (*Selection Follows Playhead*) para que al mover el *playhead* sobre cada corte se seleccione automáticamente el clip multicámara correspondiente.
5. **Cortes en el clip Multicámara exterior**: Los cortes y cambios de ángulo se hacen sobre el **clip multicámara** en la secuencia principal duplicada, nunca dentro de la secuencia fuente del multicam.
6. **Bloqueo de audio máster**: Antes de cortar o cambiar ángulos en `V1`, bloquear las pistas de audio (`lock_track` en `A1`, `A2`, etc.) para evitar cortes o cambios accidentales en los micrófonos; desbloquearlas al finalizar.

---

## Fase 1: Importación y Clasificación
1. **Importar footage (`import_media` / `import_folder`)**:
   - Identificar archivos de video y audio.
   - Clasificar por ángulos y personas:
     - `Cámara A` (Persona 1 -> Ángulo `1`)
     - `Cámara B` (Persona 2 -> Ángulo `2`)
     - `Cámara C` (Plano General / FULL -> Ángulo `3`)
     - `Cámara D` (si aplica -> Ángulo `4`)
     - `Audio A`, `Audio B`, `Audio C` (etiquetar los encabezados de pista de audio con el nombre de cada hablante).

## Fase 2: Sincronización y Armado del Clip Multicámara
1. **Apilar y sincronizar**:
   - Apilar los videos en diferentes pistas (`V1`, `V2`, `V3`...) y los audios también en sus pistas (`A1`, `A2`...).
   - Con ayuda de Premiere, sincronizar audios y videos recortando los extremos para **no dejar espacios en negro**.
   - Si alguna cámara se interrumpe en medio de la grabación, cubrir ese tramo con `Cámara C` (Full) o con la cámara activa, nunca dejando negro.
2. **Anidar cada cámara**:
   - Anidar cada pista conservando el nombre de la cámara o persona.
3. **Convertir en clip Multicámara**:
   - Anidar los 3 clips de cámara y convertir en un clip multipista con *Click derecho -> Multicámara -> Habilitar*.
   - Duplicar la línea de tiempo principal (`duplicate_sequence`) antes de empezar a cortar.

## Fase 3: Análisis de Hablantes, Pausas y Regla del 30% (Cámara C)
1. **Análisis de ondas de audio**:
   - Analizar los archivos `.WAV` originales de cada micrófono (respetando el `inPoint` exacto de cada clip en la secuencia).
   - Nota técnica: las grabadoras de audio suelen grabar en **32-bit float** (`fmt=3`); usar `ffmpeg` o lectura binaria directa en lugar de librerías limitadas a PCM 16/24-bit.
   - Medir la energía RMS en ventanas de `100 ms` para detectar quién habla y dónde están las pausas.
2. **Reglas de corte**:
   - **Cortar solo en pausas o silencios**: Todos los cortes se hacen en pausas o silencios (ej. caída debajo de `-45 dBFS` o `>= 12 dB` durante al menos `0.3s`), **nunca a mitad de palabra**.
   - **Duración mínima de plano (anti-ping-pong)**: Mantener al menos `2.5s - 3s` por plano de hablante para no cortar por reacciones breves (*"ajá"*, *"sí"*).
   - **Cámara C (30% del video y mínimo 5 segundos)**:
     - Hacer los cortes identificando a la persona hablante (`1` o `2`).
     - La **Cámara C** (`3` - Plano General) debe representar el **30% del video**, dejándola **mínimo 5 segundos** antes del siguiente corte.
     - Priorizar `Cámara C` en: inicio/cierre, momentos donde ambos hablan a la vez (`>= 0.8s`), y como plano de respiro cada `15–20s` en monólogos largos.

## Fase 4: Ejecución de Cortes, Selección por Playhead y Verificación
1. **Preparar pistas y foco**:
   - Bloquear las pistas de audio máster (`lock_track`).
   - Seleccionar `Vídeo 1` como pista objetivo (`set_target_track`).
2. **Cortar el clip multicámara (`split_clip`)**:
   - Ejecutar `split_clip` sobre el clip multicámara en cada punto de pausa/silencio calculado.
3. **Mover el Playhead y cambiar el ángulo con el teclado (`1`, `2`, `3`, `4`)**:
   - Asegurar que el foco de Premiere esté en el panel **Línea de Tiempo** (o **Monitor de Programa**), con la vista multicámara y **"La selección sigue al cabezal de reproducción"** activos.
   - Mover el *playhead* (`set_playhead_position`) al centro de cada segmento del clip multicámara (y asegurar selección con `set_clip_selection` si hace falta) y presionar **`1`, `2`, `3` o `4`** con el teclado (`osascript` / Accesibilidad macOS) para cambiar el ángulo.
4. **Verificación visual y guardado**:
   - Exportar al menos 3 fotogramas (`export_frame`) en segmentos asignados a `Cámara A`, `Cámara B` y `Cámara C` para confirmar visualmente que los ángulos cambiaron correctamente.
   - Desbloquear las pistas de audio y guardar el proyecto (`save_project`).
