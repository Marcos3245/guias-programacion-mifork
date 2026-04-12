<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
En orientación a objetos, la **herencia** es un mecanismo que permite definir una clase nueva (subclase) a partir de otra ya existente (superclase), reutilizando su estructura y comportamiento. La relación “A es-un B” significa que un objeto de la subclase puede considerarse también como un objeto de la superclase. Por ejemplo, si `Artillero` hereda de `Soldado`, entonces un artillero **es un** soldado, lo que implica que comparte su identidad básica y puede usarse en cualquier contexto donde se espere un `Soldado`.

La primera implicación importante es la **compatibilidad de tipos**. Esto significa que una referencia de tipo `Soldado` puede apuntar a objetos de cualquiera de sus subtipos (`Artillero`, `Zapador`, etc.). Es una idea clave: permite tratar distintos objetos de forma uniforme, como ya se hacía en C con punteros genéricos, pero aquí de forma segura y estructurada. Gracias a esto, se puede crear un array de `Soldado` y almacenar en él distintos tipos de soldados sin problemas.

La segunda implicación es la **herencia de estado y comportamiento**. La subclase hereda los atributos (estado) y métodos (comportamiento) de la superclase. En este caso, tanto `Artillero` como `Zapador` heredan el atributo privado `nombre` (accesible mediante métodos públicos o protegidos) y el método `saludar()`. Además, cada subclase puede añadir su propio estado específico (cohetes o minas) y comportamiento adicional, lo que permite especializar la funcionalidad sin duplicar código.

```java
// Superclase
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

// Subclase Artillero
class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

// Subclase Zapador
class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

// Uso de compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Soldado("Juan");
        ejercito[1] = new Artillero("Pedro", 5);
        ejercito[2] = new Zapador("Luis", 3);

        for (Soldado s : ejercito) {
            s.saludar(); // Todos pueden saludar
        }
    }
}
```


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Al crear un objeto de una subclase, se ejecutan **tantos constructores como niveles haya en la jerarquía**, comenzando desde la superclase hasta la subclase concreta. Es decir, si se crea un `Artillero`, primero se ejecuta el constructor de `Soldado` y después el de `Artillero`. Este orden es obligatorio porque la parte “base” del objeto debe inicializarse antes de poder completar la parte específica. Por tanto, en el ejemplo dado se ejecutan **dos constructores**, en orden: primero el de `Soldado` y luego el de `Artillero` (o `Zapador`, según corresponda).

La palabra clave `super` dentro de un constructor se utiliza para **invocar explícitamente el constructor de la superclase**. Permite pasar los parámetros necesarios para inicializar correctamente la parte heredada. Además, esta llamada debe aparecer **como primera instrucción del constructor**, ya que Java exige que la inicialización de la superclase ocurra antes que cualquier otra operación. Si no se escribe `super(...)`, el compilador intenta insertar automáticamente una llamada a `super()` (constructor sin parámetros).

Si la clase base **no tiene un constructor sin parámetros accesible (público o protegido)**, entonces es obligatorio llamar a `super` de forma explícita indicando los argumentos adecuados. En caso contrario, el código no compilará, porque el compilador no puede generar la llamada implícita a un constructor inexistente. Por tanto, cuando la superclase solo define constructores con parámetros, siempre se debe invocar manualmente a uno de ellos desde la subclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
En memoria, un objeto de una subclase **incluye también la parte correspondiente a la superclase**. Es decir, al crear un `Artillero` o un `Zapador`, el objeto resultante contiene tanto sus propios atributos como los heredados de `Soldado`. Por tanto, el atributo privado `nombre` **sí forma parte de la instancia en memoria**, aunque haya sido declarado en la superclase. Se puede entender como un único bloque de memoria que integra todos los campos definidos a lo largo de la jerarquía.

Sin embargo, que un atributo exista en memoria **no implica que sea accesible directamente desde el código de la subclase**. En Java, `private` significa que solo es visible dentro de la propia clase donde se declara. Por ello, aunque `Artillero` tenga internamente el campo `nombre` heredado de `Soldado`, no puede acceder a él directamente mediante `this.nombre`. Para interactuar con ese dato, es necesario que la superclase proporcione métodos públicos o protegidos (por ejemplo, un getter o métodos como `saludar()`).

