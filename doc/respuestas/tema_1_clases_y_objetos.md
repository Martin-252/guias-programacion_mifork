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

Las cuatro características básicas de la programación orientada a objetos son **abstracción**, **encapsulamiento**, **herencia** y **polimorfismo**. La *abstracción* consiste en identificar las partes esenciales de un objeto y ocultar los detalles irrelevantes. En lugar de mostrar cómo funciona internamente algo, se ofrece una interfaz simple que permite usarlo sin necesidad de conocer su complejidad. Esto facilita centrarse en qué hace un objeto más que en cómo lo hace.

El *encapsulamiento* se refiere a agrupar los datos y las funciones que operan sobre esos datos dentro de una misma entidad, normalmente una clase. Además, permite controlar el acceso a la información mediante modificadores de visibilidad, evitando cambios no deseados desde el exterior. De esta forma se protege el estado interno de los objetos, permitiendo interactuar con ellos únicamente a través de métodos bien definidos.

La *herencia* permite crear nuevas clases basadas en clases existentes, reutilizando código y comportamientos ya definidos. Gracias a esta característica, una clase “hija” puede extender o modificar las funcionalidades de su clase “padre” sin necesidad de escribir todo desde cero. Esto promueve la organización del código y facilita la creación de jerarquías lógicas de objetos.

Por último, el *polimorfismo* permite que distintas clases respondan de forma diferente a un mismo mensaje o método. Esto significa que una misma operación puede tener comportamientos distintos según el tipo concreto del objeto que la ejecuta. Esta característica hace posible diseñar programas más flexibles, donde el código que utiliza objetos no necesita conocer su tipo exacto para trabajar con ellos.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Cuatro lenguajes populares que permiten la programación orientada a objetos son **Java**, **C++**, **Python** y **C#**. Java se diseñó desde el principio con la orientación a objetos como base, por lo que todo su modelo de programación gira en torno a clases y objetos. Resulta especialmente adecuado para introducirse en este paradigma, ya que obliga a estructurar el código siguiendo estos conceptos desde el primer momento.

C++ combina la programación estructurada tradicional con un modelo de objetos muy potente. Esto permite reutilizar conocimientos previos de C y, al mismo tiempo, incorporar características como clases, herencia y polimorfismo. Gracias a su flexibilidad y rendimiento, se mantiene como uno de los lenguajes más utilizados en aplicaciones de alto rendimiento y sistemas complejos.

Python ofrece un enfoque más sencillo y accesible a la programación orientada a objetos. Aunque permite otros estilos de programación, su modelo de clases es fácil de utilizar y comprender, lo que lo convierte en una opción habitual en entornos educativos y de desarrollo rápido. Además, su sintaxis clara favorece la comprensión del funcionamiento de los objetos.

C#, desarrollado por Microsoft, también se basa en el paradigma orientado a objetos y posee una sintaxis similar a la de Java. Se utiliza ampliamente en el desarrollo de aplicaciones empresariales, videojuegos y software para Windows. Su diseño moderno y su amplio ecosistema de herramientas facilitan la creación de proyectos complejos siguiendo principios de diseño orientados a objetos.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La **programación estructurada** es un paradigma anterior a la orientación a objetos que organiza el código mediante un conjunto limitado de estructuras de control: secuencia, selección y repetición. Su objetivo principal es reducir el uso de saltos incondicionales como `goto`, favoreciendo un flujo de ejecución claro y predecible. Esto permite crear programas más fáciles de leer, depurar y mantener, ya que cada parte del código sigue un camino lógico bien definido.

Por su parte, la **programación modular** va un paso más allá al dividir un programa en unidades independientes llamadas *módulos* o *funciones*. Cada módulo se encarga de una tarea específica, lo que facilita la reutilización del código y mejora la organización general del programa. Además, permite trabajar en partes aisladas sin afectar al resto, favoreciendo el mantenimiento y la escalabilidad. Este enfoque introduce una forma de estructura que, aunque menos avanzada que las clases de la POO, comparte la idea de agrupar funcionalidad coherente.

Ambos paradigmas se complementan: la programación estructurada proporciona la base lógica para controlar el flujo, mientras que la modular ofrece una manera de organizar el programa en bloques de responsabilidad clara. Juntos representan los cimientos sobre los que se construyó más adelante la programación orientada a objetos, que extiende estas ideas incorporando conceptos como encapsulamiento y reutilización a través de clases y objetos.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

