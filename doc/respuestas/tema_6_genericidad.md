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
En C, una forma clásica de simular “genericidad” (sin plantillas ni POO) consiste en utilizar un array de punteros `void*`. Este tipo actúa como un puntero genérico que puede apuntar a cualquier tipo de dato, perdiendo la información de tipo en tiempo de compilación. De esta forma, se puede construir, por ejemplo, una lista basada en array que almacene direcciones de memoria de distintos tipos (enteros, estructuras, etc.). Sin embargo, es responsabilidad del programador recordar el tipo real de cada elemento y hacer los *casts* adecuados al recuperarlos.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    void** data;
    int size;
    int capacity;
} GenericArray;

GenericArray* createArray(int capacity) {
    GenericArray* arr = malloc(sizeof(GenericArray));
    arr->data = malloc(sizeof(void*) * capacity);
    arr->size = 0;
    arr->capacity = capacity;
    return arr;
}

void add(GenericArray* arr, void* element) {
    if (arr->size < arr->capacity) {
        arr->data[arr->size++] = element;
    }
}

int main() {
    GenericArray* arr = createArray(10);

    int* a = malloc(sizeof(int));
    *a = 42;
    add(arr, a);

    double* b = malloc(sizeof(double));
    *b = 3.14;
    add(arr, b);

    printf("int: %d\n", *(int*)arr->data[0]);
    printf("double: %.2f\n", *(double*)arr->data[1]);

    return 0;
}
```

En Java, antes de usar genéricos, se recurría a `Object` para lograr un efecto similar. Dado que todas las clases heredan de `Object`, se puede crear un array de `Object` capaz de almacenar cualquier tipo de referencia. Al igual que con `void*`, se pierde la información de tipo específico, por lo que es necesario hacer *casting* al recuperar los elementos, con el riesgo de errores en tiempo de ejecución si se usa un tipo incorrecto.

```java
class GenericArray {
    private Object[] data;
    private int size;

    public GenericArray(int capacity) {
        data = new Object[capacity];
        size = 0;
    }

    public void add(Object element) {
        if (size < data.length) {
            data[size++] = element;
        }
    }

    public Object get(int index) {
        return data[index];
    }

