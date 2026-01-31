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
Las cuatro características básicas de la Programación Orientada a Objetos (POO) son **abstracción**, **encapsulamiento**, **herencia** y **polimorfismo**. Estas características definen el paradigma y marcan la principal diferencia respecto a la programación estructurada, donde el foco está en las funciones y el flujo del programa, mientras que en POO el foco está en los objetos y sus responsabilidades.

La **abstracción** consiste en representar entidades del mundo real o conceptos del problema mediante clases, mostrando solo la información y comportamientos relevantes y ocultando los detalles innecesarios. Gracias a la abstracción, el programador puede trabajar con modelos más simples y cercanos al dominio del problema, sin preocuparse por cómo están implementados internamente. Por ejemplo, una clase `Coche` puede ofrecer métodos como `acelerar()` o `frenar()` sin que el usuario de la clase sepa cómo se gestionan internamente el motor o la velocidad.

El **encapsulamiento** implica agrupar los datos y los métodos que operan sobre ellos dentro de una clase, y restringir el acceso directo a esos datos. Normalmente, los atributos se mantienen privados y solo se accede a ellos mediante métodos públicos. Esto mejora la seguridad y el mantenimiento del código, ya que evita modificaciones no controladas del estado interno del objeto y permite cambiar la implementación sin afectar al resto del programa.

La **herencia** permite crear nuevas clases a partir de otras ya existentes, reutilizando su código y estableciendo una relación jerárquica entre ellas. Una clase hija hereda atributos y métodos de la clase padre, y puede ampliar o modificar su comportamiento. El **polimorfismo**, estrechamente relacionado con la herencia, permite que un mismo método se comporte de forma distinta según el objeto que lo implemente, posibilitando tratar objetos diferentes de manera uniforme a través de una misma interfaz.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Existen muchos lenguajes de programación que soportan la Programación Orientada a Objetos, ya sea como paradigma principal o combinado con otros enfoques. Entre los más populares y utilizados en la actualidad se encuentran Java, C++, Python y C#. Todos ellos permiten modelar programas mediante clases y objetos, aunque cada uno lo hace con matices y filosofías propias.

**Java** es uno de los lenguajes orientados a objetos más representativos, ya que prácticamente todo en Java se basa en clases y objetos. Fue diseñado con la POO como eje central, lo que lo convierte en un lenguaje muy usado en la enseñanza universitaria, en aplicaciones empresariales y en desarrollo para Android. Su tipado fuerte y su gestión automática de memoria facilitan centrarse en los conceptos de diseño orientado a objetos.

**C++** es un lenguaje multiparadigma que combina programación estructurada, genérica y orientada a objetos. Permite usar clases, herencia y polimorfismo, pero también conserva características de bajo nivel heredadas de C. Esto lo hace muy potente, aunque más complejo, y resulta especialmente interesante para estudiantes que ya conocen C, ya que muestra claramente la evolución desde la programación estructurada hacia la POO.

**Python** y **C#** también son lenguajes muy populares que soportan plenamente la programación orientada a objetos. Python ofrece una sintaxis sencilla y flexible, ideal para aprender conceptos sin demasiada complejidad técnica, mientras que C# es un lenguaje fuertemente orientado a objetos dentro del ecosistema .NET, muy utilizado en aplicaciones de escritorio, videojuegos y desarrollo empresarial.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La **programación estructurada** es un paradigma que surgió para mejorar la claridad, la fiabilidad y el mantenimiento del código frente a la programación no estructurada basada en saltos incontrolados (`goto`). Su idea principal es organizar el programa como una secuencia de instrucciones bien definidas, utilizando únicamente tres estructuras de control: **secuencia**, **selección** (if/else, switch) e **iteración** (for, while). De este modo, el flujo de ejecución resulta predecible y más fácil de razonar, algo fundamental cuando los programas empiezan a crecer en tamaño y complejidad.

