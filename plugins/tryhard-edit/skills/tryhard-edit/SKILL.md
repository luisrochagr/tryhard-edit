---
name: "tryhard-edit"
description: "Edita reels talking-head al estilo Tryhard Edit de Luis Rocha: cortes sin huecos, gancho disruptivo, tracking de cara, títulos detrás/delante del sujeto, motion graphics, blur y mucho SFX. Úsala cuando pidan tryhard edit o editar un reel con ese nivel. La primera vez instala las carpetas y guarda las rutas del equipo del usuario."
---

# Tryhard Edit

Estilo de edición de reels verticales (1080×1920) creado por Luis Rocha (growth operator). Referencia original: reel de Sebas / Adsurdo (IMG_3680, nov-2026).

La meta no es "editar bonito": es **ganar la atención en el primer segundo, convencer en el desarrollo y hacer que comenten**, con un acabado limpio y profesional, posicionando la marca.

Esta skill funciona en cualquier computadora. Nunca asumas rutas: todas salen de `config.json` (ver Paso 0).

---

## Paso 0 · Primer arranque (instalación en el equipo del usuario)

Hazlo **siempre antes de editar**. Si ya existe la configuración, toma 5 segundos; si no, instala.

### 0.1 ¿Ya está instalado?
Busca `config.json` en `_herramientas/tryhard-edit/` dentro de las carpetas conectadas del usuario.
- **Existe** → léelo, confirma en una línea que las rutas siguen existiendo y salta al Paso 1.
- **No existe** → sigue con 0.2. Avisa en una frase: "Es la primera vez: voy a dejar listo tu sistema de carpetas (2–5 min)."

### 0.2 Carpeta raíz
- Necesitas acceso a una carpeta de su computadora. Si no hay ninguna conectada, pídele que abra este chat en la **app de escritorio de Claude** y elija **"+" → "Add folder"** (o pide acceso a la carpeta con la herramienta de carpetas si existe en la sesión).
- Propón crearla en Documentos con el nombre **`Mis reels`**. Si ya tiene una carpeta de edición, usa la suya.

### 0.3 Crear la estructura
```
Mis reels/
├── 1 Brutos/              ← aquí deja el video tal como salió del teléfono
├── 2 Mi estilo/
│   ├── mi-estilo.md       ← marca, paleta, fuentes, palabras prohibidas
│   └── reglas-de-edicion.md  ← lo que le gusta / no le gusta (crece con el feedback)
├── 3 Listos/              ← guion aprobado + versiones exportadas (_v1, _v2…)
└── _herramientas/
    └── tryhard-edit/
        ├── config.json        ← rutas de ESTE equipo
        ├── sfx_map.json       ← qué archivo de sonido usar para cada evento
        ├── tryhard_render.py  ← imagen (se genera/ajusta por video)
        └── tryhard_mix.py     ← sonido
```
- Crea solo lo que falte. **Nunca borres ni sobrescribas** archivos que ya estén.
- `mi-estilo.md`: créalo con estas preguntas respondidas por el usuario (máximo 4, con opción recomendada): nombre de la marca y cómo se escribe · color principal (hex) · ¿títulos en SF Pro o Inter? · palabras/expresiones que nunca usa. Si no responde, deja la plantilla con la paleta por defecto (§7).
- `reglas-de-edicion.md`: arranca con la sección "Preferencias por defecto" (§10) para que el usuario la edite.

### 0.4 Sonidos (SFX)
Pregunta: "¿Tienes una carpeta de efectos de sonido?"
- **Sí** → pídele que la conecte igual que la raíz. Lista sus subcarpetas y archivos y arma `sfx_map.json` asignando a cada **evento** (§8) el archivo más parecido por nombre (hit, whoosh, click, riser, notification, snap, glitch, data, fail…). Muéstrale el mapa en una tabla corta y sigue.
- **No** → crea `Mis reels/_herramientas/sfx/` y dile qué categorías poner ahí (lista de §8). No descargues sonidos de sitios desconocidos. Sin SFX el video se puede editar igual: avisa que saldrá sin efectos.

### 0.5 Fuentes
Busca SF Pro Display (BlackItalic, HeavyItalic, SemiboldItalic, Regular, Medium, Bold):
- Mac: `~/Library/Fonts` y `/Library/Fonts` · Windows: `C:\Windows\Fonts` y `%LOCALAPPDATA%\Microsoft\Windows\Fonts`.
- Si no está → usa **Inter** (libre; se instala en el contenedor desde PyPI o GitHub). Nota de licencia: Apple limita SF Pro a diseños para sus plataformas; para clientes, Inter es la opción segura.
- Guarda la ruta de cada peso en `config.json`.

