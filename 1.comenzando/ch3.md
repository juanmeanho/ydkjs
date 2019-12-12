# Aún no sabes JS: Comenzando - 2da Edición
# Capítulo 3: Explorando más profundo

| NOTA DEL AUTOR: |
| :--- |
| Trabajo en progreso |

Si has leído los capítulos 1 y 2, y te has tomado el tiempo de digerir y filtrar, con suerte comenzarás a *obtener* un poco más de JS. Si los omitiste (especialmente el Capítulo 2), te recomiendo que lo reconsideres y pases algún tiempo con ese material.

En el Capítulo 2, nuestro enfoque estaba en la sintaxis, los patrones y los comportamientos. Aquí, nuestra atención se desplaza a algunas de las características de nivel inferior de JS que sustentan prácticamente todas las líneas de código que escribimos.

Cabe señalar que este material todavía no es una exposición exhaustiva de JS; ¡para eso es el resto de la serie de libros! Aquí, nuestro objetivo sigue siendo solo *comenzar* y sentirnos más cómodos con la *sensación* de JS, cómo fluye y refluye.

Este capítulo debería comenzar a responder algunos de los "¿Por qué?" preguntas que probablemente surjan a medida que explora JS.

## Closure

¿Qué es un Closure?

> Un Closure es la capacidad de una función de recordar y continuar accediendo a variables definidas fuera de su alcance, incluso cuando esa función se ejecuta en un alcance diferente.

Quizás sin darse cuenta, casi todos los desarrolladores de JS han hecho uso del closure. Es importante poder reconocer dónde está en uso en sus programas, ya que la presencia o falta de closures es a veces la causa de errores (o incluso deficiencias en el rendimiento).

De la definición anterior, vemos dos partes que son críticas. Primero, el closure es una característica de una función. Los objetos no tienen closures, las funciones sí. En segundo lugar, para observar un closure, debe ejecutar una función en un ámbito diferente al que se definió originalmente.

Considere:

```js
function greeting(msg) {
    return function who(name) {
        console.log(`${msg}, ${name}!`);
    };
}

var hello = greeting("Hello");
var howdy = greeting("Howdy");

hello("Kyle");
// Hello, Kyle!

hello("Sarah");
// Hello, Sarah!

howdy("Grant");
// Howdy, Grant!
```

Primero, se ejecuta la función externa `greeting (..)`, creando una instancia de la función interna `who(..)`; esa función se cierra sobre la variable `msg`, el parámetro del alcance externo de `greeting (..) `. Devolvemos esa función interna y asignamos su referencia a la variable `hello`. Luego llamamos a `greeting(..)` por segunda vez, creando una nueva instancia de función interna, con un nuevo cierre sobre un nuevo `msg`, y devolvemos esa referencia para que se asigne a` howdy`.

Cuando la función `greeting(..)` termine de ejecutarse, normalmente esperaríamos que todas sus variables vayan al recolector de basura (eliminadas de la memoria). Esperaríamos que cada `msg` desapareciera, pero no lo hacen. El motivo es el closure. Dado que las instancias de funciones internas todavía están vivas (asignadas a `hello` y `howdy` respectivamente), sus closures aún conservan las variables `msg`.

Estos closures no son una instantánea del valor de la variable `msg`; son un enlace directo y preservación de la variable misma. Eso significa que el closures realmente puede observar (¡o hacer!) actualizaciones a estas variables a lo largo del tiempo.

```js
function counter(step = 1) {
    var count = 0;
    return function increaseCount(){
        count = count + step;
        return count;
    };
}

var incBy1 = counter(1);
var incBy3 = counter(3);

incBy1();       // 1
incBy1();       // 2

incBy3();       // 3
incBy3();       // 6
incBy3();       // 9
```

Cada instancia de la función interna `increaseCount()` se cierra sobre las variables `count` y `step` desde el alcance de la función externa `counter(..)`. `step` permanece igual con el tiempo, pero `count` se actualiza en cada invocación de esa función interna. Como el closure se realiza sobre las variables y no solo sobre las instantáneas de los valores, estas actualizaciones se conservan.

El closure es más común cuando se trabaja con código asincrónico, como con devoluciones de llamada. 

Considere:

