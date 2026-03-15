<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta
En C es habitual construir estructuras más complejas **componiendo** otras más simples. Este enfoque se suele describir con la relación *“tiene-un”* o *“tiene-varios”*. Por ejemplo, una línea puede considerarse como un objeto que **tiene dos puntos**, y cada punto a su vez **tiene dos coordenadas** (`x` e `y`). De esta forma se modela la información de forma estructurada y clara.

Para representar esto en C, primero se define una estructura `Punto` con dos coordenadas. Después se define una estructura `Linea` que contiene dos puntos. En este caso se observa claramente la composición: la línea **contiene** dos estructuras `Punto`, en lugar de repetir directamente las coordenadas dentro de la línea.

```c
#include <stdio.h>
#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};
```

A partir de estas estructuras se pueden definir funciones que operen sobre ellas. Una función útil es la que calcula la **distancia entre dos puntos** del plano. Para ello se utiliza la fórmula de la distancia euclídea, que se basa en el teorema de Pitágoras.

```c
double distancia(struct Punto a, struct Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}
```

Finalmente, la longitud de una línea puede calcularse utilizando la función anterior. Como una línea está compuesta por dos puntos (`p1` y `p2`), basta con calcular la distancia entre ellos.

```c
double longitud(struct Linea l) {
    return distancia(l.p1, l.p2);
}
```

De esta manera se observa cómo una estructura más compleja (`Linea`) se construye a partir de otras más simples (`Punto`). Este tipo de composición facilita reutilizar funciones y mantener el código más organizado.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
En Java también se puede modelar la relación *“tiene-un”* mediante **composición** entre clases. En este ejemplo, una línea está formada por dos puntos, por lo que la clase `Linea` **tiene dos objetos `Punto`**. Cada punto contiene sus coordenadas `x` e `y`. A diferencia de C, Java permite aplicar **ocultación de información**, evitando que otros objetos modifiquen directamente los datos internos.

La clase `Punto` puede diseñarse como **inmutable**. Para ello, los atributos se declaran `private` y `final`, lo que impide su modificación tras la creación del objeto. Además, no se proporcionan métodos *setter*. De este modo, una vez creado un punto, sus coordenadas no pueden cambiar. La clase también incluye un método que calcula la distancia a otro punto.

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

La clase `Linea` utiliza **composición**, ya que contiene dos objetos `Punto`. Igual que en el caso anterior, los atributos se declaran `private` y `final` para impedir su modificación después de crear la línea. Esto garantiza que una línea siempre conecte exactamente los mismos dos puntos con los que fue creada.

