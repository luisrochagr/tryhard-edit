# Tryhard Edit · Phoenix OS × Luis Rocha

Skill de Claude que edita reels verticales hablados a cámara al estilo **Tryhard Edit**: cortes sin huecos, gancho disruptivo en los primeros 2 segundos, tracking de cara, títulos detrás del sujeto, motion graphics, transiciones con blur y mucho SFX, todo dentro de la zona segura de Reels.

## Instalación (Claude web o escritorio)

1. Descarga [`tryhard-edit.zip`](tryhard-edit.zip) (botón **Download**). No lo descomprimas.
2. En Claude: **Customize → Skills → + → Upload a skill** y elige el zip.
3. **Settings → Capabilities**: activa la ejecución de código y agrega a los dominios permitidos `huggingface.co` y `*.hf.co`.
4. En la app de escritorio, conecta con **Add folder** tus carpetas `Mis reels` (con `1 Brutos`, `2 Mi estilo` y `3 Listos`) y `SOUND FX`.

## Uso

```text
Hazle un tryhard edit a este reel.
Marca: {tu marca}  ·  Objetivo: {que comenten X}  ·  Vía libre: {sí / no}
```

Tu criterio va en `Mis reels/2 Mi estilo/mi-estilo.md`; Claude lo lee antes de cada edición.

Hecho por Luis Rocha · Phoenix OS.