En la programación estructurada, el problema se descompone en tareas más pequeñas que se resuelven mediante funciones o procedimientos. Estas funciones operan sobre datos que, por lo general, están separados de ellas, lo que significa que los datos suelen ser globales o se pasan como parámetros. Este enfoque es típico de lenguajes como C, donde el diseño del programa gira en torno a “qué pasos hay que ejecutar” más que en “quién es responsable de los datos”.

La **programación modular** va un paso más allá y se centra en dividir el programa en módulos independientes, cada uno con una responsabilidad concreta. Un módulo agrupa funciones relacionadas y, en algunos casos, los datos que estas manejan, ocultando los detalles internos y exponiendo solo una interfaz pública. En C, esto suele lograrse mediante archivos `.h` y `.c`, donde el archivo de cabecera define qué se puede usar desde fuera del módulo y el archivo de implementación contiene el código interno.

La principal ventaja de la programación modular es que facilita la reutilización, el trabajo en equipo y el mantenimiento del software. Al estar los módulos débilmente acoplados, es posible modificar o sustituir uno sin afectar al resto del programa, siempre que se respete su interfaz. Este paradigma sirve como puente conceptual hacia la POO, ya que introduce ideas clave como la separación de responsabilidades y el ocultamiento de información, que luego se formalizan y refuerzan mediante clases y objetos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
En Programación Orientada a Objetos, un objeto se define fundamentalmente por **tres elementos: identidad, estado y comportamiento**. Estos tres aspectos permiten diferenciar un objeto de otro y describir tanto qué es como cómo actúa dentro del programa. A diferencia de una simple estructura de datos, un objeto combina información y funcionalidad en una misma entidad.

La **identidad** es la propiedad que distingue a un objeto de cualquier otro, incluso aunque tenga el mismo estado y comportamiento. Dos objetos creados a partir de la misma clase pueden contener los mismos valores en sus atributos, pero siguen siendo objetos distintos en memoria. Esta identidad es importante porque permite referirse a un objeto concreto y seguir existiendo mientras el programa lo necesite.

El **estado** de un objeto está formado por el conjunto de valores de sus atributos en un momento dado. Representa la información interna del objeto y puede cambiar a lo largo del tiempo como consecuencia de la ejecución de métodos. Por ejemplo, en un objeto `CuentaBancaria`, el saldo o el número de cuenta forman parte de su estado.

El **comportamiento** define las acciones que un objeto puede realizar y viene dado por sus métodos. Estos métodos permiten modificar el estado del propio objeto o interactuar con otros objetos. El comportamiento es lo que da sentido al estado, ya que controla cómo y cuándo puede cambiar, reforzando así el principio de encapsulamiento propio de la POO.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una **clase** es un molde o plantilla que define cómo serán los objetos de un determinado tipo. En ella se especifican los atributos (datos) y los métodos (comportamientos) que tendrán los objetos creados a partir de esa clase. La clase no representa un elemento concreto del programa, sino una definición abstracta que describe las características comunes de un conjunto de objetos similares, como podría ser la clase `Alumno` o `Coche`.

Una clase **no es lo mismo que un objeto**. El objeto es la entidad concreta que existe en memoria y que puede interactuar con otros elementos del programa. Mientras que la clase define “qué puede tener y qué puede hacer”, el objeto es “algo que realmente existe” siguiendo esa definición. Por ejemplo, la clase `Coche` define el concepto general, pero un coche concreto con una matrícula y un color determinados es un objeto.

Una **instancia** es precisamente ese objeto concreto creado a partir de una clase. Instanciar una clase significa reservar memoria y crear un objeto que cumple la estructura definida por la clase. En este sentido, los términos *objeto* e *instancia* suelen usarse como sinónimos en la práctica, aunque técnicamente instancia enfatiza el proceso de creación a partir de una clase.