```js
function getSomeData(url) {
    ajax(url,function onResponse(resp){
        console.log(`Response (from ${url}): ${resp}`);
    });
}

getSomeData("https://some.url/wherever");
// Response (from https://some.url/wherever): ..whatever..
```

La función interna `onResponse(..)` se cierra sobre `url`, y así la conserva y recuerda hasta que la llamada Ajax regresa y ejecuta `onResponse(..)`. Aunque `getSomeData(..)` termine de inmediato, la variable de parámetro `url` se mantiene activa en el closure durante el tiempo que sea necesario.

No es necesario que el alcance externo sea una función, generalmente lo es, pero no siempre, solo que haya al menos una variable en un alcance externo que una función interna acceda, y así se cierre.

```js
for (let [idx,btn] of buttons.entries()) {
    btn.addEventListener("click",function onClick(evt){
       console.log(`Clicked on button (${idx})!`);
    });
}
```

Debido a que este bucle está utilizando declaraciones `let`, cada iteración obtiene nuevas variables `idx` y `btn` con ámbito de bloque (también conocido como local); el bucle también crea una nueva función interna `onClick(..)` cada vez. Esa función interna se cierra sobre `idx`, conservándola mientras el controlador de clic esté configurado en `btn`. Entonces, cuando se hace clic en cada botón, su controlador puede imprimir su valor de índice asociado, porque el controlador recuerda su respectiva variable 'idx'.

Recuerde: este closure no está por encima del valor (como `1` o` 3`), sino por encima de la variable `idx`.

El closure es uno de los patrones de programación más frecuentes e importantes en cualquier lenguaje. Pero eso es especialmente cierto en JS; Es difícil imaginar hacer algo útil sin aprovechar el closure de una forma u otra.

## La palabra clave `this`

Uno de los mecanismos más poderosos de JS es también uno de los más incomprendidos: la palabra clave `this`. Un error común es que el `this` de una función se refiere a la función misma. Debido a cómo `this` funciona en otros lenguajes, otro concepto erróneo es que `this` señala la instancia a la que pertenece un método. Ambos son incorrectos.

Como se discutió anteriormente, cuando se define una función, se *adjunta* su alcance de cierre a través del closure. El alcance es el conjunto de reglas que controla cómo se determinan las referencias a los identificadores (variables).

Pero las funciones también tienen otra característica además de su alcance que influye en lo que pueden acceder. Esta característica se describe mejor como un *contexto de ejecución*, y se expone a la función a través de su palabra clave `this`.

El alcance es estático y contiene un conjunto fijo de variables disponibles en el momento y la ubicación en que se define una función, pero la ejecución *contexto* de una función es dinámica, totalmente dependiente de **cómo se llama** (independientemente de dónde esté definida o incluso desde donde se ha llamado).

Es importante darse cuenta: `this` no es una característica fija de una función basada en la definición de la función, sino más bien una característica dinámica que se determina cada vez que se llama a la función.

Una forma de pensar sobre el *contexto de ejecución* es que es un objeto tangible cuyas propiedades están disponibles para una función mientras se ejecuta. Compare eso con el alcance, que también puede considerarse como un *objeto*; excepto que el *objeto de alcance* está oculto dentro del motor JS, siempre es el mismo para esa función y sus *propiedades* toman la forma de variables de identificación disponibles dentro de la función.

Considere:

```js
function classroom(teacher) {
    return function study() {
        console.log(
            `${teacher} wants you to study ${this.topic}`
        );
    };
}

var assignment = classroom("Kyle");
```

La función externa `classroom(..)` no hace referencia a la palabra clave `this`, por lo que es como cualquier otra función que hayamos visto hasta ahora. Pero la función interna `study()` hace referencia a `this`, lo que la convierte en una función consciente de `this`. En otras palabras, es una función que depende de su *contexto de ejecución*.

| NOTA: |
| :--- |
| `study()` también está cerrada sobre la variable `teacher` desde su alcance externo. |

La función interna `study()` se devuelve desde `classroom("Kyle")` y se asigna a una variable llamada `assignment`. Entonces, ¿cómo se puede llamar a `assignment()` (también conocida como `study()`)?


```js
assignment();
// Kyle wants you to study undefined  -- Oops :(
```