    public static void main(String[] args) {
        GenericArray arr = new GenericArray(10);

        arr.add(42);        // autoboxing a Integer
        arr.add(3.14);      // autoboxing a Double

        int a = (Integer) arr.get(0);
        double b = (Double) arr.get(1);

        System.out.println("int: " + a);
        System.out.println("double: " + b);
    }
}
```

Ambos enfoques permiten almacenar datos heterogéneos en una misma estructura basada en arrays primitivos, pero sacrifican la seguridad de tipos. Este inconveniente es precisamente lo que la genericidad moderna en Java pretende resolver, evitando *casts* explícitos y detectando errores en tiempo de compilación en lugar de en ejecución.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La **programación genérica** es un paradigma que permite escribir código independiente del tipo de dato con el que se trabaja, de forma que una misma estructura o algoritmo pueda reutilizarse con distintos tipos manteniendo la seguridad de tipos. En Java, esto se materializa mediante los *genéricos* (por ejemplo, `ArrayList<T>`), donde el tipo concreto se especifica en tiempo de compilación, evitando conversiones explícitas y reduciendo errores en ejecución.

El objetivo principal es separar el “qué hace” una estructura o algoritmo de los tipos concretos que maneja. Esto recuerda a ciertos enfoques en C con `void*`, pero con una diferencia clave: en programación genérica moderna, el compilador conoce los tipos y puede comprobar que el uso es correcto, mientras que con `void*` o `Object` esa responsabilidad recae en el programador.

El ejemplo anterior con `void*` en C o `Object` en Java se considera una forma primitiva o aproximación a la genericidad, pero no es programación genérica en sentido estricto. Aunque permite almacenar distintos tipos en una misma estructura, no proporciona verificación de tipos en compilación ni evita *casts*, lo que introduce riesgos de errores.

Por tanto, se puede considerar un antecedente conceptual de la programación genérica, pero no una implementación real de esta. La programación genérica auténtica es la que ofrecen mecanismos como las plantillas en C++ o los genéricos en Java, donde el tipo se parametriza de forma segura y controlada.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema es la **pérdida de comprobación de tipos en tiempo de compilación**. Al usar `void*` en C o `Object` en Java, la estructura puede almacenar cualquier tipo, pero el compilador no puede verificar si los datos insertados o recuperados son coherentes con el uso esperado. Esto implica que errores de tipo (por ejemplo, tratar un `double` como si fuera un `int`) no se detectan al compilar, sino que aparecen en tiempo de ejecución o, en el caso de C, pueden provocar comportamientos indefinidos.

Otro inconveniente es la necesidad de realizar *casts* explícitos al recuperar los elementos. Estos *casts* dependen completamente del programador, que debe recordar el tipo real de cada elemento almacenado. Si se comete un error en esa suposición, en Java se producirá una excepción como `ClassCastException`, mientras que en C puede producirse corrupción de datos o fallos difíciles de depurar. Esto reduce significativamente la robustez del código.

Además, se pierde la **expresividad del sistema de tipos**. No es posible, por ejemplo, restringir que una estructura solo acepte ciertos tipos o garantizar que las operaciones internas sean válidas para los datos almacenados. Esto limita la capacidad del compilador para ayudar a detectar errores y obliga a introducir comprobaciones manuales o convenciones externas.

En conjunto, estos problemas hacen que el uso de `void*` o `Object` para simular genericidad sea propenso a errores y menos seguro. Precisamente, los mecanismos de programación genérica modernos buscan resolver estas limitaciones proporcionando verificación de tipos en compilación sin perder flexibilidad.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los **parámetros de tipo** son identificadores simbólicos que se utilizan para representar tipos concretos dentro de clases, interfaces o métodos, permitiendo escribir código genérico. En lugar de fijar un tipo específico (como `int` o `String`), se emplea un parámetro como `T`, `E` o `K`, que será sustituido por un tipo real cuando se use la clase o método. De esta forma, se consigue reutilizar una misma implementación para distintos tipos sin perder seguridad.

En Java, estos parámetros se declaran entre símbolos `< >`, por ejemplo en una clase como `class Caja<T>`. Aquí, `T` actúa como un “placeholder” de tipo: el compilador no sabe inicialmente qué tipo será, pero sí puede garantizar que se use de forma consistente dentro de la clase. Cuando se instancia (`Caja<Integer>`, `Caja<String>`), el compilador sustituye ese parámetro por el tipo concreto y verifica que todo sea correcto.

La principal ventaja es que permiten recuperar la **comprobación de tipos en tiempo de compilación**, evitando los problemas asociados a `Object`. Ya no es necesario hacer *casts* explícitos al recuperar datos, y si se intenta usar un tipo incorrecto, el error se detecta antes de ejecutar el programa. Esto mejora tanto la seguridad como la claridad del código.

En resumen, los parámetros de tipo son el mecanismo fundamental que hace posible la programación genérica moderna: permiten abstraer sobre tipos manteniendo control estático, algo que no se lograba con enfoques como `void*` o `Object`.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
En Java, la programación genérica se utiliza mediante *generics*, lo que permite definir estructuras de datos parametrizadas por tipo. Por ejemplo, una lista dinámica puede declararse como `ArrayList<String>`, indicando que solo almacenará objetos de tipo `String`. El compilador se encarga de verificar que no se inserten otros tipos y de eliminar la necesidad de *casts* al recuperar los elementos.

```java id="javagen1"
import java.util.ArrayList;

