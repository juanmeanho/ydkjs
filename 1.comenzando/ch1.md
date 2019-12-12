# Aún no sabes JS: Comenzando - 2da Edición
# Capítulo 1: Qué *Es* JavaScript?

Aún no sabes a JavaScript. Yo tampoco, no del todo. Ninguno de nosotros lo hace. ¡Ese es el objetivo de esta serie de libros!

Pero aquí es donde comienzas ese *viaje* de conocer un poco mejor el lenguaje. Enfatizo la palabra viaje porque *saber JavaScript* no es un destino, es una dirección. No importa cuánto tiempo pases con el lenguaje,siempre podrás encontrar algo más para aprender y comprender un poco mejor.

Ten en cuenta que aunque este libro se titula "Comenzando", no pretende ser un libro para principiantes. El trabajo de este libro es prepararte para el resto de la serie, pero está escrito asumiendo que ya tienes al menos varios meses de experiencia en programación en JS.

Incluso si tienes mucha experiencia en JS, este libro no debe pasarse por alto ni omitirse. Te Recomiendo tomarte tu tiempo para procesar completamente el material. **Un buen comienzo siempre se basa en una buena base.**

En este capítulo cubriremos algunos detalles básicos de gestión interna y aclararemos algunos mitos y conceptos erróneos sobre lo que realmente es el lenguaje (¡y qué no es!). Esta es una información valiosa sobre la identidad y el proceso de cómo se organiza y mantiene JS; Todos los desarrolladores de JS deberían entenderlo.

Pero si ya te siente bastante sólido en JS detrás de escena, puedes pasar al Capítulo 2 para comenzar a estudiar la sintaxis de JS con más detalle.

## Nombre

El nombre JavaScript es probablemente el nombre del lenguaje de programación más incomprendido de la historia.

¿Está este lenguaje relacionado con Java? ¿Es solo la forma de script para Java? ¿Es solo para escribir scripts y no para programas reales?

La verdad es que el nombre JavaScript es un artefacto de travesuras de marketing. Cuando Brendan Eich concibió el lenguaje por primera vez, lo llamó en clave Mocha. Internamente en Netscape, se utilizó la marca LiveScript. Pero cuando llegó el momento de nombrar públicamente el lenguaje, "JavaScript" ganó la votación.

¿Por qué? Porque este lenguaje fue originalmente diseñado para atraer a una audiencia de programadores en su mayoría de Java, y porque la palabra "script" era popular en ese momento para referirse a programas livianos. ¡Estos "scripts" livianos serían los primeros en incrustarse dentro de las páginas en esta nueva cosa llamada web!

En otras palabras, JavaScript fue una estratagema de marketing para tratar de posicionar este lenguaje como una alternativa aceptable para escribir el Java más pesado y más conocido del momento. Podría haberse llamado fácilmente "WebJava", para el caso.

Hay algunas semejanzas superficiales entre el código de JavaScript y el código de Java. Pero en realidad son principalmente de una raíz común: C (y hasta cierto punto, C++).

Por ejemplo, usamos el `{` para comenzar un bloque de código y el `}` para terminar ese bloque de código, al igual que C / C++ y Java. También usamos el `;` para puntuar el final de una declaración.

De hecho, las relaciones son aún más profundas que las superficiales. Oracle (a través de Sun), la compañía que aún posee y ejecuta Java, también posee la marca comercial oficial del nombre "JavaScript" (a través de Netscape). Esta marca registrada casi nunca se aplica, y probablemente no podría hacerse en estos tiempos.

Algunos han sugerido que usemos JS en lugar de JavaScript. Esa es una abreviatura muy común y una buena candidata para una marca del lenguaje oficial.

Para distanciar el lenguaje de la marca registrada propiedad de Oracle, el nombre oficial del lenguaje especificado por TC39 y formalizado por el organismo de estándares ECMA es: ECMAScript. Y, de hecho, desde 2016, el nombre oficial del lenguaje también ha sido sufijado por el año de revisión; al momento de este escrito, ese es ECMAScript 2019, o abreviado ES2019.

En otras palabras, JavaScript (en su navegador o en Node.js) es *una* implementación del estándar ES2019.

| NOTA: |
| :--- |
| No utilice términos como "JS6" o "ES8" para referirse al lenguaje. Algunos lo hacen, pero esos términos solo sirven para perpetuar la confusión. "ES20xx" o simplemente "JS" son los que se deben seguir. |

