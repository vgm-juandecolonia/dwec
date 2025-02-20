# Proyecto DWEC

## Apuntes

## Módulos (ESM: ECMAScript Modules)

### ¿Qué son los módulos (ESM)?

Uno de los principales problemas que ha ido arrastrando Javascript desde sus inicios es la **dificultad de organizar** de una forma adecuada una aplicación grande, con muchas líneas de código. En muchos lenguajes de programación, cuando un programa crece, se comienza a estructurar en **funciones**. Posteriormente, se traslada a **clases**, que contienen variables (_propiedades_) y funciones (_métodos_). De esta forma organizamos de forma más lógica el código de nuestro programa. Sin embargo, no será suficiente.

Tener todo el código en **un sólo fichero Javascript** se vuelve confuso y complejo en cuanto el código crece. En la mayoría de los lenguajes de programación, el código se divide en ficheros diferentes de modo que, por ejemplo, **cada clase** está localizada **en un fichero separado**. De esta forma, todo queda mucho más organizado e intuitivo, y es fácil de localizar, cambiar y mantener, aunque crezca con el tiempo.

> Además, en el Javascript de cliente (_navegador_) esto se complica un poco más, ya que presenta algunas problemáticas que no existen en otros lenguajes de programación. Cuando accedemos a una página o webapp, estamos accediendo a un servidor, desde donde se está **descargando** el código Javascript (_hacia nuestro navegador_). Una vez descargado, se **ejecuta** en nuestro cliente.

#### Historia de los módulos

Al principio, **Javascript no tenía ninguna forma** de cargar código desde otros archivos Javascript. Todo el Javascript se tenía que escribir en un sólo archivo `.js`. La única forma de separar en diferentes archivos de forma nativa y oficial, era incluir varias etiquetas `<script>` desde nuestra página HTML.

De esta manera, podíamos tener varios ficheros Javascript separados, cada uno para una finalidad concreta (_librerías, funcionalidades, etc..._). Sin embargo, este sistema tenía varias **desventajas**: resultaba lento, era complejo interactuar entre archivos, sobrecargaba al cliente con múltiples peticiones que podían bloquear la carga de la página, etc...

Con el tiempo, se desarrollaron sistemas **no oficiales** que permitían crear módulos para organizar en varios archivos:

-   **CommonJS**: Un sistema de módulos adaptado y popularizado por NodeJS
-   **AMD**: Un sistema de módulos, donde la más popular fue _RequireJS_.
-   **UMD**: Un sistema de módulos que unificaba sistemas de módulos como CommonJS y AMD.

#### ¿Qué son los módulos ES?

A partir de **ECMAScript** se introduce una característica nativa denominada **Módulos ES** (_ESM_), que permite la importación y exportación de fragmentos de datos entre diferentes ficheros Javascript, eliminando las desventajas que teníamos hasta ahora y permitiendo trabajar de forma más flexible en nuestro código Javascript.

Para trabajar con **módulos** tenemos a nuestra disposición las siguientes palabras clave:

| Declaración | Descripción                                                                                    |
| ----------- | ---------------------------------------------------------------------------------------------- |
| `export`    | Pone los datos indicados (variables, funciones, clases...) a disposición de otros ficheros     |
| `import`    | Incorpora datos (variables, funciones, clases...) desde otros ficheros `.js` al código actual. |
| `import()`  | Permite importar módulos de forma más flexible, en tiempo real (imports dinámicos).            |

Mediante la palabra clave `export` crearemos lo que se llama un módulo de exportación que contiene datos. Estos datos pueden ser variables, funciones, clases u objetos más complejos (_a partir de ahora, elementos_). Si dicho módulo ya existe, podremos ir añadiendo más propiedades.

Por otro lado, con la palabra clave `import` podremos leer dichos módulos exportados desde otros ficheros y utilizar sus elementos en el código de nuestro fichero actual.

Veamos un ejemplo sencillo para ver el funcionamiento de `import` y `export` en su modo más básico. Tenemos un fichero `constants.js` donde vamos a exportar una constante numérica:

```js
// Fichero constants.js
export const magicNumber = 42;

// Fichero index.js
import { magicNumber } from "./constants.js";

console.log(magicNumber); // 42
```

Desde `index.js` nos traemos esa constante numérica `magicNumber` de `constants.js` para utilizarla. Obviamente, esto es sólo la modalidad básica de importación y exportación de elementos, pero existen múltiples modalidades, matices y diferencias que iremos viendo en los siguientes artículos.

#### Antes de usar módulos

Antes de empezar, recuerda que para poder utilizar `export` o `import` en nuestro código Javascript que se ejecuta directamente en el navegador, debemos cargar el fichero `.js` con la etiqueta y atributo `<script type="module">` para indicarle que utilizaremos módulos. Si no lo hacemos, obtendremos el siguiente error:

> Uncaught SyntaxError: Cannot use import statement outside a module

Al añadir el atributo `type="module"` a nuestra etiqueta `<script>` estaremos avisando al navegador que estamos cargando un módulo en el que podemos utilizar `import` y `export`:

```html
<script type="module">
    import { nombre } from "./file.js";
</script>
```

Algunos frameworks utilizan automatizadores que pueden «oscurecer» esto, ya que puede parecer que no es necesario, ya que los automatizadores lo hacen internamente, o convierten a otros sistemas de módulos, como **CommonJS** (NodeJS).

> Si este mensaje te aparece en Node, tienes que abrir el fichero `package.json`, añadir la propiedad `"type": "module"`, que sería algo equivalente a esto, pero en NodeJS.

### Exportar módulos

Como hemos mencionado anteriormente, un **módulo de exportación** es un elemento que pone a disposición de otros ficheros Javascript, datos o código que tenemos en el fichero actual. Esto puede ser algo muy interesante de cara a organizar nuestro código en diferentes ficheros, reutilizarlos y facilitar la tarea de mantenerlo.

#### Exportación de módulos

Por defecto, un fichero Javascript no tiene **módulo de exportación** si no se usa un `export` al menos una vez en su código. Si se usa al menos un `export`, entonces tendrá un objeto llamado **módulo de exportación**, donde puede tener uno o múltiples datos. Existen varias formas de exportar datos mediante la palabra clave de Javascript `export`:

| Forma                        | Descripción                                                                       |
| ---------------------------- | --------------------------------------------------------------------------------- |
| `export ...`                 | Declara un elemento o dato, a la vez que lo añade al módulo de exportación.       |
| `export { name }`            | Añade el elemento `name` al módulo de exportación.                                |
| `export { name as newName }` | Añade el elemento `name` al módulo de exportación con el nombre `newName`.        |
| `export { n1, n2, n3... }`   | Añade los elementos indicados ( `n1` , `n2` , `n3` ...) al módulo de exportación. |
| `export * from "./file.js"`  | Añade todos los elementos del módulo de `file.js` al módulo de exportación.       |
| `export default ...`         | Declara un elemento y lo añade como módulo de exportación por defecto.            |

Además, como veremos más adelante, es posible renombrar los elementos exportados utilizando `as` seguido del nuevo nombre a utilizar. Recuerda que no es posible usar `export` dentro de funciones, bucles o contextos específicos.

#### Declaración y exportación

Existen varias formas de exportar elementos. La más habitual, quizás, es la de simplemente añadir la palabra clave `export` a la izquierda de la declaración del elemento Javascript que deseamos exportar, ya sea una variable, una constante, una función, una clase u otro objeto más complejo:

```js
export let number = 42; // Se añade la variable number al módulo
export const hello = () => "Hello!"; // Se añade la función hello al módulo
export class CodeBlock {} // Se añade la clase vacía CodeBlock al módulo
```

Probablemente, es de las más sencillas porque sólo hay que añadir `export` a la izquierda. Sin embargo, podría ser fácil perderse en el código y no tener claro rápidamente que ha sido exportado y que no.

> En este ejemplo, nuestro **módulo de exportación** contendría los elementos `number`, `hello` y `CodeBlock`.

#### Exportación post-declaración

Si vienes del mundo de NodeJS, es muy probable que te resulte más intuitivo exportar módulos al final del fichero, ya que es como se ha hecho siempre en Node con los `module.exports`. Esta forma tiene como ventaja que es mucho más fácil localizar la información que ha sido exportada, ya que siempre estará al final del fichero.

Así pues, primero declarariamos la información y posteriormente, al final del fichero, exportamos lo que queramos:

```js
let number = 42;
const hello = () => "Hello!";
const goodbye = () => "¡Adiós!";
class CodeBlock {}

export { number }; // Se crea un módulo y se añade number
export { hello, goodbye as bye }; // Se añade saludar y despedir al módulo
export { hello as greet }; // Se añade otroNombre al módulo
```

> En este ejemplo, nuestro **módulo de exportación** contendría los elementos `number`, `hello`, `bye` y `greet`.

Otra forma de hacerlo, sería utilizar un único `export` y englobar todo lo que queramos exportar en el objeto:

```js
let number = 42;
const hello = () => "Hello!";
const goodbye = () => "¡Adiós!";
class CodeBlock {}

export { number, hello, goodbye as bye, hello as greet };
```

En este último ejemplo estaríamos exportando exactamente lo mismo que en el ejemplo anterior, sólo que lo hemos hecho de una sola vez, creando el objeto con las propiedades deseadas.

#### Exportación externa

Esta modalidad es menos frecuente, pero puede ser interesante en algunas ocasiones. Se trata de añadir a nuestro **módulo de exportación** del fichero actual, todos los elementos exportados en el fichero `math.js`:

```js
// CASO 1: Exporta todo lo exportado en el fichero math.js (abs, min, max, random)
export * from "./math.js";
```

```js
// CASO 2: Exporta sólo abs, min y max del fichero math.js
export { abs, min, max } from "./math.js";
```

En el segundo ejemplo, realizamos una exportación más selectiva, donde en lugar de todos los elementos exportados, sólo lo hacemos con `abs`, `min` y `max`. Estos elementos formarán parte de nuestro **módulo de exportación** del fichero actual, aunque estos elementos se encuentren en otros ficheros.

También es posible utilizar el `as` tras el `*` para renombrar el conjunto de elementos, en cuyo caso, se creará un objeto con el nombre indicado, donde se incorporarán todos los elementos del fichero `math.js`:

```js
// CASO 3: Exporta todo lo exportado en el fichero math.js en un objeto con nombre
export const number = 42;
export * as math from "./math.js";
```

> En este caso, nuestro módulo de exportación tiene un elemento `number` y un elemento `math`, que es un objeto que a su vez incluye los elementos `abs`, `min`, `max` y `random`, que hemos traído de `math.js`.

#### Exportación por defecto

Ten en cuenta que al exportar elementos y crear un **módulo de exportación**, lo que realmente creamos es un objeto donde las propiedades son los nombres de los elementos (_constantes, variables, funciones, clases..._). Existe una modalidad de exportación que es conocida como exportación por defecto.

Para realizarla, sólo tenemos que añadir la palabra clave `default` tras el `export`. En ese caso, estaremos creando un elemento en nuestro módulo de exportación que tendrá el nombre `default` y será considerado el elemento principal (_elemento por defecto_). Sólo puede haber un elemento llamado `default` por módulo de exportación, por lo que tampoco se puede hacer más de un `export default` por fichero.

Observa este ejemplo:

```js
export const number = 42; // Declaración y exportación
export default "Manz"; // Exportación por defecto
```

> En este caso, nuestro módulo de exportación tiene los elementos `number` y `default`, este último el `"Manz"`, que sería el elemento `por defecto` de nuestro módulo de exportación.

Observa que si utilizamos el `export default`, no es posible declarar la variable con `var`, `let` o `const`, puesto que ese nombre sería inútil. Lo que si podríamos hacer es exportarlo posteriormente, indicando su nombre:

```js
const number = 42;
const name = "Manz";

export default name;
```

> En este caso, nuestro módulo de exportación tiene sólo el elemento `default`, que es el `"Manz"`.

Hasta aquí, hemos aprendido a exportar elementos y añadirlos al **módulo de exportación** del fichero Javascript en el que nos encontramos. En el siguiente apartado, veremos las diferentes formas de importar estos elementos desde otro fichero.

#### Exportación CommonJS (legacy)

Es posible que en algún fragmento de código te hayas encontrado una sintaxis muy similar al `export`, pero utilizando `module.exports` como en el código siguiente:

```js
const number = 42;
const name = "Manz";

module.exports = {
    number,
    name,
};
```

Este sistema no forma parte de los **módulos oficiales de ECMAScript** que se explican en este artículo, sino que es un sistema de módulos utilizado en **NodeJS** (_Javascript de servidor_) conocido como **CommonJS** (_CJS_). El `module.exports` no funciona en navegadores de forma nativa, por lo que se desaconseja su uso a favor de `export`, ya que incluso las últimas versiones de Node lo soportan.

Si lo estás utilizando en un código de frontend, es muy posible que se esté utilizando junto a NodeJS o haya alguna herramienta o automatizador que utilice NodeJS y esté transpilando (_convirtiendo_) tu código.

### Importar módulos

En los artículos anteriores, hemos visto como crear **módulos de exportación** mediante `export`. En el artículo actual vamos a ver como realizar la operación inversa, utilizar la palabra clave `import` para cargar elementos de **módulos de exportación** de ficheros externos y utilizarlos en el fichero actual.

#### Importación de módulos

En Javascript, podemos utilizar `import` para hacer la operación inversa a `export`. Si habíamos mencionado que con `export` ponemos datos o elementos de un fichero `.js` a disposición de otros, con `import` podemos cargarlos y utilizarlos en el código de nuestro fichero actual.

Existen varias formas de importar código utilizando la palabra clave `import`:

| Forma                                            | Descripción                                                    |
| ------------------------------------------------ | -------------------------------------------------------------- |
| `import { nombre } from "./file.js"`             | Importa el elemento `nombre` de `file.js`.                     |
| `import { nombre as newName } from "./file.js"`  | Importa el elemento `nombre` de `file.js` como `newName`.      |
| `import { n1, n2... } from "./file.js"`          | Importa los elementos indicados desde `file.js`.               |
| `import nombre from "./file.js"`                 | Importa el elemento por defecto de `file.js` como `nombre`.    |
| `import * as name from "./file.js"`              | Importa todos los elementos de `file.js` en el objeto `name`.  |
| `import "./file.js"`                             | Ejecuta el código de `file.js`. No importa ningún elemento.    |
| `import { name } from "https://web.com/file.js"` | Descarga el fichero e importa el elemento `name` de su módulo. |

Al igual que hacíamos en la exportación, también puedes renombrar elementos con `import` utilizando `as` seguido del nuevo nombre. Los `import` deben hacerse siempre desde las primeras líneas del fichero Javascript y no se pueden incluir dentro de bucles, funciones o determinados contextos.

#### Importación con nombre

La forma más habitual de importar elementos es a través de la denominada importación nombrada, donde utilizamos la palabra clave `import` indicando el nombre de los elementos a importar en el interior de las llaves `{ }`, todo ello desde el **módulo de exportación** del fichero `file.js`.

```js
import { nombre } from "./file.js";
import { number, element } from "./file.js";
import { brand as brandName } from "./file.js";
```

En este fragmento de código se realizan 3 importaciones:

-   En la primera línea, estamos importando el elemento `nombre` desde el **módulo** del fichero `file.js`.
-   En la segunda línea, importamos varios elementos: `number` y `element`.
-   En la tercera línea, importamos el elemento `brand`, renombrándolo a `brandName`.

Todos ellos, deben haber sido exportados en el **módulo de exportación** del fichero `file.js`.

#### Importación por defecto

Hasta ahora, hemos utilizado **importaciones con nombre**, donde indicamos en todo momento el nombre de los elementos que queremos importar. Sin embargo, si hemos visto el artículo de export en Javascript habremos comprobado que es posible **exportar/importar elementos por defecto**.

Una importación por defecto lo único que hace es buscar el elemento llamado `default` e importarlo con el nombre indicado en el `import`:

```js
import nombre from "./math.js";
```

Observa que en este caso, la diferencia es que no hemos indicado las llaves `{ }` al indicar el nombre del elemento, lo que hará que importe el elemento `default` y lo renombre a `nombre`. En el caso de que no exista ninguna propiedad `default` en el módulo de exportación se generará un objeto vacío.

> Las **importaciones por defecto** suelen estar ligeramente mal vistas por algunos desarrolladores. Una **exportación nombrada** suele ser más intuitiva y predecible a la hora de utilizar en nuestro código.

#### Importación masiva

