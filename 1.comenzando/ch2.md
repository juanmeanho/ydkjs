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

Las variables declaradas `const` no son" inmutables ", simplemente no pueden reasignarse. No es aconsejable usar `const` con valores de objeto, porque esos valores aún se pueden cambiar a pesar de que la variable no se puede reasignar. Esto lleva a una posible confusión en el futuro, por lo que creo que es aconsejable evitar situaciones como:

```js
const actors = [ "Morgan Freeman", "Jennifer Anniston" ];

actors[2] = "Tom Cruise";   // OK :(

actors = [];                // Error!
```

The best semantic use of a `const` is when you have a simple primitive value that you want to give a useful name to, such as using `myBirthday` instead of `true`. This makes programs easier to read.

| TIP: |
| :--- |
| If you stick to using `const` only with primitive values, you never have the confusion of the difference between re-assignment (not allowed) and mutation (allowed)! That's the safest and best way to use `const`. |

Besides `var` / `let` / `const`, there are other syntactic forms that declare identifiers (variables) in various scopes. For example:

```js
function hello(name) {
    console.log(`Hello, ${name}.`);
}

hello("Kyle");
// Hello, Kyle.
```

Here, the identifier `hello` is created in the outer scope, and it's also automatically associated so that it references the function. But the named parameter `name` is created only inside the function, and thus is only accessible inside that function's scope.

Both `hello` and `name` act generally as if they were declared with `var`.

Another syntax that declares a variable is the `catch` clause of a `try..catch` statement:

```js
try {
    someError();
}
catch (err) {
    console.log(err);
}
```

The `err` is a block-scoped variable that exists only inside the `catch` clause, as if it had been declared with `let`.

## Functions

The word "function" has a variety of meanings in programming. For example, in the world of Functional Programming, "function" has a precise mathematical definition and implies a strict set of rules to abide by.

In JS, we should consider "function" to take the broader meaning of "procedure": a collection of statements that can be invoked one or more times, may be provided some inputs, and may give back one or more outputs.

In the good ol' days of JS, there was just one way to define a function:

```js
function awesomeFunction(coolThings) {
    // ..
    return amazingStuff;
}
```

This is called a function declaration because it appears as a statement by itself, not as an expression that's part of another statement. The association between the identifier `awesomeFunction` and the function value happens immediately during the compile phase of the code, before that code is executed.

By contrast, a function expression can be defined like this:

```js
// let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function(coolThings) {
    // ..
    return amazingStuff;
};
```

This function is an expression that is assigned to the variable `awesomeFunction`. Different from the function declaration form, a function expression is not associated with its identifier until that statement during runtime.

The function expression above is referred to as an *anonymous function expression*, since it has no name identifier between the `function` keyword and the `(..)` parameter list. This point confuses many JS developers because as of ES6, JS performs a "name inference" on an anonymous function:

```js
awesomeFunction.name;
// "awesomeFunction"
```

The `name` property of a function will reveal either its directly given name (in the case of a declaration) or its inferred name in the case of an anonymous function expression. That value is generally used by developer tools when inspecting a function value or when reporting an error stack trace.

So even an anonymous function expression *might* get a name. However, name inference only happens in limited cases such as when the function expression is assigned (with `=`). If you pass a function expression as an argument to a function call, for example, no name inference occurs, the `name` property will be an empty string, and the developer console will usually report "(anonymous function)".

Even if a name is inferred, **it's still an anonymous function.** Why? Because the inferred name is a metadata string value, not an available identifier to refer to the function. An anonymous function doesn't have an identifier to use to refer to itself from inside itself -- for recursion, event unbinding, etc.

Compare the anonymous function expression form to:

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

This function expression is a *named function expression*, since the identifier `someName` is directly associated with the function expression at compile time; the association with the identifier `awesomeFunction` still doesn't happen until runtime at the time of that statement. Those two identifiers don't have to match; sometimes it makes sense to have them be different, othertimes it's better to have them be the same.

Notice also that the explicit function name, the identifier `someName`, takes precedence when assignging a *name* for the `name` property.

