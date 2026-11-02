# Boletin 5

## Resumen de commits

| # | Mensaje del commit | Qué se hizo | Autor | Enlace |
|---|--------------------|-------------|-------|--------|
| 1 | `docs: script y workflow para crear un github pages para la documentación` | Creación de script y workflow para publicar una página | Javier García Cutillas | [f7fffae](https://github.com/jgarciacutillas/practicas-continuas-26-27/commit/f7fffaeaa6cda3a2ca29c948a72d0ddddefa9134) |
| 2 | `docs(boletin5): cambios en el boletin 5, añadida la parte d que ya está creada` | Se guarda la documentación de los boletines en el repositorio | Javier Garcia Cutillas | [e26cbe5](https://github.com/jgarciacutillas/practicas-continuas-26-27/commit/e26cbe5416fb5effd006cb2567c4840bd16fa835) |
| 3 | `ci(cd): pipeline de publicacion` | Se creo el workflow de release | Jonathan Aleksander Llumitasig Chuquiana | [786dc1e](https://github.com/jgarciacutillas/practicas-continuas-26-27/commit/786dc1e74a5cefe95659bd23215b9e6837ca611b) |
| 4 | `docs(boletin5): Punto A - Excepto ultimo apartado` | Se completo la documentaciñon del apartado A | Jonathan Aleksander Llumitasig Chuquiana | [752387a](https://github.com/jgarciacutillas/practicas-continuas-26-27/commit/752387a4c778f1a095865aa533bca00eafb73293) |
| 5 | `docs(boletin5): Se completa los puntos A, B y D` | Se completo el resto de apartados menos el C | Jonathan Aleksander Llumitasig Chuquiana | [d69760b](https://github.com/jgarciacutillas/practicas-continuas-26-27/commit/d69760b6a5f7ca9bf3ec420e99f1ea7183ed2a30) |
| 6 | `docs(boletin5): finalizacion del boletin 5` | Se finalizo la documentacion del boletin 5 | Jonathan Aleksander Llumitasig Chuquiana | [ed21130](https://github.com/jgarciacutillas/practicas-continuas-26-27/commit/ed21130bc17b33237041c1eceb72dbf6b753b54f) |

## Detalle por commit   

### 1. `docs: script y workflow para crear un github pages para la documentación`
- **Qué se hizo:** Creación de script y workflow para publicar una página en la que se pueden observar todos los boletines realizados durante este periodo. Se puede observar a través de esta [url](https://jgarciacutillas.github.io/practicas-continuas-26-27/).
- **Autor:** Javier García Cutillas
- **Relación con el boletín:** Parte D - GitHub Pages de la documentación
- **Uso de IA:** Claude

#### Prompt
```bash
generame el script: Como dice el [Boletín 0](https://practicas-continuas-umu.github.io/apuntes/practicas/boletin0-guia-del-curso.html), has ido guardando la memoria de cada sesión (`boletinX.md`) en un repositorio/carpeta aparte. Toca traerla al repositorio de la API y publicarla como una web navegable.

* Copia (o mueve) tus `boletinX.md` desde el repositorio donde los tenías a una carpeta `docs/` en la raíz del repositorio de la API, uno por sesión entregada hasta ahora (`docs/boletin1.md` … `docs/boletin5.md`).
* Dentro de `docs/`, escribe un script (node o python) que lea todos los `.md` de `docs/` y genere, para cada uno, un `.html` equivalente más un `index.html` que enlace a todos. Deben guardarse en `docs/site/`. Recomiendo usar la IA para esto.
* Prueba el script en local y revisa el resultado abriendo el HTML en el navegador.
```

```bash
cada boletin es una carpeta con fotos y el .md dentro
```

```bash
* Automatízalo con un workflow `.github/workflows/pages.yml` que, en cada push a `main`, ejecute el script y publique `docs/site/` en GitHub Pages con las acciones oficiales.
```

```bash
moidifca el build-site.mjs que sea bonito y minimalista
```

### 2. `docs(boletin5): cambios en el boletin 5, añadida la parte d que ya está creada`
- **Qué se hizo:** Se documetno el apartado D del boletin 5.
- **Autor:** Javier García Cutillas
- **Relación con el boletín:** Parte D - GitHub Pages de la documentación
- **Uso de IA:** No se ha usado

### 3. `ci(cd): pipeline de publicacion`
- **Qué se hizo:** Se crea el workflow para los release del repositorio para la publicación de versiones de nuestra API
- **Autor:** Jonathan Aleksander Llumitasig Chuquiana
- **Relación con el boletín:** Parte A - Pipeline de publicación
- **Uso de IA:** No se ha usado

### 4. `docs(boletin5): Punto A - Excepto ultimo apartado`
- **Qué se hizo:** Se documenta el apartado A del boletin 5, a excepcion del ultimo subapartado
- **Autor:** Jonathan Aleksander Llumitasig Chuquiana
- **Relación con el boletín:** Parte A - Pipeline de publicación
- **Uso de IA:** No se ha usado

### 5. `docs(boletin5): Se completa los puntos A, B y D`
- **Qué se hizo:** Se documenta los apartado A, B y D del boletin 5
- **Autor:** Jonathan Aleksander Llumitasig Chuquiana
- **Relación con el boletín:** Parte B - Publicar una versión de verdad
- **Uso de IA:** No se ha usado

### 6. `docs(boletin5): finalizacion del boletin 5`
- **Qué se hizo:** Se documenta todos los apartados del boletin 5 por completo
- **Autor:** Jonathan Aleksander Llunitasig Chuquiana
- **Relación con el boletín:** Parte C - Verificar la imagen publicada
- **Uso de IA:** No se ha usado


## Descripción narrativa de lo hecho en la sesión y relación con los commits

### Parte A - Pipeline de publicación

Creamos el workflow correspondiente que se ejecutara al fusionar una rama a `main` o cuando publiquemos una nueva version
```yml
name: Release

on:
  push:
    branches: [ main ]
    tags: [ "v*.*.*" ]

permissions:
  contents: read
  packages: write

jobs:
  publish:
    runs-on: ubuntu-latest
    timeout-minutes: 20
    steps:
      - uses: actions/checkout@v4

      - name: Login en GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Metadatos (tags)
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ghcr.io/${{ github.repository }}
          tags: |
            type=sha,prefix=sha-
            type=semver,pattern={{version}}
            type=semver,pattern={{major}}.{{minor}}
            type=semver,pattern={{major}}
            type=raw,value=latest,enable={{is_default_branch}}

      - name: Construir y publicar
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
``` 

**Explica qué ocurre en estas dos ocasiones:** 

**1) al fusionar un PR a main** 

Cuando se fusiona un PR a main, se genera un push a esa rama y se ejecuta el workflow. Y además con el step `Construir y publicar`, se generan distintas etiquetas para la imagen, sha-`<commit>` consiguiendo una trazabilidad absoluta de la imagen al commit que la produjo; además se genera la etiqueta `latest` porque se hace fusión a `main`, que se trata de nuestra rama principal. Y no se generan etiquetas de version porque este evento no corresponde a una etiqueta Git de versión.

**2) al publicar una etiqueta vX.Y.Z. ¿Qué tags de imagen se generan en cada caso y por qué?**

Cuando publicamos una etiquet vX.Y.Z, se ejecuta el workflow ya que coincide con el patrón `v*.*.*`, y se generarian las siguiente etiquetas, sha-`<commit>`, y las etiquetas de versión, `X.Y.Z` para la version completa, `X.Y` para la versión mayor y menor; y `X` para la versión mayor. Sin embargo, no se genera una etiqueta `latest`, ya que dependemos de `enable={{is_default_branch}}` que evalua si el evento ocurre en una rama predeterminada, y al publicar unicamente la etiqueta, este flag normalmente será falso.

**Comprueba que la imagen aparece en la pestaña Packages del repositorio, etiquetada con el SHA del commit y con latest.**

![Pestaña Package del repositorio, con el SHA del commit y la etiqueta latest](./boletin5-package-repositorio.png)

### Parte B - Publicar una versión de verdad

Para la publicación de una versión, en este caso, la primera versión de nuestra API de libros, usamos los siguientes comandos:

```bash
git tag -a v1.0.0 -m "Primera versión publicable de la API de tareas"
git push origin v1.0.0
```

Al hacer un push de la etiqueta de versión, vemos que salta el workflow porque se ha producido el evento de publicación de versión.

![Comprobación de la ejecucion del workflow de Release](./boletin5-workflow-release.png)

Y también vemos como la imagen tiene dicha etiqueta.

![Tag de version](./boletin5-tag-version.png)

Y finalmente, creamos un Release a partir de esa etiqueta de versión, en el que describimos con notas todos los cambios hechos en esa versión, y para este caso, hemos utilizado la opción "Genereate release notes".

![Release de la version inicial](./boletin5-release-version-inicial.png)

### Parte C - Verificar la imagen publicada

Para la comprobación de que la imagen publicada funciona correctamente, se realizo la descarga de la imagen y su posterior arranque, sin embargo, como nuestra imagen necesita una base de datos Postgres real, hacemos previamente una `network` para nuestros contenedor y una base de datos, todo ello en Docker, y finalmente descargamos la imagen de nuestro repositorio y lo arrancamos. Y podemos ver como mediante la peticion `curl http://localhost:8080/actuator/health` que nuestro servicio ha arrancado correctamente.

![Verificación de la imagen publicada](./boletin5-uso-imagen-publicada.png)

Finalmente, elliminamos todos los contenedores que creamos para la prueba de verificación, asi como la red que creamos.

![Eliminación de la base de datos y de la API](./boletin5-elimado-de-contenedores.png)

### Parte D - GitHub Pages de la documentación
Se ha creado un script y un workflow con Claude para crear y actualizar de manera dinámica, cada vez que se realiza un merge a main.

Este es el workflow que permite publicar todos los boletines en GitHub Pages:

```yml
name: Publicar memoria en GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Configurar Node
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Instalar marked
        run: npm install --no-save marked

      - name: Generar la web
        run: node docs/build-site.mjs

      - name: Configurar Pages
        uses: actions/configure-pages@v5

      - name: Subir artefacto
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs/site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Desplegar en GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

```

Este es el programa que permite convertir cada Markdown en HTML:
```js
#!/usr/bin/env node
// Genera una web estática, minimalista, a partir de los boletines de docs/.
//
// Estructura admitida (se pueden mezclar):
//   docs/boletin1/boletin1.md + imágenes     (una carpeta por boletín)
//   docs/boletin6.md                         (un .md suelto)
//
// Uso (desde la raíz del repositorio):
//   npm install marked        # solo la primera vez
//   node docs/build-site.mjs
//
// Salida en docs/site/:
//   boletin1/boletin1.html (+ imágenes copiadas), ..., index.html

import { readdir, readFile, writeFile, mkdir, cp, rm } from "node:fs/promises";
import { fileURLToPath } from "node:url";
import path from "node:path";
import { marked } from "marked";

const DOCS_DIR = path.dirname(fileURLToPath(import.meta.url));
const SITE_DIR = path.join(DOCS_DIR, "site");
const IGNORED_DIRS = new Set(["site", "node_modules"]);
const SITE_NAME = "Memoria de las prácticas";

const CSS = `
:root {
  --bg: #fcfcfb;
  --fg: #1c1c1e;
  --muted: #6b6b70;
  --line: #e6e6e3;
  --soft: #f3f3f0;
  --accent: #3b5bdb;
  --font: "Inter", ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  --mono: ui-monospace, "SF Mono", "JetBrains Mono", Menlo, Consolas, monospace;
}
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #131315;
    --fg: #e8e8ea;
    --muted: #9a9aa2;
    --line: #2a2a2e;
    --soft: #1b1b1e;
    --accent: #8ea2ff;
  }
}
* { box-sizing: border-box; }
html { -webkit-text-size-adjust: 100%; }
body {
  margin: 0;
  background: var(--bg);
  color: var(--fg);
  font: 17px/1.7 var(--font);
  -webkit-font-smoothing: antialiased;
}
a { color: var(--accent); text-decoration-thickness: 1px; text-underline-offset: 3px; }
a:hover { text-decoration-thickness: 2px; }

.wrap { max-width: 700px; margin: 0 auto; padding: 0 1.5rem; }
header.site { padding: 2rem 0 1.25rem; }
header.site a {
  color: var(--muted); text-decoration: none; font-size: .9rem; letter-spacing: .01em;
}
header.site a:hover { color: var(--fg); }
main { padding: 1.5rem 0 3rem; }
footer.site {
  border-top: 1px solid var(--line); padding: 1.5rem 0 3rem;
  color: var(--muted); font-size: .85rem;
}

/* Tipografía del contenido */
h1, h2, h3, h4 { line-height: 1.25; letter-spacing: -.02em; font-weight: 650; }
h1 { font-size: 2.25rem; margin: 0 0 1.5rem; letter-spacing: -.03em; }
h2 { font-size: 1.5rem; margin: 3rem 0 .75rem; }
h3 { font-size: 1.2rem; margin: 2.25rem 0 .5rem; }
h4 { font-size: 1rem; margin: 1.75rem 0 .5rem; }
p, ul, ol, pre, table, blockquote, figure { margin: 0 0 1.25rem; }
ul, ol { padding-left: 1.4rem; }
li + li { margin-top: .3rem; }
hr { border: 0; border-top: 1px solid var(--line); margin: 2.5rem 0; }
strong { font-weight: 600; }

/* Código */
code { font-family: var(--mono); font-size: .88em; }
:not(pre) > code {
  background: var(--soft); border: 1px solid var(--line);
  padding: .1em .4em; border-radius: 5px;
}
pre {
  background: var(--soft); border: 1px solid var(--line);
  padding: 1rem 1.15rem; border-radius: 10px; overflow-x: auto;
  font-size: .88rem; line-height: 1.6;
}
pre code { background: none; border: 0; padding: 0; font-size: inherit; }

/* Imágenes, citas, tablas */
img { display: block; max-width: 100%; height: auto; border-radius: 10px; margin: 1.5rem 0; }
blockquote {
  margin-left: 0; padding: .1rem 0 .1rem 1.1rem;
  border-left: 3px solid var(--line); color: var(--muted);
}
table { width: 100%; border-collapse: collapse; font-size: .95rem; display: block; overflow-x: auto; }
th, td { text-align: left; padding: .55rem .8rem; border-bottom: 1px solid var(--line); }
th { font-weight: 600; color: var(--muted); font-size: .8rem; text-transform: uppercase; letter-spacing: .04em; }

/* Índice */
.lead { color: var(--muted); margin: -.75rem 0 2.5rem; }
.toc { list-style: none; padding: 0; margin: 0; border-top: 1px solid var(--line); }
.toc li { margin: 0; }
.toc a {
  display: flex; align-items: baseline; gap: 1rem; padding: 1rem .25rem;
  border-bottom: 1px solid var(--line); color: var(--fg); text-decoration: none;
  transition: padding .15s ease, color .15s ease;
}
.toc a:hover { padding-left: .75rem; color: var(--accent); }
.toc .num { font: .85rem var(--mono); color: var(--muted); min-width: 2ch; }
.toc .arrow { margin-left: auto; color: var(--muted); }

/* Anterior / siguiente */
.pager { display: flex; justify-content: space-between; gap: 1rem; margin-top: 3.5rem; }
.pager a {
  flex: 1; display: block; padding: .9rem 1.1rem; border: 1px solid var(--line);
  border-radius: 10px; text-decoration: none; color: var(--fg); transition: border-color .15s;
}
.pager a:hover { border-color: var(--accent); }
.pager small { display: block; color: var(--muted); font-size: .75rem; letter-spacing: .04em; text-transform: uppercase; }
.pager .next { text-align: right; }

@media (max-width: 600px) {
  body { font-size: 16px; }
  h1 { font-size: 1.85rem; }
}
`;

const escapeHtml = (s) =>
  s.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");

function page({ title, body, rootHref }) {
  return `<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>${escapeHtml(title)}</title>
<style>${CSS}</style>
</head>
<body>
<div class="wrap">
  <header class="site">${rootHref ? `<a href="${rootHref}">← ${SITE_NAME}</a>` : "<a>&nbsp;</a>"}</header>
  <main>
${body}
  </main>
  <footer class="site">${SITE_NAME}</footer>
</div>
</body>
</html>
`;
}

// Título = primer encabezado "# ..." o, si no hay, el nombre del fichero.
function extractTitle(md, fallback) {
  const m = md.match(/^#\s+(.+)$/m);
  return m ? m[1].trim() : fallback;
}

// Los enlaces a otros .md pasan a apuntar a .html.
function rewriteMdLinks(html) {
  return html.replace(/href="(?!https?:|#|mailto:)([^"]+?)\.md(#[^"]*)?"/g, 'href="$1.html$2"');
}

const collator = new Intl.Collator("es", { numeric: true });
const isMd = (f) => f.toLowerCase().endsWith(".md");

// Devuelve [{ relDir, file }]: relDir "" para .md sueltos, o el nombre de la carpeta.
async function collectSources() {
  const items = await readdir(DOCS_DIR, { withFileTypes: true });
  const sources = [];
  for (const it of items) {
    if (it.isFile() && isMd(it.name)) {
      sources.push({ relDir: "", file: it.name });
    } else if (it.isDirectory() && !IGNORED_DIRS.has(it.name) && !it.name.startsWith(".")) {
      const inner = await readdir(path.join(DOCS_DIR, it.name));
      for (const f of inner.filter(isMd)) sources.push({ relDir: it.name, file: f });
    }
  }
  return sources.sort((a, b) =>
    collator.compare(path.join(a.relDir, a.file), path.join(b.relDir, b.file))
  );
}

// Enlace relativo desde la carpeta de una página a otra ruta del sitio.
const hrefFrom = (fromDir, toRel) => encodeURI(path.posix.relative(fromDir || ".", toRel));

async function main() {
  const sources = await collectSources();
  if (sources.length === 0) {
    console.error("No se encontraron ficheros .md en", DOCS_DIR);
    process.exit(1);
  }

  // Salida limpia en cada ejecución
  await rm(SITE_DIR, { recursive: true, force: true });
  await mkdir(SITE_DIR, { recursive: true });

  // Copiar cada carpeta de boletín (imágenes y demás) sin los .md
  const dirs = [...new Set(sources.map((s) => s.relDir).filter(Boolean))];
  for (const dir of dirs) {
    await cp(path.join(DOCS_DIR, dir), path.join(SITE_DIR, dir), {
      recursive: true,
      filter: (src) => !isMd(src),
    });
  }

  // Primera pasada: leer y convertir todo
  const entries = [];
  for (const { relDir, file } of sources) {
    const name = path.basename(file, path.extname(file));
    const md = await readFile(path.join(DOCS_DIR, relDir, file), "utf8");
    entries.push({
      relDir,
      file,
      title: extractTitle(md, name),
      outRel: path.posix.join(relDir, `${name}.html`),
      html: rewriteMdLinks(await marked.parse(md)),
    });
  }

  // Segunda pasada: escribir con navegación anterior / siguiente
  for (const [i, e] of entries.entries()) {
    const prev = entries[i - 1];
    const next = entries[i + 1];
    const pager =
      prev || next
        ? `<nav class="pager">
  ${prev ? `<a class="prev" href="${hrefFrom(e.relDir, prev.outRel)}"><small>← Anterior</small>${escapeHtml(prev.title)}</a>` : "<span></span>"}
  ${next ? `<a class="next" href="${hrefFrom(e.relDir, next.outRel)}"><small>Siguiente →</small>${escapeHtml(next.title)}</a>` : "<span></span>"}
</nav>`
        : "";

    await mkdir(path.join(SITE_DIR, e.relDir), { recursive: true });
    await writeFile(
      path.join(SITE_DIR, e.outRel),
      page({
        title: `${e.title} · ${SITE_NAME}`,
        body: `<article>\n${e.html}\n</article>\n${pager}`,
        rootHref: hrefFrom(e.relDir, "index.html"),
      })
    );
    console.log(`✔ ${path.join(e.relDir, e.file)} → site/${e.outRel}`);
  }

  // Índice
  const list = entries
    .map(
      (e, i) =>
        `    <li><a href="${encodeURI(e.outRel)}"><span class="num">${String(i + 1).padStart(2, "0")}</span><span>${escapeHtml(e.title)}</span><span class="arrow">→</span></a></li>`
    )
    .join("\n");
  await writeFile(
    path.join(SITE_DIR, "index.html"),
    page({
      title: SITE_NAME,
      body: `<h1>${SITE_NAME}</h1>\n<p class="lead">Una entrada por sesión.</p>\n<ol class="toc">\n${list}\n</ol>`,
      rootHref: null,
    })
  );
  console.log("✔ site/index.html");
}

main().catch((err) => {
  console.error(err);
  process.exit(1);
});

```

Y este es el resultado final:

![](./boletin5-parted.png)