En el ejemplo, cuando se crea un `Artillero`, el atributo `nombre` se inicializa en el constructor de `Soldado` y queda almacenado dentro del objeto. El método `saludar()` funciona correctamente porque pertenece a `Soldado` y sí puede acceder a ese atributo privado. En cambio, si en `Artillero` se intentase imprimir `nombre` directamente, el compilador generaría un error. Esto refuerza la idea de encapsulación: el estado heredado **forma parte del objeto**, pero su acceso sigue estando controlado por la clase que lo define.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
La **compatibilidad de tipos** en herencia implica una gran mejora en la **extensibilidad del código**. Permite escribir código general que trabaja con la superclase (`Soldado`) sin necesidad de conocer todas las subclases concretas. De este modo, el sistema queda **abierto a extensión (nuevos tipos)** pero **cerrado a modificación (el código existente no se toca)**, lo cual es una idea fundamental en diseño orientado a objetos. En términos prácticos, significa que se pueden añadir nuevos tipos de soldados sin tener que reescribir bucles, estructuras de datos ni lógica ya implementada.

Esto se debe a que todos los subtipos comparten una interfaz común (en este caso, el método `saludar()` heredado de `Soldado`). El código que recorre un array de `Soldado` no necesita saber si cada elemento es un `Artillero`, un `Zapador` o cualquier otro subtipo: simplemente invoca `saludar()`, confiando en que todos los objetos cumplen ese contrato. Esta idea es una evolución del uso de funciones genéricas en C, pero aquí se aplica de forma más segura y estructurada gracias al sistema de tipos.

A continuación se añade un nuevo tipo de soldado, por ejemplo `Medico`, sin modificar el código que recorre el array:

```java
// Nueva subclase
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}

// Código principal (NO se modifica la lógica del bucle)
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Artillero("Pedro", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Medico("Ana", 2); // Nuevo tipo añadido
        ejercito[3] = new Soldado("Juan");

        for (Soldado s : ejercito) {
            s.saludar(); // Funciona igual para todos
        }
    }
}
```

Se observa que el único cambio consiste en **añadir una nueva clase y crear objetos de ese tipo**, pero el código que procesa los soldados permanece intacto. Esto demuestra cómo la compatibilidad de tipos facilita la evolución del programa sin romper ni modificar el comportamiento existente.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
En Java, **sí es posible que una referencia del supertipo apunte a un objeto real de un subtipo**. Por ejemplo, una variable de tipo `Soldado` puede referenciar tanto a un `Soldado` como a un `Artillero` o `Zapador`. Esto es precisamente la base de la herencia y la compatibilidad de tipos. Sin embargo, con una referencia de tipo `Soldado` **solo se pueden invocar directamente los métodos definidos en `Soldado`**, aunque el objeto real sea de una subclase. El compilador decide qué métodos son accesibles según el tipo de la referencia, no del objeto real.

El **upcasting** consiste en tratar un objeto de una subclase como si fuera de su superclase (por ejemplo, asignar un `Artillero` a una referencia `Soldado`). Este proceso es automático y seguro. En cambio, el **downcasting** es el proceso inverso: convertir una referencia de supertipo a subtipo (por ejemplo, de `Soldado` a `Artillero`). Este sí requiere un cast explícito y puede ser peligroso si el objeto real no es del tipo esperado. Para evitar errores en tiempo de ejecución, se utiliza `instanceof`, que permite comprobar si un objeto pertenece a una clase concreta antes de hacer el cast.