public class EjemploJava {
    public static void main(String[] args) {
        ArrayList<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Genéricos");

        for (String s : lista) {
            System.out.println(s.toUpperCase()); // s es String con seguridad
        }
    }
}
```

En este caso, cada elemento recuperado del `ArrayList` ya es tratado como `String`, sin conversiones explícitas. Si se intentara añadir un `Integer`, el compilador produciría un error. Esto garantiza seguridad de tipos en tiempo de compilación, evitando problemas típicos del uso de `Object`.

En C++, la programación genérica se implementa mediante *templates*. La biblioteca estándar proporciona estructuras como `std::vector<T>`, donde `T` es el tipo parametrizado. Al instanciar `std::vector<std::string>`, se obtiene un contenedor que solo admite cadenas, con comprobación en compilación similar a Java.

```cpp id="cppgen1"
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;

    lista.push_back("Hola");
    lista.push_back("Mundo");
    lista.push_back("Templates");

    for (const std::string& s : lista) {
        std::cout << s << std::endl; // s es std::string con seguridad
    }

    return 0;
}
```

Aquí, el tipo `std::string` queda fijado en la instancia del `vector`, por lo que cualquier intento de insertar otro tipo generará un error de compilación. Tanto en Java como en C++, estos mecanismos permiten escribir código reutilizable manteniendo la seguridad de tipos, a diferencia de las aproximaciones basadas en `void*` o `Object`.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Cuando se instancia una clase con parámetros de tipo, el compilador debe traducir ese código genérico a algo ejecutable. En esencia, verifica que los tipos usados sean correctos y transforma la definición genérica en una forma concreta. Sin embargo, la manera de hacerlo difiere entre lenguajes. En todos los casos, el objetivo es mantener la reutilización del código sin perder seguridad de tipos, pero el momento y la forma en que se “materializan” los tipos concretos cambia.

En Java, el compilador aplica un proceso llamado **type erasure** (borrado de tipos). Esto significa que la información de los parámetros de tipo (`<T>`, `<String>`, etc.) se elimina en tiempo de compilación y se sustituye por su límite superior (normalmente `Object` si no se especifica otro). Además, el compilador inserta automáticamente los *casts* necesarios y comprobaciones de tipo. Como consecuencia, en tiempo de ejecución no existe información sobre los tipos genéricos concretos, lo que implica, por ejemplo, que no se pueden crear arrays de tipos genéricos o consultar directamente el tipo parametrizado.

En C++, en cambio, se utiliza la **instanciación de plantillas**. Cada vez que se usa una plantilla con un tipo concreto (por ejemplo, `std::vector<std::string>`), el compilador genera una versión específica del código para ese tipo. Es decir, no se borra la información, sino que se crean implementaciones distintas para cada combinación de tipos utilizada. Esto permite mayor optimización y flexibilidad (incluso trabajar con tipos primitivos sin restricciones), pero puede aumentar el tamaño del código compilado.

Por tanto, no hacen lo mismo: Java reutiliza una única implementación mediante *type erasure*, mientras que C++ genera múltiples versiones mediante instanciación de plantillas. Ambos enfoques logran genericidad con seguridad en compilación, pero con diferencias importantes en rendimiento, expresividad y comportamiento en tiempo de ejecución.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
Para definir una clase genérica `Par` en Java, se utilizan **dos parámetros de tipo**, ya que se quiere almacenar dos valores potencialmente distintos. Estos parámetros (por ejemplo, `T` y `U`) actúan como sustitutos de tipos concretos y permiten que la clase sea reutilizable. El compilador verificará que los tipos usados al instanciar la clase sean coherentes y garantizará que los valores recuperados tengan el tipo correcto sin necesidad de *casts*.

```java id="parclass1"
class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}
```

Un uso típico de esta clase es como tipo de retorno cuando se necesitan devolver varios valores desde un método. Por ejemplo, se puede calcular la media y la desviación típica de un array de `double` y devolver ambos resultados en un `Par<Double, Double>`. Gracias a los genéricos, el compilador asegura que ambos valores son `Double` y no es necesario realizar conversiones explícitas al utilizarlos.

```java id="paruse1"
public class Estadistica {