No todos los lenguajes orientados a objetos manejan el concepto de clase de la misma forma, e incluso algunos no lo usan explícitamente. Lenguajes como Java, C++ o C# son claramente **basados en clases**, pero otros como JavaScript utilizan un modelo **basado en prototipos**, donde los objetos se crean a partir de otros objetos sin necesidad de definir clases tradicionales. Aun así, todos ellos comparten los principios fundamentales de la POO, aunque los implementen de manera distinta.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
En general, los objetos en programación orientada a objetos se almacenan en la **memoria dinámica**, también conocida como *heap*. Esto se debe a que los objetos suelen tener un tiempo de vida que no está ligado estrictamente al ámbito de una función, sino que pueden existir mientras haya referencias a ellos. En lenguajes como Java o C#, todos los objetos se crean en el heap y se accede a ellos mediante referencias, lo que permite un manejo flexible de su ciclo de vida.

Sin embargo, **no es igual en todos los lenguajes**. En C++, por ejemplo, un objeto puede crearse tanto en la pila (*stack*) como en el heap, dependiendo de cómo se declare. Un objeto creado como variable local se almacena normalmente en la pila y se destruye automáticamente al salir del ámbito, mientras que uno creado con `new` se almacena en el heap y debe liberarse explícitamente con `delete`. Esta diferencia es importante porque implica distintos modelos de gestión de memoria y distintos riesgos, como fugas de memoria.

La **recolección de basura** (*garbage collection*) es un mecanismo automático de gestión de memoria presente en muchos lenguajes orientados a objetos, como Java, C# o Python. Su función es detectar qué objetos ya no están siendo utilizados por el programa, es decir, aquellos a los que no existe ninguna referencia activa, y liberar automáticamente la memoria que ocupan. De este modo, el programador no tiene que preocuparse de liberar manualmente la memoria.

La principal ventaja de la recolección de basura es que reduce errores comunes como las fugas de memoria o el acceso a memoria ya liberada. No obstante, también introduce cierto coste en tiempo de ejecución y hace que el control sobre el momento exacto de liberación de la memoria sea menos preciso. Por eso, algunos lenguajes como C++ prefieren dejar la gestión en manos del programador, mientras que otros priorizan la seguridad y la simplicidad mediante mecanismos automáticos.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un **método** es una función que forma parte de una clase y que define una acción o comportamiento que pueden realizar los objetos de esa clase. A diferencia de las funciones de la programación estructurada, un método suele operar directamente sobre los atributos del objeto al que pertenece, pudiendo consultar o modificar su estado interno. Los métodos representan lo que un objeto “sabe hacer” y son una pieza clave para implementar el comportamiento en la POO.

Desde el punto de vista conceptual, un método siempre está asociado a un objeto concreto (salvo los métodos estáticos) y se invoca sobre él. Esto permite que distintos objetos de la misma clase ejecuten el mismo método pero actúen sobre datos diferentes. Por ejemplo, dos objetos `CuentaBancaria` pueden ejecutar el método `ingresar()`, pero cada uno modificará su propio saldo, no el de otro objeto.

La **sobrecarga de métodos** consiste en definir varios métodos con el mismo nombre dentro de una misma clase, pero con diferentes listas de parámetros (diferente número, tipo u orden). El compilador o el intérprete decide qué versión del método ejecutar en función de los argumentos que se pasen en la llamada. Esto permite usar un nombre común para operaciones conceptualmente similares, mejorando la legibilidad del código.

La sobrecarga no debe confundirse con el polimorfismo por herencia, aunque están relacionados. En lenguajes como C++ o Java, la sobrecarga se resuelve en tiempo de compilación y es una forma de polimorfismo estático. No todos los lenguajes la soportan de la misma manera; por ejemplo, Python no implementa sobrecarga clásica de métodos, aunque permite comportamientos similares mediante valores por defecto o comprobación de tipos en tiempo de ejecución.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
Aquí tienes un **ejemplo mínimo y claro en Java**, siguiendo exactamente lo que pides: clase `Punto`, dos atributos `x` e `y` con visibilidad por defecto (package-private), un método `calculaDistanciaAOrigen` y un ejemplo de uso con una instancia.

