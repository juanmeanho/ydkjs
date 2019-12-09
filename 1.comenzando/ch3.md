# Aún no conoces JS: Comenzando - 2da Edición
# Chapter 3: Explorando más profundo

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

Cuando la función `greeting(..)` termine de ejecutarse, normalmente esperaríamos que todas sus variables vayan al recolectador de basura (eliminadas de la memoria). Esperaríamos que cada `msg` desapareciera, pero no lo hacen. El motivo es el closure. Dado que las instancias de funciones internas todavía están vivas (asignadas a `hello` y `howdy` respectivamente), sus closures aún conservan las variables `msg`.

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

Cada instancia de la función interna `increaseCount()` se cierra sobre las variables '`count` y `step` desde el alcance de la función externa `counter(..)`. `step` permanece igual con el tiempo, pero `count` se actualiza en cada invocación de esa función interna. Como el closure se realiza sobre las variables y no solo sobre las instantáneas de los valores, estas actualizaciones se conservan.

El closure es más común cuando se trabaja con código asincrónico, como con devoluciones de llamada. Considerar:

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

Piense en un prototipo como un enlace entre dos objetos; El vínculo está oculto detrás de escena, aunque hay formas de exponerlo y observarlo. Este enlace prototipo ocurre cuando se crea un objeto; está vinculado a otro objeto que ya existe.

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

One of the main reasons `this` supports dynamic context based on how the function is called is so that method calls on objects which delegate through the prototype chain still maintain the expected `this`.

Consider:

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

`jsHomework.study()` delegates to `homework.study()`, but its `this` (in `this.topic`) for that execution resolves to `jsHomework` because of how the function is called, so `this.topic` is `"JS"`. Similarly for `mathHomework.study()` delegating to `homework.study()` but still resolving `this` to `mathHomework`, and thus `this.topic` as `"Math"`.


The above code snippet would be far less useful if `this` was resolved to `homework`. Yet, in many other languages, it would seem `this` would be `homework` because the `study()` method is indeed defined on `homework`.

Unlike many other languages, JS's `this` being dynamic is a critical component of allowing prototype delegation, and indeed `class`, to work as expected!

## Iteration

Since programs are essentially built to process data (and make decisions on that data), the patterns used to step through the data has a big impact on the program's readability.

The iterator pattern has been around for decades, and suggests a "standardized" approach to consuming data from a source one *chunk* at a time. The idea is that it's more common and helpful iterate the data source -- to progressively handle the collection of data by processing the first part, then the next, and so on, rather than handling the entire set all at once.

Imagine a data structure that represents a relational database `SELECT` query, which typically organizes the results as rows. If this query had only one or a couple of rows, you could handle the entire result set at once, and assign each row to a local variable, and perform whatever operations on that data that were appropriate.

But if the query has 100 or 1000 (or more!) rows, you'll need iterative processing to deal with this data (typically, a loop).

The iterator pattern defines a data structure called an "iterator" that has a reference to an underlying data source (like the query result rows), which exposes a method like `next()`. Calling `next()` returns the next piece of data (ie, a "record" or "row" from a database query).

You don't always know how many pieces of data that you will need to iterate through, so the pattern typically indicates completion by some special value or exception once you iterate through the entire set and *go past the end*.

The importance of the iterator pattern is in adhering to a *standard* way of processing data iteratively, which creates cleaner and easier to understand code, as opposed to having every data structure/source define its own custom way of handling its data.

After many years of various JS community efforts around mutually-agreed-upon iteration techniques, ES6 standardized a specific protocol for the iterator pattern directly in the language. The protocol defines a `next()` method whose return is an object called an *iterator result*; the object has `value` and `done` properties, where `done` is a boolean that is `false` until the iteration over the underlying data source is complete.

### Consuming Iterators

With the ES6 iteration protocol in place, it's workable to consume a data source one value at a time, checking after each `next()` call for `done` to be `true` to stop the iteration. But this approach is rather manual, so ES6 also included several mechanisms (syntax and APIs) for standardized consumption of these iterators.

One such mechanism is the `for..of` loop:

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
| We'll omit the manual loop equivalent here, but it's definitely less readable than the `for..of` loop! |

