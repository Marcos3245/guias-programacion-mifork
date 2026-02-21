<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
La **encapsulación** y la **ocultación de información** buscan proteger el estado interno de los objetos y controlar cómo se accede y modifica. La encapsulación consiste en agrupar datos (atributos) y métodos dentro de una clase, mientras que la ocultación de información implica restringir el acceso directo a esos datos desde el exterior. En la práctica, esto se consigue declarando atributos como `private` o `protected` y proporcionando métodos públicos controlados para acceder o modificarlos.

El objetivo principal es evitar que otras partes del programa manipulen directamente los atributos internos de un objeto de forma incorrecta o incoherente. En lugar de permitir acceso libre, la clase define una interfaz pública clara que indica qué operaciones están permitidas y bajo qué condiciones. Esto hace que el uso de los objetos sea más seguro y predecible, ya que cualquier cambio en su estado pasa por métodos que pueden validar o controlar la información.

Algunas ventajas de la ocultación de información son:

* **Mayor seguridad**: se evita que los datos internos se modifiquen de forma incorrecta desde fuera de la clase.
* **Facilidad de mantenimiento**: se puede cambiar la implementación interna sin afectar al resto del programa si la interfaz pública se mantiene.
* **Reducción del acoplamiento**: otras partes del programa dependen solo de la interfaz, no de los detalles internos.
* **Mayor claridad y control**: se definen claramente las responsabilidades de cada clase y cómo debe utilizarse.

En conjunto, la encapsulación y la ocultación de información permiten construir programas más robustos, modulares y fáciles de mantener, ya que cada clase controla su propio estado y comportamiento.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La **interfaz pública** de una clase u objeto es el conjunto de métodos y atributos accesibles desde fuera de la clase. Representa todo lo que otros objetos o partes del programa pueden usar para interactuar con esa clase. Normalmente está formada por los métodos declarados como `public`, que definen las operaciones permitidas sobre los objetos y constituyen la forma oficial de utilizarlos.

La interfaz pública actúa como un contrato: indica qué puede hacerse con un objeto sin revelar cómo está implementado internamente. Por ejemplo, una clase `CuentaBancaria` puede ofrecer métodos como `ingresar()`, `retirar()` o `consultarSaldo()`. El usuario de la clase no necesita saber cómo se almacena el saldo ni cómo se realizan internamente los cálculos; solo necesita conocer los métodos disponibles y cómo utilizarlos.

Esta idea está directamente relacionada con la **ocultación de información**. Mientras la interfaz pública muestra lo necesario para usar el objeto, la implementación interna (atributos privados y métodos internos) permanece oculta. Gracias a ello, se pueden modificar detalles internos de la clase sin afectar al código que la utiliza, siempre que la interfaz pública se mantenga estable. Esto mejora la modularidad, la seguridad y el mantenimiento del software.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La **interfaz pública** de una clase debe diseñarse con cuidado porque define cómo el resto del programa va a interactuar con esa clase. Una vez que otros módulos o programadores empiezan a usarla, esa interfaz se convierte en un contrato estable. Si está mal diseñada, puede provocar errores de uso, acoplamiento excesivo o dificultades para mantener y ampliar el sistema.

No es fácil cambiar la interfaz pública una vez que el código depende de ella. Cualquier modificación en los métodos públicos, sus nombres o parámetros puede obligar a modificar todas las partes del programa que la utilizan. En proyectos grandes, esto puede afectar a muchos archivos y generar errores. Por ello, se intenta que la interfaz pública sea clara, coherente y estable desde el principio.

Diseñar bien la interfaz pública permite ocultar la implementación interna y mantener flexibilidad para cambiarla sin afectar al exterior. En cambio, una mala interfaz limita la evolución del software y hace que cualquier cambio sea costoso y arriesgado. Por eso se considera una de las decisiones más importantes en el diseño orientado a objetos.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las **invariantes de clase** son condiciones o propiedades que deben cumplirse siempre para que un objeto de esa clase esté en un estado válido. Son reglas internas que garantizan la coherencia de los datos del objeto a lo largo de su vida. Por ejemplo, en una clase `CuentaBancaria`, una invariante podría ser que el saldo nunca sea negativo, o en una clase `Punto`, que las coordenadas sean valores válidos dentro de un determinado rango.

