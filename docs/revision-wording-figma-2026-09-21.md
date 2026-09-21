# Revisión de wording contra Figma — 2026-09-21

**Resultado final: se aplicaron todas las diferencias aprobadas del Figma y se corrigieron sus erratas sin contradecir el catálogo público.**

**Actualización posterior a la revisión:** el usuario aprobó agregar «solo por digital». Aplicado a 21 textos de bienvenida del HTML y su espejo `contenido/wordings/flujo-1-elegir.json`: 10 en Viajar, 10 en el caso A de Beneficios (incluido Infinite Qore) y 1 en el caso B de Beneficios. Los bonos de Signature Qore y Platinum Qore conservan su texto, porque las fichas revisadas de Figma no incluyen esa condición. El resto de este informe describe la comparación original; los demás puntos siguen pendientes.

**Actualización 2:** el acceso al comparador ahora adapta su etiqueta a las tarjetas aprobadas: «Acumulación de millas / Puntos» con LATAM Pass y Qore, «Acumulación de millas» con solo LATAM Pass y «Acumulación de puntos» con solo Qore. Sin un programa detectable conserva la etiqueta general.

**Actualización 3:** el contenido del comparador de acumulación aplica la misma regla. Muestra las tablas de millas y puntos cuando hay tarjetas de ambos programas, o únicamente la tabla correspondiente cuando solo existe uno. Cada tabla contiene solo las tarjetas aprobadas detectadas. El wording y los valores Qore se tomaron del Figma solicitado.

**Actualización 4:** las recomendaciones de q21, q22 y q23 ahora usan Visa Oro Qore (`TCRLY3`). En q21 muestran «Acumula 1 Punto Qore por cada $1 de consumo.»; en q22 y q23 muestran «Membresía anual S/170 (GRATIS si consumes S/1 al mes).». También se ajustó la separación visual: 48 px efectivos entre la tabla de millas y «Estos son los puntos por consumo mensual», y 32 px antes de «Te recomendamos esta tarjeta».

**Actualización 5:** el encabezado de q23 ahora dice «Exoneración de membresía por consumo mensual» y todos los importes de consumo añaden «al mes». Visa Light conserva «Gratis». Visa Clásica Qore y Visa Oro Qore mantienen el valor corregido de S/1 al mes, respaldado por el catálogo público, en lugar de copiar los montos de membresía contradictorios del Figma.

**Actualización 6:** se aplicaron las seis diferencias claras de «Ahorrar en costos»: punto final en Visa Light; wording de descuentos de Visa Clásica LATAM Pass y Visa Clásica Qore; nueva descripción de Visa Oro LATAM Pass; corrección de S/5,00 a S/5,000 en Visa Infinite Qore; y asterisco final en la membresía de Visa Infinite Sapphire. Las discrepancias de datos se resuelven con la referencia web pública, no copiando cifras contradictorias del Figma.

**Actualización 7:** «Obtener más beneficios» se alineó con Figma: se actualizaron las 14 descripciones del caso A, las cuatro fichas del caso B, la ficha completa de Visa Oro LATAM Pass, el typo «Acumua» y el asterisco de acumulación de Iridium.

**Actualización 8:** «Experiencias exclusivas» se alineó con Figma en descripciones, Priority Pass, seguros, orden y contenido de las fichas Platinum, puntuación y eliminación de Skybox. Las cifras de Sapphire y Signature y el beneficio de restaurantes se ajustaron al catálogo público. La búsqueda interna de Figma confirmó que sus dos fichas llamadas «Visa Platinum Qore» contienen acumulación Platinum de 1.5 puntos; se conservaron como variantes de Platinum y no se reasignaron a Signature.

**Actualización 9:** la burbuja junto al botón flotante ahora dice «¡Hola! Te ayudaré a elegir una tarjeta».

**Actualización 10:** se corrigieron «Puntos Qores», «Ideal para acumula» y «e consumo». La Visa Clásica sin código quedó preparada con S/50 al mes y una descripción de cashback. Ya no quedan puntos de esta revisión pendientes de implementación.

