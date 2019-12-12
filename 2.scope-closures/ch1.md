# Aún no conoces JS: Alcance & Closures - 2da Edición
# Capítulo 1: ¿Como se Determina el Alcance?


| NOTA DEL AUTOR: |
| :--- |
| Trabajo en progreso |

Una base clave de los lenguajes de programación es almacenar valores en variables y recuperar esos valores más adelante. De hecho, este proceso es la forma principal en que modelamos el *estado* de un programa. Un programa sin *estado* es un programa bastante poco interesante, por lo que este tema es el núcleo de lo que significa escribir programas.

Pero para entender cómo funcionan las variables, primero debemos preguntarnos: ¿dónde *viven* las variables? En otras palabras, ¿cómo el motor JS organiza y accede a las variables de tu programa?

Estas preguntas implican la necesidad de un conjunto bien definido de reglas para administrar variables, llamado *alcance* (Scope). El primer paso para comprender el alcance es analizar cómo el motor JS procesa y ejecuta un programa.

## Acerca de este libro

Si ya terminaste *Comenzando* (el primer libro de esta serie), ¡estás en el lugar correcto! Si no, antes de continuar, te animo a que *comiences* desde allí para obtener una mejor base.

Nuestro enfoque en este segundo libro es el primer pilar (¡de tres!) En el lenguaje JS: el sistema de alcance y sus closures de funciones, y cómo estos mecanismos permiten el patrón de diseño de módulo.

JS generalmente se clasifica como un lenguaje de secuencias de comandos interpretado, por lo que se supone que los programas se procesan en un solo paso de arriba hacia abajo. Pero JS de hecho se analiza / compila en una fase separada **antes de que comience la ejecución**. Las decisiones del autor del código sobre dónde colocar variables, funciones y bloques entre sí se analizan de acuerdo con las reglas de alcance, durante la fase inicial de análisis / compilación. El alcance resultante no se ve afectado por las condiciones de tiempo de ejecución.

Las funciones JS son en sí mismas valores, lo que significa que pueden asignarse y pasarse como números o cadenas. Pero dado que estas funciones contienen y acceden a variables, mantienen su alcance original sin importar en qué parte del programa se ejecuten las funciones. Esto se llama closure.

Los módulos son un patrón para la organización del código que se caracteriza por una colección de métodos públicos que tienen acceso (a través del closure) a variables y funciones que están ocultas dentro del alcance interno del módulo.

## Compilando el Codigo

Dependiendo de su nivel de experiencia con varios lenguajes, puede sorprenderse al saber que JS está compilado, a pesar de que normalmente se etiqueta como "dinámico" o "interpretado". JS generalmente *no* se compila con mucha anticipación, al igual que muchos lenguajes compilados tradicionalmente, ni los resultados de la compilación son portátiles entre varios motores JS distribuidos. Sin embargo, el motor JS esencialmente realiza pasos similares a los de otros compiladores de lenguajes tradicionales.

La razón por la que necesitamos hablar sobre la compilación de código para comprender el alcance es porque el alcance de JS está completamente determinado durante esta fase.

Un programa generalmente es procesado por un compilador en tres etapas básicas:

1. **Tokenizing/Lexing:** separa una cadena de caracteres en fragmentos significativos (para el lenguaje), llamados tokens. Por ejemplo, considere el programa: `var a = 2;`. Es probable que este programa se divida en los siguientes tokens: `var`, `a`, `=`, `2` y `;`. El espacio en blanco puede o no persistir como un token, dependiendo de si es significativo o no.

| NOTA: |
| :--- |
| La diferencia entre tokenizing y lexing es sutil y académica, pero se centra en si estos tokens se identifican o no de manera *sin estado* o *con estado*. En pocas palabras, si el tokenizador invocara reglas de análisis con estado para determinar si `a` debe considerarse un token distinto o solo parte de otro token, *eso* sería **lexing**. |

