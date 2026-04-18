<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El **polimorfismo** es la capacidad de tratar objetos de distintas clases como si fueran del mismo tipo común, normalmente a través de una clase base o una interfaz. Dicho de forma más cercana a lo que se conoce de C/C++, permite usar una misma “forma” (por ejemplo, una referencia a una clase padre) para manejar objetos diferentes, haciendo que cada uno responda de manera específica. En Java, esto se logra principalmente mediante la herencia: una variable del tipo de la superclase puede apuntar a objetos de sus subclases, y al invocar un método, se ejecuta la versión correspondiente al objeto real.

Esto sirve para escribir código más flexible y reutilizable. En lugar de hacer múltiples estructuras condicionales para distinguir tipos (como podría ocurrir en C con `struct` y funciones separadas), se delega el comportamiento en cada clase. Así, se pueden añadir nuevas clases sin modificar el código existente, siempre que respeten la misma interfaz o clase base, lo que mejora la mantenibilidad y extensibilidad del programa.

La **sobreescritura de métodos** (method overriding) ocurre cuando una subclase proporciona su propia implementación de un método que ya está definido en su superclase. El método en la subclase debe tener el mismo nombre, parámetros y tipo de retorno que el de la clase padre. Cuando se llama a ese método a través de una referencia de la superclase, Java decide en tiempo de ejecución qué versión ejecutar, según el tipo real del objeto.

Este mecanismo es clave para el polimorfismo, ya que permite que distintas clases respondan de forma diferente al mismo mensaje (llamada a método). Por ejemplo, si varias clases heredan de una clase `Animal` y todas sobreescriben el método `hacerSonido()`, cada una podrá emitir su propio sonido, aunque se utilice una referencia de tipo `Animal` para invocar el método.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La **ligadura dinámica** (o *enlace tardío*) consiste en que la decisión de qué implementación de un método se va a ejecutar se toma **en tiempo de ejecución**, y no en tiempo de compilación. Es decir, cuando se invoca un método sobre una referencia, no se elige la versión en función del tipo de la variable, sino del **tipo real del objeto** al que apunta. Esto contrasta con lo que se conoce de C/C++, donde una llamada a función normalmente se resuelve en compilación (ligadura estática), salvo que se utilicen mecanismos específicos.

Su relación con el **polimorfismo** es directa: la ligadura dinámica es el mecanismo que permite que el polimorfismo funcione. Gracias a ella, se puede tener una referencia de tipo general (por ejemplo, una clase padre) y que, al llamar a un método, cada objeto responda con su propia implementación. Sin ligadura dinámica, todas las llamadas se resolverían según el tipo declarado de la referencia, y no se obtendría el comportamiento polimórfico esperado.

En **C++**, la ligadura dinámica **no es automática**: hay que indicarla explícitamente mediante la palabra clave `virtual` en los métodos de la clase base. Si no se hace, las llamadas se resuelven en tiempo de compilación (ligadura estática), incluso si se usan punteros o referencias a clases base. En cambio, en **Java**, todos los métodos de instancia (salvo los marcados como `final`, `static` o `private`) utilizan ligadura dinámica por defecto, por lo que no es necesario indicarlo explícitamente para obtener comportamiento polimórfico.

En **Python**, la ligadura dinámica también es el comportamiento por defecto, e incluso más flexible que en Java. No existe una declaración estricta de tipos como en Java o C++, y cualquier objeto que tenga el método que se intenta invocar puede responder a la llamada (lo que se conoce como *duck typing*). Por tanto, el enlace tardío es inherente al lenguaje y no requiere ninguna indicación explícita por parte del programador.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
A continuación se muestra un ejemplo sencillo donde se parte de una clase base `Soldado` con un método `saludar()`, y dos subclases: `Zapador` y `Artillero`. La clase `Zapador` **sobreescribe** completamente el método `saludar()`, mientras que `Artillero` mantiene el comportamiento original heredado (aunque también podría redefinirlo si se quisiera). Esto ilustra cómo distintas clases pueden compartir una misma interfaz pero responder de forma diferente.

```java
class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma genérica.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("El zapador saluda mientras prepara explosivos.");
    }
}

class Artillero extends Soldado {
    // No sobreescribe, usa el comportamiento de Soldado
}
```

