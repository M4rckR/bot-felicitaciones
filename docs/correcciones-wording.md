# Correcciones y decisiones — Tarjetín, bot de /felicitaciones

**Última actualización:** 2026-09-11

---

## Cómo usar este documento

Este archivo está escrito para que **una IA pueda leerlo sin ningún otro contexto y explicárselo al equipo
de UI**. No hace falta abrir el código ni los mockups.

Si sos una IA y te pidieron presentar esto:

- Todo lo que necesitás está acá. No inventes cambios que no figuren en estas tablas.
- Cada corrección trae **qué decía, qué dice ahora, y la regla que se aplicó**. Usá la regla para justificar,
  no el gusto personal.
- Separá siempre **correcciones** (había un error) de **decisiones** (no había error, se eligió una opción).
  Es la confusión más probable en la reunión.
- Si UI pregunta por algo que está en "Temas abiertos", la respuesta correcta es *"no está decidido, lo
  necesitamos de ustedes"*, no una opinión.

---

## Contexto mínimo

**Tarjetín** es un bot de ayuda que se inserta en la página `/felicitaciones` del BCP. Esa página es el paso
**posterior a la aprobación**: el usuario ya fue aprobado y tiene que elegir cuál de sus tarjetas
aprobadas quiere. El bot responde "¿cuál de mis tarjetas elijo y qué significan estas condiciones?".

Tiene tres caminos desde el menú inicial:

1. **Ayúdame a elegir una tarjeta** — perfila al usuario por lo que quiere hacer con la tarjeta.
2. **Quiero comparar mis tarjetas** — tablas comparativas (millas, membresía, exoneración, Priority Pass).
3. **Resolver dudas para elegir** — preguntas frecuentes.

**Quién escribe qué:** el contenido y los textos los entrega el equipo de negocio/UX pantalla por pantalla,
con mockups. El equipo de desarrollo los implementa **tal cual**, sin reescribir. Cuando aparece un error
de redacción, se marca y se consulta; solo se corrige con aprobación explícita. Este documento es el
registro de esas consultas y sus respuestas.

---

## El criterio de corrección

Acordado el 2026-09-09:

> **Corregir solo lo que se quiso comunicar y las palabras mal formateadas. No reescribir, no mejorar el
> estilo, no inventar.**

En la práctica esto significa:

| Sí se corrige | No se toca |
| --- | --- |
| Concordancia de género y número | Tono, largo o ritmo de la frase |
| Tildes faltantes | Elección de palabras |
| Signos de puntuación faltantes o duplicados | Orden de las ideas |
| Erratas de tipeo evidentes | Datos de negocio (montos, condiciones) |

Cada corrección de la lista siguiente toca lo mínimo posible: en ningún caso se reescribió una frase entera.

---

## Parte 1 — Correcciones de redacción

**12 correcciones sobre 10 pantallas.** Todas aprobadas. Son de redacción la 1-6, la 8-10 y la 12; la 7 y la 11 son unificaciones.

### 1. Inicio — concordancia de número

| | |
| --- | --- |
| **Decía** | Te ayudaré a elegir una **tarjetas** de crédito. |
| **Dice** | Te ayudaré a elegir una **tarjeta** de crédito. |
| **Regla** | El determinante "una" es singular y exige un sustantivo singular. |

*Si UI pregunta:* era un error de tipeo, no un cambio de sentido. La frase sigue diciendo exactamente lo
mismo.

### 2. Inicio (botón) — tilde faltante

| | |
| --- | --- |
| **Decía** | **Ayudame** a elegir una tarjeta |
| **Dice** | **Ayúdame** a elegir una tarjeta |
| **Regla** | "Ayúdame" es esdrújula (verbo + pronombre enclítico). Las esdrújulas llevan tilde siempre. |

*Si UI pregunta:* sin tilde la palabra no existe en español. No es una preferencia de estilo.

### 3. Ayúdame a elegir una tarjeta — concordancia sujeto-verbo

| | |
| --- | --- |
| **Decía** | …te recomendaré algunas opciones que mejor **se adapte** a ti. |
| **Dice** | …te recomendaré algunas opciones que mejor **se adapten** a ti. |
| **Regla** | El sujeto es "opciones" (plural), el verbo tiene que ir en plural. |

*Contexto que conviene mencionar:* esta frase venía de un cambio previo. Antes decía "te recomendaré **solo
la opción** que mejor se adapte a ti" y se cambió a "**algunas opciones**". Al pasar a plural, el verbo se
quedó en singular. Es un residuo de esa edición.

**Ojo, esto tiene consecuencia de producto:** la frase ahora promete **varias** tarjetas, no una. La primera
de las cuatro pantallas que siguen a este menú ya está hecha y cumple la promesa: muestra hasta 4 tarjetas.
Las otras tres todavía no tienen contenido, y cuando se escriban tienen que recomendar más de una.

### 4. Acumulación de millas — concordancia de género

| | |
| --- | --- |
| **Decía** | **Estos** son las millas por consumo mensual. |
| **Dice** | **Estas** son las millas por consumo mensual. |
| **Regla** | "Millas" es femenino, el demostrativo tiene que concordar. |

### 5. Exoneración de membresía — dos preposiciones seguidas

| | |
| --- | --- |
| **Decía** | Estos son los montos **de para** la exoneración de membresía… |
| **Dice** | Estos son los montos **para** la exoneración de membresía… |
| **Regla** | "De para" no es una construcción válida. Sobraba una de las dos. |

**Esta es la única corrección donde hubo que elegir**, así que conviene explicarla bien:

- **Se eligió dejar "para"** → *"los montos **para** la exoneración"*. Los números de esa tabla son el
  consumo mensual que hay que alcanzar **para conseguir** la exoneración. "Para" es la preposición que
  carga ese significado.
- **La alternativa era dejar "de"** → *"los montos **de** la exoneración"*. Calcaría la estructura de la
  pantalla hermana ("los montos **de** membresía anual"), pero deja tres "de" en la misma oración: *"los
  montos de la exoneración de membresía de tus tarjetas"*.

Si UI prefiere la segunda por consistencia entre pantallas, es un cambio de una palabra.