Ya sea que lo llames JavaScript, JS, ECMAScript o ES2019, ¡definitivamente no es una variante del lenguaje Java!

> "Java es a JavaScript lo que Car es a Carnival". --Jeremey Keith, 2009

## Muchas Caras

El término "paradigma" en el contexto del lenguaje de programación se refiere a una mentalidad y enfoque amplios (casi universales) para estructurar el código. Dentro de un paradigma, existen innumerables variaciones de estilo y forma que distinguen los programas, incluidas innumerables bibliotecas y marcos diferentes que dejan su firma única en cualquier código dado.

Pero no importa cuál sea el estilo individual de un programa, las divisiones generales en torno a los paradigmas son casi siempre evidentes a primera vista de cualquier programa.

Ejemplos de distinciones típicas de código a nivel de paradigma: de procedimiento, orientado a objetos (OO / clases) y funcional (FP).

En términos generales, el código de procedimiento se centra en la progresión lineal de arriba hacia abajo a través de un conjunto predeterminado de operaciones, reunidas en unidades llamadas procedimientos. El código OO generalmente orienta la lógica y los datos en unidades llamadas clases. El código FP enfatiza las funciones (cálculos puros en oposición a los procedimientos) y las adaptaciones de funciones como valores.

Los paradigmas no son ni correctos ni incorrectos. Son orientaciones que guían y moldean cómo los programadores abordan los problemas y las soluciones, cómo estructuran y mantienen su código.

Algunos lenguajes están muy inclinados hacia un paradigma: C es de procedimiento, Java / C ++ está casi completamente orientado a clases y Haskell es FP de principio a fin.

Pero muchos lenguajes también admiten patrones de código que pueden provenir, e incluso mezclar y combinar diferentes paradigmas. Los llamados "lenguajes de paradigmas múltiples" ofrecen la máxima flexibilidad. En algunos casos, un solo programa puede incluso tener dos o más expresiones de estos paradigmas sentados uno al lado del otro.

JavaScript es definitivamente un lenguaje multi-paradigmático. Puedes escribir código de procedimiento, orientado a clases o estilo FP, y puedes tomar estas decisiones línea por línea en lugar de verte obligado a tomar una decisión de todo o nada.

## Especificación

Mencioné anteriormente que TC39, el comité directivo técnico que administra JS, vota sobre los cambios para presentar a ECMA, el organismo de estándares.

El conjunto de sintaxis y comportamiento que *es* JavaScript se define en la especificación ES.

ES2019 es la décima especificación / revisión principal numerada desde el inicio de JavaScript en 1995, por lo que en la URL oficial de la especificación alojada por ECMA, encontrará "10.0":

https://www.ecma-international.org/ecma-262/10.0/

El comité TC39 está compuesto por entre 50 y alrededor de 100 personas diferentes de una amplia sección de empresas con inversión en la web, como fabricantes de navegadores (Mozilla, Google, Apple) y fabricantes de dispositivos (Samsung, etc.). Todos los miembros del comité son voluntarios, aunque muchos de ellos son empleados de estas compañías y, por lo tanto, pueden recibir una compensación en parte por sus deberes en el comité.

TC39 se reúne generalmente cada dos meses, generalmente durante aproximadamente 3 días, para revisar el trabajo realizado por los miembros desde la última reunión, discutir temas y votar propuestas. Los lugares de reunión rotan entre las compañías miembros que desean organizar.

Todas las propuestas TC39 avanzan a través de un proceso de cinco etapas, por supuesto, dado que somos programadores, ¡está basado en 0! - Etapa 0 a la Etapa 4. Puede leer más sobre el proceso de la Etapa aquí: https://tc39.es/process-document/

La etapa 0 significa, más o menos, que alguien en TC39 cree que tiene una buena idea y planea defenderla y trabajar en ella. Eso significa que muchas ideas que los "miembros" que no son miembros del TC39 "proponen", a través de medios informales como las redes sociales o publicaciones en blogs, son realmente "pre-etapa 0". Tienes que conseguir que un miembro de TC39 defienda una propuesta para que se considere oficialmente "Etapa 0".

Una vez que una propuesta alcanza el estado de "Etapa 4", es elegible para ser incluida en la próxima revisión anual del lenguaje. Puede tomar desde varios meses hasta algunos años para que una propuesta funcione en estas etapas.

Todas las propuestas se gestionan de forma abierta, en el repositorio Github de TC39: https://github.com/tc39/proposals

