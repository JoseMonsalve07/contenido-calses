# 🏫 Clase: Prototipos y `this` en JavaScript

## 1. Introducción: ¿Qué es un prototipo?

- En JavaScript, **todo objeto tiene un prototipo** (salvo `Object.create(null)`).
- El prototipo es simplemente **otro objeto** del cual se heredan propiedades y métodos.
- La cadena de prototipos termina en `null`.

```js
const obj = { nombre: "Carlos" };
console.log(obj.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__); // null
```

---

## 2. Diferencia entre `prototype` y `__proto__`

- **`prototype`**: propiedad de funciones constructoras o clases. Define qué heredarán las instancias.
- **`__proto__`**: propiedad interna de cada objeto que apunta a su prototipo inmediato.

```js
function Persona(nombre) {
  this.nombre = nombre;
}

const juan = new Persona("Juan");

console.log(Persona.prototype); // { constructor: f }
console.log(juan.__proto__ === Persona.prototype); // true
```

---

## 3. Clases como sugar sintax

- `class` no introduce clases reales como en Java, solo una sintaxis más limpia.
- Internamente sigue usando funciones constructoras + prototipos.

```js
// Función constructora
function Animal(nombre) {
  this.nombre = nombre;
}
Animal.prototype.mover = function () {
  console.log(this.nombre + " se mueve");
};

// class (azúcar sintáctico)
class Animal2 {
  constructor(nombre) {
    this.nombre = nombre;
  }
  mover() {
    console.log(this.nombre + " se mueve");
  }
}
```

---

## 4. Cadena de prototipos con `extends`

```js
class Animal {
  mover() {
    console.log("Se mueve");
  }
}
class Perro extends Animal {
  ladrar() {
    console.log("Guau");
  }
}

const fido = new Perro();

console.log(fido.__proto__ === Perro.prototype); // true
console.log(Perro.prototype.__proto__ === Animal.prototype); // true
console.log(Animal.prototype.__proto__ === Object.prototype); // true
```

👉 Diagrama:

```
fido → Perro.prototype → Animal.prototype → Object.prototype → null
```

---

## 5. La otra cadena: constructores

```js
console.log(Perro.__proto__ === Animal); // true
console.log(Animal.__proto__ === Function.prototype); // true
console.log(Function.prototype.__proto__ === Object.prototype); // true
```

👉 Diagrama paralelo:

```
Perro → Animal → Function.prototype → Object.prototype → null
```

---

## 6. `this` en funciones normales vs arrow functions

- Funciones normales: `this` depende de **cómo se invoca**.
- Arrow functions: `this` se hereda del **contexto léxico** (donde se escribieron).

```js
const obj = {
  nombre: "Carlos",
  normal() {
    console.log(this.nombre);
  },
  arrow: () => {
    console.log(this.nombre);
  },
};

obj.normal(); // "Carlos"
obj.arrow(); // undefined (o Window.nombre)
```

---

## 7. Arrow functions anidadas

```js
const obj = {
  nombre: "Carlos",
  metodoNormal: function () {
    const arrow = () => console.log(this.nombre);
    arrow();
  },
};

obj.metodoNormal(); // "Carlos"
```

Explicación: la arrow hereda el `this` del método normal (`obj`).

---

## 8. Ejercicio de predicción

```js
function Animal() {
  this.tipo = "animal";
  this.arrow = () => console.log("arrow:", this.tipo);
}
Animal.prototype.hablar = function () {
  console.log("hablar:", this.tipo);
};

const gato = new Animal();
gato.hablar(); // ?
gato.arrow(); // ?
```

👉 Solución esperada:

- `gato.hablar()` → `"animal"` (porque se llama desde la instancia).
- `gato.arrow()` → `"animal"` también, porque la arrow se definió dentro del constructor y capturó el `this` de ese momento (la instancia).

---

## 9. Preguntas trampas

1. ¿Qué pasa si hago `obj.prototype` en un objeto literal?  
   👉 Respuesta: `undefined`. Solo las funciones tienen `.prototype`.

2. ¿Por qué las arrow functions “no tienen `this`”?  
   👉 Porque no crean uno nuevo, heredan el del contexto.

3. ¿Qué devuelve `Perro.__proto__.__proto__`?  
   👉 `Function.prototype`.

---

## 10. Cierre de la clase

- En JS **no hay clases reales**, sino objetos que heredan de otros objetos (modelo basado en prototipos).
- `class` es azúcar sintáctico para simplificar funciones constructoras + prototipos.
- Las instancias heredan por la cadena `__proto__`.
- `this` depende de la forma de declarar (normal vs arrow) y de la forma de invocar.
