# contenido/wordings/

Espejo editable de toda la copy de `adobe-target/piloto/bot.html`, partido por flujo.

> **`adobe-target/piloto/bot.html` NUNCA lee estos archivos. Ni en producción, ni en el preview local, ni nunca.**
> No agregar un `fetch`, un `import`, ni ninguna otra forma de cargarlos desde el snippet.

Son un artefacto de desarrollo puro: material de referencia para trabajar la copy sin tener que abrir el
HTML. El snippet se sigue pegando entero como oferta en Adobe Target y sigue siendo autocontenido —
`config.nodes` con la copy en duro adentro es y sigue siendo lo que se ejecuta.

## Para qué existe

1. **Economía de contexto.** Las cuatro pantallas de comparación (`q21`–`q24`) son tablas de 17 filas
   incrustadas como arrays de strings dentro de un archivo de ~3.900 líneas. Abrir la copy de una pantalla
   obligaba a arrastrar las otras tres. Con un archivo por pantalla se carga solo la que se va a tocar.
2. **Control de cambios.** Un ajuste de wording se ve como un diff de una línea en un JSON, no como un
   bloque de JS reindentado.
3. **Revisión sin leer código.** Marco puede validar la copy sin abrir el HTML.

## Contrato: quién manda

**`adobe-target/piloto/bot.html` es la fuente de verdad.** Estos JSON son un espejo. El flujo correcto es:

```
editar contenido/wordings/*.json  →  aplicar el cambio a adobe-target/piloto/bot.html  →  verificarlo en preview/index.html
```

Los tres pasos, siempre.

> **Un cambio que solo existe en `wordings/*.json` no es un cambio.** Nadie carga estos archivos: editarlos
> deja el bot byte a byte igual. Si el paso 2 no se hizo, no se hizo nada — y no hay build, ni tests, ni
> script de sincronización que lo detecte.

Y no se da por terminado hasta **verlo renderizado** en el preview, no hasta que el diff se vea bien. En
este proyecto ya hubo bugs invisibles en el código y obvios en pantalla: pipes que se renderizaban
literales, una barra de scroll horizontal, una animación que escalaba la fila entera en vez de la burbuja.

Si se edita el HTML directo, hay que actualizar el JSON en el mismo paso. Si divergen, gana el HTML.

## Cómo volcar un cambio al HTML

- **`texto.intro` y `texto.parrafos`** → entran como strings del array `text: [...]`, que se une con
  `.join("\n")`. Una línea vacía `""` entre bloques es un separador (produce espacio, no un `<br>`; el
  motor no emite `<br>` en ninguna parte).
- **`texto.tabla`** → cada fila se serializa como `"| celda | celda |"`. Después de la fila de encabezado
  va siempre la fila separadora `"| --- | --- |"`. El nodo necesita `richText: true` para que
  `formatRichText` la parsee, y `tc0091-node-ancho` en el `className` para que la burbuja ocupe el 100 %
  del panel.
- **`texto.notaTabla`** → la llamada al pie de la tabla. **No va en el array `text`**: vive en
  `config.comparador.<tabla>.nota` y el motor la pinta aparte, en un `<p class="tc0091-nota">` a 12/18,
  entre la tabla y el recuadro "Te recomendamos esta tarjeta". Solo se pinta **si alguna fila visible
  lleva `*`** en su valor; filtradas por lead, hay usuarios que no tienen ninguna de esas tarjetas y la
  nota quedaría sin nada a que referirse.
- **Emojis** → se guardan como entidad HTML (`&#128179;`), igual que en el snippet. Marco los manda como
  emoji literal; la conversión es parte del volcado, no del JSON.
- **`lineas`** → es una pista para saltar directo en `adobe-target/piloto/bot.html`. **Se desactualiza en cuanto se
  agregan nodos.** Antes de editar, confirmar con `grep -n 'id: "q22"' adobe-target/piloto/bot.html`.

## Esquema

Cada archivo de flujo:

| Campo | Significado |
| --- | --- |
| `flujo` | Nombre del flujo tal como lo nombra Marco (1.1, 2.2, …) |
| `estado` | `completo` · `parcial` · `pendiente` |
| `nodos[]` | Un objeto por nodo del grafo |
| `nodos[].id` | Id en `config.nodes`. Es la llave de correspondencia con el HTML |
| `nodos[].lineas` | Rango en `adobe-target/piloto/bot.html` (pista, ver arriba) |
| `nodos[].tipo` | `question` · `auto` · `mixed` · `rating` |
| `nodos[].titulo` | Campo `title` del nodo. `""` cuando la pantalla no lleva título |
| `nodos[].texto` | La copy, desestructurada en `intro` / `parrafos` / `tabla` / `notaTabla` |
| `nodos[].recomendacion` | Bloque "Te recomendamos esta tarjeta", si existe |
| `nodos[].menuText` | Copy entre la tarjeta recomendada y los botones |
| `nodos[].opciones[]` | `label` + destino (`next`, o `href`/`isClose`) |
| `nodos[].pendientes[]` | Qué falta o está sin confirmar en esta pantalla |

`ui-fija.json` no sigue este esquema: es la copy que **no** vive en `config.nodes` (markup del launcher y
la cabecera, y strings hardcodeados en el motor).

## Archivos

| Archivo | Flujo | Estado |
| --- | --- | --- |
| `_index.json` | Mapa de todos los nodos y en qué archivo está cada uno | — |
| `ui-fija.json` | Launcher, cabecera, errores, rating, aria-labels | completo |
| `flujo-0-inicio.json` | `q0` menú raíz | completo |
| `flujo-1-elegir.json` | `q1` + `q11`–`q14` (Marco: 1.1–1.4) | 4 stubs pendientes |
| `flujo-2-comparar.json` | `q2` menú de comparación | completo |
| `flujo-2.1-millas.json` | `q21` acumulación de millas | falta tarjeta recomendada |
| `flujo-2.2-membresia.json` | `q22` membresía anual | completo (referencia) |
| `flujo-2.3-exoneracion.json` | `q23` exoneración de membresía | completo, con 2 dudas de datos |
| `flujo-2.4-priority-pass.json` | `q24` Priority Pass | intro por aprobar, sin recomendación |
| `flujo-3-dudas.json` | `q3` + `q31`–`q35` | 5 stubs pendientes |
| `flujo-cierre.json` | `mixed1`, `rating*`, `mixed100` | completo, ruta de entrada asumida |
