# Tryhard Edit · Phoenix OS × Luis Rocha

Skill de Claude que edita reels verticales hablados a cámara al estilo **Tryhard Edit**: cortes sin huecos, gancho disruptivo en los primeros 2 segundos, tracking de cara, títulos detrás del sujeto, motion graphics, transiciones con blur y mucho SFX, todo dentro de la zona segura de Reels.

La primera vez que la usas, Claude monta el sistema de carpetas en **tu** computadora y guarda tus rutas en `config.json`. No trae rutas de nadie más.

## Instalación

**Opción A · Desde este repo (se actualiza sola)**

1. En Claude (web o escritorio): **Customize → Plugins → Add → Add marketplace**.
2. Pega: `https://github.com/luisrochagr/tryhard-edit`
3. En **Discover**, abre **tryhard-edit** y pulsa **Add**.

En Claude Code (terminal):

```text
/plugin marketplace add luisrochagr/tryhard-edit
/plugin install tryhard-edit@luis-rocha
```

**Opción B · Subiendo el zip**

1. Descarga [`tryhard-edit.zip`](tryhard-edit.zip) (botón **Download**). No lo descomprimas.
2. En Claude: **Customize → Skills → + → Upload a skill** y elige el zip.

## Antes de la primera edición

1. **Settings → Capabilities**: activa **Create and edit files** y agrega a los dominios permitidos `huggingface.co` y `*.hf.co` (para transcribir).
2. Abre la **app de escritorio**, conecta una carpeta con **"+" → Add folder** (por ejemplo Documentos) y, si tienes, tu carpeta de efectos de sonido.
3. Escribe: `Instala tryhard edit en mi computadora.`

Claude crea:

```text
Mis reels/
├── 1 Brutos/          ← el video tal como salió del teléfono
├── 2 Mi estilo/       ← mi-estilo.md y reglas-de-edicion.md
├── 3 Listos/          ← guion aprobado y versiones exportadas
└── _herramientas/tryhard-edit/
    ├── config.json    ← rutas de tu equipo
    └── sfx_map.json   ← qué sonido va en cada evento
```

## Uso

```text
Hazle un tryhard edit a IMG_1234.
Marca: {tu marca}  ·  Objetivo: {que comenten X}  ·  Vía libre: {sí / no}
```

Claude te deja el guion en `3 Listos` y no edita hasta que digas "sí". Cada corrección que le hagas queda escrita en `2 Mi estilo/reglas-de-edicion.md` y la aplica en el siguiente video.

## Estructura del repo

```text
.claude-plugin/marketplace.json
plugins/tryhard-edit/.claude-plugin/plugin.json
plugins/tryhard-edit/skills/tryhard-edit/SKILL.md   ← el criterio
SKILL.md                                           ← copia para leer en GitHub
tryhard-edit.zip                                   ← para la opción B
```
