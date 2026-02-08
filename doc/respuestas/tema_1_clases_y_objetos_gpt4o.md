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

Abstracción,  Encapsulación, Herencia  y Polimorfismo. 

-Abstracción intenta olvidar detalles para: a) Tratar con temas complejos y b) Para facilitar el cambio.

-Encapsulación: a) Unir estado y comportamiento y b) Ocultar partes.

-Herencia: Establecer jerarquías.

-Polimorfismo: Misma función, distintas formas (o implementaciones).

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Java, C++, Python y Rust.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La programación estructurada consiste en estructuras de control condicional (if), iterativa (while, for, double). No tiene GOTO (Salto arbitrario). La programación Modular contiene otro tipo de objetos: "librería", "paquete", "modulo". Permite agrupar código para facilitar su uso para otros programas o módulos.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Identidad, Estado y Comportamiento.

-Identidad: dirección de memoria.

-Estado: atributos ( lo que en los struts eran campos).

-Comportamiento: métodos (son las funciones que todos los objetos de esa clase puedan hacer).


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

-Clase: Molde para crear instancias durante la ejecución. Define la estructura del estado y el comportamiento.

-Objeto: Variables de tipo alguna clase con un estado concreto de sus atributos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

```
class Punto {
    //Estado, es decir, atributos
	int x;
	int y;

	//Comportamiento, es decir, métodos...
	double calcularDistanciaAOrigen() {
		return Math.sqrt(x*x + y*y);
	}

}


public class Ejercicio1 {
	public static void main (String args []) {
		Punto mipunto = new Punto();
		mipunto.x = 5;
		mipunto.y = 6;

		double aOrigen = mipunto.calcularDistanciaAOrigen(mipunto);		
	}
}

```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
