# Portadas WebP optimizadas para Buffer

La plantilla original de `index.html` no se modifica. La salida conserva 1350 × 1080 píxeles.

Para crear una o dos portadas, añade un JSON a `requests/<id-unico>.json` en `main`:

```json
{
  "items": [
    {
      "titulo": "Título verificado de la noticia",
      "imagen": "https://ejemplo.org/fotografia.jpg",
      "slug": "01-noticia"
    }
  ]
}
```

GitHub Actions abre el generador m_news, captura el canvas original en PNG y genera además un WebP optimizado con Sharp, comenzando en calidad 90 y bajando gradualmente hasta 82 solo si hace falta para intentar no superar 700 KiB. Si el objetivo no es posible con calidad mínima 82, se conserva el WebP resultante y se indica `objetivo_700kb: false` en el manifiesto; no se sacrifica más calidad.

El resultado se guarda en `portadas/<id>/<slug>.webp` y `portadas/<id>/<slug>.png` (respaldo). El manifiesto está en:

`https://raw.githubusercontent.com/iniwor/m_news/main/portadas/<id>/manifest.json`

**Para Buffer:** usa `portadas[].url_directa` (WebP), nunca el PNG de respaldo ni la URL del generador. Verifica la disponibilidad de la URL y el tipo de imagen antes de programar. El manifiesto incluye `url_png`, `calidad`, `bytes`, `bytes_png`, `ahorro_pct` y `objetivo_700kb`. Si Buffer rechaza WebP para algún canal, usa `url_png` y registra la incidencia, sin inventar enlaces ni alterar la plantilla.

Nunca incluyas tokens ni credenciales en archivos públicos o solicitudes JSON. Si falla el renderizado o la carga de imágenes, no programes.
