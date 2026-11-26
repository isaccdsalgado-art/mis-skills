---
name: edicion-real-multicamara
description: >-
  Proceso completo de Edición Real Multicámara en Adobe Premiere Pro: importación y
  clasificación de footage dentro de la carpeta de cada episodio (incluyendo sus timelines
  EPX Edit y EPX SYNC MC), sincronización sin espacios en negro, anidado por cámara y
  creación de clip multicámara, detección de hablantes y silencios, cortes exclusivos en
  pausas/silencios (ritmo ~7-8s, reacciones de 1.7-2.9s, puente en Cámara C/FULL para
  cruces de mirada, 30% de plano general de mínimo 5s y Lower Thirds al inicio y mitad),
  puntos de revisión obligatorios con el usuario antes de avanzar entre etapas críticas,
  y selección real del ángulo activo ([MC1], [MC2], [MC3], [MC4]) moviendo el playhead y
  seleccionando 1, 2, 3, 4 con el teclado sobre el clip multicámara nativo. Úsese siempre
  que el usuario pida armar, cortar o cambiar ángulos automáticamente en una edición
  multicámara o video podcast en Premiere Pro.
---

# Edición Real Multicámara (Premiere Pro)

Sigue este procedimiento paso a paso, respeta todas las reglas obligatorias y **detente en cada punto de revisión crítica** hasta recibir confirmación explícita del usuario.

## Reglas Obligatorias
1. **Puntos de revisión crítica (alto obligatorio)**: Al terminar cada etapa crítica (**Fase 1**, **Fase 2**, **Fase 3** y **Fase 4**), **NO avances a la siguiente fase sin mostrar al usuario lo realizado y recibir su confirmación explícita** de que todo está bien.
2. **Cuidado con múltiples episodios en la misma carpeta**: Una misma carpeta de contenido puede contener archivos de varios episodios o grabaciones distintas. Antes de importar o vincular, inspecciona fechas, nombres de archivo, códigos de tiempo y duración para aislar únicamente los archivos del episodio actual, y confirma con el usuario si hay duda.
3. **Orden por carpeta de Episodio (Timelines dentro de su episodio)**: Cada elemento creado (secuencias anidadas, clip multicámara, copias de línea de tiempo, audios y videos) debe clasificarse dentro de la carpeta del episodio donde vive su footage, **nunca en una carpeta global aparte**.
   - Estructura recomendada: `Ep 1 / SEQ / EP1 Edit`, `EP1 SYNC MC` (junto con `FOOTAGE`, `AUDIO`, `VIDEO`, `COMPS` dentro de `Ep 1`).
4. **Protección de originales**: Toda línea de tiempo se duplica (`duplicate_sequence`) antes de editar, sin tocar jamás los originales.
5. **Vista Multicam**: Activar la ventana de vista multicámara en el Monitor de Programa.
6. **La selección sigue al cabezal de reproducción**: En la barra superior del panel de Línea de Tiempo (*Timeline*), mantener activo el icono **"La selección sigue al cabezal de reproducción"** (*Selection Follows Playhead*) para que al mover el *playhead* sobre cada corte se seleccione automáticamente el clip multicámara correspondiente.
7. **Cortes en el clip Multicámara exterior**: Los cortes y cambios de ángulo se hacen sobre el **clip multicámara** en la secuencia principal duplicada, nunca dentro de la secuencia fuente del multicam.
8. **Bloqueo de audio máster**: Antes de cortar o cambiar ángulos en `V1`, bloquear las pistas de audio (`lock_track` en `A1`, `A2`, etc.) para evitar cortes o cambios accidentales en los micrófonos; desbloquearlas al finalizar.

---

## Fase 1: Importación y Clasificación
1. **Verificar episodio correcto (si hay varios episodios en la carpeta)**:
   - Listar el contenido de la carpeta de footage y filtrar por episodio, fecha de grabación o prefijo antes de importar.
   - Si existen múltiples episodios en la misma carpeta, presentar la lista al usuario y confirmar cuáles archivos corresponden al episodio a editar.
2. **Importar footage (`import_media` / `import_folder`)**:
   - Importar únicamente los archivos de video y audio del episodio confirmado dentro de su carpeta de episodio (ej. `Ep 1`).
   - Clasificar por ángulos y personas:
     - `Cámara A` (Persona 1 -> Ángulo `1`)
     - `Cámara B` (Persona 2 -> Ángulo `2`)
     - `Cámara C` (Plano General / FULL -> Ángulo `3`)
     - `Cámara D` (si aplica -> Ángulo `4`)
     - `Audio A`, `Audio B`, `Audio C` (etiquetar los encabezados de pista de audio con el nombre de cada hablante).
3. **CHECKPOINT 1 (Revisión obligatoria)**: Mostrar al usuario qué archivos se importaron, en qué carpeta de episodio quedaron y qué ángulo/nombre corresponde a cada cámara y micrófono. **Esperar aprobación antes de pasar a Fase 2.**

## Fase 2: Sincronización y Armado del Clip Multicámara
1. **Apilar y sincronizar**:
   - Crear la secuencia dentro de la carpeta `SEQ` del episodio (ej. `Ep 1 / SEQ / EP1 SYNC MC`).
   - Apilar los videos en diferentes pistas (`V1`, `V2`, `V3`...) y los audios también en sus pistas (`A1`, `A2`...).
   - Con ayuda de Premiere, sincronizar audios y videos recortando los extremos para **no dejar espacios en negro**.
   - Si alguna cámara se interrumpe en medio de la grabación, cubrir ese tramo con `Cámara C` (Full) o con la cámara activa, nunca dejando negro.
