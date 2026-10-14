# Carmen Vélart - template web editorial

**Versión 8 - nuevas páginas Tienda y Libros, refinamiento visual y responsive.**

Template estático HTML/CSS/JS para una marca de prendas pintadas a mano y personalizadas.

## Dirección visual

La propuesta evita el aspecto de ecommerce genérico y trabaja la marca como un cruce entre galería, estudio de artista y moda de autor: composiciones editoriales, fotografía grande, espacios amplios, jerarquía tipográfica delgada y bloques de color tomados de la identidad visual.

### Paleta aplicada

- Crema: `#F4EDD9`
- Rosa: `#F29BAC`
- Vino: `#670627`
- Carbón: `#26252B`
- Ocre: `#CF8E32`
- Oliva: `#706F52`
- Azul cielo: `#87A5BF`
- Azul marino: `#0A254C`

### Tipografía

- Títulos: Instrument Serif
- Cuerpo / UI: Instrument Sans

### Iconografía

[Phosphor Icons](https://phosphoricons.com) (licencia MIT), empaquetado localmente y **subconjunto** a los 90 glifos que usa el template:

- `assets/fonts/phosphor-bold.woff2` (~11 KB) - flechas y controles de interfaz, clase `ph-bold`.
- `assets/fonts/phosphor-duotone.woff2` (~14 KB) - iconos temáticos dentro de cápsulas, clase `ph-duotone`.

Las declaraciones `@font-face` y las clases de cada icono viven dentro del `<style>` de cada página, que solo incluye los glifos que esa página usa (entre 3 y 15 de los 90 disponibles). Una página sin iconos duotone no declara esa fuente y no la descarga.

Uso: `<i class="ph-duotone ph-paint-brush" aria-hidden="true"></i>`. No hay CDN ni petición externa.

Para añadir un icono que no esté en el subconjunto hay que regenerar los `.woff2` desde el paquete `@phosphor-icons/web`; los duotone dibujan dos capas con `::before` / `::after`, por lo que el `<i>` nunca debe ser `display:flex` ni `grid` (usa `inline-block` + `line-height`, como hacen las cápsulas del CSS).

Las tipografías se cargan desde Google Fonts y tienen fallbacks locales.

## Páginas

- `index.html` - Home editorial
- `coleccion.html` - Galería / colección con filtros
- `producto.html?pieza=colibri` - Detalle dinámico de pieza
- `personaliza.html` - Solicitud de personalización en 3 pasos
- `nosotros.html` - Estudio / filosofía / proceso
- `contacto.html` - Contacto y preparación de mensaje
- `preguntas-frecuentes.html` - FAQ
- `tienda.html` - Piezas listas para comprar (consulta y compra por WhatsApp)
- `libros.html` - Publicaciones de Carmen Vélart
- `404.html` - Error 404

IDs de piezas disponibles para `producto.html?pieza=`:

- `colibri`
- `retrato-floral`
- `alpaca-andina`
- `mariposa`
- `fauno`
- `alpaca-solar`

## Estructura

```text
index.html            cada .html incluye su propio <style> y su propio <script>
coleccion.html
producto.html
personaliza.html
nosotros.html
contacto.html
preguntas-frecuentes.html
404.html
assets/
  brand/     logos y recursos gráficos de la identidad
  img/       fotografías optimizadas en WebP
  fonts/     subconjunto local de Phosphor Icons (.woff2)
```

No hay carpetas `css/` ni `js/`: no queda ningún archivo externo salvo las fuentes de iconos y las imágenes.

### Qué lleva cada página

Cada `<style>` contiene únicamente las reglas cuyos selectores existen en esa página, y cada `<script>` únicamente el comportamiento que esa página necesita.

| Página | CSS | JS |
|---|---|---|
| `index.html` | base + hero + bento + proceso + pasos | núcleo |
| `coleccion.html` | base + rejilla y tarjetas de colección | núcleo + filtros |
| `producto.html` | base + ficha y galería | núcleo + catálogo y render |
| `personaliza.html` | base + formulario por pasos + diálogo | núcleo + resumen + pasos |
| `nosotros.html` | base + manifiesto + valores + atelier | núcleo |
| `contacto.html` | base + panel y formulario + diálogo | núcleo + resumen + contacto |
| `preguntas-frecuentes.html` | base + acordeón | núcleo + acordeón |
| `404.html` | base mínima | ninguno |

El "núcleo" es el año del footer, el estado de la cabecera al hacer scroll, el indicador deslizante de la navbar, el menú móvil, las animaciones de entrada y los enlaces de contacto.

Para editar un estilo compartido (navbar, footer, tipografía) hay que tocar las 8 páginas: el bloque es idéntico en todas, así que una búsqueda y reemplazo sobre los `.html` es suficiente.

## Configurar contacto

Al inicio del `<script>` de cada página hay un bloque `CONFIG`:

```js
const CONFIG = {
  whatsapp: "51999999999",
  email: "hola@tudominio.com",
  instagram: "https://instagram.com/tuusuario"
};
```

Es idéntico en las 7 páginas que lo llevan (`404.html` no tiene script), así que conviene cambiarlo con búsqueda y reemplazo en todos los `.html` a la vez.

Si un canal queda vacío, el sitio no inventa ni publica ese dato: el enlace simplemente no aparece, sin dejar avisos ni marcadores en la página. El formulario genera un resumen local para copiar, descargar o abrir en el canal configurado; no existe backend de envío incluido.

## Antes de publicar

1. Configura los canales reales de contacto en el bloque `CONFIG` de cada página.
2. Revisa y reemplaza cualquier texto comercial que dependa de políticas reales del estudio: precio, tiempos, disponibilidad y cuidados.
3. Retira `noindex, nofollow` de las páginas cuando el dominio y el contenido definitivo estén listos para indexación.
4. Si deseas recibir archivos adjuntos desde el formulario, conecta el sitio a un backend o servicio de formularios.
5. Optimiza/cambia fotografías manteniendo las mismas proporciones para conservar el layout.

## Responsive

La composición tiene ajustes específicos para escritorio, tablet y móvil. Se mantienen gutters consistentes, espacios verticales por sección, alineación de columnas y tarjetas, navegación móvil, imágenes con `object-fit` y animaciones respetando `prefers-reduced-motion`.


## Ajustes V6

**Gutters**

- Los heros usaban `width:100%` en su contenedor, lo que anulaba el margen lateral y pegaba el texto al borde. Ahora todas las secciones comparten el mismo `--gutter`.

**Cajas vacías**

- Las tarjetas de pasos, valores y contacto eran rectángulos translúcidos sin fondo: ahora son superficies reales con degradado, borde, sombra y estado hover.
- El hero se recompuso: la ficha de la pieza dejó de estirarse de lado a lado y los bloques decorativos ya no se pisan entre sí.
- En móvil, el bloque rosa vacío se sustituye por el tejido de marca.
- El marcador de canales sin configurar es una tarjeta punteada, no un bloque blanco.

**Navbar**

- Cápsula flotante que se encoge al hacer scroll (el contenedor también se estrecha).
- Píldora vino que se desliza entre los enlaces según el hover y vuelve al de la página actual; degrada a un marcado estático sin JS.
- CTA con relleno ascendente y flecha animada; menú móvil sobre panel flotante.

**Cards**

- Colección, bento, pasos, valores, relacionadas, FAQ y opciones de prenda: contenedor con degradado, borde luminoso, cápsula de icono, elevación y revelado en hover.
- Campos de formulario con superficie, foco visible y selector de archivo estilizado.

**Correcciones de marcado heredadas**

- El acordeón de FAQ se estilaba con `.accordion-item > button`, pero el botón vive dentro de un `<h2>`; nunca se aplicaba.
- La ficha de producto asignaba la columna de 88 px a la imagen principal en vez de a las miniaturas.
- El tejido del footer y del 404 se funde con el fondo en lugar de cortarse en seco.


## Ajustes V7

- El CSS y el JS dejaron de ser archivos globales: cada `.html` embebe su `<style>` y su `<script>` con lo que esa página realmente usa.
- El reparto se hizo comprobando en el navegador qué selectores existen en cada página, y el resultado se verificó píxel a píxel contra la versión anterior en escritorio y móvil: el render es idéntico.
- En el camino se cayeron reglas muertas heredadas de versiones anteriores (`.step-label`, `.mini-process`, `.accordion-item > button`, `.contact-placeholder`, entre otras).
- El diálogo de resumen estaba duplicado en 7 páginas pero solo se abre desde los formularios de `personaliza` y `contacto`: se quitó de las otras cinco.
- Se eliminó el aviso "Canales por configurar" de la página de contacto. Sin canales configurados, esa zona simplemente no muestra nada.
- Las etiquetas de formulario se estilaban con `.field label`, pero el `<label>` **es** el `.field`: nunca se aplicaba. Ahora tienen su tamaño y peso previstos.


## Ajustes V8

**Navegación**

- La navbar, el menú móvil y el footer suman **Tienda** y **Libros**: Colección · Tienda · Libros · Personaliza · El estudio · Contacto.
- Con seis enlaces la cápsula se compacta por debajo de 1320 px; el menú hamburguesa sigue entrando en 980 px. El panel móvil hace scroll si no cabe en la pantalla.
- El footer reparte «Explora» en dos columnas.

**Páginas nuevas**

- `tienda.html`: portada con sello «1/1», filtros, tarjetas con «Pieza única», precio «a consultar» y botón **Lo quiero**, que abre WhatsApp con el nombre de la pieza en el mensaje. Si `CONFIG.whatsapp` está vacío, el botón lleva a `contacto.html`. Incluye «Así compras» (3 pasos) y un cierre hacia Personaliza.
- `libros.html`: portada azul con libros en volumen (tapa, lomo y canto hechos con CSS sobre las fotos del estudio), tarjetas «Próximamente» con **Avísame cuando salga** (WhatsApp o contacto) y una cita final.
- Los títulos, descripciones y precios de Tienda y Libros son de ejemplo: cámbialos en el HTML de cada tarjeta antes de publicar.

**Home**

- Nuevo bloque «Nuevo en el universo Vélart» con dos tarjetas grandes hacia Tienda y Libros.

**Refinamiento visual**

- Los títulos de portada ocupan 3 líneas en lugar de 5 (medida editorial y tamaño ajustado en todas las páginas).
- `text-wrap: balance` en títulos y `pretty` en párrafos, para evitar palabras huérfanas.
- Imágenes de portada con sombra suave y zoom lento al pasar el cursor; tarjetas con elevación.

**Responsive**

- Móvil: títulos más grandes y legibles; los sellos del hero de la home van en una sola fila deslizable; en la tarjeta «Más historias» el texto ya no se monta sobre el tejido.
- Verificado sin scroll horizontal en 390, 820, 1024, 1280 y 1440 px.