Cualquiera, ya sea en TC39 o no, puede participar en estas discusiones públicas y en los procesos para trabajar en las propuestas. Sin embargo, solo los miembros del TC39 pueden asistir a las reuniones y votar las propuestas y los cambios. En efecto, la voz de un miembro de TC39 tiene mucho peso sobre dónde irá JS.

Contrario a algunos mitos establecidos y frustrantemente perpetuados, *no* hay múltiples versiones de JavaScript sin orden. Solo hay **un JS**, el estándar oficial mantenido por TC39 y ECMA.

A principios de la década de 2000, cuando Microsoft mantenía una versión bifurcada y de ingeniería inversa (y no totalmente compatible) de JS llamada "JScript", había legítimamente "versiones múltiples" de JS. Pero esos días ya pasaron. Es obsoleto e inexacto hacer tales afirmaciones sobre JS hoy.

Todos los principales navegadores y fabricantes de dispositivos se han comprometido a mantener sus implementaciones JS compatibles con esta especificación central. Por supuesto, los motores implementan características en diferentes momentos. Pero nunca debería darse el caso de que el motor v8 (motor JS de Chrome) implemente una característica específica de manera diferente o incompatible en comparación con el motor SpiderMonkey (motor JS de Mozilla).

Eso significa que puede aprender **un JS** y confiar en ese mismo JS en todas partes.

### La web gobierna todo sobre (JS)

Mientras que la variedad de entornos que ejecutan JS se expande constantemente, desde navegadores, servidores (Node.js), robots, bombillas, hasta ..., el único entorno que gobierna JS es la web. En otras palabras, la forma en que JS se implementa para los navegadores web es, en la práctica, la única realidad que importa.

En su mayor parte, el JS definido en la especificación y el JS que se ejecuta en motores JS basados ​​en navegador es el mismo. Pero hay algunas diferencias que deben considerarse.

A veces, la especificación JS dictará un comportamiento nuevo o refinado, y sin embargo, eso no coincidirá exactamente con cómo funciona en los motores JS basados ​​en navegador. Tal desajuste es histórico: los motores JS han tenido más de 20 años de comportamientos observables en torno a casos de características en las que el contenido web se ha basado. Como tal, a veces los motores JS se negarán a ajustarse a un cambio dictado por la especificación porque rompería ese contenido web.

En estos casos, a menudo TC39 retrocederá y simplemente elegirá adaptar la especificación a la realidad de la web. Por ejemplo, TC39 planeó agregar un método `contains(..)` para matrices, pero se descubrió que este nombre entraba en conflicto con los marcos JS antiguos que todavía se usaban en algunos sitios, por lo que cambiaron el nombre a uno no conflictivo  `includes(..)`. Lo mismo sucedió con una tragicómica *crisis en la comunidad de JS* llamada "smooshgate", donde el método planificado `flatten(..)` fue finalmente renombrado como `flat(..)`.

Pero ocasionalmente, TC39 decidirá que la especificación debe mantenerse firme en algún punto, aunque es poco probable que los motores JS basados ​​en navegador alguna vez se ajusten.

¿La solución? Apéndice B, "Características adicionales de ECMAScript para navegadores web. Para el momento de la redacción, aquí está el Apéndice B de ES2019: https://www.ecma-international.org/ecma-262/10.0/#sec-additional-ecmascript-features-for-web-browsers 

La especificación JS incluye este apéndice para detallar cualquier desajuste conocido entre la especificación oficial JS y la realidad de JS en la web. En otras palabras, estas son excepciones que están permitidas *solo* para web JS; otros entornos JS deben cumplir la ley al pie de la letra.

Las secciones B.1 y B.2 cubren *adiciones* a JS (sintaxis y API) que la web JS incluye, nuevamente por razones históricas, pero que TC39 no planea especificar formalmente en el núcleo de JS. Los ejemplos incluyen literales octales con prefijo `0`, las utilidades globales `escape(..)` / `unescape(..) `, "helpers" de cadena como `anchor(..)` y `blink()`, y el método RegExp `compile(..)`. El uso de estas *adiciones* en un motor JS que no sea web generalmente se romperá por completo, así que úselas con mucha precaución.

La sección B.3 incluye algunos conflictos en los que el código puede ejecutarse en motores JS web y no web, pero donde el comportamiento *podría* ser notablemente diferente, dando como resultado resultados diferentes. Un ejemplo notable de ese tipo de conflicto es para las declaraciones de función de ámbito de bloque (B.3.3).

Considere cuál debería ser el resultado de este programa:

```js
var x = true;

if (x) {
    function gotcha() {
        console.log("One!");
    }
}
else {
    function gotcha() {
        console.log("Two!");
    }
}

gotcha();                // ??
```

Si bien esto puede parecer lógico (imprimir "¡One!"), La realidad es mucho más fea. Hay **muchas** variaciones diferentes de este escenario, y cada variación tiene una semántica ligeramente diferente.

El mejor consejo para navegar por este tipo de *gotchas* del Apéndice B es evitar el uso de las construcciones. Nunca declare funciones en ámbitos de bloque (como sentencias `if`), solo en ámbitos de función.

### No todo (Web) es JS...

¿Es este código un programa JS?

```js
alert("Hello, JS!");
```

Depende de cómo veas las cosas. La función `alert(..)` que se muestra aquí no está incluida en la especificación JS. Sin embargo, está en todos los entornos web JS. Sin embargo, no lo encontrará en el Apéndice B. Entonces, ¿qué da?

Varios entornos JS (como los motores JS del navegador, Node.js, etc.) agregan API en el alcance global de sus programas JS que le brindan capacidades específicas del entorno, como poder abrir un cuadro de estilo de alerta en el navegador del usuario.

De hecho, una amplia gama de API de aspecto JS, como `fetch(..)`, `getCurrentLocation(..)` y `getUserMedia(..)`, son todas API web que se parecen a JS. En Node.js, podemos acceder a cientos de métodos API desde los módulos integrados, como `fs.write(..)`.

Otro ejemplo común es `console.log(..)` (y todos los demás métodos `console.*`!). Estos no se especifican en JS, pero debido a su utilidad universal se definen en casi todos los entornos de JS, según un consenso más o menos acordado.

Entonces, `alert(..)` y `console.log(..)` no están definidos por JS. Pero *se parecen* a JS. Son funciones y métodos de objeto y obedecen las reglas de sintaxis JS. Los comportamientos detrás de ellos están controlados por el entorno que ejecuta el motor JS, pero en la superficie definitivamente deben cumplir con JS para poder jugar en el patio de recreo de JS.

La mayoría de las diferencias entre navegadores de las que la gente se queja diciendo "¡JS es tan inconsistente!", esos reclamos en realidad se deben a diferencias en cómo funcionan esos comportamientos del entorno, no en cómo funciona el propio JS.

De modo que una llamada `alert(..)` *es* JS, pero en realidad es solo un invitado; No es parte del JS oficial.

## Hacia atras & Hacia adelante

Uno de los principios más fundamentales que guía a JavaScript es la preservación de la *compatibilidad con versiones anteriores*. Muchos están confundidos por las implicaciones de este término, y a menudo lo confunden con un término relacionado pero diferente: *compatibilidad directa*.

Vamos a dejar las cosas claras.

La compatibilidad con versiones anteriores significa que una vez que algo se acepta como JS válido, no habrá un cambio futuro en el lenguaje que haga que ese código se convierta en JS no válido. Código escrito en 1995, ¡por primitivo o limitado que haya sido! - Todavía debería funcionar hoy. Como a menudo proclaman los miembros de TC39, "¡no rompemos la red!"

La idea es que los desarrolladores de JS puedan escribir código con la confianza de que su código no dejará de funcionar de manera impredecible porque se lanza una actualización del navegador. Esto hace que la decisión de elegir JS para un programa sea una inversión más sabia y segura, por años en el futuro.

Esa "garantía" no es poca cosa. Mantener la compatibilidad con versiones anteriores, que se extiende a lo largo de casi 25 años de la historia del lenguaje, crea una carga enorme y una gran cantidad de desafíos únicos. Sería difícil encontrar otro ejemplo en toda la informática de tal compromiso con la compatibilidad con versiones anteriores.

Los costos de apegarse a este principio no deben descartarse casualmente. Crea necesariamente una barra muy alta para incluir el cambio o la extensión del lenguaje; cualquier decisión se vuelve efectivamente permanente, errores y todo. Una vez que está en JS, no se puede quitar porque podría interrumpir los programas, ¡incluso si realmente quisiéramos eliminarlo!

Hay algunas pequeñas excepciones a esta regla. JS ha tenido algunos cambios incompatibles con versiones anteriores, pero TC39 es extremadamente cauteloso al hacerlo. Estudian el código existente en la web (a través de la recopilación de datos del navegador) para estimar el impacto de dicha rotura, y los navegadores finalmente deciden y votan si están dispuestos a tomar el reclamo de los usuarios por una rotura a muy pequeña escala contra los beneficios de arreglar o mejorar algún aspecto del lenguaje para muchos más sitios (y usuarios).