Estas invariantes deben mantenerse después de construir el objeto y tras cada operación pública que pueda modificar su estado. Es responsabilidad de los métodos de la clase asegurarse de que dichas condiciones se cumplen. Si una operación violara una invariante, el objeto quedaría en un estado incorrecto y podría provocar errores en el programa.

La **ocultación de información** ayuda a mantener las invariantes porque impide que el código externo modifique directamente los atributos del objeto. Si los atributos son privados, solo los métodos de la clase pueden cambiarlos, y estos métodos pueden comprobar que cualquier modificación respeta las reglas establecidas. Así se evita que desde fuera se asignen valores inválidos sin control.

En resumen, la ocultación de información protege las invariantes de clase al centralizar el control de los datos dentro de la propia clase. Esto garantiza que el objeto se mantenga siempre en un estado coherente y facilita la creación de programas más robustos y fiables.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
Un ejemplo de clase `Punto` en Java que haga uso de la **ocultación de información** sería el siguiente:

```java
public class Punto {
    private double x;
    private double y;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método público
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Métodos de acceso (opcional)
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}
```

En este diseño, los atributos `x` e `y` están declarados como `private`, lo que significa que no pueden ser accedidos directamente desde fuera de la clase. Solo pueden consultarse o modificarse mediante métodos públicos definidos por la propia clase. Esto garantiza que el estado del objeto esté controlado y se mantenga coherente.

La **interfaz pública** de la clase `Punto` está formada por el constructor `Punto(double x, double y)` y los métodos públicos `calcularDistanciaAOrigen()`, `getX()` y `getY()`. Estos son los únicos elementos accesibles desde fuera de la clase y constituyen la forma oficial de interactuar con un objeto `Punto`. Los atributos `x` e `y` no forman parte de la interfaz pública porque están ocultos.

La palabra clave `public` indica que un atributo o método es accesible desde cualquier parte del programa. En cambio, `private` significa que solo puede utilizarse dentro de la propia clase. Gracias a esta distinción, se puede exponer únicamente lo necesario (interfaz pública) y mantener ocultos los detalles internos, favoreciendo la seguridad, el mantenimiento y la claridad del diseño.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores de acceso `public` y `private` se aplican principalmente a los **miembros de una clase** y a las propias clases. Permiten controlar desde qué partes del programa se puede acceder a cada elemento, lo cual es fundamental para la encapsulación y la ocultación de información en la programación orientada a objetos.

Estos modificadores pueden aplicarse a **atributos (variables de instancia o de clase)**, **métodos** y **constructores**. Si un atributo o método se declara `public`, puede ser utilizado desde cualquier otra clase del programa. Si se declara `private`, solo puede ser utilizado dentro de la propia clase donde se define. Esto permite, por ejemplo, mantener los atributos ocultos y ofrecer acceso controlado mediante métodos públicos.

También pueden aplicarse a las **clases**. Una clase declarada `public` es accesible desde cualquier paquete del programa, mientras que una clase sin modificador (visibilidad por defecto) solo es accesible dentro de su propio paquete. Sin embargo, las clases no pueden declararse `private` a nivel superior (fuera de otra clase); solo pueden ser `private` si están definidas como clases internas dentro de otra clase.

En resumen, `public` y `private` se usan para controlar el acceso a clases, atributos, métodos y constructores. Su uso adecuado permite diseñar interfaces públicas claras y mantener ocultos los detalles internos, facilitando el mantenimiento y la seguridad del código.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
En programación orientada a objetos no solo existen visibilidades pública y privada; en muchos lenguajes hay **niveles intermedios de acceso** que permiten un control más fino sobre quién puede usar un atributo o método. Estos niveles intermedios se utilizan para equilibrar encapsulación y reutilización, especialmente en sistemas grandes con herencia y múltiples módulos.

En **Java** existen cuatro niveles de visibilidad:

* **public**: accesible desde cualquier clase del programa.
* **private**: accesible solo dentro de la propia clase.
* **protected**: accesible dentro de la misma clase, en su paquete y en las subclases (aunque estén en otros paquetes).
* **por defecto (package-private)**: cuando no se indica ningún modificador; el miembro solo es accesible desde clases del mismo paquete.

Este sistema permite controlar con bastante precisión el acceso a atributos y métodos, facilitando tanto la ocultación de información como la reutilización mediante herencia.

En otros lenguajes orientados a objetos también existen varios niveles de visibilidad, aunque con diferencias. En **C++**, por ejemplo, se usan `public`, `private` y `protected`, pero no existe el concepto de paquete como en Java. En **C#** hay más variantes, como `internal` (visible dentro del mismo ensamblado) o combinaciones como `protected internal`. En **Python**, la visibilidad es más flexible: no hay restricciones estrictas a nivel del lenguaje, aunque por convención se usan prefijos como `_` o `__` para indicar miembros internos o privados.

En general, todos estos mecanismos buscan el mismo objetivo: controlar el acceso a los componentes de una clase para mantener la coherencia interna y facilitar el diseño modular del software. La diferencia está en el grado de formalidad y control que cada lenguaje ofrece.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
La respuesta correcta es **(a) otras clases**. En Java, los miembros privados de una clase están ocultos para cualquier otra clase externa, pero **sí son accesibles desde otras instancias de la misma clase**. Esto ocurre porque la restricción `private` se aplica a nivel de clase, no a nivel de objeto individual.

Podemos verlo con un ejemplo en la clase `Punto`, donde los atributos `x` e `y` son privados y añadimos un método `calcularDistanciaAPunto` que recibe otro objeto `Punto`:

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;  // acceso permitido
        double dy = this.y - otro.y;  // acceso permitido
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En este método, aunque `x` e `y` son privados, se puede acceder a `otro.x` y `otro.y`. Esto demuestra que un objeto de la clase `Punto` puede acceder a los miembros privados de otro objeto de la misma clase. La razón es que el acceso `private` se controla a nivel de clase: todo el código dentro de la clase `Punto` puede acceder a esos miembros, independientemente de qué instancia concreta se esté usando.

Sin embargo, si otra clase diferente intentara acceder directamente a `x` o `y`, el compilador daría error. Por tanto, los miembros privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto permite que los objetos cooperen entre sí internamente sin romper el principio de encapsulación frente al exterior.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos **getter** y **setter** son métodos utilizados para acceder y modificar los atributos privados de una clase de forma controlada. Se emplean como parte del principio de encapsulación: en lugar de permitir el acceso directo a los atributos, se declaran como `private` y se proporcionan métodos públicos para leerlos o modificarlos.

Un **getter** es un método que devuelve el valor de un atributo. Normalmente no recibe parámetros y su nombre suele empezar por `get`, seguido del nombre del atributo. Por ejemplo, `getEdad()` devuelve el valor del atributo `edad`. Un **setter** es un método que permite modificar el valor de un atributo. Recibe como parámetro el nuevo valor y suele llamarse `setEdad()`, `setNombre()`, etc.

El uso de getters y setters permite controlar cómo se accede y modifica el estado de un objeto. Por ejemplo, un setter puede comprobar que un valor sea válido antes de asignarlo, evitando estados incorrectos. Además, si en el futuro cambia la forma de almacenar internamente los datos, se puede modificar la implementación sin cambiar la interfaz pública de la clase.

En resumen, los getters y setters son métodos auxiliares que facilitan la ocultación de información y el control del acceso a los atributos, mejorando la seguridad, el mantenimiento y la claridad del diseño en programación orientada a objetos.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
No. Cuando en POO se dice que la ocultación de información mejora la **seguridad**, no se refiere principalmente a evitar que el programa sea hackeado o atacado desde fuera, sino a la **seguridad interna del diseño del software**. Es decir, se busca evitar errores de uso, modificaciones incorrectas de datos y estados incoherentes dentro del propio programa.

