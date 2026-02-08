<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

Buscan agrupar datos y comportamiento en una misma entidad (**clase**) y proteger el estado interno para que no sea manipulado incorrectamente desde fuera.
**Ventajas:**
* **Mantenibilidad:** Puedes cambiar el código interno sin romper el de quien usa tu clase.
* **Integridad:** Evitas que los objetos tengan estados inválidos (ej. una fecha con mes 13).
* **Abstracción:** Ocultas la complejidad, ofreciendo solo lo necesario para usar el objeto.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La **interfaz pública** son los métodos y atributos declarados como `public` que la clase expone al mundo exterior para interactuar con ella. Se relaciona con la ocultación porque actúa como una "barrera" o ventanilla única: todo lo que no está en la interfaz pública debe estar oculto (`private`), separando el **qué** hace la clase del **cómo** lo hace internamente.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

Porque la interfaz pública es un **contrato** con otros programadores (o contigo mismo en el futuro). Si cambias un método público (nombre, parámetros), rompes el código de todas las partes del programa que lo usan. Por eso es **difícil cambiarla** una vez publicada; es mejor exponer lo mínimo posible al principio.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las **invariantes** son reglas lógicas que siempre deben ser verdaderas para que un objeto sea válido (ej: en una cuenta bancaria, `saldo + crédito >= 0`). La ocultación ayuda porque, al hacer los atributos `private`, obligas a pasar por métodos (setters) donde puedes programar chequeos para asegurar que estas reglas nunca se rompan.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

```java
public class Punto {
    private double x, y; // Ocultación de información

    public Punto(double x, double y) { // Constructor
        this.x = x; this.y = y;
    }

    public double calcularDistanciaAOrigen() { // Interfaz pública
        return Math.sqrt(x*x + y*y);
    }
}
```
La interfaz pública es el método *calcularDistanciaAOrigen*. Por otra parte, *public* significa accesible desde cualquier parte del programa, y *private* significa accesible solo desde dentro de la propia clase Punto.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

Se pueden aplicar a la definición de la **clase** (aunque una clase principal no suele ser privada), a los **atributos** (variables miembro), a los **métodos** y a los **constructores**.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Sí. En Java existen también `protected` (visible en el mismo paquete y en subclases) y la visibilidad **por defecto** o *package-private* (si no escribes nada, solo es visible dentro del mismo paquete). En C++, por ejemplo, existe el concepto de `friend` (clases amigas) que permite saltarse la privacidad, algo que Java no permite para ser más estricto con la encapsulación.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

Están ocultos para **(a) otras clases**. Entre instancias de la misma clase **SÍ** se ven los privados. La privacidad es a nivel de *clase*, no de objeto.
```java
public double calcularDistanciaAPunto(Punto otro) {
    // Podemos acceder a 'otro.x' aunque sea private porque
    // 'this' y 'otro' pertenecen a la misma clase 'Punto'.
    return Math.sqrt(Math.pow(this.x - otro.x, 2) + Math.pow(this.y - otro.y,2));
}
```
**Explicación:** La privacidad en Java es a nivel de **clase** (el "molde"), no a nivel de objeto individual. Por eso, un objeto `Punto` puede tocar las variables privadas de otro objeto `Punto`.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Son métodos públicos estandarizados para leer (`get`) o modificar (`set`) atributos privados desde fuera de la clase.

* **Getter (Accesor):** Devuelve el valor del atributo (o una copia segura) para que otros objetos lo lean.
* **Setter (Mutador):** Asigna un nuevo valor al atributo. Es fundamental porque permite incluir lógica de **validación** antes de guardar el dato (ej: impedir que una edad sea negativa), protegiendo así la integridad del objeto.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, no se refiere a seguridad informática (ciberseguridad). Se refiere a **robustez** y **seguridad contra errores**. Evita que el programa falle o se comporte de forma impredecible porque un objeto haya quedado en un estado inconsistente debido a una modificación externa incorrecta.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

El miembro de instancia pertenece a cada objeto individual (cada `Punto` tiene su propia coordenada `x`), mientras que el miembro de clase (marcado con `static`) es único y compartido por todos los objetos de esa clase. **Sí**, los miembros estáticos también pueden ser `private` (por ejemplo, constantes internas o contadores ocultos).

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene mucho sentido en dos casos principales:
1.  En **clases de utilidad** que no se deben instanciar nunca (como `java.lang.Math`, que es todo estático).
2.  Para implementar el patrón **Singleton**, asegurando que solo pueda existir una única instancia de esa clase en todo el programa controlada desde dentro.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

Se indican con la palabra clave `static`.