En este fragmento, llamamos a `assignment()` como una función normal, sin proporcionarle ningún *contexto de ejecución*.

Dado que este programa no está en modo estricto (consulte el Capítulo 1, "Hablando estrictamente"), las funciones conscientes del contexto que se llaman **sin ningún contexto especificado** predeterminan el contexto al objeto global (`window` en el navegador). Como no existe una variable global llamada `topic` (y, por lo tanto, no existe tal propiedad en el objeto global), `this.topic` se resuelve en `undefined`.

Ahora considere:

```js
var homework = {
    topic: "JS",
    assignment: assignment
};

homework.assignment();
// Kyle wants you to study JS
```

Una copia de la referencia de función `assignment` se establece como una propiedad en el objeto `homework`, y luego se llama como `homework.assignment()`. Eso significa que el `this` para esa llamada de función será el objeto `homework`. Por lo tanto, `this.topic` se resuelve en `"JS" `.

Finalmente:

```js
var otherHomework = {
    topic: "Math"
};

assignment.call(otherHomework);
// Kyle wants you to study Math
```

Una tercera forma de invocar una función es con el método `call(..)`, que toma un objeto (`otherHomework` aquí) para configurar la referencia `this` para la llamada a la función. `this.topic` se resuelve así a `"Math"`.

La misma función sensible al contexto invocada de tres maneras diferentes, da respuestas diferentes cada vez para qué se hace referencia al objeto `this`.

El beneficio de las funciones conscientes de `this`, y su contexto dinámico, es la capacidad de reutilizar de manera más flexible una sola función con datos de diferentes objetos. Una función que se cierra sobre un ámbito nunca puede hacer referencia a un ámbito o conjunto de variables diferente. Pero una función que tenga conciencia dinámica de contexto `this` puede ser bastante útil para ciertas tareas.

## Prototipos

Donde `this` es una característica de la ejecución de funciones, un prototipo es una característica de un objeto, y específicamente la resolución de un acceso a la propiedad.

Piense en un prototipo como un enlace entre dos objetos; El vínculo está oculto tras bastidores, aunque hay formas de exponerlo y observarlo. Este enlace prototipo ocurre cuando se crea un objeto; está vinculado a otro objeto que ya existe.

Una serie de objetos unidos entre sí a través de prototipos se denomina "cadena de prototipos".

El propósito de este enlace prototipo (es decir, de un objeto B a otro objeto A) es que los accesos contra B para propiedades/métodos que B no tiene, se *deleguen* a A para ser manejado. La delegación de acceso a propiedades/métodos permite que dos (¡o más!) Objetos cooperen entre sí para realizar una tarea.

Considere definir un objeto como un literal normal:

```js
var homework = {
    topic: "JS"
};
```

El objeto `homework` solo tiene una única propiedad: `topic`. Sin embargo, su enlace prototipo predeterminado se conecta con el objeto `Object.prototype`, que tiene métodos incorporados comunes como `toString()` y `valueOf()`, entre otros.

Podemos observar en este enlace prototipo una *delegación* desde `homework` a `Object.prototype`:

```js
homework.toString();    // [object Object]
```

`homework.toString()` funciona aunque `homework` no tiene definido un método `toString()`; la delegación invoca `Object.prototype.toString()` en su lugar.

### Enlace de objeto

Una forma de crear un enlace prototipo de objeto es crear el objeto usando la utilidad `Object.create(..)`:

```js
var homework = {
    topic: "JS"
};

var otherHomework = Object.create(homework);

otherHomework.topic;        // "JS"
```

`Object.create(..)` espera que un argumento especifique un objeto para vincular el objeto recién creado.

| NOTA: |
| :--- |
| `Object.create(null)` crea un objeto que no está vinculado a un prototipo, por lo que es simplemente un objeto independiente; en algunas circunstancias puede ser preferible. |

La delegación a través de la cadena de prototipos solo se aplica a los accesos para buscar el valor en una propiedad. Si asigna a una propiedad de un objeto, eso se aplicará directamente al objeto, independientemente de dónde esté vinculado ese prototipo.

Considere:

```js
homework.topic;
// "JS"

otherHomework.topic;
// "JS"

otherHomework.topic = "Math";
otherHomework.topic;
// "Math"

homework.topic;
// "JS" -- not "Math"
```

