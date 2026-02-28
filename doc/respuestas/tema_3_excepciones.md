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
En C, como no existen excepciones, el control de errores debe hacerse **mediante valores de retorno o parámetros adicionales**. La función no “lanza” un error, sino que **lo comunica al código que la llama**, y es ese código externo quien decide cómo informar al usuario. Esto obliga a diseñar explícitamente cómo se señalizan los errores.

## Opción 1: devolver un valor especial

La función devuelve un valor que indique error. En funciones matemáticas con `float` o `double`, una opción habitual es devolver un valor imposible o especial, como `-1` (si sabemos que no es válido) o mejor aún `NAN` (Not-a-Number).

```c id="copt001"
#include <stdio.h>
#include <math.h>

double raiz(double x) {
    if (x < 0) {
        return NAN;  // indica error
    }
    return sqrt(x);
}

int main() {
    double r = raiz(-4);

    if (isnan(r)) {
        printf("Error: no se puede calcular la raíz de un número negativo\n");
    } else {
        printf("Resultado: %f\n", r);
    }

    return 0;
}
```

Ventaja: es simple y no cambia la firma de la función.
Desventaja: depende de que exista un valor especial reconocible, y el programador debe acordarse de comprobarlo.

---

## Opción 2: devolver un código de estado y usar un parámetro de salida

La función devuelve un **código de éxito o error** (por ejemplo `0` = éxito, `-1` = error) y el resultado se guarda en un parámetro pasado por referencia (puntero).

```c id="copt002"
#include <stdio.h>
#include <math.h>

int raiz(double x, double *resultado) {
    if (x < 0) {
        return -1;  // código de error
    }
    *resultado = sqrt(x);
    return 0;      // éxito
}

int main() {
    double r;
    if (raiz(-4, &r) != 0) {
        printf("Error: no se puede calcular la raíz de un número negativo\n");
    } else {
        printf("Resultado: %f\n", r);
    }

    return 0;
}
```

Ventaja: es el método más robusto y general, porque separa claramente el **resultado** del **estado de la operación**.
Desventaja: la sintaxis es más compleja y requiere punteros.

---

Ambos enfoques son comunes en C. El segundo es el más utilizado en bibliotecas profesionales, porque permite manejar múltiples tipos de error de forma clara y sin ambigüedades.


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una **excepción** es un mecanismo de control de errores que permite **interrumpir el flujo normal de ejecución de un programa cuando ocurre una situación anómala**, y transferir el control a un bloque especial de código preparado para manejar ese error. En lugar de devolver códigos de error manualmente (como en C), el sistema “lanza” una excepción, que puede ser capturada y tratada en otro lugar del programa.

El objetivo al **implementar funciones** es poder señalar errores o situaciones inválidas de forma clara y obligar a que se tengan en cuenta. Por ejemplo, una función puede lanzar una excepción si recibe un parámetro inválido o si falla una operación (abrir un archivo inexistente, dividir por cero, etc.), evitando devolver resultados incorrectos o ambiguos.

El objetivo al **llamar funciones** es poder manejar esos errores de forma estructurada, separando el código normal del código de tratamiento de errores. Esto mejora la claridad, la robustez y el mantenimiento del programa, ya que el flujo principal no se llena de comprobaciones constantes, y los errores se gestionan en bloques específicos (por ejemplo, `try`–`catch` en Java).


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
En Java, en lugar de devolver valores especiales, lo más adecuado es **lanzar una excepción** cuando el parámetro es inválido. La función `raiz` indicará el error con `throw`, y el código que la llama lo controlará usando `try`–`catch`, manteniendo separada la lógica normal del tratamiento de errores.

```java id="calc001"
public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "No se puede calcular la raíz de un número negativo: " + x
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double resultado = Calculadora.raiz(-4);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error detectado: " + e.getMessage());
        }
    }
}
```

En este ejemplo, el método `raiz` **lanza una excepción** (`IllegalArgumentException`) si el argumento es negativo. El método `main` llama a `raiz` dentro de un bloque `try`; si ocurre la excepción, el flujo salta automáticamente al bloque `catch`, donde se informa al usuario. Esto permite que el error se detecte dentro de la función, pero se gestione desde fuera, que es el objetivo del mecanismo de excepciones.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
**Lanzar una excepción** significa crear y señalar un objeto de tipo excepción usando la palabra clave `throw`, indicando que ha ocurrido un error y que la ejecución normal no puede continuar en ese punto. Por ejemplo, en el método `raiz`, cuando detectamos un número negativo, hacemos:

```java
public static double raiz(double x) {
    if (x < 0) {
        throw new IllegalArgumentException("Número negativo: " + x);
    }
    return Math.sqrt(x);
}
```

En ese instante, la ejecución del método **se interrumpe inmediatamente** y no se ejecuta ninguna instrucción posterior dentro de ese método.

**Capturar o controlar una excepción** significa interceptarla usando un bloque `try`–`catch`, para poder reaccionar al error (mostrar un mensaje, recuperar el sistema, etc.). Por ejemplo, en `main`:

```java
public static void main(String[] args) {
    try {
        double resultado = raiz(-4);
        System.out.println(resultado); // no se ejecuta
    } catch (IllegalArgumentException e) {
        System.out.println("Error: " + e.getMessage());
    }
}
```

Cuando `raiz(-4)` lanza la excepción, el flujo salta directamente al `catch`, evitando que el programa termine abruptamente.

**Propagar una excepción** significa que, si un método no la captura, la excepción **sube automáticamente al método que lo llamó**, y así sucesivamente. Por ejemplo, si tenemos:

```java
public static double operacion(double x) {
    return raiz(x); // no hay try-catch aquí
}
```

y `main` llama a `operacion(-4)`, la excepción se lanza en `raiz`, pasa por `operacion` sin controlarse, y llega a `main`, donde sí se captura. Durante esta propagación, **cada método en la pila de llamadas termina inmediatamente**, liberando su contexto.

Es importante entender que las funciones por donde pasa la excepción **no se reanudan después**. Se eliminan de la pila (esto se llama *stack unwinding*), y su ejecución termina definitivamente. Solo continúa la ejecución en el punto donde la excepción es capturada. Si nadie la captura, el programa termina con un error y la máquina virtual muestra el *stack trace*, indicando la cadena de llamadas que llevó a la excepción.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
La **propagación natural de excepciones** permite que un error se transmita automáticamente desde la función donde ocurre hasta otra función superior que esté preparada para manejarlo, sin necesidad de comprobar manualmente cada valor de retorno. En C, cada función debe devolver un código de error y cada llamada debe comprobarlo explícitamente; si el programador olvida hacerlo en algún punto, el error puede pasar desapercibido y provocar fallos más graves o resultados incorrectos.

Una ventaja importante es la **separación clara entre la lógica normal y el tratamiento de errores**. Con excepciones, el código principal se escribe de forma limpia, y el manejo de errores se concentra en bloques `catch`. En C, en cambio, el código suele llenarse de comprobaciones (`if (error) return error;`), lo que reduce la legibilidad y hace el mantenimiento más difícil.

Otra ventaja es que las excepciones permiten que el error sea manejado **en el nivel más adecuado de la aplicación**, no necesariamente donde ocurre. Por ejemplo, una función matemática puede lanzar la excepción, una función intermedia puede ignorarla, y el programa principal puede decidir informar al usuario. En C, cada función intermedia debe recibir el código de error y reenviarlo manualmente, lo que aumenta la complejidad y la probabilidad de errores.

Finalmente, la propagación automática garantiza que las funciones que no pueden manejar el error **terminen inmediatamente y no continúen en un estado inconsistente**. Esto mejora la robustez del programa, ya que evita que el sistema siga ejecutándose con datos inválidos o incompletos, algo que en C depende completamente de la disciplina del programador.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
Sí, en la mayoría de lenguajes orientados a objetos, como Java, **las excepciones son objetos**. En Java, todas las excepciones son clases que heredan directa o indirectamente de la clase base `Exception` (y en última instancia de `Throwable`). Esto significa que una excepción es una instancia con **atributos y métodos**, igual que cualquier otro objeto, y puede transportar información sobre el error ocurrido.

Esto aporta ventajas importantes en términos de **encapsulación**. La excepción puede contener dentro de sí misma toda la información relevante del error (mensaje, código, valores que causaron el fallo, etc.), y proporcionar métodos públicos para acceder a esa información sin exponer los detalles internos. En lugar de devolver simples códigos numéricos ambiguos (como en C), se devuelve un objeto bien definido que describe claramente qué ha ocurrido y permite tratar distintos tipos de error de forma específica mediante el polimorfismo (`catch (IOException e)`, `catch (IllegalArgumentException e)`, etc.).