La ocultación de información protege los datos de un objeto frente a accesos indebidos desde otras partes del código. Si los atributos son privados, nadie puede modificarlos directamente sin pasar por los métodos públicos definidos por la clase. Estos métodos pueden validar valores, impedir operaciones incorrectas o mantener invariantes. De esta forma se reduce el riesgo de que el programa entre en estados inválidos por errores de programación.

Por ejemplo, si una clase `CuentaBancaria` tiene el atributo `saldo` privado, no se podrá asignar cualquier valor desde fuera. Solo se modificará mediante métodos como `ingresar()` o `retirar()`, que pueden comprobar que el saldo no se vuelva negativo. Esto mejora la robustez del programa y evita fallos lógicos.

Aunque una buena encapsulación puede contribuir indirectamente a la seguridad frente a fallos o usos indebidos, no es un mecanismo de seguridad informática frente a hackers. La seguridad frente a ataques externos depende de otros factores como autenticación, cifrado, control de acceso, redes o validación de entradas. En POO, “seguridad” significa principalmente **fiabilidad y protección del estado interno del programa**.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
La diferencia principal entre un **miembro de instancia** y un **miembro de clase** está en **a qué pertenece y cómo se accede**:

1. **Miembro de instancia**: pertenece a cada objeto concreto de la clase. Cada instancia tiene su propia copia de los atributos de instancia y los métodos que operan sobre ellos usan los datos de esa instancia. Por ejemplo, en la clase `Punto`, `x` e `y` son miembros de instancia: cada objeto `Punto` tiene sus propias coordenadas independientes de otros puntos.

2. **Miembro de clase**: pertenece a la propia clase, no a las instancias. Se declara usando la palabra clave `static` en Java. Solo existe una copia compartida por todos los objetos de la clase, y se puede acceder directamente desde la clase sin crear objetos. Por ejemplo, un contador de cuántos puntos se han creado podría declararse así:

```java
public class Punto {
    private double x;
    private double y;
    private static int contadorPuntos = 0; // miembro de clase

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        contadorPuntos++;
    }

    public static int getContadorPuntos() {
        return contadorPuntos;
    }
}
```

Sí, **los miembros de clase también se pueden ocultar** usando `private`. Por ejemplo, `contadorPuntos` es privado y solo se puede consultar mediante el método público `getContadorPuntos()`. Esto permite **mantener el control sobre los datos compartidos** y aplicar el mismo principio de encapsulación y ocultación de información que con los miembros de instancia.

En resumen: los miembros de instancia pertenecen a cada objeto y pueden variar entre instancias; los miembros de clase pertenecen a la clase y son compartidos por todos los objetos. Ambos tipos pueden ser privados o públicos, dependiendo de cómo queramos controlar su acceso.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
De forma general, **no es común que los constructores sean privados**. En la mayoría de las clases, los constructores deben ser **públicos** para que otras partes del programa puedan crear instancias normalmente. Declararlos privados solo tiene sentido en casos especiales, como los patrones de diseño (Singleton, métodos de fábrica, creación controlada de objetos) donde se quiere restringir o centralizar la creación de instancias.

Para una clase estándar que representa objetos cotidianos, como `Punto` o `Empleado`, los constructores públicos permiten que cada usuario de la clase cree sus propios objetos de manera sencilla y coherente. Hacerlos privados en estos casos complicaría innecesariamente el diseño y dificultaría la reutilización.

En resumen: **los constructores privados se usan solo cuando se necesita controlar estrictamente la creación de objetos**, pero la regla general en POO es que los constructores sean públicos.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los **miembros de clase** se indican con la palabra clave `static`. Esto aplica tanto a atributos como a métodos, y significa que ese miembro **pertenece a la clase en sí**, no a cada objeto individual. Solo existe una copia compartida por todos los objetos, y se puede acceder directamente usando el nombre de la clase sin necesidad de instanciarla.

Podemos añadir a la clase `Punto` miembros de clase para registrar los valores máximos de `x` e `y` de todos los puntos creados hasta el momento:

```java
public class Punto {
    private double x;
    private double y;

    // Miembros de clase
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        // Actualizar los valores máximos de clase
        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    // Método de instancia
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Métodos estáticos para acceder a los máximos
    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}
```

Ejemplo de uso:

```java
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(3, 4);
        Punto p2 = new Punto(7, 2);
        Punto p3 = new Punto(5, 9);

        System.out.println("Max X: " + Punto.getMaxX()); // 7.0
        System.out.println("Max Y: " + Punto.getMaxY()); // 9.0
    }
}
```

En este ejemplo:

* `maxX` y `maxY` son **miembros de clase** compartidos por todos los puntos.
* Cada vez que se crea un `Punto`, el constructor actualiza los valores máximos si los nuevos `x` o `y` son mayores.
* Los métodos `getMaxX()` y `getMaxY()` son **métodos de clase** (`static`) y permiten consultar los máximos sin necesidad de instanciar un objeto.

Esto demuestra cómo los miembros de clase permiten almacenar información **compartida por todas las instancias**, a diferencia de los atributos normales de instancia que son propios de cada objeto.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
Sí, para un **método factoría** dentro de `Punto` se utiliza `static`, porque el método debe poder **crear una instancia de la clase sin depender de un objeto existente**. Aquí tienes el código del método:

```java
public static Punto crearRedondeado(double x, double y) {
    int xRedondeado = (int) Math.round(x);
    int yRedondeado = (int) Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
```

**Explicación breve:**

* El método es `static` porque se llama desde la clase, no desde un objeto.
* Recibe dos coordenadas `double`, las redondea al entero más cercano usando `Math.round()`.
* Devuelve un nuevo objeto `Punto` con los valores redondeados.

Ejemplo de uso:

```java
Punto p = Punto.crearRedondeado(3.6, 4.3); // crea un Punto(4, 4)
```

De esta manera, el método factoría permite **controlar cómo se crean instancias**, aplicando lógica adicional antes de construir el objeto.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
Podemos cambiar la implementación interna de `Punto` usando un **array de dos posiciones** para almacenar las coordenadas, manteniendo la misma interfaz pública (constructor, métodos de acceso y cálculo de distancias). Esto es un ejemplo de cómo la **ocultación de información** permite cambiar la representación interna sin afectar al código que usa la clase.

Aquí tienes la clase `Punto` modificada:

```java
public class Punto {
    private double[] coordenadas = new double[2]; // array interno: [0] = x, [1] = y

    // Constructor
    public Punto(double x, double y) {
        coordenadas[0] = x;
        coordenadas[1] = y;
    }

    // Métodos de acceso
    public double getX() {
        return coordenadas[0];
    }

    public double getY() {
        return coordenadas[1];
    }

    // Distancia al origen
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + coordenadas[1] * coordenadas[1]);
    }

    // Distancia a otro punto
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coordenadas[0] - otro.coordenadas[0];
        double dy = this.coordenadas[1] - otro.coordenadas[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Método factoría (redondeado)
    public static Punto crearRedondeado(double x, double y) {
        int xRedondeado = (int) Math.round(x);
        int yRedondeado = (int) Math.round(y);
        return new Punto(xRedondeado, yRedondeado);
    }
}
```

**Qué ha cambiado:**

* Internamente ya no hay dos atributos `x` e `y`; ahora se usan `coordenadas[0]` y `coordenadas[1]`.
* La interfaz pública (`getX()`, `getY()`, `calcularDistanciaAOrigen()`, `calcularDistanciaAPunto()`) **no cambia**, por lo que el resto del programa que use `Punto` seguirá funcionando igual.

Esto demuestra cómo la **ocultación de información** permite modificar la implementación interna sin romper el contrato de la clase.

Si quieres, puedo mostrar también cómo **actualizar los miembros de clase `maxX` y `maxY`** para que funcionen con esta nueva implementación usando el array. ¿Quieres que haga eso?


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
No, **no es mejor declararlo público**, aunque luego tenga getters y setters. La convención más habitual en Java y en POO en general es declarar los atributos **privados** y exponer solo los métodos necesarios para acceder o modificarlos. Esto permite **controlar cómo se manipulan los datos**, incluso cuando se usan getters y setters, y protege las invariantes de clase.

