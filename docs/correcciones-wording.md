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

### Nota sobre la corrección 4

Al reenviar la pantalla de Acumulación de millas, el texto volvía a decir "**Estos** son las millas". Se
mantuvo la versión corregida ("**Estas**"), y la corrección quedó reconfirmada. Vale tenerlo presente: si
el texto original sigue circulando en otros documentos, todavía tiene el error.

---

## Parte 2 — Decisiones de formato

No son correcciones de texto: son reglas de cómo se ve el chat. Se tomaron durante la implementación y
afectan a **todas** las pantallas.

### Las tarjetas recomendadas van en carrusel

**Decidido el 2026-09-10.** En las cuatro pantallas del perfilador (1.1 a 1.4), las tarjetas aprobadas
estaban apiladas una debajo de la otra. Con 3 o 4 tarjetas el bloque quedaba larguísimo: había que
scrollear todo el panel y se perdía la comparación entre una tarjeta y la siguiente.

**Ahora es un carrusel horizontal.** Se ve **una tarjeta a la vez**, ocupando el 82% del ancho, de modo que
**asoma un ~18% de la siguiente** por el borde derecho para que se note que hay más. Se desliza con el dedo
o con las flechas, y siempre queda calzada en una tarjeta: no hay scroll libre a medio camino.

Debajo del carrusel hay una fila de controles centrada:

| Control | Cómo se ve |
| --- | --- |
| Flecha anterior | círculo gris, **deshabilitada** cuando estás en la primera tarjeta |
| Puntos de posición | el activo azul, los inactivos gris claro. Son clickeables |
| Flecha siguiente | círculo naranja, el mismo naranja del botón "Elegir tarjeta" |

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
| Viajar y acumular millas (1.1) | El botón de cierre es **"Finalizar"**, no "Cerrar" como el resto del bot. Ver abajo. |

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

### 1.1 — el botón de cierre dice "Finalizar"

Las siete pantallas terminadas del bot cierran con **"Cerrar"**, que fue una unificación acordada (ver
corrección 7). El texto de 1.1 llegó con **"Finalizar"**.