2. **Parsing:** toma una secuencia (arreglo) de tokens y lo convierte en un árbol de elementos anidados, que representan colectivamente la estructura gramatical del programa. Este árbol se llama "AST" (<b>A</b>bstract <b>S</b>yntax <b>T</b>ree).

    Por ejemplo, el árbol para `var a = 2;` podría comenzar con un nodo de nivel superior llamado `VariableDeclaration`, con un nodo hijo llamado` Identifier` (cuyo valor es `a`), y otro hijo llamado `AssignmentExpression` que tiene un hijo llamado `NumericLiteral` (cuyo valor es` 2`).

3. **Generación de código:** toma un AST y lo convierte en código ejecutable. Esta parte varía mucho según el lenguaje, la plataforma a la que se dirige, etc.

    El motor de JS toma nuestro AST descrito anteriormente para `var a = 2;` y lo convierte en un conjunto de instrucciones de la máquina para realmente *crear* una variable llamada `a` (incluida la reserva de memoria, etc.), y luego almacenar un valor en `a`.

| NOTA: |
| :--- |
| Los detalles de implementación de un motor JS (utilizando recursos de memoria del sistema, etc.) son mucho más profundos de lo que expondremos aquí. Nos mantendremos enfocados en el comportamiento observable de nuestros programas y dejaremos que el motor JS gestione esas abstracciones del sistema. |

El motor JS es mucho más complejo que *solo* esas tres etapas. En el proceso de análisis y generación de código, hay pasos para optimizar el rendimiento de la ejecución, incluido el colapso de elementos redundantes, etc. De hecho, el código incluso se puede volver a compilar y volver a optimizar durante la progresión de la ejecución.

Entonces, acá solo estoy pintando con trazos amplios. Pero verás en breve por qué *estos* detalles que *cubrimos*, incluso a un nivel alto, son relevantes.

Los motores JS no tienen el lujo de tener mucho tiempo para optimizar, porque la compilación JS no ocurre en un paso de compilación antes de tiempo, como ocurre con otros lenguajes. Por lo general, debe suceder en meros microsegundos (¡o menos!) Justo antes de que se ejecute el código. Para garantizar el rendimiento más rápido bajo estas restricciones, los motores JS utilizan todo tipo de trucos (como JIT, que compila de forma diferida e incluso una recompilación en caliente, etc.) que están más allá del "alcance" de nuestra discusión aquí.

### Requerido: Dos fases

Para decirlo de la manera más simple posible, un programa JS se procesa en (al menos) dos fases: parsing (análisis)/compilación primero, luego ejecución.

El desglose de una fase de parsing/compilación separada de la fase de ejecución posterior es un hecho observable, no una teoría u opinión. Mientras que la especificación JS no requiere "compilación" explícitamente, requiere un comportamiento que es esencialmente práctico en una cadencia de compilación y ejecución.

Hay tres características del programa que puedes usar para probarlo: errores de sintaxis, "errores tempranos" y hoisting (cubierto en el Capítulo 3).

Considere este programa:

```js
var greeting = "Hello";
console.log(greeting);
greeting = ."Hi";
// SyntaxError: unexpected token .
```

Este programa no produce ningún resultado (no se imprime `"Hello"`), sino que arroja un `SyntaxError` sobre el token inesperado `.` justo antes de la cadena `"Hi"`. Dado que el error de sintaxis ocurre después de la instrucción bien formada `console.log(..)`, si JS se ejecutara de arriba hacía abajo línea por línea, uno esperaría que se imprimiera el mensaje de `"Hello"` antes de que se lanzara el error de sintaxis . Eso no pasa. De hecho, la única forma en que el motor JS podría conocer el error de sintaxis en la tercera línea, antes de ejecutar la primera y la segunda, es porque el motor JS analiza primero todo este programa antes de ejecutarlo.

Ahora, considere:

```js
console.log("Howdy");
saySomething("Hello","Hi");
// Uncaught SyntaxError: Duplicate parameter name not allowed in this context

function saySomething(greeting,greeting) {
    "use strict";
    console.log(greeting);
}
```

