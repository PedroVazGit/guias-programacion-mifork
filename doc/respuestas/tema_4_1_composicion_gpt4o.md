<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición

## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta
En C, la composición se realiza incluyendo una estructura como campo dentro de otra. Al no existir funciones dentro de los `struct`, la lógica matemática se separa en funciones globales externas.

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

double distanciaEntrePuntos(Punto a, Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

double longitudLinea(Linea l) {
    return distanciaEntrePuntos(l.p1, l.p2);
}
```

---

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
En Java se encapsulan los datos y los métodos juntos. La inmutabilidad se garantiza declarando los atributos como `private final` y omitiendo los métodos modificadores (`setters`).

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double calcularLongitud() {
        return p1.distanciaA(p2);
    }
}
```

---

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La multiplicidad indica el número de instancias de una clase que pueden relacionarse con una instancia de la otra clase.
* **De `Linea` a `Punto`:** Es exactamente **2** (una línea está compuesta por dos puntos obligatoriamente).
* **De `Punto` a `Linea`:** Es **0..*** (un punto puede formar parte de ninguna, de una o de muchas líneas simultáneamente).

---

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
* **Composición Fuerte (Composición):** El ciclo de vida de los objetos hijos está ligado rígidamente al del contenedor. Si el contenedor se destruye, las partes mueren con él. Las partes pertenecen exclusivamente a un único todo.
* **Composición Débil (Agregación/Asociación):** Las partes tienen vida independiente del contenedor. Pueden existir antes de la creación del contenedor y sobrevivir a su destrucción, pudiendo ser compartidas.

---

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
Hablamos de **dependencia** (relación de uso). A diferencia de la composición, la dependencia es una relación estructural transitoria y no permanente; la clase utilizada no se almacena como un atributo del objeto contenedor.

---

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
```java
// Composición fuerte (La línea crea y es dueña de sus puntos)
public class LineaFuerte {
    private final Punto p1, p2;
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

// Composición débil / Agregación (Los puntos se reciben desde fuera)
public class LineaDebil {
    private final Punto p1, p2;
    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
}
```

---

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
No se observa destrucción explícita porque Java carece de destrucción manual de memoria. El encargado es el **Garbage Collector**. En la composición fuerte, cuando el contenedor (`Linea`) pierde todas sus referencias y queda marcado para su destrucción, los objetos internos (`Punto`) también pierden su única vía de acceso, quedando automáticamente listos para ser reciclados por el recolector.

---

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
```java
public class Departamento {
    private Profesor[] profesores = new Profesor[50];
    private int contador = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Director requerido");
        profesores[0] = directorInicial;
        this.director = directorInicial;
        this.contador = 1;
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException();
        if (contador >= 50) throw new IllegalStateException("Capacidad llena");
        profesores[contador++] = p;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= contador) throw new IndexOutOfBoundsException();
        if (profesores[pos] == director) throw new IllegalStateException("No se puede eliminar al director activo");
        
        for (int i = pos; i < contador - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--contador] = null;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        boolean pertenece = false;
        for (int i = 0; i < contador; i++) {
            if (profesores[i] == nuevoDirector) pertenece = true;
        }
        if (!pertenece) throw new IllegalArgumentException("El director debe pertenecer al departamento");
        this.director = nuevoDirector;
    }

    public int getCantidadProfesores() { return contador; }
    public Profesor getProfesor(int pos) { 
        if (pos < 0 || pos >= contador) throw new IndexOutOfBoundsException();
        return profesores[pos]; 
    }
}
```

---

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
El uso de `List` (como `ArrayList`) ahorra la gestión manual del tamaño del almacenamiento, el control del índice de inserción y el algoritmo para desplazar elementos y rellenar huecos al eliminar.

Si un método devolviera directamente la lista interna, se **rompería la encapsulación**, permitiendo a agentes externos añadir o borrar elementos saltándose las invariantes del departamento (como borrar al director). Se resuelve devolviendo una **copia nueva de la lista** (`new ArrayList<>(this.profesores)`) o exponiéndola como inmutable.

```java
import java.util.ArrayList;
import java.util.List;

public class DepartamentoList {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoList(Profesor directorInicial) {
        profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public void añadirProfesor(Profesor p) { profesores.add(p); }

    public void eliminarProfesor(int pos) {
        if (profesores.get(pos) == director) throw new IllegalStateException("Es el director");
        profesores.remove(pos);
    }

    public List<Profesor> obtenerTodosLosProfesores() {
        return new ArrayList<>(this.profesores); // Protección por copia
    }
}
```

---

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo inmutable de una `Persona` que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
La composición recursiva se da cuando los atributos de una clase hacen referencia a objetos de su misma clase. Otros ejemplos típicos de este concepto son los sistemas de archivos de carpetas (una carpeta contiene otras carpetas), los componentes de menús visuales jerárquicos o las estructuras de datos dinámicas como los nodos en una lista enlazada.

```java
public class Persona {
    private final String nombre;
    private final Persona madre; // Atributo recursivo

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre = new Persona("Elena", abuela);
        Persona nieto = new Persona("Carlos", madre);
        
        System.out.println("Nieto instanciado en un modelo familiar.");
    }
}
```

---

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
Las relaciones bidireccionales permiten que ambos extremos de la relación guarden una referencia mutua del otro, facilitando la navegación en los dos sentidos (el departamento conoce a sus profesores y el profesor conoce su departamento asignado).

Para implementarlo, se añade un atributo de tipo `Departamento` dentro de la clase `Profesor`. Además, se debe programar lógica de sincronización rigurosa en el método `añadirProfesor` de `Departamento` para que, al incorporar al docente a la lista, se invoque un método interno en el profesor que actualice su referencia hacia el departamento actual (`this`).

```java
public class Profesor {
    private Departamento departamento;
    public void setDepartamento(Departamento d) { this.departamento = d; }
}

// Dentro de Departamento:
public void añadirProfesor(Profesor p) {
    profesores.add(p);
    p.setDepartamento(this); // Establece el enlace inverso
}
```