Otra modalidad de importación interesante es aquella donde podemos hacer una **importación masiva**. Es decir, si utilizamos el símbolo `*` a la hora importar, estaremos indicando que se deben cargar todos los elementos del **módulo de exportación** del fichero indicado.

En esta modalidad, es obligatorio utilizar el `as` seguido del nombre del elemento, ya que debemos indicar un nombre para crear un **objeto** que contendrá todos los elementos importados:

```js
import * as module from "./file.js";
```

> En este caso, creamos un `module` que incluye todos los elementos del **módulo de exportación** de `file.js`.

#### Importación de código

Existe una última forma de importar código que no es tan frecuente encontrarla, al menos en el mundo de los frameworks Javascript. Sin embargo, si se utiliza bastante en el mundo de los Web Components.

Se trata de la **importación de código** sin importar elementos, simplemente ejecutando el código del fichero indicado:

```js
import "./math.js";
```

Si realizamos un `import` donde únicamente establecemos el fichero a importar, lo que estaremos haciendo es indicar que el navegador debe leer el código de ese fichero y procesarlo, sin importar ningún elemento como en los casos anteriores.

A efectos prácticos, esto sería exactamente lo mismo que si tuvieramos el código de ese otro fichero en el fichero actual, no obstante, de esta forma se puede organizar y separar en ficheros diferentes.

#### Importaciones remotas

Aunque en los ejemplos anteriores siempre indicamos ficheros `.js` locales, también es posible indicar **ficheros remotos**, es decir, que estén en un dominio diferente al nuestro:

```js
import { ceil } from "https://unpkg.com/lodash-es@4.17.21/lodash.js";
```

Por ejemplo, en este caso, estamos descargando desde el dominio unpkg.com la librería lodash.js, desde la cuál importaremos el elemento `ceil` de su módulo de exportación.

Hay que tener en cuenta varias cosas de las **importaciones remotas**:

-   **Disponibilidad**: Ten en mente que al hacer una importación remota dependemos del dominio indicado. Si dicho dominio no está disponible o no podemos conectar a él, no podremos descargar el fichero ni procesarlo, por lo que puede ser conveniente tener esos ficheros en nuestro sitio web.

-   **Descarga**: Para importar el elemento del módulo indicado, primero es necesario descargar el fichero, por lo que si la velocidad de conexión es lenta y el tamaño del fichero `.js` es grande, puede ralentizar la carga de la página.

-   **ECMAScript modules**: Ten en cuenta que para poder hacer importaciones de este tipo, es necesario que la librería utilice los **módulos ESM**. Existen repositorios como cdnjs.com, jsdelivr.com o skypack.dev donde puedes encontrar librerías y proyectos Javascript subidos a un **CDN**, listos para utilizar.

#### Importación CommonJS (legacy)

Es posible que en algún fragmento de código te hayas encontrado una sintaxis parecida a `import`, pero utilizando `require` como en el código siguiente:

```js
const library = require("library");
```

Este sistema no forma parte de los **módulos oficiales de ECMAScript** que se explican en este artículo, sino que es un sistema de módulos utilizado en **NodeJS** (_Javascript de servidor_) conocido como **CommonJS** (_CJS_). El `require()` y el `module.exports` no funcionan en navegadores de forma nativa, por lo que se desaconseja su uso a favor de `export`, ya que incluso las últimas versiones de Node lo soportan.

Si lo estás utilizando en un código de frontend, es muy posible que se esté utilizando junto a NodeJS o haya alguna herramienta o automatizador que utilice NodeJS y esté transpilando (_convirtiendo_) tu código.

## NodeJS

### ¿Qué es NodeJS?

En 2009, aparece un **entorno multiplataforma** llamado **NodeJS**. La idea era sencilla: Dada la popularidad en ascenso de Javascript, que en ese momento sólo era posible utilizarlo dentro de un navegador, se ideó un sistema que «sacara» Javascript del navegador, haciendo posible ejecutarlo en cualquier lugar fuera de él, por ejemplo, en un servidor web, convirtiéndolo en un lenguaje de servidor y no sólo un lenguaje de cliente (_navegador_) como hasta el momento.

#### Requisitos previos

**NodeJS** es un entorno multiplataforma, por lo que da igual si se trata de un sistema operativo Windows, Mac o GNU/Linux, NodeJS es capaz de ejecutarse en cualquiera de ellos. Sin embargo, y de forma totalmente subjetiva por quién escribe estas lineas, si estás utilizando un sistema operativo **Windows**, te recomiendo tomarte un tiempo en instalar y configurar **WSL**.

**WSL** (_Windows Subsystem for Linux_) es un sistema que te permite disponer de una terminal de Linux como si fuese nativa, lo que simplifica muchísimo el desarrollo web, ya que en la mayoría de los casos, terminamos creando desarrollos para subirlos a máquinas Linux.

#### Instalación de Node

Javascript viene integrado en los navegadores, sin embargo, para utilizar NodeJS necesitaremos instalarlo en nuestro sistema. Esto se puede hacer desde la web oficial de NodeJS, donde se instalará de una forma u otra dependiendo del sistema operativo que utilices.

Existen varias formas de instalar NodeJS:

-   Instalar Node mediante PNPM utilizando WSL
-   Instalar NodeJS con NVM utilizando WSL
-   Instalar NodeJS mediante winget/fnm, que no requiere WSL

Antes de continuar, te recomiendo abrir una terminal y escribir el siguiente comando:

```powershell
$ node --version
v20.9.0
```

Si el comando anterior te da un error o te devuelve una versión inferior a `v18`, te recomiendo revisar los enlaces anteriores y actualizar tu NodeJS, pues que algunas de las cosas que veremos pueden no estar disponibles para tu versión instalada.

#### Instalación de NPM

**NodeJS** incorpora un sistema instalador de paquetes llamado **NPM**, que nos permitirá instalar las librerías de terceros que necesitemos para desarrollar nuestra aplicación de forma sencilla y rápida.

-   PNPM: Un instalador paralelo mejorado de NPM
-   Yarn: Un instalador de NPM mejorado.
-   NPM: El instalador oficial de NodeJS (_ya viene instalado con NodeJS_)

Nuevamente, para comprobar que todo está bien instalado y funcionando correctamente, escribe en una terminal:

```powershell
$ npm --version
10.1.0
```

Si te devuelve una versión menor a 10, te recomiendo actualizarlo.

> Si estás empezando, da igual cuál uses. Incluso, es mejor NPM, que viene por defecto. Con el tiempo, probablemente NPM te parezca muy lento y quieras dar el salto a PNPM. Al principio, no importa.

### Primeros pasos con NodeJS

Para empezar con Node, si ya te has asegurado que cumples los requisitos necesarios, vamos a crear nuestro primer ejemplo en NodeJS, para aprender a escribir algunos fragmentos de código Javascript sencillos y ejecutar nuestros primeros scripts.

#### Inicializa el proyecto

En primer lugar, vamos a crear una nueva carpeta `node-project` e inicializar un proyecto utilizando el gestor de paquetes `npm` (_o cualquier otro que desees_):

```powershell
mkdir node-project
cd node-project

# Estrictamente, sólo obligatorio si necesitamos instalar paquetes npm
npm init -y
```

Es posible que posteriormente, al ejecutar nuestro primer ejemplo nos aparezca un error similar al siguiente:

> SyntaxError: Cannot use import statement outside a module

En estos tutoriales vamos a utilizar **NodeJS moderno**, por lo que si obtienes este error al ejecutar tu `index.js`, simplemente asegúrate de que existe un `"type": "module"` en tu archivo `package.json`, y si no es así, añadelo. También es posible saltarse este error renombrando la extensión de `.js` a `.mjs`, aunque es preferible utilizar la primera solución en lugar de esta última.

> También es conveniente crear un archivo `jsconfig.json`, con un objeto vacío `{}`, ya que en algunas ocasiones puede que no funcione correctamente el autocompletado de VSCode si no existe.

#### Mi primer ejemplo en NodeJS

Ahora que tenemos nuestra carpeta y proyecto creado, vamos a crear un fichero `index.js` donde escribiremos un pequeño fragmento de código Javascript. Lo habitual es tener instalado **Visual Studio Code**. Una vez lo tenemos instalado, podemos ejecutar el comando `code` para abrirlo en la carpeta actual:

```powershell
code index.js
```

