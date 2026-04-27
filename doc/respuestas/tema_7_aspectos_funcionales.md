<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
Un puntero a una función es una variable que no almacena un dato normal, sino la dirección de memoria de una función. De forma parecida a como un puntero común puede apuntar a una variable entera o a una estructura, un puntero a función permite apuntar a una función concreta para poder invocarla de manera indirecta. Esto resulta útil cuando se quiere decidir en tiempo de ejecución qué función ejecutar, o cuando se desea pasar una función como parámetro a otra.

En C, la declaración de un puntero a función debe coincidir exactamente con la firma de la función a la que apunta, es decir, con su tipo de retorno y con los parámetros que recibe. Si la función recibe una cadena de caracteres (`char *`) y devuelve también una cadena (`char *`), el puntero deberá declararse con esa misma forma. Después, basta asignarle el nombre de la función y usarlo como si fuese una llamada normal.

```c
#include <stdio.h>
#include <ctype.h>

char* convertirMayusculas(char* texto) {
    int i = 0;

    while (texto[i] != '\0') {
        texto[i] = toupper((unsigned char) texto[i]);
        i++;
    }

    return texto;
}

int main() {
    char cadena[] = "hola mundo";

    /* Puntero a función */
    char* (*aMayusculas)(char*);

    /* Asignación de la función al puntero */
    aMayusculas = convertirMayusculas;

    /* Invocación mediante el puntero */
    printf("%s\n", aMayusculas(cadena));

    return 0;
}
```

En este ejemplo, `aMayusculas` almacena la dirección de la función `convertirMayusculas`. Al ejecutar `aMayusculas(cadena)`, realmente se está llamando a esa función a través del puntero. La cadena original se modifica directamente porque en C las cadenas se manejan mediante arrays de caracteres y se trabaja sobre su memoria de forma directa.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda es una función anónima, es decir, una función que puede definirse sin necesidad de asignarle un nombre propio como ocurre con los métodos tradicionales. Su objetivo principal es permitir expresar comportamientos de forma más breve y directa, especialmente cuando una función se va a usar como valor, se va a pasar como parámetro o se va a guardar en una variable. En programación funcional, las funciones se tratan como datos, por lo que pueden asignarse, devolverse o enviarse como argumento.

A diferencia del puntero a función en C, donde se almacena la dirección de una función ya existente, en lenguajes como JavaScript o Java puede definirse directamente la lógica mediante una lambda y asignarla a una variable local. La variable `aMayusculas` actuará como referencia a esa función. Después, bastará invocarla con una cadena para obtener el resultado en mayúsculas.

```javascript
function main() {
    let aMayusculas = (texto) => {
        return texto.toUpperCase();
    };

    console.log(aMayusculas("hola mundo"));
}

main();
```

En Java, las funciones lambda se apoyan en interfaces funcionales, es decir, interfaces que contienen un único método abstracto. La interfaz `Function<T, R>` del paquete `java.util.function` representa una función que recibe un valor de tipo `T` y devuelve otro de tipo `R`. En este caso, se utiliza `Function<String, String>` porque se recibe una cadena y se devuelve otra.

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = texto -> {
            return texto.toUpperCase();
        };

        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