### 0.6 Permisos de red (una sola vez)
La transcripción usa faster-whisper, que descarga el modelo de Hugging Face. Pídele al usuario que permita en **claude.ai → Settings → Capabilities**: `huggingface.co` **y** `*.hf.co` (o `us.aws.cdn.hf.co` + `cas-server.xethub.hf.co`). No intentes saltarte el bloqueo del proxy.

### 0.7 Guardar `config.json`
```json
{
  "version": 1,
  "sistema": "mac | windows",
  "raiz": "<ruta absoluta de Mis reels>",
  "brutos": "<raiz>/1 Brutos",
  "estilo": "<raiz>/2 Mi estilo",
  "listos": "<raiz>/3 Listos",
  "herramientas": "<raiz>/_herramientas/tryhard-edit",
  "sfx": "<ruta de la carpeta de sonidos o null>",
  "fuentes": { "titulo": "<ruta BlackItalic>", "clave": "<ruta HeavyItalic>", "subtitulo": "<ruta Regular>" },
  "familia": "SF Pro Display | Inter",
  "marca": { "nombre": "", "acento": "#7A5CFF" },
  "instalado": "<fecha>"
}
```
Cierra con un resumen de 3 líneas: qué se creó, qué falta (SFX, fuentes, permisos) y "Deja tu primer bruto en `1 Brutos` y dime 'edítalo'".

---

## Paso 0.8 · Antes de cada edición
1. Crea la lista de tareas. Lee `config.json`, `2 Mi estilo/mi-estilo.md` y `2 Mi estilo/reglas-de-edicion.md`.
2. Ubica el bruto (en `1 Brutos` o adjunto). Si pesa >30 MB: edita en el contenedor y entrega solo el resultado.
3. Marca/cliente y cómo se escribe (ej.: **Adsurdo**, no "absurdo").
4. Canción de fondo (opcional; si no hay, se avisa y se recomienda añadirla en Instagram).

### Entorno: trampas ya resueltas
- faster-whisper puede fallar con `av.open(... metadata_errors)`: pásale el audio como numpy (`wave` → float32/32768).
- OpenCV 5 ya no trae los haarcascades y GitHub raw puede estar bloqueado: instala `opencv-python-headless<5` (PyPI).
- rembg `u2net_human_seg` se descarga de GitHub releases (sí funciona).
- `git clone` dentro de la carpeta compartida falla: clona fuera y copia sin `.git`.
- Las herramientas de archivos del equipo admiten ≤30 MB por archivo.
- El trabajo pesado va en el contenedor, no en la shell del equipo del usuario.

---

## 1. Transcribir
- Proxy de trabajo: `scale=-2:1280,fps=30` → `a.wav` mono 16 kHz.
- faster-whisper `small`, `language='es'`, `word_timestamps=True` → `words.json` `[inicio, fin, palabra]`.
- Los tiempos de whisper al inicio de cada frase suelen adelantarse ~0,5 s: los silencios reales se miden con `silencedetect`.

## 2. Limpiar el contenido (errores del personaje son normales)
- Detecta **tomas repetidas** (misma frase dicha 2+ veces): quédate con la **última toma completa y limpia**.
- Quita **ideas repetidas** aunque cambie la redacción.
- Quita arranques bajitos/de fondo, cues ("Dale"), muletillas.
- Corrige texto: nombres de marca, tildes.

## 3. Guion y aprobación
Guarda en `3 Listos/<video>_guion.md`: tabla por momentos (segundo, lo que dice, lo que se ve, efecto, herramienta) + lista SE QUEDA / SE VA con tiempos del bruto. Pregunta máximo 3 cosas con opción recomendada. **No edites hasta el "sí"** (salvo que el usuario ya haya dado vía libre).

## 4. Estructura cognitiva (lo que manda)
En Instagram el 99% está en piloto automático. Orden obligatorio:

| Momento | Objetivo mental | Cómo |
|---|---|---|
| 0–2 s | **Disrupción** (romper el patrón, dolor/pérdida) | Abrir con la frase más fuerte aunque esté más adelante en el bruto (reordenar). Texto gigante visible desde el fotograma 0, golpe de SFX, flash, sacudida, color de alerta. |
| Desarrollo 1 | **Problema / identificación** | Visualizar el dolor (gráfica cayendo en rojo, repetición en eco). |
| Desarrollo 2 | **Solución + autoridad + marca** | Nombre de la marca gigante detrás del sujeto, insignias, inserto a pantalla completa. |
| Desarrollo 3 | **Prueba / resultado** | Contraste: medidor que se llena rojo→verde, gráfica subiendo en verde. |
| Cierre | **Acción** | Palabra de acción gigante + UI de comentario que se escribe sola + cierre en eco. |