```java
public class Punto {
    private double x, y;
    // Miembros de clase (static): compartidos por todos los puntos
    private static double maxX = Double.MIN_VALUE;
    private static double maxY = Double.MIN_VALUE;

    public Punto(double x, double y) {
        this.x = x; this.y = y;
        // Actualizamos los máximos históricos al crear cada punto
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}
```

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

```java
public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
```
**Sí, he usado `static`.** Es obligatorio porque, al ser una factoría, necesitamos llamar a este método **antes** de tener el objeto creado. Se invoca sobre la clase directamente: `Punto.crearPuntoRedondeado(...)`.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

```java
public class Punto {
    // Cambio radical en la estructura interna (encapsulación)
    private double[] coords = new double[2]; 

    public Punto(double x, double y) {
        coords[0] = x; coords[1] = y;
    }

    // La interfaz pública (getters) NO cambia, el usuario no nota nada
    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }
}
```

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

**No es mejor declararlo público.** La convención es **siempre atributos privados**. Si los haces públicos pierdes el control para siempre: no puedes añadir validaciones más tarde sin romper código externo. Las invariantes dependen totalmente de que uses *setters* para validar los datos antes de que entren en el objeto.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase es **inmutable** si su estado no puede cambiar una vez creado el objeto (no tiene *setters* ni métodos que alteren sus atributos). Un método modificador es cualquiera que cambie el estado (no solo *setters*, también podría ser `lista.add()`).

**Ventajas:** Son *Thread-safe* (seguros en concurrencia), fáciles de cachear y no dan sorpresas al pasarlos como parámetros.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

**No.** Solo debes incluirlos si el objeto necesita cambiar de estado lógicamente. Si puedes diseñar la clase para que sea inmutable (pasando todos los datos necesarios por el constructor), el código resultante es mucho más robusto y menos propenso a errores.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

`String` es **inmutable**. Al concatenar (`"a" + "b"`), Java crea un **nuevo objeto** en memoria con el resultado, dejando los originales intactos. Si concatenas muchas veces (ej: en un bucle), debes usar **`StringBuilder`**, que es una clase mutable diseñada para esto, evitando llenar la memoria de objetos temporales basura.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

Por defecto, el operador `==` compara por **identidad** (dirección de memoria, si son *el mismo* objeto). El método `equals()` sirve para definir igualdad por **contenido**. Por defecto (si no lo redefines), `equals` hace lo mismo que `==`, pero debemos sobrescribirlo para comparar atributos.
Las cadenas (`String`) **siempre** se deben comparar con `.equals()`.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Son clases que "envuelven" tipos primitivos (ej: `Integer` envuelve a `int`) para tratarlos como objetos. En Java moderno es automático (**autoboxing/unboxing**).
**Ventaja:** Permiten usar primitivos en Colecciones (como `ArrayList<Integer>`) que solo aceptan objetos, no tipos primitivos. No todos los lenguajes los necesitan; en Python o Ruby todo es un objeto desde el principio.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Es un tipo con un conjunto fijo y limitado de constantes con nombre. En Java, un `enum` **Es una clase** completa (puede tener constructores, métodos y atributos).
**Ventaja:** Seguridad de tipos (el compilador garantiza que la variable solo tendrá un valor válido de la lista) y permite centralizar la lógica relacionada con esas constantes dentro del propio enum.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

```java
public enum Mes {
    ENERO(31), FEBRERO(28), MARZO(31), ABRIL(30), MAYO(31), JUNIO(30),
    JULIO(31), AGOSTO(31), SEPTIEMBRE(30), OCTUBRE(31), NOVIEMBRE(30), DICIEMBRE(31);

    private final int dias;

    // Constructor privado (solo Java puede instanciar las constantes del enum)
    private Mes(int dias) { this.dias = dias; }

    public int getDias() { return dias; }
    public int getOrdinal() { return this.ordinal() + 1; } // ordinal() devuelve 0-11

    // Lógica simplificada de estaciones
    public boolean esDeInvierno(boolean norte) {
        return norte ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
                     : (this == JUNIO || this == JULIO || this == AGOSTO);
    }
    
    // Podemos reutilizar la lógica invirtiendo el hemisferio para el verano
    public boolean esDeVerano(boolean norte) { return esDeInvierno(!norte); }
    
    // Se implementarían primavera y otoño de forma similar...
    public boolean esDePrimavera(boolean norte) {
         return norte ? (this == MARZO || this == ABRIL || this == MAYO)
                      : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }
    public boolean esDeOtoño(boolean norte) { return esDePrimavera(!norte); }
}
```