### 6. Cambio de línea de crédito — signo de apertura faltante

| | |
| --- | --- |
| **Decía** | **Sí!** 😊 Si la línea de crédito ofrecida es mayor… |
| **Dice** | **¡Sí!** 😊 Si la línea de crédito ofrecida es mayor… |
| **Regla** | En español los signos de exclamación se abren y se cierran. |

*El motivo real fue la inconsistencia:* la otra pantalla de dudas ("¿Es seguro solicitarla por aquí?")
**sí** abría con "¡Sí!". Dos pantallas hermanas empezaban distinto.

> ⚠️ **Cuidado con esta en la reunión.** El saludo de la primera pantalla dice **"Hola!"** sin signo de
> apertura, y eso **se dejó así a propósito** (ver Parte 3). A primera vista parecen contradictorias. La
> diferencia es que el saludo se revisó y se decidió mantenerlo; este "Sí!" se revisó y se decidió
> corregirlo.

### 7. Las cuatro pantallas de comparar — unificación del botón de cierre

| | |
| --- | --- |
| **Decía** | Botón **"✅ Elegir una tarjeta"** |
| **Dice** | Botón **"Cerrar"** |
| **Motivo** | No es un error de redacción: es una unificación. |

Las cuatro pantallas de comparación (millas, membresía, exoneración y Priority Pass) terminaban con
"↩️ Volver a las alternativas" + "✅ Elegir una tarjeta". El mockup nuevo de Acumulación de millas cambió
ese segundo botón por **"Cerrar"**, y se decidió aplicarlo a las cuatro para que no queden desparejas.

Con esto **todo el bot cierra igual**: las cuatro de comparar y las tres de Resolver dudas terminan con
"↩️ Volver a las alternativas" + "Cerrar".

> ⚠️ **Consecuencia que hay que decidir.** "✅ Elegir una tarjeta" era el **único** camino que llevaba a la
> encuesta de satisfacción (las 5 estrellas y el "¡Gracias por tu calificación!"). Al reemplazarlo por
> "Cerrar", esa encuesta **quedó sin forma de aparecer**: no hay ningún recorrido que llegue a ella.
>
> Las pantallas siguen construidas y funcionando, solo que nada las llama. Hay que decidir una de dos:
> definir desde dónde se entra a la encuesta, o confirmar que se elimina. **Mientras tanto, las métricas de
> calificación no van a registrar nada.**

### 8. Viajar y acumular millas — errata en el nombre de una tarjeta

| | |
| --- | --- |
| **Decía** | Visa Infinite **Shapphire** LATAM Pass |
| **Dice** | Visa Infinite **Sapphire** LATAM Pass |
| **Regla** | Es el nombre comercial de la tarjeta y se escribe "Sapphire". La tabla de afinidad de esa misma tarjeta ya lo escribía bien. |

> ⚠️ **El mockup también dice "Shapphire".** Si UI compara la pantalla con el Figma va a ver la
> diferencia. Está corregido a propósito.

### 9. Viajar y acumular millas — concordancia sujeto-verbo

| | |
| --- | --- |
| **Decía** | Ideal para viajeros frecuentes que **quiere** beneficios premium. |
| **Dice** | Ideal para viajeros frecuentes que **quieren** beneficios premium. |
| **Regla** | El sujeto es "viajeros", plural. |

### 10. Viajar y acumular millas — mayúscula en un nombre propio

| | |
| --- | --- |
| **Decía** | Visa **infinite** Qore |
| **Dice** | Visa **Infinite** Qore |
| **Regla** | "Infinite" es parte del nombre de la tarjeta y en el resto del bot va con mayúscula. |

### 11. Todo el bot — la marca se escribe "American Express"

| | |
| --- | --- |
| **Decía** | **AMEX** Clásica LATAM Pass, **AMEX** Oro, **AMEX** Platinum, **AMEX** Black |
| **Dijo un tiempo** | **Amex** Clásica LATAM Pass, **Amex** Oro, **Amex** Platinum, **Amex** Black |
| **Dice** | **American Express** Clásica LATAM Pass, **American Express** Oro, **American Express** Platinum, **American Express** Black |
| **Motivo** | No es una errata: es una unificación. La marca va con su nombre completo en todo el bot, sin abreviar. |

