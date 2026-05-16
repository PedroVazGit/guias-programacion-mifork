<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El **polimorfismo** es la capacidad de una referencia de superclase de adoptar múltiples formas al apuntar a objetos de distintas subclases, permitiendo invocar métodos comunes que se comportarán de forma diferente según el objeto real. Sirve para escribir código genérico y extensible. La **sobreescritura** (*overriding*) es el mecanismo por el cual una subclase proporciona una implementación específica de un método que ya estaba definido en su superclase.

---

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La **ligadura dinámica** es el proceso por el cual el lenguaje decide qué implementación de un método ejecutar en tiempo de ejecución (según el objeto real) y no en tiempo de compilación. Es el motor técnico que hace posible el polimorfismo. 
* En **Java**, es el comportamiento por defecto (todos los métodos no finales tienen ligadura dinámica).
* En **C++**, hay que indicarlo explícitamente usando la palabra clave `virtual`.
* En **Python**, al ser un lenguaje dinámico, todos los métodos usan ligadura dinámica por defecto.



---

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
El siguiente código muestra cómo la referencia de tipo `Soldado` dentro del bucle ejecuta la versión correcta del método según la instancia real del objeto.

```java
public class Soldado {
    public void saludar() { System.out.println("Soldado presentándose."); }
}

public class Zapador extends Soldado {
    @Override
    public void saludar() { System.out.println("Zapador despejando el camino."); }
}

public class Artillero extends Soldado {
    // Hereda saludar() de Soldado
}

// Ejemplo de polimorfismo
Soldado[] peloton = { new Zapador(), new Artillero() };
for (Soldado s : peloton) {
    s.saludar(); // Ejecuta la versión específica de cada objeto
}
```

---

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, es posible reutilizar la lógica de la superclase. Para ello se utiliza la palabra clave **`super`**. Esto permite extender el comportamiento en lugar de sustituirlo por completo.

```java
public class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Invoca el método del padre
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

---

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
* **Restricciones:** Los parámetros deben ser idénticos en tipo y orden. El tipo de retorno debe ser el mismo o un subtipo (retorno covariante).
* **Diferencia:** La sobreescritura ocurre en clases distintas (herencia) con la misma firma. La sobrecarga ocurre en la misma clase con distintos parámetros.
* **`@Override`:** Es una instrucción para el compilador que verifica que realmente estamos sobreescribiendo un método del padre. Evita errores sutiles (como equivocarse en una letra al escribir el nombre del método).

---

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
**Sí**. Como toda clase en Java hereda de la clase `Object`, al redefinir `toString()` o `equals()` estamos sobreescribiendo métodos de la clase raíz. Cuando la API de Java (por ejemplo, `System.out.println`) recibe nuestro objeto y llama a `toString()`, se activa la ligadura dinámica para ejecutar nuestra versión personalizada, lo cual es polimorfismo puro.

---

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
* **Clase abstracta:** Una clase que no se puede instanciar (no se puede hacer `new`) y sirve como molde incompleto para otras.
* **Método abstracto:** Un método declarado (firma) pero sin implementación (cuerpo).
* **Uso de `abstract`:** Debe ponerse tanto en la cabecera de la clase como en la firma de cada método abstracto.

```java
public abstract class Soldado {
    public void saludar() { System.out.println("Soldado listo."); }
    public abstract void atacar(); // Obliga a los hijos a implementarlo
}
```

---

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
* **Clase `final`:** No se puede heredar de ella (prohíbe la extensión).
* **Método `final`:** No se puede sobreescribir en las subclases.
* **Relación:** El uso de `final` **limita o anula el polimorfismo**, ya que impide que las subclases cambien el comportamiento esperado.
* **Ejemplo API:** La clase **`String`** es `final`. No puedes crear una subclase de `String`.

---

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
Una **interfaz** es un contrato que define un conjunto de métodos que una clase debe implementar. Son similares a las clases abstractas pero más puras (tradicionalmente no tenían estado ni código). La gran diferencia es que, mientras que en Java solo puedes heredar de una clase, **una clase puede implementar múltiples interfaces**, lo que permite polimorfismo múltiple.

---

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`... Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es...

### Respuesta
Este diseño muestra cómo el polimorfismo permite que la clase `Linea` funcione con cualquier tipo de punto sin conocer sus dimensiones internas.

```java
public abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

public class Punto2D extends Punto {
    double x, y;
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) throw new IllegalArgumentException();
        Punto2D p = (Punto2D) otro; // Downcasting
        return Math.sqrt(Math.pow(p.x - this.x, 2) + Math.pow(p.y - this.y, 2));
    }
}

public class Linea {
    private Punto p1, p2;
    public Linea(Punto p1, Punto p2) { this.p1 = p1; this.p2 = p2; }
    public double longitud() { return p1.calcularDistanciaA(p2); } // Polimorfismo
}
```

---

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible`...

### Respuesta
La herencia de interfaces ocurre cuando una interfaz extiende a otra mediante la palabra clave `extends`. A diferencia de las clases, **sí existe la herencia múltiple de interfaces** (una interfaz puede extender varias a la vez).

```java
public interface Fichero {
    String leer();
}

public interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}
```