Para ilustrar el **polimorfismo**, se puede crear un array de tipo `Soldado` que contenga objetos de distintas subclases. Aunque todas las referencias son de tipo `Soldado`, en tiempo de ejecución cada objeto responderá con su propia versión del método `saludar()`, gracias a la ligadura dinámica.

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Soldado();
        ejercito[1] = new Zapador();
        ejercito[2] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

Al ejecutar este código, se observará que cada objeto produce una salida distinta según su tipo real. El `Soldado` y el `Artillero` usarán la implementación de la clase base, mientras que el `Zapador` ejecutará su versión redefinida. Esto demuestra cómo una misma llamada (`saludar()`) puede tener comportamientos diferentes dependiendo del objeto, que es precisamente la esencia del polimorfismo.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, al **sobreescribir** un método es posible invocar la implementación de la clase base y, a partir de ahí, añadir o modificar comportamiento. Esto resulta útil cuando no se quiere reemplazar completamente la lógica original, sino extenderla. En lugar de duplicar el código del método base, se reutiliza directamente su implementación y se complementa con nuevas acciones.

En Java, esto se hace utilizando la palabra clave `super`, que permite acceder a los métodos y atributos de la superclase. Al llamar a `super.saludar()`, se ejecuta el método tal como está definido en `Soldado`, y después se puede añadir el comportamiento específico de `Zapador`.

```java
class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma genérica.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();  // llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

De este modo, cuando un objeto de tipo `Zapador` invoque `saludar()`, primero ejecutará el saludo estándar del `Soldado` y, a continuación, añadirá su propio mensaje. La palabra clave utilizada para invocar el método de la clase base es **`super`**, y es fundamental para reutilizar comportamiento heredado sin tener que reescribirlo completamente.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al **sobreescribir** un método en Java, la firma debe coincidir con la de la superclase: mismo nombre y **mismos tipos de parámetros (y en el mismo orden)**. No se permite cambiar la lista de parámetros, porque entonces ya no sería el mismo método. En cuanto al **tipo de retorno**, debe ser el mismo o uno **covariante** (un subtipo del original). Además, no se pueden declarar excepciones comprobadas más generales que las del método base (solo las mismas o más específicas), y la visibilidad no puede ser más restrictiva (por ejemplo, no se puede pasar de `public` a `protected`).

La diferencia entre **sobreescritura (overriding)** y **sobrecarga (overloading)** es fundamental. La sobreescritura ocurre entre clases relacionadas por herencia y redefine un método existente para cambiar su comportamiento en la subclase; está ligada al **polimorfismo** y a la ligadura dinámica. En cambio, la sobrecarga consiste en tener **varios métodos con el mismo nombre pero distinta lista de parámetros** dentro de la misma clase (o heredados), y se resuelve en **tiempo de compilación**. Es decir, en sobrecarga no interviene el tipo real del objeto, sino la firma elegida según los argumentos.

La anotación `@Override` se utiliza para indicar explícitamente que un método pretende **sobreescribir** otro de la superclase. El compilador verifica entonces que realmente existe un método con esa misma firma en la clase base. Si hay un error (por ejemplo, un parámetro mal tipado o un nombre ligeramente distinto), el compilador lo detecta inmediatamente en lugar de dejar pasar un método nuevo que no sobreescribe nada.

Por eso es muy recomendable usar siempre `@Override`: actúa como una **garantía de corrección** y evita errores sutiles difíciles de detectar. Sin esta anotación, un fallo en la firma podría hacer que se estuviera creando un método distinto (sobrecarga involuntaria) en lugar de una verdadera sobreescritura, rompiendo el comportamiento polimórfico esperado.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, en la práctica se está utilizando **polimorfismo desde muy pronto** al estudiar Java, aunque no siempre se nombre explícitamente así al principio. En cuanto se trabaja con herencia y se empiezan a redefinir métodos de una clase base, ya se está aprovechando el mecanismo de ligadura dinámica. Java lo facilita porque muchos métodos de uso común ya están definidos en la clase base java.lang.Object, de la que heredan todas las clases.

Por ejemplo, cuando se **sobreescribe `toString()` o `equals()`**, efectivamente ya se está usando polimorfismo. Aunque se tenga una referencia de tipo `Object` o de una clase más general, al invocar esos métodos se ejecutará la versión específica de la clase real del objeto. Esto encaja exactamente con la idea de “una misma llamada, distintos comportamientos según el objeto”, que es la esencia del polimorfismo.

Sin embargo, al principio suele pasar desapercibido porque no se trabaja todavía con colecciones heterogéneas o jerarquías más complejas donde el efecto se ve más claramente (como arrays o listas de tipo base con objetos de distintos tipos). En esos casos más avanzados, el polimorfismo se vuelve mucho más evidente, ya que permite tratar de forma uniforme objetos distintos sin necesidad de comprobar su tipo manualmente.

En resumen, sí: al sobreescribir métodos como `toString()` o `equals()`, ya se está utilizando polimorfismo, aunque en una forma básica. Es simplemente que más adelante se explota de forma más consciente y potente para diseñar programas más flexibles.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una **clase abstracta** es una clase que no está pensada para crear objetos directamente, sino para servir como base común de otras clases. Puede contener atributos y métodos ya implementados, pero también puede declarar métodos sin implementación que obligan a las subclases a definir su propio comportamiento. Por su parte, un **método abstracto** es un método declarado sin cuerpo (sin código), cuya implementación se deja a las subclases.

No se pueden crear instancias de una clase abstracta. Es decir, no se puede hacer `new Soldado()` si `Soldado` es abstracta. Su función es definir una estructura común y garantizar que todas las clases derivadas implementen ciertos métodos. Esto resulta muy útil para forzar un diseño coherente: todas las subclases deberán proporcionar su propia versión de los métodos abstractos.

Para indicar esto en Java, se utiliza la palabra clave `abstract` tanto en la clase como en el método. Si una clase tiene al menos un método abstracto, obligatoriamente debe declararse también como abstracta. A continuación se muestra el ejemplo modificado:

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma genérica.");
    }

    public abstract void atacar(); // método sin implementación
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El artillero dispara el cañón.");
    }
}
```