Este tipo de cambios son raros y casi siempre se producen en casos extremos de uso que es poco probable que se rompan de manera observable en muchos sitios.

Compare *compatibilidad con versiones anteriores* con su contraparte, *compatibilidad con versiones posteriores*. Ser compatible con versiones anteriores significa que incluir una nueva adición al lenguaje en un programa no causaría que el programa se rompa si se ejecuta en un motor JS anterior. **JS no es compatible con versiones posteriores**, a pesar de que muchos lo deseen, e incluso creen incorrectamente el mito de que sí lo es.

HTML y CSS son, por el contrario, son compatibles con versiones posteriores, pero no son compatibles con versiones anteriores. Si encuentro algo de HTML o CSS de 1995, es muy posible que hoy no funcione (o funcione igual). Pero, si usa una nueva función CSS de 2019 y se ejecuta en un motor CSS de 2010, el CSS no se rompe; el motor de estilo simplemente pasa por alto cualquier cosa que no reconoce.

### Saltando la brecha

Dado que JS no es compatible con versiones anteriores, significa que siempre existe la posibilidad de una brecha entre el código que puede escribir que sea JS válido y el motor más antiguo que su sitio o aplicación necesita admitir. Si ejecuta un programa que utiliza una función ES2019 en un motor de 2016, es muy probable que vea que el programa se rompe y se bloquea.

Si la característica es una nueva sintaxis, el programa en general no podrá compilarse y ejecutarse por completo, generalmente arrojando un error de sintaxis. Si la función es una API (como `Object.is(..)` de ES6),el programa puede ejecutarse hasta cierto punto pero luego generar una excepción de tiempo de ejecución y detenerse una vez que encuentra la referencia a la API desconocida.

¿Significa esto que los desarrolladores de JS siempre deberían estar a la zaga del ritmo de progreso, utilizando solo el código que está al borde de los entornos de motores JS más antiguos que necesitan soportar? ¡No!

Pero sí significa que los desarrolladores de JS deben tener especial cuidado para abordar esta brecha.

