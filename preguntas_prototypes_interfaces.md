# 📝 Preguntas Prototypes, `this` y Interfaces

## 1. Prototypes & `this` en JavaScript

### Pregunta 1

¿Qué devuelve este código?

```js
function Animal() {}
function Perro() {}

Perro.prototype = Object.create(Animal.prototype);

const fido = new Perro();

console.log(fido.__proto__ === Perro.prototype);
```

a) false  
b) true  
c) undefined  
d) Error

**Respuesta:** ✅ b) true  
**Explicación:** Una instancia se conecta directamente al `.prototype` de su constructor.

---

### Pregunta 2

¿Qué pasa si sobrescribes `Perro.prototype` en lugar de extenderlo?

```js
function Perro() {}
Perro.prototype = {
  ladrar() {
    console.log("Guau");
  },
};
```

a) Se pierden las propiedades del `prototype` original  
b) Hereda automáticamente de `Object.prototype`  
c) Ambos a y b  
d) Error

**Respuesta:** ✅ c) Ambos a y b  
**Explicación:** Sobrescribir elimina la cadena previa, pero como todo objeto hereda de `Object.prototype`, esa conexión se mantiene.

---

### Pregunta 3

¿Qué imprimirá este código?

```js
const obj = {
  nombre: "JS",
  arrow: () => console.log(this.nombre),
  normal: function () {
    console.log(this.nombre);
  },
};

obj.arrow();
obj.normal();
```

a) JS y JS  
b) undefined y JS  
c) undefined y undefined  
d) Error

**Respuesta:** ✅ b) undefined y JS  
**Explicación:** Las arrow functions no tienen su propio `this`, toman el del contexto léxico (en este caso el global, no el objeto).

---

### Pregunta 4

¿Qué ocurre aquí?

```js
(() => {
  console.log(this);
})();
```

a) window o globalThis  
b) undefined en modo estricto  
c) El objeto que ejecuta la función  
d) Error

**Respuesta:** ✅ a) window o globalThis  
**Explicación:** Una arrow function no crea un `this` nuevo, captura el del contexto superior, que en este caso es global.

---

### Pregunta 5 (trampa)

¿Qué muestra?

```js
function A() {}
console.log(A.__proto__ === Function.prototype);
```

a) false  
b) true  
c) undefined  
d) Error

**Respuesta:** ✅ b) true  
**Explicación:** Todas las funciones en JS son instancias de `Function`, por lo tanto su `__proto__` apunta a `Function.prototype`.

---

## 2. Interfaces en TypeScript

### Pregunta 1

¿Cuál de las siguientes NO se puede hacer con `interface`?

a) Definir contratos para clases  
b) Extender de múltiples interfaces  
c) Definir un alias de tipo primitivo  
d) Usar propiedades opcionales

**Respuesta:** ✅ c) Definir un alias de tipo primitivo  
**Explicación:** Eso es algo que solo se puede hacer con `type`.

---

### Pregunta 2

¿Qué imprimirá este código?

```ts
interface Usuario {
  id: number;
  nombre: string;
}

interface Usuario {
  edad: number;
}

const u: Usuario = { id: 1, nombre: "Ana", edad: 25 };
console.log(u);
```

a) Error: interfaces no se pueden redefinir  
b) { id: 1, nombre: "Ana", edad: 25 }  
c) undefined  
d) Solo imprime { id: 1, nombre: "Ana" }

**Respuesta:** ✅ b) { id: 1, nombre: "Ana", edad: 25 }  
**Explicación:** Las interfaces permiten **merging**, por eso ambas se combinan.

---

### Pregunta 3

¿Qué pasa con este código?

```ts
interface A {
  a: string;
}
interface B {
  b: string;
}
interface C extends A, B {
  c: string;
}

const obj: C = { a: "uno", b: "dos", c: "tres" };
console.log(obj.a, obj.b, obj.c);
```

a) Imprime uno dos tres  
b) Error: no se pueden extender múltiples interfaces  
c) Error de tipo  
d) Imprime undefined undefined undefined

**Respuesta:** ✅ a) Imprime uno dos tres  
**Explicación:** Una interface puede extender de múltiples interfaces sin problema.

---

### Pregunta 4

¿Qué ocurre aquí?

```ts
interface Fn {
  (x: number, y: number): number;
}

const sumar: Fn = (a, b) => a + b;
console.log(sumar(2, 3));
```

a) Imprime 5  
b) Error: no se puede tipar funciones con interfaces  
c) undefined  
d) Error de compilación

**Respuesta:** ✅ a) Imprime 5  
**Explicación:** Una interface puede describir la forma de una función perfectamente.

---

### Pregunta 5 (trampa)

```ts
type X = { a: number };
interface Y {
  a: number;
}

let uno: X = { a: 1 };
let dos: Y = { a: 2 };

uno = dos;
dos = uno;

console.log(uno, dos);
```

¿Qué ocurre?

a) Error de asignación en uno = dos;  
b) Error de asignación en dos = uno;  
c) Ambos son compatibles e imprime { a: 1 } { a: 1 }  
d) Ambos son compatibles e imprime { a: 1 } { a: 2 }

**Respuesta:** ✅ d) Ambos son compatibles e imprime { a: 1 } { a: 2 }  
**Explicación:** En TypeScript lo importante es la **estructura**, no si es `type` o `interface`. Ambos son intercambiables aquí.