Should function expressions be named or anonymous? Opinions vary widely on this. Most developers tend to be unconcerned with using anonymous functions. They're shorter, and unquestionably more common in the broad sphere of JS code out there.

| MY TAKE: |
| :--- |
| If a function exists in your program, it has a purpose; otherwise, take it out! If it has a purpose, it has a natural name that describes that purpose. If it has a name, you the code author should include that name in the code, so that the reader does not have to infer that name from reading and mentally executing that function's source code. Even a trivial function body like `x * 2` has to be read to infer a name like "double" or "multBy2"; that brief extra mental work is unnecessary when you could just take a second to name the function "double" or "multBy2" *once*, saving the reader that repeated mental work every time it's read in the future. |

There are, regretably in some respects, many other function definition forms in JS in 2019.

Here are some more declaration forms:

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

And here are some more of the (many!) function expression forms:

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

Keep in mind that arrow function expressions are **syntactically anonymous**, meaning the syntax doesn't provide a way to provide a direct name identifier for the function. The function expression may get an inferred name, but only if it's one of the assignment forms, not in the (more common!) form of being passed as a function call argument (as in the last line of the snippet).

Functions can also be specified in class definitions and object literal definitions. They're typically referred to as "methods" when in these forms, though in JS this term doesn't have much observable difference over "function".

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

Phew! That's a lot of different ways to define functions.

There's no simple shortcut path here; you just have to build familiarity with all the function forms so you can recognize them in existing code and use them appropriately in the code you write. Study them closely and practice!

## Comparisons

Making decisions in programs requires comparing values to determine their identity and relationship to each other. JS has several mechanisms to enable value comparison, so let's take a closer look at them.

### Equal...ish

The most common comparison in JS programs asks the question, "is this X value *the same as* that Y value?" What exactly does "the same as" really mean to JS, though?

For ergonomic and historical reasons, the meaning is more complicated than the obvious *exact identity* sort of matching. Sometimes an equality comparison intends *exact* matching, but other times the desired comparison is a bit broader, allowing *closely similar* or *interchangable* matching. In other words, we must be aware of the nuanced differences between an **equality** comparison and an **equivalence** comparison.

If you've spent any time working with and reading about JS, you've certainly seen the so called "triple-equals" `===` operator, also described as the "strict equality" operator. That seems rather straight forward, right? Surely, "strict" means strict, as in narrow and *exact*.

Not *exact*ly.

Yes, most values participating in an `===` equality comparison will fit with that *exact same* intuition. Consider some examples:

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

| NOTE: |
| :--- |
| Another way `===`'s equality comparison is often described is, "checking both the value and the type". In several of the above examples, like `42 === "42"`, the *type* of both values -- number, string, etc -- does seem to be the distinguishing factor. There's more to it than that, though. **All** value comparisons in JS consider the type of the values being compared, not *just* the `===` operator. Specifically, `===` disallows any sort of type conversion (aka, "coercion") in its comparison, where other JS comparisons *do* allow coercion. |

But the `===` operator does have some nuance to it, a fact many JS developers gloss over, to their detriment. The `===` operator is designed to *lie* in two cases of special values: `NaN` and `-0`. Consider:

```js
NaN === NaN;            // false
0 === -0;               // true
```

In the case of `NaN`, the `===` operator *lies* and says that an occurrence of `NaN` is not equal to another `NaN`. In the case of `-0` -- yes, this is a real, distinct value you can use intentionally in your programs! -- the `===` operator *lies* and says it's equal to the regular `0` value.

| TIP: |
| :--- |
| When making such comparisons, since the *lying* is likely bothersome, don't use `===`. For `NaN` comparisons, use the `Number.isNaN(..)` utility, which does not *lie*. For `-0` comparison, use the `Object.is(..)` utility, which also does not *lie*. `Object.is(..)` can also be used for non-*lying* `NaN` checks, if you prefer. Humorously, you could think of `Object.is(..)` as the "quadruple-equals" `====`, the really-really-strict comparison! |

There are deeper historical and technical reasons for these *lies*, but that doesn't change the fact that `===` is not actually *strictly exactly equal* comparison, in the *strictest* sense.

