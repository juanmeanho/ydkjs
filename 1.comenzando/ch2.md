# Aún no conoces JS: Comenzando - 2da Edición
# Capítulo 2: Conociendo a JS

| NOTA DEL AUTOR: |
| :--- |
| Trabajo en progreso |

No hay sustituto en el aprendizaje de JS que no sea simplemente comenzar a escribir JS. Para hacer eso, necesitas saber cómo funciona el lenguaje. Entonces, familiaricémonos con su sintaxis.

Este capítulo no es una referencia exhaustiva sobre cada bit de sintaxis del lenguaje JS. Tampoco pretende ser una introducción a "JS".

En cambio, solo vamos a examinar algunas de las principales áreas temáticas del lenguaje. Nuestro objetivo es obtener una mejor *sensación* para que podamos avanzar escribiendo nuestros propios programas con más confianza.

Sin embargo, no esperes que este capítulo sea una lectura rápida. Es largo y hay muchos detalles para masticar. Tómate tu tiempo.

| TIP: |
| :--- |
| Si aún te estás familiarizando con JS, te sugiero que reserves mucho tiempo extra para trabajar en este capítulo. Toma cada sección, reflexiona y explora el tema por un tiempo. Mira a través de los programas JS existentes y compara lo que ves en ellos con el código y las explicaciones (¡y opiniones!) presentadas aquí. Obtendráa mucho más del resto del libro y la serie con una base sólida de la *naturaleza* de JS. |

## Archivos Como Programas

Casi todos los sitios web (aplicaciones web) que usas se componen de muchos archivos JS diferentes (generalmente con la extensión de archivo .js). Es tentador pensar en toda (la aplicación) como un solo programa. Pero JS lo ve de manera diferente.

En JS, cada archivo independiente es su propio programa separado.

La razón por la que esto importa se debe principalmente al manejo de errores. Dado que JS trata los archivos como programas, un archivo puede fallar (durante el análisis / compilación o ejecución) y eso no impedirá necesariamente que se procese el siguiente archivo. Obviamente, si su aplicación depende de cinco archivos .js, y uno de ellos falla, la aplicación general probablemente solo operará parcialmente, en el mejor de los casos. Es importante asegurarse de que cada archivo funcione correctamente y que, en la medida de lo posible, manejen las fallas en otros archivos de la manera más elegante posible.

Puede sorprenderte considerar archivos .js separados como programas JS separados. Desde la perspectiva de su uso de una aplicación, seguramente parece un gran programa. Esto se debe a que la ejecución de la aplicación permite que estos *programas* individuales cooperen y actúen como un solo programa.

La única forma en que múltiples archivos .js independientes actúen como un solo programa es compartiendo su estado (y acceso a su funcionalidad pública) a través del "alcance global". Se mezclan en este espacio de nombres de alcance global, por lo que en tiempo de ejecución actúan como un todo.

Desde ES6, JS también ha admitido un formato de módulo además del formato típico de programa JS independiente. Los módulos también están basados ​​en archivos. Si un archivo se carga mediante un mecanismo de carga de módulos, como una declaración `import` o una etiqueta `<script type=module>`, todo su código se trata como un solo módulo.

Aunque normalmente no pensarías en un módulo -- básicamente, una colección del estado y métodos públicos para operar en ese estado -- como un programa independiente, JS de hecho todavía trata cada módulo por separado. De forma similar a cómo el "alcance global" permite que los archivos independientes se mezclen en tiempo de ejecución, la importación de un módulo en otro permite la interoperación de tiempo de ejecución entre ellos.

Independientemente de qué patrón de organización de código (y mecanismo de carga) se use para un archivo, independiente o módulo, aún debes considerar cada archivo como su propio (mini) programa, que luego puede cooperar con otros (mini) programas para realizar Las funciones de tu aplicación general.

## Valores

La unidad de información más fundamental en un programa es un valor. Los valores son datos. Son cómo el programa mantiene el estado. Los valores vienen en dos formas en JS: **primitivo** y **objeto**.

Los valores están incrustados en nuestros programas usando *literales*. Por ejemplo:

```js
console.log("My name is Kyle.");
// My name is Kyle.
```

En este programa, el valor `"Mi nombre es Kyle"` es un literal de cadena primitivo; Las cadenas son colecciones ordenadas de caracteres, generalmente utilizadas para representar palabras y oraciones.

Utilicé el carácter de comillas dobles `"` para *delimitar* (rodear, separar, definir) el valor de la cadena. Pero también podría haber utilizado el carácter de comillas simples `'` La elección de qué carácter de comillas usar es completamente estilística. Lo importante, en aras de la legibilidad del código y el mantenimiento, es elegir uno y usarlo de manera consistente a lo largo del programa.

Otra opción para delimitar un literal de cadena es utilizar el carácter de retroceso (backtick) `` ` ``. Sin embargo, esta elección no es meramente estilística; También hay una diferencia de comportamiento. 

Considerar:

```js
console.log("My name is ${firstName}.");
// My name is ${firstName}.

console.log('My name is ${firstName}.');
// My name is ${firstName}.

console.log(`My name is ${firstName}.`);
// My name is Kyle.
```

Suponiendo que este programa ya haya definido una variable `firstName` con el valor de cadena `"Kyle"`, el `` ` `` -delimita  la cadena y luego resuelve la expresión variable (indicada con `${..}`) a su valor actual. Esto se llama **interpolación**.