En programación orientada a objetos, un objeto se define por **tres elementos fundamentales**: *atributos*, *métodos* e *identidad*. Cada uno de ellos contribuye a representar de forma completa a una entidad dentro del programa. Los atributos permiten almacenar el estado, los métodos describen el comportamiento y la identidad diferencia a cada objeto incluso cuando comparte valores y métodos con otros.

Los **atributos** representan las propiedades o datos internos de un objeto. Son equivalentes a las variables que contienen la información relevante para describir su estado en un momento dado. Por ejemplo, en un objeto que represente un coche, los atributos podrían ser la matrícula, la velocidad o el nivel de combustible. Estos valores pueden cambiar durante la ejecución del programa, reflejando la evolución del objeto.

Los **métodos** son las funciones asociadas a un objeto que definen su comportamiento. A través de ellos, el objeto puede realizar acciones, modificar sus atributos o interactuar con otros objetos. Siguiendo el ejemplo anterior, un coche podría tener métodos como *acelerar()*, *frenar()* o *repostar()*. Estos métodos permiten controlar de forma ordenada cómo se altera el estado interno del objeto.

Por último, la **identidad** distingue a un objeto de todos los demás, incluso cuando comparten atributos con los mismos valores. En lenguajes orientados a objetos, esta identidad suele corresponder a la dirección de memoria o a un identificador interno único. Gracias a este concepto, es posible diferenciar, por ejemplo, dos objetos *Coche* con la misma marca y color, ya que cada uno representa una instancia distinta dentro del programa.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una **clase** es una plantilla o modelo que define cómo serán los objetos que se creen a partir de ella. En dicha plantilla se especifican los atributos que describen el estado y los métodos que determinan el comportamiento. Puede entenderse como un plano: indica qué características y acciones tendrá un tipo de entidad dentro del programa, pero por sí sola no ocupa memoria ni representa algo concreto en la ejecución.

Un **objeto**, en cambio, es una realización concreta de la clase. Corresponde a una entidad que existe en memoria y posee valores específicos en sus atributos. Aunque varios objetos se basen en la misma clase, cada uno mantiene su propio estado y se comporta de forma independiente. Por tanto, una clase y un objeto no son lo mismo: la clase es el molde y el objeto es el resultado final construido a partir de dicho molde.

El término **instancia** se usa como sinónimo de objeto, resaltando que dicho objeto “es una instancia de” una clase determinada. Esto indica que el objeto pertenece a un tipo concreto y ha sido creado siguiendo la definición proporcionada por la clase. Cada vez que se crea un objeto, se está generando una nueva instancia con su propia identidad y su propio estado interno.

No todos los lenguajes orientados a objetos utilizan el concepto de clase de la misma manera o incluso no lo emplean en absoluto. Algunos lenguajes, como Java o C#, son estrictamente basados en clases. Otros, como JavaScript, utilizan un modelo orientado a objetos basado en **prototipos**, donde no se definen clases tradicionales, sino que los objetos heredan directamente de otros objetos. Esto demuestra que, aunque el paradigma orientado a objetos es común, su implementación puede variar según el lenguaje.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

En la mayoría de lenguajes orientados a objetos, los objetos se almacenan en memoria dinámica, normalmente en el **heap**. Este espacio se utiliza para datos cuyo tamaño o duración no se conoce en tiempo de compilación, como ocurre con las instancias creadas durante la ejecución. Al crearse un objeto, el lenguaje reserva un bloque de memoria en el heap y devuelve una referencia o puntero que permite acceder a él. Esto permite que los objetos tengan una vida útil flexible y no ligada estrictamente al ámbito de una función.

Sin embargo, **no todos los lenguajes gestionan la memoria de la misma manera**. En C++, por ejemplo, un objeto puede almacenarse tanto en el stack como en el heap, dependiendo de cómo se declare. Un objeto declarado como variable local de una función normalmente se guarda en el stack, mientras que uno creado con `new` se aloja en el heap. Otros lenguajes como Java o C# almacenan todos los objetos en el heap y gestionan automáticamente su ciclo de vida. Por tanto, la ubicación en memoria depende del modelo concreto definido por el lenguaje y no es uniforme en todos ellos.