El operador `instanceof` devuelve `true` si el objeto es instancia de una clase o de alguna de sus subclases. Se usa típicamente antes de un downcasting para garantizar que la conversión es válida. Esto es necesario cuando se quiere acceder a métodos específicos de una subclase (como `getCohetes()` en `Artillero`) desde una referencia de tipo general (`Soldado`).

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Artillero("Pedro", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Soldado("Juan");

        for (Soldado s : ejercito) {
            s.saludar();

            // Comprobación del tipo real
            if (s instanceof Artillero) {
                Artillero a = (Artillero) s; // downcasting
                System.out.println("Cohetes: " + a.getCohetes());
            }
        }
    }
}
```


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso **protegido (`protected`)** es un nivel de visibilidad intermedio entre `private` y `public`. Un miembro declarado como `protected` puede ser accedido **desde la propia clase, desde sus subclases (aunque estén en otros paquetes) y desde otras clases del mismo paquete**. A diferencia de `private`, que restringe el acceso únicamente a la clase donde se declara, `protected` permite que las subclases reutilicen directamente el estado o comportamiento heredado, facilitando la extensión controlada del código.

En Java, se implementa simplemente utilizando la palabra clave `protected` en la declaración de atributos o métodos. Esto resulta útil cuando se desea que una subclase tenga acceso directo a ciertos datos internos sin exponerlos completamente al exterior (como ocurriría con `public`). De este modo, se mantiene la encapsulación hacia fuera, pero se flexibiliza el uso dentro de la jerarquía de herencia.

En el ejemplo, si el atributo `nombre` de `Soldado` se declara como `protected`, la subclase `Zapador` puede utilizarlo directamente en sus propios métodos, como por ejemplo al colocar minas:

```java
// Superclase
class Soldado {
    protected String nombre; // ahora es accesible en subclases

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

// Subclase Zapador
class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }

    public void ponerMina() {
        System.out.println(nombre + " ha colocado una mina");
    }
}
```

En este caso, `Zapador` puede acceder directamente a `nombre` porque es `protected`. Esto simplifica el código (no hace falta un getter), pero también implica una menor ocultación de información que con `private`. Por tanto, su uso debe valorarse cuidadosamente para no romper el encapsulamiento más de lo necesario.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En muchos lenguajes orientados a objetos existe el concepto de una **clase base común** de la que derivan (directa o indirectamente) todos los objetos. Esta clase suele proporcionar comportamiento básico compartido, como comparación, conversión a texto o gestión general. Sin embargo, **no es una regla universal en todos los lenguajes**: algunos lenguajes orientados a objetos más flexibles o híbridos (como C++ sin uso estricto de OOP) no obligan a que todas las clases hereden de una única raíz común.

En Java, sí existe una clase base universal: **Object**. Todas las clases en Java heredan implícitamente de `Object`, aunque no se escriba explícitamente con `extends`. Esto significa que cualquier objeto en Java es, en última instancia, un `Object`. Gracias a esto, todos los objetos comparten ciertos métodos fundamentales como `toString()`, `equals()` o `hashCode()`, que pueden ser reutilizados o redefinidos según las necesidades.

Esta característica refuerza la uniformidad del lenguaje y facilita la generalización. Por ejemplo, se pueden manejar colecciones de objetos sin importar su tipo concreto, ya que todos son instancias de `Object`. En resumen, en Java siempre existe una jerarquía con una raíz común (`Object`), mientras que en otros lenguajes esto puede variar dependiendo de su diseño y de cómo implementen la orientación a objetos.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La **herencia múltiple** es un mecanismo por el cual una clase puede **heredar de más de una superclase a la vez**. Esto significa que una subclase podría reutilizar estado y comportamiento de varias clases base diferentes. Aunque resulta potente, también introduce complejidad, especialmente cuando varias superclases definen métodos o atributos con el mismo nombre (lo que se conoce como ambigüedad o “problema del diamante”).

En **Java no existe la herencia múltiple de clases**. Es decir, una clase solo puede extender directamente de una única superclase (`extends`). Esta decisión de diseño evita conflictos y simplifica el modelo de herencia. Sin embargo, Java sí ofrece una alternativa: una clase puede implementar múltiples interfaces (`implements`), lo que permite heredar **comportamiento abstracto** (y desde Java 8, también métodos por defecto) sin los problemas clásicos de la herencia múltiple de clases.

Por tanto, en Java se combina **herencia simple de clases** con **herencia múltiple de interfaces**. Esto permite lograr gran parte de la flexibilidad de la herencia múltiple, pero de forma más controlada. En el contexto del ejemplo, una clase como `Artillero` solo puede extender de `Soldado`, pero podría implementar varias interfaces adicionales si se desea añadir capacidades específicas sin comprometer la claridad del diseño.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
En Java, las excepciones son objetos y forman parte de la jerarquía cuya raíz es **Object**. Esto permite crear excepciones personalizadas adaptadas al dominio del problema. Una excepción es **no controlada (unchecked)** cuando hereda de `RuntimeException`, lo que implica que **no es obligatorio declararla ni capturarla**. Este tipo de excepciones suele utilizarse para errores de programación o situaciones inesperadas.

Al crear una excepción personalizada, es habitual incluir información adicional que ayude a diagnosticar el problema. En este caso, se puede componer la excepción con un objeto `Usuario`, de forma que la excepción transporte el contexto del error. Además, es buena práctica proporcionar constructores sobrecargados que permitan incluir una **causa subyacente (`Throwable`)**, lo que facilita el encadenamiento de excepciones y el análisis posterior.

A continuación se muestra un ejemplo completo:

```java id="1w8v6s"
// Clase de dominio simple
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