Sí, también podemos **crear excepciones personalizadas**, lo cual es una práctica habitual cuando queremos representar errores específicos de nuestra aplicación. Por ejemplo:

```java id="excp001"
public class NumeroNegativoException extends Exception {

    public NumeroNegativoException(double valor) {
        super("No se permite un número negativo: " + valor);
    }
}
```

Y usarla:

```java id="excp002"
public static double raiz(double x) throws NumeroNegativoException {
    if (x < 0) {
        throw new NumeroNegativoException(x);
    }
    return Math.sqrt(x);
}
```

Esto mejora el diseño porque cada tipo de error tiene su propia clase, claramente identificable y encapsulada, facilitando un manejo más preciso, seguro y mantenible de los errores.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Un objeto excepción en Java transporta **información esencial sobre el error ocurrido**, algo que en C normalmente se pierde o debe gestionarse manualmente. La primera información clave es el **tipo concreto de la excepción** (por ejemplo, `IllegalArgumentException`, `IOException`, etc.). Esto permite al manejador saber exactamente qué clase de problema ocurrió y reaccionar de forma distinta según el tipo, sin depender de códigos numéricos ambiguos.

La segunda información fundamental es el **mensaje descriptivo del error**, accesible mediante métodos como `getMessage()`. Este mensaje suele incluir detalles específicos, como el valor inválido que causó el fallo (`"Número negativo: -4"`). Gracias a la encapsulación, esta información viaja dentro del propio objeto excepción desde el punto donde se produjo el error hasta el punto donde se captura.

Otra información muy importante es el **stack trace** (la traza de la pila de llamadas), que indica qué métodos estaban activos y en qué línea ocurrió el error. Esto permite reconstruir el camino exacto que siguió la ejecución hasta el fallo, algo extremadamente útil para depuración. En C, esta información no existe automáticamente y debe reconstruirse manualmente, si es que se hace.

En resumen, una excepción encapsula al menos: **el tipo de error, un mensaje descriptivo y la traza de llamadas**, y opcionalmente otros datos adicionales definidos por el programador. Esta encapsulación hace que el manejo de errores sea mucho más informativo, preciso y mantenible que el uso de simples códigos de error.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Sí, en Java un bloque `try` puede tener **varios bloques `catch`**, cada uno diseñado para capturar un tipo diferente de excepción. Esto permite tratar de forma distinta cada clase de error, aprovechando que las excepciones forman una jerarquía de clases. Por ejemplo, se puede capturar primero una excepción específica y después una más general:

```java
try {
    double r = Calculadora.raiz(-4);
} catch (IllegalArgumentException e) {
    System.out.println("Argumento inválido: " + e.getMessage());
} catch (Exception e) {
    System.out.println("Error general: " + e.getMessage());
}
```

Sin embargo, **solo se ejecuta un bloque `catch`**, el primero que sea compatible con el tipo de la excepción lanzada. Una vez que se encuentra un `catch` adecuado, los demás bloques `catch` se ignoran y la ejecución continúa después del conjunto `try`–`catch`.

Por este motivo, el orden de los `catch` es importante: **deben colocarse primero las excepciones más específicas y después las más generales**. Si se pusiera primero `catch (Exception e)`, este capturaría casi todas las excepciones y los bloques más específicos nunca se ejecutarían, lo que el compilador incluso puede señalar como error.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
En Java, el bloque **`finally`** se utiliza para garantizar que cierto código se ejecuta **siempre**, independientemente de si ocurre o no una excepción, o incluso si la excepción se propaga a un nivel superior. Esto es fundamental para liberar recursos como ficheros, conexiones de red o memoria, ya que una excepción puede interrumpir el flujo normal y evitar que se ejecuten las instrucciones de cierre si no se usa `finally`.

### Ejemplo con `try–catch–finally`

Aquí se captura la excepción y, además, el bloque `finally` se ejecuta siempre:

```java id="fin001"
import java.io.FileWriter;
import java.io.IOException;

public class EjemploFinally {

    public static void main(String[] args) {
        FileWriter fichero = null;

        try {
            fichero = new FileWriter("salida.txt");
            fichero.write("Hola\n");

            int x = -1;
            if (x < 0) {
                throw new IllegalArgumentException("Valor negativo");
            }

        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());

        } catch (IOException e) {
            System.out.println("Error de fichero: " + e.getMessage());

        } finally {
            System.out.println("Cerrando fichero...");
            try {
                if (fichero != null) {
                    fichero.close();
                }
            } catch (IOException e) {
                System.out.println("Error al cerrar el fichero");
            }
        }
    }
}
```