```java
public class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

Ejemplo de uso de la clase, creando una instancia y llamando al método:

```java
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println(distancia);
    }
}
```

En este ejemplo, el objeto `p` representa un punto concreto del plano con coordenadas `(3, 4)`. Al llamar al método `calculaDistanciaAOrigen`, el método opera sobre los propios atributos del objeto (`x` e `y`) y devuelve la distancia al origen `(0, 0)`, que en este caso será `5.0`. Esto muestra de forma sencilla cómo una clase define estructura y comportamiento, y cómo una instancia utiliza ambos.


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
El punto de entrada de un programa en Java es el método `public static void main(String[] args)`. Es el método que la Máquina Virtual de Java (JVM) busca y ejecuta cuando se inicia un programa. Debe tener exactamente esa firma para que la JVM pueda reconocerlo, ya que se trata de un método especial que no pertenece a un objeto concreto, sino a la clase en sí, y desde el cual se comienza la ejecución del programa.

La palabra clave `static` indica que un atributo o método pertenece a la clase y no a las instancias de la misma. Esto significa que se puede acceder a un miembro `static` sin necesidad de crear un objeto. En el caso de `main`, esto es necesario porque, cuando el programa empieza, todavía no existe ningún objeto creado; la JVM necesita poder ejecutar el método directamente desde la clase. En general, `static` se usa para métodos de utilidad, constantes compartidas o comportamientos que no dependen del estado de un objeto concreto.

`static` no se emplea únicamente para el método `main`. Se utiliza con frecuencia para definir atributos comunes a todas las instancias de una clase, como contadores, configuraciones globales o constantes, y también para métodos que realizan operaciones generales. Por ejemplo, la clase `Math` de Java está formada casi exclusivamente por métodos `static`, ya que no tiene sentido crear objetos `Math` para realizar cálculos.

La combinación de `static` con `final` se utiliza principalmente para definir **constantes**. Un atributo `static final` tiene un único valor compartido por toda la clase y no puede modificarse una vez inicializado. Esto se usa, por ejemplo, para valores fijos como `Math.PI`. De esta forma se garantiza que el valor es único, inmutable y accesible sin crear instancias, lo que mejora la claridad del código y evita errores.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
En Java, el proceso de ejecución de un programa implica **compilación y ejecución en la Máquina Virtual de Java (JVM)**, por lo que no funciona exactamente igual que en C o C++, donde el programa se compila directamente a código máquina. Para ejecutar un programa básico desde línea de comandos, los pasos son los siguientes:

1. **Escribir el código fuente** en un archivo con extensión `.java`. Por ejemplo, si tenemos el programa mínimo con la clase `Punto` y el método `main` en un archivo llamado `Main.java`.

2. **Compilar el programa** usando el comando `javac`. Esto convierte el código fuente en *byte-code*, que es un código intermedio independiente de la plataforma:

   ```bash
   javac Main.java
   ```

   Este comando genera un archivo `Main.class` que contiene el byte-code de la clase `Main`. Si la clase `Punto` está en un archivo separado `Punto.java`, también se compilará a `Punto.class`.

3. **Ejecutar el programa** con el comando `java`, indicando la clase que contiene el método `main`:

   ```bash
   java Main
   ```

   La JVM carga el archivo `Main.class`, interpreta o ejecuta el byte-code y produce la salida del programa.

En este sentido, **Java es un lenguaje compilado e interpretado**. Se compila primero a *byte-code*, que no es código máquina, y luego este byte-code es interpretado o ejecutado por la JVM, lo que le da portabilidad: el mismo archivo `.class` puede ejecutarse en cualquier sistema que tenga una JVM.

La **Máquina Virtual de Java (JVM)** es un entorno de ejecución que interpreta o ejecuta el byte-code y proporciona servicios como gestión de memoria, recolección de basura y seguridad. El **byte-code** es el código intermedio generado por el compilador `javac`; no depende del sistema operativo ni de la arquitectura del procesador. Los archivos con extensión `.class` son precisamente los que contienen este byte-code, listos para ser ejecutados por la JVM. Esto explica por qué un mismo `.class` puede ejecutarse en Windows, Linux o Mac sin recompilar, siempre que haya una JVM disponible.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
En el código de la clase `Punto`, la palabra clave `new` se utiliza para **crear una nueva instancia de la clase**, es decir, para reservar memoria en el heap y devolver una referencia al objeto recién creado. Por ejemplo:

```java
Punto p = new Punto();
```

Aquí, `new Punto()` crea un objeto `Punto` en memoria y `p` guarda la referencia a ese objeto. Sin `new`, no se puede instanciar un objeto en Java (a excepción de algunos casos especiales como literales de tipo primitivo o Strings).

Un **constructor** es un método especial de una clase que se llama automáticamente al crear un objeto mediante `new`. Su función principal es inicializar los atributos del objeto y dejarlo listo para usarse. Los constructores tienen el mismo nombre que la clase, **no tienen tipo de retorno** (ni siquiera `void`) y pueden sobrecargarse, es decir, puede haber varios constructores con distintos parámetros.

Por ejemplo, una clase `Empleado` con un constructor que inicializa los atributos `DNI`, `nombre` y `apellidos` podría definirse así:

```java
public class Empleado {
    String DNI;
    String nombre;
    String apellidos;