La **recolección de basura** (*garbage collection*) es un mecanismo automático de gestión de memoria utilizado por lenguajes como Java, C# o Python. Su función principal consiste en identificar objetos que ya no están siendo utilizados por el programa y liberar el espacio que ocupan en el heap. De este modo, se evita que el programador tenga que liberar manualmente la memoria, reduciendo errores como fugas de memoria o accesos a zonas ya liberadas.

Este proceso se ejecuta en segundo plano y sigue diferentes estrategias según la implementación del lenguaje, como la detección de objetos inalcanzables o el uso de contadores de referencias. Aunque la recolección de basura simplifica la gestión de memoria, también introduce un pequeño coste de rendimiento, ya que requiere tiempo de CPU para analizar qué objetos pueden eliminarse. Aun así, constituye una ventaja significativa en términos de seguridad y facilidad de programación, especialmente para quienes comienzan en la programación orientada a objetos.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un **método** es una función que forma parte de una clase y define las acciones que pueden realizar los objetos creados a partir de ella. Actúa como el mecanismo que permite modificar los atributos del objeto, consultar su estado o ejecutar comportamientos específicos relacionados con su propósito. De este modo, los métodos son esenciales para describir cómo interactúan los objetos entre sí y cómo evolucionan durante la ejecución del programa. Sin la existencia de métodos, una clase se limitaría a ser un conjunto pasivo de datos sin capacidad de operar sobre ellos.

La **sobrecarga de métodos** consiste en definir varios métodos con el mismo nombre dentro de una misma clase, siempre que sus parámetros sean diferentes en número, tipo o ambos. Esta característica permite ofrecer variantes de una misma acción sin necesidad de inventar nombres nuevos, manteniendo así el código más claro y coherente. Cuando se realiza una llamada a un método sobrecargado, el compilador selecciona automáticamente la versión adecuada en función de los argumentos utilizados. De esa manera se facilita la flexibilidad y se mejora la legibilidad del diseño orientado a objetos.

Además, la sobrecarga contribuye a que una clase sea más expresiva, ya que un mismo comportamiento puede adaptarse a distintos contextos según los datos disponibles. Esto resulta especialmente útil en lenguajes como Java, donde no existe la posibilidad de definir funciones globales y las acciones deben organizarse dentro de clases. Gracias a la sobrecarga, se puede mantener una interfaz uniforme para operaciones conceptualmente similares, evitando redundancia y mejorando la organización interna del código.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

A continuación se muestra un ejemplo mínimo de una clase **Java** llamada `Punto` con dos atributos `x` e `y` (con **visibilidad por defecto**, es decir, sin modificadores como `private` o `public`), y un método `calculaDistanciaAOrigen()` que devuelve la distancia al origen `(0,0)`. El método utiliza la fórmula de la distancia euclídea, aplicando `Math.sqrt(x*x + y*y)` para calcular la raíz cuadrada de la suma de los cuadrados.

Se incluye además un ejemplo de uso con una instancia, asignando valores a `x` e `y`, invocando el método y mostrando el resultado por consola. Para mayor simplicidad, el `main` se coloca en una clase separada (`App`) dentro del mismo paquete por defecto, lo cual permite acceder a los atributos con visibilidad por defecto.

```java
// Clase Punto (atributos con visibilidad por defecto)
class Punto {
    int x; // visibilidad por defecto (package-private)
    int y; // visibilidad por defecto (package-private)

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

```java
// Ejemplo de uso
public class App {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;  // acceso permitido por visibilidad por defecto en el mismo paquete
        p.y = 4;

