<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Son **Abstracción** (simplificar la realidad quedándote con lo importante), **Encapsulamiento** (proteger los datos dentro del objeto), **Herencia** (crear nuevas clases a partir de otras existentes reutilizando código) y **Polimorfismo** (capacidad de un objeto de comportarse de diferentes formas según el contexto).

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

**Java**, **C++**, **Python** y **C#** (lenguaje de Microsoft muy similar a Java). 

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La programación **estructurada** organiza el código con flujo lógico (bucles, condicionales, ... ) evitando el caos de los "GOTO" (Saltos arbitrarios). La **modular** va un paso más allá, dividiendo el programa en partes pequeñas e independientes (módulos o funciones), para hacerlo más manejable, similar a cuando separas código en archivos `.h` y `.cpp` en C++.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto se define por su **Identidad** (lo que lo distingue de los demás, su dirección en memoria), su **Estado** (los valores de sus atributos en un momento dado) y su **Comportamiento** (lo que puede hacer a través de sus métodos).


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

La **clase** es la plantilla o el código base, mientras que el **objeto** (o instancia) es esa plantilla hecha realidad en la memoria. No son lo mismo: la clase es el concepto abstracto y la instancia es lo tangible. No todos los lenguajes usan clases; JavaScript, por ejemplo, usa prototipos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

Los objetos suelen almacenarse en el **Heap** (memoria dinámica). En Java, la **recolección de basura** (*Garbage Collector*) es un proceso automático que libera la memoria de los objetos que ya no se usan, a diferencia de C++ donde tendrías que hacer un `delete` manual.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un método es simplemente una función que pertenece a una clase y define el comportamiento de los objetos. La **sobrecarga** consiste en tener varios métodos con el mismo nombre pero diferente lista de parámetros (tipos o cantidad), permitiendo realizar la misma acción con distintos datos.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

```java
class Punto {
    //Estado, es decir, atributos
	int x;
	int y;

	//Comportamiento, es decir, métodos...
	double calcularDistanciaAOrigen() {
		return Math.sqrt(x*x + y*y);
	}

}


public class Pregunta8 {
	public static void main (String args []) {
		Punto miPunto = new Punto();
		miPunto.x = 5;
		miPunto.y = 6;
		double distancia = miPunto.calcularDistanciaAOrigen();		
	}
}

```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

El punto de entrada es `public static void main(String[] args)`. `static` indica que el miembro pertenece a la clase y no a una instancia (se puede usar sin hacer `new`). No es exclusivo de `main`; se usa para variables o métodos compartidos. Combinado con `final`, sirve para definir **constantes** que no cambian.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Compilas con `javac Archivo.java` (genera el `.class`) y ejecutas con `java Archivo`. El **byte-code** es el código intermedio universal que genera el compilador. Los ficheros `.class` contienen ese byte-code, que no es código máquina directo, sino instrucciones para la JVM.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

`new` es el operador que reserva memoria para el objeto en el **Heap**. El **constructor** es un método especial que se ejecuta al instanciar la clase para inicializar sus atributos. Ejemplo:

```java
Empleado(String d, String n, String a) {
    dni = d; 
    nombre = n; 
    apellidos = a;
}
```

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

`this` es una referencia al propio objeto que está ejecutando el código (similar al puntero `this` de C++ o `self` en Python). En Punto es útil para evitar ambigüedades:

```java
class Punto {
    //Atributos
	int x;
	int y;

	//Constructor
	Punto(int x, int y){
		this.x = x;
		this.y = y;
	}

	//Métodos
	double calcularDistanciaAOrigen() {
		return Math.sqrt(x*x + y*y);
	}

}
```

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

```java
class Punto {
    //Atributos
	int x;
	int y;

	//Constructor
	Punto(int x, int y){
		this.x = x;
		this.y = y;
	}

	//Métodos
	double calcularDistanciaAOrigen() {
		return Math.sqrt(x*x + y*y);
	}

	//Método nuevo
	double distanciaA(Punto otroPunto) {
        int dx = this.x - otroPunto.x;
        int dy = this.y - otroPunto.y;
        return Math.sqrt(dx * dx + dy * dy);
	}	
}
```

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java se pasa el valor de la referencia (la dirección de memoria). A efectos prácticos: si cambias atributos del objeto Punto dentro del método, sí cambian fuera. Pero si pasas un primitivo como un `int`, se pasa una copia del valor, así que los cambios dentro no afectan fuera.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

Es el método estándar para devolver la representación en texto de un objeto (como `__str__` en Python). Es muy útil para depurar.

```java
public String toString() { 
    return "Punto(" + x + "," + y + ")"; 
}
```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

Sí, un struct es el ancestro de la clase. Básicamente agrupa datos. Para ser una clase le falta poder incluir métodos (funciones) dentro de la propia estructura y mecanismos de control de acceso (como `private` o `public`) para proteger esos datos (encapsulamiento).


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Crearías un `struct Punto { int x, y; }` y una función externa `distancia(struct Punto *p)`. En este caso, el puntero `*p` que pasas como argumento hace el trabajo de `this`: indicar explícitamente sobre qué estructura de datos debe operar la función.