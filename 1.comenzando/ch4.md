# Aún no sabes JS: Comenzando - 2da Edición
# Capítulo 4: El Resto

| NOTA DEL AUTOR: |
| :--- |
| Trabajo en progreso |

Este libro analiza lo que debes tener en cuenta al *comenzar* con JS. El objetivo es llenar los vacíos con que los lectores más nuevos en JS podrían haber tropezado en sus primeros encuentros con el lenguaje. También espero haber insinuado suficientes detalles más profundos para despertar su curiosidad y querer profundizar más en el lenguaje.

El resto de los libros de esta serie es donde desempacaremos todo el resto del lenguaje, con mucho más detalle de lo que podríamos haber hecho en unos breves capítulos aquí.

Sin embargo, recuerda tomarte tu tiempo. En lugar de apresurarte hacia el próximo libro en un intento de repasar todos los libros de manera conveniente, pase un tiempo repasando el material de este libro. Pasa más tiempo mirando el código en tus proyectos actuales y comparando lo que ves con lo que se ha discutido aquí.

Cuando estés listo, este capítulo final proporciona una breve vista previa y una hoja de ruta de lo que puede esperar del resto de la serie de libros, y cómo sugiero que continúe. Además, no omita el Apéndice A, "¡Practique, practique, practique!"

## Pilar 1: Alcance y Closure

La organización de variables en unidades de alcance (funciones, bloques) es una de las características más fundamentales de cualquier lenguaje; quizás ninguna otra característica tenga un mayor impacto en cómo se comportan los programas.

Los ámbitos son como cubos, y las variables son como canicas que pones en esos cubos. El modelo de alcance de un lenguaje es como las reglas que lo ayudan a determinar qué canicas de color van en qué cubos de colores coincidentes.

Los ámbitos se anidan uno dentro del otro, y para cualquier expresión o declaración dada, solo las variables en ese nivel de anidación de ámbito, o en ámbitos superiores/externos, son accesibles; Las variables de los ámbitos inferior/interno están ocultas e inaccesibles.

Así es como se comportan los ámbitos en la mayoría de los lenguajes, lo que se denomina ámbito léxico. Los límites de la unidad de alcance y cómo se organizan las variables en ellos se determinan en el momento en que se analiza (compila) el programa. En otras palabras, es una decisión de autor-tiempo: donde ubica una función/alcance en el programa determina cuál será la estructura del alcance de esa parte del programa.

JS tiene un ámbito léxico, aunque muchos afirman que no, debido a dos características particulares de su modelo que no están presentes en otros lenguajes de ámbito léxico.

El primero se llama comúnmente *hoisting* (elevación): cuando todas las variables declaradas en cualquier lugar de un ámbito se tratan como si se declararan al principio del ámbito. La otra es que las variables declaradas `var` tienen un alcance de función, incluso si aparecen dentro de un bloque.

| NOTA: |
| :--- |
| A menudo se afirma que solo se suben las declaraciones `var`, pero no `let` y `const`, debido a su comportamiento de "zona muerta temporal" (TDZ). Pero esta afirmación es incorrecta; de hecho, los tres declaradores están *hoistiados*, porque los identificadores declarados con cualquiera de estas palabras clave son observables (¡si no utilizables!) en todo el ámbito. La diferencia real es que las declaraciones `var` son **también** autoinicializadas a `undefined` al comienzo de un ámbito, de modo que puedan utilizarse en todo el ámbito. Por el contrario, las declaraciones `let` y `const` no se inicializan hasta que se ejecutan sus declaraciones, lo que significa que esos identificadores están presentes pero no se pueden usar hasta entonces (es decir, TDZ). |

Ni el hoisting ni el alcance de función de `var` son suficientes para respaldar la afirmación de que JS no tiene un alcance léxico, como tampoco lo haría TDZ (también exclusivo de las declaraciones `let` y `const` de JS). Estas son solo partes únicas del lenguaje que todos los desarrolladores de JS deben aprender y comprender.