```

En ambos ejemplos, `aMayusculas` contiene una referencia a una función lambda que transforma una cadena en mayúsculas. La diferencia principal es que en JavaScript las funciones son elementos nativos del lenguaje, mientras que en Java se necesita una interfaz funcional para representar esa referencia de forma tipada.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El paradigma funcional es un estilo de programación en el que el elemento principal no son los objetos ni los procedimientos, sino las funciones. Se basa en construir programas mediante la composición de funciones, intentando que estas transformen datos de entrada en datos de salida sin modificar el estado externo. Se busca que una función produzca siempre el mismo resultado para los mismos argumentos y que tenga el menor número posible de efectos secundarios, como modificar variables globales o alterar estructuras compartidas.

Este enfoque contrasta con la programación orientada a objetos, donde el diseño suele centrarse en clases, objetos y cambios de estado. En el paradigma funcional, interesa más describir qué transformación se desea realizar que cómo se modifica paso a paso la memoria. Por ejemplo, en lugar de recorrer manualmente una colección con bucles y variables auxiliares, puede aplicarse una función que filtre, transforme o combine los elementos de forma declarativa.

A lenguajes como Java 8 se les llama multi-paradigma porque permiten usar más de un estilo de programación dentro del mismo lenguaje. Java sigue siendo principalmente orientado a objetos, ya que trabaja con clases, herencia y polimorfismo, pero desde Java 8 también incorpora herramientas propias del paradigma funcional, como expresiones lambda, referencias a métodos y la API de *streams*. Esto permite elegir el enfoque más adecuado según el problema.

Decir que las funciones son “ciudadanos de primera clase” significa que pueden tratarse como cualquier otro valor del lenguaje. Es decir, pueden guardarse en variables, pasarse como parámetro a otras funciones, devolverse como resultado e incluso construirse dinámicamente. En C esto se consigue mediante punteros a función, mientras que en Java moderno se logra mediante lambdas e interfaces funcionales. Esta capacidad es una de las bases más importantes de la programación funcional.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
En Java, una función lambda es una forma compacta de representar una implementación de una interfaz funcional, es decir, una interfaz que contiene un único método abstracto. En lugar de crear una clase completa o una clase anónima para implementar ese método, puede escribirse directamente la lógica mediante una expresión lambda. Esto reduce código repetitivo y hace más clara la intención del programa.

La sintaxis básica es `(parámetros) -> expresión` o `(parámetros) -> { bloque de código }`. A la izquierda de `->` se colocan los parámetros de entrada y a la derecha el comportamiento que se desea ejecutar. Si la lambda contiene una única expresión simple, puede escribirse sin llaves; si contiene varias instrucciones, deben usarse llaves y, si corresponde, la sentencia `return`. El tipo de los parámetros normalmente no es necesario indicarlo porque Java puede inferirlo a partir del contexto.

```java id="m9k3x2"
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas =
                texto -> texto.toUpperCase();

        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
```

En este ejemplo, `Function<String, String>` indica que la función recibe un `String` y devuelve otro `String`. La variable `aMayusculas` almacena la lambda `texto -> texto.toUpperCase()`, donde `texto` es el parámetro y `texto.toUpperCase()` es el valor devuelto. La ejecución se realiza mediante el método `apply()`, que es el método abstracto definido en la interfaz funcional `Function`.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Recibir una función como parámetro significa que un método puede delegar parte de su comportamiento en una función externa. En lugar de escribir dentro del propio método toda la lógica de transformación, se recibe una función que será la encargada de realizar esa tarea. Esto permite reutilizar el mismo método con distintos comportamientos sin necesidad de duplicar código.

En este caso, el método `transformar` recibe primero una cadena y después una función transformadora como `aMayusculas`. Desde dentro del método se invoca esa función sobre el texto recibido y se devuelve el resultado. Esta idea es una de las bases de la programación funcional, ya que permite tratar las funciones como valores que pueden pasarse entre métodos.

```javascript id="js47t2"
function transformar(texto, transformadora) {
    return transformadora(texto);
}

function main() {
    let aMayusculas = (texto) => {
        return texto.toUpperCase();
    };

    console.log(transformar("hola mundo", aMayusculas));
}