La asignación a `topic` crea una propiedad de ese nombre directamente en `otherHomework`; no hay efecto en la propiedad `topic` en `homework`. La siguiente declaración accede a `otherHomework.topic`, y vemos la respuesta no delegada de esa nueva propiedad: `"Math"`.

### Enlace "Class"

Otra forma, francamente más complicada, de crear un objeto con un enlace prototipo es utilizar el patrón "clase prototípica", antes de que se añadiera ES6 `clase` al lenguaje (ver Capítulo 2," Clases ").

Considere:

```js
function Classroom() {
    // ..
}

Classroom.prototype.welcome = function hello() {
    console.log("Welcome, students!");
};

var mathClass = new Classroom();

mathClass.welcome();
// Welcome, students!
```

Todas las funciones hacen referencia por defecto a un objeto vacío en una propiedad llamada `prototype`. A pesar de los nombres confusos, este **no** es el *prototipo* de la función, --donde la función está vinculada al prototipo--, sino más bien el objeto prototipo a *enlazar* cuando se crean otros objetos llamando a la función con `new `.

Agregamos una propiedad `welcome` a ese objeto vacío `Classroom.prototype`, apuntando a una función `hello()`.

Entonces `new Classroom()` crea un nuevo objeto (asignado a `mathClass`), y el prototipo lo vincula al objeto existente `Classroom.prototype`.

Aunque `mathClass` no tiene una propiedad/función `welcome ()`, delega con éxito a `Classroom.prototype.welcome()`.

Este patrón de "clase de prototipo" ahora se desaconseja fuertemente, a favor de `class` de ES6 (ver Capítulo 2, "Clases"); Aquí está el mismo código expresado con `class`:

```js
class Classroom {
    constructor() {
        // ..
    }

    welcome() {
        console.log("Welcome, students!");
    }
}

var mathClass = new Classroom();

mathClass.welcome();
// Welcome, students!
```

Debajo de las cubiertas, el mismo enlace prototipo está conectado, pero esta sintaxis de `class` se ajusta al patrón de diseño orientado a la clase de manera mucho más limpia que las "clases de prototipo".

### Repaso a `this`

Una de las principales razones por las que `this` es compatible con el contexto dinámico basado en cómo se llama la función es para que el método invoque objetos que delegan a través de la cadena de prototipos siga manteniendo el esperado `this`.

Considere:

```js
var homework = {
    study() {
        console.log(`Please study ${this.topic}`);
    }
};

var jsHomework = Object.create(homework);
jsHomework.topic = "JS";
jsHomework.study();
// Please study JS

var mathHomework = Object.create(homework);
mathHomework.topic = "Math";
mathHomework.study();
// Please study Math
```

En los dos objetos `jsHomework` y `mathHomework` cada prototipo se vincula con el único objeto `homework`, que tiene la función `study()`. `jsHomework` y `mathHomework` tienen cada una su propia propiedad `topic`.

`jsHomework.study()` delega a `homework.study()`, pero su `this` (en `this.topic`) para esa ejecución se resuelve en `jsHomework` debido a cómo se llama la función, por lo que `this.topic` es `"JS"`. De manera similar para `mathHomework.study()` delegando a `homework.study()` pero aún resolviendo `this` a `mathHomework`, y por lo tanto `this.topic` como `"Math" `.

El fragmento de código anterior sería mucho menos útil si `this` se resolviera como `homework`. Sin embargo, en muchos otros lenguajes, parecería que `this` sería `homework` porque el método `study()` está definido en `homework()`.

A diferencia de muchos otros lenguajes, el hecho de que el `this` de JS sea dinámico es un componente crítico para permitir la delegación de prototipos, y de hecho `class`, funciona como se espera.

## Iteración

Dado que los programas se crean esencialmente para procesar datos (y tomar decisiones sobre esos datos), los patrones utilizados para recorrer los datos tienen un gran impacto en la legibilidad del programa.

El patrón iterador ha existido durante décadas, y sugiere un enfoque "estandarizado" para consumir datos de una fuente *fragmento* a la vez. La idea es que es más común y útil iterar la fuente de datos: manejar progresivamente la recopilación de datos procesando la primera parte, luego la siguiente, y así sucesivamente, en lugar de manejar todo el conjunto de una vez.