El closure es un resultado natural del alcance léxico cuando el lenguaje tiene funciones como valores de primera clase, como lo hace JS. Cuando una función hace referencia a variables desde un ámbito externo, y esa función se pasa como un valor y se ejecuta en otros ámbitos, mantiene el acceso a sus variables de ámbito originales; Esto es closure.

En toda la programación, pero especialmente en JS, el closure impulsa muchos de los patrones de programación más importantes, incluidos los módulos. Tal como lo veo, los módulos son *el corriente* cuando se trata de la organización del código en JS.

Para profundizar en el alcance, los closures y el funcionamiento de los módulos, lea el Libro 2, *Alcance y closures*.

## Pilar 2: Prototipo

El segundo pilar del lenguaje es el sistema de prototipos. Cubrimos este tema en profundidad en el Capítulo 3 ("Prototipos"), pero solo quiero hacer algunos comentarios más sobre su importancia.

JS es uno de los pocos lenguajes en los que tienes la opción de crear objetos a medida, sin necesidad de definir primero su estructura en una clase.

Durante muchos años, las personas implementaron el patrón de diseño de clase sobre los prototipos, llamado "herencia prototípica", y luego con el advenimiento de la palabra clave `class` de ES6, el lenguaje se duplicó en su inclinación hacia el estilo de programación OO / clases.

Pero creo que ese enfoque ha oscurecido la belleza y el poder del sistema prototipo: la capacidad de dos objetos para simplemente conectarse entre sí y cooperar dinámicamente (durante la ejecución de la función / método) al compartir un contexto 'this'.

Las clases son solo un patrón que puedes construir con ese poder. Pero otro enfoque, en una dirección muy diferente, es simplemente abrazar los objetos como objetos, olvidar las clases por completo y dejar que los objetos cooperen a través de la cadena de prototipos. Esto se llama *delegación de comportamiento*. Creo que la delegación es más poderosa que la herencia de clases, como un medio para organizar el comportamiento y los datos en nuestros programas.

Pero la herencia de clase recibe casi toda la atención. Y el resto va a la programación funcional (FP), como una forma "anti-clase" de diseño de programas. Esto me entristece, porque elimina cualquier posibilidad de exploración de la delegación como una alternativa viable.

Te animo a que pases mucho tiempo en el Libro 3, *Objetos y clases*, para ver cómo la delegación de objetos tiene mucho más potencial del que quizás nos hayamos dado cuenta. Este no es un mensaje anti-`clase`, pero es intencionalmente un mensaje "las clases no son la única forma de usar objetos" que quiero que consideren más desarrolladores de JS.

La delegación de objetos es, diría, mucho más *el corriente* de JS que las clases (más sobre *corriente* en un momento).

## Pillar 3: Tipos y Coercion

El tercer pilar de JS es, con mucho, la parte más ignorada de la identidad de JS.

La gran mayoría de los desarrolladores tienen ideas erróneas sobre cómo funcionan los *tipos* en los lenguajes de programación, y especialmente cómo funcionan en JS. Una ola de interés en la comunidad JS más amplia ha comenzado a cambiar a enfoques de "tipeo estático", utilizando herramientas de tipo consciente como TypeScript o Flow.

Estoy de acuerdo en que los desarrolladores de JS deberían aprender más sobre los tipos, y deberían aprender más sobre cómo JS gestiona las conversiones de tipos. ¡También estoy de acuerdo en que las herramientas con reconocimiento de tipos pueden ayudar a los desarrolladores, suponiendo que hayan adquirido y utilizado este conocimiento en primer lugar!

Pero no estoy de acuerdo en absoluto en que la conclusión inevitable de esto es decidir que el mecanismo de tipos de JS es malo y que necesitamos encubrir los tipos de JS con soluciones fuera del lenguaje. No tenemos que seguir la forma de "escritura estática" para ser inteligentes y sólidos con los tipos en nuestros programas. Hay otras opciones, si solo estás dispuesto a ir *contra la corriente* de la multitud y *con la corriente* de JS (de nuevo, más sobre eso a continuación).

Podría decirse que este pilar es más importante que los otros dos, en el sentido de que ningún programa de JS hará nada útil si no aprovecha adecuadamente los tipos de valor de JS, así como también los valores se convierten (coaccionan) entre tipos.