Esto pasó en dos tiempos. La pantalla 1.1 llegó con las dos abreviaturas mezcladas ("Amex Black" y "AMEX
Platinum" en la misma lista); el **2026-09-09** se eligió **"Amex"** y se aplicó también a las cuatro tablas
de comparar, que decían "AMEX". El **2026-09-11** Marco pidió el **nombre completo, "American Express"**, y
se reemplazaron todas: ninguna pantalla del bot dice ya "Amex" ni "AMEX".

Son **33 textos visibles**: las 16 filas de las cuatro tablas de comparar, los 16 nombres de tarjeta del
perfilador (1.1–1.4) y una viñeta de la American Express Oro LATAM Pass que decía "beneficios AMEX".

> **Conviene mirarlo en pantalla.** El nombre completo es ~10 caracteres más largo que "Amex". En las tablas
> de comparar, que van a 13px dentro de un panel de 374px, "American Express Platinum LATAM Pass" es ahora la
> fila más larga del bot y es probable que ocupe tres líneas.

**Se aplicó el mismo criterio a dos grafías más, y esto sí conviene confirmarlo:** los textos de las
pantallas 1.2, 1.3 y 1.4 escriben en algunos lugares **"Latam"** en vez de "LATAM", y **"Shapphire"** en vez
de "Sapphire". Los nombres de tarjeta se dejaron con la grafía única que ya usa el resto del bot. Si alguna
de esas formas era intencional, hay que decirlo.

### 12. Las cuatro tablas de comparar — se completa el "Pass" que faltaba

| | |
| --- | --- |
| **Decía** | American Express Black **LATAM** |
| **Dice** | American Express Black **LATAM Pass** |
| **Regla** | El nombre completo del producto lleva "Pass". Era la única fila de las cuatro tablas a la que le faltaba. |

Esta tarjeta se llamaba distinto según la pantalla: en el perfilador (1.1) *"American Express Black LATAM
**Pass**"* y en las cuatro tablas de comparar *"American Express Black LATAM"*, sin el final. Todas las demás
filas LATAM Pass de esas tablas sí lo llevaban. Se completó el 2026-09-11 y **los dos nombres ya coinciden**.

Son 4 filas, una por tabla. Las tarjetas Qore, la Visa Light y la Visa Clásica siguen sin "LATAM Pass"
porque no son de ese programa: eso no es una falta, es el nombre correcto.

*Efecto lateral que vale la pena saber:* con la marca completa y el "Pass" puesto, **los nombres que muestra
el bot ahora coinciden letra por letra con los que imprime la propia página** en las tarjetas de las que
tenemos captura. Antes no era así.

### 13. Exoneración de membresía — las dos filas Qore que repetían la membresía

| | |
| --- | --- |
| **Decía** | Visa Clásica Qore **S/80** · Visa Oro Qore **S/170** |
| **Dice** | Visa Clásica Qore **S/1** · Visa Oro Qore **S/1** |
| **Regla** | El monto de exoneración lo fija el catálogo público de producto, que para estas dos tarjetas dice *"Consume S/ 1 al mes y no pagues membresía"*. |

Este era el primer punto de "Datos que parecen inconsistentes" y estuvo abierto desde el 2026-09-09. Las
dos cifras no eran montos de exoneración: **eran sus propios montos de membresía anual**, copiados de la
pantalla de al lado. Las demás tarjetas Qore sí usaban la escala correcta (S/1,200 / S/3,500 / S/5,000).

Se corrigió el **2026-09-21**, con el catálogo público de tarjetas de viabcp.com —*"Elige la Tarjeta de
Crédito que va contigo"*— como fuente. Ahí las dos fichas dicen lo mismo: se exoneran consumiendo **S/1 al
mes**, igual que la Visa Clásica LATAM Pass, la Visa Oro LATAM Pass y las American Express Clásica y Oro.

**El bot ya se contradecía a sí mismo**, y esto lo resuelve: las pantallas del perfilador que presentan
estas dos tarjetas siempre dijeron *"Exoneración consumiendo S/1 al mes"*. Quien entraba por el perfilador
leía S/1 y quien entraba por la tabla de comparar leía S/80 o S/170, para la misma tarjeta.

**La membresía anual no cambia:** la Visa Clásica Qore sigue costando S/80 al año y la Visa Oro Qore S/170.
Lo que se corrigió es cuánto hay que consumir al mes para no pagarla.

### 14. Ahorrar en costos — membresía y exoneración pasan a una sola línea

| | |
| --- | --- |
| **Decía** | • Membresía anual de S/80.<br>• Exoneración consumiendo S/1 al mes. |
| **Dice** | • Membresía anual S/80 **(GRATIS si consumes S/1 al mes).** |
| **Regla** | Un solo dato, una sola viñeta, con el formato del catálogo público de BCP. Es el mismo estilo de la ficha nueva de la tarjeta destacada. |

**Pedido por Marco el 2026-09-21**, al ver que la ficha nueva de la Visa Oro Qore usaba un formato y el
resto otro. Son **17 fichas**, todas de la pantalla *Ahorrar en costos (1.2)* salvo la destacada: ninguna
otra pantalla menciona la membresía.

De paso se unificaron tres cosas que estaban a medias entre esas fichas:

| Antes | Cuántas | Ahora |
| --- | --- | --- |
| "Membresía anual **de** S/80." | 10 | "Membresía anual S/80…" |
| "Membresía anual**:** S/350." | 5 | igual, sin los dos puntos |
| "Exoneración**:** consumiendo…" | 1 | "…(GRATIS si consumes…)" |
| Sin punto final | 4 | con punto |

Una de las 17 (Visa Platinum Qore) ya tenía los dos datos en una línea, separados por un punto; se le puso
el mismo formato que a las demás.

En esa unificación no se cambió ninguna cifra. Posteriormente, el punto 6 de la revisión contra Figma
corrigió `S/5,00` a `S/5,000` para Visa Infinite Qore, respaldado también por el catálogo público. Queda
pendiente la cifra de la Visa Clásica sin código, que no se muestra hoy.

### 15. Ahorrar en costos — seis diferencias claras contra Figma

Aplicado el **2026-09-21** por indicación de Marco:

| Tarjeta | Cambio aplicado |
| --- | --- |
| Visa Light | Se añadió el punto final a «Membresía 0 sin consumo mínimo.» |
| Visa Clásica LATAM Pass | «Acceso a descuentos y promociones BCP.» |
| Visa Clásica Qore | «Acceso a descuentos y promociones BCP.» |
| Visa Oro LATAM Pass | «Ideal para viajeros frecuentes que buscan acumular más millas con facilidad.» |
| Visa Infinite Qore | Se corrigió `S/5,00` a `S/5,000` al mes. |
| Visa Infinite Sapphire LATAM Pass | La viñeta de membresía termina con `.*`, igual que Figma. |

**Regla de fuentes:** para wording se siguió Figma. Para cifras y beneficios manda el catálogo público de
`viabcp.com`; por eso American Express Oro conserva S/170 aunque una ficha aislada del Figma diga S/80.

### 16. Obtener más beneficios — alineación de los casos A y B

Aplicado el **2026-09-21**: se actualizaron las 14 descripciones del caso A y las cuatro fichas del caso B
según Figma. Visa Oro LATAM Pass recuperó la ficha completa; Visa Clásica y Visa Light ahora separan su
descripción de las viñetas; se corrigió «Acumua» a «Acumula» y se quitó el asterisco de acumulación de
Iridium. La errata «Ideal para acumula…» se corrigió después en la corrección 18.

### 17. Burbuja del launcher — nuevo mensaje

| | |
| --- | --- |
| **Decía** | ¡Hola! Soy Tarjetín. 👋 ¿En qué te puedo ayudar? |
| **Dice** | ¡Hola! Te ayudaré a elegir una tarjeta |
| **Regla** | Wording literal del Figma de `/felicitaciones`. No modifica el saludo conversacional de q0. |

### 18. Experiencias exclusivas y erratas finales

Se alineó q14 con el Figma en las siete descripciones verificadas, la redacción de Priority Pass y
seguros, las fichas Platinum y la puntuación. Se quitó Skybox de Visa Clásica LATAM Pass. Los datos de
producto se cotejaron con el catálogo público: Sapphire usa **1.5 millas**, Signature LATAM Pass
**1.25 millas**, y Visa Platinum LATAM Pass ya no muestra el descuento de S/150 de American Express.

También se corrigieron **«Puntos Qores» → «Puntos Qore»**, **«Ideal para acumula» → «Ideal para
acumular»** y **«e consumo» → «de consumo»**. La Visa Clásica sin código queda preparada con
exoneración de **S/50 al mes** y una descripción de cashback.

Figma contiene dos variantes llamadas Visa Platinum Qore. La búsqueda interna confirmó que ambas usan
la acumulación Platinum de 1.5 puntos; no son una ficha de Signature Qore. El proyecto conserva
Signature con sus datos propios y usa para Platinum la variante «Beneficios superiores…».

### 19. Reauditoría visual y de producto de las fichas visibles

Aplicado el **2026-09-21** tras una revisión directa de la página enlazada de Figma y de las fichas
públicas de `viabcp.com`:

- En *Viajar*, American Express Platinum adopta la descripción y las cuatro viñetas de Figma, incluidas
  «solo por digital» y Cuotas Sin Intereses. Marco precisó que la ficha pública solo reemplaza a Figma
  cuando existe una contradicción expresa; la ausencia de un dato en la ficha pública no basta para
  eliminarlo.
- En el caso B de *Experiencias exclusivas* se actualizaron American Express Oro, Visa Clásica LATAM
  Pass, American Express Clásica LATAM Pass, Visa Clásica Qore y Visa Light. Figma define descripción,
  jerarquía y selección de contenido; las fichas públicas respaldan acumulación, promociones, seguros,
  descuentos, Qore y cuotas según corresponda.
- Se eliminaron las viñetas heredadas que Figma ya no asigna a esas fichas. Las variantes ordinarias de
  Visa Oro Qore no se tocaron en esta corrección porque el motor las sustituye por la destacada común
  cuando `TCRLY3` es lead.
- La variante de Signature Qore en Experiencias sigue siendo una excepción documentada: Figma contiene
  dos fichas llamadas Platinum Qore y no ofrece una correspondencia inequívoca para Signature.

### Nota sobre la corrección 4

Al reenviar la pantalla de Acumulación de millas, el texto volvía a decir "**Estos** son las millas". Se
mantuvo la versión corregida ("**Estas**"), y la corrección quedó reconfirmada. Vale tenerlo presente: si
el texto original sigue circulando en otros documentos, todavía tiene el error.

**Actualización del 2026-09-21:** ese encabezado dejó de mostrarse. Marco aprobó sustituir los encabezados
separados de millas y puntos por un solo texto: «¡Buena elección! 😊 Descubre cuántas millas y puntos
podrías ganar mensualmente con tus compras.».

---

## Parte 2 — Decisiones de formato

No son correcciones de texto: son reglas de cómo se ve el chat. Se tomaron durante la implementación y
afectan a **todas** las pantallas.

### Las tarjetas recomendadas van en carrusel

**Decidido el 2026-09-10.** En las cuatro pantallas del perfilador (1.1 a 1.4), las tarjetas aprobadas
estaban apiladas una debajo de la otra. Con 3 o 4 tarjetas el bloque quedaba larguísimo: había que
scrollear todo el panel y se perdía la comparación entre una tarjeta y la siguiente.

**Ahora es un carrusel horizontal.** Se ve **una tarjeta a la vez**, ocupando el 90% del ancho (era 82%
hasta el 2026-09-22), de modo que **asoma un ~10% de la siguiente** por el borde derecho para que se note
que hay más. Se desliza con el dedo o con las flechas, y siempre queda calzada en una tarjeta: no hay
scroll libre a medio camino.

Debajo del carrusel hay una fila de controles centrada:

| Control | Cómo se ve |
| --- | --- |
| Flecha anterior | círculo **naranja** si hay tarjetas hacia la izquierda; **gris y deshabilitada** en la primera tarjeta |
| Puntos de posición | el activo azul, los inactivos gris claro. Son clickeables |
| Flecha siguiente | círculo **naranja** si hay tarjetas hacia la derecha; **gris y deshabilitada** en la última tarjeta |

**El color de las flechas indica hacia dónde hay más tarjetas** (corregido el 2026-09-14). Antes la flecha
anterior era siempre gris y la siguiente siempre naranja, y la deshabilitada solo se atenuaba: en la última
tarjeta quedaba un naranja desvaído a la derecha y un gris activo a la izquierda, al revés de lo que había
que comunicar. Ahora naranja es "hay opciones por este lado" y gris es "no hay más", en ambas direcciones.

**La tarjeta en sí no cambió en nada**: mismo fondo, mismo borde fino, mismas esquinas redondeadas, mismo
título azul marino, misma bajada, mismas viñetas y el mismo botón naranja alineado a la derecha. El borde
verde de la tarjeta destacada también sigue igual. Solo cambió cómo se recorren.

Dos detalles que se decidieron al implementarlo:

- **Todas las tarjetas quedan con la misma altura**, la de la más alta, para que no salte el layout al
  deslizar. Como consecuencia, en las tarjetas más cortas el botón "Elegir tarjeta" se apoya **al fondo** de
  la caja en vez de quedar flotando a media altura.
- **Si hay una sola tarjeta no hay carrusel**: se muestra a todo el ancho y sin flechas ni puntos, porque
  una fila de controles para una sola tarjeta sobra.

### Ancho de las burbujas

**Regla:** el ancho lo decide el contenido, con tope en el 100% del ancho del chat. Además, si la burbuja
tiene **botones o una tabla**, se lleva al 100%.

**Por qué:** antes todas las burbujas del bot estaban fijas en 90%, con una excepción manual para las
tablas. Eso hacía que una burbuja de tres palabras ocupara lo mismo que una tabla de 17 filas. Con la regla
nueva:

| Burbuja | Ancho |
| --- | --- |
| Tablas comparativas | 100% |
| Menús con botones | 100% |
| "¡Gracias por utilizar nuestro asistente!" | 89% |
| "¡Gracias por tu calificación!" | 61% |
| Respuestas del usuario (burbuja azul) | 40–65% según el largo |

**Efecto secundario bueno:** los botones de "Resolver dudas" se partían en dos líneas y dejaron de hacerlo,
porque la burbuja ahora se estira para que quepan.

### Separación entre burbujas

**Regla:** 24px cuando cambia quien habla, **12px (la mitad)** cuando se encadenan dos burbujas del mismo
lado.

**Por qué:** al final del flujo hay cuatro mensajes seguidos del bot. Con una separación pareja se leían
como cuatro mensajes sueltos; con la mitad se leen como un bloque, que es lo que son.

### Interlineado

**Regla:** todo el texto de 14px va con **interlineado de 20px**.

**Por qué:** es la medida del sistema de diseño. Antes cada regla usaba su propia proporción (1,2 o 1,3),
que a 14px daban 16,8 y 18,2 píxeles. Ahora hay un solo valor.

**Dos efectos que se van a notar al comparar con capturas anteriores:**

| Elemento | Antes | Ahora |
| --- | --- | --- |
| Botones de opción | 34px de alto | **37px** |
| Nombre de la tarjeta en el perfilador | 15px | **14px**, igual que su descripción |

El nombre de la tarjeta ahora se distingue de su descripción solo por la negrita, no por el tamaño.

> **Ojo:** el nombre de la tarjeta dentro del recuadro de las pantallas de comparar **sigue en 15px**. Es
> el hermano del que sí cambió, así que las dos pantallas ya no lo escriben igual. Falta decidir si se
> unifica.

### Separación entre párrafos dentro de una burbuja

**Regla:** 16px, tanto entre párrafos como entre el último párrafo y los botones.

**Por qué:** antes eran 8px entre párrafos y 16px solo antes de los botones, lo que se veía apretado.

### Dos formas de cortar una línea

Esto es lo más útil de saber para quien escribe la copy:

| Lo que escribas | Lo que sale |
| --- | --- |
| **Salto de línea simple** | La frase baja al renglón siguiente, **pegada**, sin espacio. |
| **Línea en blanco** | Párrafo nuevo, con 16px de aire. |

Se implementó así porque un mockup encadenaba dos frases sin espacio entre ellas y reservaba el aire para
separar bloques de idea. Ahora esa intención se puede expresar directamente en el texto.

### Cada respuesta se muestra desde su inicio

**Decidido el 2026-09-14.** Al elegir una opción, el chat bajaba solo hasta el final de la respuesta. En las
pantallas largas (tablas, carrusel) el usuario quedaba frente al menú "¿Qué deseas hacer ahora?" sin haber
visto nada de lo de arriba.

**Ahora arriba del panel queda la opción que eligió el usuario y, justo debajo, el inicio de la respuesta**,
y el usuario baja leyendo.
Mientras el bot "piensa", el chat sigue bajando para mostrar el mensaje del usuario y los puntos de espera.

**El movimiento es suave, no un salto**, tanto al bajar a los puntos de espera como al subir a la respuesta.
Si el usuario tiene activado "reducir movimiento" en su sistema, se respeta y el desplazamiento es directo.

---

## Parte 3 — Diferencias deliberadas con el Figma

**Nada de esto es un error.** Si alguien compara el bot con los mockups va a encontrar estas cuatro
diferencias, y todas están decididas.

| Elemento | En el Figma | En el bot | Por qué |
| --- | --- | --- | --- |
| Saludo inicial | `Hola! 👋 Soy Tarjetín.` | Igual, **sin** el "¡" | Se revisó y se decidió mantenerlo como el mockup. La burbuja del launcher sí lleva "¡Hola!". |
| Punto de "En línea" | Blanco | **Verde** | Decisión explícita. El bot de la home lo tiene blanco; acá se eligió verde. |
| Robotito en el chat | Aparece junto a cada respuesta del bot | **No aparece** | Se quitó por decisión previa. El avatar de la cabecera sí se mantiene. |
| Ancho de los botones | Entran en una línea | Pueden partirse | El chat del mockup está dibujado más ancho que el real. En el ancho real, algunos textos largos no entran. |

**Un caso al revés:** la cabecera decía "Asistente **v**irtual" con minúscula y el Figma la tenía con
mayúscula. Se cambió a **"Asistente Virtual"** para respetar el Figma. Vale aclararlo porque el bot de la
home sigue usando minúscula, y esa diferencia entre productos ahora es intencional.

### La burbuja del usuario repite el botón completo

**Decidido el 2026-09-09.** Es la diferencia con el Figma que más se va a notar, así que conviene
explicarla bien.

**Cómo funciona el bot:** cuando el usuario toca un botón, aparece una burbuja azul a la derecha que
muestra lo que "dijo". Esa burbuja **se genera automáticamente copiando el texto del botón**. No son dos
textos independientes: son el mismo.

**Qué muestran los mockups:** en varias pantallas el botón tiene una frase larga y conversacional, pero la
burbuja azul aparece dibujada con una etiqueta más corta.

| El usuario toca | Figma muestra en la burbuja | El bot muestra |
| --- | --- | --- |
| ⚖️ Quiero comparar mis tarjetas | "Comparar tarjetas" | "Quiero comparar mis tarjetas" |
| ❓ Resolver dudas para elegir | "Resolver dudas" | "Resolver dudas para elegir" |
| 🤔 ¿Por qué me ofrecieron esa línea? | "…esa línea de crédito?" | "…esa línea?" |

**Qué se decidió:** dejar que la burbuja repita el botón completo. Los botones **no** se acortan y las
burbujas **no** se acortan.

**Por qué:** la alternativa era agregarle al bot la posibilidad de que cada botón lleve una etiqueta
distinta para la burbuja. Se evaluó y se descartó: agrega una pieza más que mantener a cambio de una
diferencia que en la conversación real casi no se nota, porque la burbuja larga también se lee natural.

**Esto aplica a todo el bot**, no solo a las tres filas del cuadro. Si en un mockup nuevo la burbuja azul
no coincide con su botón, la regla ya está tomada: **manda el botón**.

---

## Parte 4 — Cambios de contenido pedidos

No son correcciones ni decisiones de diseño: son cambios de qué dice el bot, pedidos durante el proyecto.

| Pantalla | Cambio |
| --- | --- |
| Resolver dudas | Pasó de **5 preguntas a 3**. Salieron "¿Qué beneficios tiene mi tarjeta?", "¿Cuánto cuesta la membresía?" y "¿Puedo exonerar la membresía?". Entró "¿Por qué me ofrecieron esa línea?". |
| Ayúdame a elegir una tarjeta | "te recomendaré **solo la opción**" pasó a "te recomendaré **algunas opciones**". |
| Viajar y acumular millas (1.1) | El segundo caso —el del usuario que no tiene tarjetas de millas— **sale sin la Visa Oro LATAM Pass**. Ver abajo. |
| Las cuatro del perfilador (1.1–1.4) | La tarjeta destacada, la del recuadro verde con "La más usada", pasa de **Visa Oro LATAM Pass** a **Visa Oro Qore**. Ver abajo. |
| Las cuatro pantallas del perfilador (1.1 a 1.4) | El botón de cierre pasó de **"Finalizar"** a **"Cerrar"**, como el resto del bot. Ver abajo. |
| Priority Pass (2.4) | **Sale el recuadro "Te recomendamos esta tarjeta"**. Recomendaba la Visa Oro LATAM Pass, que en la tabla de esa misma pantalla figura sin Priority Pass. Ver abajo. |
| Todo el bot | **Cerrar el bot con la X ya no conserva la conversación**: al volver a abrirlo empieza desde el inicio. Ver abajo. |

### 1.1 — la Visa Oro se sale del segundo caso

La pantalla "Viajar y acumular millas" tiene dos versiones según el usuario. La segunda es para quien **no
tiene ninguna tarjeta de millas**, y empieza diciendo:

> Actualmente no cuentas con una tarjeta especializada en acumulación de millas o beneficios para viajes.

Pero la primera tarjeta que recomendaba era la **Visa Oro LATAM Pass**, marcada como "La más usada". Esa
tarjeta **sí es de millas** — su propia viñeta dice "Acumula 1 Milla por cada $1.5 de consumo". Si el
usuario la tiene, entonces sí tiene una tarjeta de millas y le correspondería la otra versión de la
pantalla. El texto se contradecía a sí mismo en su primer ítem.

**Se decidió sacarla.** El segundo caso arranca ahora con Visa Infinite Qore y queda sin ninguna tarjeta
marcada como "La más usada".

*Si UI pregunta:* no es un cambio de estilo ni de prioridad comercial. Es que la tarjeta que se recomendaba
era exactamente la que el texto acababa de decir que el usuario no tenía.

### La tarjeta destacada pasa a ser la Visa Oro Qore

**Pedido por Marco el 2026-09-21, con mockup.** En las cuatro pantallas del perfilador (1.1 a 1.4) hay una
**cuarta tarjeta que siempre va al final, dentro del recuadro verde y con la etiqueta "La más usada"**. Era
la **Visa Oro LATAM Pass**. Ahora es la **Visa Oro Qore**.

La ficha entregada, que es la misma para las cuatro pantallas:

> **La más usada**
> 💳 **Visa Oro Qore**
> Orientada a beneficios cotidianos y promociones.
> - Membresía anual S/170 **(GRATIS si consumes S/1 al mes).**
> - 1 punto Qore por cada $ consumido.
> - Cuotas Sin Intereses en comercios afiliados.
> - Acumulación de Puntos Qore para canjes.
>
> *[Elegir tarjeta]*

Tres cosas que conviene saber:

**1. Sigue mostrándose solo si el cliente tiene esa tarjeta aprobada.** No cambia la regla de siempre: quien
no tenga la Visa Oro Qore ve 3 tarjetas y ningún recuadro verde. El botón *Elegir tarjeta* pulsa el botón
"Seleccionar" de la Visa Oro Qore en la propia página.

**2. Antes había cuatro fichas distintas y ahora hay una sola.** Cada pantalla describía la Visa Oro LATAM
Pass con sus propias viñetas, enfocadas al criterio de esa pantalla. El mockup entregado trae una única
ficha, y es la que se usa en las cuatro.

**3. Es la primera viñeta del bot con negrita.** El mockup marca en negrita *"(GRATIS si consumes S/1 al
mes)."*, igual que lo escribe el catálogo de producto. Hasta ahora ninguna viñeta de tarjeta llevaba
formato, así que se habilitó la negrita en ese sitio. El resto de viñetas del bot no cambia.