main();
```

En JavaScript, las funciones son elementos nativos del lenguaje y pueden pasarse directamente como argumentos. El método `transformar` recibe la cadena y la función `transformadora`, y la ejecuta con `transformadora(texto)`. Esto permite que el mismo método pueda usarse con cualquier otra función que reciba una cadena y devuelva otra.

```java id="jv84n1"
import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas =
                texto -> texto.toUpperCase();

        System.out.println(
                transformar("hola mundo", aMayusculas)
        );
    }
}
```

En Java, se utiliza `Function<String, String>` para representar la función recibida como parámetro. El método `transformar` invoca esa función mediante `apply()`, que es el método abstracto de la interfaz funcional. De esta forma, el método queda desacoplado de una transformación concreta y puede reutilizarse con cualquier lambda compatible.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Además de guardar una función lambda en una variable como `aMayusculas`, también es posible definirla directamente en el momento de la llamada a un método. Esto resulta útil cuando la función solo se va a utilizar una vez y no tiene sentido declararla previamente. De esta forma, el código puede resultar más compacto y la intención queda visible justo en el punto donde se usa.

En este caso, el método `transformar` se invoca pasando una nueva lambda que invierte la cadena recibida. La función se define como argumento del propio método, sin almacenarla antes en una variable local. Así, `transformar` sigue siendo genérico y puede trabajar con cualquier transformación compatible.

```javascript id="js92k4"
function transformar(texto, transformadora) {
    return transformadora(texto);
}

function main() {
    console.log(
        transformar("hola mundo", (texto) => {
            return texto.split("").reverse().join("");
        })
    );
}

main();
```

En JavaScript, la lambda se escribe directamente como segundo parámetro de `transformar`. La expresión `split("")` separa la cadena en caracteres, `reverse()` invierte el orden y `join("")` vuelve a construir la cadena final. No es necesario declarar previamente una variable como `aMayusculas` porque la función solo se utiliza en esa llamada.

```java id="jv35p8"
import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        System.out.println(
            transformar(
                "hola mundo",
                texto -> new StringBuilder(texto)
                            .reverse()
                            .toString()
            )
        );
    }
}
```

En Java, la lambda también se define directamente en la llamada a `transformar`. Se utiliza `StringBuilder` porque la clase `String` es inmutable y no dispone de un método directo para invertir el contenido. La lambda recibe `texto`, crea una versión invertida y la devuelve, manteniendo la misma estructura funcional que en el ejemplo de JavaScript.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un cierre o *closure* es una función que, además de su propio código, conserva el acceso al contexto donde fue creada. Esto significa que una función lambda puede utilizar variables locales definidas fuera de ella, aunque esas variables no formen parte de sus parámetros. La lambda “recuerda” ese entorno y puede usar esos valores cuando se ejecuta más tarde.

En Java, esto también ocurre con las expresiones lambda, aunque con una restricción importante: las variables locales externas que se usan dentro de la lambda deben ser finales o efectivamente finales, es decir, no deben modificarse después de su inicialización. Esto permite que Java garantice un comportamiento seguro y predecible. Gracias a esto, una lambda puede apoyarse en información del contexto sin necesidad de recibirla explícitamente como parámetro.

```java id="jv61m7"
import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = " desde Java";

        Function<String, String> concatenar =
                texto -> texto + sufijo;

        System.out.println(
                transformar("Hola mundo", concatenar)
        );
    }
}
```

En este ejemplo, la variable local `sufijo` está definida dentro de `main`, fuera de la función lambda. Sin embargo, la lambda `texto -> texto + sufijo` puede acceder a ella y utilizarla para construir el resultado final. Esto demuestra el concepto de cierre: la función conserva acceso al entorno donde fue definida. Como `sufijo` no cambia después de declararse, cumple la condición de ser efectivamente final y puede usarse dentro de la lambda sin problema.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
Aunque ambos mecanismos permiten trabajar con funciones de forma indirecta, una función lambda y un puntero a función en C no son exactamente lo mismo. Un puntero a función simplemente almacena la dirección de memoria de una función ya existente. Su finalidad principal es permitir invocar esa función de forma dinámica o pasarla como parámetro, pero no añade comportamiento adicional ni conserva información del contexto donde se usa.

Una función lambda, en cambio, no solo representa una función, sino también una forma más flexible de definir comportamiento en el propio lugar donde se necesita. Puede escribirse sin nombre, asignarse directamente a una variable o pasarse como argumento en la misma llamada. Además, una lambda puede capturar variables del entorno donde fue creada, formando un cierre (*closure*). Esto significa que no solo se guarda la lógica de la función, sino también parte de su contexto.

En C, un puntero a función no recuerda variables locales externas. Si una función necesita datos adicionales, estos deben pasarse explícitamente como parámetros o gestionarse mediante variables globales. En Java, una lambda puede acceder a variables locales efectivamente finales sin necesidad de incluirlas en la lista de parámetros, lo que hace el código más expresivo y cercano al paradigma funcional.

Por tanto, puede decirse que el puntero a función en C es un mecanismo más básico y de bajo nivel, centrado en direcciones de memoria, mientras que la función lambda es una abstracción de más alto nivel orientada a expresar comportamiento y composición de funciones. Ambos permiten tratar funciones como valores, pero las lambdas ofrecen mayor integración con el paradigma funcional y mayor capacidad de abstracción.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Cuando una función puede devolver otra función como resultado, se refuerza la idea de que las funciones pueden tratarse como valores. En este caso, `crearDescuento(porcentaje)` no devuelve directamente un número, sino una función que más adelante podrá aplicarse sobre una cantidad. Esa función devuelta representará una operación concreta de descuento, por ejemplo, aplicar un 10% o un 25% sobre un precio.

La ventaja de este enfoque es que permite construir funciones especializadas a partir de una función más general. En lugar de escribir una función distinta para cada tipo de descuento, se crea una única función generadora. Cada vez que se invoca `crearDescuento`, se obtiene una nueva lambda configurada con el porcentaje indicado. Esto es un ejemplo claro de programación funcional y también de uso de cierres (*closures*).

```java id="jv48n3"
import java.util.function.Function;

