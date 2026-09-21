# Seguimiento del proyecto Tarjetín

Este archivo registra las reglas de contenido y el avance de la alineación del bot de `/felicitaciones`.
Debe leerse antes de modificar wording, cifras de producto o recomendaciones.

## Prioridad de fuentes

1. La instrucción explícita más reciente del usuario define el alcance del cambio.
2. Para datos del producto, manda el catálogo público de tarjetas de `viabcp.com`, sección
   **«Elige la Tarjeta de Crédito que va contigo»**. Incluye membresías, consumos para exonerar,
   millas/puntos y beneficios. Su registro local está en
   `contenido/tarjetas-catalogo.json`, campo `_meta.catalogo_publico_viabcp`.
3. Para wording y presentación, manda el Figma
   **5.34.TC.BOT_web**, siempre que no contradiga un dato del catálogo público:
   `https://www.figma.com/design/5M71uw8ny9baMkeQ3145Zm/5.34.TC.BOT_web?node-id=940-2642`.
4. Las correcciones ya aprobadas y sus motivos están en `docs/correcciones-wording.md`.

Cuando Figma y la web pública discrepen en una cifra o beneficio, conservar el dato de la web y
documentar la diferencia. No copiar una contradicción solo para igualar el mockup.

## Cómo aplicar cambios de contenido

- El archivo que se entrega es `adobe-target/piloto/bot.html`.
- Mantener actualizado su espejo correspondiente en `contenido/wordings/`.
- Registrar cada bloque resuelto en `docs/revision-wording-figma-2026-09-21.md`.
- No editar `referencia/bot-actual.html`, `referencia/bot-insertado.html` ni
  `referencia/paginas/*.html`; son evidencia de solo lectura.
- Después de editar, validar el JavaScript del HTML, todos los JSON tocados y `git diff --check`.
- Verificar en `http://127.0.0.1:8000/preview/` el caso de tarjetas afectado.

## Estado de alineación con Figma

1. **Resuelto:** se añadió «solo por digital» a las bienvenidas que lo muestran en Figma.
2. **Resuelto:** la opción de acumulación cambia entre millas, puntos o ambas según las tarjetas.
3. **Resuelto:** q21 muestra dinámicamente las tablas LATAM Pass y Qore.
4. **Resuelto:** q21, q22 y q23 recomiendan Visa Oro Qore; también se ajustaron los espacios.
5. **Resuelto:** q23 usa «Exoneración de membresía por consumo mensual» y valores «al mes».
6. **Resuelto:** las seis diferencias claras de «Ahorrar en costos» se copiaron de Figma.
7. **Resuelto:** se alinearon las descripciones y fichas A/B de «Obtener más beneficios».
8. **Resuelto:** se alinearon las descripciones y beneficios de «Experiencias exclusivas».
9. **Resuelto:** la burbuja flotante dice «¡Hola! Te ayudaré a elegir una tarjeta».
10. **Resuelto:** se normalizaron las erratas y se resolvieron las contradicciones con el catálogo público.

## Decisiones de datos vigentes

- Visa Clásica Qore y Visa Oro Qore exoneran con **S/1 al mes**. Los valores S/80 y S/170 que
  aparecen en una tabla de Figma son sus membresías, no sus consumos de exoneración.
- American Express Oro LATAM Pass mantiene membresía anual de **S/170**. Una ficha de Figma dice
  S/80, pero contradice la tabla y la referencia web.
- Visa Infinite Qore usa **S/5,000 al mes** para exonerar. `S/5,00` era un cero faltante.
- La recomendada Visa Oro Qore conserva la descripción común
  «Orientada a beneficios cotidianos y promociones.» hasta que se pida cambiar esa decisión.
- La fila de Visa Signature Qore usa «Puntos Qore»; «Puntos Qores» era una errata de Figma.
- Las dos fichas que Figma titula «Visa Platinum Qore» contienen acumulación Platinum de 1.5 puntos.
  Son variantes duplicadas, no una ficha de Signature Qore. El proyecto conserva Signature Qore con
  sus datos propios y usa para Platinum la variante limpia «Beneficios superiores…».
- Visa Clásica sin código queda preparada con exoneración de S/50 al mes y una descripción de cashback.
