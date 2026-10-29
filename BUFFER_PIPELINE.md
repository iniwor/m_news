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

## Recuperación automática de errores

Cada noticia puede incluir `"imagenes_alternativas": ["https://foto-alternativa-1.jpg", "https://foto-alternativa-2.jpg"]` (hasta **3**). Las URLs deben ser HTTPS, reales, verificadas y mostrar **el mismo hecho** que la noticia; nunca inventarlas ni usar imágenes genéricas. El render reintenta la fotografía principal una vez y, si no carga, prueba las alternativas en orden. El manifiesto registra `imagen_utilizada`, `alternativa_usada` e `intentos_imagen` para auditar la recuperación.

Al guardar portadas, GitHub Actions reintenta hasta **5 veces** el `git push` cuando llegan commits concurrentes: sincroniza la rama con `git fetch` y `git rebase`, sin sobrescribir cambios remotos. Si el rebase no se puede resolver automáticamente, el trabajo falla explícitamente.

Si todas las fotos fallan, no se publica la noticia en Buffer. El editor debe buscar otra fotografía verificable del mismo hecho y enviar una solicitud nueva, o descartar la noticia. Las pruebas técnicas no se envían a redes sociales.
