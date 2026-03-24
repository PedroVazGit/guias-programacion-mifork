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

En el lenguaje C, la composición se manifiesta mediante la inclusión de una estructura (`struct`) como un campo o miembro de otra estructura superior. Esta relación define técnicamente que una entidad compleja está físicamente formada por partes más simples, permitiendo agrupar datos relacionados bajo un mismo nombre.

Para operar con estos datos, se definen funciones externas que reciben las estructuras como argumentos. Al no existir el concepto de "método" vinculado a los datos en C, la lógica (como el cálculo de distancias) se mantiene separada de la declaración de las estructuras, accediendo directamente a los miembros internos de cada componente.



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

La composición en Java permite que una clase contenga referencias a objetos de otras clases como atributos privados. A diferencia de C, la Orientación a Objetos permite encapsular tanto los datos como el comportamiento (métodos) dentro de la misma unidad, logrando que el objeto `Linea` utilice los servicios del objeto `Punto` para realizar sus cálculos internos.

Para superar la seguridad de C y garantizar la inmutabilidad, se utilizan los modificadores `private` y `final`. Al declarar los atributos como `final` y no proporcionar métodos "setter", se asegura que una vez que una `Linea` o un `Punto` han sido creados, su estado no pueda ser alterado, protegiendo la integridad de la estructura frente a accesos externos no controlados.

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

    public double obtenerLongitud() {
        return p1.distanciaA(p2);
    }
}
```

---


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad representa el número de instancias de una clase que pueden estar asociadas a una instancia de otra clase en un momento dado. Es una restricción de diseño que define los límites inferiores y superiores de la relación, indicando cuántos objetos "parte" pertenecen al objeto "todo".



En el ejemplo de la línea, la multiplicidad de **Linea hacia Punto es exactamente 2**, ya que por definición geométrica una línea requiere dos extremos. En sentido inverso, la multiplicidad de **Punto hacia Linea es 0..***, puesto que un punto puede existir de forma independiente sin pertenecer a ninguna línea, o bien ser el vértice compartido de múltiples líneas.

---


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición **fuerte** implica una dependencia total en el ciclo de vida: las partes son creadas y destruidas por el contenedor, y no tienen sentido fuera de él. Por el contrario, la composición **débil** permite que las partes existan antes que el contenedor y permanezcan activas después de que este desaparezca, permitiendo que dichas partes sean compartidas entre diferentes objetos.



En el análisis de software, se utiliza el término **"composición"** estrictamente para la variante fuerte (relación "todo-parte" exclusiva). La variante débil se denomina habitualmente **"agregación"** o asociación, reflejando que la relación es de pertenencia temporal o de simple conocimiento entre objetos independientes.

---


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Se habla de **dependencia** cuando una clase utiliza a otra de forma efímera o transitoria, pero no la mantiene como parte de su estado permanente. Esto ocurre habitualmente cuando se recibe un objeto como parámetro en un método o se crea una variable local que se destruye al finalizar la ejecución del bloque de código.

A diferencia de la composición, donde la relación es estructural y persistente (un atributo de clase), la dependencia es una relación de uso puntual. Si la clase "A" depende de "B", un cambio en la interfaz de "B" podría obligar a modificar "A", pero "A" no es dueña ni responsable del ciclo de vida de "B".

---


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

En la composición fuerte, la responsabilidad de crear los objetos internos recae exclusivamente en la clase contenedora, generalmente dentro de su constructor. Esto garantiza que los componentes sean privados y exclusivos de esa instancia, naciendo y muriendo con ella.

En la composición débil (agregación), el contenedor recibe las referencias de objetos ya existentes a través del constructor o métodos. Esto permite que el ciclo de vida de los componentes sea gestionado de forma externa, facilitando la reutilización de los mismos objetos en distintas estructuras del programa.

```java
// Composición FUERTE: La línea es dueña total de los puntos
public class LineaFuerte {
    private final Punto p1, p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

// Composición DÉBIL (Agregación): Los puntos existen independientemente
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

En Java, no existe un mecanismo de destrucción explícita de objetos como el comando `free` en C o `delete` en C++. La liberación de memoria es gestionada por el **Garbage Collector** (Recolector de Basura), el cual identifica y elimina objetos que ya no tienen ninguna referencia activa apuntando hacia ellos.



En una composición fuerte, cuando el objeto contenedor (la `Linea`) deja de ser referenciado y es marcado para su eliminación, las referencias internas hacia sus componentes (los `Punto`) también se pierden. Al ser estas las únicas referencias existentes debido al diseño fuerte, el Recolector de Basura destruye automáticamente los puntos junto con la línea.

---


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Se implementa una relación de agregación donde el `Departamento` gestiona un conjunto de profesores y un director. Se aplican invariantes de clase para asegurar que el director siempre sea un miembro activo del departamento, utilizando excepciones para prevenir estados inconsistentes, como eliminar a un profesor que actualmente ejerce la dirección.

La encapsulación se mantiene al no permitir el acceso directo al array interno. Todas las operaciones de inserción, borrado y consulta se realizan a través de métodos controlados que validan la posición y el estado de los objetos, garantizando que el array se mantenga compacto y sin huecos nulos intermedios.

```java
public class Departamento {
    private Profesor[] profesores;
    private int contador;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Se requiere director");
        this.profesores = new Profesor[50];
        this.profesores[0] = directorInicial;
        this.director = directorInicial;
        this.contador = 1;
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException();
        if (contador >= 50) throw new IllegalStateException("Departamento lleno");
        profesores[contador++] = p;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= contador) throw new IndexOutOfBoundsException();
        if (profesores[pos] == director) throw new IllegalStateException("No se puede eliminar al director");
        
        for (int i = pos; i < contador - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--contador] = null;
    }

    public void cambiarDirector(Profesor nuevo) {
        boolean encontrado = false;
        for (int i = 0; i < contador; i++) {
            if (profesores[i] == nuevo) encontrado = true;
        }
        if (!encontrado) throw new IllegalArgumentException("El director debe ser profesor del dpto");
        this.director = nuevo;
    }

    public int getNumProfesores() { return contador; }
    public Profesor getProfesor(int pos) { return profesores[pos]; }
}
```

---


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

El uso de la interfaz `List` simplifica la implementación al eliminar la gestión manual del tamaño del array y los desplazamientos de elementos tras un borrado. Al emplear `ArrayList`, se delega la complejidad técnica a la biblioteca estándar de Java, permitiendo centrar la lógica del programador en las reglas de negocio y las invariantes de la clase.

Si se devolviera la lista interna directamente a través de un método, se incurriría en una vulnerabilidad de diseño, ya que cualquier código externo podría modificar la colección sin pasar por los filtros de seguridad del departamento. La solución consiste en devolver una **copia de la lista** o una vista no modificable, protegiendo así el estado interno de alteraciones externas.

```java
import java.util.ArrayList;
import java.util.List;

public class DepartamentoList {
    private List<Profesor> lista = new ArrayList<>();
    private Profesor director;

    public DepartamentoList(Profesor d) {
        lista.add(d);
        director = d;
    }

    // Devuelve una copia para proteger la encapsulación
    public List<Profesor> getProfesores() {
        return new ArrayList<>(lista);
    }
}
```

---


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

La composición recursiva ocurre cuando una clase contiene una referencia a un objeto de su propio tipo, permitiendo modelar jerarquías naturales o estructuras de datos dinámicas. Este patrón es fundamental para representar relaciones de ascendencia, donde cada entidad tiene un vínculo con otra entidad de la misma naturaleza.

Además del árbol genealógico, existen ejemplos clásicos como las estructuras de directorios en un sistema de archivos (donde una carpeta contiene otras carpetas) o las expresiones matemáticas (donde una operación está compuesta por otras sub-operaciones). En Java, las excepciones encadenadas son un uso común de este concepto para trazar la causa original de un error.

```java
public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public static void main(String[] args) {
        Persona abuela = new Persona("Elena", null);
        Persona madre = new Persona("Rosa", abuela);
        Persona nieto = new Persona("Luis", madre);
        
        System.out.println("El nieto es Luis, su madre es Rosa y su abuela es Elena.");
    }
}
```

---

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las relaciones bidireccionales permiten la navegación en ambos sentidos: el contenedor conoce a sus componentes y cada componente mantiene una referencia hacia su contenedor. Esto es útil cuando el objeto contenido necesita solicitar información o servicios al objeto superior al que pertenece.

Para implementar esto en el ejemplo de profesores, se debe añadir un atributo `Departamento` dentro de la clase `Profesor`. Es imperativo gestionar la sincronización de estas referencias; cuando un profesor se añade a un departamento, el código debe asegurar que el campo "departamento" del profesor se actualice simultáneamente para apuntar al objeto correcto, evitando inconsistencias donde un objeto crea estar vinculado a un ente inexistente.

```java
public class Profesor {
    private String nombre;
    private Departamento dpto;

    public void asignarDepartamento(Departamento d) {
        this.dpto = d;
    }
}

// En la clase Departamento, el método añadir sería:
public void añadirProfesor(Profesor p) {
    profesores.add(p);
    p.asignarDepartamento(this); // Sincronización bidireccional
}
```
---