**La Visa Oro LATAM Pass no desaparece:** sigue en las listas de las cuatro pantallas como una tarjeta más.
Lo que pierde es el puesto fijo al final y el recuadro verde.

**4. Ahora también sale destacada en la segunda versión de cada pantalla.** Las pantallas del perfilador
tienen dos versiones según las tarjetas del cliente. La segunda nunca había tenido tarjeta destacada, y con
la Visa Oro LATAM Pass eso no se notaba, porque por cómo están armadas las listas sus clientes siempre
entraban por la primera. La Visa Oro Qore sí puede caer en la segunda —le pasa a quien **solo tiene
tarjetas Qore**—, así que **Marco pidió que ahí también salga en verde**.

Para que nadie pierda nada, esa segunda versión sigue mostrando **4 tarjetas como máximo**: cuando el
cliente tiene la Visa Oro Qore, se muestran 3 más la destacada; cuando no la tiene, se muestran las 4 de
siempre y no hay recuadro verde. Antes y después, 4.

### Perfilador — el botón de cierre pasa de "Finalizar" a "Cerrar"

Las pantallas de Comparar y Resolver dudas cierran con **"Cerrar"**, que fue una unificación acordada (ver
corrección 7). El texto de 1.1 llegó con **"Finalizar"**, y el 2026-09-09 se decidió mantenerlo solo en el
perfilador, lo que dejaba **dos etiquetas distintas para la misma acción**.