La cadena delimitada por el backtick `` ` `` se puede usar sin incluir expresiones interpoladas, pero eso anula todo el propósito de esa sintaxis adicional.


```js
console.log(`Am I confusing you by omitting interpolation?`);
// Am I confusing you by omitting interpolation?
```

El mejor enfoque es usar `"` o `'` (de nuevo, ¡elija uno y manténgalo!) Para cadenas *a menos que necesite* interpolación; reserve `` ` `` solo para cadenas que incluirán expresiones interpoladas.

Además de las cadenas, los programas JS a menudo contienen otros valores literales primitivos, como booleanos y números.

```js
while (false) {
    console.log(3.141592);
}
```

`while` representa un tipo de bucle, una forma de repetir operaciones *mientras* su condición sea verdadera.

En este caso, el bucle nunca se ejecutará (y no se imprimirá nada), porque utilizamos el valor booleano `false` como condicional del bucle. `verdadero` habría resultado en un bucle que continúa para siempre, ¡así que ten cuidado!

El número `3.141592` es, como ya sabrá, una aproximación matemática de PI a los primeros seis dígitos. Sin embargo, en lugar de incrustar dicho valor, normalmente usaría el valor predefinido `Math.PI` para ese propósito. Otra variación en los números es el tipo primitivo 'bigint' (entero grande), que se utiliza para almacenar números arbitrariamente grandes.

Los números se usan con mayor frecuencia en programas para contar pasos, como iteraciones de bucle y acceder a información en posiciones numéricas (es decir, un índice de matriz). Los valores de los objetos representan una colección de varios valores. Las matrices son un subconjunto especial donde se ordena la colección, por lo tanto, la posición de un elemento (índice) es numérica.

Por ejemplo, si hubiera una matriz llamada `names`, podríamos acceder al elemento en la segunda posición así:

```js
console.log(`My name is ${ names[1] }.`);
// My name is Kyle.
```

Utilizamos `1` para el elemento en la segunda posición, en lugar de` 2`, porque como en la mayoría de los lenguajes de programación, los índices de matriz JS están basados ​​en 0 (`0` es la primera posición).