Incluso si le encanta TypeScript/Flow, no obtendrá el máximo provecho de esas herramientas o enfoques de codificación si no está profundamente familiarizado con la forma en que el lenguaje gestiona los tipos de valor.

No puedo exponer completamente el caso aquí, para que aprendas sobre los tipos JS y la coerción; ¡para eso está todo el Libro 4, *Tipos y Gramática*! Pero no omita este tema porque siempre ha escuchado que podemos usar `===` y olvidarnos del resto.

¡Tus bases en JS son inestables e incompletas en el mejor de los casos, sin este pilar!

## En Orden

Así que ahora tienes una mejor idea de lo que queda por explorar en JS.

Pero una de las preguntas más comunes que recibo es: "¿En qué orden debo leer los libros?" Aquí hay una respuesta directa, pero también depende.

Mi sugerencia para la mayoría de los lectores es, esta es la mejor manera de continuar con esta serie:

1. Comience con una base sólida de JS con *Comenzando* (este libro).

2. En *Scope & Closures*, aprenda el primer pilar de JS: alcance léxico, cómo eso apoya al closure y cómo el patrón de módulo organiza el código.

3. En *Objetos y Clases*, concéntrese en el segundo pilar de JS: cómo funciona `this` de JS, cómo los prototipos de objetos admiten la delegación y cómo los prototipos habilitan el mecanismo `class` para la organización del código al estilo OO.

4. En *Tipos y Gramática*, aborde el tercer y último pilar de JS: tipos y coerción de tipos, así como la sintaxis y la gramática de JS definen cómo escribimos nuestro código.

5. Con los **tres pilares** firmemente en su lugar, luego en *Sync & Async* puedes explorar cómo usamos el control de flujo para modelar el cambio de estado en nuestros programas, tanto sincrónicamente (de inmediato) como asincrónicamente (con el tiempo).

6. La serie concluye con *ES.Next & Beyond*, una mirada hacia el futuro a corto y mediano plazo de JS, que incluye una variedad de características que probablemente lleguen a sus programas JS en poco tiempo.

Esa es el orden previsto para leer esta serie de libros.

Sin embargo, los libros 2, 3 y 4 generalmente se pueden leer en cualquier orden, dependiendo del tema sobre el que se sienta más curioso y cómodo explorando primero. Pero no le recomiendo que omita ninguno de estos tres libros, ni siquiera los tipos/coerción, ¡ya que algunos de ustedes estarán tentados a hacerlo! - incluso si ya te sientes cómodo con ese tema a simple vista.

El Libro 5 (*Sync & Async*) es crucial para comprender a fondo JS, pero si comienza a investigar y encuentra que es demasiado intimidante, este libro puede diferirse hasta que tenga más experiencia con el lenguaje. Mientras más JS haya escrito (¡y haya luchado!), Más apreciará este libro.

El libro final de la serie, *ES.Next & Beyond*, en algunos aspectos está solo. Se puede leer al final, como sugiero, o justo después de *Getting Started* si estás buscando un atajo para ampliar tu radar de lo que se trata JS. También es más probable que este libro reciba actualizaciones en el futuro, por lo que es probable que desee volver a visitarlo ocasionalmente.

## Con la corriente

Una nota final sobre cómo continuar su viaje de aprendizaje con JS y su camino a través del resto de esta serie de libros: tenga en cuenta la *corriente* -- recuerde varias referencias a la *corriente* anteriormente en este capítulo.

Primero, considere la *corriente* a cómo la mayoría de las personas se acerca y usa JS. Probablemente ya haya notado que estos libros cortan esa *corriente* en muchos aspectos. En YDKJSY, respeto al lector lo suficiente como para explicar todas las partes de JS, no solo algunas partes populares seleccionadas. Creo que ambos son capaces y merecedores de ese conocimiento.

Pero eso no es lo que encontrará en muchos otros materiales. También significa que cuanto más siga y se adhiera a la guía de estos libros, que piense detenidamente y analice por sí mismo lo que es mejor en su código, más se destacará. Eso puede ser algo bueno y malo. Si alguna vez quieres salir de la multitud, ¡tendrás que separarte de cómo lo hace la multitud!