public class Main {

    public static Function<Double, Double> crearDescuento(
            double porcentaje) {

        return cantidad ->
                cantidad - (cantidad * porcentaje / 100.0);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 =
                crearDescuento(10);

        Function<Double, Double> descuento25 =
                crearDescuento(25);

        double precio = 200.0;

        System.out.println(
                "Con 10%: " + descuento10.apply(precio)
        );

        System.out.println(
                "Con 25%: " + descuento25.apply(precio)
        );
    }
}
```

La *closure* aparece porque la lambda que se devuelve utiliza la variable `porcentaje`, aunque dicha variable pertenece al método `crearDescuento` y no a la propia lambda. Cuando la función se crea, la lambda conserva ese valor y lo recuerda incluso después de que el método haya terminado. Así, `descuento10` mantiene internamente el valor `10`, mientras que `descuento25` conserva `25`. Cada función devuelta tiene su propio contexto capturado, lo que permite que ambas se comporten de forma distinta aunque provengan del mismo método.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
En Java, una función lambda no existe como un elemento aislado con un tipo propio, sino que siempre debe asociarse a un tipo conocido por el compilador. Ese tipo es una interfaz funcional. Una interfaz funcional es una interfaz diseñada para representar una única operación abstracta, de manera que una expresión lambda pueda usarse como implementación de esa operación. Por ejemplo, `Function<String, String>` representa una función que recibe un `String` y devuelve otro `String`.

La idea es que la lambda sustituye la necesidad de crear una clase que implemente manualmente esa interfaz. En lugar de escribir una clase completa con un método, basta con proporcionar directamente el cuerpo de ese método mediante la expresión lambda. Por eso, cuando se escribe `texto -> texto.toUpperCase()`, Java necesita saber a qué interfaz funcional corresponde esa expresión para poder determinar los tipos de entrada y salida.

El requisito principal de una interfaz funcional es que debe tener exactamente un único método abstracto. Si tuviera dos o más métodos abstractos, Java no sabría cuál de ellos está implementando la lambda. Sin embargo, sí puede contener métodos `default`, métodos `static` e incluso métodos heredados de `Object`, porque estos no cuentan como métodos abstractos adicionales. También es habitual marcarla con la anotación `@FunctionalInterface`, aunque no es obligatorio.

Esta anotación sirve como ayuda para el compilador y para el programador, ya que verifica que realmente se cumple la condición de tener un solo método abstracto. Si accidentalmente se añade otro método abstracto, el compilador mostrará un error. Algunas interfaces funcionales muy usadas ya están incluidas en Java, como `Function`, `Predicate`, `Consumer` y `Supplier`, que facilitan el uso de lambdas sin necesidad de definir nuevas interfaces.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Una interfaz funcional propia permite definir de forma explícita el tipo que tendrá una función lambda dentro del programa. Aunque Java ya proporciona interfaces como `Function<T, R>`, en muchos casos resulta más claro crear una interfaz con un nombre relacionado con el problema que se está resolviendo. En este caso, `Transformador` representará cualquier operación capaz de recibir una cadena y devolver otra cadena transformada.

Para que sea una interfaz funcional válida, debe contener un único método abstracto. Ese método será el que implementarán las expresiones lambda asociadas a ella. La anotación `@FunctionalInterface` no es obligatoria, pero se recomienda porque ayuda a comprobar que la interfaz sigue cumpliendo esta condición y evita errores si más adelante se añaden métodos abstractos adicionales.

```java id="jv72f5"
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