El bloque `finally` se ejecuta tanto si hay excepción como si no, garantizando el cierre del fichero.

---

### Ejemplo con `try–finally` (sin `catch`)

Aquí no se captura la excepción; se ejecuta el `finally` y luego la excepción **se sigue propagando**:

```java id="fin002"
public class EjemploFinallySinCatch {

    public static double raiz(double x) {
        try {
            if (x < 0) {
                throw new IllegalArgumentException("Número negativo");
            }
            return Math.sqrt(x);

        } finally {
            System.out.println("Fin del método raiz (siempre se ejecuta)");
        }
    }

    public static void main(String[] args) {
        double r = raiz(-4); // la excepción se propagará
        System.out.println(r);
    }
}
```

En este caso, el mensaje del `finally` se imprime, pero como no hay `catch`, la excepción continúa propagándose y el programa termina con error.

En resumen, **`finally` garantiza la ejecución de código crítico de limpieza**, tanto si la excepción se captura como si continúa propagándose, lo que lo convierte en un mecanismo esencial para la gestión segura de recursos.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí, en Java el bloque **`finally` puede usarse sin `catch`**. La estructura válida es `try–finally`, donde el `try` contiene el código que puede producir una excepción y el `finally` contiene el código que debe ejecutarse siempre, normalmente para liberar recursos. En este caso, si ocurre una excepción, el bloque `finally` se ejecuta y después la excepción **se sigue propagando** al método llamador.

El bloque **`finally` se ejecuta siempre**, tanto si ocurre una excepción como si no ocurre ninguna. Su propósito es garantizar la ejecución de código crítico de limpieza, independientemente de cómo termine el bloque `try`. Por ejemplo:

```java id="fin003"
public static void ejemplo() {
    try {
        System.out.println("Dentro del try");
    } finally {
        System.out.println("Dentro del finally (siempre se ejecuta)");
    }
}
```

Incluso si hay un **`return` dentro del `try`**, el bloque `finally` se ejecuta antes de que el método termine. Por ejemplo:

```java id="fin004"
public static int ejemploReturn() {
    try {
        System.out.println("Dentro del try");
        return 10;
    } finally {
        System.out.println("Dentro del finally");
    }
}
```

La salida será:

```
Dentro del try
Dentro del finally
```

y el método devolverá `10`. Es decir, Java **ejecuta primero el `finally` y luego completa el `return`**. La única situación en la que `finally` podría no ejecutarse es si la JVM termina abruptamente (por ejemplo, con `System.exit()` o un fallo crítico del sistema).


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java, las excepciones se dividen en **controladas (checked)** y **no controladas (unchecked)** según cómo obliga el compilador a manejarlas:

---

### 1. Excepciones **controladas** (*checked exceptions*)

* Son aquellas que **el compilador exige capturar o declarar** con `throws` en la firma del método.
* Suelen representar errores **previsibles y recuperables**, como fallos de E/S, archivos no encontrados o problemas de red.
* Al usar estas excepciones, el programador se ve obligado a pensar explícitamente cómo manejarlas, mejorando la robustez del código.

Ejemplos típicos:

* `IOException` – error al leer o escribir un archivo.
* `FileNotFoundException` – archivo no encontrado al intentar abrirlo.
* `SQLException` – fallo al acceder a una base de datos.
* Excepciones personalizadas que representen condiciones recuperables, por ejemplo, `SaldoInsuficienteException` en una clase `CuentaBancaria`.

Situaciones donde **se suele preferir una excepción controlada**:

1. Lectura/escritura de ficheros.
2. Conexiones de red o sockets que pueden fallar.
3. Acceso a bases de datos.
4. Validaciones de entrada que requieren recuperación (por ejemplo, usuario introduce datos incorrectos en un formulario).

---

### 2. Excepciones **no controladas** (*unchecked exceptions*)

* Heredan de `RuntimeException`.
* El compilador **no obliga** a capturarlas ni declararlas.
* Representan errores de **programación** o condiciones que normalmente no se pueden recuperar en tiempo de ejecución: errores lógicos, violaciones de contratos de la API.

Ejemplos típicos:

* `NullPointerException` – acceso a un objeto nulo.
* `ArrayIndexOutOfBoundsException` – índice fuera de los límites de un arreglo.
* `ArithmeticException` – división por cero.
* Excepciones personalizadas que indican un fallo lógico, como `OperacionInvalidaException` para una operación que viola reglas internas de la aplicación.

Situaciones donde **se suele preferir una excepción no controlada**:

1. División por cero en cálculos matemáticos.
2. Acceso a elementos de un array fuera de rango.
3. Referencias nulas inesperadas (`null`).
4. Llamadas a métodos con precondiciones incumplidas (por ejemplo, método que requiere que un parámetro no sea negativo).

---

En resumen:

* **Checked exceptions**: se usan para errores previsibles y recuperables; obligan a manejar o declarar.
* **Unchecked exceptions (RuntimeException)**: se usan para errores de programación o violaciones de contrato; no se requiere captura explícita.

Esto permite que el lenguaje combine **robustez y claridad** con **fluidez en el código**, según el tipo de error que pueda ocurrir.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
En Java, la palabra clave **`throws`** se utiliza en la **firma de un método** para declarar que dicho método **puede lanzar ciertas excepciones controladas** (*checked exceptions*). Esto le indica al compilador y a quien llama al método que estas excepciones deben ser **manejadas** en algún nivel superior, ya sea capturándolas con `try–catch` o propagándolas nuevamente.

Por ejemplo:

```java id="thr001"
public void leerArchivo(String nombre) throws IOException {
    FileReader fr = new FileReader(nombre); // puede lanzar FileNotFoundException
    // operaciones de lectura...
}
```

Aquí, `leerArchivo` **no captura la excepción** dentro de sí mismo, sino que la **propaga** al código que lo llame. Esto permite que el programador que llama al método decida cómo tratar el error.

El uso de `throws` es una **alternativa a capturar la excepción dentro del propio método**. En lugar de manejar el error en el método donde ocurre, se deja que la excepción se **propague** hacia un nivel superior, lo que puede ser útil cuando:

1. El método no tiene suficiente contexto para decidir cómo reaccionar ante el error.
2. Se desea centralizar la gestión de errores en un nivel superior de la aplicación (por ejemplo, en el `main` o en un controlador de la interfaz de usuario).
3. Se quiere mantener el método limpio y concentrarse solo en la lógica principal, delegando la recuperación de errores a quien lo llame.

En resumen, `throws` permite que un método indique al mundo exterior “puedo lanzar esta excepción; quien me llame debe ocuparse de manejarla”, mientras que capturar la excepción dentro del método implicaría **resolver o procesar el error localmente**. Esto es parte del diseño estructurado y seguro del manejo de errores en Java.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
Aquí tienes un ejemplo completo en Java que ilustra cómo usar **`throws`** en la firma de un método que abre un fichero, dejando que la excepción `FileNotFoundException` se propague hacia el llamador, pero garantizando con **`finally`** que el recurso se cierre correctamente si fue abierto:

```java id="throws001"
import java.io.FileReader;
import java.io.FileNotFoundException;
import java.io.IOException;

public class EjemploThrowsFinally {

    // Declaramos que este método puede lanzar FileNotFoundException
    public static void abrirFichero(String nombreFichero) throws FileNotFoundException {
        FileReader fr = null;
        try {
            fr = new FileReader(nombreFichero); // puede lanzar FileNotFoundException
            System.out.println("Fichero abierto correctamente.");
            // Aquí se podrían hacer operaciones de lectura...
        } finally {
            // Este bloque siempre se ejecuta, incluso si se lanza la excepción
            if (fr != null) {
                try {
                    fr.close();
                    System.out.println("Fichero cerrado en finally.");
                } catch (IOException e) {
                    System.out.println("Error al cerrar el fichero: " + e.getMessage());
                }
            }
        }
    }

    public static void main(String[] args) {
        try {
            abrirFichero("archivo_inexistente.txt");
        } catch (FileNotFoundException e) {
            System.out.println("El fichero no existe: " + e.getMessage());
        }
    }
}
```

**Explicación clave:**

1. `throws FileNotFoundException` en la firma indica que el método **no maneja esta excepción**, sino que se propaga a quien lo llame.
2. El bloque `finally` garantiza que `fr.close()` se ejecute si se llegó a abrir el fichero, incluso si ocurre la excepción.
3. En `main`, se captura la excepción propagada para informar al usuario que el fichero no existe.