        double d = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + d); // debería imprimir 5.0
    }
}
```



## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

En Java, el punto de entrada de un programa es el método `main` con la firma exacta `public static void main(String[] args)` dentro de alguna clase pública (o accesible). La JVM busca ese método para comenzar la ejecución; debe ser `public` para que la JVM pueda invocarlo, `static` para poder llamarlo sin necesidad de crear un objeto, `void` porque no devuelve resultado, y recibe un array de `String` con los argumentos de la línea de comandos. Existen variantes permitidas como `String... args` (varargs), pero el modificador `static` y la visibilidad pública se mantienen como requisitos para que se reconozca como punto de entrada.

El modificador **`static`** indica que un miembro (método, campo o bloque) pertenece a la **clase** y no a las **instancias**. Un método `static` puede llamarse sin crear objetos y no puede acceder directamente a atributos ni métodos de instancia (porque no tiene un `this` disponible). Un campo `static` es compartido por todas las instancias de la clase (existe una única copia). También existen *bloques estáticos* que se ejecutan una vez al cargar la clase, útiles para inicializaciones que no dependen de objetos concretos.

No se emplea `static` únicamente en el `main`. Es habitual usar **métodos estáticos** para utilidades puras (por ejemplo, `Math.sqrt`), **campos estáticos** para valores compartidos o contadores globales, y **métodos de fábrica** (`static`) que crean instancias controlando su construcción. Asimismo, puede declararse **clases anidadas estáticas** (static nested classes), que no requieren una instancia de la clase externa para existir y resultan útiles para agrupar tipos relacionados sin la semántica de “pertenecer a una instancia”.

La combinación **`static final`** se utiliza típicamente para **constantes** de clase (por ejemplo, `static final double PI = 3.1415926535;`), garantizando que el valor es único para la clase y no puede modificarse. Por su parte, `final` por sí solo significa cosas distintas según dónde se aplique: en **variables**, que no pueden cambiar su referencia/valor tras asignarse; en **métodos**, que no pueden sobrescribirse en clases hijas; y en **clases**, que no pueden heredarse. Un método `static` también puede ser `final` para impedir que se oculte (*method hiding*) en subclases, y una clase anidada puede ser `static final` si se desea que sea de clase y no extensible.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para compilar y ejecutar un programa Java desde la línea de comandos se emplean los comandos **`javac`** y **`java`**. Primero se guarda el código fuente en un fichero con extensión `.java` y se compila con `javac NombreFichero.java`, lo que genera uno o varios ficheros `.class`. Estos contienen el *byte-code* que entiende la máquina virtual de Java. Una vez compilado, la ejecución se realiza usando `java NombreDeLaClaseQueContieneMain`, indicando únicamente el nombre de la clase y no el fichero. De este modo, se separa claramente el proceso de compilación del de ejecución, lo que refleja la arquitectura de Java basada en dos fases distintas.

Java se considera un lenguaje **compilado e interpretado a la vez**, porque su código fuente no se traduce directamente a código máquina nativo. En lugar de ello, el compilador `javac` produce *byte-code*, un código intermedio independiente de la plataforma. Posteriormente, la ejecución real la lleva a cabo la *Java Virtual Machine* (JVM), que interpreta o compila just-in-time ese *byte-code* a instrucciones específicas de la máquina física. Esta estrategia permite escribir un programa una sola vez y ejecutarlo en cualquier sistema donde exista una JVM compatible, cumpliendo el principio “write once, run anywhere”.

La **máquina virtual de Java (JVM)** es un componente de software encargado de ejecutar el *byte-code*. Funciona como un entorno de ejecución aislado que provee seguridad, manejo automático de memoria, recolección de basura y otras facilidades del lenguaje. Gracias a esta abstracción, los programas Java no dependen de detalles del sistema operativo o la arquitectura hardware, ya que es la JVM la que se adapta a cada plataforma. Esto explica por qué Java se considera altamente portátil y consistente entre entornos.

El **byte-code** es el formato intermedio generado por el compilador, diseñado para ser eficiente y compacto. Dicho byte-code se almacena en ficheros con extensión **`.class`**, cada uno representando la definición compilada de una clase concreta. Estos ficheros son los que realmente ejecuta la JVM, ya sea interpretándolos instrucción por instrucción o convirtiéndolos dinámicamente en código nativo mediante técnicas como el *Just-In-Time compilation*. Por este motivo, la separación entre código fuente `.java` y código ejecutable `.class` es esencial en el ciclo de vida de un programa Java.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

En Java, **`new`** es el operador que **reserva memoria en el heap** y **crea una instancia** (objeto) de una clase, devolviendo una **referencia** a ese objeto. A diferencia de C++ (donde puede crearse un objeto automático en el stack sin `new`), en Java *todas* las instancias de clase se crean con `new` (o con mecanismos equivalentes como fábricas o reflexión), y la memoria se gestiona automáticamente con recolección de basura. Por ejemplo, `new Punto()` crea un objeto `Punto`, inicializa sus campos a valores por defecto y devuelve la referencia para poder usarlo.

Un **constructor** es un bloque especial de código dentro de una clase que **inicializa el objeto** en el momento de su creación. Tiene el **mismo nombre que la clase**, **no tiene tipo de retorno** (ni siquiera `void`) y puede estar **sobrecargado** para ofrecer distintas formas de inicialización (por ejemplo, con o sin parámetros). Si no se declara ninguno, Java genera un **constructor por defecto** sin parámetros que deja los campos con sus valores por defecto (cero para tipos numéricos, `false` para `boolean`, `null` para referencias). Declarar constructores explícitos permite asegurar que el objeto nazca coherente con valores válidos.

A continuación, un ejemplo de clase `Empleado` con **DNI**, **nombre** y **apellidos**, mostrando un **constructor por parámetros** y, opcionalmente, un **constructor por defecto**. Se incluye además un ejemplo de uso que crea una instancia con `new` y muestra sus datos:

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor por defecto (opcional)
    Empleado() {
        // Valores iniciales simples
        this.dni = "";
        this.nombre = "";
        this.apellidos = "";
    }

    // Constructor con parámetros
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    // Método de utilidad para mostrar datos
    void mostrar() {
        System.out.println(dni + " - " + nombre + " " + apellidos);
    }
}
```