La marca debe verse grande al menos una vez. **Nunca inventes cifras**: si el video no las dice, la gráfica no lleva números.

## 5. Cortes: cero huecos
- Silencios con `silencedetect=n=-36dB:d=0.18` **dentro** de cada frase elegida; corta todo >0,18 s dejando 0,06 s de margen.
- Fusiona microcortes <0,15 s del mismo tramo (no saltar dentro de una palabra).
- Ajusta cada tramo a fotogramas exactos (n/30) y recorta el audio a esa duración (`apad,atrim`) con fades de 12–15 ms → sin desincronía.
- Extrae del original a **1440×2560** para que los zooms no pierdan calidad. Concat → `base.mov`. Guarda `timeline.json` (src_a, src_b, off).

## 6. Tracking de cara
- Haar frontal (+ perfil de respaldo) sobre cada fotograma reducido a 480 px.
- Filtra imposibles, interpola huecos, mediana 9 + media 11, **por tramo** (reinicia en cada corte) → `track.json` (x, y, ancho en coords de 1440×2560).
- La cámara centra la cara en x y la coloca al 42% de alto.

## 7. Imagen: reglas del Tryhard Edit

### Zona segura de Reels (obligatoria)
Nada de texto fuera de: arriba ≥250 px, abajo ≤1350 px, derecha ≤920 px, izquierda ≥60 px. Centro horizontal de textos ≈ x 480–490. Revisa con las zonas pintadas en rojo sobre los fotogramas.

### Subtítulos rápidos
- **Siempre debajo de la cara** (tracking): y = cara_y + 0,78·ancho_cara + 55, limitado a 760–1240; x sigue la cara dentro de la zona segura.
- 1–3 palabras; no terminar trozo en palabra débil (de, tu, la, y, te, un, lo, en, es, a, que); mínimo 0,28 s en pantalla.
- Fuente `fuentes.subtitulo` 72 + palabras clave en `fuentes.clave` 80, en minúsculas naturales (marca como se escribe).
- **Sin borde negro**. Solo sombra suave (blur 12–14, opacidad ~120–150).
- Entrada con **blur-in** (desenfoque 12→0 + escala 1,05→1 en 0,2 s). Nada de rebotes.
- Se ocultan mientras hay un título grande que dice lo mismo.

### Títulos grandes (`fuentes.titulo`)
- **Detrás del sujeto** (máscara rembg `u2net_human_seg` a 540×960, fondo atenuado ~27%): marca, conceptos clave y la palabra de acción. y = cara_y − 0,72·ancho_cara − 60, para que la cabeza solo roce la base de las letras. Ancho máx. 860.
- **Delante, grande**: golpes y cierres con **eco**: palabra principal delante, 3 copias decrecientes detrás del sujeto.
- Entrada blur-in + escala 1,12→1; salida con blur.
- Palabras pequeñas encima de las grandes, **juntas** (poco aire).

### Motion graphics (catálogo)
- Tarjeta de vidrio esmerilado (crop del fondo + blur 24 + blanco 150–175 alfa, radio 40, sombra suave) en la franja 290–600 px.
- Gráfica de rendimiento: curva que se dibuja, rojo cayendo (problema) / verde subiendo (solución), chip "↓ cayendo" / "↑ subiendo", punto final con halo.
- Medidor 0→100% rojo→verde.
- Insignia con degradado del acento de marca y destellos, junto a la cabeza.
- Inserto a pantalla completa: fondo oscuro, tarjetas que se multiplican 1→2→3→4→6→9 columnas, velo central y título.
- Caja de comentario de vidrio que se escribe sola, cursor azul, "Publicar" que se activa al terminar. Nunca se solapa con el título de acción.
- Degradado oscuro superior cuando hay texto sobre fondo claro.
- Paleta por defecto: blanco + rojo #FF3B30 (dolor) + verde #34C759 (resultado) + violeta #7A5CFF→#B36BFF (marca). Si `mi-estilo.md` define otra, manda esa.

