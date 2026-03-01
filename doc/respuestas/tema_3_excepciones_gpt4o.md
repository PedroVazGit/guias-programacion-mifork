<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

Sin excepciones, debemos usar mecanismos manuales de retorno para avisar al llamador.

**Opción 1: Código de error como valor de retorno.**
La función devuelve un valor imposible (centinela) si hay error (por ejemplo, -1.0, ya que una raíz no puede ser negativa).

```c
double raiz(double n) {
    if (n < 0) return -1.0; // Valor centinela de error
    return sqrt(n);
}
```
**Opción 2: Parámetro por referencia y booleano de estado.**
La función devuelve `1` (true) o `0` (false) para indicar éxito o fracaso, y el resultado real se deposita en una variable pasada por puntero.

```c
int raiz(double n, double *resultado) {
    if (n < 0) return 0; // 0 indica error
    *resultado = sqrt(n);
    return 1; // 1 indica éxito
}
```
---

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una **excepción** es un evento anómalo o error que ocurre durante la ejecución de un programa e interrumpe el flujo normal de las instrucciones.

**Objetivo:** Se usan para separar el código de la "lógica de negocio" (lo que debe pasar) del código de "gestión de errores" (lo que pasa si falla). Esto permite tratar los errores de forma centralizada y limpia, evitando llenar el código principal de sentencias `if-else` de comprobación.

---


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

```java
class Calculadora {
    public static double raiz(double n) {
        if (n < 0) {
            // Lanzamos la excepción para que "el de fuera" decida qué hacer
            throw new IllegalArgumentException("No existe raíz de negativo");
        }
        return Math.sqrt(n);
    }
}

// 2. La clase externa que usa la Calculadora
public class Principal {
    public static void main(String[] args) {
        try {
            // Intentamos usar la calculadora desde fuera
            double resultado = Calculadora.raiz(-5);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            // Aquí "controlamos" el error que ha ocurrido en la otra clase
            System.out.println("Error capturado: " + e.getMessage());
        }
    }
}
```

---

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

* **Lanzar (`throw`):** Es la acción de crear el objeto excepción y arrojarlo al entorno de ejecución (`throw new...`) para señalar que ha ocurrido un error.
* **Capturar (`catch`):** Es interceptar ese objeto "al vuelo" mediante un bloque `try-catch` para solucionar el problema o informar de él.
* **Propagar:** Ocurre cuando un método no captura la excepción; esta "salta" automáticamente al método que lo llamó anteriormente.
* **Efecto en la pila:** Las funciones por donde pasa la excepción sin ser capturada **terminan abruptamente** y se eliminan de la pila de llamadas.
* **¿Se reanudan?:** **No**. Las funciones que no controlan la excepción mueren en la línea del error y no ejecutan el código restante.

---

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La gran ventaja es que **no obliga a gestionar el error inmediatamente** en cada función intermedia.
En C, si la función A llama a B, y B llama a C, y el error ocurre en C, tanto B como A deben tener código explícito (`if error return error`) para pasar el fallo hacia arriba. En Java, la función B puede ignorar el error, y este viajará "automáticamente" hasta A si es allí donde está el `catch`. Esto reduce drásticamente el código repetitivo de comprobación de errores.

---

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

Sí, en Java y la mayoría de lenguajes modernos, las excepciones son **objetos** (instancias de clases).

* **Ventaja de encapsulación:** Al ser objetos, pueden guardar datos complejos sobre el error (mensaje descriptivo, la pila de llamadas o *stack trace*, el código de error, etc.) dentro de una sola variable.
* **Excepciones personalizadas:** Sí, podemos crear nuestras propias clases (ej. `SaldoInsuficienteException`) heredando de la clase `Exception`. Esto permite dar un significado semántico específico a los errores de nuestro dominio.

---

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

Un objeto excepción lleva tres piezas de información crítica que los códigos de error simples de C no tienen:
1.  **El mensaje:** Una descripción textual del error (ej. "File not found").
2.  **El Stack Trace (Traza de pila):** La lista exacta de métodos y números de línea donde ocurrió el error y la ruta de llamadas hasta llegar allí.
3.  **La Causa (Cause):** Puede contener dentro otra excepción que originó el problema (encadenamiento de excepciones).

---

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, se pueden colocar múltiples bloques `catch` seguidos para manejar distintos tipos de excepciones de forma diferente.
Solo se ejecuta **uno**: el primero que coincida con el tipo de excepción que ha ocurrido. Por eso, es importante poner las excepciones más específicas (hijas) antes que las genéricas (padres).

---

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Para garantizar la ejecución de código de limpieza, usamos el bloque **`finally`**, que se ejecuta **siempre**, tanto si hubo error como si no.