    public static Par<Double, Double> mediaYDesviacion(double[] datos) {
        double suma = 0.0;
        for (double d : datos) {
            suma += d;
        }
        double media = suma / datos.length;

        double sumaCuadrados = 0.0;
        for (double d : datos) {
            sumaCuadrados += Math.pow(d - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] datos = {1.0, 2.0, 3.0, 4.0, 5.0};

        Par<Double, Double> resultado = mediaYDesviacion(datos);

        double media = resultado.getPrimero();
        double desviacion = resultado.getSegundo();

        System.out.println("Media: " + media);
        System.out.println("Desviación típica: " + desviacion);
    }
}
```

En este ejemplo se observa cómo los parámetros de tipo permiten reutilizar la clase `Par` con distintos tipos concretos. Además, el compilador garantiza que los valores recuperados coinciden con los tipos declarados (`Double` en este caso), eliminando errores típicos asociados al uso de `Object` y mejorando la claridad del código.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
En Java, los métodos también pueden declarar **parámetros de tipo propios**, independientes de la clase. Esto permite escribir operaciones genéricas sin necesidad de parametrizar toda la clase. Un ejemplo es un método `seleccionaUno` que, dados dos objetos del mismo tipo, devuelve uno de ellos de forma aleatoria. Definido con genéricos, el compilador conoce el tipo concreto y puede garantizar su uso correcto en todo momento.

```java id="genmethod1"
import java.util.Random;

public class Util {
    public static <T> T seleccionaUno(T a, T b) {
        Random r = new Random();
        return r.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String s = seleccionaUno("Hola", "Mundo"); // sin casting
        System.out.println(s.toUpperCase());
    }
}
```

Si se definiera el mismo método usando `Object`, se perdería la información de tipo. El método podría aceptar cualquier combinación de objetos y devolvería un `Object`, obligando a realizar *downcasting* al recuperar el valor. Esto introduce riesgos en tiempo de ejecución si el tipo asumido no coincide con el real.

```java id="genmethod2"
import java.util.Random;

public class UtilObject {
    public static Object seleccionaUno(Object a, Object b) {
        Random r = new Random();
        return r.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        Object o = seleccionaUno("Hola", 42); // permitido
        String s = (String) o; // posible ClassCastException
        System.out.println(s);
    }
}
```

La diferencia clave es doble. Por un lado, el uso de `<T>` evita el *downcasting*, ya que el compilador sabe que el resultado es del mismo tipo que los argumentos. Por otro, fuerza que ambos parámetros sean del mismo tipo: no sería posible llamar a `seleccionaUno("Hola", 42)` en la versión genérica, ya que produciría un error de compilación. En cambio, con `Object`, esa incoherencia solo se detecta (si se detecta) en tiempo de ejecución.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, en Java se pueden establecer **restricciones (bounds)** sobre los parámetros de tipo. Esto permite limitar los tipos que pueden usarse al instanciar una clase genérica. Por ejemplo, se puede indicar que un tipo `<T>` debe ser al menos `Number`, usando `T extends Number`. Esto garantiza que solo se acepten tipos numéricos (`Integer`, `Double`, etc.) y permite invocar métodos propios de `Number` dentro de la clase.

Una primera solución sencilla consiste en definir el `Punto` usando directamente `Number`. Esto permite almacenar cualquier tipo numérico, pero no distingue si ambos valores son del mismo tipo concreto ni evita conversiones internas.

```java id="point1"
class PuntoNumber {
    private Number x;
    private Number y;

    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double distanciaA(PuntoNumber p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

El problema de esta versión es que se pierde información de tipo más específica y siempre se trabaja como `Number`, lo que implica conversiones a `double` y menor control en compilación.

Una segunda solución mejora esto usando **genéricos acotados**, asegurando que las coordenadas son de un tipo numérico concreto y consistente.

```java id="point2"
class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double distanciaA(Punto<T> p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Aquí el compilador garantiza que ambos puntos usan el mismo tipo numérico (`T`), lo que refuerza la seguridad de tipos y evita mezclar, por ejemplo, `Integer` con `Double` dentro del mismo uso del objeto.

Respecto a **type erasure**, en ambas soluciones el tipo genérico desaparece en tiempo de ejecución. En la versión genérica, `Punto<T>` se convierte finalmente en `Punto` con `Number` como límite superior efectivo. Es decir, tras la compilación, el tipo real almacenado es equivalente a:

* `Punto<T extends Number>` → `Punto` (con `Number` internamente)

Por tanto, en tiempo de ejecución no existe información sobre `T`, solo sobre `Number`, aunque en compilación sí se haya garantizado que todos los usos de `T` son coherentes.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
En la solución sin *generics*, donde las coordenadas se declaran como `Number`, sí es posible asignar tipos distintos a cada coordenada, por ejemplo un `Integer` para `x` y un `Double` para `y`. Esto es válido porque ambos son subtipos de `Number`, y el compilador solo exige que cumplan esa relación de herencia. Sin embargo, esto introduce una pérdida de coherencia en el modelo, ya que un mismo punto puede mezclar tipos numéricos sin restricción.

En la solución con *generics* (`Punto<T extends Number>`), no es posible mezclar tipos distintos dentro del mismo punto. Al instanciar la clase, se fija un único tipo concreto para `T`, por ejemplo `Punto<Integer>` o `Punto<Double>`. Esto significa que ambas coordenadas deben ser del mismo tipo, y el compilador impide directamente crear algo como un punto con `Integer` en `x` y `Double` en `y`. Este refuerzo mejora la consistencia del modelo y evita errores conceptuales ya en tiempo de compilación.

Respecto al tipo de retorno de `getX`, en la versión sin generics el método devuelve `Number`, ya que es el tipo declarado del atributo. Esto obliga a realizar conversiones si se quiere trabajar con un tipo más específico (`Integer`, `Double`, etc.), perdiendo precisión en el control de tipos.

En la versión con generics, `getX` devuelve el tipo parametrizado `T`, es decir, el mismo tipo concreto con el que se instanció la clase. Por ejemplo, en `Punto<Integer>`, `getX` devuelve `Integer`. Esto elimina la necesidad de casts y permite que el compilador mantenga un control estricto sobre el tipo real utilizado en todo momento.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta
En este diseño inicial, el problema principal es que la interfaz `Punto` no distingue entre tipos concretos de punto. Esto obliga a que `distanciaA` reciba un `Punto` genérico, y en las implementaciones se tenga que comprobar manualmente el tipo con `instanceof` y realizar *downcasting*. Esto rompe el polimorfismo limpio y desplaza la seguridad de tipos al tiempo de ejecución.

Para resolverlo, se puede introducir **generics con recursión de tipos (F-bounded polymorphism)**, de forma que cada implementación de `Punto` trabaje únicamente con su propio tipo. Esto permite que el compilador garantice que solo se comparan puntos del mismo tipo, eliminando `instanceof` y casts.

```java id="pgen1"
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

En este caso, el parámetro de tipo `T` representa “el mismo tipo concreto de punto”, lo que fuerza coherencia entre el tipo del objeto receptor y el argumento del método.

```java id="pgen2"
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        double dx = x - p.x;
        double dy = y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

```java id="pgen3"
public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        double dx = x - p.x;
        double dy = y - p.y;
        double dz = z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
```

Con este enfoque, el compilador impide directamente comparar un `Punto2D` con un `Punto3D`, ya que no son del mismo tipo parametrizado. Esto elimina la necesidad de `instanceof` y evita errores en tiempo de ejecución.

En resumen, la introducción de generics convierte una verificación dinámica (en ejecución) en una verificación estática (en compilación), reforzando la seguridad de tipos y haciendo el diseño más limpio y extensible.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
No, en Java **`List<String>` no es subtipo de `List<Object>`**, aunque sí es cierto que `String` es subtipo de `Object`. En cambio, **`String[] sí es subtipo de `Object[]`**. Esta diferencia se debe a cómo se implementa la seguridad de tipos en arrays frente a los genéricos.

En el caso de los **arrays**, Java los hizo *covariantes* por diseño histórico: si `String` es un `Object`, entonces `String[]` puede tratarse como `Object[]`. Sin embargo, esto puede provocar un problema en tiempo de ejecución. Por ejemplo:

```java
Object[] arr = new String[10];
arr[0] = 123; // Error en tiempo de ejecución
```

El compilador lo permite porque `arr` es de tipo `Object[]`, pero en realidad el array es de `String[]`. En ejecución, la JVM detecta que se está intentando insertar un `Integer` en un array de `String` y lanza una `ArrayStoreException`. Este es un ejemplo de cómo la covarianza en arrays puede romper la seguridad de tipos en tiempo de ejecución.

En cambio, las **listas genéricas en Java son invariantes**. Esto significa que aunque `String` sea subtipo de `Object`, **`List<String>` no es subtipo de `List<Object>`**. El motivo es precisamente evitar el problema anterior: si se permitiera esa relación, se podría hacer algo como:

```java
List<String> lista = new ArrayList<>();
List<Object> ref = lista; // si esto fuera válido
ref.add(123); // rompería la seguridad de tipos
```

Esto se evita en compilación gracias a la invariancia de los genéricos, garantizando seguridad de tipos estática sin necesidad de comprobaciones en ejecución.

A partir de esto se definen tres relaciones:

* **Covarianza**: si `A` es subtipo de `B`, entonces `F<A>` es subtipo de `F<B>`. Ejemplo: arrays en Java (`String[]` ⊂ `Object[]`).
* **Contravarianza**: si `A` es subtipo de `B`, entonces `F<B>` es subtipo de `F<A>`. Se usa típicamente en tipos que solo “consumen” valores (por ejemplo, con `? super T`).
* **Invariancia**: no existe relación entre `F<A>` y `F<B>` aunque `A` sea subtipo de `B`. Ejemplo: `List<T>` en Java.

En resumen, los arrays son covariantes pero menos seguros en tiempo de ejecución, mientras que los genéricos son invariantes para garantizar seguridad de tipos en compilación y evitar errores como `ArrayStoreException` o *casts* incorrectos.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un **wildcard** en Java (`?`) es un comodín que representa un tipo desconocido dentro de los genéricos. Permite flexibilizar las restricciones de tipo cuando no es necesario fijar exactamente el parámetro genérico, sino trabajar con una familia de tipos relacionados. Su uso principal es recuperar de forma controlada la covarianza y la contravarianza en estructuras genéricas, que por defecto son invariantes.

La expresión `List<? extends T>` indica **covarianza controlada**: la lista puede contener elementos de un tipo que es `T` o cualquier subtipo de `T`. Se utiliza cuando interesa **leer datos de la estructura**, pero no añadir nuevos elementos (excepto `null`). Esto es porque el tipo exacto no se conoce, solo se sabe que es “algún subtipo de T”.

```java id="wild1"
import java.util.List;

public class Util {

    public static double suma(List<? extends Number> lista) {
        double total = 0;
        for (Number n : lista) {
            total += n.doubleValue();
        }
        return total;
    }
}
```

En este caso, se permite pasar `List<Integer>`, `List<Double>`, etc., porque todos son subtipos de `Number`. Sin embargo, no se puede hacer `lista.add(...)`, ya que el compilador no puede garantizar qué tipo concreto contiene la lista.

Por otro lado, `List<? super T>` indica **contravarianza controlada**: la lista puede contener elementos de tipo `T` o de cualquier supertipo de `T`. Se usa cuando interesa **escribir o insertar datos**, pero no se puede asumir con precisión el tipo al leer (solo se garantiza `Object`).

```java id="wild2"
import java.util.List;

public class Util2 {

    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(1);
        lista.add(2);
        lista.add(3);
    }
}
```

Aquí se puede pasar `List<Integer>`, `List<Number>` o `List<Object>`, porque todos pueden aceptar `Integer`. En cambio, al leer elementos solo se obtiene `Object`, ya que no se conoce el tipo más específico.

En resumen, `? extends T` se usa para **consumir datos de forma segura (lectura)**, mientras que `? super T` se usa para **producir datos de forma segura (escritura)**. Esta regla se resume comúnmente como *PECS*: *Producer Extends, Consumer Super*.