Referencia: [5.34.TC.BOT_web, enlace solicitado](https://www.figma.com/design/5M71uw8ny9baMkeQ3145Zm/5.34.TC.BOT_web?node-id=940-2642).
Revisado desde Chrome: página Mobile de «UI - Inferfaz - Felicitaciones», sección `Handoff_mobile`, mensajes de los flujos de inicio, elección, comparación y dudas. Se leyeron las capas expandidas y se cotejaron visualmente los menús y las tablas. Comparación con el contenido actual de `adobe-target/piloto/bot.html`, incluyendo los cambios que ya estaban sin commit al iniciar la revisión.

Alcance: textos del bot. No se evaluaron diseño, espaciado, colores ni funcionamiento. No se modificó el HTML ni los JSON de contenido. Esta revisión no certifica otras páginas del archivo Figma, como Desktop, MVP o TOBE. Las capas llamadas `Text`, `Tag` o `cell-programa` no se tomaron como contenido literal; se consultaron las pantallas para leer los textos correspondientes.

## Diferencias principales

| Ubicación | Proyecto actual | Figma actual |
| --- | --- | --- |
| Menú de comparación, q2 | «Acumulación de millas por consumo» | «Acumulación de millas / Puntos» |
| Comparador q21 | Una tabla de millas; las cinco Qore dicen «No Aplica». | Incluye el texto «Estos son los puntos por consumo mensual» y una tabla con acumulación Qore. |
| Recomendación q21 | Visa Oro LATAM Pass; «Membresía: S/170» y «Exoneración: S/1 mensual en consumo.» | Visa Oro Qore; «Acumula 1 Punto Qore por cada $1 de consumo.» |
| Recomendaciones q22 y q23 | Visa Oro LATAM Pass, con los dos textos anteriores. | Visa Oro Qore; «Membresía anual S/170 (GRATIS si consumes S/1 al mes).» |
| Encabezado de exoneración | «Exoneración de membresía» | «Exoneración de membresía por consumo mensual» |
| Valores de exoneración | «S/1», «S/1,200», etc. | Añaden «al mes». Visa Light conserva «Gratis». Hay dos cifras Qore contradictorias, detalladas más abajo. |
| Bienvenida en Viajar y acumular millas, q11 | Las 10 fichas LATAM Pass omiten «solo por digital». | Las 10 fichas lo incluyen: desde las 15,000 millas de Iridium hasta las 1,000 de las clásicas. |
| Bienvenida en Obtener más beneficios, q13 | Las fichas con bono conservan el wording anterior. | Añaden «solo por digital» a las bienvenidas LATAM Pass y a los 15,000 Puntos Qore de Visa Infinite Qore. Signature y Platinum Qore no añaden esa condición en las fichas leídas. |
| Descripciones en Obtener más beneficios, q13 | Predominan «Obtén…», «Acumula…» y otras formulaciones anteriores. | Se reescriben mayoritariamente como «Ideal para…». Ver tabla siguiente. |
| Descripciones en Experiencias exclusivas, q14 | Formulaciones anteriores como «Experiencia premium enfocada en viajes internacionales». | Nuevas frases centradas en experiencias exclusivas. También cambian beneficios concretos. |
| Burbuja de apertura, junto al botón flotante | «¡Hola! Soy Tarjetín. 👋 ¿En qué te puedo ayudar?» | En la pantalla de página con el launcher se lee «¡Hola! Te ayudaré a elegir una tarjeta». |

La nueva tabla de puntos de Figma contiene:

| Tarjeta | Texto en Figma |
| --- | --- |
| Visa Clásica Qore | 1 Punto Qore por cada $2 |
| Visa Oro Qore | 1 Punto Qore por cada $1 |
| Visa Platinum Qore | Hasta 1.5 Puntos Qore por cada $1 |
| Visa Signature Qore | Hasta 2.5 Puntos Qores por cada $1 |
| Visa Infinite Qore | Hasta 3 Puntos Qore por cada $1 |

«Qores» aparece así en la fila de Signature: es una errata de la referencia. La separación en dos tablas se registra como contexto del contenido nuevo; no se propone un cambio de diseño en esta revisión.

## Viajar y acumular millas — q11

Además de «solo por digital» en las 10 bienvenidas:

- Visa Oro LATAM Pass añade «Cuotas Sin Intereses en comercios afiliados.»; el proyecto no tiene esa viñeta en este criterio.
- Iridium: el proyecto termina la viñeta de acumulación con `.*`; la ficha de Figma leída termina con punto, sin asterisco.
- Los nombres abreviados «AMEX»/«Amex», «Shapphire» y la concordancia «viajeros frecuentes que quiere» son diferencias ya corregidas deliberadamente en el proyecto. No deben confundirse con contenido nuevo pendiente.
- La ficha destacada de Visa Oro Qore con «Orientada a beneficios cotidianos y promociones.» y sus cuatro viñetas ya está en el proyecto. En Figma también hay fichas ordinarias de esa tarjeta con otros textos; no son todas la misma variante.

## Ahorrar en costos — q12

Las formulaciones «Membresía anual … (GRATIS si consumes … al mes)» ya están presentes en el HTML actual. Las diferencias pendientes observadas son:

| Tarjeta / frase | Proyecto | Figma |
| --- | --- | --- |
| Visa Light | «Membresía 0 sin consumo mínimo» | Misma frase con punto final. |
| Visa Clásica LATAM Pass | «Promociones y descuentos BCP.» | «Acceso a descuentos y promociones BCP.» |
| Visa Clásica Qore | «Acceso a descuentos y promociones.» | «Acceso a descuentos y promociones BCP.» |
| Visa Oro LATAM Pass, caso A | «Ideal para quienes buscan acumular más millas manteniendo un requisito de uso accesible.» | «Ideal para viajeros frecuentes que buscan acumular más millas con facilidad.» |
| Visa Infinite Qore, caso B | «…S/5,00 al mes…» | «…S/5,000 al mes…» |
| Sapphire, caso B | La viñeta de membresía no tiene asterisco final. | La viñeta termina en `.*`. |

En la variante sin tarjetas de ahorro, Figma destaca Visa Oro Qore con «Ideal para quienes buscan Puntos Qore en sus compras frecuentes». El proyecto usa para la destacada la descripción común «Orientada a beneficios cotidianos y promociones», por una decisión documentada del 21 de septiembre. Se registra la diferencia sin revertir esa decisión.

La ficha de American Express Oro en Figma dice membresía **S/80**, aunque la tabla de membresía del mismo Figma dice **S/170**. El proyecto tiene S/170. No es una diferencia que deba copiarse sin resolver la contradicción.

## Obtener más beneficios — q13

Descripciones de las fichas del caso A:

| Tarjeta | Proyecto | Figma |
| --- | --- | --- |
| Visa Infinite Iridium LATAM Pass | Obtén recompensas mediante una alta acumulación de millas. | Ideal para obtener recompensas mediante una alta acumulación de millas. |
| Visa Infinite Sapphire LATAM Pass | Acumula millas rápidamente para canjear beneficios y viajes. | Ideal para acumula millas rápidamente para canjear beneficios y viajes. |
| Visa Signature LATAM Pass | Obtén más valor por tus consumos diarios. | Ideal para obtener más millas y beneficios premium con cada compra. |
| American Express Platinum LATAM Pass | Obtén más beneficios por tus consumos frecuentes. | Ideal para obtener más beneficios por tus consumos frecuentes. |
| American Express Black LATAM Pass | Acumulación acelerada y beneficios premium. | Ideal para disfrutar de beneficios exclusivos mientras acumulas millas más rápido. |
| Visa Infinite Qore | Combina beneficios exclusivos y experiencia premium. | Ideal para disfrutar beneficios exclusivos y una experiencia superior al viajar. |
| Visa Signature Qore | Mayor valor para consumo frecuente. | Ideal para maximizar tus recompensas con cada compra. |
| Visa Platinum Qore | Más promociones y beneficios para el día a día. | Ideal para quienes buscan más beneficios y una mayor acumulación de Puntos Qore. |
| Visa Platinum LATAM Pass | Combina beneficios y acumulación para usuarios frecuentes. | Ideal para viajeros frecuentes que quieren obtener más millas con cada compra. |
| American Express Oro LATAM Pass | Para los que buscan viajar con más frecuencia. | Ideal para los que buscan viajar con más frecuencia. |
| Visa Oro Qore, ficha ordinaria | Acumulación de puntos para recompensas. | Ideal para obtener recompensas frecuentes con tus consumos diarios. |
| Visa Oro LATAM Pass | Mayor acumulación de millas sin incrementar el requisito de uso. | Ideal para viajeros frecuentes que buscan acumular más millas con facilidad. |
| American Express Clásica LATAM Pass | Acumula recompensas por tus compras diarias. | Ideal para comenzar a acumular millas con tus compras diarias. |
| Visa Clásica Qore | Es la puerta de entrada al ecosistema de beneficios Qore. | Ideal para comenzar a obtener recompensas con tus compras diarias. |

«Ideal para acumula» es una errata presente en Figma, no una propuesta de corrección. La descripción ordinaria de Oro Qore puede no llegar a mostrarse porque el motor sustituye esa ficha por la destacada común.

Otras diferencias:

- Iridium: desaparece el asterisco final de la viñeta de acumulación en la ficha leída de Figma.
- Hay diferencias menores de puntos finales y de «Millas»/«millas», además de la condición «solo por digital».
- Caso B, Visa Oro LATAM Pass: Figma presenta la ficha completa de beneficios, con «Ideal para viajeros frecuentes que buscan acumular más millas con facilidad», bienvenida digital, acumulación «en compras», promociones LATAM Pass y Cuotas Sin Intereses. El proyecto mantiene «Incrementa la acumulación de millas en compras frecuentes» y solo tres viñetas.
- Caso B, Visa Clásica LATAM Pass: «Empieza a acumular recompensas por tus compras» pasa a «Ideal para empezar a acumular recompensas por tus compras».
- Caso B, Visa Clásica: «Tarjeta premium para viajes y acumulación de millas» pasa a «Ideal para viajes y acumulación de millas». El resto de la ficha continúa hablando de compras y financiamiento; la contradicción de contenido no desaparece.
- Caso B, Visa Light: Figma dice «Alternativa simple para realizar compras», «Cuotas Sin Intereses en comercios afiliados» y «Membresía S/0». El proyecto conserva además «…control financiero» y «Pensada para simplicidad antes que para maximizar beneficios».

## Experiencias exclusivas — q14

En Figma hay variantes con y sin las viñetas de acumulación; no deben mezclarse al actualizar el contenido. En la variante extensa con acumulación se observó:

| Tarjeta | Descripción del proyecto | Descripción en Figma |
| --- | --- | --- |
| Visa Infinite Iridium LATAM Pass | Experiencia premium enfocada en viajes internacionales. | Ideal para quienes buscan experiencias únicas y privilegios exclusivos en cada viaje. |
| Visa Infinite Sapphire LATAM Pass | Experiencia premium enfocada en viajes y comodidad. | Ideal para quienes buscan experiencias exclusivas y beneficios únicos en cada viaje. |
| American Express Black LATAM Pass | Experiencia premium para viajeros frecuentes. | Ideal para quienes buscan una experiencia de viaje exclusiva y beneficios premium. |
| American Express Platinum LATAM Pass | Experiencia premium combinada con acumulación de millas. | Ideal para disfrutar experiencias únicas y beneficios exclusivos que acompañan cada viaje. |
| Visa Infinite Qore | Orientada a experiencias exclusivas. | Ideal para quienes buscan experiencias exclusivas y el máximo nivel de beneficios Qore. |
| Visa Signature LATAM Pass | Excelente equilibrio entre exclusividad y acumulación de millas frecuentes. | Ideal para quienes buscan viajar con más beneficios y experiencias exclusivas. |
| Visa Platinum LATAM Pass | Primer nivel premium dentro de LATAM Pass. | Ideal para quienes valoran experiencias exclusivas y más beneficios al viajar. |

Cambios adicionales de contenido:

- Iridium y Sapphire: Figma usa «Priority Pass para titular + … invitados» y «Seguros Visa Infinite». El proyecto usa «Priority Pass + …» y «Beneficios y seguros Visa Infinite» en este criterio.
- Visa Platinum LATAM Pass: en Figma ya no aparecen «Canje de millas por vuelos y productos LATAM» ni «Hasta S/150 de descuento en restaurantes». Se conservan Concierge, Luxury Hotel Collection, Airport Companion, seguros y acumulación, con otro orden de las viñetas.
- Figma muestra **dos fichas tituladas Visa Platinum Qore**: una comienza «Ideal para quienes buscan experiencias exclusivas y más beneficios al viajar», y otra «Beneficios superiores para clientes con consumos frecuentes». El proyecto tiene una Signature Qore y una Platinum Qore. La referencia no permite asignar de forma inequívoca esos dos textos a los productos del proyecto.
- En la segunda ficha de Platinum Qore de Figma ya no aparecen «Canje de puntos por experiencias y descuentos» ni «Beneficios exclusivos Visa Platinum»; la frase de acumulación termina en «de consumo». En la otra ficha aún aparece «e consumo».
- Visa Oro LATAM Pass, caso A: Figma añade punto final a «Campañas exclusivas para acumular más millas».
- Caso B, Visa Clásica LATAM Pass: Figma no incluye «Skybox gratis por 1 año», que sí está en el proyecto.
- El resto del contenido leído del caso B conserva el wording, salvo las normalizaciones de nombres ya acordadas y la sustitución de Oro Qore por la destacada común en runtime.

## Coincidencias y diferencias deliberadas

- Inicio: coinciden «Hola! 👋 Soy Tarjetín.», el mensaje de ayuda y las opciones, excepto la tilde de «Ayúdame», corregida en el proyecto y todavía ausente en Figma.
- Menú de elección: coinciden las cuatro opciones. Figma conserva «opciones que mejor se adapte»; el proyecto tiene la concordancia aprobada «se adapten».
- Menú de comparación: coinciden la pregunta inicial y las opciones de membresía, exoneración y Priority Pass. Cambia la opción de millas/puntos.
- Resolver dudas: coinciden las tres preguntas y los textos de las tres respuestas, salvo el «¡» inicial ya corregido de q31.
- Coinciden los mensajes introductorios de los casos A y B del perfilador leídos.
- Los montos de la tabla de membresía leídos coinciden; las diferencias de nombres son «AMEX» frente a «American Express» y el «Pass» faltante en Black.
- La tabla de Priority Pass conserva la misma distribución Sí/No observada y no incluye tarjeta recomendada. Se mantienen las diferencias de nombres anteriores.
- Figma conserva «Estos son las millas»; el proyecto tiene «Estas son las millas», corrección registrada.
- Se mantienen como decisiones anteriores: «American Express» completo, «Sapphire», «LATAM», «Infinite», «quieren» en la descripción de viajeros, burbujas del usuario que repiten el botón completo y cierre unificado «Cerrar». Esta revisión no propone revertirlas para calcar una errata o un texto abreviado del mockup.

## Contradicciones y erratas de la referencia

1. Exoneración de Visa Clásica Qore y Visa Oro Qore: la tabla Figma sigue indicando **S/80 al mes** y **S/170 al mes**, pero las fichas dicen **S/1 al mes**. El proyecto ya tiene S/1 por una corrección documentada. Esos dos montos no deberían revertirse por coincidencia literal.
2. American Express Oro en ahorro: **S/80** en la ficha, **S/170** en la tabla de membresía de Figma. El proyecto utiliza S/170.
3. Experiencias: siguen **1.25 millas** para Sapphire y **1.5** para Signature LATAM Pass, diferentes de otros criterios y de la tabla del proyecto. Coincidir en esas frases no resuelve su inconsistencia.
4. Dos fichas con nombre Visa Platinum Qore y ausencia de una identificación inequívoca de Signature Qore en esa secuencia de experiencias.
5. Erratas todavía visibles: «Acumua», «e consumo», «Ideal para acumula», «Puntos Qores», «Shapphire», «se adapte» con sujeto plural y «Estos son las millas». Una variante de la descripción de Platinum Qore termina además con `}`.
6. Persisten textos de Visa Clásica que hablan de millas en ahorro y beneficios, mientras otras partes del proyecto la clasifican sin acumulación de millas.

## Estado de la revisión

Los puntos 1 al 10 quedaron resueltos. Las erratas y contradicciones enumeradas arriba permanecen en la referencia de Figma, pero el proyecto aplica la redacción corregida y los datos del catálogo público.