En este ejemplo, la interfaz `Transformador` define un único método abstracto llamado `transformar`, que recibe un `String` y devuelve otro `String`. Cualquier lambda compatible con esa firma podrá asignarse a una variable de este tipo. Por ejemplo, una lambda como `texto -> texto.toUpperCase()` sería una implementación válida de esta interfaz.

De esta forma, en lugar de usar `Function<String, String>`, puede trabajarse con un tipo más expresivo y cercano al dominio del problema. Esto mejora la legibilidad del código, ya que el nombre `Transformador` describe mejor la intención que una interfaz genérica reutilizable para muchos casos distintos.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Para hacer la interfaz funcional más reutilizable, puede emplearse genericidad en lugar de fijar directamente el tipo `String`. De esta forma, la misma interfaz podrá servir para transformar cualquier tipo de dato en otro distinto. Igual que ocurre con `Function<T, R>` de la biblioteca estándar, se definen dos parámetros genéricos: uno para el tipo de entrada y otro para el tipo de salida.

Así, `Transformador<T, R>` representará una operación que recibe un valor de tipo `T` y devuelve otro de tipo `R`. Esto permite usar la misma estructura para transformar cadenas, números o cualquier otro objeto sin necesidad de crear una interfaz distinta para cada caso. Se mantiene la idea de interfaz funcional porque sigue existiendo un único método abstracto.

```java id="jv83m2"
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```

Un ejemplo sencillo consiste en transformar un `Double` en un `Integer`, redondeando el valor decimal al entero más cercano. La lambda recibirá un número real y devolverá el resultado de aplicar `Math.round()`, convertido posteriormente a `Integer`.

```java id="jv19k6"
public class Main {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
                numero -> (int) Math.round(numero);

        System.out.println(
                redondear.transformar(15.7)
        );
    }
}
```

En este caso, `Transformador<Double, Integer>` indica que la entrada será un `Double` y la salida un `Integer`. La lambda `numero -> (int) Math.round(numero)` implementa ese comportamiento. Así, la misma interfaz genérica puede reutilizarse en muchos contextos distintos sin necesidad de redefinir nuevas interfaces funcionales para cada transformación concreta.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Java incluye varias interfaces funcionales predefinidas en el paquete `java.util.function`, pensadas para cubrir los casos más habituales al trabajar con funciones lambda. En lugar de crear una interfaz propia para cada situación, normalmente se reutilizan estas interfaces estándar, ya que representan operaciones comunes como transformar valores, consumir datos, comprobar condiciones o generar resultados.