    // Constructor
    public Empleado(String DNI, String nombre, String apellidos) {
        this.DNI = DNI;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

Uso del constructor al crear un objeto:

```java
public class Main {
    public static void main(String[] args) {
        Empleado e = new Empleado("12345678A", "Ana", "Pérez");
        System.out.println(e.nombre + " " + e.apellidos);
    }
}
```

En este ejemplo, el constructor garantiza que cada `Empleado` creado tenga ya definidos sus atributos `DNI`, `nombre` y `apellidos` desde el momento de su creación, evitando que queden valores por defecto sin inicializar. Esto es más seguro y organizado que asignarlos después de crear el objeto, como se hacía en el ejemplo de `Punto`.


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
En Java, la referencia **`this`** es un puntero implícito que hace referencia al **objeto actual** sobre el que se está ejecutando un método o constructor. Es útil cuando quieres diferenciar los atributos del objeto de variables locales o parámetros con el mismo nombre, o cuando necesitas pasar la propia instancia a otro método u objeto. Básicamente, `this` permite que un objeto se refiera a sí mismo dentro de su propia clase.

No todos los lenguajes orientados a objetos utilizan el mismo nombre para esta referencia. Por ejemplo, en **C++** también se usa `this`, mientras que en **Python** se emplea `self`. En otros lenguajes como JavaScript dentro de objetos, `this` se comporta de manera más dinámica dependiendo del contexto de ejecución, lo que puede generar confusión. Lo importante es que casi todos los lenguajes POO ofrecen un mecanismo equivalente para referirse al objeto actual.

En la clase `Punto`, un ejemplo típico de uso de `this` sería en un constructor o un método que reciba parámetros con el mismo nombre que los atributos de la clase:

```java
public class Punto {
    double x;
    double y;

    // Constructor con parámetros
    public Punto(double x, double y) {
        this.x = x; // el this.x es el atributo de la clase
        this.y = y; // el x e y sin this serían los parámetros
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p.calculaDistanciaAOrigen()); // 5.0
    }
}
```

En este ejemplo, `this.x` y `this.y` se refieren a los atributos del objeto, mientras que `x` y `y` son los parámetros del constructor. Sin `this`, el compilador no podría distinguir entre los atributos y los parámetros, y los valores de los atributos no se inicializarían correctamente.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
Perfecto, podemos añadir el método `distanciaA` a la clase `Punto`. Este método recibirá un objeto `Punto` como parámetro y calculará la distancia entre el punto actual (`this`) y el punto proporcionado, usando la fórmula de distancia euclidiana en el plano:

$$
	{distancia} = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}
$$

Aquí tienes la clase `Punto` actualizada con el nuevo método y un ejemplo de uso:

```java
public class Punto {
    double x;
    double y;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Distancia a otro punto
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Ejemplo de uso:

```java
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(3, 4);
        Punto p2 = new Punto(6, 8);