En el archivo vamos a escribir un código Javascript que va a importar las constantes del sistema operativo `hostname` y `type`, que nos dan el nombre del sistema y el tipo de máquina que es (_Linux, Windows, etc..._).

Observa también que a la hora de importar los módulos, indicamos `node:` seguido del nombre del módulo que queremos importar. Esto es la manera recomendada de importar módulos que sean específicos de Node:

```js
import { hostname, type } from "node:os";

console.log(`¡Hola mundo! Ejecutando en ${hostname} bajo ${type}`);
```

Ahora, si vamos a una terminal y escribimos `node index.js`, ejecutaremos nuestro script y node lo ejecutará. Asegúrate de estar en la ruta de la carpeta que hemos creado:

```powershell
$ node index.js
¡Hola mundo! Ejecutándose en MANZDEV bajo Linux
```

¡Ya tenemos nuestro primer script en NodeJS! Observa que en mi caso, aún usando Windows, me aparece `bajo Linux` porque estoy usando WSL y a efectos prácticos se trata de un Linux dentro de Windows.

#### Vigilando cambios: `node --watch`

Observa que cada vez que escribamos `node index.js` se ejecutará el código y nos mostrará el resultado. Esto está muy bien, pero puede ser algo incómodo a la hora de desarrollar.

Vamos a ampliar nuestro ejemplo y a utilizar una característica llamada `--watch`, que se encarga de ejecutar el script **cada vez que el código cambia**:

```js
import { uptime } from "node:os";

const hours = Math.floor(uptime() / 60 / 60);
const RTF = new Intl.RelativeTimeFormat("es-ES");
const time = RTF.format(-hours, "hours");

console.log(`¡Hola mundo! Ejecutando en un sistema encendido ${time}`);
```

En este caso, observa que hemos hecho varias cosas:

-   Importamos `uptime`, una función del sistema operativo `node:os`
-   Dicha función nos devuelve el tiempo que lleva la máquina encendida (_en segundos_)
-   Creamos una constante `hours` que convierte los segundos en horas y los redondea
-   Creamos una constante `RTF`, que es un objeto RelativeTimeFormat. Este objeto formateará números en tiempos relativos (_hace X_, _dentro de X..._).
-   Creamos una constante `time` que donde formateamos las `hours` y las guardamos como .

Ahora, vamos a ejecutar nuestro script, colocando el parámetro `--watch` para que vigile los cambios. Ahora, si hacemos modificaciones en nuestro código y guardamos con `CTRL` + `S`, comprobaremos que se vuelve a ejecutar el código sólo, haciéndolo mucho más cómodo para el programador:

```powershell
$ node --watch index.js

¡Hola mundo! Ejecutando en un sistema encendido hace 91 horas
Completed running 'index.js'
```

Observarás que si todo ha ido bien, cada vez que guardes los cambios del código, aparecerá un mensaje `Restarting 'index.js'`, que indica que ha detectado cambios y vuelto a ejecutar el script.

> Antiguamente, se solía utilizar nodemon para este propósito. Sin embargo, ya no es necesario, puesto que podemos utilizar `--watch`.

### Sistema de ficheros (fs)

Antes de meternos en algún proyecto más avanzado con Node, vamos a aprender a leer ficheros de texto utilizando la API `fs` de NodeJS. Como siempre, la importaremos utilizando `node:fs` y utilizaremos un método llamado readFile.

Vamos a comenzar teniendo en cuenta que tenemos un fichero llamado `file.txt` que contiene varias líneas de texto con una frase en cada una.

#### El método `readFile` / `readFileSync`

En NodeJS podemos utilizar dos formas de leer un fichero, `readFile` (_versión asíncrona_) y `readFileSync` (_versión síncrona_). Aquí puedes ver un ejemplo sencillo que lee el contenido del fichero de texto:

```js
// Utilizamos readFile (asíncrono)

import { readFile } from "node:fs/promises";
import { styleText } from "node:util";

const FILE = "./file.txt";

const fileContent = await readFile(FILE, { encoding: "utf-8" });
const lines = fileContent.trim().split("\n");

console.log(styleText("magenta", "El contenido del fichero:"));
console.log(lines);
```

```js
// Utilizamos readFileSync (síncrono)

import { readFileSync } from "node:fs";
import { styleText } from "node:util";

const FILE = "./file.txt";

const fileContent = readFileSync(FILE, { encoding: "utf-8" });
const lines = fileContent.trim().split("\n");

console.log(styleText("magenta", "El contenido del fichero:"));
console.log(lines);
```

Observa que al usar `readFile()` le pasamos la ruta del fichero, así como un objeto de opciones donde le indicamos la codificación del fichero (_generalmente, utf-8_). En la siguiente línea hacemos un `trim()` para eliminar los espacios sobrantes al principio o final del texto.

Finalmente, dividimos el contenido por cada línea, separando por `\n`, es decir, por los **saltos de línea** (_ENTER_), y los mostramos por pantalla.

Observa las diferencias de la versión síncrona, que se llama `readFileSync()` y la versión asíncrona, donde se usa `await` y se importa de `node:fs/promises`.

> Aunque tenemos un método asíncrono `readFile` en `node:fs`, funciona mediante callbacks, y hoy en día, lo ideal sería utilizar el `readFile` asíncrono de `node:fs/promises`, ya que tiene integración con promesas y es mucho más cómodo.

De la misma forma que tenemos un `readFile()` y un `readFileSync()`, también tenemos un `writeFile()` y un `writeFileSync()` que funcionan de forma muy parecida y siguen la misma lógica. En este caso, el primer parámetro es la ruta del fichero, el segundo parámetro es el contenido que quieres guardar en el fichero, y el tercero es el objeto de opciones.

#### El método `readdir()`

Con el método `readdir()` podemos leer los archivos (_y carpetas_) que contiene una ruta indicada. Dicha ruta la obtenemos con el método `cwd()`, que nos devuelve la ruta actual donde nos encontramos al ejecutar el programa.

El método `readdir()` nos devuelve un array de con los archivos y carpetas de la ruta, en el segundo parámetro podemos indicarle un valor `recursive` para que revise las subcarpetas interiores y `withFileTypes` para que devuelva un objeto con partes separadas en lugar de un :

```js
import { cwd } from "node:process";
import { readdir } from "node:fs/promises";
import { styleText } from "node:util";

const CURRENT_PATH = cwd();
const files = await readdir(CURRENT_PATH, {
    recursive: false,
    withFileTypes: true,
});

const getIcon = (entry) => {
    if (entry.isDirectory()) return "📁";
    else if (entry.isFile()) return "📄";
    else return "❓";
};

console.log(styleText("magenta", `El contenido de la carpeta`), ":");
console.log(CURRENT_PATH);

files.forEach((entry) => {
    const icon = getIcon(entry);
    const { name } = entry;

    console.log(styleText("yellow", "- "), `${icon} ${name}`);
});
```

```powershell
$ node index.js
El contenido de la carpeta: /node-examples/fs-promises
-  📄 index.js
-  📄 jsconfig.json
-  📁 node_modules
-  📄 package.json
-  📄 pnpm-lock.yaml
-  📁 scripts
```

Al final del código, observa que hacemos un `forEach` de `files` obteniendo un icono y un nombre, extraído de cada entrada de `readdir()`. Observa que el método `getIcon()` utiliza métodos como `.isDirectory()` o `.isFile()` para saber si es un fichero o una carpeta. Con `.name` obtenemos el nombre del fichero y con `.path` obtenemos la ruta, entre otras cosas.

#### El método `lstat()`

Si necesitamos más información de un fichero o carpeta, podemos utilizar el método `lstat()`. Por ejemplo, podemos obtener el tamaño del fichero con `.size` o la fecha de modificación con `.mtime`. En este fragmento de código, obtenemos esa información en la constante `stat`, y los mostramos por pantalla en forma de columna:

```js
import { cwd } from "node:process";
import { readdir, lstat } from "node:fs/promises";
import { styleText } from "node:util";

const CURRENT_PATH = cwd();
const files = await readdir(CURRENT_PATH, { recursive: false, withFileTypes: true });

console.log(styleText("magenta", `El contenido de la carpeta`), `: ${CURRENT_PATH}`);
const RTF = new Intl.RelativeTimeFormat("es-ES");

files.forEach(async (entry) => {
  const stat = await lstat(`${entry.path}/${entry.name}`);

  const sizes = [20, 30, 15];
  const colors = ["white", "yellow", "green"];
  const columns = [entry.name, stat.mtime.toISOString(), stat.size + " bytes"];

  const line = [];
  columns.forEach((column, index) => {
    line.push(styleText(colors[index], column.padStart(sizes[index], " ")));
  });
  console.log(...line);
});`
```

```powershell
El contenido de la carpeta : /node-examples/fs-promises
            index.js       2024-06-25T19:14:52.614Z       800 bytes
       jsconfig.json       2024-06-25T12:51:32.470Z         3 bytes
        node_modules       2024-06-25T12:50:16.020Z      4096 bytes
        package.json       2024-06-25T12:50:16.020Z       240 bytes
      pnpm-lock.yaml       2024-06-25T12:50:16.020Z        93 bytes
             scripts       2024-06-25T16:36:37.346Z      4096 bytes
```

#### Otros métodos interesantes

Existen muchos otros métodos que pueden resultar interesantes, relacionados con el sistema de ficheros y carpetas. Por ejemplo, aquí algunos:

| Método         | Descripción                                                                |
| -------------- | -------------------------------------------------------------------------- |
| `access()`     | Comprueba si tenemos permiso sobre ese fichero o carpeta.                  |
| `appendFile()` | Añade información a un fichero que ya existe o inexistente.                |
| `chmod()`      | Cambia los permisos de un fichero.                                         |
| `chown()`      | Cambia el propietario de un fichero.                                       |
| `cp()`         | Copia un fichero a una ruta específica.                                    |
| `glob()`       | Busca los archivos que coincidan con un patrón dado.                       |
| `link()`       | Crea un enlace simbólico a un fichero o carpeta existente desde otra ruta. |
| `mkdir()`      | Crea una carpeta nueva.                                                    |
| `realpath()`   | Devuelve la ruta real de un fichero, generalmente de un enlace simbólico.  |
| `rmdir()`      | Elimina una carpeta.                                                       |
| `rm()`         | Elimina ficheros o carpetas/directorios.                                   |
| `unlink()`     | Elimina un fichero.                                                        |
| `watch()`      | Vigila cambios en ficheros o carpetas.                                     |

## NPM

### ¿Qué es NPM?

**NPM** se puede considerar como las siglas de **Node Package Manager**, es decir, gestor de paquetes de NodeJS, un entorno de ejecución multiplataforma para ejecutar Javascript no sólo en un navegador web (_como se concibió originalmente_) sino fuera de él, y poder utilizarlo en sistemas de escritorio o servidores web.

Este gestor de paquetes (_muy similar al concepto de apt-get en GNU/Linux_), nos permitirá **instalar de forma muy sencilla y automática** paquetes Javascript (_tanto de Node como Javascript para el navegador_) para poder utilizarlos y mantenerlos en los proyectos o sistemas que utilicemos.

#### ¿Tengo Node o NPM instalado?

Para comenzar, necesitaremos instalar NodeJS en su versión LTS (_recomendada_) o en su última versión (_experimental_) si quieres tener las últimas novedades. Al instalar **Node**, se instalará automáticamente su gestor de paquetes y algunas otras utilidades interesantes que necesitaremos.

En primer lugar, vamos a comprobar si tenemos NodeJS/NPM instalado y que versión tenemos:

```powershell
# Muestra la versión de Node
$ node --version

# Muestra la versión de NPM
$ npm --version
```

En el caso de que no tengamos `node` instalado en nuestro sistema, se nos mostrará un mensaje de error como `node: command not found` o similar, en cuyo caso deberemos proceder a instalarlo (_necesitaremos tener privilegios de root o permisos de sudo_).

En el caso de tener una versión muy antigua, también podemos realizar los pasos que veremos a continuación y aprovechar para actualizarlo.

#### ¿Para que usaremos NPM?

Ahora que tenemos `node` y su gestor de paquetes `npm` disponible en nuestro sistema, podremos **instalar paquetes de NPM** en nuestros proyectos y/o en nuestro sistema.

Aunque profundizaremos más adelante, hay 2 modalidades principales para utilizar **NPM**:

-   **A nivel de proyecto**: Probablemente la modalidad más utilizada es la de usar NPM como un gestor de dependencias de un proyecto, esto es, un sistema con el que controlamos que paquetes o librerías Javascript están instalados (_y que versión_), de modo que quedan asociados al proyecto en sí. Esto facilita que si un usuario diferente se descarga el proyecto, pueda gestionarlo fácil y rápidamente (instalar paquetes, actualizarlos, etc...)

-   **A nivel global**: Existen algunas situaciones específicas, en las que los paquetes son realmente utilidades que no se utilizan en proyectos, muy común en aplicaciones de **línea de comandos** (_CLI_) que usamos desde terminal. En esta modalidad, los paquetes se instalan a nivel del sistema (_no en la carpeta del proyecto_), por lo que están disponibles siempre que el usuario quiera utilizarlos, sin necesidad de tenerlo en cada proyecto.

#### Alternativas a NPM

Existen algunas alternativas a NPM que añaden características y/o permiten realizar las mismas tareas de NPM pero de una forma alternativa que ofrece ciertas ventajas. Entre estas alternativas podemos encontrar:

| Nombre | Descripción                                                           |
| ------ | --------------------------------------------------------------------- |
| PNPM   | Alternativa rápida que hace uso eficiente del espacio en disco.       |
| Ultra  | Alternativa ultrarápida a NPM. Mejora aún más el rendimiento de PNPM. |
| Yarn   | Una de las primeras alternativas a NPM.                               |

### Crear nuevo proyecto con NPM

**NPM** (_Node Package Manager_) nos permite, entre otras cosas, **gestionar las dependencias** de un proyecto de modo que no tenemos que hacerlo manualmente, sino de una forma relativamente automática y controlada, acelerando el desarrollo y reduciendo el tiempo necesario para mantener nuestros proyectos.

En este primer artículo vamos a repasar el **scaffolding** (_estructura de carpetas_) de un proyecto de frontend y como crear desde cero un proyecto con NPM.

#### Inicializar el proyecto

El primer paso será acceder a nuestra carpeta de proyectos (_por ejemplo, `/documents/workspace`_) y una vez allí, crear la carpeta del proyecto actual e inicializarlo. Por lo tanto, nuestro primer paso debe ser elegir un **buen nombre** para la carpeta de nuestro proyecto:

```powershell
# Accedemos a la carpeta de todos nuestros proyectos
$ cd /documents/workspace

# Creamos la carpeta de nuestro proyecto
$ mkdir frontend-project

# Accedemos a su carpeta raíz (carpeta inicial)
$ cd frontend-project
```

> Aunque pueda parecer una tontería, es muy importante escoger un **buen nombre** para la carpeta del proyecto. Ese nombre se suele utilizar en el repositorio de GitHub o en la URL para publicarlo si lo subimos a **GitHub Pages**, por ejemplo.

Algunos buenos consejos sobre el nombre del proyecto:

1. Utiliza siempre minúsculas.
2. No utilices espacios en el nombre. Usa guiones en su lugar.
3. Evita el uso de carácteres especiales, signos de puntuación, etc...

Una vez en la **carpeta raíz** del proyecto, sería una buena idea preparar **git** con un `git init` para llevar el **control de versiones** del proyecto cuanto antes. Esto creará una carpeta oculta `.git` donde se guardará toda la información relativa a **git**:

```powershell
# Inicializamos el control de versiones
$ git init
Inicializado repositorio Git vacío en /documents/workspace/frontend-project/.git/

# Añadimos la URL de GitHub (u otro servicio) como repositorio remoto
$ git remote add origin https://github.com/ManzDev/frontend-project.git
```

También sería un buen momento para crear un archivo .gitignore, que indique las carpetas que vamos a ignorar con `git`. La carpeta `node_modules/` debe estar obligatoriamente en dicho fichero.

Una vez hecho esto, inicializamos el proyecto con NPM, escribiendo `npm init -y`. Esto creará un fichero llamado `package.json` del que hablaremos más adelante y que contendrá toda la información del proyecto:

```powershell
# Inicializamos el proyecto con NPM
$ npm init -y
Wrote to /home/manz/workspace/frontend-project/package.json:

{
  "name": "frontend-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/ManzDev/test.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/ManzDev/test/issues"
  },
  "homepage": "https://github.com/ManzDev/test#readme"
}
```

El parámetro `-y` de `npm init` omite el asistente interactivo, creando el `package.json` con los valores por defecto directamente y sin preguntarnos. Si prefieres utilizar el asistente, omite ese parámetro.

> Observa que es interesante crear primero el repositorio de git y añadir el `remote`, puesto que así el `package.json` generado con NPM ya tendrá las entradas `repository`, `bugs` y `homepage`. De hacerlo posteriormente, tendríamos que añadirlas a mano.

#### Crear la estructura de carpetas

Para trabajar en un proyecto, es necesario conocer bien el scaffolding (_o estructura de carpetas_). Aunque esta estructura puede variar de un proyecto a otro, utilizaremos la siguiente, ya que considero que es bastante estándar y que puede servir como punto de partida:

```powershell
- frontend-project/       # Carpeta raíz del proyecto
  - .git/                 # Carpeta oculta de datos de git
  - node_modules/         # Carpeta de paquetes de Node/NPM
  - dist/                 # Carpeta de código generado (cuando se usan preprocesadores)
  - src/                  # Carpeta de código fuente (código editable)
    - assets/             # Carpeta de estáticos (imágenes, audio, video, fuentes...)
    - js/                 # Carpeta de Javascript
      - index.js
    - css/                # Carpeta de CSS
      - index.css
    - index.html
  - package.json          # Archivo del proyecto NPM
  - package-lock.json     # Histórico de versiones de dependencias
  - .gitignore            # Ficheros y carpetas a ignorar por git
```

Hasta ahora, solo deberíamos tener la carpeta `.git`, el archivo `package.json` y el archivo `.gitignore` (_si lo hemos creado_). La carpeta `node_modules` se creará desde que instalemos al menos un paquete en el proyecto (_no de forma global_), por lo que no nos preocuparemos de ella de momento.

La carpeta `dist` se suele crear sólo en proyectos donde estamos utilizando preprocesadores, transpiladores o herramientas consideradas «build tools», que de momento no estamos usando.

Así pues, vamos a crear la estructura de carpetas `src`, de la que si debemos preocuparnos:

```powershell
# Creamos las carpetas src y subcarpetas
mkdir -p src/{assets,js,css}

# Creamos los ficheros HTML, CSS y Javascript
touch src/index.html
touch src/js/index.js
touch src/css/index.css
```

Una vez creados los archivos, faltaría crear un código HTML de base en el `index.html` y enlazar correctamente al archivo `index.css` en una etiqueta `<link>` y al archivo `index.js` en una etiqueta `<script>`.

### Instalar paquetes con NPM

Quizás, la finalidad más popular de `npm` es la de **gestionar** (_instalar, desinstalar y actualizar_) los **paquetes** o **dependencias** de un proyecto de forma cómoda, rápida y amigable para el desarrollador, permitiendo ahorrar tiempo y esfuerzo. Sin embargo, si no tenemos los conocimientos suficientes sobre **como funciona npm**, es posible que esta tarea se nos vuelva cuesta arriba.

> Antiguamente, uno de los problemas de crear un proyecto, es que gestionabamos las **dependencias de forma manual**, es decir, colocabamos las librerías en carpetas específicas, pero dependíamos del conocimiento, memoria y experiencia de la persona que colocó esas dependencias, que en el mejor de los casos, lo habrá documentado. Las tareas de actualización de dichas librerías se volvían un proceso bastante tedioso (_a veces hasta imposible_) y complejo. Con `npm` la idea es automatizar todo este proceso, para que sea una tarea rápida y sencilla.

Vamos a repasar las funcionalidades principales de **npm** para instalar y desinstalar paquetes en un proyecto. Para ello, utilizaremos principalmente el comando `install` y `uninstall` de `npm`, además de un pequeño comando ayudante, denominado `npx`, del que hablaremos más adelante.

Así pues, un resumen (_a grandes rasgos_) de lo que veremos sería: tareas de **búsqueda**, tareas de **instalación**, tareas de **desinstalación** y **ejecución** de ciertos comandos. A continuación los comandos que deberíamos utilizar para ello:

| Comando                   | Abreviatura       | Descripción                                                           |
| ------------------------- | ----------------- | --------------------------------------------------------------------- |
| `npm search <palabra>`    | `npm s <palabra>` | Realiza una búsqueda de paquetes en NPM.                              |
| `npm install <paquete>`   | `npm i <paquete>` | Instala el paquete indicado en `node_modules/`.                       |
| `npm uninstall <paquete>` | `npm r <paquete>` | Desinstala el paquete indicado, eliminándolo de `node_modules/`.      |
| `npx <comando>`           |                   | Ejecuta paquetes CLI instalados a nivel de proyecto (o sin instalar). |

> Es posible abreviar o utilizar alias en ciertos comandos. Por ejemplo `npm add` o `npm i` son equivalentes a `npm install`, mientras que `npm unlink`, `npm remove`, `npm rm` o `npm r` son equivalentes a `npm uninstall`.

Una vez hemos creado e inicializado nuestro proyecto con NPM, debemos aprender a instalar y desinstalar dependencias de nuestro proyecto de forma correcta, que es lo que veremos a continuación.

#### Buscar paquetes de NPM

Pero antes de instalar un paquete, primero debemos encontrarlos. Los paquetes de NPM podemos buscarlos a través del buscador de NPM, disponible en la web NPMjs.com. No obstante, también podemos utilizar el **buscador de linea de comandos** de `npm`:

```powershell
$ npm search howler

NAME              | DESCRIPTION       | AUTHOR      | DATE       | VERSION | KEYWORDS
howler            | Javascript audio… | =goldfire   | 2020-05-17 | 2.2.0   | howler ...
@jimbly/howler    | GLOV.js fork of…  | =jimbly     | 2020-05-15 | 0.0.6   | howler ...
w-howler          | A wrapper howler… | =semisphere | 2020-06-01 | 1.0.8   | package ...
w-audioplayer-vue | An audio player…  | =semisphere | 2020-06-01 | 1.0.13  | component ...
...
```

Si finalmente encontramos un paquete en el que estamos interesados y queremos ver más información sobre él, podemos utilizar el comando `npm show`, que nos mostrará más información del paquete o librería:

```powershell
$ npm show howler

howler@2.2.0 | MIT | deps: none | versions: 47
Javascript audio library for the modern web.
https://howlerjs.com

keywords: howler, howler.js, audio, sound, web audio, webaudio, browser, html5, ...
```

Una vez encontrado el paquete que buscamos, toca instalarlo.

#### Instalar paquetes de NPM

Para instalar un paquete en nuestro proyecto, simplemente debemos utilizar el comando `npm install`, situados en la carpeta de nuestro proyecto. Esto instalará dicho paquete y todas sus dependencias en la carpeta `node_modules/`, a la vez que actualiza el fichero `package.json` añadiendo el paquete y su versión como una dependencia del proyecto.

Existen varias formas de instalar un paquete:

| Comando          | Formato largo | Descripción                                                        |
| ---------------- | ------------- | ------------------------------------------------------------------ |
| `npm install -D` | `--save-dev`  | Instala el paquete en el proyecto, como dependencia de desarrollo. |
| `npm install`    | `--save-prod` | Instala el paquete en el proyecto, como dependencia de producción. |
| `npm install -g` | `--global`    | Instala el paquete en el sistema, sin asociarlo al proyecto.       |

Las **dependencias de desarrollo** son aquellos paquetes que necesitamos en un proyecto mientras estamos desarrollándolo, pero una vez tenemos el código generado del proyecto, no vuelven a hacer falta. Los paquetes instalados con el flag `--save-dev` o `-D` se instalan en esta modalidad, guardándolos en la sección `devDependences` del fichero `package.json`.

Por otro lado, las **dependencias de producción** son aquellos paquetes que necesitamos tener en la web final generada, como librerías Javascript necesarias para su funcionamiento o paquetes similares. Los paquetes instalados con el flag `--save-prod`, `-P` o directamente sin ningún flag se instalan en esta modalidad, guardándolos en la sección `dependences` del fichero `package.json`.

Es importante diferenciar ambas modalidades, aunque al principio es habitual no saber exactamente cuando se trata de un paquete de desarrollo y cuando un paquete de producción.

Veamos un ejemplo de instalación con ambos tipos de paquetes:

```powershell
# Instala en modalidad de desarrollo el paquete "gh-pages"
$ npm install --save-dev gh-pages