```java
public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

Gracias a la encapsulación y a la inmutabilidad, el diseño en Java resulta más seguro que en C. No es posible modificar directamente las coordenadas de un punto ni cambiar los extremos de una línea después de su creación. Esto evita estados inconsistentes y permite razonar con mayor facilidad sobre el comportamiento de los objetos que utilizan estas clases.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
En orientación a objetos, la **multiplicidad** indica cuántas instancias de una clase pueden estar relacionadas con una instancia de otra clase dentro de una relación (como composición o asociación). Es una forma de expresar la **cantidad mínima y máxima** de objetos que pueden participar en esa relación. En los diagramas de clases UML suele indicarse con valores como `1`, `0..1`, `1..*` o `*`.

En el ejemplo anterior existe una relación de **composición** entre `Linea` y `Punto`, ya que una línea está formada por puntos. La multiplicidad describe cuántos puntos forman una línea y, al mismo tiempo, a cuántas líneas puede pertenecer un punto según el modelo definido.

Desde **`Linea` hacia `Punto`**, la multiplicidad es **2** (o `2..2`). Esto significa que cada objeto `Linea` está compuesto exactamente por **dos puntos**: el punto inicial (`p1`) y el punto final (`p2`). No puede existir una línea con menos o más puntos dentro de este modelo.

Desde **`Punto` hacia `Linea`**, la multiplicidad es **0..***. Esto indica que un mismo punto puede no pertenecer a ninguna línea o puede formar parte de varias líneas distintas. Por ejemplo, el mismo objeto `Punto` podría utilizarse como extremo de varias líneas diferentes dentro de un programa.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
En orientación a objetos, la **composición** describe una relación *“tiene-un”* entre objetos, pero esta relación puede tener distintos niveles de dependencia entre las partes. Por ello suele distinguirse entre **composición fuerte** y **composición débil**, según el grado en que el objeto contenedor controla la existencia de los objetos que contiene.

La **composición fuerte** implica una relación muy estrecha entre el objeto contenedor y sus componentes. Los objetos contenidos **no tienen sentido por sí mismos fuera del objeto principal** y su ciclo de vida depende completamente de él. Cuando el objeto contenedor se crea, normalmente crea también sus componentes, y cuando el objeto contenedor se destruye, los componentes también dejan de existir. A este tipo de relación se le suele llamar **composición propiamente dicha**.

En cambio, la **composición débil** ocurre cuando un objeto simplemente mantiene referencias a otros objetos, pero **estos pueden existir independientemente**. El objeto contenedor no controla necesariamente su creación ni su destrucción, y los mismos objetos pueden estar asociados a varios contenedores. En este caso los ciclos de vida son independientes: si el objeto contenedor desaparece, los otros objetos pueden seguir existiendo.

Por esta razón, la composición débil suele denominarse **agregación** o **asociación**, mientras que el término **composición** en sentido estricto se reserva para la composición fuerte. La diferencia principal entre ambas está en la **dependencia del ciclo de vida** entre los objetos involucrados.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
Cuando una clase utiliza otra únicamente dentro de sus métodos —por ejemplo, como **parámetro**, como **valor de retorno**, creando un objeto con `new` dentro del método o empleándolo como **variable local**— no se habla de composición, sino de una relación llamada **dependencia**. En este caso, una clase necesita conocer la otra para poder realizar alguna operación, pero no la mantiene como parte de su estado interno.

La **dependencia** indica que una clase **usa temporalmente** otra clase para llevar a cabo cierta funcionalidad. Esta relación es más débil que la composición o la agregación, ya que el objeto utilizado no forma parte permanente del objeto que lo usa. Una vez que termina la ejecución del método, ese objeto puede desaparecer o dejar de utilizarse sin afectar a la estructura del objeto principal.

Por el contrario, en la **composición** (o agregación) los objetos utilizados se almacenan normalmente como **atributos de la clase**. Esto significa que forman parte de la estructura del objeto durante toda su vida, representando una relación más estable del tipo *“tiene-un”*. En ese caso, la relación no es solo de uso momentáneo, sino de pertenencia.

Por tanto, cuando una clase solo **recibe, devuelve o utiliza temporalmente** objetos de otra clase dentro de métodos, se considera una **dependencia**. Este tipo de relación suele representarse en UML con una **flecha discontinua**, indicando que el acoplamiento entre las clases es relativamente débil.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
En el ejemplo de `Linea` y `Punto`, la relación puede implementarse de dos formas según cómo se gestione el **ciclo de vida de los objetos**. Si los puntos solo existen como parte de la línea y son creados dentro de ella, se habla de **composición fuerte**. En cambio, si los puntos pueden existir independientemente y simplemente se pasan a la línea para construirla, se trata de una **composición débil (agregación)**.

En la **composición fuerte**, la clase `Linea` controla completamente la creación de los puntos. En lugar de recibir objetos `Punto`, el constructor recibe las coordenadas y crea internamente los puntos. De esta manera, los puntos solo existen dentro de la línea y su ciclo de vida queda ligado al de ella.

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

En la **composición débil (agregación)**, los puntos se crean fuera de la línea y se pasan como parámetros al constructor. En este caso, los objetos `Punto` pueden existir independientemente de cualquier línea y pueden incluso reutilizarse en varias líneas distintas.

```java
public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```

La diferencia principal entre ambos enfoques es el **control del ciclo de vida de los objetos**. En la composición fuerte, `Linea` crea y posee completamente sus puntos, mientras que en la composición débil los puntos existen por separado y la línea simplemente mantiene referencias a ellos.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
En **Java**, en una relación de **composición fuerte**, el objeto contenedor no destruye explícitamente a los objetos que contiene. Esto ocurre porque Java dispone de **recolección automática de basura (garbage collection)**. Cuando un objeto deja de ser accesible desde el programa, el recolector de basura puede liberar automáticamente la memoria que ocupa.

En el ejemplo de `Linea` y `Punto`, la clase `Linea` crea internamente los objetos `Punto`. Mientras exista la instancia de `Linea`, también existirán referencias a esos puntos (`p1` y `p2`). Cuando la línea deja de utilizarse y ya no existen referencias a ella, el objeto `Linea` pasa a ser **inaccesible** para el programa. En ese momento, también se pierden las únicas referencias a los objetos `Punto` que contenía.

Como consecuencia, los objetos `Punto` también se vuelven inaccesibles y el **garbage collector** podrá liberar su memoria en algún momento posterior. Por este motivo no se necesita (ni se puede normalmente) escribir código explícito para destruirlos, como sí ocurre en lenguajes como C o C++ donde la gestión de memoria es manual.

Por tanto, en Java la composición fuerte se expresa principalmente a través del **diseño del programa** (quién crea los objetos y quién mantiene las referencias), mientras que la destrucción real de los objetos se gestiona automáticamente mediante el sistema de recolección de basura del lenguaje.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
Un ejemplo de **composición débil (agregación)** puede darse entre un `Departamento` y varios `Profesor`. Los profesores existen independientemente del departamento: pueden crearse fuera y después añadirse al departamento. El departamento simplemente mantiene referencias a ellos. Además, el departamento tiene un **director**, que también es un `Profesor`, pero debe cumplir la invariante de que **siempre pertenece a la lista de profesores del departamento**.

Para mantener la **encapsulación**, la implementación interna usa un array `Profesor[]` con capacidad máxima 50, pero este detalle no se expone al exterior. En su lugar, se proporcionan métodos para **añadir profesores**, **eliminar por posición**, **consultar cuántos hay** y **obtener uno por índice**. También se controla mediante excepciones que no se rompa la invariante: el departamento siempre debe tener director y el director siempre debe estar dentro de la lista.

```java
public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