En este ejemplo, la palabra clave `abstract` se coloca en la definición de la clase `Soldado` y en el método `atacar()`. Las subclases (`Zapador` y `Artillero`) están obligadas a implementar ese método. De esta forma, se asegura que cualquier tipo de `Soldado` sabrá cómo “atacar”, aunque cada uno lo haga de manera diferente, aprovechando el polimorfismo.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave `final` en Java tiene efectos distintos según dónde se aplique. En una **clase**, indica que no puede ser extendida, es decir, no se pueden crear subclases a partir de ella. En un **método**, significa que no puede ser **sobreescrito** en las subclases. Esto “bloquea” la herencia en ese punto concreto: o bien se impide completamente (clase `final`), o bien se impide modificar el comportamiento de un método concreto.

En relación con el **polimorfismo**, `final` actúa como una limitación. El polimorfismo basado en herencia necesita que las subclases puedan redefinir métodos para ofrecer comportamientos distintos. Si un método es `final`, no puede ser sobreescrito, por lo que no participará en el polimorfismo dinámico. De forma similar, si una clase es `final`, no podrá tener subclases, así que tampoco podrá formar parte de una jerarquía polimórfica basada en herencia.

Esto no significa que `final` sea “malo”; se utiliza cuando se quiere garantizar que cierto comportamiento no sea alterado, ya sea por motivos de diseño (asegurar invariantes) o de seguridad. Por ejemplo, la clase java.lang.String es `final` en la API estándar de Java. Esto asegura que su comportamiento no pueda ser modificado mediante herencia, lo cual es importante porque `String` se usa de forma muy intensiva y su inmutabilidad y seguridad son fundamentales.

En resumen, `final` restringe el uso del polimorfismo al impedir la extensión o la sobreescritura, y se emplea cuando se quiere un diseño más controlado. Es una herramienta para fijar comportamientos y evitar que futuras clases cambien aspectos críticos del funcionamiento de un programa.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
En Java, una **interfaz** es un tipo que define un conjunto de métodos que una clase debe implementar, pero sin proporcionar (en general) la implementación de esos métodos. Es como un “contrato”: cualquier clase que implemente esa interfaz se compromete a ofrecer esas operaciones. A diferencia de lo que se conoce en C/C++, no es una estructura con datos, sino una forma de especificar comportamiento común sin imponer cómo se implementa.