Este patrón combina **propagación de excepciones controladas** con la seguridad de **liberación de recursos**, lo cual es fundamental para manejar ficheros, conexiones de red o bases de datos en Java.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Sí, en Java se **puede declarar en `throws` excepciones no controladas** (unchecked), como `RuntimeException` o sus subclases, pero **no es obligatorio**. El compilador **no exige** que el llamador las capture ni que las declare, porque se considera que son errores de programación que normalmente no se pueden recuperar en tiempo de ejecución, como `NullPointerException` o `ArrayIndexOutOfBoundsException`.

Por ejemplo:

```java id="rtex001"
public void dividir(int a, int b) throws ArithmeticException {
    int resultado = a / b; // puede lanzar ArithmeticException si b == 0
}
```

En este caso, declarar `throws ArithmeticException` es **informativo**, pero el llamador **no está obligado a usar `try-catch`**. Si decide capturarla, puede reaccionar ante un error lógico y evitar que el programa termine abruptamente; si no la captura, la excepción se propagará normalmente y terminará el programa si no hay ningún bloque que la maneje más arriba.

El **sentido de declararlas** en `throws` es principalmente:

1. **Documentar** que un método puede generar esa excepción, ayudando a quien lee o usa el método a estar consciente de posibles errores.
2. Facilitar la **estrategia de captura opcional**, es decir, el llamador puede decidir explícitamente si quiere tratar la excepción o dejarla propagarse.
3. Aclarar la intención de la API: distingue qué errores se consideran previsibles y cuáles se consideran errores de programación.

En resumen, poner `RuntimeException` en `throws` **no cambia la obligación del compilador**, pero sí aporta documentación y claridad. La captura en el llamador es opcional y tiene sentido cuando queremos evitar que errores de programación simples terminen abruptamente la ejecución, por ejemplo en interfaces de usuario o sistemas críticos.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
En Java y otros lenguajes que diferencian **excepciones controladas (checked)** y **no controladas (unchecked)**, la recomendación general es:

---

### 1. **Excepciones controladas (checked)**

* Se usan para **errores previsibles y recuperables** que dependen de factores externos al programa, como fallos de E/S, archivos no encontrados, errores de red o bases de datos.
* Permiten obligar al llamador a **pensar cómo manejar el error**, ya sea capturándolo o propagándolo.
* Ejemplos típicos:

  * `IOException` al leer o escribir un fichero.
  * `FileNotFoundException` si un archivo no existe.
  * `SQLException` al acceder a una base de datos.
* Son útiles cuando se espera que el error pueda **ocurrir en condiciones normales** y el programa puede continuar después de recuperarse.

---

### 2. **Excepciones no controladas (unchecked / RuntimeException)**

* Se usan para **errores de programación**, fallos lógicos o violaciones de contrato, donde normalmente **no se puede ni se debe intentar recuperarse**.
* No obligan al compilador a que se capturen, permitiendo escribir código más limpio.
* Ejemplos típicos:

  * `IllegalArgumentException` si un parámetro no cumple los requisitos.
  * `NullPointerException` al intentar acceder a un objeto nulo.
  * `ArrayIndexOutOfBoundsException` si se accede fuera de un array.
* Se utilizan para **alertar al programador de errores en tiempo de ejecución**, no para controlar situaciones externas previsibles.

---

### 3. Disponibilidad en otros lenguajes

* **No todos los lenguajes diferencian checked y unchecked**.

  * Por ejemplo, **Python** o **C++** solo tienen un tipo de excepciones; no hay distinción obligatoria por el compilador.
  * En estos casos, todas las excepciones se comportan como lo que en Java sería **unchecked**, es decir, el programador puede capturarlas si quiere, pero no hay obligación.
* La opción más habitual en lenguajes sin distinción es **no controlada**, y se espera que el programador decida dónde capturarlas según la lógica de la aplicación.

---

En resumen:

* **Checked** → errores externos previsibles, recuperables, que deben ser tratados.
* **Unchecked** → errores internos de programación, no recuperables, opcional capturarlos.
* Solo Java y algunos lenguajes hacen esta distinción; la mayoría de lenguajes orientados a objetos modernos usan un solo tipo de excepción, equivalente a **unchecked**.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Sí, en Java **tiene sentido lanzar excepciones dentro de un bloque `catch`**, y se hace frecuentemente cuando queremos **transformar un error, añadir contexto adicional o propagarlo a un nivel superior**. Esto permite que el manejo local del error no sea definitivo, sino que la excepción continúe su propagación hacia un llamador que pueda gestionarla mejor, o que se convierta en un tipo de excepción más apropiado para la API que estamos diseñando.