**El 2026-09-14 se unificó:** las cuatro pantallas de "Ayúdame a elegir una tarjeta" cierran ahora con
"Cerrar". Ya no queda ningún "Finalizar" en el bot.

### Priority Pass — sale la tarjeta recomendada

Las cuatro pantallas de comparación recomendaban la **Visa Oro LATAM Pass**. En Priority Pass eso no
encajaba: la tabla de esa misma pantalla la muestra con **"No"**, así que el bot decía "estas son tus
tarjetas que incluyen Priority Pass" y recomendaba una que no lo incluye. Además los dos detalles del
recuadro ("Membresía: S/170", "Exoneración: S/1 mensual en consumo") no hablaban de Priority Pass.

**Se decidió el 2026-09-14 quitar el recuadro de esa pantalla.** Priority Pass queda con el intro, la tabla
y el menú "¿Qué deseas hacer ahora?". Las otras tres pantallas de comparación no cambian.

### Cerrar con la X reinicia la conversación

Hasta ahora, cerrar el panel con la X (o tocando fuera de él) solo lo ocultaba: al reabrir, la conversación
seguía donde había quedado. El botón "Cerrar" del final de cada pantalla, en cambio, ya empezaba de cero.

**Desde el 2026-09-14 las dos formas de cerrar se comportan igual:** al volver a abrir el bot, arranca desde
el saludo inicial. Consecuencia: el aviso "Error en el envío / Reintentar", que aparecía al reabrir si se
había cerrado el panel mientras el bot "pensaba", ya no puede mostrarse por esa vía.