Another mechanism that's often used for consuming iterators is the `...` operator. This operator actually has two symmetrical forms: *spread* and *rest* (or *gather*, as I prefer). The *spread* form is an iterator-consumer.

To *spread* an iterator, you have to have *something* to spread it into. There are two possibilities in JS: an array or an argument list for a function call.

An array spread:

```js
// spread an iterator into an array,
// with each iterated value occupying
// an array element position.
var vals = [ ...it ];
```

A function call spread:

```js
// spread an iterator into a function,
// call with each iterated value
// occupying an argument position.
doSomethingUseful( ...it );
```

In both cases, the iterator-spread form of `...` follows the iterator-consumption protocol (the same as the `for..of` loop) to retrieve all available values from an iterator and place (aka, spread) them into the receiving context (array, argument list).

### Iterables

The iterator-consumption protocol is technically defined for consuming *iterables*; an iterable is a value that can be iterated over.

The protocol automatically creates an iterator instance from an iterable, and consumes *just that iterator instance* to its completion. This means a single iterable could be consumed more than once; each time, a new iterator instance would be created and used.

So where do we find iterables?

ES6 defined the basic data structure/collection types in JS as iterables. This includes strings, arrays, maps, sets, and others.

Consider:

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

Since arrays are iterables, we can shallow-copy an array using iterator consumption via the `...` spread operator:

```js
var arrCopy = [ ...arr ];
```

We can also iterate the characters in a string one at a time:

```js
var greeting = "Hello world!";
var chars = [ ...greeting ];

chars;
// [ "H", "e", "l", "l", "o", " ",
//   "w", "o", "r", "l", "d", "!" ]
```

A `Map` data structure uses objects as keys, associating a value (of any type) with that object. Maps have a different default iteration than seen above, in that the iteration is not just over the map's values but instead its *entries* -- an *entry* is a tuple (2-element array) including both a key and a value.

Consider:

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

In the `for..of` loop over the default map iteration, we use the `[btn,btnName]` syntax (called "array destructuring") to break down each consumed tuple into the respective key/value pairs (`btn1` / `"Button 1"` and `btn2` / `"Button 2"`).

Each of the built-in iterables in JS expose a default iteration, one which likely matches your intution. But you can also choose a more specific iteration if necessary. For example, if we want to consume only the values of the above `buttonNames` map, we can call `values()` to get a values-only iterator:

```js
for (let btnName of buttonNames.values()) {
    console.log(btnName);
}
// Button 1
// Button 2
```

Or if we want the index *and* value in an array iteration, we can make an entries iterator with the `entries()` method:

```js
var arr = [ 10, 20, 30 ];

for (let [idx,val] of arr.entries()) {
    console.log(`[${idx}]: ${val}`);
}
// [0]: 10
// [1]: 20
// [2]: 30
```

For the most part, all built-in iterables in JS have three iterator forms available: keys-only (`keys()`), values-only (`values()`), and entries (`entries()`).

| NOTE: |
| :--- |
| You may have noticed a nuanced shift that occured in this discussion. We started by talking about consuming **iterators**, but then switched to talking about iterating over **iterables**. The iteration-consumption protocol expects an *iterable*, but the reason we can provide a direct *iterator* is, an iterator is just an iterable of itself! In other words, when JS tries to create an iterator instance **from something that's already an iterator**, it just returns the iterator. |

Beyond just using built-in iterables, you can also ensure your own data structures adhere to the iteration protocol; doing so means you opt into the ability to consume your data with `for..of` loops and the `...` operator. "Standardizing" on this protocol means code that is overall more readily recognizable and readable.

## Asking Why

The intended take-away from this chapter is that there's a lot more to JS under the hood than is obvious from glancing at the surface.

As you are *getting started* learning and knowing JS more closely, one of the most important skills you can practice and bolster is curiosity, and the art of asking "why?" when you encounter something in the language.

Even though this chapter has gone deeper on some of the topics, many details have been entirely skimmed over. There's much more to learn here, and the path to that starts with you asking the *right* questions of the code.

In the final chapter of this book, we're going to briefly look at how to approach the rest of the *You Don't Know JS Yet* book series. Also, don't miss Appendix A, which has some practice code to review some of the main topics covered in this book.