La más conocida es `Function<T, R>`, que representa una función que recibe un valor de tipo `T` y devuelve otro de tipo `R`, por lo que es equivalente al `Transformador<T, R>` definido anteriormente. Junto a ella aparecen otras interfaces muy utilizadas como `Predicate<T>`, que recibe un valor y devuelve un `boolean`; `Consumer<T>`, que recibe un valor pero no devuelve nada; y `Supplier<T>`, que no recibe parámetros y produce un valor como resultado.

```java id="jv56p4"
Function<String, Integer> longitud =
        texto -> texto.length();

Predicate<Integer> esPar =
        numero -> numero % 2 == 0;

Consumer<String> imprimir =
        texto -> System.out.println(texto);

Supplier<Double> aleatorio =
        () -> Math.random();
```

También existen versiones especializadas para tipos primitivos, como `IntFunction`, `DoubleConsumer`, `LongSupplier` o `BiFunction<T, U, R>`, que permite trabajar con dos parámetros de entrada. Estas variantes evitan conversiones innecesarias entre tipos primitivos y objetos (`boxing` y `unboxing`), mejorando la eficiencia.

El uso de estas interfaces predefinidas hace que el código sea más uniforme y fácil de entender, ya que forman parte de la biblioteca estándar de Java. Además, se integran directamente con otras herramientas como la API de *streams*, donde aparecen constantemente en operaciones como `map`, `filter`, `forEach` o `reduce`.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
El método `forEach` de `List` permite recorrer una colección aplicando una función a cada uno de sus elementos. Puede verse como una versión más funcional del bucle `for`, ya que en lugar de escribir explícitamente el recorrido con índices o iteradores, se indica directamente qué debe hacerse con cada elemento. Esto hace que el código sea más declarativo y que la atención se centre en la operación, no en el mecanismo de recorrido.

`forEach` recibe como parámetro una función lambda compatible con la interfaz funcional `Consumer<T>`, ya que consume cada elemento de la lista y no devuelve ningún valor. En este caso, se recorrerá una lista de `Integer` y se mostrará un mensaje únicamente cuando el número sea positivo. La comprobación se realiza dentro de la propia lambda.

```java id="jv64r1"
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros =
                Arrays.asList(-3, 7, 0, 12, -1, 5);

        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println(
                        numero + " es positivo"
                );
            }
        });
    }
}
```

En este ejemplo, `forEach` recorre automáticamente todos los elementos de la lista `numeros`. La lambda `numero -> { ... }` recibe cada entero uno a uno, y si su valor es mayor que cero, se imprime el mensaje correspondiente. No es necesario escribir un `for (int i = 0; ...)` ni acceder manualmente a cada posición.

Este estilo resulta especialmente útil cuando la operación sobre cada elemento es breve y clara. Además, se integra de forma natural con otras herramientas funcionales de Java, como `stream()`, `filter()` o `map()`, donde las lambdas permiten construir transformaciones más complejas sobre colecciones de datos.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
En la firma de `forEach`, el parámetro aparece como `Consumer<? super T>` en lugar de `Consumer<T>` para hacer el método más flexible. `forEach` va a entregar elementos de tipo `T` a una función consumidora, por lo que no es necesario exigir exactamente un `Consumer<T>`, sino cualquier consumidor capaz de aceptar objetos de tipo `T` o de un tipo más general. Por ejemplo, si se tiene una lista de `Integer`, también debería poder usarse un `Consumer<Number>` o incluso un `Consumer<Object>`, porque ambos pueden consumir enteros sin problema.