```java
public class App {
    public static void main(String[] args) {
        // Crear un Empleado usando el constructor con parámetros
        Empleado e = new Empleado("12345678A", "Ana", "Pérez López");
        e.mostrar();

        // Crear un Empleado con el constructor por defecto y asignar después
        Empleado e2 = new Empleado();
        e2.dni = "87654321B";
        e2.nombre = "Luis";
        e2.apellidos = "García Soto";
        e2.mostrar();
    }
}
```


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La referencia **`this`** en Java señala **al objeto actual** dentro de un contexto de instancia. Permite acceder a los **atributos** y **métodos** del propio objeto y se utiliza especialmente cuando hay **sombras de nombres** (por ejemplo, parámetros del constructor con el mismo nombre que los campos). También sirve para pasar la propia referencia como argumento a otros métodos, y para **encadenar constructores** con `this(...)` (llamar a otro constructor de la misma clase desde un constructor).

No se llama igual en todos los lenguajes, aunque la idea es similar. En C++ y C# también se usa `this`. En Python la referencia explícita al objeto se llama `self` y se pasa como primer parámetro de los métodos de instancia. En JavaScript, `this` existe pero su valor depende del **contexto de llamada** (ligado dinámicamente salvo que se use `=>` o `bind/call/apply`), lo que difiere del comportamiento más estable de Java.

A continuación, un ejemplo de uso de `this` en la clase `Punto`. Se muestran: (1) un **constructor** que desambigua `x` e `y` usando `this.x = x;`, (2) un **método** que llama a otro método del mismo objeto mediante `this`, y (3) un ejemplo de **encadenamiento de constructores** con `this(0, 0)`:

```java
class Punto {
    int x; // visibilidad por defecto (package-private)
    int y;

    // Constructor por defecto: delega en el constructor principal
    Punto() {
        this(0, 0); // llama al otro constructor de la misma clase
    }

    // Constructor principal con parámetros (desambiguación con this)
    Punto(int x, int y) {
        this.x = x; // this.x = campo; x = parámetro
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Método que usa this para llamar a otro método del mismo objeto
    double distanciaAOtro(Punto otro) {
        // reutiliza el propio método calculaDistanciaAOrigen sobre un vector traslación
        int dx = otro.x - this.x;
        int dy = otro.y - this.y;
        // crea un punto "traslación" y reutiliza el cálculo
        Punto traslacion = new Punto(dx, dy);
        return traslacion.calculaDistanciaAOrigen();
    }
}
```

```java
public class App {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p.calculaDistanciaAOrigen()); // 5.0

        Punto q = new Punto(); // (0,0) por encadenamiento de constructores
        System.out.println(q.distanciaAOtro(p)); // 5.0
    }
}
```


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

Se puede añadir un método `distanciaA(Punto otro)` que calcule la distancia entre el objeto actual (`this`) y el punto recibido como parámetro. Para ello, se obtiene el vector diferencia `(dx, dy)` entre ambos puntos y se aplica la fórmula de la distancia euclídea: `sqrt(dx*dx + dy*dy)`. Este método es de instancia y utiliza los atributos `x` e `y` del propio objeto junto con los del parámetro.