Pero también he hecho que muchas personas me digan que citaron algún tema / explicación de estos libros durante una entrevista de trabajo, y el entrevistador le dijo al candidato que estaban equivocados; de hecho, según los informes, las personas han perdido ofertas de trabajo.

En la medida de lo posible, me esfuerzo en estos libros para proporcionar información completamente precisa sobre JS, informada generalmente de la propia especificación. Pero también comparto algunas de mis opiniones sobre cómo puede interpretar y utilizar JS para el mejor beneficio en sus programas. No presento la opinión como un hecho, o viceversa. Siempre sabrás cuál es cuál en estos libros.

Los hechos sobre JS no están realmente en discusión. O la especificación dice algo o no. Si no te gusta lo que dice la especificación, o mi retransmisión de la misma, ¡tómalo con TC39! Si está en una entrevista y afirman que está equivocado en los hechos, pregúnteles en ese mismo momento si puede buscarlo en la especificación. Si el entrevistador no lo vuelve a considerar, entonces no debería querer trabajar allí de todos modos.

Pero si elige alinearse con mis opiniones, debe estar preparado para respaldar esas elecciones con *por qué* se siente de esa manera. No solo repitas lo que digo. Sé dueño de tus opiniones. Defiéndelos. Y si alguien con quien esperabas trabajar no está de acuerdo, aléjate con la cabeza en alto. Es un gran JS, y hay mucho espacio para muchas formas diferentes.

En otras palabras, no tengas miedo de ir en contra de la *corriente*, como lo he hecho con estos libros y todas mis enseñanzas. Nadie puede decirle cómo utilizará mejor JS; Eso es para que usted decida. Simplemente estoy tratando de empoderarlo para llegar a sus propias conclusiones, sin importar cuáles sean.

Por otro lado, hay una *corriente* a la que realmente debes prestar atención y seguir: la *corriente* de cómo funciona JS, a nivel de lenguaje. Hay cosas que funcionan bien y naturalmente en JS, dada la práctica y el enfoque correctos, y hay cosas que realmente no deberías intentar hacer en el lenguaje.

¿Puedes hacer que tu programa JS parezca un programa Java, C # o Perl? ¿Qué pasa con Python o Ruby, o incluso PHP? En diversos grados, seguro que puedes. ¿Pero deberías?

No, no creo que debas. Creo que deberías aprender y adoptar la forma JS, y hacer que tus programas JS sean lo más prácticos posible. Algunos pensarán que eso significa programación descuidada e informal, pero no lo digo en absoluto. Solo quiero decir que JS tiene muchos patrones y expresiones idiomáticas que son reconociblemente "JS", y seguir esa *corriente* es el camino general hacia el mejor éxito.

Finalmente, quizás la *corriente* más importante para reconocer es cómo los programas existentes en los que está trabajando y los desarrolladores con los que está trabajando hacen cosas. No leas estos libros y luego intentes cambiar *toda esa corriente* en sus proyectos existentes durante la noche. Ese enfoque siempre fallará.

Tendrás que cambiar estas cosas poco a poco, con el tiempo. Trabaja para llegar a un consenso con tus colegas desarrolladores sobre por qué es importante volver a visitar y volver a considerar un enfoque. Pero hágalo con solo un pequeño tema a la vez, y deje que las comparaciones de código antes y después hagan la mayor parte de la conversación. Reúna a todos los miembros del equipo para debatir y presione para tomar decisiones basadas en el análisis y la evidencia del código en lugar de la inercia de "nuestros desarrolladores senior siempre lo han hecho de esta manera".

Ese es el consejo más importante con el que puedo dejarte. Siempre busca mejores formas de usar lo que JS nos da para crear códigos más legibles. ¡Todos los que trabajen en tu código, incluido tu yo futuro, te lo agradecerán!

Antes de pasar al siguiente libro de la serie, *Scope & Closures*, revisa y practica los fragmentos en el Apéndice A, "¡Practica, practica, practica!" ¿Mencioné que deberías ir a practicar? No hay mejor manera de aprender código que escribirlo.