Por ejemplo, podemos **lanzar una nueva excepción desde el `catch`** para añadir información contextual:

```java id="excatch001"
try {
    double r = Calculadora.raiz(-4);
} catch (IllegalArgumentException e) {
    // Añadimos contexto y lanzamos otra excepción
    throw new RuntimeException("Error en cálculo de raíz en main", e);
}
```

Aquí, la excepción original (`IllegalArgumentException`) se encapsula como causa (`e`) de la nueva excepción (`RuntimeException`). Esto es útil cuando queremos que el llamador superior reciba información adicional sobre **dónde y por qué ocurrió el error**.

También podemos **relanzar la misma excepción capturada** usando simplemente `throw e;` dentro del bloque `catch`. Esto se hace cuando queremos **ejecutar alguna acción secundaria al detectar la excepción** (como registrar un log, liberar un recurso temporal, o enviar una notificación) antes de que la excepción siga propagándose, sin modificarla:

```java id="excatch002"
try {
    double r = Calculadora.raiz(-4);
} catch (IllegalArgumentException e) {
    System.out.println("Se detectó un número negativo: " + e.getMessage());
    throw e;  // relanzamos la misma excepción
}
```

En este caso, la captura permite ejecutar código adicional, pero la excepción continúa subiendo por la pila hasta que algún nivel superior la maneje. Esto **tiene sentido cuando queremos registrar o limpiar información local** sin absorber el error, asegurando que el flujo de propagación original se mantenga intacto.

En resumen:

* Lanzar una nueva excepción en `catch` sirve para **transformar, añadir contexto o cambiar el tipo** de la excepción.
* Relanzar la misma excepción sirve para **hacer tareas auxiliares** antes de que la excepción siga propagándose. Ambos enfoques aprovechan la flexibilidad del manejo de excepciones en Java.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
En Java, que una excepción sea la **“causa”** de otra significa que estamos creando una nueva excepción que **envuelve** a otra previamente ocurrida, de manera que la primera indica **el contexto más general**, mientras que la segunda describe **el detalle original del error**. Esto es útil para **propagar errores a niveles superiores**, manteniendo información sobre lo que pasó originalmente, y permitiendo que un manejador de alto nivel pueda entender tanto la situación general como la causa concreta.

Por ejemplo, podemos capturar una excepción de bajo nivel, como `FileNotFoundException`, y lanzarla dentro de una excepción personalizada de más alto nivel, como `OperacionArchivoException`:

```java id="exc_causa001"
import java.io.FileReader;
import java.io.FileNotFoundException;

class OperacionArchivoException extends Exception {
    public OperacionArchivoException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class EjemploCausa {

    public static void abrirArchivo(String nombre) throws OperacionArchivoException {
        try {
            FileReader fr = new FileReader(nombre); // puede lanzar FileNotFoundException
        } catch (FileNotFoundException e) {
            // encapsulamos la excepción de bajo nivel en otra de alto nivel
            throw new OperacionArchivoException("No se pudo abrir el archivo " + nombre, e);
        }
    }

    public static void main(String[] args) {
        try {
            abrirArchivo("archivo_inexistente.txt");
        } catch (OperacionArchivoException e) {
            e.printStackTrace(); // muestra la excepción y su causa
        }
    }
}
```

Cuando ejecutamos este código, `e.printStackTrace()` muestra la excepción de alto nivel (`OperacionArchivoException`) **y debajo, indentada, la traza de la causa original** (`FileNotFoundException`). Así podemos ver **toda la cadena de errores** desde la excepción que se lanzó hasta la causa inicial, lo que facilita la depuración y la comprensión del problema.

En resumen:

* La causa permite **mantener la relación entre excepciones**, agregando contexto sin perder la información original.
* En la salida por pantalla (`printStackTrace()`), la causa se muestra automáticamente, mostrando la jerarquía de excepciones y sus ubicaciones en la pila de llamadas.
* Este patrón es común en APIs robustas que quieren **ocultar detalles de bajo nivel** al llamador, pero sin perder información crítica del error original.