**Se decidió mantener "Finalizar", pero solo en el perfilador** (las cuatro pantallas de "Ayúdame a elegir
una tarjeta"). Comparar y Resolver dudas siguen con "Cerrar".

*Conviene saberlo antes de la reunión:* quedan **dos etiquetas distintas para la misma acción** dentro del
mismo bot. Es una decisión tomada, no un descuido, pero es lo primero que va a saltar si alguien compara
pantallas.

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
| Exoneración de membresía | Dos filas de tarjetas Qore (`Visa Clásica Qore S/80` y `Visa Oro Qore S/170`) coinciden exactamente con sus montos de **membresía** de otra pantalla, no con montos de exoneración. Las demás filas Qore sí usan la escala de exoneración (S/1,200 / S/3,500 / S/5,000). **Puede ser un dato mal copiado.** |
| Membresía anual | Visa Light figura como `0` a secas, mientras el resto usa formato `S/80`, `S/170`, etc. |

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

> ⚠️ **Esto dejó una contradicción a la vista.** El recuadro "Te recomendamos esta tarjeta" de esas cuatro
> pantallas sigue fijo en **Visa Oro LATAM Pass** y no se filtra. Un usuario que no tenga esa tarjeta ahora
> ve una tabla de, por ejemplo, cuatro filas donde la Visa Oro no está — y justo debajo, un recuadro que se
> la recomienda. Antes del filtro era discutible; ahora la pantalla se contradice sola.

### Erratas y contradicciones del perfilador (1.2, 1.3 y 1.4)

Estas **se reprodujeron tal cual** y no se corrigieron, porque no hay aprobación. Todas son de los textos
entregados el 2026-09-09.

| Pantalla | Dice | Qué pasa |
| --- | --- | --- |
| 1.2, caso sin tarjetas de ahorro | Visa Infinite Qore: "Exoneración consumiendo **S/5,00** al mes" | Le falta un cero. La Iridium, en la misma lista y con la misma membresía de S/500, dice S/5,000. |
| 1.3 | Visa Platinum Qore: "**Acumua** hasta 1.5 Puntos Qore" | Errata de tipeo: "Acumula". |
| 1.4 | Visa Platinum Qore: "por cada $1 **e** consumo" | Falta la "d": "de consumo". |
| 1.3, caso sin tarjetas de beneficios | Visa Clásica: "**Tarjeta premium para viajes y acumulación de millas**" | Se contradice con sus propias viñetas, que dicen "Tarjeta básica para compras y financiamiento". Además es exactamente la frase que describe a la American Express Platinum en 1.2: parece un copiar-pegar. |

**Dos tarjetas acumulan distinto según la pantalla:**

| Tarjeta | En 1.1 y 1.3 | En 1.4 |
| --- | --- | --- |
| Visa Infinite Sapphire LATAM Pass | hasta **1.5** Millas por $1 | hasta **1.25** Millas por $1 |
| Visa Signature LATAM Pass | hasta **1.25** Millas por $1 | hasta **1.5** Millas por $1 |

Son datos de negocio, así que no se tocaron. Pero el mismo usuario puede ver las dos cifras entrando por
dos caminos distintos del bot.

**Siete tarjetas quedaron sin frase de presentación.** En el texto entregado, la mayoría de las tarjetas
tiene una línea del tipo *"Ideal para quienes…"*, separada de las viñetas por un espacio en blanco. En
siete casos ese espacio no venía, así que **todo el contenido entró como viñetas** y esas tarjetas no
tienen frase de presentación: en 1.3 la Visa Clásica y la Visa Light; en 1.4 la Visa Clásica LATAM Pass, la
American Express Clásica LATAM Pass, la Visa Clásica Qore, la Visa Clásica y la Visa Light.

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
| **El botón "Elegir tarjeta" de la tarjeta recomendada puede quedar sin efecto** | Desde el 2026-09-11 ese botón no navega a ninguna URL: le da clic al botón **"Seleccionar"** de esa misma tarjeta en la página, así hace exactamente lo mismo que haría el cliente por su cuenta.<br><br>El problema es que las cuatro pantallas de comparación recomiendan siempre la **Visa Oro LATAM Pass**, sin mirar si el cliente la tiene aprobada. Si no la tiene, esa tarjeta no está en la página, no hay botón que apretar y **el botón del bot no hace nada**. Con el cliente del ejemplo de producción pasa exactamente eso: la tabla le muestra correctamente sus 4 tarjetas y debajo se le recomienda una quinta que no tiene.<br><br>Se resuelve solo cuando se definan las condiciones para elegir la tarjeta recomendada según el caso. Mientras tanto hay que decidir si, cuando el cliente no tiene la tarjeta recomendada, **se le esconde el recuadro entero**. |
| Última burbuja del flujo | Dice "¿Qué deseas hacer ahora?" con un único botón "Cerrar", y por la regla de ancho ocupa el 100%, lo que la deja con mucho espacio vacío. Falta decidir si se deja pareja con los demás menús o se hace una excepción. |
| **Priority Pass recomienda una tarjeta que no tiene Priority Pass** | Las cuatro pantallas de comparación recomiendan la misma tarjeta: **Visa Oro LATAM Pass**. En tres de ellas encaja. En Priority Pass **no**: esa tarjeta figura como "No" en la tabla de esa misma pantalla. Queda diciendo "estas son tus tarjetas que incluyen Priority Pass" y recomendando una que no lo incluye.<br><br>El bloque se agregó por pedido expreso, replicando el de las otras tres. **Es provisional y está asumido:** la tarjeta recomendada hoy está fija en las cuatro pantallas, y más adelante se van a definir las condiciones para que se elija según el caso. Cuando eso ocurra, esta pantalla debería recomendar alguna de las que sí tienen el beneficio (Visa Signature LATAM Pass, Visa Infinite Sapphire, Visa Infinite Iridium, American Express Black LATAM Pass, Visa Signature Qore o Visa Infinite Qore). |
| Los datos de la tarjeta recomendada no aplican en Priority Pass | Los dos detalles del recuadro — "Membresía: S/170" y "Exoneración: S/1 mensual en consumo" — hablan de membresía. En las otras tres pantallas tienen sentido; en Priority Pass quedan fuera de tema. |
| Priority Pass: el intro y la tabla no dicen lo mismo | El intro dice *"Estas son tus tarjetas aprobadas **que incluyen** el beneficio Priority Pass"*, lo que sugiere una lista filtrada, pero la tabla muestra las 17 tarjetas con Sí/No. Es menor, pero puede confundir. |

---

## Resumen en una frase

Se corrigieron **10 errores de redacción en 8 pantallas** (concordancias, una tilde, un signo de apertura,
una preposición duplicada y un nombre de tarjeta incompleto), sin reescribir ninguna frase. Se tomaron **2 unificaciones** (el botón de cierre y la grafía de la marca) y **5 decisiones de formato** que
afectan a todo el chat. Hay **4 diferencias deliberadas** con el Figma que no son errores. Ya **no queda
ninguna frase escrita por desarrollo**: todo el texto del bot lo escribió el equipo de contenido.

Quedan abiertos: **datos de dos tablas** que parecen mal copiados, una decisión sobre **la encuesta de
satisfacción, que quedó sin forma de aparecer**, y el hecho de que el bot ahora **cierra con dos etiquetas
distintas** ("Cerrar" en la mayoría, "Finalizar" en el perfilador).