2. **Anidar cada cámara**:
   - Anidar cada pista conservando el nombre de la cámara o persona y guardarlas dentro de la carpeta del episodio.
3. **Convertir en clip Multicámara**:
   - Anidar los 3 clips de cámara (`EP1 SYNC MC`) y convertir en un clip multipista con *Click derecho -> Multicámara -> Habilitar*.
   - Crear y duplicar la línea de tiempo principal dentro de `SEQ` del episodio (`EP1 Edit` -> `EP1 Edit copia`) antes de empezar a cortar.
4. **CHECKPOINT 2 (Revisión obligatoria)**: Indicar al usuario que revise la sincronización, que no haya espacios en negro, que la vista multicámara esté activa y que las secuencias estén dentro de la carpeta del episodio. **Esperar aprobación antes de pasar a Fase 3.**

## Fase 3: Análisis de Hablantes, Estilo de Montaje y Regla del 30%
1. **Análisis de audio (optimizado)**:
   - Si la secuencia ya tiene transcripción nativa en Premiere (`transcribe_audio`), aprovechar sus tiempos de hablantes y pausas.
   - Si no está transcrita, analizar los archivos `.WAV` originales de cada micrófono (respetando el `inPoint` exacto de cada clip en la secuencia) en bloques de `100 ms`.
   - Nota técnica: las grabadoras de audio suelen grabar en **32-bit float** (`fmt=3`); usar lectura binaria directa o `ffmpeg` en lugar de librerías limitadas a PCM 16/24-bit.
2. **Estilo de montaje, momentos y mirada**:
   - **Cortar solo en pausas o silencios**: Todos los cortes se hacen en pausas o silencios (ej. caída debajo de `-45 dBFS` o `>= 12 dB` durante al menos `0.3s`), **nunca a mitad de palabra**.
   - **Ritmo objetivo (`~7–8s` promedio por plano)**:
     - `~15–20%` **planos de reacción / mirada (`1.7s – 2.9s`)**: cortes breves a quien escucha para capturar sonrisas, asentimientos o cambios de mirada antes de responder.
     - `~45–50%` **diálogo dinámico (`3s – 8s`)**.
     - `~25%` **planos sostenidos (`8s – 15s`)**.
     - `~10%` **momentos largos (`15s – 40s`)** cuando la intensidad del discurso pide mantener la cámara fija.
   - **Transición de mirada en la intro**: Cuando el presentador mira a cámara al presentar al invitado y luego pasa a dialogar con él, insertar un **plano corto de reacción del invitado (`~1.9–2.5s`)** seguido de un **puente en `Cámara C` (`FULL`, `~4–5s`)** para mostrar el cruce de miradas antes de entrar al plano individual del invitado.
   - **Cámara C (30% del video y mínimo 5 segundos)**:
     - La **Cámara C** (`3` - Plano General) debe representar el **30% del video**, dejándola **mínimo 5 segundos** antes del siguiente corte.
     - Priorizar `Cámara C` en: puentes de mirada, momentos donde ambos hablan a la vez (`>= 0.8s`), y como plano de respiro cada `15–20s` en monólogos largos.
   - **Lower Thirds**: Colocar los *Lower Thirds* (`~7.5s`) al presentar a cada participante en la intro y **repetirlos a mitad del episodio** (`~10–12 min`).
3. **CHECKPOINT 3 (Revisión obligatoria)**: Presentar al usuario el resumen del plan de cortes (cantidad de cortes, ritmo promedio, reacciones, porcentaje de `Cámara A`, `Cámara B` y `Cámara C`) antes de modificar la línea de tiempo. **Esperar aprobación antes de pasar a Fase 4.**

## Fase 4: Ejecución de Cortes, Selección por Playhead y Verificación
1. **Preparar pistas y foco**:
   - Bloquear las pistas de audio máster (`lock_track`).
   - Seleccionar `Vídeo 1` como pista objetivo (`set_target_track`).
2. **Cortar el clip multicámara (`split_clip`)**:
   - Ejecutar `split_clip` sobre el clip multicámara en cada punto de pausa/silencio calculado.
3. **Mover el Playhead y cambiar el ángulo con el teclado (`1`, `2`, `3`, `4`) — optimizado**:
   - Enfocar la ventana de Premiere y el panel **Línea de Tiempo** (con la vista multicámara y **"La selección sigue al cabezal de reproducción"** activos) **una sola vez al inicio** para evitar esperas repetidas en cada corte.
   - Mover el *playhead* (`set_playhead_position`) al centro de cada segmento del clip multicámara (y asegurar selección con `set_clip_selection` si hace falta) y presionar **`1`, `2`, `3` o `4`** con el teclado (`osascript` / Accesibilidad macOS, con pausa breve `~0.15s`) para cambiar el ángulo.
4. **Verificación visual y guardado**:
   - Exportar al menos 3 fotogramas (`export_frame`) en segmentos asignados a `Cámara A`, `Cámara B` y `Cámara C` para confirmar visualmente que los ángulos cambiaron correctamente.
   - Desbloquear las pistas de audio y guardar el proyecto (`save_project`).
5. **CHECKPOINT 4 (Revisión final)**: Mostrar al usuario los resultados y fotogramas de verificación para que apruebe la edición final.