The story gets even more complicated when we consider comparisons of object values (non-primitives). Consider:

```js
[ 1, 2, 3 ] === [ 1, 2, 3 ];    // false
{ a: 42 } === { a: 42 }         // false
(x => x * 2) === (x => x * 2)   // false
```

What's going on here?

It may seem reasonable to assume that an equality check considers the *nature* or *contents* of the value; after all, `42 === 42` considers the actual `42` value and compares it. But when it comes to objects, a content-aware comparison is generally referred to as "structural equality".

JS does not define `===` as *structural equality* for object values. Instead, `===` uses *identity equality* for object values.

In JS, all object values are held by reference, they are assigned and passed by reference-copy, **and** to our current discussion, they are compared by reference (identity) equality. Consider:

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

In this snippet, `y === x` is true because both variables hold a reference to the same initial array. But the `=== [1,2,3]` comparisons both fail because `y` and `x`, respectively, are being compared to new *different* arrays `[1,2,3]`. The array structure and contents don't matter in this comparison, only the **reference identity**.

JS does not provide a mechanism for structural equality comparison of object values, only reference identity comparison. To do structural equality comparison, you'll need to implement the checks yourself.

But beware, it's more complicated than you'll assume. For example, how might you determine if two function references are "structurally equivalent"? Even stringifying to compare their source code text wouldn't take into account things like closure. JS doesn't provide structural equality comparison because it's almost intractable to handle all the corner cases!

### Coercive Comparisons

Few JS features draw more ire in the broader JS community than the `==` operator, generally referred to as the "loose equality" operator. The majority of all writing and public discourse on JS condemns this operator as poorly designed and dangerous/bug-ridden when used in JS programs. Even the creator of the language himself, Brendan Eich, has lamented how it was designed as a big mistake.

From what I can tell, most of this frustration comes from a pretty short list of confusing corner cases, but a deeper problem is the extremely widespread misconception that it performs its comparisons without considering the types of its compared values.

The `==` operator performs an equality comparison similarly to how the `===` performs it. In fact, both operators consider the type of the values being compared. And if the comparison is between the same value type, both `==` and `===` **do exactly the same thing, no difference whatsoever.**

If the value types being compared are different, the `==` differs from `===` in that it allows coercion before the comparison. In other words, they both want to compare values of like types, but `==` allows type conversions *first*, and once the types have been converted to be the same on both sides, then `==` does the same thing as `===`. Instead of "loose equality", the `==` operator should be described as "coercive equality".

Consider:

```js
42 == "42";             // true
1 == true;              // true
```

In both comparisons, the value types are different, so the `==` causes the non-number values (`"42"` and `true`) to be converted to numbers (`42` and `1`, respectively) before the comparisons are made.

Just being aware of this nature of `==` helps you avoid most of the troublesome corner cases, such as staying away from a gotchas like `"" == 0` or `0 == false`.

You may be thinking, "Oh, well, I will always just avoid any coercive equality comparison (using `===` instead) to avoid those corner cases"! Eh, sorry, that's not quite as likely as you would hope.

There's a pretty good chance that you'll use relational comparison operators like `<`, `>` (and even `<=` and `>=`).

Just like `==`, these operators will perform as if they're "strict" if the types being relationally compared already match, but they'll allow coercion first (generally, to numbers) if the types differ.

Consider:

```js
var arr = [ "1", "10", "100", "1000" ];
for (let i = 0; i < arr.length && arr[i] < 500; i++) {
    // will run 3 times
}
```

The `i < arr.length` comparison is "safe" from coercion because `i` and `arr.length` are always numbers. The `arr[i] < 500` invokes coercion, though, because the `arr[i]` values are all strings. Those comparisons thus become `1 < 500`, `10 < 500`, `100 < 500`, and `1000 < 500`. Since that last one is false, the loop stops after its third iteration.

These relational operators typically use numeric comparisons, except in the case where **both** values being compared are already strings; in this case, they use alphabetical (dictionary-like) comparison of the strings:

```js
var x = "10";
var y = "9";

x < y;      // true, watch out!
```

There's no way to get these relational operators to avoid coercion, other than to just never use mismatched types in the comparisons. That's perhaps admirable as a goal, but it's still pretty likely you're going to run into a case where the types *may* differ.

The wiser approach is not to avoid coercive comparisons, but to embrace and learn their ins and outs.

#### Conditionals

Conditional expressions, such as those computed in `if` statements, as well as `while` and `for` loops, perform an implicit value comparison. But what sort? Is it "strict" or "coercive"? Both, actually.

Consider:

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

You might think of these `(x)` conditional expressions like this:

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

In this specific case -- the value of `x` being `1` -- that mental model works, but it's not accurate more broadly. Consider:

```js
var x = "hello";

if (x) {
    // will run!
}

if (x == true) {
    // won't run :(
}
```

Oops. So what is the `if` statement actually doing? This is the more accurate mental model.

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

Since the `Boolean(..)` function always returns a value of type boolean, the `==` vs `===` in that above snippet is irrelevant; they'll both do the same thing. But the important part is to see that before the comparison, a coercion occurs, from whatever type `x` currently is, to boolean.

You just can't get away from coercions in JS comparisons. Buckle down and learn them.

## Code Organization

Two major patterns for organizing code (data and behavior) are used broadly across the JS ecosystem: classes and modules. These patterns are not mutually exclusive; many programs can and do use both.

In some respects, these patterns are very different. But interestingly, in other ways, they're just different sides of the same coin. Being proficient in JS requires understanding both patterns and where they are appropriate (and not!).

### Classes

The terms "object oriented", "class oriented" and "classes" are all very loaded full of detail and nuance; they're not universal in definition.

We will use a common and somewhat traditional definition here, the one most likely familiar to those with backgrounds in "object oriented" languages like C++ and Java.

A class in a program is a definition of a "type" of custom data structure that includes both data and behaviors that operate on that data. Classes define how such a data structure works, but classes are not themselves concrete values. To get a concrete value that you can use in the program, a class must be *instantiated* (with the `new` keyword) one or more times.

Consider:

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

In the `Page` class, the data is a string of text stored in a `this.text` member property. The behavior is `print()`, a method that dumps the text to the console.

For the `Notebook` class, the data is an array of `Page` instances. The behavior is `addPage(..)`, a method that instantiates new `Page` pages and adds them to the list, as well as `print()` which prints out all the pages in the notebook.

The statement `mathNotes = new Notebook()` creates an instance of the `Notebook` class, and `page = new Page(text)` is where instances of the `Page` class are created.

Behavior (methods) can only be called on instances (not the classes themselves), such as `mathNotes.addPage(..)` and `page.print()`.

The `class` mechanism allows packaging data (`text` and `pages`) to be organized together with their behaviors (`addPage(..)`, `print()`). The same program could have been built without any `class` definitions, but it would likely have been much less organized, harder to read and reason about, and more susceptible to bugs and subpar maintenance.

#### Class Inheritance

Another aspect inherent to traditional "class oriented" design, though a bit less commonly used in JS, is "inheritance" (and "polymorphism"). Consider:

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

This `Publication` class defines a set of common behavior that any publication might need.

Now let's consider more specific types of publication, like `Book` and `BlogPost`:

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

Both `Book` and `BlogPost` use the `extends` clause to *extend* the general definition of `Publication` to include additional behavior. The `super(..)` call in each constructor delegates to the parent `Publication` class's constructor for its initialization work, and then they do more specific things according to their respective publication type (aka, "sub-class" or "child class").

Now consider using these child classes:

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

Notice that both child class instances have a `print()` method, which was an override of the *inherited* `print()` method from the parent `Publication` class. Each of those overridden child class `print()` methods call `super.print()` to invoke the inherited version of the `print()` method.

The fact that both the inherited and overriden methods can have the same name and co-exist is called *polymorphism*.

Inheritance is a powerful tool for organizing data/behavior in separate logical units (classes), but allowing the child class to cooperate with the parent by accessing/using its behavior and data.