Imagine una estructura de datos que representa una consulta de base de datos relacional `SELECT`, que generalmente organiza los resultados en filas. Si esta consulta tuviera solo una o un par de filas, podría manejar todo el conjunto de resultados a la vez, y asignar cada fila a una variable local, y realizar cualquier operación en esos datos que fuera apropiada.

Pero si la consulta tiene 100 o 1000 (¡o más!) filas, necesitará un procesamiento iterativo para manejar estos datos (generalmente, un bucle).

El patrón iterador define una estructura de datos llamada "iterador" que tiene una referencia a una fuente de datos subyacente (como las filas de resultados de la consulta), que expone un método como `next()`. Llamar a `next()` devuelve la siguiente pieza de datos (es decir, un "registro" o "fila" de una consulta de base de datos).

No siempre sabe cuántos datos necesitará recorrer, por lo que el patrón generalmente indica la finalización mediante algún valor especial o excepción una vez que recorre todo el conjunto y *pasa el final*.

La importancia del patrón iterador radica en adherirse a una forma *estándar* de procesamiento de datos de forma iterativa, lo que crea un código más limpio y fácil de entender, en lugar de que cada estructura / fuente de datos defina su propia forma personalizada de manejar sus datos.

Después de muchos años de varios esfuerzos de la comunidad JS en torno a técnicas de iteración mutuamente acordadas, ES6 estandarizó un protocolo específico para el patrón iterador directamente en el lenguaje. El protocolo define un método `next()` cuyo retorno es un objeto llamado *resultado iterador*; el objeto tiene propiedades `value` y `done`, donde `done` es un valor booleano que es `false` hasta que se complete la iteración sobre la fuente de datos subyacente.

### Iteradores consumidores

Con el protocolo de iteración ES6 implementado, es factible consumir un origen de datos un valor a la vez, verificando después de cada llamada a `next()` para que `done` sea `true` y detener la iteración. Pero este enfoque es bastante manual, por lo que ES6 también incluyó varios mecanismos (sintaxis y API) para el consumo estandarizado de estos iteradores.

Uno de estos mecanismos es el bucle `for..of`:

```js
// given an iterator of some data source:
var it = /* .. */;

// loop over its results one at a time
for (let val of it) {
    console.log(`Iterator value: ${val}`);
}
// Iterator value: ..
// Iterator value: ..
// ..
```

| NOTE: |
| :--- |
| Omitiremos el equivalente del bucle manual aquí, ¡pero definitivamente es menos legible que el bucle `for..of`! |

Otro mecanismo que se usa a menudo para consumir iteradores es el operador `...`. Este operador en realidad tiene dos formas simétricas: *spread* y *rest* (o *collect*, como prefiero). La forma *spread* es un consumidor iterador.

Para *difundir* (spread) un iterador, debe tener *algo* para distribuir. Hay dos posibilidades en JS: un arreglo o una lista de argumentos para una llamada de función.

Un arreglo spread:

```js
// spread an iterator into an array,
// with each iterated value occupying
// an array element position.
var vals = [ ...it ];
```
Una llamada de función spread

```js
// spread an iterator into a function,
// call with each iterated value
// occupying an argument position.
doSomethingUseful( ...it );
```

En ambos casos, la forma de propagar-iterar de `...` sigue el protocolo de consumo de iterador (igual que el bucle `for..of`) para recuperar todos los valores disponibles de un iterador y colocarlos (también conocido como propagación) en el contexto receptor (arreglo, lista de argumentos).

### Iterables

El protocolo de consumo de iterador se define técnicamente para consumir *iterables*; un iterable es un valor que se puede repetir.

El protocolo crea automáticamente una instancia del iterador a partir de un iterable, y consume *solo esa instancia de iterador* hasta su finalización. Esto significa que un solo iterable podría consumirse más de una vez; cada vez, se crearía y usaría una nueva instancia de iterador.

Entonces, ¿dónde encontramos iterables?

ES6 definió la estructura de tipos de datos/colecciones básicos en JS como iterables. Esto incluye cadenas, matrices, mapas, conjuntos y otros.

So where do we find iterables?

Considere:

```js
// an array is an interable
var arr = [ 10, 20, 30 ];

for (let val of arr) {
    console.log(`Array value: ${val}`);
}
// Array value: 10
// Array value: 20
// Array value: 30
```