Esta idea se resume en la regla **PECS**, que significa *Producer Extends, Consumer Super*. La regla indica que si una estructura produce valores que se van a leer, conviene usar `extends`; si consume valores que se le van a pasar, conviene usar `super`. En otras palabras: “si produce, usa `? extends`; si consume, usa `? super`”. Esto ayuda a decidir cómo declarar tipos genéricos con comodines (`wildcards`) de forma segura y más reutilizable.

En `forEach`, el `Consumer` consume elementos de la lista, por eso se escribe `Consumer<? super T>`. No tendría sentido usar `? extends T`, porque eso indicaría que el consumidor trabaja con un subtipo más específico y podría no aceptar todos los elementos de la lista. En cambio, con `? super T`, se garantiza que cualquier elemento de tipo `T` podrá pasarse correctamente a la función.

Algo parecido ocurre al mejorar el método `transformar`. Si se define como `Function<String, String>`, solo se acepta exactamente ese tipo. Pero puede hacerse más flexible usando `Function<? super String, ? extends String>`. Así, la función transformadora podrá recibir un `String` o un tipo más general como `Object`, y devolver un `String` o un subtipo compatible. Se aplica PECS porque el parámetro de entrada se consume (*Consumer → super*) y el valor devuelto se produce (*Producer → extends*).

```java id="jv91t4"
public static String transformar(
        String texto,
        Function<? super String, ? extends String> transformadora) {
    return transformadora.apply(texto);
}
```

Con esta versión, el método acepta más tipos de funciones sin perder seguridad de tipos. Esto mejora la reutilización del código y muestra cómo la regla PECS no solo se aplica a colecciones, sino también a interfaces funcionales cuando se trabaja con programación funcional y genericidad en Java.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Una referencia a método permite usar un método ya existente como si fuese una función que puede guardarse en una variable, pasarse como parámetro o invocarse más tarde. Es una idea muy relacionada con las funciones lambda, pero en lugar de escribir una nueva lambda, se reutiliza directamente un método que ya está definido. Esto hace que el código sea más breve y, en muchos casos, más expresivo.

La referencia puede apuntar a métodos de instancia, como los de un objeto concreto, o a métodos estáticos de una clase. En este ejemplo se utilizará un método de instancia llamado `saludar` dentro de la clase `Persona`. Se creará un objeto con un nombre y después se guardará una referencia a ese método en una variable local para invocarlo posteriormente.

```javascript id="js58n2"
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        return "Hola, soy " + this.nombre;
    }
}

function main() {
    let persona = new Persona("Ana");

    let saludar = persona.saludar.bind(persona);

    console.log(saludar());
}

main();
```

En JavaScript, al obtener una referencia a un método de objeto, es importante conservar el contexto de `this`. Por eso se utiliza `bind(persona)`, que crea una nueva función donde `this` queda asociado al objeto `persona`. Así, al ejecutar `saludar()`, el método sigue teniendo acceso correcto al atributo `nombre`.

```java id="jv27q6"
import java.util.function.Supplier;

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola, soy " + nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");

        Supplier<String> saludar = persona::saludar;

        System.out.println(saludar.get());
    }
}
```

En Java, la sintaxis `persona::saludar` representa una referencia al método de instancia `saludar` del objeto `persona`. Se usa `Supplier<String>` porque el método no recibe parámetros y devuelve un `String`. La invocación se realiza mediante `get()`, que es el método abstracto de esa interfaz funcional. Esta sintaxis evita escribir una lambda equivalente como `() -> persona.saludar()`.



## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
En Java, una referencia a método es una forma abreviada de expresar una lambda que simplemente llama a un método ya existente. Su sintaxis general es `objeto::metodo`, `Clase::metodo` o `Clase::new`, según el caso. En lugar de escribir una lambda como `x -> metodo(x)`, puede utilizarse una referencia más directa y legible cuando no se necesita lógica adicional.

