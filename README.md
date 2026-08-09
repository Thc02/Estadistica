# Estadística · Ciencias Navales

Material del curso, escrito en [Quarto](https://quarto.org) con celdas de R
ejecutables en el navegador ([quarto-live](https://r-wasm.github.io/quarto-live/)).

Sitio publicado: <https://thc02.github.io/Estadistica/>

## Qué archivo tocar

| Archivo | Para qué |
|---|---|
| `index.qmd` | Portada del curso y contenido general |
| `unidad-1/tema-1-1.qmd` | Contenido del Tema 1.1 |
| `_quarto.yml` | Navegación: barra lateral, menú superior, pie de página |
| `estilo.scss` | Colores, tipografías y los bloques con estilo propio |

**Nunca editar nada dentro de `_site/`.** Es la salida renderizada: se regenera
entera en cada `render` y cualquier cambio hecho ahí se pierde.

## Rutina de trabajo

```powershell
cd "C:\Users\User\OneDrive\Documentos\curso-estadistica"

# 1. Vista previa mientras editas (se recarga sola al guardar)
& "C:\Program Files\RStudio\resources\app\bin\quarto\bin\quarto.exe" preview

# 2. Guardar los cambios en el historial
git add .
git commit -m "Descripción del cambio"
git push

# 3. Publicar el sitio actualizado
& "C:\Program Files\RStudio\resources\app\bin\quarto\bin\quarto.exe" publish gh-pages
```

El `&` inicial es obligatorio en PowerShell: sin él, una ruta entre comillas se
interpreta como texto y no como comando.

Los pasos 2 y 3 son distintos y ambos hacen falta. El **2** guarda el código fuente
en la rama `main`; el **3** renderiza y publica el sitio en la rama `gh-pages`. Si
sólo se hace el 3, el repositorio queda desactualizado respecto de lo publicado.

## Bloques de formato disponibles

Además del markdown normal, este material usa clases definidas en `estilo.scss`:

```markdown
::: {.tarjeta}
#### Título de la tarjeta
Texto sobre fondo claro.
:::

::: {.tarjeta-oscura}
#### Título
Texto sobre fondo azul marino, para bloques de énfasis.
:::

::: {.franja}
Síntesis o advertencia, en una franja azul de ancho completo.
:::

::: {.predice}
Pregunta que el guardiamarina debe responder **antes** de ejecutar la celda.
Aparece con el rótulo "Antes de ejecutar".
:::

[Texto en cursiva discreta, para los ejemplos navales.]{.ejemplo}
```

Para poner tarjetas en columnas se usa la retícula de Quarto:

```markdown
::: {.grid}
::: {.g-col-6}
  ... primera columna ...
:::
::: {.g-col-6}
  ... segunda columna ...
:::
:::
```

Los números de `g-col-*` suman 12: dos columnas son `g-col-6`, tres son `g-col-4`.

## Celdas de R ejecutables

Una celda normal, que el guardiamarina puede editar y ejecutar:

````markdown
```{webr}
mean(flota$nudos)
```
````

Un ejercicio con huecos, pista y solución:

````markdown
```{webr}
#| exercise: nombre_del_ejercicio
resultado <- ______
```

::: {.hint exercise="nombre_del_ejercicio"}
La pista que se revela al pulsar el botón.
:::

::: {.solution exercise="nombre_del_ejercicio"}
```r
resultado <- mean(flota$calado)
```
:::
````

Los huecos se marcan con **seis guiones bajos o más**. El bloque oculto al principio
de `tema-1-1.qmd` define el objeto `flota`, disponible en todas las celdas de la página.

Conviene usar **R base** y evitar paquetes externos: cada paquete se descarga en el
navegador del guardiamarina y alarga la primera carga.

## Limitaciones conocidas

- **Rutas con tildes.** El filtro de `quarto-live` falla si la ruta del proyecto
  contiene acentos. Por eso el proyecto vive en `Documentos\curso-estadistica` y no
  dentro de `Probabilidad y Estadística`.
- **No funciona con `file://`.** Abrir el HTML desde el disco muestra el aviso
  *"The OJS runtime does not work with file:// URLs"*. Hay que servirlo: `quarto preview`
  en local, GitHub Pages para los guardiamarinas.
- El servidor sólo entrega archivos. **R se ejecuta en el navegador del guardiamarina**,
  así que hace falta conexión para cargar la página, no para calcular.