### Perfilador — sale el asterisco final de cuatro viñetas

Cuatro fichas del perfilador terminaban en un **asterisco de nota al pie que no tenía nota al pie**. El bot
no muestra ninguna llamada aclaratoria al final de la tarjeta, así que el lector se quedaba buscando un
texto que no existe.

| Tarjeta | Pantalla | Decía | Dice |
| --- | --- | --- | --- |
| Visa Infinite Iridium LATAM Pass | 1.1 Viajar | `Acumula hasta 2 Millas por cada $1 de consumo.`**\*** | `…de consumo.` |
| Visa Infinite Qore | 1.2 Ahorrar | `…(GRATIS si consumes S/5,000 al mes).`**\*** | `…al mes).` |
| Visa Infinite Sapphire LATAM Pass | 1.2 Ahorrar | `…(GRATIS si consumes S/4,500 al mes).`**\*** | `…al mes).` |
| Visa Infinite Iridium LATAM Pass | 1.2 Ahorrar | `…(GRATIS si consumes S/5,000 al mes).`**\*** | `…al mes).` |

**Pedido por Marco el 2026-09-22.** No cambia ninguna cifra ni ninguna palabra: solo se borra el asterisco.
La negrita de "(GRATIS si consumes…)" se conserva tal cual.

### 2.3 Exoneración de membresía — asterisco en las Infinite y llamada al pie