Para una sintaxis nueva e incompatible, la solución es la transpilación. La transpilación es un término inventado por la comunidad para describir el uso de una herramienta para convertir el código fuente de un programa de una forma a otra (pero aún como código fuente textual). Por lo general,los problemas de compatibilidad hacia adelante relacionados con la sintaxis se resuelven mediante el uso de un transpilador, el más común es Babel (https://babeljs.io), para convertir de esa nueva versión de sintaxis JS a un equivalente de código que usa un sintaxis anterior

Por ejemplo, un desarrollador puede escribir un fragmento de código como:

```js
if (something) {
    let x = 3;
    console.log(x);
}
else {
    let x = 4;
    console.log(x);
}
```

Así es como se vería el código en el árbol de código fuente para esa aplicación. Pero al producir los archivos para implementar en el sitio web público, el transpilador de Babel podría convertir ese código para que se vea así:

```js
var x$0;
var x$1;
if (something) {
    x$0 = 3;
    console.log(x$0);
}
else {
    x$1 = 4;
    console.log(x$1);
}
```

El fragmento original dependía de `let` para crear variables `x` con ámbito de bloque en las cláusulas `if` y` else` que no interferían entre sí. Un programa equivalente (con un mínimo de reelaboración) que Babel puede producir simplemente elige nombrar dos variables diferentes con nombres únicos, produciendo el mismo resultado sin interferencia.

| NOTA: |
| :--- |
| La palabra clave `let` se agregó en ES6 (en 2015). El ejemplo anterior de transpilación solo necesitaría aplicarse si una aplicación necesita ejecutarse en un entorno JS compatible con versiones anteriores a ES6. El ejemplo aquí es solo por simplicidad de ilustración. Cuando el ES6 era nuevo, la necesidad de tal transpilación era bastante frecuente, pero en 2019 es mucho menos común tener que soportar entornos anteriores al ES6. El "objetivo" utilizado para la transpiración es, por lo tanto, una ventana deslizante que se mueve hacia arriba solo cuando se toman decisiones para que un sitio / aplicación deje de admitir algún navegador / motor antiguo. |

Los desarrolladores deben centrarse en escribir codigo con una sintaxis nueva y limpia, y dejar que las herramientas se encarguen de producir una versión de ese código compatible con versiones posteriores que sea adecuada para implementar y ejecutar en los entornos de motores JS compatibles más antiguos.

### Llenando los espacios vacios

Si el problema de compatibilidad directa no está relacionado con la nueva sintaxis, sino con un método de API faltante que se agregó recientemente, la solución más común es proporcionar una definición para ese método de API que falta y que actúa como si fuera el entorno más antiguo ya lo había definido de forma nativa. Este patrón se llama polyfill (también conocido como "cuña").

Considere este código:

```js
// getSomeRecords() returns us a promise for some
// data it will fetch
var pr = getSomeRecords();

// show the UI spinner while we get the data
startSpinner();

pr
.then(renderRecords)   // render if successful
.catch(showError)      // show an error if not
.finally(hideSpinner)  // always hide the spinner
```
Este código utiliza una función ES2019, el método `finally(..)` en el prototipo de promesa. Si este código se usara en un entorno anterior a ES2019, el método `finally(..)` no existiría y se produciría un error.

Un polyfill básico para `finally(..)` en entornos anteriores a ES2019 podría verse así:

```js
if (!Promise.prototype.finally) {
    Promise.prototype.finally = function f(fn){
        return this.then(fn,fn);
    };
}
```
| NOTA: |
| :--- |
| Esta es solo una ilustración simple de un ingenuo polyfill para `finally(..)`. No use este enfoque en su código; use un polyfill oficial y robusto siempre que sea posible, como la colección de polyfills / shims en ES-Shim. |

La instrucción `if` protege la definición de polyfill evitando que se ejecute en cualquier entorno donde el motor JS ya haya definido ese método. En entornos más antiguos, el polyfill está definido, pero en entornos más nuevos, la declaración `if` se omite silenciosamente.

Los transpiladores como Babel generalmente detectan qué polyfills necesita su código y se los proporcionan automáticamente. Pero ocasionalmente es posible que deba incluirlos / definirlos explícitamente,lo que funciona de manera similar al fragmento anterior.

Siempre escriba el código utilizando las características más apropiadas para comunicar sus ideas e intenciones de manera efectiva. En general, esto significa usar la versión estable más reciente de JS. Evite afectar negativamente la legibilidad del código al intentar ajustar manualmente las brechas de sintaxis / API. ¡Para eso están las herramientas!

La Transpilación y el polyfills son dos técnicas altamente efectivas para abordar esa brecha entre el código que usa las últimas características estables en el lenguaje y los entornos antiguos que un sitio o aplicación aún necesita soportar. Como JS no va a dejar de mejorar, la brecha nunca desaparecerá. Ambas técnicas deben adoptarse como parte estándar de la cadena de producción de cada proyecto JS en el futuro.

## ¿Qué hay en una interpretación?

Una pregunta largamente debatida para el código escrito en JS: ¿es un script interpretado o un programa compilado? La opinión mayoritaria parece ser que JS es un lenguaje interpretado (scripting). Pero la verdad es más complicada que eso.

Consideremos la pregunta con más detalle. Para empezar, ¿por qué la gente incluso debate o se preocupa por esto? ¿Por qué eso importa?

Durante gran parte de la historia de los lenguajes de programación, los lenguajes "interpretados" y los "scripts" han sido menospreciados en comparación con sus homólogos compilados. Las razones de esta acritud son numerosas, incluida la percepción de falta de optimización del rendimiento, así como el disgusto de ciertas características del lenguaje,como los lenguajes de secuencias de comandos que generalmente usan el tipeo dinámico en lugar de los lenguajes tipeados estáticamente "más maduros".

Estas afirmaciones y críticas mal informadas deben dejarse de lado. La verdadera razón por la que es importante tener una idea clara de si JS se interpreta o compila se relaciona con la naturaleza de cómo se manejan los errores.

Históricamente, los lenguajes de scripts o interpretados se ejecutaban generalmente de arriba hacia abajo y línea por línea; Por lo general, no hay un paso inicial a través del programa para procesarlo antes de que comience la ejecución. En esos lenguajes, no se descubre un error en la línea 5 hasta que las líneas 1 a 4 ya se hayan ejecutado. En particular, ese error en la línea 5 puede deberse a una condición de tiempo de ejecución, como una variable o valor que tiene un valor inadecuado para una operación, o puede deberse a una instrucción / comando mal formado en esa línea. Dependiendo del contexto, diferir el manejo de errores a la línea en la que ocurre el error puede ser un efecto deseable o indeseable.

Compare eso con los lenguajes que pasan por un paso de procesamiento (típicamente llamado "parsing") antes de que ocurra cualquier ejecución. En ese escenario, un comando no válido (como una sintaxis rota) en la línea 5 se capturaría durante este análisis, antes de que se inicie cualquier ejecución, y ninguno de los programas se ejecutará. Para detectar errores de sintaxis (o de otro modo "estáticos"), generalmente se prefiere conocerlos antes de cualquier ejecución parcial condenada.

Entonces, ¿qué tienen en común los lenguajes "parseados" con los lenguajes "compilados"? Primero, se analizan todos los lenguajes compilados. Entonces, un lenguaje parseados está bastante avanzado en el camino hacia la compilación ya. En la teoría de compilación clásica, el último paso restante después del "parseo" es la generación de código: producir un ejecutable.

Una vez que cualquier programa fuente ha sido parseado por completo, es muy común que su ejecución posterior incluya, de alguna forma o manera, una traducción de la forma parseada del programa, generalmente llamado Árbol de sintaxis abstracta (AST).

En otras palabras, los lenguajes parseados generalmente también tienen generación de código antes de la ejecución, por lo que no es tan difícil decir que, en espíritu, es un lenguaje compilado.

El código fuente de JS se analiza antes de ejecutarse. La especificación requiere tanto, porque requiere que se informen "errores tempranos" (errores determinados estáticamente en el código, como un nombre de parámetro duplicado) antes de que el código comience a ejecutarse. Esos errores no pueden reconocerse sin haber analizado el código.

Entonces JS es un lenguaje parseado, pero ¿está compilado?

La respuesta está más cerca del sí que del no. El JS analizado se convierte a una forma optimizada (binaria), y ese "código" se ejecuta posteriormente; el motor normalmente no vuelve a cambiar al modo de ejecución línea por línea después de haber terminado todo el arduo trabajo de análisis, la mayoría de los lenguajes / motores no lo harían, porque eso sería muy ineficiente.

Para ser específicos, esta "compilación" produce un código de bytes binarios (de tipo), que luego se entrega a la "máquina virtual JS" para que se ejecute. A algunos les gusta decir que esta VM está "interpretando" el código de bytes. Pero eso significa que Java, y una docena de otros lenguajes basados ​​en JVM, para el caso, se interpretan en lugar de compilarse. Eso contradice la afirmación típica de que Java / etc son lenguajes compilados. Si bien Java y JavaScript son lenguajes muy diferentes, la cuestión de la interpretación / compilación está bastante estrechamente relacionada entre ellos.

Otra falla es que los motores JS pueden emplear múltiples pases de procesamiento / optimización JIT (Just-In-Time) en el código del programa,que nuevamente podría etiquetarse razonablemente como "compilación" o "interpretación" dependiendo de la perspectiva. En realidad, es una situación fantásticamente compleja bajo el capó de un motor JS.

Entonces, ¿a qué se reducen estos detalles esenciales?

Retroceda y considere todo el flujo de un programa fuente JS: después de que sale del editor de un desarrollador, Babel lo transpila, luego lo empaqueta Webpack (y probablemente se somete a media docena de procesos de compilación), luego se entrega de esa forma muy diferente a un motor JS, luego ese motor analiza el código en un AST, luego el motor convierte ese AST en un tipo de código de bytes, luego ese código de bytes se convierte aún más por el compilador JIT de optimización, y finalmente la VM JS ejecuta el código.

¿Se parece más a un guión interpretado línea por línea (como Bash), o se parece más a un lenguaje compilado que se procesa primero en uno a varios pases, antes de la ejecución?

Creo que está claro que, en espíritu, si no en la práctica, JS es un lenguaje compilado.

Y nuevamente, la razón que importa es que, dado que se compila JS, se nos informa de los errores estáticos (como la sintaxis con formato incorrecto)antes de que se ejecute nuestro código. ¡Es un modelo de interacción sustancialmente diferente al que obtenemos con los programas tradicionales de "scripting", y podría decirse que es más útil!

## E*strict*amente Hablando

En 2009, con el lanzamiento de ES5, JS agregó *strict mode* como un mecanismo opcional para fomentar mejores programas de JS.

Los beneficios del modo estricto superan con creces los costos, pero los viejos hábitos son difíciles de erradicar y la inercia de las bases de código existentes (también conocidas como "legacy") son realmente difíciles de cambiar. Lamentablemente, más de 10 años después, la *opcionalidad* del modo estricto significa que todavía no es necesariamente el valor predeterminado para los programadores de JS.

¿Por qué modo estricto? El modo estricto no debe considerarse como una restricción de lo que no puede hacer, sino como una guía de la mejor manera de hacer las cosas para que el motor JS tenga la mejor oportunidad de optimizar y ejecutar el código de manera eficiente.

La mayoría de los controles de modo estricto tienen la forma de *errores tempranos*, lo que significa que no son estrictamente errores de sintaxis,pero que aún se generan en tiempo de compilación (antes de que se ejecute el código). Por ejemplo, el modo estricto no permite nombrar dos parámetros de función de la misma forma y genera un error temprano. Algunos otros controles de modo estricto solo son observables durante el tiempo de ejecución, como la forma en que la palabra clave `this` se predetermina a `undefined` en lugar del objeto global.

En lugar de pelear y discutir con el modo estricto, como un niño que solo quiere desafiar lo que sea que sus padres les digan que no hagan, la mejor mentalidad es que el modo estricto es como un *linter* que te recuerda cómo JS *debe* ser escrito para tener la más alta calidad y la mejor oportunidad de rendimiento. Si te sientes esposado, tratando de evitar el modo estricto, esa debería ser una alarmante señal de advertencia roja que debes respaldar y repensar todo el enfoque.

El modo estricto se activa por programa (¡por archivo!) Así:

```js
// only whitespace and comments are allowed
// before the use-strict pragma

"use strict";

// everything in this file runs in strict
// mode
```

Un pequeño inconveniente a tener en cuenta es que incluso un `;` perdido que aparezca por sí solo ante el pragma hará que el pragma sea inútil; no se arrojan errores porque es válido para JS tener una expresión literal de cadena en una posición de declaración, ¡pero también silenciosamente *no* se activará el modo estricto!

El modo estricto se puede activar alternativamente por alcance de función,con exactamente las mismas reglas / advertencias sobre su posicionamiento:

```js
function someOperations() {
    // whitespace and comments are fine here
    "use strict";

    // all this code will run in strict mode
}
```

Curiosamente, si un archivo tiene activado el modo estricto, los pragmas de modo estricto de nivel de función no están permitidos. Entonces tienes que elegir uno u otro.

El **único** motivo válido para utilizar un enfoque por función para el modo estricto es cuando se está convirtiendo un archivo de programa de modo no estricto existente y necesita realizar los cambios poco a poco con el tiempo. De lo contrario, es mucho mejor simplemente activar el modo estricto para todo el archivo / programa.

Muchos se han preguntado si alguna vez hubo un momento en que JS hiciera que el modo estricto sea el predeterminado. La respuesta es, casi seguro que no. Como discutimos anteriormente sobre la compatibilidad con versiones anteriores, si una actualización del motor JS comenzara a suponer que el código era un modo estricto, incluso si no está marcado como tal, es posible que este código se rompa como resultado de los controles del modo estricto.

Sin embargo, hay algunos factores que reducen el impacto futuro de esta "oscuridad" no predeterminada del modo estricto.

Por un lado, prácticamente todo el código transpilado termina en modo estricto, incluso si el código fuente original no está escrito como tal. La mayoría del código JS en producción se ha transpilado, por lo que eso significa que la mayoría del JS ya se está adhiriendo al modo estricto. Es posible deshacer esa suposición, pero realmente tiene que salir de su camino para hacerlo, por lo que es muy poco probable.

Además, se está produciendo un gran cambio hacia la escritura del código JS más nuevo utilizando el formato del módulo ES6. Los módulos ES6 asumen el modo estricto, por lo que todo el código en dichos archivos se establece automáticamente en modo estricto.

En conjunto, el modo estricto es en gran medida el valor predeterminado de facto, aunque técnicamente no es el valor predeterminado.

## Definido

JS es una implementación del estándar ECMAScript (versión ES2019 al momento de escribir esto), que está guiado por el comité TC39 y patrocinado por ECMA. Se ejecuta en navegadores y otros entornos JS como Node.js.

JS es un lenguaje multi-paradigmático, lo que significa que la sintaxis y las capacidades permiten a un desarrollador mezclar y combinar conceptos (¡y doblar y remodelar!) de varios paradigmas principales, como procedimientos, orientados a objetos (OO / clases) y funcionales. (FP).

JS es un lenguaje compilado, lo que significa que las herramientas (incluido el motor JS) procesan y verifican un programa (¡informando cualquier error!) Antes de que se ejecute.

Con nuestro lenguaje ahora *definido*, comencemos a conocer sus entresijos.