Por el contrario, los objetos regulares son colecciones de valores desordenados y codificados; en otras palabras, accede al elemento por un nombre de ubicación de cadena (también conocido como "clave" o "propiedad") en lugar de por su posición numérica (como con las matrices. 

Por ejemplo:


```js
console.log(`My name is ${ name.first }.`);
```

Aquí, `name` representa un objeto, y `first` representa el nombre de una ubicación de información en ese objeto (colección de valores). Otra opción de sintaxis que accede a la información en un objeto por su propiedad / clave utiliza los corchetes `[ ]`, como `name["first"]`.

Además de las cadenas, los números y los valores booleanos, otros dos valores *primitivos* en los programas JS son `null` y `undefined`. Si bien existen diferencias entre ellos (algunos históricos y otros contemporáneos), en su mayor parte ambos valores sirven para indicar *vacío* (o ausencia) de un valor.

Muchos desarrolladores prefieren tratarlos a ambos de manera consistente, es decir, se supone que los valores son indistinguibles. Si se tiene cuidado, esto a menudo es posible. Sin embargo, es más seguro y mejor usar solo `undefined` como el único valor vacío, a pesar de que `null` parece atractivo porque es más corto de escribir.

```js
while (value != undefined) {
    console.log("Still got something!");
}
```
El valor primitivo final a tener en cuenta es `Symbol`, que es un valor de propósito especial que se comporta como un valor oculto no cuestionable. Los símbolos se usan casi exclusivamente como teclas especiales en los objetos.

```js
hitchhikersGuide[ Symbol("meaning of life") ];
// 42
```

Para distinguir los valores, el operador `typeof` le dice su tipo incorporado, si es primitivo, o de lo contrario `"objeto"`:

```js
typeof 42;                // "number"
typeof "abc";             // "string"
typeof true;              // "boolean"
typeof undefined;         // "undefined"
typeof null;              // "object" -- oops, JS bug!
typeof { "a": 1 };        // "object"
typeof [1,2,3];           // "object"
typeof function foo(){};  // "function"
```

| NOTA: |
| :--- |
| `typeof null` desafortunadamente devuelve `"object" `en lugar del esperado" `"null"`. Además, `typeof` devuelve` "function"` para funciones,pero no devuelve un `"array"` esperado para arrays. |


## Variables

Para ser explícito sobre algo que puede no haber sido obvio en la sección anterior: en los programas JS, los valores pueden aparecer como valores literales (como ilustran muchos de los ejemplos anteriores), o pueden mantenerse en variables; Piense en las variables como solo contenedores de valores.

Las variables tienen que ser declaradas (creadas) para ser utilizadas. Hay varias formas de sintaxis que declaran variables (también conocidas como "identificadores"), y cada forma tiene comportamientos implícitos diferentes.

Por ejemplo, considere la declaración `var`:

```js
var name = "Kyle";
var age;
```

La palabra clave `var` declara que una variable se usará en esa parte del programa, y ​​opcionalmente permite una asignación inicial de un valor.

Otra palabra clave similar es `let`:

```js
let name = "Kyle";
let age;
```

La palabra clave `let` tiene algunas diferencias con `var`, siendo la más obvia que `let` permite un acceso más limitado a la variable que `var`. Esto se denomina "alcance de bloque" en lugar de alcance normal o de función.

Considere:

```js
var adult = true;

if (adult) {
    var name = "Kyle";
    let age = 39;
    console.log("Shhh, this is a secret!");
}

console.log(name);
// Kyle

console.log(age);
// Error!
```

El intento de acceder a `age` fuera de la instrucción `if` da como resultado un error, porque `age` tenía un alcance de bloque a `if`, mientras que `name` no.

El alcance de bloque es muy útil para limitar la extensión de las declaraciones de variables en nuestros programas, lo que ayuda a evitar la superposición accidental de sus nombres.

Pero `var` sigue siendo útil porque comunica "esta variable será vista por un alcance más amplio". Ambas formas de declaración pueden ser apropiadas en cualquier parte de un programa, dependiendo de las circunstancias.

| NOTA: |
| :--- |
| Es muy común sugerir que `var` debe evitarse a favor de `let` (o `const`!), generalmente debido a la confusión percibida sobre cómo ha funcionado el comportamiento de alcance de `var` desde el comienzo de JS. Creo que esto es un consejo demasiado restrictivo y, en última instancia, inútil. Se supone que no puede aprender y usar una función correctamente en combinación con otras funciones. ¡Creo que *puedes* y *debes* aprender cualquier función disponible, y usarla cuando sea apropiado! |

Un tercera forma de declaración es `const`. Es como `let` pero tiene una limitación adicional de que se le debe dar un valor en el momento en que se declara, y no se le puede reasignar un valor diferente más adelante.

Considere:

```js
const myBirthday = true;
let age = 39;

if (myBirthday) {
    age = age + 1;    // OK!
    myBirthday = false;  // Error!
}
```

No se permite reasignar la constante `myBirthday`.

Las variables declaradas `const` no son "inmutables", simplemente no pueden reasignarse. No es aconsejable usar `const` con valores de objeto, porque esos valores aún se pueden cambiar a pesar de que la variable no se puede reasignar. Esto lleva a una posible confusión en el futuro, por lo que creo que es aconsejable evitar situaciones como:

```js
const actors = [ "Morgan Freeman", "Jennifer Anniston" ];

actors[2] = "Tom Cruise";   // OK :(

actors = [];                // Error!
```

El mejor uso semántico de un `const` es cuando tienes un valor primitivo simple al que quieres darle un nombre útil, como usar `myBirthday` en lugar de `true`. Esto hace que los programas sean más fáciles de leer.


| TIP: |
| :--- |
| Si te limitas a usar `const` solo con valores primitivos, ¡nunca tendrás la confusión de la diferencia entre reasignación (no permitido) y mutación (permitido)! Esa es la mejor y más segura forma de usar `const`. |

Además de `var` /` let` / `const`, hay otras formas sintácticas que declaran identificadores (variables) en varios ámbitos. Por ejemplo:


```js
function hello(name) {
    console.log(`Hello, ${name}.`);
}

hello("Kyle");
// Hello, Kyle.
```

Aquí, el identificador `hello` se crea en el ámbito externo, y también se asocia automáticamente para que haga referencia a la función. Pero el parámetro con nombre `name` se crea solo dentro de la función y, por lo tanto, solo es accesible dentro del alcance de esa función.

Tanto `hello` como `name` actúan generalmente como si fueran declaradas con `var`.

Otra sintaxis que declara una variable es la cláusula `catch` de una instrucción `try..catch`:


```js
try {
    someError();
}
catch (err) {
    console.log(err);
}
```

El `err` es una variable de ámbito de bloque que existe solo dentro de la cláusula` catch`, como si se hubiera declarado con `let`.

## Funciones

La palabra "función" tiene una variedad de significados en la programación. Por ejemplo, en el mundo de la Programación Funcional, la "función" tiene una definición matemática precisa e implica un estricto conjunto de reglas a cumplir.

En JS, deberíamos considerar "función" para tomar el significado más amplio de "procedimiento": una colección de declaraciones que pueden invocarse una o más veces, pueden proporcionarse algunas entradas y pueden devolver una o más salidas.

En los buenos tiempos de JS, solo había una forma de definir una función:

```js
function awesomeFunction(coolThings) {
    // ..
    return amazingStuff;
}
```

Esto se llama declaración de función porque aparece como una declaración en sí misma, no como una expresión que es parte de otra declaración. La asociación entre el identificador 'awesomeFunction' y el valor de la función ocurre inmediatamente durante la fase de compilación del código, antes de que se ejecute ese código.

Por el contrario, una expresión de función se puede definir así:


```js
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function(coolThings) {
    // ..
    return amazingStuff;
};
```

Esta función es una expresión que se asigna a la variable `awesomeFunction`. A diferencia de la forma de declaración de función, una expresión de función no está asociada con su identificador hasta esa declaración durante el tiempo de ejecución.

La expresión de función anterior se conoce como *expresión de función anónima*, ya que no tiene un identificador de nombre entre la palabra clave `function` y la lista de parámetros `(..)`. Este punto confunde a muchos desarrolladores de JS porque a partir de ES6, JS realiza una "inferencia de nombre" en una función anónima:

```js
awesomeFunction.name;
// "awesomeFunction"
```

La propiedad `name` de una función revelará su nombre directamente dado (en el caso de una declaración) o su nombre inferido en el caso de una expresión de función anónima. Las herramientas de desarrollador generalmente usan ese valor al inspeccionar un valor de función o al informar un seguimiento de la pila de errores.

Entonces, incluso una expresión de función anónima *podría* obtener un nombre. Sin embargo, la inferencia de nombres solo ocurre en casos limitados, como cuando se asigna la expresión de función (con `=`). Si pasa una expresión de función como argumento a una llamada de función, por ejemplo, no se produce inferencia de nombre, la propiedad `name` será una cadena vacía y la consola del desarrollador generalmente informará "(función anónima)".

Incluso si se infiere un nombre, **sigue siendo una función anónima.** ¿Por qué? Debido a que el nombre inferido es un valor de cadena de metadatos, no es un identificador disponible para referirse a la función. Una función anónima no tiene un identificador para usar para referirse a sí misma desde dentro de sí misma: para la recursividad, la desvinculación de eventos, etc.

Compare la forma de expresión de función anónima con:

```js
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function someName(coolThings) {
    // ..
    return amazingStuff;
};

awesomeFunction.name;
// "someName"
```

Esta expresión de función es una *expresión de función nombrada*, ya que el identificador `someName` está directamente asociado con la expresión de función en tiempo de compilación; la asociación con el identificador `awesomeFunction` todavía no ocurre hasta el tiempo de ejecución en el momento de esa declaración. Esos dos identificadores no tienen que coincidir; a veces tiene sentido que sean diferentes, otras veces es mejor que sean iguales.

Observe también que el nombre explícito de la función, el identificador `someName`, tiene prioridad al asignar un *nombre* para la propiedad `name`.

¿Deben las expresiones de funciones ser nombradas o anónimas? Las opiniones varían ampliamente sobre esto. La mayoría de los desarrolladores tienden a no preocuparse por el uso de funciones anónimas. Son más cortas e indudablemente más comunes en la amplia esfera del código JS.


| MY OPINION: |
| :--- |
| Si existe una función en tu programa, es porque tiene un propósito; de lo contrario, sácala! Si tiene un propósito, tiene un nombre natural que describe ese propósito. Si tiene un nombre, tu, el autor del código, debes incluir ese nombre en el código, para que el lector no tenga que inferir ese nombre al leer y ejecutar mentalmente el código fuente de esa función. Incluso un cuerpo de función trivial como `x * 2` tiene que leerse para inferir un nombre como "doble "o" multPor2 "; ese breve trabajo mental adicional es innecesario cuando solo puede tomarse un segundo para nombrar la función "doble" o "multPor2" *una vez*, ahorrando al lector ese trabajo mental repetido cada vez que se lea en el futuro.|

Lamentablemente, en algunos aspectos, hay muchas otras formas de definición de funciones en JS en 2019.

Aquí hay algunas otras formas de declaración:

```js
// generator function declaration
function *two() { .. }

// async function declaration
async function three() { .. }

// async generator function declaration
async function *four() { .. }

// named function export declaration (ES6 modules)
export function five() { .. }
```

Y aquí hay algunas de las (¡muchas!) formas de expresión de funciones :

```js
// IIFE
(function(){ .. })();
(function namedIIFE(){ .. })();

// asynchronous IIFE
(async function(){ .. })();
(async function namedAIIFE(){ .. })();

// arrow function expressions
var f;
f = () => 42;
f = x => x * 2;
f = (x) => x * 2;
f = (x,y) => x * y;
f = x => ({ x: x * 2 });
f = x => { return x * 2; };
f = async x => {
    var y = await doSomethingAsync(x);
    return y * 2;
};
someOperation( x => x * 2 );
// ..
```

Tenga en cuenta que las expresiones de función de flecha son **sintácticamente anónimas**, lo que significa que la sintaxis no proporciona una forma de asignar un identificador de nombre directo para la función. La expresión de función puede obtener un nombre inferido, pero solo si es una de las formas de asignación, no en la forma (¡más común!) de pasar como un argumento de llamada de función (como en la última línea del fragmento).

Keep in mind that arrow function expressions are **syntactically anonymous**, meaning the syntax doesn't provide a way to provide a direct name identifier for the function. The function expression may get an inferred name, but only if it's one of the assignment forms, not in the (more common!) form of being passed as a function call argument (as in the last line of the snippet).

Las funciones también se pueden especificar en definiciones de clase y definiciones literales de objeto. Normalmente se les conoce como "métodos" cuando están en estas formas, aunque en JS este término no tiene mucha diferencia observable sobre "función".

```js
class SomethingKindaGreat {
    // class methods
    coolMethod() { .. }   // no commas!
    boringMethod() { .. }
}

var EntirelyDifferent = {
    // object methods
    coolMethod() { .. },   // commas!
    boringMethod() { .. },

    // (anonymous) function expression property
    oldSchool: function() { .. }
};
```
¡Uf! Esas son muchas formas diferentes de definir funciones.

No hay atajos sencillos aquí; solo debes familiarizarte con todas las formas de funciones para poder reconocerlas en el código existente y usarlos adecuadamente en el código que escribas. ¡Estudíalos de cerca y practica!

## Comparaciones

Tomar decisiones en los programas requiere comparar valores para determinar su identidad y relación entre sí. JS tiene varios mecanismos para permitir la comparación de valores, así que echemos un vistazo más de cerca a ellos.

### Igual...ar

La comparación más común en los programas JS hace la pregunta, "¿es este valor X *lo mismo que* ese valor Y?" Sin embargo, ¿qué significa exactamente "lo mismo que" para JS?

Por razones ergonómicas e históricas, el significado es más complicado que la obvia *identidad exacta* o coincidencia. A veces, una comparación de igualdad pretende una coincidencia *exacta*, pero otras veces la comparación deseada es un poco más amplia, lo que permite una coincidencia *muy similar* o *intercambiable*. En otras palabras, debemos ser conscientes de las diferencias matizadas entre una comparación de **igualdad** y una comparación de **equivalencia**.

Si ha pasado algún tiempo trabajando y leyendo sobre JS, ciertamente ha visto el llamado operador de "triple igualdad" `===`, también descrito como el operador de "igualdad estricta". Parece bastante sencillo, ¿verdad? Seguramente, "estricto" significa estricto, como en estrecho y *exacto*.

No exacta*mente*.

Sí, la mayoría de los valores que participan en una comparación de igualdad `===` encajarán con esa intuición *exactamente igual*. Considere algunos ejemplos:

```js
3 === 3.0;              // true
"yes" === "yes";        // true
null === null;          // true
false === false;        // true

42 === "42";            // false
"hello" === "Hello";    // false
true === 1;             // false
0 === null;             // false
"" === null;            // false
null === undefined;     // false
```

| NOTA: |
| :--- |
| Otra forma en que a menudo se describe la comparación de igualdad de `===` es que se está "verificando tanto el valor como el tipo". En varios de los ejemplos anteriores, como `42 === "42"`, el *tipo* de ambos valores - número, cadena, etc. - parece ser el factor distintivo. Sin embargo, hay más que eso. **Todas las** comparaciones de valores en JS consideran el tipo de valores que se comparan, no *solo* el operador `===`. Específicamente, `===` no permite ningún tipo de conversión de tipo (también conocido como "coerción") en su comparación, donde otras comparaciones JS *sí* permiten la coerción. |

Pero el operador `===` tiene algunos matices, un hecho que muchos desarrolladores de JS pasan por alto, en detrimento de ellos. El operador `===` está diseñado para *mentir* en dos casos de valores especiales: `NaN` y `-0`. 

Considere:


```js
NaN === NaN;            // false
0 === -0;               // true
```

En el caso de `NaN`, el operador `===` *miente* y dice que una aparición de `NaN` no es igual a otro `NaN`. En el caso de `-0` - sí, ¡este es un valor real y distinto que puedes usar intencionalmente en tus programas! - el operador `===` *miente* y dice que es igual al valor regular `0`.

| TIP: |
| :--- |
| Al hacer tales comparaciones, dado que *mentir* es probablemente molesto, no use `===`. Para las comparaciones `NaN`, use la utilidad `Number.isNaN(..)`, que no *miente*. Para la comparación de `-0`, use la utilidad` Object.is(..)`, que tampoco *miente*. `Object.is(..)` también puede usarse para verificaciones no *mentirosas* de `NaN`, si lo prefiere. Humorísticamente, podrías pensar en `Object.is(..)` como el "cuádruple-igual" `====`, ¡la comparación muy, muy estricta!|

Hay razones históricas y técnicas más profundas para estas *mentiras*, pero eso no cambia el hecho de que `===` no es una comparación *estrictamente exacta*, en el sentido *más estricto*.

La historia se vuelve aún más complicada cuando consideramos las comparaciones de valores de objetos (no primitivos). 

Considere:

```js
[ 1, 2, 3 ] === [ 1, 2, 3 ];    // false
{ a: 42 } === { a: 42 }         // false
(x => x * 2) === (x => x * 2)   // false
```

¿Que está pasando aqui?

Puede parecer razonable suponer que una verificación de igualdad considera la *naturaleza* o *contenido* del valor; después de todo, `42 === 42` considera el valor real de` 42` y lo compara. Pero cuando se trata de objetos, una comparación consciente del contenido generalmente se conoce como "igualdad estructural".

JS no define `===` como *igualdad estructural* para valores de objeto. En cambio, `===` usa *igualdad de identidad* para valores de objeto.

En JS, todos los valores de los objetos se mantienen por referencia, se asignan y se pasan por copia de referencia, **y** en nuestra discusión actual, se comparan por igualdad de referencia (identidad). Considerar:


```js
var x = [ 1, 2, 3 ];

// assignment is by reference-copy, so
// y references the *same* array as x,
// not another copy of it.
var y = x;

y === x;              // true
y === [ 1, 2, 3 ];    // false
x === [ 1, 2, 3 ];    // false
```

En este fragmento, `y === x` es verdadero porque ambas variables tienen una referencia a la misma matriz inicial. Pero las comparaciones `=== [1,2,3]` fallan porque `y` y` x`, respectivamente, se comparan con las nuevas matrices *diferentes* `[1,2,3]`. La estructura de la matriz y los contenidos no importan en esta comparación, solo la **identidad de referencia**.

JS no proporciona un mecanismo para la comparación de igualdad estructural de valores de objeto, solo comparación de identidad de referencia. Para hacer una comparación de igualdad estructural, deberá implementar las comprobaciones usted mismo.

Pero cuidado, es más complicado de lo que supones. Por ejemplo, ¿cómo podrías determinar si dos referencias de función son "estructuralmente equivalentes"? Incluso convertir en cadenas su codigo para compararlo no tomaría en cuenta cosas como los closures. ¡JS no proporciona una comparación de igualdad estructural porque es casi intratable manejar todos los casos!

### Comparaciones coercitivas

Pocas funciones JS atraen más ira en la comunidad JS más amplia que el operador `==`, generalmente denominado operador de "igualdad suelta". La mayoría de todos los artículos y el discurso público sobre JS condenan a este operador como mal diseñado y peligroso / lleno de errores cuando se usa en programas JS. Incluso el creador del lenguaje, Brendan Eich, se ha lamentado de cómo fue diseñado, calificandolo como un gran error.

Por lo que puedo decir, la mayor parte de esta frustración proviene de una lista bastante corta de casos confusos, pero un problema más profundo es la idea errónea extremadamente generalizada de que realiza sus comparaciones sin considerar los tipos de sus valores comparados.

El operador `==` realiza una comparación de igualdad de manera similar a como lo realiza el `===`. De hecho, ambos operadores consideran el tipo de valores que se comparan. Y si la comparación es entre el mismo tipo de valor, tanto `==` como `===` **hacen exactamente lo mismo, sin ninguna diferencia.**

Si los tipos de valores que se comparan son diferentes, el `==` difiere de `===` en que permite la coerción antes de la comparación. En otras palabras, ambos quieren comparar valores de tipos similares, pero `==` permite las conversiones de tipos *primero*, y una vez que los tipos se han convertido para ser iguales en ambos lados, entonces `==` hace lo mismo que `===`. En lugar de "igualdad suelta", el operador `==` debe describirse como "igualdad coercitiva".

Considere:

```js
42 == "42";             // true
1 == true;              // true
```

En ambas comparaciones, los tipos de valores son diferentes, por lo que `==` hace que los valores no numéricos (`"42"` y `true`) se conviertan en números (`42` y `1`, respectivamente) antes de hacer las comparaciones.

El solo hecho de conocer esta naturaleza de `==` te ayuda a evitar la mayoría de los casos problemáticos, como mantenerse alejado de problemas como `" "== 0` o `0 == false`.

Puedes estar pensando, "¡Oh, bueno, siempre evitaré cualquier comparación de igualdad coercitiva (usando `===` en su lugar) para evitar esos casos curiosos"! Eh, lo siento, eso no es tan probable como cabría esperar.

Hay muchas posibilidades de que utilices operadores de comparación relacional como `<`, `>` (e incluso `<=` y `> =`).

Al igual que `==`, estos operadores funcionarán como si fueran "estrictos" si los tipos que se comparan relacionalmente ya coinciden, pero permitirán primero la coerción (generalmente, a números) si los tipos difieren.

Considere:

```js
var arr = [ "1", "10", "100", "1000" ];
for (let i = 0; i < arr.length && arr[i] < 500; i++) {
    // will run 3 times
}
```

La comparación `i < arr.length` está "a salvo" de la coerción porque `i` y `arr.length` son siempre números. Sin embargo, `arr[i] < 500` invoca la coerción, porque los valores `arr[i]` son todas cadenas. Esas comparaciones se convierten así en `1 < 500`,` 10 < 500`, `100 < 500` y `1000 < 500`. Como ese último es falso, el ciclo se detiene después de su tercera iteración.

Estos operadores relacionales generalmente usan comparaciones numéricas, excepto en el caso en que **ambos** valores que se comparan ya sean cadenas; en este caso, usan la comparación alfabética (similar a un diccionario) de las cadenas:

```js
var x = "10";
var y = "9";

x < y;      // true, watch out!
```

No hay forma de obtener estos operadores relacionales para evitar la coerción, aparte de nunca usar tipos no coincidentes en las comparaciones. Tal vez sea un objetivo admirable, pero es bastante probable que te encuentres con un caso en el que los tipos *pueden* diferir.

El enfoque más sabio no es evitar comparaciones coercitivas, sino abrazar y aprender sus entresijos.

#### Condicionales

Las expresiones condicionales, como las calculadas en las declaraciones `if`, así como los bucles `while` y `for`, realizan una comparación de valor implícito. ¿Pero de qué tipo? ¿Es "estricto" o "coercitivo"? Ambos, en realidad.

Considere:

```js
var x = 1;

if (x) {
    // will run!
}

while (x) {
    // will run, once!
    x = false;
}
```

Puede pensar en esta `(x)` expresion condicional así:

```js
var x = 1;

if (x == true) {
    // will run!
}

while (x == true) {
    // will run, once!
    x = false;
}
```

En este caso específico -- el valor de `x` se convierte en `1` --, es un modelo mental que funciona, pero no es el más exacto. 

Considere:

```js
var x = "hello";

if (x) {
    // will run!
}

if (x == true) {
    // won't run :(
}
```

Ups Entonces, ¿qué está haciendo realmente la declaración `if`? Este es el modelo mental más preciso.

```js
var x = "hello";

if (Boolean(x) == true) {
    // will run
}

// which is the same as:

if (Boolean(x) === true) {
    // will run
}
```

Dado que la función `Boolean(..)` siempre devuelve un valor de tipo boolean, el `==` vs `===` en ese fragmento anterior es irrelevante; ambos harán lo mismo. Pero la parte importante es ver que antes de la comparación, se produce una coerción, desde cualquier tipo `x` actualmente, hasta booleana.

Simplemente no puede escapar de las coerciones en las comparaciones de JS. Abróchate el cinturón y aprende.


## Organizaciòn del còdigo

Dos patrones principales para organizar el código (datos y comportamiento)se utilizan ampliamente en todo el ecosistema JS: clases y módulos. Estos patrones no son mutuamente excluyentes; muchos programas pueden y usan ambos.

En algunos aspectos, estos patrones son muy diferentes. Pero curiosamente,de otras maneras, son solo lados diferentes de la misma moneda. Ser competente en JS requiere comprender ambos patrones y dónde son apropiados (¡y donde no!).

### Clases

Los términos "orientado a objetos", "orientado a clases" y "clases" están muy cargados de detalles y matices; no son universales en definición.

Aquí usaremos una definición común y algo tradicional, la que probablemente sea familiar para aquellos con antecedentes en lenguajes "orientados a objetos" como C ++ y Java.

Una clase en un programa es una definición de un "tipo" de estructura de datos personalizada que incluye datos y comportamientos que operan en esos datos. Las clases definen cómo funciona dicha estructura de datos, pero las clases no son en sí mismas valores concretos. Para obtener un valor concreto que pueda usar en el programa, una clase debe ser *instanciada* (con la palabra clave `new`) una o más veces.

Considere:

```js
class Page {
    constructor(text) {
        this.text = text;
    }

    print() {
        console.log(this.text);
    }
}

class Notebook {
    constructor() {
        this.pages = [];
    }

    addPage(text) {
        var page = new Page(text);
        this.pages.push(page);
    }

    print() {
        for (let page of this.pages) {
            page.print();
        }
    }
}

var mathNotes = new Notebook();
mathNotes.addPage("Arithmetic: + - * / ...");
mathNotes.addPage("Trigonometry: sin cos tan ...");

mathNotes.print();
// ..
```

En la clase `Page`, los datos son una cadena de texto almacenada en una propiedad miembro `this.text`. El comportamiento es `print()`, un método que volca el texto en la consola.

Para la clase `Notebook`, los datos son una matriz de instancias `Page`. El comportamiento es `addPage(..)`, un método que crea instancias de nuevas páginas `Page` y las agrega a la lista, así como `print()` que imprime todas las páginas en el notebook.

La declaración `mathNotes = new Notebook()` crea una instancia de la clase `Notebook`, y `page = new Page(text) `es donde se crean las instancias de la clase `Page`.

El comportamiento (métodos) solo se puede invocar en instancias (no en las clases mismas), como `mathNotes.addPage(..)` y `page.print()`.

El mecanismo `class` permite organizar empaquetar los datos (`text` y `pages`) junto con sus comportamientos (`addPage(..)`, `print()`). El mismo programa podría haberse construido sin ninguna definición de 'clase', pero probablemente habría sido mucho menos organizado, más difícil de leer y razonar, y más susceptible a errores y mantenimiento deficiente.

#### Herencia de Clase

Otro aspecto inherente al diseño tradicional "orientado a la clase", aunque un poco menos utilizado en JS, es la "herencia" (y el "polimorfismo"). 

Considere:

```js
class Publication {
    constructor(title,author,pubDate) {
        this.title = title;
        this.author = author;
        this.pubDate = pubDate;
    }

    print() {
        console.log(`
            Title: ${this.title}
            By: ${this.author}
            ${this.pubDate}
        `);
    }
}
```

Esta clase `Publication` define un conjunto de comportamientos comunes que cualquier publicación podría necesitar.

Ahora consideremos tipos de publicación más específicos, como `Book` y `BlogPost`:

```js
class Book extends Publication {
    constructor(bookDetails) {
        super(
            bookDetails.title,
            bookDetails.author,
            bookDetails.publishedOn
        );
        this.publisher = bookDetails.publisher;
        this.ISBN = bookDetails.ISBN;
    }

    print() {
        super.print();
        console.log(`
            Published By: ${this.publisher}
            ISBN: ${this.ISBN}
        `);
    }
}

class BlogPost extends Publication {
    constructor(title,author,pubDate,URL) {
        super(title,author,pubDate);
        this.URL = URL;
    }

    print() {
        super.print();
        console.log(this.URL);
    }
}
```

Tanto `Book` como `BlogPost` usan la cláusula `extend` para *extender* la definición general de `Publication` para incluir un comportamiento adicional. La llamada `super(..)` en cada constructor delega al constructor de la clase padre `Publication` para su trabajo de inicialización, y luego hacen cosas más específicas de acuerdo con su tipo de publicación respectiva (también conocida como "subclase" o "clase hija").

Ahora considere usar estas clases hijas:

```js
var YDKJS = new Book({
    title: "You Don't Know JS",
    author: "Kyle Simpson",
    publishedOn: "June 2014",
    publisher: "O'reilly",
    ISBN: "123456-789"
});

YDKJS.print();
// Title: You Don't Know JS
// By: Kyle Simpson
// June 2014
// Published By: O'reilly
// ISBN: 123456-789

var forAgainstLet = new BlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```

Observe que ambas instancias de clases hijas tienen un método `print()`, que fue una anulación del método *heredado* `print()` de la clase padre `Publication`. Cada uno de esos métodos `print()` de clase hija anulados llama a `super.print()` para invocar la versión heredada del método `print()`.

El hecho de que tanto los métodos heredados como los reemplazados pueden tener el mismo nombre y coexistir se llama *polimorfismo*.

La herencia es una herramienta poderosa para organizar datos / comportamiento en unidades lógicas separadas (clases), pero permite que la clase secundaria coopere con el padre al acceder / usar su comportamiento y datos.

### Modules

The module pattern has essentially the same goal as the class pattern, which is to group data and behavior together into logical units. Also like classes, modules can "include" or "access" the data and behaviors of other modules, for cooperation sake.

But modules have some important differences from classes. Most notably, the syntax is entirely different.

#### Módulos Clásicos

ES6 agregó una forma de sintaxis de módulo, que veremos en un momento. Pero desde los primeros días de JS, los módulos fueron un patrón importante y común que se aprovechó en innumerables programas de JS, incluso sin una sintaxis dedicada.

Las características principales de un *módulo clásico* son una función externa (que se ejecuta al menos una vez), que devuelve una "instancia" del módulo con una o más funciones expuestas que pueden operar en los datos internos (ocultos) de la instancia del módulo.

Debido a que un módulo de esta forma es *solo una función*, y llamarlo produce una "instancia" del módulo, otra descripción para estas funciones es "fábricas de módulos".

Considere la forma clásica de módulo de las clases anteriores `Publication`,` Book` y `BlogPost`:

```js
function Publication(title,author,pubDate) {
    var publicAPI = {
        print() {
            console.log(`
                Title: ${title}
                By: ${author}
                ${pubDate}
            `);
        }
    };

    return publicAPI;
}

function Book(bookDetails) {
    var pub = Publication(
        bookDetails.title,
        bookDetails.author,
        bookDetails.publishedOn
    );

    var publicAPI = {
        print() {
            pub.print();
            console.log(`
                Published By: ${bookDetails.publisher}
                ISBN: ${bookDetails.ISBN}
            `);
        }
    };

    return publicAPI;
}

function BlogPost(title,author,pubDate,URL) {
    var pub = Publication(title,author,pubDate);

    var publicAPI = {
        print() {
            pub.print();
            console.log(URL);
        }
    };

    return publicAPI;
}
```
Comparando estas formas con las formas de 'clase', hay más similitudes que diferencias.

La forma `class` almacena métodos y datos en una instancia de objeto, a la que se accede mucho con el prefijo `this`. Con los módulos, se accede a los métodos y datos como variables de identificación en el alcance, sin ningún prefijo `this`.

Con `class`, la "API "de una instancia está implícita en la definición de la clase; además, todos los datos y métodos son públicos. Con la función de fábrica del módulo, usted crea y devuelve explícitamente un objeto con cualquier método expuesto públicamente, y cualquier dato u otro método sin referencia permanece privado dentro de la función de fábrica.

| NOTA: |
| :--- |
| Hay otras variaciones de esta forma de función de fábrica que son bastante comunes en JS, incluso en 2019; puede ejecutarse de esta manera en diferentes programas JS: AMD ("Definición de módulo asíncrono"), UMD ("Definición de módulo universal") y CommonJS (módulos clásicos de estilo Node.js). Sin embargo, las variaciones son menores. Todas estas formas se basan en los mismos principios básicos. |

Considere también el uso (también conocido como "instanciación") de estas funciones de fábrica de módulos:

```js
var YDKJS = Book({
    title: "You Don't Know JS",
    author: "Kyle Simpson",
    publishedOn: "June 2014",
    publisher: "O'reilly",
    ISBN: "123456-789"
});

YDKJS.print();
// Title: You Don't Know JS
// By: Kyle Simpson
// June 2014
// Published By: O'reilly
// ISBN: 123456-789

var forAgainstLet = BlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```
La única diferencia observable aquí es la falta de usar `new`, llamando a las fábricas de módulos como funciones normales.

#### Módulos ES6

Los módulos ES6 están destinados a servir con el mismo espíritu y propósito que los *módulos clásicos* recién descritos, especialmente teniendo en cuenta las variaciones importantes y los casos de uso de AMD, UMD y CommonJS.

Sin embargo, el enfoque de implementación difiere significativamente.

Primero, no hay una función de ajuste para *definir* un módulo. El contexto de envoltura es un archivo. Los módulos ES6 siempre están basados ​​en archivos; Un archivo, un módulo.

En segundo lugar, no interactúa explícitamente con la "API" de un módulo, sino que usa la palabra clave `export` para agregar una variable o método a su definición de API pública. Si algo se define en un módulo pero no se exporta, entonces permanece oculto (como con *módulos clásicos*).

Tercero, y tal vez más notablemente diferente de los patrones discutidos anteriormente, tu no "instancias" un módulo ES6, simplemente lo "importas" para usar su instancia única. Los módulos ES6 son, en efecto, "singletons", ya que solo hay una instancia creada, al principio `import` en su programa, y ​​todos los demás` import`s solo reciben una referencia a esa misma instancia única. Si su módulo necesita soportar múltiples instancias, debe proporcionar una función de fábrica de estilo *módulo clásico* en su definición de módulo ES6 para ese propósito.

En nuestro ejemplo en ejecución, asumimos instancias múltiples, por lo que estos siguientes fragmentos mezclarán tanto los módulos ES6 como los *módulos clásicos* :.

Considere el archivo `publication.js`:

```js
function printDetails(title,author,pubDate) {
    console.log(`
        Title: ${title}
        By: ${author}
        ${pubDate}
    `);
}

export function create(title,author,pubDate) {
    var publicAPI = {
        print() {
            printDetails(title,author,pubDate);
        }
    };

    return publicAPI;
}
```

Para importar y usar este módulo, desde otro módulo ES6 como `blogpost.js`:

```js
import { create as createPub } from "publication.js";

function printDetails(pub,URL) {
    pub.print();
    console.log(URL);
}

export function create(title,author,pubDate,URL) {
    var pub = createPub(title,author,pubDate);

    var publicAPI = {
        print() {
            printDetails(pub,URL);
        }
    };

    return publicAPI;
}
```

Y finalmente, para usar este módulo, importamos a otro módulo ES6 como `main.js`:

```js
import { create as createBlogPost } from "blogpost.js";

var forAgainstLet = createBlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```
| NOTA: |
| :--- |
| La cláusula `as createBlogPost` en la declaración `import` anterior es opcional; si se omite, se importará una función de nivel superior que solo se llame `create(..)`. En este caso, lo renombraré por razones de legibilidad; su nombre de fábrica más genérico de `create(..)` se vuelve más semánticamente descriptivo de su propósito como `createBlogPost(..)`. |

Como se muestra, los módulos ES6 pueden utilizar *módulos clásicos* internamente si necesitan soportar múltiples instancias. Alternativamente,podríamos haber expuesto una `clase` de nuestro módulo en lugar de una función de fábrica `crear(...)`, con el mismo resultado en general. Sin embargo, dado que ya estás utilizando módulos ES6 en ese momento, te recomiendo que continues con *módulos clásicos* en lugar de `class`.

Si su módulo solo necesita una única instancia, puede omitir las capas adicionales de complejidad: "exportar" sus métodos públicos directamente.

## Antes de continuar

Como se prometió en la parte superior de este capítulo, solo miramos una amplia superficie de las partes principales del lenguaje JS. Incluso con esta encuesta "breve" de JS, hay un montón de detalles en este documento que debe considerar cuidadosamente y asegurarse de que se sienta cómodo. Sugiero releer este capítulo, tal vez algunas veces.

En el próximo capítulo, vamos a profundizar en algunos aspectos importantes de cómo funciona JS. Asegúrese de tomarse su tiempo con el material de este capítulo antes de continuar.