Las tres tarjetas **Infinite** llevan ahora un asterisco al final del monto, y la tabla cierra con la
aclaración correspondiente:

| Tarjeta | Decía | Dice |
| --- | --- | --- |
| Visa Infinite Sapphire LATAM Pass | `S/4,500 al mes` | `S/4,500 al mes*` |
| Visa Infinite Iridium LATAM Pass | `S/5,000 al mes` | `S/5,000 al mes*` |
| Visa Infinite Qore | `S/5,000 al mes` | `S/5,000 al mes*` |

Debajo de la tabla, y **antes del recuadro "Te recomendamos esta tarjeta"**, aparece:

> \*Sujeto a términos y condiciones

**Tamaño 12px, interlineado 18px**, sin recuadro ni ícono. Es la segunda excepción a la regla de 14/20 que
rige el resto del hilo; la primera es el aviso azul de 1.1, que usa esa misma escala.

**Pedido por Marco el 2026-09-22.** Ninguna cifra cambió.

**Una regla que va con esto:** la tabla se filtra por las tarjetas que el usuario tiene aprobadas, así que
hay clientes a los que no les queda ninguna fila con asterisco. **En esos casos la nota no se muestra**, para
no repetir el problema del asterisco sin nota al pie que se corrigió ese mismo día en el perfilador.

### Perfilador — la ficha pasa a ocupar el 90% del carrusel

La tarjeta del carrusel ocupaba el **82%** del ancho, dejando asomar un ~18% de la siguiente. **Desde el
2026-09-22 ocupa el 90%**, a pedido de Marco: se prioriza la lectura de la ficha y queda una franja más
angosta de la siguiente como pista visual de que el carrusel se desplaza.

Es el mismo valor que ya usaba la variación premium, así que las dos versiones vuelven a coincidir en todo
lo que no sea el fondo de las tarjetas. No cambia nada más del carrusel: las flechas, los puntos y el gesto
de deslizar siguen igual.

---

## Parte 5 — Texto que no escribió el equipo de contenido

Hay **una sola** frase en todo el bot escrita por desarrollo, y necesita aprobación.

**Ya no queda ninguna.** Había una sola: el intro de Priority Pass, que había llegado incompleto del mockup
(`¡Buena elección! 😊 . ---`) y se redactó provisoriamente copiando la estructura de las otras pantallas.

El 2026-09-09 se entregó el texto definitivo — *"¡Buena elección! 😊 Estas son tus tarjetas aprobadas que
incluyen el beneficio Priority Pass."* — y reemplazó al provisorio. **Todo el texto del bot fue escrito por
el equipo de contenido.**

---

## Parte 6 — Temas abiertos

Nada de esto está decidido. Si UI pregunta, la respuesta es que se necesita definición.

### Datos que parecen inconsistentes

| Pantalla | Qué pasa |
| --- | --- |
| Membresía anual | Visa Light figura como `0` a secas, mientras el resto usa formato `S/80`, `S/170`, etc. |

> ✅ **Resuelto el 2026-09-21.** Las dos filas Qore de *Exoneración de membresía* (`Visa Clásica Qore S/80`
> y `Visa Oro Qore S/170`) sí eran un dato mal copiado: repetían sus montos de membresía. Ahora dicen
> **S/1**, según el catálogo público de producto. Ver la **corrección 13**.