### Cámara y transiciones
- Zoom alternado por tramo (abierto 1,0–1,07 / cerrado 1,16–1,40) con deriva suave; cortes virtuales en tramos largos (>3,5 s).
- Gancho en plano cerrado (≈1,32–1,40).
- Pulso de zoom (+5%) en golpes clave; sacudida corta en las palabras de impacto.
- **Transición = blur** (desenfoque gaussiano ~18 px que cae en 0,1 s a cada lado del corte), también al entrar/salir del inserto. Flash blanco de 2 fotogramas solo en el golpe del gancho.

## 8. Sonido
- Voz: `acompressor threshold=-20dB ratio=3 attack=5 release=80 makeup=2`.
- SFX según `sfx_map.json`, **muchos pero discretos**: cada uno normalizado a pico −3 dBFS, recortado y con fade; el **bus de SFX ≈8 dB por debajo de la voz**.
- Eventos (el nombre entre paréntesis es el archivo que usa Luis, como referencia):
  - Apertura: golpe metálico (Metallic Hit) · Segunda palabra: disolución UI (UI Dissolve) · Golpe rojo: flash (Flash) · Palabra siguiente: hit corto (HIT blogovs)
  - Cortes: whoosh variados (Whoosh 01 / Swoosh / Woosh 02 / Hollow Whoosh)
  - Gráfica aparece: carga de datos ~1,3 s (Data Load) · Curva toca fondo: fallo (Failure) · Eco: clic rápido (Transition Rapid Click)
  - Insignia: ráfaga digital (Digital Burst) · Antes del inserto: último 1 s de un riser invertido que termina en el corte (Reverse Drum Riser) · Cada nivel de tarjetas: botón UI (UI Button 2) · Título del inserto: snap (Snap)
  - Medidor: tic (TIC blogovs) · Gráfica verde: menú UI (UI Menu) · Palabra de resultado: golpe metálico
  - Caja de comentario: notificación (Notification) · Tecleo: botón UI cada 2 letras (muy bajo) · Cierre: clic rápido + snap
- **No usar Clean Swoosh.**
- Música (si la dan): por debajo de la voz con ducking (sidechaincompress).
- Master: `alimiter 0.85` → loudnorm en 2 pasadas a **−14 LUFS, TP −1,5** (lineal).

## 9. Revisión y entrega
- Hoja de contacto de ~24 fotogramas con zonas seguras pintadas. Comprueba: gancho entendible sin sonido en 2 s, nada tapa cara/manos, títulos de detrás legibles, nada fuera de zona segura, sin choques entre gráficos.
- Mide LUFS final y la relación voz/SFX.
- Exporta 1080×1920 H.264 CRF 17 + AAC 192k `+faststart`. Guarda en `3 Listos/<video>_vN.mp4` (nunca borrar versiones anteriores), envíalo al chat y resume en pocas líneas qué se hizo y qué ajustarías. Di claramente que no puedes escucharlo y pide revisar uniones, volumen de SFX y sincronía en el móvil.
- Todo feedback del usuario se añade a `2 Mi estilo/reglas-de-edicion.md` (una línea por regla, SÍ / NO).

## 10. Preferencias por defecto (las de Luis; el usuario las cambia en reglas-de-edicion.md)
- "Más": movimiento, motion graphics, SFX — pero limpio y profesional (subtítulos de 1–3 palabras, claves en grande, notificaciones, insertos a pantalla completa).
- NO: borde negro en letras, Clean Swoosh, rebotes exagerados, pastillas blancas tipo etiqueta, huecos de silencio, textos en zonas que tapa Instagram, textos que tapan cara o manos.
- SÍ: blur en letras y transiciones, títulos en Black Italic, subtítulos en Regular, énfasis en Italic, textos detrás del sujeto, subtítulos debajo de la cara.
- Avanza y decide con criterio; pregunta solo lo imprescindible.

## 11. Scripts
Si `tryhard_render.py` y `tryhard_mix.py` no están en `config.herramientas`, reconstrúyelos desde este documento y guárdalos ahí. Para cada video nuevo cambia:
- `timeline.json`, `track.json`, `words.json` del video.
- `EV` (ventanas de cada evento), `NO_SUBS`, `VCUTS`, `CAM` (zoom por tramo), `PUNCH`, `SHAKE`, textos de títulos y posiciones, `KEY` (palabras clave), textos de gráficas y comentario.
- Prueba fotogramas sueltos (`python3 tryhard_render.py 0.3 2.5 6.9`) antes del render completo.
- `tryhard_mix.py`: lista `EVENTS` (evento → archivo según `sfx_map.json`, desde, duración, segundo, volumen) y el tecleo.