// Excepción personalizada no controlada
class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    // Constructor con causa
    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

En este diseño, la excepción no solo indica el error, sino también **qué usuario concreto lo provocó**, lo cual es especialmente útil en depuración o registro de errores. Además, al permitir una causa (`Throwable`), se puede mantener la traza completa del problema original, siguiendo buenas prácticas en el manejo de excepciones en Java.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
Se considera que **la herencia no debe usarse únicamente para reutilizar código** porque implica una relación estructural fuerte entre clases (“A es-un B”) que afecta directamente al diseño del sistema. Cuando se hereda solo para evitar duplicación de código, se corre el riesgo de crear una jerarquía artificial donde no existe una verdadera relación semántica entre los tipos. Esto puede llevar a diseños rígidos y difíciles de mantener, ya que las clases quedan fuertemente acopladas.

El principal problema es el **acoplamiento fuerte** entre superclase y subclase. La subclase hereda no solo el código, sino también decisiones de diseño de la superclase (estado, comportamiento y posibles cambios futuros). Si la superclase cambia, las subclases pueden verse afectadas de forma inesperada. Además, la herencia expone internamente partes de la implementación (especialmente si se usan miembros `protected`), rompiendo parcialmente la encapsulación. Esto hace que el sistema sea menos flexible a largo plazo.

Por eso, en muchos casos se prefiere la **composición**, que sigue la idea de “tiene-un” en lugar de “es-un”. En composición, una clase reutiliza comportamiento delegando en otra clase en lugar de heredarlo. Esto permite mayor flexibilidad, porque los componentes pueden cambiarse o sustituirse sin modificar la jerarquía de tipos. Por ejemplo, en lugar de crear una clase `Artillero` solo para reutilizar lógica de disparo, podría tener un objeto `Arma` o `SistemaDeDisparo` dentro, lo que desacopla completamente ambas partes y facilita la evolución del sistema.

En resumen, la herencia debe reservarse para relaciones verdaderamente conceptuales (“es-un”), mientras que la composición es preferible para reutilización de código y construcción flexible de comportamientos.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Se dice que se debe **favorecer la composición frente a la herencia** porque la composición permite construir sistemas más **flexibles, mantenibles y con menor acoplamiento**. Mientras que la herencia establece una relación rígida “es-un” entre clases, la composición se basa en “tiene-un”, lo que permite que una clase reutilice comportamiento delegando en otros objetos en lugar de depender estructuralmente de ellos.

El problema principal de la herencia es que crea una **dependencia fuerte con la superclase**. La subclase hereda no solo código, sino también decisiones de diseño que pueden cambiar en el futuro. Si la superclase evoluciona, las subclases pueden romperse o comportarse de forma inesperada. Además, la herencia expone parte de la implementación interna (por ejemplo mediante `protected`), lo que debilita la encapsulación. Esto hace que los cambios sean más costosos y arriesgados.

En cambio, la composición permite **combinar objetos independientes**, lo que reduce el acoplamiento. Cada componente tiene una responsabilidad clara y puede modificarse, sustituirse o ampliarse sin afectar al resto del sistema. Por ejemplo, en lugar de crear una jerarquía `Soldado → Artillero → ArtilleroPesado`, se podría tener una clase `Soldado` que *contiene* un objeto `SistemaDeDisparo`. Así, distintos tipos de soldados se construyen combinando comportamientos en lugar de heredarlos.