        System.out.println("Distancia al origen de p1: " + p1.calculaDistanciaAOrigen());
        System.out.println("Distancia entre p1 y p2: " + p1.distanciaA(p2));
    }
}
```

En este ejemplo, `p1.distanciaA(p2)` utiliza `this` para referirse a `p1` y accede a `p2` a través del parámetro `otro`. La fórmula calcula correctamente la distancia entre ambos puntos. Esto demuestra cómo los objetos pueden interactuar entre sí y cómo los métodos pueden usar tanto los atributos del objeto actual como los de otros objetos del mismo tipo.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
En Java, **el paso de objetos como parámetros se realiza por valor de la referencia**, no por referencia directa como en C++ con punteros. Esto significa que cuando pasamos un objeto, como un `Punto`, al método, **se copia la referencia** al objeto, no el objeto completo. Por lo tanto, dentro del método se puede modificar el estado del objeto (sus atributos) y esos cambios **sí afectarán al objeto original** fuera del método, porque ambos, el parámetro y la variable original, apuntan al mismo objeto en memoria.

Por ejemplo, si tenemos este código:

```java
void moverX(Punto p) {
    p.x += 10; // modifica el atributo x del objeto original
}

public static void main(String[] args) {
    Punto p1 = new Punto(3, 4);
    moverX(p1);
    System.out.println(p1.x); // imprimirá 13, el cambio persiste
}
```

Aquí, aunque el parámetro `p` se pasa “por valor”, el valor es la referencia al objeto, por eso `p.x` modifica directamente el objeto original.

En cambio, cuando pasamos **tipos primitivos** como `int`, `double` o `boolean`, **el valor se copia completamente**. Modificar el parámetro dentro del método **no afecta a la variable original** fuera del método:

```java
void sumarCinco(int n) {
    n += 5; // solo modifica la copia local
}