> ✅ **Resuelto el 2026-09-21.** El cotejo contra el catálogo público dejó cinco diferencias de producto.
> Todas quedaron corregidas:

| Pantalla | Qué pasa |
| --- | --- |
| Experiencias exclusivas (1.4) | Sapphire quedó en **1.5** y Signature LATAM Pass en **1.25** millas por $1. |
| Experiencias exclusivas (1.4) | Se eliminó de Visa Platinum LATAM Pass el descuento de S/150 que corresponde a American Express. |
| Ahorrar en costos (1.2) | Visa Clásica usa **S/50 al mes** para exonerar. |
| Ahorrar en costos (1.2) | Visa Clásica se describe como tarjeta de cashback, sin atribuirle millas. |

La Visa Clásica sigue sin código y no se muestra hoy, pero su contenido ya es correcto para cuando pueda detectarse.

### Qué tarjetas pueden aparecer hoy, y una que no

**Actualizado el 2026-09-10.** El bot reconoce a cada tarjeta por su código, y sin código no la puede
detectar: no aparece nunca, en ninguna pantalla, aunque el cliente la tenga aprobada. De las 17 del
catálogo, **16 ya tienen código**.

| Tarjeta | Estado |
| --- | --- |
| **Visa Platinum Qore** | **Ya aparece.** Su código (`TCRLY4`) llegó el 2026-09-10. Hasta entonces estaba invisible: figuraba en las cuatro tablas de comparar y en tres pantallas del perfilador (1.2, 1.3 y 1.4), pero nunca se llegaba a mostrar. |
| **Visa Clásica** (la que no es LATAM Pass ni Qore) | **No aparece.** Es la única sin código. Se decidió dejarla así de momento: no muestra nada, y el día que tenga código empieza a incluirse sola, sin tocar el bot. |

> ⚠️ **Un nombre por confirmar.** El código `AMXGRE` figura en la lista de códigos como "Amex green", pero
> la página de /felicitaciones lo dibuja como **American Express Black LATAM Pass**. El bot se guía por lo
> que dibuja la página, que es evidencia directa. Si resultara que `AMXGRE` es de verdad una Amex Green,
> entonces la American Express Black se quedaría sin código y desaparecería de las tres pantallas del perfilador donde
> hoy figura (1.1, 1.3 y 1.4).

### Las tablas de comparar ahora muestran solo tus tarjetas

**Decidido el 2026-09-09.** Las cuatro pantallas de comparación (millas, membresía, exoneración y Priority
Pass) mostraban las 17 tarjetas del catálogo. Ahora muestran **únicamente las que el usuario tiene
aprobadas**, igual que el perfilador. El orden de cada tabla no cambia; solo se sacan las filas que no
aplican.

> ✅ **Resuelto el 2026-09-21.** q21, q22 y q23 recomiendan **Visa Oro Qore** y el bloque se filtra por el
> código `TCRLY3`: si el usuario no tiene esa tarjeta aprobada, la recomendación no aparece. q24 no tiene
> bloque recomendado.

### Erratas y contradicciones del perfilador

Las erratas «e consumo», «Ideal para acumula» y «Puntos Qores» están corregidas. Sapphire y Signature
LATAM Pass ya muestran la misma acumulación en todos los criterios: 1.5 y 1.25 millas, respectivamente.

**Resuelto para las fichas visibles el 2026-09-21.** Visa Clásica LATAM Pass, American Express Clásica
LATAM Pass, Visa Clásica Qore y Visa Light ya separan la descripción de sus viñetas según Figma. La
Visa Clásica sin código continúa fuera del runtime; no se le copió la descripción contradictoria de
Figma porque el catálogo público la clasifica como tarjeta de cashback, no como tarjeta de millas.

**Un emoji distinto:** en 1.3 la Visa Infinite Qore lleva 🥇 en vez del 💳 que llevan las otras 59
tarjetas. Se dejó tal cual, pero contradice lo acordado de que todas usan el mismo emoji.

### Inconsistencias de nomenclatura

| Qué | Detalle |
| --- | --- |
| Orden de las tarjetas | Las tablas no listan las tarjetas en el mismo orden entre sí. Se respetó el orden de cada mockup sin unificarlos. |

### Decisiones de flujo pendientes

| Qué | Detalle |
| --- | --- |
| Las tres preguntas retiradas | ¿Salen del bot definitivamente o reaparecen en otra pantalla? |
| **El recuadro "Te recomendamos esta tarjeta" no siempre aparece** | Resuelto: las pantallas de millas, membresía y exoneración recomiendan la **Visa Oro Qore** y el recuadro solo se muestra si `TCRLY3` está entre las tarjetas aprobadas. Priority Pass no lleva recomendación. El beneficio se presenta como párrafo, sin viñeta. |
| Última burbuja del flujo | Dice "¿Qué deseas hacer ahora?" con un único botón "Cerrar", y por la regla de ancho ocupa el 100%, lo que la deja con mucho espacio vacío. Falta decidir si se deja pareja con los demás menús o se hace una excepción. |
| Priority Pass: el intro y la tabla no dicen lo mismo | El intro dice *"Estas son tus tarjetas aprobadas **que incluyen** el beneficio Priority Pass"*, lo que sugiere una lista filtrada, pero la tabla muestra las 17 tarjetas con Sí/No. Es menor, pero puede confundir. |

---

## Resumen en una frase

Se corrigieron las diferencias de wording aprobadas, las erratas detectadas y las contradicciones de
producto encontradas en el cotejo. Se tomaron **2 unificaciones** (el botón de cierre y la grafía de la marca) y **5 decisiones de formato** que
afectan a todo el chat. Hay **4 diferencias deliberadas** con el Figma que no son errores. Ya **no queda
ninguna frase escrita por desarrollo**: todo el texto del bot lo escribió el equipo de contenido.

No quedan contradicciones abiertas con el catálogo público en esta revisión. Sigue abierta una decisión
sobre **la encuesta de satisfacción, que quedó sin forma de aparecer**. El cierre ya
usa **una sola etiqueta** ("Cerrar") en todo el bot desde el 2026-09-14.