**Ejemplo con catch (gestiona y limpia):**
```java
try {
    abrirFichero();
    leerDatos();
} catch (IOException e) {
    System.out.println("Error de lectura");
} finally {
    cerrarFichero(); // Se ejecuta SIEMPRE, haya error o no
}
```

**Ejemplo sin catch (limpia y propaga):**
```java
try {
    abrirFichero();
    // Si falla aquí, la excepción salta fuera...
} finally {
    cerrarFichero(); // ...pero ANTES de irse, ejecuta esto.
}
```

---

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

* **¿Puede ir sin catch?:** Sí (estructura `try-finally`). Se usa cuando no queremos arreglar el error ahí, pero necesitamos limpiar recursos antes de dejarlo pasar.
* **¿Se ejecuta siempre?:** Sí, ocurra o no la excepción.
* **¿Con return?:** Sí. Aunque haya un `return` dentro del `try`, el bloque `finally` se ejecuta **antes** de que el método devuelva efectivamente el control.

---

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

* **Controladas (Checked):** Heredan de `Exception`. El compilador **obliga** a capturarlas (`try-catch`) o declararlas (`throws`). Se usan para fallos externos de los que el programa podría recuperarse.
* **No controladas (Unchecked):** Heredan de `RuntimeException`. El compilador **no obliga** a nada. Se usan para errores de programación o bugs lógicos.

**Listas de uso:**
* **Preferir Controlada (Checked):**
    1.  Fichero de configuración no encontrado.
    2.  Conexión a base de datos caída temporalmente.
    3.  Formato de archivo inválido proporcionado por un usuario.
* **Preferir No Controlada (Unchecked):**
    1.  Acceso a un índice fuera de rango en un array (`IndexOutOfBounds`).
    2.  Pasar un objeto `null` donde no se debe (`NullPointer`).
    3.  Intentar dividir por cero (`ArithmeticException`).

---

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

`throws` es una palabra clave que se coloca en la firma del método para avisar: "Este método puede fallar y lanzar estas excepciones".
Es la alternativa al `try-catch` porque permite **delegar la responsabilidad**. En lugar de solucionar el error tú mismo, le "pasas la patata caliente" al método que te llamó para que él decida qué hacer.

---

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

```java
import java.io.*;

// Declara throws porque NO usa catch para IOException
public void procesarArchivo() throws IOException {
    FileReader fr = null;
    try {
        fr = new FileReader("datos.txt");
        // Operaciones de lectura...
    } finally {
        // Usamos finally para asegurar el cierre aunque propaguemos la excepción
        if (fr != null) {
            fr.close();
        }
    }
}
```

---

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, se pueden declarar en el `throws`, pero **no es obligatorio** ni el compilador lo exige.
El método llamador **no está obligado** a poner `try-catch`.
El sentido de hacerlo es puramente **documental**: sirve para avisar a otros programadores que usen tu código: "Cuidado, si usas mal este método (ej. pasas null), lanzaré esta excepción".

---

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

* **Recomendación Java:** Usa *Controladas* si esperas que el programa pueda recuperarse del error (ej. reintentar conexión). Usa *No Controladas* si es un error de lógica del programador que no debería ocurrir en producción.
* **Otros lenguajes:** No. La distinción Checked/Unchecked es casi exclusiva de Java.
* **Lo habitual:** Lenguajes modernos como C#, Python, Kotlin o Swift usan **solo excepciones no controladas**. Dejan al programador la libertad de decidir qué errores capturar y cuáles dejar pasar.

---

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, tiene sentido.

**Caso 1: Relanzar la misma excepción (`rethrow`).**
Útil si quieres hacer algo intermedio (como registrar el error en un log) pero dejar que el error siga subiendo para que lo trate otro.
```java
try {
    // código...
} catch (SQLException e) {
    System.out.println("Registro el error en log: " + e.getMessage());
    throw e; // La vuelvo a lanzar
}
```

**Caso 2: Lanzar una nueva excepción (Wrapping).**
Útil para ocultar detalles técnicos y lanzar una excepción más comprensible para el usuario.
```java
try {
    // código...
} catch (SQLException e) {
    // Oculto el error SQL y lanzo uno de mi aplicación
    throw new ErrorDeBaseDeDatosException("No se pudo guardar el usuario");
}
```

---

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Consiste en meter la excepción original (la de bajo nivel) dentro de una nueva excepción (la de alto nivel) usando el constructor. Esto se llama **encadenamiento de excepciones**. Permite dar un error claro al usuario sin perder la información técnica original.
Sí, al imprimir la traza (`printStackTrace`), se ve la excepción nueva y debajo aparece "Caused by..." con la excepción original.

```java
try {
    abrirConfiguracion();
} catch (IOException e) {
    // Creamos nuestra excepción pasando 'e' como la CAUSA
    throw new ConfiguracionException("Fallo al iniciar la app", e);
}
```