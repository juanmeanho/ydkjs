# Aún no conoces JS: Comenzando - 2da Edición
# Apéndice A: Práctica, Práctica, Práctica!

| NOTA DEL AUTOR: |
| :--- |
| Trabajo en progreso |

En este apéndice, exploraremos algunos ejercicios y sus soluciones sugeridas. Estos son solo para *comenzar* con la práctica sobre los conceptos del libro.

## Practicando Comparaciones

En este apéndice, exploraremos algunos ejercicios y sus soluciones sugeridas. Estos son solo para *comenzar* con la práctica sobre los conceptos del libro.

Practiquemos trabajar con tipos de valores y comparaciones donde la coerción tendrá que estar involucrada.

`scheduleMeeting(..)` debe tomar una hora de inicio (en formato de 24 horas como una cadena "hh:mm") y una duración de la reunión (número de minutos). Debería devolver `verdadero` si la reunión se realiza completamente dentro del día laboral (de acuerdo con los tiempos especificados en `dayStart` y `dayEnd`); devolver `falso` si la reunión viola los límites del día laboral.

```js
const dayStart = "07:30";
const dayEnd = "17:45";

function scheduleMeeting(startTime,durationMinutes) {
    // ..TODO..
}

scheduleMeeting("7:00",15);     // false
scheduleMeeting("07:15",30);    // false
scheduleMeeting("7:30",30);     // true
scheduleMeeting("11:30",60);    // true
scheduleMeeting("17:00",45);    // true
scheduleMeeting("17:30",30);    // false
scheduleMeeting("18:00",15);    // false
```

Try to solve this yourself first. Consider the usage of equality and relational comparison operators, and how coercion impacts this code. Once you have code that works, *compare* your solution(s) to the code in "Suggested Solutions" at the end of this appendix.

## Practicando Closures

Ahora practiquemos con closure.

La función `range(..)` toma un número como primer argumento, representando el primer número en un rango deseado de números. El segundo argumento también es un número que representa el final del rango deseado (inclusive). Si se omite el segundo argumento, se debe devolver otra función que espere ese argumento.

```js
function range(start,end) {
    // ..TODO..
}

range(3,3);    // [3]
range(3,8);    // [3,4,5,6,7,8]
range(3,0);    // []

var start3 = range(3);
var start4 = range(4);

start3(3);     // [3]
start3(8);     // [3,4,5,6,7,8]
start3(0);     // []

start4(6);     // [4,5,6]
```

Intenta resolver esto tú mismo primero.

Una vez que tenga un código que funcione, *compare* sus soluciones con el código en "Soluciones sugeridas" al final de este apéndice.

## Practicando Prototipos

Trabajemos en `this` y en los objetos vinculados mediante un prototipo.

Defina una máquina tragamonedas con 3 carretes que puedan `spin()` (girar) individualmente y luego `display()` (mostrar) el contenido actual de todos los carretes.

El comportamiento básico de un solo carrete se define en el objeto `reel` a continuación. Pero la máquina tragamonedas necesita carretes individuales, objetos que delegan en 'carrete' y que tienen una propiedad de 'posición'.

Un carrete solo *sabe cómo* `display()` (mostrar) su símbolo de ranura actual, pero una máquina tragamonedas generalmente muestra 3 símbolos por carrete: la ranura actual (`position`), una ranura arriba (`position - 1`), y una ranura debajo (`position + 1`). Entonces, mostrar la máquina tragamonedas debería terminar mostrando una cuadrícula de símbolos de tragamonedas 3 x 3.

```js
function randMax(max) {
    return Math.trunc(1E9 * Math.random()) % max;
}

var reel = {
    symbols: [ "♠", "♥", "♦", "♣", "☺", "★", "☾", "☀" ],
    spin() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        this.position = (
            this.position + 100 + randMax(100)
        ) % this.symbols.length;
    },
    display() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        return this.symbols[this.position];
    }
};

var slotMachine = {
    reels: [
        // this slot machine needs 3 separate reels
        // hint: Object.create(..)
    ],
    spin() {
        this.reels.forEach(function spinReel(reel){
            reel.spin();
        });
    },
    display() {
        // TODO
    }
};

slotMachine.spin();
slotMachine.display();
// ☾ | ☀ | ★
// ☀ | ♠ | ☾
// ♠ | ♥ | ☀

slotMachine.spin();
slotMachine.display();
// ♦ | ♠ | ♣
// ♣ | ♥ | ☺
// ☺ | ♦ | ★
```

Intenta resolver esto tú mismo primero.

Consejos:

1. use el operador de módulo `%` para ajustar la `position` a medida que accede a los símbolos de forma circular alrededor de un carrete.

2. use `Object.create(..)` para crear un objeto y prototipo-vinculado a otro objeto. Una vez vinculado, la delegación permite que los objetos compartan este contexto durante la invocación del método.