public static void main(String[] args) {
    int numero = 10;
    sumarCinco(numero);
    System.out.println(numero); // imprimirá 10, no se modificó
}
```

En resumen:

* **Objetos**: se pasa la referencia por valor → cambios en atributos afectan al objeto original.
* **Primitivos**: se pasa el valor → cambios no afectan a la variable original.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
En Java, el método **`toString()`** es un método especial definido en la clase base `Object`, de la cual heredan todas las clases. Su propósito es **proporcionar una representación en forma de cadena de texto del objeto**, generalmente para mostrar información útil sobre su estado. Por defecto, `Object.toString()` devuelve un valor poco legible, que combina el nombre de la clase y el código hash del objeto, por lo que es común **sobrescribirlo** en nuestras clases para mostrar los atributos de manera comprensible.

El concepto de `toString()` existe en otros lenguajes, aunque con distinto nombre o mecanismo. Por ejemplo, en **Python** se usa `__str__()` o `__repr__()`, en **C#** también se llama `ToString()`, y en **C++** se suelen sobrecargar operadores como `<<` para lograr un efecto similar. La idea general es la misma: permitir obtener una versión en texto del objeto, útil para depuración o impresión.

En la clase `Punto`, podemos sobrescribir `toString()` así:

```java
public class Punto {
    double x;
    double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Sobrescribir toString()
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}
```

Ejemplo de uso:

```java
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(3, 4);
        System.out.println(p1); // imprime: (3.0, 4.0)
    }
}
```

Al sobrescribir `toString()`, ahora `System.out.println(p1)` no imprime la referencia del objeto, sino una representación clara y legible de sus coordenadas. Esto es muy útil para depuración, registro de datos y cualquier situación donde queramos mostrar información del objeto sin acceder manualmente a cada atributo.


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta
Sí, se puede establecer una **analogía entre una clase en Java y un `struct` en C**, pero hay diferencias importantes. Un `struct` en C es simplemente un **contenedor de datos**, donde agrupas variables relacionadas bajo un mismo nombre. Por ejemplo, un `struct Punto` podría tener dos campos `x` e `y` para representar coordenadas. Esto es parecido a los atributos de una clase, pero **el `struct` no puede tener métodos, constructores ni comportamiento asociado**: solo almacena datos.

Lo que le falta al `struct` para ser como una clase es precisamente la **capacidad de encapsular comportamiento** junto con los datos. En una clase, los atributos y los métodos se combinan, de modo que cada objeto (instancia) tiene estado propio y puede realizar acciones definidas por los métodos. Además, las clases permiten otras características de POO que los `structs` de C no tienen: **herencia, polimorfismo, encapsulamiento y constructores**. Estas herramientas permiten diseñar sistemas más flexibles y seguros que simplemente agrupar datos.

Cuando hablamos de instancias, un `struct` en C no genera objetos con identidad propia gestionados por la memoria dinámica, como ocurre con los objetos en Java. Un objeto en POO tiene identidad, estado y comportamiento, y su ciclo de vida se controla en parte mediante la creación (`new`) y destrucción (o recolección de basura). En C, un `struct` se almacena en la pila o en memoria estática, y no hay mecanismo automático para asociar métodos o comportamientos a cada variable de tipo `struct`. Por eso, para que un `struct` sea “como una clase”, tendría que poder **tener métodos, constructores, control de acceso a sus campos y referencias dinámicas**.

En resumen: un `struct` es **solo datos**, mientras que una clase combina **datos y comportamiento** y permite que cada instancia sea un objeto completo y autónomo dentro del programa. Esto convierte a la POO en un paradigma mucho más potente para modelar sistemas complejos.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
Podemos “emular” una clase como `Punto` en C usando un **`struct` para los datos** y una **función externa** que opere sobre ese `struct`. En C no existen métodos asociados a los structs ni la referencia implícita `this`, así que tenemos que pasar **un puntero al struct como parámetro** cada vez que queramos operar sobre él. Ese puntero cumple el papel de `this` en POO: permite que la función acceda a los campos del objeto concreto que estamos manipulando.

Por ejemplo, podríamos emular la clase `Punto` así:

```c
#include <stdio.h>
#include <math.h>

// Definición del "struct"
typedef struct {
    double x;
    double y;
} Punto;

// Función que actúa como método
double calculaDistanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

int main() {
    Punto p1;
    p1.x = 3;
    p1.y = 4;

    printf("Distancia al origen: %.2f\n", calculaDistanciaAOrigen(&p1));
    return 0;
}
```

Aquí ocurre lo siguiente:

1. `Punto` contiene solo los datos (`x` e `y`), no hay métodos.
2. `calculaDistanciaAOrigen` es una función externa que recibe un puntero al struct. Este puntero funciona como `this`: permite acceder a los campos del “objeto” concreto (`p->x`, `p->y`).
3. No hay magia: no hay construcción automática, ni encapsulamiento ni recolección de basura; todo se gestiona explícitamente.

En otras palabras, **el concepto de objeto se descompone en datos y funciones independientes**, y `this` se reemplaza por un puntero explícito al struct. Esta aproximación es lo más cercano que se puede hacer en C para emular una clase, aunque carece de muchas ventajas de la POO, como la herencia o el polimorfismo.