El mensaje `"Howdy"` no se imprime, a pesar de ser una declaración bien formada. En cambio, al igual que el fragmento anterior, el `SyntaxError` se lanza antes de que se ejecute el programa. En este caso, es porque el modo estricto (habilitado solo para la función `saySomething(..)` prohíbe, entre muchas otras cosas, que las funciones tengan nombres de parámetros duplicados; esto siempre se ha permitido en modo no estricto. Esto no es un error de sintaxis en el sentido de ser una cadena de tokens con formato incorrecto (como el `."Hi"` anterior), pero la especificación requiere que se lance como un "error temprano" para los programas en modo estricto.

¿Cómo sabe el motor JS que el parámetro `greeting` se ha duplicado? ¿Cómo sabe que la función `saySomething(..)` está incluso en modo estricto mientras procesa la lista de parámetros (el pragma `"use strict"` aparece solo en el cuerpo de la función)? Nuevamente, la única respuesta razonable a estas preguntas es que el código debe analizarse primero antes de la ejecución.

Finalmente, considere:

```js
function saySomething() {
    var greeting = "Hello";
    {
        greeting = "Howdy";
        let greeting = "Hi";
        console.log(greeting);
    }
}

saySomething();
// ReferenceError: Cannot access 'greeting' before initialization
```

El `ReferenceError` mencionado aparece en la línea con la instrucción `greeting = "Howdy"`. Lo que se indica es que la variable `greeting` para esa declaración es la de la línea siguiente, `let greeting = "Hi"`, en lugar de la declaración anterior `var greeting = "Hello"`.

La única forma en que el motor JS podría saber, en la línea donde se produce el error, que la *siguiente declaración* declararía una variable de ámbito de bloque del mismo nombre (`greeting`), lo que crea el conflicto de acceso a la variable demasiado pronto, mientras se encuentra en su llamado "TDZ", Temporal Dead Zone (ver Capítulo 3) - es si el motor JS ya había procesado este código en una pasada anterior, y ya configuró todos los ámbitos y sus asociaciones variables. Este procesamiento de ámbitos y declaraciones solo se puede realizar con precisión al analizar el programa antes de la ejecución, y se llama "elevación" (hoisting) (consulte el Capítulo 3).

| ADVERTENCIA: |
| :--- |
| A menudo se afirma que las declaraciones `let` y `const` no se izan, como una explicación de la ocurrencia del comportamiento "TDZ" (Capítulo 3) que se acaba de ilustrar. Esto no es exacto. Si no se izaran este tipo de declaraciones, entonces la asignación `greeting = "Howdy "` estaría simplemente apuntando a la variable `var greeting` desde el alcance externo (función), sin necesidad de arrojar un error; el `greeting` con alcance de bloque no *existiría* todavía. ¡Pero el error TDZ en sí mismo demuestra que el `greeting` de alcance de bloque debe haber sido elevado a la parte superior de ese alcance de bloque! |

Esperemos que ahora esté convencido de que los programas JS se analizan antes de que comience cualquier ejecución. ¿Pero eso prueba que están compilados?

Esta es una pregunta interesante para reflexionar. ¿Podría JS analizar un programa, pero luego ejecutar ese programa *interpretando* el AST nodo por nodo **sin** compilar el programa en el camino? Sí, eso es *posible*, pero es extremadamente improbable, ya que sería muy ineficiente en términos de rendimiento. Es difícil imaginar un escenario en el que un motor JS de calidad de producción haga todo el esfuerzo de analizar un programa en un AST, pero luego no convierta (también conocido como "compilar") ese AST en la representación (binaria) más eficiente para el motor y luego ejecutarlo.

Muchos han intentado rebuscarse con esta terminología, ya que hay muchos matices para alimentar las interjecciones "bueno, en realidad...". Pero en espíritu y en la práctica, lo que el motor JS está haciendo al procesar programas JS es **una compilación mucho más parecida** que diferente.

La clasificación de JS como un lenguaje compilado no trata de un modelo de distribución para sus representaciones ejecutables binarias (o de código de bytes), sino de mantener una clara distinción en nuestras mentes sobre la fase donde se procesa y analiza el código de JS, que indiscutiblemente ocurre de manera observable *antes* que el código comienza a ejecutarse. Necesitamos modelos mentales adecuados para la forma en que el motor JS trata nuestro código si queremos entender a JS de manera efectiva.

## Habla el Compilador

Definamos un programa JS simple para analizar en los próximos capítulos:

```js
var students = [
    { id: 14, name: "Kyle" },
    { id: 73, name: "Suzy" },
    { id: 112, name: "Frank" },
    { id: 6, name: "Sarah" }
];

function getStudentName(studentID) {
    for (let student of students) {
        if (student.id == studentID) {
            return student.name;
        }
    }
}

var nextStudent = getStudentName(73);

console.log(nextStudent);
// Suzy
```

Además de las declaraciones, todas las ocurrencias de variables / identificadores en un programa sirven en uno de dos "roles": o son el *objetivo* de una asignación o son la *fuente* de un valor.

| NOTE: |
| :--- |
| Cuando aprendí por primera vez la teoría del compilador en mi carrera de Informática, nos enseñaron los términos "LHS" (también conocido como *objetivo*) y "RHS" (también conocido como *fuente*) para estos roles. Como puede suponer por la "L" y la "R", las siglas significan "Lado izquierdo" y "Lado derecho", respectivamente, como en los lados izquierdo y derecho de un operador de asignación `=`. Sin embargo, los objetivos y las fuentes de asignación no siempre aparecen literalmente a la izquierda o derecha de un `=`, por lo que probablemente sea menos confuso pensar en términos de *objetivo* / *fuente* en lugar de *izquierda* / *derecha*. |

¿Cómo saber si una variable es un *objetivo*? Verifique si hay un valor en cualquier lugar que se le haya asignado; si es así, es un *objetivo*. Si no, entonces la variable es una *fuente*.

### Objetivos

Veamos nuestro ejemplo de programa con respecto a estos roles. 

Considerar:

```js
students = [
    /* .. */
];
```

Esta declaración es claramente una operación de asignación; recuerde, la parte `var students` se maneja completamente como una declaración en tiempo de compilación, y por lo tanto es irrelevante durante la ejecución. Lo mismo con la instrucción `nextStudent = getStudentName(73)`.

Hay otras tres operaciones de asignación *objetivo* en el código que tal vez son menos obvias.

```js
for (let student of students) {
```

Esa declaración asigna un valor a `student` para cada iteración del ciclo. Otra referencia *objetivo*:

```js
getStudentName(73)
```

Pero, ¿cómo es eso una asignación a un *objetivo*? Mire de cerca: el argumento `73` se asigna al parámetro `studentID`.

Y hay una última referencia (sutil) *objetivo* en nuestro programa. ¿Puedes distinguirla?

..

..

..

¿Identificaste esta?

```js
function getStudentName(studentID) {
```

Una declaración `function` es un caso especial de una referencia *objetivo*. Podría pensarlo como `var getStudentName = function (studentID)`, pero eso no es exactamente correcto. Se declara un identificador `getStudentName` (en tiempo de compilación), pero la parte `= function (studentID)` también se maneja en la compilación; la asociación entre `getStudentName` y la función se configura automáticamente al comienzo del alcance en lugar de esperar a que se ejecute una instrucción de asignación` = `.

| NOTA: |
| :--- |
| Esta asignación automática inmediata de funciones a partir de declaraciones `function` se denomina "hoisting de funciones" y se tratará en el Capítulo 3. |

### Fuentes

Ya que hemos identificado las cinco referencias *objetivo* en el programa. Las otras referencias variables deben ser referencias *fuente* (¡porque es la única otra opción!).

En `for(let student of students)`, dijimos que `student` es un *objetivo*, pero `students` es la *fuente* de referencia. En la declaración `if(student.id == studentID)`, tanto `student` como `studentID` son referencias *fuente*. `student` también es una referencia *fuente* en `return student.name`.

En `getStudentName(73)`, `getStudentName` es una referencia *fuente* (que esperamos resuelva un valor de referencia de función). En `console.log(nextStudent)`, `console` es una referencia *fuente*, al igual que `nextStudent`.


| NOTA: |
| :--- |
| En caso de que se lo pregunte, `id`, `name` y `log` son todas propiedades, no referencias variables. |

¿Cuál es la importancia de comprender *objetivos* vs. *fuentes*? En el Capítulo 2, revisaremos este tema y cubriremos cómo el rol de una variable impacta su búsqueda (específicamente, si la búsqueda falla).

## Hacer trampa: Modificaciones del alcance en tiempo de ejecución

Ahora deberías estar claro que el alcance se determina a medida que se compila el programa, y ​​no debe verse afectado por ninguna condición de tiempo de ejecución. Sin embargo, en modo no estricto, técnicamente todavía hay dos formas de engañar a esta regla y modificar los ámbitos durante el tiempo de ejecución.

Ninguna de estas técnicas *debería* usarse: ambas son ideas muy malas, y de todos modos deberías usar el modo estricto, pero es importante tenerlas en cuenta en caso de que se encuentre con un código que lo haga.

La función `eval(..)` recibe una cadena de código para compilar y ejecutar sobre la marcha durante el tiempo de ejecución del programa. Si esa cadena de código tiene una declaración `var` o `function`, esas declaraciones modificarán el alcance en el que se está ejecutando actualmente `eval(..)` en:

```js
function badIdea() {
    eval("var oops = 'Ugh!';");
    console.log(oops);
}

badIdea();
// Ugh!
```

Si el `eval(..)` no hubiera estado presente, la variable `oops` en `console.log(oops)` no existiría y arrojaría un Error de referencia. Pero `eval(..)` modifica el alcance de la función `badIdea()` en tiempo de ejecución. Esta es una mala idea por muchas razones, incluido el impacto en el rendimiento de modificar el alcance ya compilado y optimizado, cada vez que se ejecuta `badIdea()`. ¡No lo hagas!

El segundo truco es la palabra clave `with`, que esencialmente convierte dinámicamente un objeto en un ámbito local; sus propiedades se tratan como identificadores en el bloque de ese ámbito:

```js
var badIdea = {
    oops: "Ugh!"
};

with (badIdea) {
    console.log(oops);
    // Ugh!
}
```

El alcance global no se modificó aquí, pero `badIdea` se convirtió en un alcance en tiempo de ejecución en lugar de tiempo de compilación. Nuevamente, esta es una idea terrible, por razones de rendimiento y legibilidad. No lo hagas.

A toda costa, evita `eval(..)` (al menos, `eval(..)` creando declaraciones) y `with`. Como se mencionó, ninguno de estos trucos está disponible en modo estricto, por lo que si solo usa el modo estricto, ¡deberías hacerlo! - Entonces se va la tentación.

## Alcance léxico

Para un lenguaje cuyo alcance se determina en tiempo de compilación, su modelo de alcance se denomina "alcance léxico". Este término "léxico" está relacionado con la etapa de "compilación". El concepto clave es que el alcance léxico de un programa se controla por completo mediante la colocación de funciones, bloques y ámbitos, entre sí.

Si coloca una declaración de variable dentro de una función, el compilador maneja esta declaración mientras analiza la función y asocia esa declaración con el alcance de la función. Si una variable se declara con alcance de bloque (`let` / `const`), entonces está asociada con el bloque `{..}` de cierre más cercano, en lugar de su función de cierre (como sucede con `var`).

Una referencia (*objetivo* o *fuente*) para una variable debe resolverse para que provenga de uno de los ámbitos que están *disponibles léxicamente*, de lo contrario se dice que la variable está "no declarada" (¡lo que generalmente produce un error!). Si la variable no está en el alcance actual, se consultará el siguiente alcance externo / envolvente. Este proceso de reducir un nivel de anidamiento de alcance continúa hasta que se encuentre una declaración de variable coincidente o se alcance el alcance global y no haya otro lugar a donde ir.

Es importante tener en cuenta que la compilación en realidad *no hace nada* en términos de reservar memoria para ámbitos y variables.

En cambio, la compilación crea un mapa de todos los ámbitos léxicos que el programa necesitará a medida que se ejecute. Puede pensar en este plan / mapa como un código insertado que definirá todos los ámbitos (también conocidos como "entornos léxicos") y registrará todos los identificadores para cada ámbito.

Por lo tanto, los ámbitos se planifican durante la compilación, es por eso que nos referimos al "alcance léxico" como una decisión en tiempo de compilación, pero en realidad no se crean hasta el tiempo de ejecución. Cada ámbito se instancia en la memoria cada vez que necesita ejecutarse.

En el próximo capítulo, construiremos una comprensión conceptual más profunda del alcance léxico.