+ gh-pages@3.1.0
added 50 packages from 12 contributors and audited 92 packages in 1.998s
```

```powershell
# Instala en modalidad de producción la librería "Howler"
$ npm install howler

+ howler@2.2.0
added 1 package from 1 contributor and audited 93 packages in 1.615s
```

> Observa que en ambos casos, se nos indica la versión del paquete instalada tras el carácter `@`.

En el primer caso estamos instalando el paquete gh-pages, una librería y comando CLI para desplegar fácilmente un proyecto en **GitHub Pages**. Como se trata de un paquete que no es necesario incluir en la web final (_se utiliza en desarrollo para desplegar_), pues lo instalamos con los flags `--save-dev` o `-D`.

En el segundo caso, estamos instalando el paquete Howler, una librería Javascript que permite manipular y gestionar archivos multimedia de audio desde el navegador. En este caso se trata de una librería JS que si estará incluida en la versión definitiva de la página, por lo que la instalaremos con el flag `--save-prod`, `-P` o sin indicar ninguno, ya que es la opción por defecto.

> En la última modalidad los **paquetes instalados de forma global** no se asocian al proyecto, sino al sistema. Se explican en detalle en el capítulo Instalaciones globales con NPM.

#### Desinstalar paquetes

Si queremos **desinstalar un paquete** de nuestro proyecto, simplemente escribimos el siguiente comando en la terminal de texto:

```powershell
npm uninstall howler
```

Esto se encargará de eliminar el paquete de `node_modules/` y eliminar su asociación al proyecto en el `package.json`, independientemente de ser una dependencia de desarrollo o de producción.

## React

### ¿Qué es React?

React es una librería para crear interfaces web de usuario con Javascript. El objetivo de **React** es tener a nuestra disposición un lenguaje que simplifique la cantidad de detalles que hay que hacer en Javascript, sin perder demasiado control o personalización en lo que hacemos.

Por esa razón, muchos de los conceptos que vamos a ver relacionados con React, cambian los fundamentos o conceptos utilizados en el desarrollo web, introduciendo nuevos conceptos o realizando muchas tareas de forma transparente por debajo, de modo que el desarrollador no tenga que controlar al detalle muchas de esas cosas.

> **OJO**: Que no tengas que controlar muchos de estos detalles no quiere decir que puedas permitirte desconocerlos. Si ignoras estos fundamentos o bases, podrás hacer cosas sencillas, pero te costará mucho dominar o trabajar con React.

#### Historia y comparación

Actualmente, existen 4 librerías o frameworks populares para crear interfaces de usuario:

-   Angular: Framework «todo-en-uno». Suele interesar a desarrolladores que provienen de backend, porque utiliza patrones muy similares y habituales en ese sector. Requiere conocer Typescript.

-   React: Librería para crear UI para web. Si te encuentras cómodo trabajando con Javascript (_o conoces Javascript del lado del servidor como Node, Deno o Bun_), pero eres ajeno a las bases del frontend (_HTML, CSS, ..._), probablemente será tu opción preferida.

-   VueJS: Framework progresivo para frontend. Si eres un desarrollador que proviene o tiene bases sólidas de HTML y CSS, es muy probable que Vue te resulte un framework más agradable que el resto.

-   Svelte: Librería Javascript que une lo mejor de los mundos de React y Vue, introduciendo ciertos conceptos propios. La única pega es que es más moderna y su adopción es menor.

#### Conceptos

Antes de continuar, recuerda que **React** es una abstracción, es decir, un lenguaje que está sobre Javascript, y para simplificar el trabajo del desarrollador, cambia muchos patrones y formas de trabajar por otras, con el objetivo de que sea más sencillo.

Veamos una lista de **conceptos** que se utilizan en React como equivalencia a patrones utilizados en Javascript u otros sectores:

##### Componentes

En React se trabaja con **componentes**. A grandes rasgos, se trata de crear «etiquetas propias» que se comportan como etiquetas HTML. La forma preferida de crearlas es utilizar los llamados **componentes funcionales**, es decir, funciones de Javascript que devuelven código HTML (_o casi_):

```jsx
function component() {
    return <h1>¡Hola, amigo!</h1>;
}
```

Antiguamente, se utilizaban **componentes de clases** (_programación orientada a objetos_). Sin embargo, aunque la orientación a objetos sigue siendo actualmente una forma válida de trabajar, **en React** se ha reemplazado por los **componentes funcionales**, ya que son más simples dentro de su ecosistema.

##### JSX

En React se utiliza **JSX** como lenguaje de marcado en lugar de HTML. **JSX** es una sintaxis que, aunque parece HTML, realmente es Javascript y se encarga de leer el código JSX y convertirlo a un HTML equivalente con añadidos Javascript:

```jsx
function component() {
    const name = "Manz";
    return (
        <div>
            <h1>¡Hola, {name}!</h1>
        </div>
    );
}
```

Al principio, suele disgustar mucho por su mezcla de HTML y Javascript, algo que siempre se ha tendido a separar. Sin embargo, el objetivo de esto es no separar tecnologías: crear componentes de modo que, siempre que tengan relación entre sí, estén juntos, independientemente de la tecnología utilizada.

##### Virtual DOM

En el ecosistema de React no se suele trabajar directamente con el DOM. Aunque puedes hacerlo, está mal visto y se considera una mala práctica. Esto es así porque React utiliza su llamado **Virtual DOM**: una especie de DOM ligero en memoria que es al que **React** accede directamente. Por esta razón, normalmente el desarrollador de React no necesita preocuparse por el DOM. React se encargará de mantenerlo actualizado cuando sea necesario.

Existe un **mito** que afirma que el «**DOM es lento**» y es mejor utilizar un Virtual DOM. Esto no es cierto. Realmente, el **DOM** no es lento, sino que es muy común que se gestione incorrectamente por el desarrollador. Si evitamos acceder al DOM real y dejamos esa tarea al Virtual DOM de React, evitamos la posibilidad de gestionarlo incorrectamente.

##### Datos unidireccionales

Al contrario de como se trabaja en otros ecosistemas, en **React** los datos viajan de forma **unidireccional**, o sea, esto significa que un elemento HTML padre puede enviar datos a sus elementos HTML hijos, pero no a la inversa.

Lo que debemos tener en cuenta es que debido a esta «limitación» tenemos una ventaja y una desventaja:

-   🟩 Es más simple y predecible gestionar los datos.
-   🟥 Es menos flexible al no poder trabajar de forma bidireccional.

En casos donde se requiera compartir datos de forma más compleja, se suelen optar por stores (librerías con estado centralizado), de las que hablaremos más adelante.

### Instalación de React

Antes de comenzar, debes tener en cuenta que, aunque **React** sea una librería de frontend, la forma habitual de utilizar **React** requiere ciertas herramientas que transpilan el código. Esto significa que el Javascript que escribes, se convierte a otro código Javascript, que es el que finalmente se lee en el navegador.

En este artículo vamos a ver los pasos que debes saber para prepararte para utilizar **React**.

#### Requisitos

-   NodeJS instalado
-   NPM, pnpm o un gestor de paquetes para NodeJS.
-   **React** se suele usar mediante un automatizador como Vite, Webpack o similar.

#### Instalación

Para crear un proyecto de **React** de la forma más automática posible, podemos utilizar el siguiente comando con `npm`:

```powershell
$ npm create vite@latest project-name -- --template react
```

Este comando crea una carpeta `project-name` utilizando Vite conjuntamente a una plantilla preconfigurada de React. Si este método no te sirve, puedes probar con la instalación manual.

#### Estructura de carpetas

Para empezar, es conveniente tener clara la estructura de carpetas que utilizaremos en nuestras aplicaciones web hechas con React. De momento, tendremos las carpetas `public/` y `components/` vacías, pero más adelante iremos incluyendo ficheros en su interior.

```powershell
src/
|---- public/       # Carpeta de estáticos (imágenes, fuentes, sonidos...)
|---- components/   # Carpeta de componentes .jsx
|---- App.jsx       # Componente principal de la App
|---- index.html    # Fichero principal HTML
|---- index.jsx     # Fichero principal Javascript
```

En estos primeros ejemplos, los ficheros `index.html` e `index.jsx` son los primeros que se leen por parte del navegador, haciendose referencia luego al fichero `App.jsx`, que es el componente global de la App.

El resto de componentes los iremos incluyendo en `components/` e iremos ampliando la estructura de carpetas.

#### Primer ejemplo de React

Para trabajar con React debemos asegurarnos de que tenemos una correcta estructura de ficheros y carpetas. Lo primero, localizaremos nuestro fichero `index.html` (_siempre será el primer archivo que el navegador lee_). Una vez localizado nos aseguramos que esté cargando nuestro fichero Javascript principal:

```html
<head>
    <script type="module" src="./index.jsx"></script>
