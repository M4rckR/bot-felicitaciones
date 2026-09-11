# Tarjetín — bot de `/felicitaciones` (TC0091)

Bot conversacional de árbol de decisión que se inserta en la página **post-aprobación** del BCP
(`/felicitaciones`), donde el cliente ya fue aprobado y tiene que elegir una de sus tarjetas.

Se entrega como **oferta HTML de Adobe Target**: un único archivo que se pega entero en la herramienta.
No hay build, ni gestor de paquetes, ni tests.

---

## El experimento tiene dos variantes

Un A/B test necesita los dos lados, y **son dos archivos distintos que se pegan en dos ofertas distintas**:

| | Archivo | Qué es | Tageo que emite |
| --- | --- | --- | --- |
| **Piloto** | `adobe-target/piloto/bot.html` | El bot completo | Los 7 eventos ` - P` |
| **Control** | `adobe-target/control/control.html` | Sin bot. Solo el tag | 1 evento ` - C` |

El control no pinta nada: es un `<script>` que empuja un único `trackPromotionView` a `digitalData`. Existe
porque en el grupo de control **el bot no se inyecta**, así que ninguno de los siete eventos del piloto
puede dispararse ahí, y sin un evento común las dos ramas no se pueden comparar.

**No mezclarlos.** El control no lleva nada del bot, y al piloto no se le agrega el tag ` - C`.

---

## Ver el preview

### Opción 1 — Netlify (para mirarlo desde el celular)

El repo ya trae `netlify.toml` configurado. Netlify lo detecta solo: **no hay que elegir framework ni
escribir comandos**. Conectá el repositorio y desplegá; la raíz del sitio abre el preview.

### Opción 2 — local

```bash
# desde la raíz del proyecto
python3 -m http.server 8000 --bind 127.0.0.1
```

Y abrir **http://localhost:8000/preview/**

> Tiene que ser por servidor. Abrir el archivo con doble clic (`file://`) no funciona: el navegador bloquea
> el `fetch` con el que el preview lee la oferta. El propio preview lo detecta y te avisa.

**Para verlo en el celular estando en la misma red**, levantá el servidor sin restringir la interfaz y
entrá desde el teléfono a la IP de tu máquina:

```bash
python3 -m http.server 8000          # sin --bind
ipconfig getifaddr en0               # te da la IP, p. ej. 192.168.1.40
# en el celular: http://192.168.1.40:8000/preview/
```

### Qué hace el preview

- **Lee la oferta del archivo en cada carga** y la reinyecta sola cuando cambia (revisa cada 1 s). Nunca
  copia el contenido, así que el preview y la oferta no pueden quedar desincronizados.
- **Panel de casuística**: simula qué tarjetas tiene aprobadas el cliente, fabricando el mismo DOM que
  publica la página real. Así se ejercita la detección de verdad, no una versión de mentira.
- **Panel `digitalData`**: muestra cada evento de analítica en vivo, de los dos orígenes. Los del **bot**
  van en gris; los de la **página** —los tres que dispara `/felicitaciones` sola al elegir una tarjeta— van
  marcados con una franja ámbar y la etiqueta `página`. El bot no los emite: se replican para que se vea la
  secuencia completa que recibirá analítica.
- **Panel de consola**: ahí aparecen los errores de `validateGraph`.
- **Toggle Desktop / Móvil 390px** en pantalla ancha. En un celular no aparece, porque el ancho ya es real.
- **La barra de controles no ocupa alto.** Está oculta: el bot se queda con la pantalla entera. Para
  sacarla, acercá el mouse al borde de arriba, tocá el **≡** de la esquina superior izquierda o apretá
  **H**. El puntito del ≡ destella cada vez que el bot se recarga solo.
- **El panel lateral se pliega** con **☰ panel**, tanto en pantalla ancha como angosta. Con el panel
  plegado y la barra escondida, lo único que se ve es el bot.

Se renderiza sobre un fondo neutro con solo los design tokens que `/felicitaciones` define de verdad. **La
página real nunca se carga**: eso dispararía los píxeles de tracking de BCP, Adobe y Meta.

---

## El directorio

```
adobe-target/      Lo que se pega en Adobe Target. Nada más que esto sale a producción.
  piloto/bot.html
  control/control.html
preview/           El harness de desarrollo. Es lo único que publica Netlify.
contenido/         Material de trabajo. El bot NUNCA lee estos archivos.
  wordings/          Espejo de todos los textos, uno por flujo.
  tarjetas-catalogo.json   Cruce de las 17 tarjetas: código, nombre, afinidad, casuística.
  afinidad-tarjetas.json   Tabla de afinidad tal como la entregó Marco.
docs/              correcciones-wording.md — entregable para el equipo de UI.
referencia/        Solo lectura. Evidencia, no se edita y NO se publica.
CLAUDE.md          Documentación técnica completa.
```

### ⚠️ `referencia/paginas/` no se publica nunca

Esos cuatro snapshots son capturas reales de la página y **contienen datos de clientes reales**: nombre
completo y línea de crédito aprobada. Están deliberadamente fuera de lo que Netlify sube (ver el
comentario en `netlify.toml`). No agregarlos al deploy, ni siquiera para probar algo puntual.

---

## Dónde mirar según la tarea

| Si vas a… | Abrí |
| --- | --- |
| cambiar un texto | `contenido/wordings/_index.json` para ubicar el flujo, después el archivo del flujo **y** la oferta |
| tocar datos de tarjetas | `contenido/tarjetas-catalogo.json` |
| entender el motor, el tageo o el estado del proyecto | `CLAUDE.md` |
| presentarle correcciones al equipo de UI | `docs/correcciones-wording.md` |

**Los archivos de `contenido/wordings/` son un espejo, no una fuente.** El bot no los carga nunca: un
cambio que solo existe ahí no cambia nada. Siempre hay que aplicarlo también en la oferta.