Dado que los arreglos son iterables, podemos copiar superficialmente un arreglo utilizando el consumo del iterador a través del operador de propagación `...`:

```js
var arrCopy = [ ...arr ];
```
También podemos iterar los caracteres en una cadena uno a la vez:

```js
var greeting = "Hello world!";
var chars = [ ...greeting ];

chars;
// [ "H", "e", "l", "l", "o", " ",
//   "w", "o", "r", "l", "d", "!" ]
```

Una estructura de datos `Map` usa objetos como claves, asociando un valor (de cualquier tipo) con ese objeto. Los mapas tienen una iteración predeterminada diferente a la que se ve arriba, ya que la iteración no solo supera los valores del mapa, sino que sus *entradas*:  -- una *entrada* es una tupla (arreglo de 2 elementos) que incluye tanto una clave como un valor.

Considere:

```js
// given two DOM elements, `btn1` and `btn2`

var buttonNames = new Map();
buttonNames.set(btn1,"Button 1");
buttonNames.set(btn2,"Button 2");

for (let [btn,btnName] of buttonNames) {
    btn.addEventListener("click",function onClick(){
        console.log(`Clicked ${btnName}`);
    });
}
```

En el bucle `for..of` sobre la iteración de mapa predeterminada, usamos la sintaxis `[btn, btnName]` (llamada "desestructuración de arreglos") para dividir cada tupla consumida en los pares clave/valor respectivos (`btn1` / `"Button 1"` y `btn2` / `"Button 2"`).

Cada uno de los iterables integrados en JS expone una iteración predeterminada, una que probablemente coincida con tu intuición. Pero también puedes elegir una iteración más específica si es necesario. Por ejemplo, si queremos consumir solo los valores del mapa anterior `buttonNames`, podemos llamar a `values​​()` para obtener un iterador de solo valores:

```js
for (let btnName of buttonNames.values()) {
    console.log(btnName);
}
// Button 1
// Button 2
```

O si queremos el índice *y* el valor de un arreglo en una iteración, podemos hacer un iterador de entradas con el método `entries()`:

```js
var arr = [ 10, 20, 30 ];

for (let [idx,val] of arr.entries()) {
    console.log(`[${idx}]: ${val}`);
}
// [0]: 10
// [1]: 20
// [2]: 30
```

En su mayor parte, todos los iterables integrados en JS tienen tres formas de iterar disponibles: solo claves (`keys()`), solo valores (`values()`) y entradas (`entries()`).

| NOTA: |
| :--- |
| Es posible que hayas notado que ocurrió un cambio matizado en esta discusión. Comenzamos hablando de consumir **iteradores**, pero luego pasamos a hablar sobre iterar sobre **iterables**. El protocolo de consumo de iteración espera un *iterable*, pero la razón por la que podemos proporcionar un *iterador* directo es que un iterador es solo un iterable de sí mismo. En otras palabras, cuando JS intenta crear una instancia de iterador **a partir de algo que ya es un iterador**, simplemente devuelve el iterador. |

Además de utilizar iterables integrados, también puedes asegurarte de que tus propias estructuras de datos se adhieran al protocolo de iteración; hacerlo significa que optas por la capacidad de consumir tus datos con bucles `for..of` y el operador `...`. "Estandarizar" en este protocolo significa código que en general es más fácilmente reconocible y legible.

## Preguntando Por Qué

El objetivo de este capítulo es que hay mucho más de JS tras bastidores de lo que es obvio al mirar la superficie.

A medida que comienzas a aprender y a conocer JS más de cerca, una de las habilidades más importantes que puedes practicar y reforzar es la curiosidad y el arte de preguntar "¿por qué?" cuando encuentras algo en el lenguaje.

Aunque este capítulo ha profundizado en algunos de los temas, muchos detalles se han descuidado por completo. Hay mucho más que aprender, y el camino hacia eso comienza con las preguntas *correctas* del código.

En el capítulo final de este libro, veremos brevemente cómo abordar el resto de la serie de libros *You Don't Know JS Yet*. Además, no te pierdas el Apéndice A, que tiene un código de práctica para revisar algunos de los principales temas tratados en este libro.