</head>
```

Este será el primer fichero Javascript que leerá nuestra aplicación, por lo que en él tendremos que cargar nuestra aplicación React.

> En **React** lo normal es utilizar ficheros `.jsx` en lugar de `.js`. Aunque el navegador no es capaz de leer este tipo de ficheros, habíamos mencionado que React (_junto a Vite_) convierte nuestro código fuente a un código Javascript diferente, que es el que realmente lee el navegador.

Observa el siguiente código, que es el contenido del fichero `index.jsx`:

```jsx
import { createRoot } from "react-dom/client";
import { App } from "./App.jsx";

createRoot(document.body).render(<App />);
```

En este caso, realizamos varias cosas:

1. Importamos la función `createRoot()` para definir un elemento HTML como **padre principal**.
2. En ese elemento, se renderiza el componente `App.jsx` (_que devolverá código JSX_).

El código de este fichero siempre es mu similar, por lo que no debemos preocuparnos excesivamente por él en este momento. Realmente donde empezaremos a editar código es en el fichero `App.jsx`.

Así pues, veamos el contenido del fichero `App.jsx`:

```jsx
export function App() {
    const name = "Manz";
    return (
        <div>
            <h1>¡Hola, {name}!</h1>
        </div>
    );
}
```

Observa que tras el `return` tenemos unos paréntesis. Aunque parece que estamos devolviendo HTML, esos paréntesis contienen **código JSX** (_una mezcla de HTML y Javascript_). Este será uno de los primeros temas que veremos a continuación.

#### Otras formas de usar React

En este tutorial estamos utilizando React como librería mediante un popular automatizador llamado Vite. Sin embargo, no es la única forma de utilizar React hoy en día.

-   **Create React App**: Es posible que leas que otra forma de utilizar React es mediante **CRA** (_Create React App_). Esta es una forma antigua y desaconsejada, por lo que si encuentras una guía o te aconsejan usar CRA, lo más probable es que se trate de información desactualizada.

-   **NextJS**: Una de las formas más comunes de utilizar React hoy en día es utilizando NextJS. En ese caso estaríamos utilizando React como un framework, y no como una librería. Como ventaja, muchas características están automatizadas y son más sencillas, mientras que como inconveniente es que el frontend comienza a estar mezclado con backend y servicios de NextJS (_u otro proveedor_), generando cierta dependencia y acoplamiento a dichos servicios.

### JSX: El HTML de React

Cuando trabajamos con React y necesitamos escribir el HTML de un fichero o componente, no vamos a escribir HTML directamente (_aunque pueda parecer que lo estamos haciendo_). En React se utiliza una sintaxis llamada **JSX**, que realmente es código Javascript con apariencia de HTML que se escribe en el interior de funciones llamados **componentes funcionales** en los que profundizaremos un poco más adelante.

#### ¿Qué es JSX?

Como hemos mencionado, **JSX** es código Javascript con apariencia de marcado HTML. Observa la siguiente función de ejemplo que devuelve un fragmento de código **JSX**:

```jsx
function component() {
    return <h1>Hola</h1>;
}
```

Si conoces minimamente Javascript, esto te resultará bastante extraño, ya que no se puede añadir una etiqueta HTML directamente en el código Javascript (_observa que no tiene las comillas de un string_). Pero como hemos mencionado, estamos trabajando con ficheros `.jsx` que son procesados por Vite y React antes de llegar al navegador, por lo que el Javascript final es diferente.

Para dejarlo más claro, vamos a ver que es realmente lo que le llega al navegador de ese fragmento de **código JSX**:

```jsx
function component() {
    return <h1>Hola</h1>;
}

const jsx = component();
console.log(jsx);
```

```js
/* El objeto JSX devuelto por la función */
{
  $$typeof: Symbol,
  key: null,
  props: { ... },
  _owner: FiberNode,
  _store: { ... }
}
```

React se encargará internamente de trabajar con ese objeto por nosotros y añadirlo al HTML real de la página, de modo que nosotros solo nos tengamos que preocupar del código que escribimos en el editor.

#### HTML vs JSX

Aunque pueda parecerlo, JSX no es 100% compatible con HTML, y tiene sus propias normas. Además, JSX permite evaluar código Javascript y devolverlo evaluado, lo que hace que sea mucho más cómodo a la hora de trabajar.

La idea es que con React podamos trabajar con lógica de programación y estructuras de datos en Javascript antes del `return`, y luego al devolver la información, se devuelva el código JSX para construir el HTML.

En cierta forma, la forma de pensar al trabajar con React es **crear funciones que devuelven código JSX, y que se utilizarán como si fueran una etiqueta HTML**. Veamos ahora un ejemplo donde un código HTML correcto no funcionaría en React:

```jsx
export function App() {
  /* Aquí iría nuestra lógica Javascript */
  return (
    <div>
      <p>¡Hola, Manz! <br> ¿Qué tal estás?</p>
    </div>
  );
}
```

Este fragmento de código tiene una etiqueta HTML `<br>` que no requiere cierre. Esto en HTML es correcto, sin embargo, React te dará error ya que **se espera que todas las etiquetas HTML se cierren**. Hay dos formas de solucionarlo en React:

1. Escribir una etiqueta de cierre `</br>`. Correcto en React pero incorrecto en HTML puro.
2. «Autocerrar» la etiqueta sin cierre `<br />`. Válida en HTML (_heredado de XHTML_).

Lo mismo ocurre con otras etiquetas HTML que no requieren cierre como `<img>`, `<hr>`, `<meta>`, `<link>`, `<input>`, `<source>`, `<track>`, `<base>`, etc.

> Observa que cuando tenemos **fragmentos JSX muy extensos**, de forma opcional se puede envolver todo en paréntesis para mejorar la legibilidad.

#### Javascript en JSX

La **sintaxis JSX** es inteligente y nos permite añadir código Javascript de múltiples formas, adaptándose automáticamente. Dos ejemplos de ello:

-   El `name` lo insertamos literalmente en el párrafo `<p>`, por lo que se genera como texto.
-   El `styles` contiene varias propiedades CSS. JSX es lo suficientemente inteligente para entender que lo estamos asociando a estilos en linea, por lo que lo convierte al formato que necesita por nosotros:

```jsx
export function App() {
    const name = "Manz";
    const styles = {
        background: "indigo",
        color: "white",
        padding: "5px 15px",
    };

    return (
        <div style={styles}>
            <p>¡Hola, {name}!</p>
        </div>
    );
}
```

```html
<!-- Código HTML generado -->
<div style="background:indigo;color:white;padding:5px 15px">
    <p>¡Hola, Manz!</p>
</div>
```

> **Cuidado**: Esta no es la forma adecuada de manejar estilos en React.

## Otros conceptos

Iniciación del repositorio de GitHub en local

1. `git clone {url}`
2. `git push origin main`

---

## Actividades

-   [Ejercicio 1 - Crear una web utilizando módulos: NASA](./src/ejercicio1-nasa/)
-   [Ejercicio 2 - Crear un programa en node.js utilizando módulos](./src/ejercicio2-programa_node/)
-   [Ejercicio 3 - Utilizar librerías nativas de node.js](./src/ejercicio3-librerias_nativas/)
-   [Ejercicio 4 - Utilizar librerías de npm](./src/ejercicio4-librerias_npm/)
-   [Ejercicio 5 - Crear react sin dependencias](./src/ejercicio5-react_sin_dependencias/)
-   [Ejercicio 6 - Haz un ejercicio con JSX, utilizando VITE](./src/ejercicio6-jsx_vite/)

---

## Bibliografía

-   [Apuntes desarrollados por Manz.dev](https://lenguajejs.com/)
-   Documentación sobre paquetes:
    -   [npm](https://www.npmjs.com/)
    -   [Node.js](https://nodejs.org/api/)
-   [Tutorial Node.js](https://www.youtube.com/watch?v=yB4n_K7dZV8)
-   [Tutorial React](https://www.youtube.com/watch?v=7iobxzd_2wY)
