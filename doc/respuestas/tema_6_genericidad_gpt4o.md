<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
Tanto `void*` como `Object` funcionan como punteros o referencias universales. Al ser `Object` la raíz de la jerarquía en Java, un array de este tipo puede apuntar a cualquier instancia.

```java
public class ContenedorUniversal {
    private Object[] elementos = new Object[10];
    private int contador = 0;

    public void añadir(Object o) { elementos[contador++] = o; }
    public Object obtener(int i) { return elementos[i]; }
}
```

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

### Respuesta
La **programación genérica** es un estilo de programación que permite escribir algoritmos y estructuras de datos de forma abstracta, sin especificar los tipos de datos exactos hasta el momento de su uso. El ejemplo anterior es un ejemplo básico y "primitivo" de genericidad basado en el **polimorfismo de la clase base**, pero carece de seguridad de tipos.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas.

### Respuesta
1. **Falta de seguridad en tiempo de compilación:** El compilador no puede impedir que mezcles peras con manzanas en el mismo contenedor.
2. **Necesidad de Casting:** Al recuperar un dato, este viene como `Object`, lo que obliga a realizar un *downcasting* manual, aumentando el riesgo de errores (`ClassCastException`) en tiempo de ejecución.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**?

### Respuesta
Los **parámetros de tipo** son etiquetas (comúnmente letras como `<T>`, `<E>`, `<K>`) que actúan como "huecos" o variables de tipo. Permiten que el programador indique al compilador qué tipo concreto se va a usar en esa instancia específica, permitiendo que el compilador realice el chequeo de tipos por nosotros.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos...

### Respuesta
En ambos casos, el compilador garantiza que solo se inserten `String` y que no haga falta casting al extraerlos.

```cpp
// C++ Templates
std::vector<std::string> lista;
lista.push_back("Hola");
std::string s = lista[0]; // Seguridad total
```

```java
// Java Generics
List<String> lista = new ArrayList<>();
lista.add("Hola");
String s = lista.get(0); // Sin necesidad de casting
```

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
No hacen lo mismo.
* **Java (Type Erasure):** El compilador elimina los tipos genéricos tras comprobar la seguridad y los sustituye por `Object` (o su límite). Existe una sola clase en el ejecutable.
* **C++ (Instanciación de plantillas):** El compilador genera un código fuente distinto para cada tipo utilizado (ej. crea una clase para `vector<int>` y otra para `vector<string>`).

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes...

### Respuesta
La clase `Par` permite devolver dos valores de distinto tipo como una única unidad.

```java
public class Par<T, U> {
    private T primero;
    private U segundo;
    public Par(T p, U s) { primero = p; segundo = s; }
    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

// Ejemplo: devolver media (Double) y desviación (Double)
public Par<Double, Double> estadisticas(double[] datos) {
    // ... cálculos ...
    return new Par<>(8.5, 1.2);
}
```

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método... Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo...

### Respuesta
Al usar parámetros de tipo `<T>`, el compilador obliga a que ambos argumentos sean del mismo tipo y garantiza que el retorno sea de ese mismo tipo `T`, eliminando la necesidad de casting externo.

```java
public <T> T seleccionaUno(T a, T b) {
    return Math.random() > 0.5 ? a : b;
}
// seleccionaUno("Hola", 5); // ERROR de compilación (tipos distintos)
String s = seleccionaUno("A", "B"); // OK y sin casting
```

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? ... Pon un ejemplo en Java de un `Punto` con dos coordenadas...

### Respuesta
Sí, se usa la cláusula `extends` (Bounded Type Parameters).
* **Solución sin generics:** Usa `Number` directamente; permite mezclar tipos de número.
* **Solución con generics:** Usa `<T extends Number>`; tras la compilación (type erasure), el tipo `T` se convierte en `Number`.

```java
public class Punto<T extends Number> {
    private T x, y;
    public Punto(T x, T y) { this.x = x; this.y = y; }
    public T getX() { return x; }
    
    public double distanciaA(Punto<T> otro) {
        return Math.sqrt(Math.pow(x.doubleValue() - otro.x.doubleValue(), 2) + ...);
    }
}
```

## 10. Sobre las soluciones anteriores... ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real?

### Respuesta
* **Sin generics (`Number x, y`):** SÍ permite mezclar (ej. un `int` y un `double`), pero `getX()` devuelve siempre `Number`.
* **Con generics (`T x, y`):** NO permite mezclar si se usa el mismo parámetro `T` para ambas; obliga a que ambas coordenadas sean del mismo tipo específico (ej. ambas `Integer`). `getX()` devuelve el tipo exacto `T`.

## 11. Hagamos un ejemplo avanzado... Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo...

### Respuesta
Al parametrizar la interfaz, forzamos a que el método `distanciaA` reciba exactamente el tipo de la clase que lo implementa, eliminando la necesidad de `instanceof`.

```java
public interface Punto<T> { 
    public double distanciaA(T p); 
} 

public class Punto2D implements Punto<Punto2D> { 
    private double x, y;
    @Override 
    public double distanciaA(Punto2D p2d) { // Firma exacta, sin casting
        return Math.sqrt(Math.pow(x - p2d.x, 2) + Math.pow(y - p2d.y, 2));
    } 
}
```

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? ...

### Respuesta
* **Arrays:** `String[]` ES subtipo de `Object[]` (**Covariantes**). Problema: Puedes meter un `Integer` en un `Object[]` que realmente es de `String`, provocando un `ArrayStoreException` en ejecución.
* **Generics:** `List<String>` NO es subtipo de `List<Object>` (**Invariantes**). El compilador lo prohíbe para evitar errores en ejecución.
* **Conceptos:**
    * **Covariante:** Mantiene la relación de herencia.
    * **Contravariante:** Invierte la relación de herencia.
    * **Invariante:** No existe relación de herencia entre los contenedores.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)?

### Respuesta
El **wildcard** representa un tipo desconocido.
* **`? extends T` (Covarianza):** Permite **leer** objetos de tipo `T` o subclases (Uso: Productores).
* **`? super T` (Contravarianza):** Permite **escribir** objetos de tipo `T` o subclases (Uso: Consumidores).

```java
// (i) Sumar: solo lectura (? extends Number)
public double sumar(List<? extends Number> lista) {
    double s = 0;
    for(Number n : lista) s += n.doubleValue();
    return s;
}

// (ii) Añadir: solo escritura (? super Integer)
public void añadirEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
}
```