### Modules

The module pattern has essentially the same goal as the class pattern, which is to group data and behavior together into logical units. Also like classes, modules can "include" or "access" the data and behaviors of other modules, for cooperation sake.

But modules have some important differences from classes. Most notably, the syntax is entirely different.

#### Classic Modules

ES6 added a module syntax form, which we'll look at in a moment. But from the early days of JS, modules was an important and common pattern that was leveraged in countless JS programs, even without a dedicated syntax.

The key hallmarks of a *classic module* are an outer function (that runs at least once), which returns an "instance" of the module with one or more functions exposed that can operate on the module instance's internal (hidden) data.

Because a module of this form is *just a function*, and calling it produces an "instance" of the module, another description for these functions is "module factories".

Consider the classic module form of the earlier `Publication`, `Book`, and `BlogPost` classes:

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

Comparing these forms to the `class` forms, there are more similarities than differences.

The `class` form stores methods and data on an object instance, which much be accessed with the `this.` prefix. With modules, the methods and data are accessed as identifier variables in scope, without any `this.` prefix.

With `class`, the "API" of an instance is implicit in the class definition -- also, all data and methods are public. With the module factory function, you explicitly create and return an object with any publicly exposed methods, and any data or other unreferenced methods remain private inside the factory function.

| NOTE: |
| :--- |
| There are other variations to this factory function form that are quite common across JS, even in 2019; you may run across these forms in different JS programs: AMD ("Asynchronous Module Definition"), UMD ("Universal Module Definition"), and CommonJS (classic Node.js style modules). The variations are minor, though. All of these forms rely on the same basic principles. |

Consider also the usage (aka, "instantiation") of these module factory functions:

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

The only observable difference here is the lack of using `new`, calling the module factories as normal functions.

#### ES6 Modules

ES6 modules are meant to serve much the same spirit and purpose as the existing *classic modules* just described, especially taking into account important variations and use-cases from AMD, UMD, and CommonJS.

The implementation approach does however differ significantly.

First, there's no wrapping function to *define* a module. The wrapping context is a file. ES6 modules are always file-based; one file, one module.

Second, you don't interact with a module's "API" explicitly, but rather use the `export` keyword to add a variable or method to its public API definition. If something is defined in a module but not `export`ed, then it stays hidden (just as with *classic modules*).

Third, and maybe most noticeably different from previously discussed patterns, you don't "instantiate" an ES6 module, you just `import` it to use its single instance. ES6 modules are, in effect, "singletons", in that there's only one instance ever created, at first `import` in your program, and all other `import`s just receive a reference to that same single instance. If your module needs to support multiple instantiations, you have to provide a *classic module* style factory function on your ES6 module definition for that purpose.

In our running example, we do assume multiple-instantation, so these following snippets will mix both ES6 modules and *classic modules*:.

Consider the file `publication.js`:

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

To import and use this module, from another ES6 module like `blogpost.js`:

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

And finally, to use this module, we import into another ES6 module like `main.js`:

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

| NOTE: |
| :--- |
| The `as createBlogPost` clause in the `import` statement above is optional; if omitted, a top level function just named `create(..)` would be imported. In this case, I'm renaming it for readability sake; its more generic factory name of `create(..)` becomes more semantically descriptive of its purpose as `createBlogPost(..)`. |

As shown, ES6 modules can utilize *classic modules* internally if they need to support multiple-instantiation. Alternatively, we could have exposed a `class` from our module instead of a `create(..)` factory function, with generally the same outcome. However, since you're already using ES6 modules at that point, I'd recommend sticking with *classic modules* instead of `class`.

If your module only needs a single instance, you can skip the extra layers of complexity: `export` its public methods directly.

## Before You Go On

As promised at the top of this chapter, we just glanced over a wide surface area of the main parts of the JS language. Even with this "brief" survey of JS, there's a ton of details herein you should carefully consider and ensure you are comfortable with. I suggest re-reading this chapter, maybe a few times.

In the next chapter, we're going to dive deeper into some important aspects of how JS works. Make sure you take your time with this chapter's material before you proceed.
