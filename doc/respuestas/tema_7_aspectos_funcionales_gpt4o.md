<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C...

### Respuesta
Un **puntero a función** es una variable que almacena la dirección de memoria donde reside el código ejecutable de una función. Permite invocar funciones de forma dinámica o pasarlas como argumentos a otras funciones.

```c
#include <stdio.h>
#include <ctype.h>

char* convertirAMayusculas(char* str) {
    for (int i = 0; str[i]; i++) str[i] = toupper(str[i]);
    return str;
}

int main() {
    char cad[] = "hola";
    char* (*aMayusculas)(char*) = &convertirAMayusculas; // Puntero a función
    printf("%s", aMayusculas(cad));
    return 0;
}
```

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java...

### Respuesta
Una **función lambda** es una función anónima (sin nombre) que se define en el lugar donde se necesita. Se trata como un dato más, permitiendo asignarla a variables o pasarla por parámetro.

```javascript
// JavaScript
let aMayusculas = (str) => str.toUpperCase();
console.log(aMayusculas("hola"));
```

```java
// Java
Function<String, String> aMayusculas = (str) -> str.toUpperCase();
System.out.println(aMayusculas.apply("hola"));
```

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El **paradigma funcional** se basa en tratar el cómputo como la evaluación de funciones matemáticas, evitando el cambio de estado y los datos mutables. Java 8 es **multi-paradigma** porque integra capacidades funcionales sobre su base orientada a objetos. Que las funciones sean **"ciudadanos de primera clase"** significa que pueden ser asignadas a variables, pasadas como parámetros y devueltas por otras funciones, igual que cualquier objeto o tipo primitivo.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis consta de tres partes: `(parámetros) -> { cuerpo }`.
* **Parámetros:** Si es uno solo, los paréntesis son opcionales. Si no hay parámetros, se usan `()`.
* **Flecha:** El operador `->` separa los parámetros del cuerpo.
* **Cuerpo:** Si es una sola línea, no requiere llaves `{}` ni la palabra `return`.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores...

### Respuesta
Este patrón permite que el comportamiento del método `transformar` sea dinámico según la función que reciba.

```java
// Java
public static String transformar(String s, Function<String, String> f) {
    return f.apply(s);
}
// Uso: transformar("hola", aMayusculas);
```

```javascript
// JavaScript
const transformar = (s, f) => f(s);
// Uso: transformar("hola", aMayusculas);
```

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada...

### Respuesta
Se define la lógica "al vuelo" sin necesidad de crear una variable previa.

```java
String resultado = transformar("hola", s -> new StringBuilder(s).reverse().toString());
```

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local...

### Respuesta
Un **cierre (closure)** es la capacidad de una función lambda de "capturar" y recordar el entorno (variables locales) donde fue creada. En Java, la variable capturada debe ser **final** o **efectivamente final** (no cambiar su valor tras la asignación).

```java
String prefijo = "LOG: ";
Function<String, String> concatenar = s -> prefijo + s; // Captura 'prefijo'
System.out.println(transformar("mensaje", concatenar));
```

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La diferencia principal es que la lambda es un **objeto** con estado capturado (closure), mientras que el puntero a función en C es solo una **dirección de memoria**. La lambda "viaja" con los datos de su contexto, lo que permite una programación mucho más expresiva y segura que el simple salto de ejecución de C.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento"...

### Respuesta
Aquí la función `crearDescuento` es una función de orden superior porque devuelve otra función. La **closure** se produce porque la lambda devuelta "recuerda" el valor de `porcentaje` aunque la función original ya haya terminado de ejecutarse.

```java
public static Function<Double, Double> crearDescuento(double porcentaje) {
    return (precio) -> precio - (precio * porcentaje / 100);
}

// Uso
Function<Double, Double> desc10 = crearDescuento(10);
System.out.println(desc10.apply(100.0)); // 90.0
```

## 10. En Java... ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una **interfaz funcional** es una interfaz que tiene **exactamente un método abstracto**. Es el tipo que Java asigna a las expresiones lambda. Puede tener otros métodos (estáticos o por defecto), pero solo uno pendiente de implementar. Se suele marcar con `@FunctionalInterface`.

## 11. Creemos una interfaz funcional a mano...

### Respuesta
```java
@FunctionalInterface
public interface Transformador {
    String transformar(String s);
}
```

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics...

### Respuesta
```java
@FunctionalInterface
public interface TransformadorGenerico<T, R> {
    R transformar(T t);
}

// Ejemplo: Double a Integer
TransformadorGenerico<Double, Integer> redondeo = d -> (int) Math.round(d);
```

## 13. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Java ofrece un catálogo estándar en `java.util.function`:
* **`Function<T, R>`**: Recibe T, devuelve R.
* **`Predicate<T>`**: Recibe T, devuelve boolean.
* **`Consumer<T>`**: Recibe T, no devuelve nada (void).
* **`Supplier<T>`**: No recibe nada, devuelve T.
* **`BiFunction<T, U, R>`**: Recibe dos parámetros, devuelve R.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`...

### Respuesta
```java
List<Integer> lista = Arrays.asList(-1, 5, -2, 10);
lista.forEach(n -> {
    if (n > 0) System.out.println(n);
});
```

## 15. Repasando el tema de genericidad... ¿qué significa **PECS**, y explícalo para el caso de mejorar el método `transformar`?

### Respuesta
**PECS** significa *Producer Extends, Consumer Super*. 
* Se usa `? extends T` cuando la estructura **produce** (leemos de ella).
* Se usa `? super T` cuando la estructura **consume** (escribimos en ella).
En `transformar(T t, Function<? super T, ? extends R> f)`, usamos `? super T` porque la función va a "consumir" el objeto de tipo T, permitiendo así aceptar funciones que operen sobre tipos más generales (superclases).

## 16. Referencias a métodos... Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`...

### Respuesta
La referencia a método permite tratar un método existente como una lambda.

```java
// Java
Persona p = new Persona("Juan");
Runnable saludo = p::saludar; // Referencia a método de instancia
saludo.run();
```

```javascript
// JavaScript
const p = { nombre: "Juan", saludar: function() { console.log(this.nombre); } };
const saludo = p.saludar.bind(p); // En JS hay que vincular el contexto
saludo();
```

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java?

### Respuesta
1. **Estático:** `Clase::metodoEstatico` (ej. `Math::abs`).
2. **Constructor:** `Clase::new` (ej. `ArrayList::new`).
3. **Instancia de objeto concreto:** `miObjeto::metodo` (ej. `p::saludar`).
4. **Instancia de tipo arbitrario:** `Clase::metodo` (ej. `String::toUpperCase`).

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`...

### Respuesta
```java
// Versión Manual
Collections.sort(personas, (p1, p2) -> {
    int res = Integer.compare(p1.getEdad(), p2.getEdad());
    if (res == 0) return p1.getNombre().compareTo(p2.getNombre());
    return res;
});

// Versión con Comparator (más funcional)
personas.sort(Comparator.comparingInt(Persona::getEdad)
                        .thenComparing(Persona::getNombre));
```
