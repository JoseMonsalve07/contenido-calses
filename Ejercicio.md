# 🧩 Reto: Gestor de Biblioteca con Tipos Deducidos

Este reto está diseñado para que combines **lógica de programación** con el uso de **tipos y Utility Types en TypeScript**.  
El objetivo es que deduzcas qué tipos necesitas y qué utilidades (`Pick`, `Omit`, `Partial`, `Record`, etc.) usar.

---

## 🎯 Enunciado

Imagina que estás construyendo un pequeño sistema para una biblioteca digital.  
Debes implementar lo siguiente en **TypeScript**, pero **NO se indica qué utility types usar ni los tipos exactos**: deberás deducirlos.

---

### 1. Tipo genérico de recurso

Define un **tipo genérico** `ItemBiblioteca<T>` que represente un recurso de la biblioteca.  
Debe incluir:

- `id`
- `titulo`
- `disponible`
- `detalles` (que varía según el recurso).

---

### 2. Tipos de detalles

Define dos estructuras:

- `Libro` con propiedades: `autor`, `paginas`.
- `Revista` con propiedades: `edicion`, `tema`.

---

### 3. Función de préstamo

Crea una función `prestamo<T>` que reciba un `ItemBiblioteca<T>` y:

- Si `disponible` es `true`, lo cambie a `false` y devuelva un objeto con todas sus propiedades, pero con `disponible` en `false`.
- Si `disponible` es `false`, devuelva un `string` indicando que no está disponible.

---

### 4. Filtrar disponibles

Crea una función que reciba una lista de items (`ItemBiblioteca<Libro | Revista>[]`) y:

- Devuelva un **arreglo de solo títulos (`string[]`)** de los que están disponibles.

---

### 5. Conteo por categoría (extra 🌟)

Crea un **enum** `Categoria` con valores `Libro` y `Revista`.  
A partir de la lista de items, genera un `Record<Categoria, number>` que lleve el conteo de cuántos recursos de cada tipo hay en la biblioteca.

---
