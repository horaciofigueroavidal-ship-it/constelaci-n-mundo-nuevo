# Constelación Mundo Nuevo — sitio independiente

Grafo interactivo de autores, conceptos, números y artículos citados de la
revista *Mundo Nuevo* (1966–1968), reconstruido a partir de los wikilinks de
la bóveda de Obsidian del Proyecto FAI. Este paquete es un sitio estático
autónomo: no depende de Obsidian ni de ningún servicio de Claude/Anthropic.

## Estructura

```
sitio-constelacion/
├── index.html            página completa (HTML + CSS + JS + datos del grafo embebidos)
├── assets/
│   └── d3.min.js          librería D3 v7.9.0, vendorizada en local
├── documentos/
│   ├── REVISTAS/          28 números completos de la revista (PDF)
│   ├── GLOSARIO/          70 artículos recortados, uno por sub-carpeta de concepto (PDF)
│   ├── glosario_mundo_nuevo.pdf
│   └── autores_mundo_nuevo.pdf
└── README.md              este archivo
```

`index.html` referencia los PDF con rutas relativas (`documentos/...`), así
que la carpeta `documentos/` tiene que viajar siempre junto al `index.html` y
mantener su estructura interna intacta (no renombrar ni aplanar las
sub-carpetas de `GLOSARIO/`).

## Cómo alojarlo

Cualquier servidor de archivos estáticos sirve: subí la carpeta completa tal
cual a GitHub Pages, Netlify, Vercel, un hosting compartido o un bucket S3
con hosting estático activado. No hace falta build ni backend.

También podés abrir `index.html` con doble clic desde el explorador de
archivos — funciona igual, salvo que algunos navegadores son más estrictos
con archivos abiertos vía `file://`; si algo no carga, probá servirlo con un
servidor mínimo, por ejemplo desde esta misma carpeta:

```
python -m http.server 8000
```

y abrí `http://localhost:8000` en el navegador.

## Dependencias externas

- **D3.js**: vendorizado en `assets/d3.min.js`, no se descarga de ningún CDN.
- **Google Fonts** (Fraunces, Piazzolla, IBM Plex Mono): se siguen cargando
  desde `fonts.googleapis.com` vía `<link>` en el `<head>`. Es la única
  llamada de red que hace la página además de abrir los PDF locales. Si
  necesitás que el sitio funcione 100% sin conexión, hay que descargar los
  `.woff2` de esas tres familias y reescribir los `@font-face` para que
  apunten a archivos locales (no incluido en este paquete).

No hay ninguna dependencia de Claude, Anthropic ni de la plataforma de
Artifacts: el archivo es HTML/CSS/JS plano.

## Datos

El grafo (120 nodos, 208 conexiones) está embebido como JSON dentro de
`index.html` (`const GRAPH_DATA = {...}`), extraído de los wikilinks
`[[...]]` de las notas en `AUTORES/`, `CONCEPTOS/` y de los enlaces a
`REVISTAS/` y `GLOSARIO/`. Si la bóveda de Obsidian cambia (nuevos autores,
conceptos o artículos), este archivo no se actualiza solo: hay que volver a
generar el JSON y pegarlo en `index.html`.