La clase `Departamento` mantiene dos relaciones: una con **todos los profesores** y otra con **el director**. El constructor exige un director inicial y lo añade automáticamente a la lista de profesores. Se comprueba también que no se supere la capacidad máxima y que las posiciones solicitadas sean válidas.

```java
public class Departamento {

    private Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor director) {
        if (director == null) {
            throw new IllegalArgumentException("Debe existir un director inicial");
        }
        this.director = director;
        profesores[numProfesores++] = director;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException();
        }
        return profesores[pos];
    }

    public Profesor getDirector() {
        return director;
    }

    public void añadirProfesor(Profesor p) {
        if (numProfesores >= 50) {
            throw new IllegalStateException("Capacidad máxima alcanzada");
        }
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException();
        }
        if (profesores[pos] == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }

        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        boolean encontrado = false;

        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }

        if (!encontrado) {
            throw new IllegalArgumentException(
                "El director debe ser un profesor del departamento");
        }

        director = nuevoDirector;
    }
}
```

Este diseño ilustra dos **composiciones débiles simultáneas**: una entre `Departamento` y el conjunto de `Profesor`, y otra entre `Departamento` y su `director`. Los profesores pueden existir fuera del departamento y reutilizarse en otros contextos, lo que caracteriza la agregación. Al mismo tiempo, el código asegura mediante comprobaciones que la **invariante de clase** se mantenga siempre: el departamento tiene director desde su creación y dicho director forma parte de la lista de profesores.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
En Java es habitual utilizar la interfaz `List` (por ejemplo, con la clase `ArrayList`) para gestionar colecciones de objetos. Esto evita tener que manejar manualmente arrays, contadores de elementos y desplazamientos al eliminar posiciones. Desde el punto de vista del diseño, la relación entre `Departamento` y `Profesor` sigue siendo una **composición débil**, ya que los profesores se crean fuera y el departamento solo mantiene referencias a ellos.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {

    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor director) {
        if (director == null) {
            throw new IllegalArgumentException("Debe existir un director inicial");
        }
        this.director = director;
        profesores.add(director);
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public Profesor getDirector() {
        return director;
    }

    public void añadirProfesor(Profesor p) {
        if (profesores.size() >= 50) {
            throw new IllegalStateException("Capacidad máxima alcanzada");
        }
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        profesores.remove(pos);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException(
                "El director debe ser un profesor del departamento");
        }
        director = nuevoDirector;
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
```

Al utilizar `List`, se evita gran parte del código necesario con arrays. Ya no es necesario mantener una variable como `numProfesores`, ni escribir bucles para desplazar elementos al eliminar uno. Las operaciones de añadir, eliminar, contar o acceder por posición ya están implementadas mediante métodos como `add`, `remove`, `size` o `get`. Esto simplifica el código y reduce la probabilidad de errores.

Si existiera un método que devolviera **todos los profesores**, no sería buena idea devolver directamente la lista interna (`return profesores`). El problema es que el código que reciba esa lista podría **modificarla directamente** (añadir o eliminar profesores), rompiendo la encapsulación e incluso la invariante del sistema, por ejemplo eliminando al director sin control.

Para evitarlo, se puede devolver una **vista no modificable** de la lista interna, como se hace en el método `getProfesores()` mediante `Collections.unmodifiableList(profesores)`. De esta forma el exterior puede recorrer o consultar la lista, pero cualquier intento de modificarla producirá una excepción, preservando así la encapsulación del objeto `Departamento`.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
En Java, se pueden modelar **composiciones recursivas** cuando un objeto contiene una referencia a otro objeto de su mismo tipo. Esto permite representar jerarquías o relaciones anidadas de manera natural. En este ejemplo, una `Persona` tiene un atributo `madre` de tipo `Persona`, y la clase se diseña como **inmutable**: sus atributos son `final` y no existen métodos que permitan modificarla después de la creación.

```java id="g3kpt1"
public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    @Override
    public String toString() {
        return nombre + (madre != null ? ", hija de " + madre.nombre : "");
    }
}
```

Un ejemplo de uso con una familia, desde la abuela hasta el nieto, muestra cómo se pueden encadenar estas referencias de manera recursiva:

```java id="n6skla"
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Abuela", null);
        Persona madre = new Persona("Madre", abuela);
        Persona nieto = new Persona("Nieto", madre);

        System.out.println(nieto); // Imprime: Nieto, hija de Madre
        System.out.println(nieto.getMadre().getNombre()); // Madre
        System.out.println(nieto.getMadre().getMadre().getNombre()); // Abuela
    }
}
```

En este diseño, cada `Persona` es inmutable y puede existir como un nodo en una **cadena de composición recursiva**, ya que `madre` puede ser otra `Persona`, y así sucesivamente hasta que la referencia sea `null`. Esto recuerda al comportamiento de las **excepciones encadenadas** en Java (`Throwable` con `getCause()`), donde una excepción puede contener otra excepción como causa.

Otros ejemplos clásicos de **composiciones recursivas** incluyen:

* **Árboles binarios** o n-arios, donde cada nodo contiene referencias a otros nodos del mismo tipo.
* **Listas enlazadas**, donde cada nodo contiene un objeto del mismo tipo o una referencia al siguiente nodo.
* **Directorios y archivos**, donde un directorio puede contener subdirectorios que a su vez contienen archivos o directorios.
* **Excepciones encadenadas** en Java, donde una `Throwable` puede tener otra `Throwable` como causa.

Estos ejemplos muestran cómo la composición recursiva permite modelar relaciones jerárquicas o secuenciales de forma elegante y segura.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
Una relación de composición **bidireccional** es aquella en la que ambos objetos involucrados mantienen referencias entre sí de manera explícita. No solo el objeto contenedor conoce a sus componentes, sino que cada componente también **sabe a qué contenedor pertenece**. Esto permite navegar la relación desde cualquier extremo, pero introduce complejidad adicional porque hay que mantener la consistencia entre las dos referencias: si se añade o elimina un objeto de un lado, la otra referencia debe actualizarse automáticamente.

En el ejemplo de `Profesor` y `Departamento`, actualmente la relación es **unidireccional**: el `Departamento` conoce a sus `Profesor` y al `director`, pero los profesores no saben a qué departamento pertenecen. Para hacerla **bidireccional**, se podría añadir un atributo `departamento` en la clase `Profesor`. Al mismo tiempo, cualquier método que añada, elimine o cambie un profesor en el departamento debería actualizar esta referencia en el profesor correspondiente.

Por ejemplo, se podrían implementar cambios como estos:

1. Añadir en `Profesor` un atributo `Departamento departamento` y un método `setDepartamento(Departamento d)` privado o package-private.
2. Modificar el método `añadirProfesor` de `Departamento` para que, además de añadir al profesor a la lista, **asigne el departamento al profesor**.
3. Modificar el método `eliminarProfesor` para que **ponga la referencia a null** en el profesor eliminado.
4. Controlar invariantes, como que el director siempre pertenezca al departamento, para que la referencia bidireccional siga siendo consistente.

De esta forma, la relación se convierte en bidireccional: desde el `Departamento` se puede acceder a todos los `Profesor`, y desde cualquier `Profesor` se puede saber a qué `Departamento` pertenece, garantizando que ambas referencias reflejen siempre la misma composición. Esto es útil, por ejemplo, para consultas rápidas o para mantener la coherencia de los datos, pero hay que ser cuidadoso para evitar inconsistencias o ciclos infinitos en operaciones recursivas.