3. en lugar de modificar el objeto del carrete directamente para mostrar cada una de las tres posiciones, puede usar otro objeto temporal (`Object.create(..)` nuevamente) con su propia `position`, para delegar.

Una vez que tenga un código que funcione, *compare* sus soluciones con el código en "Soluciones sugeridas" al final de este apéndice.

## Solucions sugeridas

Tenga en cuenta que estas soluciones sugeridas son solo eso: sugerencias. Hay muchas formas diferentes de resolver estos ejercicios de práctica. Compare su enfoque con lo que ve aquí, y considere los pros y los contras de cada uno.

Solución sugerida para la práctica de "Comparaciones":

```js
const dayStart = "07:30";
const dayEnd = "17:45";

function scheduleMeeting(startTime,durationMinutes) {
    var [ , meetingStartHour, meetingStartMinutes ] =
        startTime.match(/^(\d{1,2}):(\d{2})$/) || [];

    durationMinutes = Number(durationMinutes);

    if (
        typeof meetingStartHour == "string" &&
        typeof meetingStartMinutes == "string"
    ) {
        let durationHours = Math.floor(durationMinutes / 60);
        durationMinutes = durationMinutes - (durationHours * 60);
        let meetingEndHour = Number(meetingStartHour) + durationHours;
        let meetingEndMinutes = Number(meetingStartMinutes) + durationMinutes;

        if (meetingEndMinutes > 60) {
            meetingEndHour = meetingEndHour + 1;
            meetingEndMinutes = meetingEndMinutes - 60;
        }

        // re-compose fully-qualified time strings
        // (to make comparison easier)
        let meetingStart = `${
            meetingStartHour.padStart(2,"0")
        }:${
            meetingStartMinutes.padStart(2,"0")
        }`;
        let meetingEnd = `${
            String(meetingEndHour).padStart(2,"0")
        }:${
            String(meetingEndMinutes).padStart(2,"0")
        }`;

        // NOTE: since expressions are all strings,
        // comparisons here are alphabetic, but that's
        // safe here since they're fully qualified
        // time strings (ie, "07:15" < "07:30")
        return (
            meetingStart >= dayStart &&
            meetingEnd <= dayEnd
        );
    }

    return false;
}

scheduleMeeting("7:00",15);     // false
scheduleMeeting("07:15",30);    // false
scheduleMeeting("7:30",30);     // true
scheduleMeeting("11:30",60);    // true
scheduleMeeting("17:00",45);    // true
scheduleMeeting("17:30",30);    // false
scheduleMeeting("18:00",15);    // false
```
Solución sugerida para la práctica de "Closure":

```js
function range(start,end) {
    start = Number(start) || 0;

    if (end === undefined) {
        return function getEnd(end) {
            return getRange(start,end);
        };
    }
    else {
        end = Number(end) || 0;
        return getRange(start,end);
    }


    // **********************

    function getRange(start,end) {
        var ret = [];
        for (let i = start; i <= end; i++) {
            ret.push(i);
        }
        return ret;
    }
}

range(3,3);    // [3]
range(3,8);    // [3,4,5,6,7,8]
range(3,0);    // []

var start3 = range(3);
var start4 = range(4);

start3(3);     // [3]
start3(8);     // [3,4,5,6,7,8]
start3(0);     // []

start4(6);     // [4,5,6]
```
Solución sugerida para la práctica de "Prototipos":

```js
function randMax(max) {
    return Math.trunc(1E9 * Math.random()) % max;
}

var reel = {
    symbols: [ "♠", "♥", "♦", "♣", "☺", "★", "☾", "☀" ],
    spin() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        this.position = (
            this.position + 100 + randMax(100)
        ) % this.symbols.length;
    },
    display() {
        if (this.position == null) {
            this.position = randMax(this.symbols.length - 1);
        }
        return this.symbols[this.position];
    }
};

var slotMachine = {
    reels: [
        Object.create(reel),
        Object.create(reel),
        Object.create(reel)
    ],
    spin() {
        this.reels.forEach(function spinReel(reel){
            reel.spin();
        });
    },
    display() {
        var lines = [];

        // display all 3 lines on the slot machine
        for (let linePos = -1; linePos <= 1; linePos++) {
            let line = this.reels.map(function getSlot(reel){
                var slot = Object.create(reel);
                slot.position = (
                    reel.symbols.length + reel.position + linePos
                ) % reel.symbols.length;
                return reel.display.call(slot);
            });
            lines.push(line.join(" | "));
        }

        return lines.join("\n");
    }
};

slotMachine.spin();
slotMachine.display();
// ☾ | ☀ | ★
// ☀ | ♠ | ☾
// ♠ | ♥ | ☀

slotMachine.spin();
slotMachine.display();
// ♦ | ♠ | ♣
// ♣ | ♥ | ☺
// ☺ | ♦ | ★
```

Eso es todo por este libro. Pero ahora es el momento de buscar proyectos reales para practicar estas ideas. ¡Solo sigue codificando, porque esa es la mejor manera de aprender!