A continuación, se muestra la clase `Punto` (con atributos de **visibilidad por defecto**) incluyendo el nuevo método, y un ejemplo de uso creando dos instancias y calculando la distancia entre ellas:

```java
// Clase Punto (atributos con visibilidad por defecto)
class Punto {
    int x; // package-private (por defecto)
    int y; // package-private (por defecto)

    Punto() {
        this(0, 0);
    }

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Nuevo método: distancia entre this y el punto 'otro'
    double distanciaA(Punto otro) {
        int dx = otro.x - this.x;
        int dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

```java
// Ejemplo de uso
public class App {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        Punto q = new Punto(0, 0);

        System.out.println("Distancia de p al origen: " + p.calculaDistanciaAOrigen()); // 5.0
        System.out.println("Distancia entre p y q: " + p.distanciaA(q));                // 5.0

        Punto r = new Punto(6, 8);
        System.out.println("Distancia entre p y r: " + p.distanciaA(r));                // 5.0
    }
}
```


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java, **todo se pasa por valor**. Cuando se entrega un `Punto` como parámetro, en realidad se pasa **por valor la referencia** al objeto. Esto implica que, dentro del método, se está apuntando al **mismo objeto** en memoria: si se modifican sus **atributos** (`x`, `y`), esos cambios **sí** se verán fuera del método. En cambio, si dentro del método se **reasigna** el parámetro (`otro = new Punto(...)`), esa nueva referencia solo afecta a la variable local del método y **no** sustituye el objeto que el llamador tenía.

Por el contrario, cuando el parámetro es un **primitivo** como `int`, también se pasa **por valor**, pero aquí el “valor” es el número en sí. Si se cambia el `int` dentro del método, esa modificación **no** afecta a la variable original del llamador, porque se trabaja con una **copia independiente**. Este comportamiento contrasta con C++: allí existen referencias verdaderas y punteros; en Java no hay punteros visibles y las “referencias de objeto” se comportan como **valores que apuntan** a un mismo objeto.

Ejemplo con objeto (`Punto`): modificar atributos **sí** persiste fuera; **reasignar** la referencia no:

```java
void mueveDerecha(Punto p) {
    p.x += 10;          // afecta al objeto original
    p = new Punto(0,0); // NO afecta al objeto del llamador (solo cambia la referencia local)
}
```

Ejemplo con `int`: la modificación **no** persiste fuera:

```java
void incrementa(int n) {
    n += 1; // solo cambia la copia local; el original en el llamador no se modifica
}
```


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

El método **`toString()`** en Java es el mecanismo estándar para obtener una **representación textual** de un objeto. Todas las clases heredan una versión por defecto desde `java.lang.Object`, que devuelve algo poco informativo (el nombre de la clase y un código hash en hexadecimal). Por ello, se suele **sobrescribir** (`@Override`) para devolver un texto legible que describa el estado del objeto. Esta representación se usa automáticamente al imprimir con `System.out.println(obj)`, al concatenar con cadenas (`"..." + obj`) o en registradores (*loggers*).

Conceptos similares existen en otros lenguajes. En **C#**, se usa igualmente `ToString()`. En **Python**, el equivalente idiomático es `__str__` (y `__repr__` para una representación más técnica). En **JavaScript**, los objetos tienen `toString()`, aunque su utilidad varía según el tipo y el contexto de conversión. En **C++**, no existe un método universal, pero se suele lograr algo parecido sobrecargando el operador de inserción `<<` para `std::ostream`.

A continuación, un ejemplo de `toString()` en la clase `Punto`, devolviendo una forma legible `Punto(x=..., y=...)`:

```java
class Punto {
    int x; // visibilidad por defecto
    int y; // visibilidad por defecto