Se parecen a las **clases abstractas**, pero no son lo mismo. Ambas pueden tener métodos sin implementar, pero una clase abstracta puede incluir atributos y métodos con código (estado y comportamiento parcial), mientras que una interfaz está pensada principalmente para definir capacidades. En versiones modernas de Java, las interfaces pueden incluir métodos con implementación (`default` o `static`), pero siguen sin tener estado de instancia como una clase normal. En general, se usa una clase abstracta cuando se quiere compartir código y una interfaz cuando se quiere definir un comportamiento común sin imponer herencia concreta.

Sí, una clase en Java puede **implementar más de una interfaz**, y esto es una de sus principales ventajas. Mientras que una clase solo puede heredar de una única superclase (herencia simple), puede implementar múltiples interfaces, lo que permite combinar distintos comportamientos. Por ejemplo, una clase podría implementar `Serializable`, `Comparable` y otras interfaces, adquiriendo así varias “capacidades” sin necesidad de heredar de múltiples clases.

Esto está muy relacionado con el polimorfismo: una misma clase puede ser tratada como distintos tipos según la interfaz que implemente. Por ejemplo, un objeto puede ser manipulado como un `Comparable` o como otro tipo de interfaz, dependiendo del contexto, permitiendo escribir código más flexible y desacoplado.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
Se puede diseñar una jerarquía donde `Punto` sea una clase abstracta con un método también abstracto `calcularDistanciaA`. De este modo, cada subtipo (`Punto2D`, `Punto3D`) implementa su propia fórmula de distancia. Para garantizar que se comparan puntos del mismo tipo, se utiliza `instanceof` y *downcasting*: primero se comprueba el tipo real y después se convierte para acceder a sus atributos específicos.

```java id="punto_base"
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}
```

A continuación se definen las implementaciones concretas. Cada una verifica que el argumento es del tipo correcto antes de calcular la distancia. Si no lo es, se puede lanzar una excepción para evitar resultados incoherentes.

```java id="puntos_concretos"
class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Tipos incompatibles");
        }
        Punto2D p = (Punto2D) otro; // downcasting
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Tipos incompatibles");
        }
        Punto3D p = (Punto3D) otro; // downcasting
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
```

Con este diseño, se puede crear una clase `Linea` que trabaje de forma **polimórfica** con objetos `Punto`, sin conocer si son 2D o 3D. Simplemente delega el cálculo en el método `calcularDistanciaA`, que será resuelto dinámicamente según el tipo real de los puntos.

```java id="linea"
class Linea {
    private Punto a, b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}
```

De esta forma, `Linea` no necesita saber nada sobre dimensiones ni fórmulas concretas. El polimorfismo permite que funcione correctamente tanto con `Punto2D` como con `Punto3D`, siempre que ambos extremos sean compatibles. El uso de `instanceof` y *downcasting* asegura que cada implementación opere únicamente con su propio tipo, manteniendo la coherencia del cálculo.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La **herencia de interfaces** en Java consiste en que una interfaz puede **extender** a otra (o varias), heredando sus métodos y, por tanto, ampliando el “contrato” que deben cumplir las clases que la implementen. Es un mecanismo similar a la herencia entre clases, pero enfocado únicamente a definir capacidades (métodos), no a compartir estado. De este modo, una interfaz más específica puede construirse a partir de otra más general, añadiendo nuevas operaciones.

Sí, existe **herencia múltiple de interfaces** en Java. A diferencia de las clases (que solo pueden extender una), una interfaz puede extender varias interfaces a la vez. Esto permite combinar distintos comportamientos sin los problemas clásicos de la herencia múltiple de clases. Las clases que implementan esa interfaz heredada deberán implementar todos los métodos definidos en todas las interfaces de las que proviene.

A continuación se muestra un ejemplo sencillo. Primero se define una interfaz `Fichero` con un método para leer contenido como `String`. Luego se crea `FicheroEscribible`, que **extiende** a `Fichero` y añade nuevas operaciones:

```java id="interfaces_fichero"
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}
```

Con este diseño, cualquier clase que implemente `FicheroEscribible` estará obligada a implementar **todos los métodos**: tanto `leer()` (heredado de `Fichero`) como `escribir()` y `eliminar()`. Esto permite modelar distintos niveles de capacidad: un objeto puede ser solo “legible” (`Fichero`) o también “escribible” (`FicheroEscribible`), y el polimorfismo permite tratarlos de forma uniforme según la interfaz que se utilice.