Existen cuatro tipos principales de referencias a método: referencia a método estático, referencia a constructor, referencia a método de instancia de un objeto concreto y referencia a método de instancia sobre cualquier objeto de un tipo determinado. Todas ellas se apoyan en interfaces funcionales, ya que la referencia debe encajar con la firma del único método abstracto de esa interfaz.

```java id="jv44w8"
import java.util.function.Function;
import java.util.function.Supplier;

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola, soy " + nombre;
    }

    public static Integer longitud(String texto) {
        return texto.length();
    }
}

public class Main {
    public static void main(String[] args) {

        // 1. Método estático
        Function<String, Integer> refEstatica =
                Persona::longitud;

        // 2. Constructor
        Function<String, Persona> refConstructor =
                Persona::new;

        // 3. Método de instancia de un objeto concreto
        Persona persona = new Persona("Ana");
        Supplier<String> refInstanciaConcreta =
                persona::saludar;

        // 4. Método de instancia sobre cualquier instancia
        Function<Persona, String> refCualquierInstancia =
                Persona::saludar;

        System.out.println(refEstatica.apply("Hola"));
        System.out.println(
                refConstructor.apply("Luis").saludar()
        );
        System.out.println(refInstanciaConcreta.get());
        System.out.println(
                refCualquierInstancia.apply(persona)
        );
    }
}
```

La referencia a método estático usa `Clase::metodo`, como `Persona::longitud`. La referencia a constructor usa `Clase::new`, permitiendo crear objetos como si fuese una función. La referencia a método de una instancia concreta usa `objeto::metodo`, como `persona::saludar`, donde el objeto ya está fijado. Finalmente, la referencia a método de cualquier instancia usa `Clase::metodo`, pero aplicada a métodos no estáticos, como `Persona::saludar`, donde la propia instancia se convierte en el primer parámetro implícito de la función.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
El método `Collections.sort` permite ordenar una lista indicando un criterio de comparación mediante un `Comparator`. Este comparador recibe dos objetos y debe devolver un valor negativo, cero o positivo según el orden deseado. Gracias a las funciones lambda, no es necesario crear una clase aparte que implemente `Comparator`, sino que puede definirse la lógica directamente en la llamada al método.

En este caso, se quiere ordenar una lista de `Persona` primero por edad y, si dos personas tienen la misma edad, por orden alfabético del nombre. Esto puede hacerse de forma manual comparando los valores paso a paso, o utilizando los métodos auxiliares que ofrece la propia interfaz `Comparator`, que hacen el código más expresivo y reutilizable.

```java id="jv31h9"
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();

        personas.add(new Persona("Carlos", 30));
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Luis", 30));
        personas.add(new Persona("Bea", 25));

        // Comparación manual
        Collections.sort(personas, (p1, p2) -> {
            if (p1.getEdad() != p2.getEdad()) {
                return p1.getEdad() - p2.getEdad();
            }
            return p1.getNombre()
                     .compareTo(p2.getNombre());
        });

        System.out.println(personas);
    }
}
```

En esta primera versión, la lambda compara primero la edad de ambas personas. Si son distintas, devuelve la diferencia entre ellas. Si la edad coincide, se utiliza `compareTo()` sobre el nombre para ordenar alfabéticamente. Es una solución válida, aunque la lógica queda escrita manualmente dentro de la lambda.

```java id="jv68k2"
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();

        personas.add(new Persona("Carlos", 30));
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Luis", 30));
        personas.add(new Persona("Bea", 25));

        // Usando Comparator
        Collections.sort(
            personas,
            Comparator.comparing(Persona::getEdad)
                      .thenComparing(Persona::getNombre)
        );

        System.out.println(personas);
    }
}
```

En esta segunda versión, se utiliza `Comparator.comparing()` para indicar que el primer criterio de ordenación será la edad, y `thenComparing()` para añadir el segundo criterio sobre el nombre. Esta forma es más declarativa, más fácil de leer y menos propensa a errores, especialmente cuando los criterios de ordenación se vuelven más complejos.