Si un atributo fuera público, cualquier parte del programa podría cambiarlo directamente, sin pasar por lógica de validación o control. En cambio, si es privado y se accede mediante getters y setters, la clase puede:

* Validar valores antes de asignarlos en el setter.
* Mantener invariantes, es decir, garantizar que el objeto siempre está en un estado válido.
* Cambiar la implementación interna sin afectar al código externo que usa la clase.

Por ejemplo, en una clase `CuentaBancaria`, el atributo `saldo` sería privado. El setter podría impedir que se asigne un saldo negativo, manteniendo así la **invariante de clase** “el saldo nunca puede ser negativo”. Si `saldo` fuera público, cualquier código podría modificarlo directamente y romper esta invariante.

En resumen, **los atributos suelen ser privados**, y los getters y setters forman parte de la interfaz pública que permite un acceso controlado, protegiendo la consistencia y seguridad del objeto. Esto está directamente relacionado con las invariantes de clase, porque garantizan que siempre se cumplen las reglas internas del objeto.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase se considera **inmutable** cuando, una vez creada una instancia de esa clase, **su estado interno no puede cambiar**. Esto significa que todos los atributos se establecen durante la creación del objeto (normalmente mediante el constructor) y no existen métodos que los modifiquen después. Por ejemplo, la clase `String` en Java es inmutable: una vez que creas un objeto `String`, su contenido no puede cambiar.

Un **método modificador** es cualquier método que altera el estado interno de un objeto. Normalmente se asocia con los **setters**, que modifican atributos específicos, pero no todos los métodos modificadores son setters estrictamente. Por ejemplo, un método `depositar(double cantidad)` en una clase `CuentaBancaria` también es un método modificador porque cambia el saldo, aunque no se llame `setSaldo()`. La característica clave es que cualquier método que cambie el valor de los atributos del objeto se considera modificador.

Las clases inmutables ofrecen varias ventajas:

* **Seguridad frente a errores**: los objetos no cambian inesperadamente, evitando inconsistencias.
* **Simplicidad en concurrencia**: al no modificarse, se pueden compartir entre hilos sin necesidad de sincronización.
* **Facilidad de depuración**: los objetos permanecen constantes, lo que hace más predecible su comportamiento.
* **Facilita la construcción de colecciones y estructuras de datos confiables**, ya que no se corre el riesgo de que un objeto compartido cambie su estado mientras se usa en varias partes del programa.

En resumen, una clase inmutable no tiene métodos modificadores que alteren su estado; los setters y otros métodos que cambien atributos rompen la inmutabilidad. Esto aporta seguridad, claridad y eficiencia en entornos concurrentes o complejos.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No, **no siempre es recomendable incluir métodos `setter`**. La convención de POO es declarar los atributos como privados y proporcionar getters cuando se necesita acceso, pero los setters solo deben añadirse **cuando realmente sea necesario permitir modificar el estado del objeto**.

Incluir setters por defecto puede ser problemático porque:

* Permite cambiar el estado del objeto libremente, lo que puede **romper invariantes de clase**.
* Hace que los objetos sean más difíciles de usar en entornos concurrentes o multihilo, ya que su estado puede modificarse en cualquier momento.
* Reduce la capacidad de mantener un control claro sobre cómo y cuándo se modifican los datos internos.

Por convención, se recomienda:

* Usar **solo getters** para atributos que deben ser consultables pero no modificables desde fuera.
* Incluir setters solo si hay una **necesidad real** de modificar el atributo y se pueden aplicar validaciones para mantener la coherencia del objeto.
* Considerar la **inmutabilidad** cuando tenga sentido: objetos que no cambian su estado después de crearse suelen ser más seguros y fáciles de mantener.

En resumen, los setters no son obligatorios y su inclusión depende de la lógica de la clase. La filosofía de diseño es **exponer lo mínimo necesario** para mantener control y coherencia, evitando modificaciones innecesarias desde fuera de la clase.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
