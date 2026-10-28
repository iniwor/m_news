# Portadas PNG directas para Buffer

La plantilla visual original permanece sin cambios en index.html.

Para crear 1 o 2 portadas, añade un archivo JSON nuevo al directorio requests/ en la rama main. Ejemplo:

    {
      "items": [
        {
          "titulo": "Título comprobado de la noticia",
          "imagen": "https://ejemplo.org/fotografia.jpg",
          "slug": "01-noticia"
        }
      ]
    }

Al añadir el archivo se ejecuta GitHub Actions. El renderizador abre el mismo generador original m_news y captura el canvas a resolución 1350 × 1080. El PNG queda guardado en portadas/<id-solicitud>/<slug>.png, y se crea portadas/<id-solicitud>/manifest.json.

URL del manifiesto:
https://raw.githubusercontent.com/iniwor/m_news/main/portadas/<id-solicitud>/manifest.json

Cada entrada del manifiesto tiene un campo url_directa que Buffer acepta como URL de imagen. Antes de programar, verificar que el manifiesto y el PNG final sean accesibles. Si falla la generación, no programar.

Nunca incluir credenciales en la página HTML ni en las solicitudes JSON. El workflow necesita permisos de escritura sobre contents; las solicitudes deben crearse mediante el conector GitHub autorizado.
