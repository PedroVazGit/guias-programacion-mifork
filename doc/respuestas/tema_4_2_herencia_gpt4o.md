<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
La herencia es un mecanismo que permite definir una clase nueva a partir de una existente. La relación **"A es-un B"** indica que la subclase es una especialización de la superclase.
1. **Compatibilidad de tipos:** Un objeto de la subclase puede ser tratado como si fuera del tipo de la superclase (polimorfismo).
2. **Herencia de estado y comportamiento:** El hijo adquiere los atributos y métodos definidos en el padre.

```java
public class Soldado {
    private String nombre;
    public Soldado(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Soy el soldado " + nombre); }
}

public class Artillero extends Soldado {
    private int cohetes;
    public Artillero(String nombre, int c) { super(nombre); this.cohetes = c; }
    public int getCohetes() { return cohetes; }
}

public class Zapador extends Soldado {
    private int minas;
    public Zapador(String nombre, int m) { super(nombre); this.minas = m; }
    public int getMinas() { return minas; }
}

// Ejemplo de compatibilidad de tipos
Soldado[] ejercito = { new Artillero("Rambo", 10), new Zapador("Explosivo", 5) };
for (Soldado s : ejercito) {
    s.saludar();
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Se ejecutan **dos constructores** por cada objeto: primero el de la superclase (`Soldado`) y luego el de la subclase (`Artillero` o `Zapador`). `super` referencia a la clase padre; dentro de un constructor, se usa para invocar al constructor del padre. Si la clase base no tiene un constructor sin parámetros, es **obligatorio** llamar a `super(...)` explícitamente en la primera línea del constructor hijo.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
**Sí**, los atributos privados (como `nombre`) forman parte física del objeto en memoria. Sin embargo, **no pueden usarse directamente** desde el código de la subclase debido a la encapsulación (`private`). Para acceder a ellos, la subclase debe emplear métodos públicos o protegidos definidos en la superclase.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
Implica que el sistema es **fácilmente ampliable**: se pueden añadir nuevos tipos de soldados sin necesidad de modificar o recompilar el código que ya gestiona la lógica general de los soldados.

```java
public class Medico extends Soldado {
    public Medico(String nombre) { super(nombre); }
}
// El bucle original 'for (Soldado s : ejercito) s.saludar();' sigue funcionando intacto con el Medico.
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
Es posible tener la referencia, pero no se pueden invocar métodos específicos del subtipo a través de ella. 
* **Upcasting:** Ver un objeto hijo como padre (automático).
* **Downcasting:** Convertir una referencia de padre a hijo (explícito).
* **instanceof:** Operador que verifica el tipo real del objeto en ejecución.

```java
for (Soldado s : ejercito) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso **protegido** (`protected`) permite que los miembros sean visibles para la propia clase, sus subclases y otras clases del mismo paquete. En Java se implementa con la palabra clave `protected`.

```java
public class Soldado { protected String nombre; }

public class Zapador extends Soldado {
    public void ponerMinas() {
        System.out.println(nombre + " está colocando minas."); // Acceso directo por ser protected
    }
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
No ocurre en todos los lenguajes (C++ es un ejemplo donde no hay raíz única). En Java, **sí existe**: la clase **`Object`** es la superclase universal de la que heredan todas las clases de forma automática.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
Es la capacidad de una clase de heredar de más de una superclase. **En Java no existe herencia múltiple de clases** para evitar conflictos de ambigüedad; en su lugar, se permite la implementación de múltiples interfaces.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
Se crea extendiendo de `RuntimeException` para que sea no controlada (unchecked).

```java
public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;
    
    public UsuarioNoEncontradoException(Usuario u) { this.usuario = u; }
    
    public UsuarioNoEncontradoException(Usuario u, Throwable causa) { 
        super(causa);
        this.usuario = u; 
    }
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
Porque la herencia crea un **acoplamiento muy fuerte** y rígido. Si se usa herencia solo para "copiar" funciones de otra clase sin que exista una relación real de "es-un", se ensucia el modelo lógico y se dificulta el mantenimiento futuro.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Porque la composición es mucho más **flexible** y dinámica. Permite cambiar el comportamiento en tiempo de ejecución (intercambiando los objetos contenidos) y evita las jerarquías de clases excesivamente complejas y profundas.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Se refiere a que la subclase depende de la implementación interna de la superclase. Un cambio en el funcionamiento interno del padre puede provocar fallos inesperados en el hijo, incluso si la interfaz pública no ha cambiado.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
Se comparan la jerarquía vertical (herencia) frente a la asociación horizontal (composición).

```java
// Opción A: Herencia
public class Persona { String dni, nombre; }
public class Estudiante extends Persona { }

// Opción B: Composición
public class DatosPersonales { String dni, nombre; }
public class Estudiante {
    private DatosPersonales datos;
    public Estudiante(DatosPersonales dp) { this.datos = dp; }
}
```
