# 🏫 Clase: Interfaces en TypeScript

## 1. Introducción: ¿Qué es una interface?
- Una **interface** es un contrato que define la forma (shape) que debe tener un objeto, clase o función.  
- No genera código en tiempo de ejecución, solo existe en compilación para validar tipos.  

```ts
interface Persona {
  nombre: string;
  edad: number;
}

const juan: Persona = {
  nombre: "Juan",
  edad: 30
};
```

---

## 2. Diferencias entre `type` e `interface`
1. **Declaración vs Composición**  
   - `type` puede crear alias de tipos, unir tipos, literales, unir primitivos.  
   - `interface` solo describe **la forma de objetos, clases o funciones**.  

2. **Extensión**  
   - `interface` usa `extends` para heredar.  
   - `type` usa intersecciones (`&`).  

3. **Merging**  
   - **Interfaces se pueden reabrir y extender automáticamente.**  
   - Los `type` son inmutables: no se pueden redefinir.  

```ts
interface Animal {
  especie: string;
}
interface Animal {
  edad: number;
}

const perro: Animal = {
  especie: "Canino",
  edad: 5
};
```

---

## 3. Uso con clases
Las interfaces sirven como contratos que las clases deben implementar.  

```ts
interface Volador {
  volar(): void;
}

class Pajaro implements Volador {
  volar() {
    console.log("El pájaro vuela 🐦");
  }
}
```

---

## 4. Interfaces anidadas y composición
```ts
interface Direccion {
  calle: string;
  ciudad: string;
}

interface Persona {
  nombre: string;
  direccion: Direccion;
}

const maria: Persona = {
  nombre: "María",
  direccion: { calle: "Principal", ciudad: "Madrid" }
};
```

---

## 5. Propiedades opcionales y de solo lectura
```ts
interface Usuario {
  readonly id: number;
  nombre: string;
  edad?: number; // opcional
}

const u: Usuario = { id: 1, nombre: "Ana" };
// u.id = 2; ❌ Error porque es readonly
```

---

## 6. Firmas de índice (index signatures)
Permiten describir objetos dinámicos.  

```ts
interface Contador {
  [letra: string]: number;
}

const letras: Contador = {
  a: 5,
  b: 3,
  z: 1
};
```

---

## 7. Interfaces para funciones
Sí, una interfaz también puede describir funciones:  

```ts
interface Suma {
  (a: number, b: number): number;
}

const sumar: Suma = (x, y) => x + y;
```

---

## 8. Extensión múltiple
```ts
interface A { a: string; }
interface B { b: string; }

interface C extends A, B {
  c: string;
}

const obj: C = { a: "uno", b: "dos", c: "tres" };
```

---

## 9. Diferencias prácticas con `type`
- Usa **`type`** cuando: quieras combinaciones, uniones, tipos literales, primitivos, utilitarios.  
- Usa **`interface`** cuando: describas **formas de objetos**, contratos de clases, o quieras **declaración incremental (merging)**.  

---

## 10. Ejercicio de práctica
👉 Define una interfaz `Vehiculo` con `marca` y `encender()`.  
👉 Extiende a `Coche` con `puertas`.  
👉 Crea una clase que implemente `Coche`.  
👉 Haz una función que reciba un array de `Vehiculo` y los encienda.  