    Punto() {
        this(0, 0);
    }

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    double distanciaA(Punto otro) {
        int dx = otro.x - this.x;
        int dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    @Override
    public String toString() {
        return "Punto(x=" + x + ", y=" + y + ")";
    }
}

// Ejemplo de uso
public class App {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p);                 // usa toString(): Punto(x=3, y=4)
        System.out.println("p = " + p);        // concatenación también invoca toString()
    }
}
```


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

Una **clase** se parece a un `struct` de C en el sentido de que ambos agrupan datos bajo un mismo tipo compuesto. En un `struct` se pueden definir varios campos que representan las características de una entidad, igual que en una clase se definen atributos. Desde este punto de vista, ambos sirven para modelar datos complejos y permiten declarar variables que contienen esos datos organizados. Sin embargo, la similitud termina ahí, ya que el `struct` forma parte de la programación estructurada, mientras que la clase pertenece al paradigma orientado a objetos.

Lo que **le falta al `struct`** para ser equivalente a una clase es la posibilidad de **definir comportamiento asociado**, es decir, **métodos**. Un `struct` sólo contiene datos, pero una clase combina datos *y* funciones que operan sobre esos datos, formando una unidad lógica llamada objeto. Además, en Java (y en la mayoría de lenguajes OO), las clases soportan características como **ocultación de información**, **constructores**, **herencia**, **polimorfismo**, y **métodos especiales** como `toString()`, que no tienen equivalente directo en un `struct` de C.

Otro aspecto importante es que, en programación orientada a objetos, una clase es un **molde** del que se crean **instancias** mediante el operador `new`. Dichas instancias tienen identidad propia en tiempo de ejecución, incluso si sus atributos son iguales. En cambio, en C, una variable de tipo `struct` es simplemente un bloque de memoria estático o automático que contiene campos, sin mecanismos adicionales como identidad de objeto, referencia implícita (`this`) ni control automático del ciclo de vida.

Por tanto, puede considerarse que un `struct` es una versión **muy limitada** de lo que posteriormente evolucionó hasta convertirse en una clase. Para que un `struct` fuese “como una clase”, necesitaría incorporar **métodos asociados**, **encapsulamiento**, **constructores**, **visibilidad**, **herencia** y **polimorfismo**, además de un modelo claro de **instanciación a través de referencias** en lugar de simples copias de memoria.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

En C se puede **emular** una clase simple como `Punto` usando un `struct` para los datos y **funciones** separadas para el comportamiento. No existen métodos asociados ni encapsulamiento, así que se suele usar un **prefijo** en los nombres de función para simular el “namespace” de la clase (por ejemplo, `Punto_calculaDistanciaAOrigen`). Tampoco hay constructores: se puede proporcionar una función de inicialización que asigne valores a los campos del `struct`.

El equivalente a `this` **no existe** en C; en su lugar, se pasa **explícitamente** un puntero (o una referencia simulada) al `struct` como **primer parámetro** de cada función que opere sobre ese “objeto”. Es decir, lo que en Java se escribe como `this.x`, en C se accede como `p->x` dentro de la función, donde `p` es el puntero recibido. Si se desea evitar copias y permitir que la función modifique el “objeto”, se pasa un `Punto*`; si solo se va a leer, se pasa un `const Punto*`.

A continuación, un ejemplo mínimo en C que modela `Punto` y su “método” para calcular la distancia al origen, junto con una función de inicialización y un ejemplo de uso:

```c
// punto.h (opcional)
#ifndef PUNTO_H
#define PUNTO_H

typedef struct {
    int x;
    int y;
} Punto;

// “constructor”/inicializador
void Punto_init(Punto* p, int x, int y);

// “método” que calcula distancia al origen (solo lectura)
double Punto_calculaDistanciaAOrigen(const Punto* p);

#endif
```

```c
// punto.c
#include <math.h>
#include "punto.h"

void Punto_init(Punto* p, int x, int y) {
    p->x = x;
    p->y = y;
}

double Punto_calculaDistanciaAOrigen(const Punto* p) {
    return sqrt((double)p->x * (double)p->x + (double)p->y * (double)p->y);
}
```

```c
// main.c
#include <stdio.h>
#include "punto.h"

int main(void) {
    Punto p;
    Punto_init(&p, 3, 4);

    double d = Punto_calculaDistanciaAOrigen(&p);
    printf("Distancia al origen: %.1f\n", d); // imprime 5.0

    return 0;
}
```

En este diseño, el “objeto” es simplemente una **zona de memoria** (`struct Punto`) y el papel de `this` lo cumple el **parámetro** `Punto*` que se pasa a cada función. Las funciones no están “dentro” del `struct`, por lo que no hay métodos ni visibilidad; todo es público. A cambio, se obtiene una emulación suficiente para casos simples: datos agrupados y operaciones coherentes sobre ellos, con la disciplina de pasar explícitamente el puntero a la entidad que se quiere procesar.