En resumen, la composición se prefiere porque proporciona mayor reutilización real sin rigidez estructural, mejora la encapsulación y facilita la evolución del software sin romper código existente.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Cuando se dice que la **herencia rompe la encapsulación**, se hace referencia a que la relación entre superclase y subclase obliga a la subclase a depender de detalles internos de implementación de la clase base. Aunque en teoría la encapsulación busca ocultar el estado interno y exponer solo una interfaz controlada, la herencia introduce un canal directo de acceso y dependencia que debilita este principio.

Esto ocurre porque la subclase no solo usa la interfaz pública de la superclase, sino que también puede verse afectada por su implementación interna. Por ejemplo, si la superclase utiliza métodos `protected` o si la subclase sobreescribe métodos y depende del comportamiento interno de la clase base, cualquier cambio en la superclase puede alterar el funcionamiento de la subclase sin cambiar su interfaz. Esto hace que la subclase “conozca demasiado” sobre cómo está construida la superclase, lo cual contradice la idea de ocultación de información.

Un caso típico es cuando la superclase llama a métodos que la subclase puede sobrescribir (llamadas polimórficas dentro del constructor o métodos de plantilla). La superclase cree estar ejecutando su lógica interna, pero en realidad puede estar ejecutando código redefinido en la subclase, lo que introduce dependencias implícitas difíciles de controlar. Esto rompe la encapsulación porque el comportamiento interno deja de estar completamente bajo control de la clase base.

En resumen, la herencia rompe la encapsulación porque expone la estructura interna de la superclase a las subclases y crea dependencias ocultas entre ambas. Por eso, en muchos casos se prefiere la composición, ya que permite reutilizar comportamiento sin acceder ni depender de la implementación interna de otras clases.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
A continuación se modela el mismo problema de dos formas distintas: **usando herencia** y **usando composición**. En ambos casos se busca reutilizar los datos comunes (`dni` y `nombre`), pero cambia completamente la relación entre las clases.

---

## 1. Solución con **herencia** (Persona como superclase)

En este caso se asume una relación “**Estudiante es una Persona**” y “**Trabajador es una Persona**”. Por tanto, ambos heredan directamente los atributos comunes.

```java
// Superclase
class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public void mostrarDatos() {
        System.out.println(nombre + " (" + dni + ")");
    }
}

// Subclase Estudiante
class Estudiante extends Persona {
    private String carrera;

    public Estudiante(String dni, String nombre, String carrera) {
        super(dni, nombre);
        this.carrera = carrera;
    }
}

// Subclase Trabajador
class Trabajador extends Persona {
    private double salario;

    public Trabajador(String dni, String nombre, double salario) {
        super(dni, nombre);
        this.salario = salario;
    }
}
```

En este modelo, `Estudiante` y `Trabajador` dependen directamente de la estructura de `Persona`. Cualquier cambio en la superclase puede afectar a ambas subclases.

---

## 2. Solución con **composición** (DatosPersonales como objeto reutilizable)

En este caso no se usa “es-un”, sino “**tiene-un**”. Tanto `Estudiante` como `Trabajador` contienen un objeto `DatosPersonales`.

```java
// Clase reutilizable
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }

    public void mostrar() {
        System.out.println(nombre + " (" + dni + ")");
    }
}

// Estudiante usa composición
class Estudiante {
    private DatosPersonales datos;
    private String carrera;

    public Estudiante(DatosPersonales datos, String carrera) {
        this.datos = datos;
        this.carrera = carrera;
    }

    public void mostrar() {
        datos.mostrar();
    }
}

// Trabajador usa composición
class Trabajador {
    private DatosPersonales datos;
    private double salario;

    public Trabajador(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }

    public void mostrar() {
        datos.mostrar();
    }
}
```

---

## Conclusión

En la versión con herencia, la reutilización se consigue mediante una jerarquía rígida donde `Persona` impone su estructura. En cambio, con composición, `DatosPersonales` se reutiliza como un **bloque independiente**, que puede compartirse, modificarse o sustituirse sin afectar la jerarquía de tipos.

Por ello, la composición ofrece mayor flexibilidad y menor acoplamiento, mientras que la herencia es más adecuada cuando existe una relación real y estable de tipo “es-un”.
