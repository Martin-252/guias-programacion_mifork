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
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

En orientación a objetos, la **herencia** es un mecanismo que permite definir una clase nueva a partir de otra ya existente, estableciendo una relación conceptual del tipo **“A es-un B”**. Esta expresión significa que una instancia de la clase derivada puede considerarse también como una instancia de la clase base. Por ejemplo, decir que *un Artillero es-un Soldado* implica que todo artillero comparte las características esenciales de cualquier soldado, además de poseer otras más específicas. Esta idea resulta natural si se compara con los tipos en C: una estructura extendida contiene todo lo de la estructura “base”, más campos adicionales.

La primera implicación importante de la herencia es la **compatibilidad de tipos**. Cuando una clase hereda de otra, los objetos de la subclase pueden usarse allí donde se espere un objeto de la superclase. En Java, esto significa que una variable de tipo `Soldado` puede referenciar tanto a un `Soldado` como a un `Artillero` o un `Zapador`. Este comportamiento permite escribir código genérico que trabaje con la clase base sin conocer el tipo concreto, algo que recuerda al uso de punteros a estructuras en C, pero con comprobación de tipos y seguridad en tiempo de compilación.

La segunda implicación es la **herencia de estado y comportamiento**. La subclase hereda los atributos y métodos accesibles de la superclase, por lo que no es necesario reescribir código ya existente. En el ejemplo, tanto `Artillero` como `Zapador` heredan el atributo `nombre` (privado y encapsulado) y el método `saludar()`, reutilizando exactamente el mismo comportamiento. Cada subclase añade únicamente su estado específico (cohetes o minas) mediante nuevos atributos y métodos, lo que conecta directamente con la idea de reutilización ya vista en la composición, pero aplicada ahora mediante una relación jerárquica.

### Ejemplo sencillo en Java

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}
```

```java
class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }
}
```

```java
class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Soldado("Carlos");
        ejercito[1] = new Artillero("Luis", 5);
        ejercito[2] = new Zapador("Miguel", 3);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

En este último fragmento se aprovecha la **compatibilidad de tipos**: aunque el array es de `Soldado`, contiene objetos de distintos subtipos y todos pueden ejecutar `saludar()` sin necesidad de saber su tipo concreto.



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de una **clase concreta que hereda de otra**, **siempre se ejecutan varios constructores**: el de la clase concreta y el de su clase base. El número total de constructores ejecutados corresponde a toda la cadena de herencia, desde la clase más derivada hasta `Object`. El **orden** está claramente definido: **primero se ejecuta el constructor de la superclase y después el de la subclase**. Esto tiene sentido porque la parte “base” del objeto debe quedar correctamente inicializada antes de construir la parte más específica.

La palabra clave `super`, usada dentro de un constructor, sirve para **invocar explícitamente un constructor de la clase base**. Es el mecanismo que permite pasar parámetros hacia arriba en la jerarquía y controlar cómo se inicializan los atributos heredados. Conceptualmente, `super(...)` indica que la responsabilidad de inicializar la parte común del objeto se delega a la superclase. En Java, esta llamada debe aparecer **siempre como la primera instrucción del constructor**, ya que no tendría sentido manipular el objeto antes de que la clase base haya sido construida.

Si la clase base **no tiene un constructor sin parámetros accesible**, entonces **es obligatorio llamar a `super(...)` de forma explícita** desde el constructor de la subclase. Java no puede inventar valores para inicializar la superclase y, si no se indica cómo hacerlo, el código no compila. En cambio, si la clase base sí dispone de un constructor visible sin parámetros, el compilador inserta automáticamente una llamada implícita a `super()` cuando no se escribe ninguna. Por tanto, la regla práctica es: **si la superclase necesita datos para construirse, la subclase debe proporcionarlos explícitamente mediante `super`**.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, **los atributos privados de la superclase forman parte físicamente de una instancia de la subclase en memoria**. Cuando se crea, por ejemplo, un objeto `Artillero`, este objeto contiene **todo el estado definido en `Soldado`** (como `nombre`), además de los atributos propios de `Artillero` (como `numCohetes`). Desde el punto de vista de la memoria y del modelo de objetos de Java, no existen “dos objetos separados”, sino **un único objeto** cuya estructura incluye la parte correspondiente a cada clase de la jerarquía.

Ahora bien, que un atributo privado exista en memoria **no implica que pueda usarse directamente desde el código de la subclase**. La palabra clave `private` impone una restricción de acceso a nivel de código, no de memoria. Por tanto, aunque `Artillero` *tenga* el atributo `nombre` heredado de `Soldado`, **no puede acceder a él directamente**, ni leerlo ni modificarlo. Esta separación refuerza la encapsulación y garantiza que solo la propia clase que declara el atributo controla su uso.

En el ejemplo de `Soldado`, el atributo `nombre` es privado, pero la subclase puede **interactuar con él de forma indirecta** mediante métodos públicos o protegidos definidos en la superclase. Por ejemplo, `Artillero` no puede escribir `this.nombre`, pero sí puede llamar a `getNombre()` o usar `saludar()`, porque esos métodos pertenecen a la interfaz visible de `Soldado`. Esto muestra una idea clave de la herencia: **el estado heredado existe y se reutiliza, pero su acceso sigue sujeto a las reglas de encapsulación**, incluso dentro de las subclases.

```java
class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
        // System.out.println(nombre); // ERROR: nombre es private en Soldado
        System.out.println(getNombre()); // Correcto: acceso indirecto
    }
}
```


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La **compatibilidad de tipos** que introduce la herencia tiene una consecuencia directa sobre la **extensibilidad del código**: permite añadir nuevos comportamientos concretos sin modificar el código que trabaja con la abstracción común. Al programar contra la clase base (`Soldado`) y no contra clases concretas, el sistema queda **abierto a extensión pero cerrado a modificación**, ya que pueden aparecer nuevos tipos de soldados sin necesidad de reescribir la lógica existente. Esta idea resulta especialmente valiosa en programas grandes, donde cambiar código ya probado aumenta el riesgo de errores.

En el ejemplo, el código que pide a los soldados que saluden solo conoce el tipo `Soldado`. Gracias a la compatibilidad de tipos, cualquier nueva subclase será automáticamente aceptada allí donde se espere un `Soldado`. No es necesario añadir condiciones, comprobaciones de tipo ni duplicar lógica. Este enfoque contrasta con un diseño sin herencia, donde añadir un nuevo tipo suele obligar a modificar estructuras de control o añadir nuevos casos, rompiendo la extensibilidad.

Para ilustrarlo, se puede añadir un nuevo tipo de soldado, por ejemplo un `Medico`, con su propio estado específico. La clase hereda el atributo `nombre` y el método `saludar()`, y añade únicamente lo necesario para su función concreta. El código que recorre el array de `Soldado` **no se modifica en absoluto**, pero el nuevo tipo participa automáticamente en el comportamiento común.

```java
class Medico extends Soldado {
    private int numVendajes;

    public Medico(String nombre, int numVendajes) {
        super(nombre);
        this.numVendajes = numVendajes;
    }

    public int getNumVendajes() {
        return numVendajes;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Soldado("Carlos");
        ejercito[1] = new Artillero("Luis", 5);
        ejercito[2] = new Zapador("Miguel", 3);
        ejercito[3] = new Medico("Ana", 10);

        for (Soldado s : ejercito) {
            s.saludar(); // No cambia aunque haya nuevos tipos
        }
    }
}
```

Este ejemplo muestra que la herencia, gracias a la compatibilidad de tipos, permite **extender el sistema de forma limpia**, manteniendo intacto el código ya existente y favoreciendo diseños más flexibles y mantenibles.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, en Java **una referencia del supertipo puede apuntar a objetos reales de un subtipo**. Esto es una consecuencia directa de la herencia y de la compatibilidad de tipos: si `Artillero` hereda de `Soldado`, entonces todo artillero *es un* soldado y puede almacenarse en una variable de tipo `Soldado`. Esta idea es fundamental para trabajar con polimorfismo y colecciones heterogéneas, como arrays o listas que contienen objetos de distintas subclases pero que comparten una misma superclase.

Sin embargo, usando una referencia del supertipo **solo se pueden invocar los métodos que estén definidos en el supertipo** (y sean visibles). Aunque el objeto real sea un `Artillero`, si la referencia es de tipo `Soldado` no se puede llamar directamente a métodos específicos como `getNumCohetes()`. El compilador decide qué métodos son accesibles basándose en el **tipo de la referencia**, no en el tipo real del objeto. Esto evita errores y obliga a tratar los objetos como lo que conceptualmente se espera: soldados genéricos.

El **upcasting** consiste en tratar un objeto de una subclase como si fuese de la superclase. Es automático, seguro y no requiere ninguna conversión explícita, por ejemplo al asignar un `Artillero` a una variable `Soldado`. El **downcasting**, en cambio, consiste en convertir una referencia de la superclase a una subclase concreta. Esta operación **no es segura por defecto**: solo es válida si el objeto real pertenece realmente a esa subclase. Para evitar errores en tiempo de ejecución (`ClassCastException`), se utiliza el operador `instanceof`, que permite comprobar el tipo real del objeto antes de hacer el casting.

```java
Soldado s1 = new Artillero("Luis", 5); // Upcasting implícito

// Downcasting seguro usando instanceof
if (s1 instanceof Artillero) {
    Artillero a = (Artillero) s1;
    System.out.println(a.getNumCohetes());
}
```

Un ejemplo típico aparece al recorrer un array de `Soldado` y querer acceder a información específica solo de algunos subtipos. Se comprueba el tipo real del objeto y, si coincide, se hace el downcasting correspondiente para acceder a sus métodos propios, manteniendo al mismo tiempo un diseño extensible y seguro.

```java
Soldado[] ejercito = {
    new Soldado("Carlos"),
    new Artillero("Luis", 5),
    new Zapador("Miguel", 3)
};

for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getNumCohetes());
    }
}
```

Este patrón muestra cómo Java combina **flexibilidad y seguridad de tipos**, permitiendo trabajar de forma genérica con la herencia sin perder la posibilidad de acceder a comportamientos concretos cuando resulta necesario.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

En el contexto de ocultación de información y herencia, el acceso **protegido** permite encontrar un punto intermedio entre `private` y `public`. Un atributo o método protegido **no es accesible desde cualquier clase**, pero **sí lo es desde las propias subclases**, incluso aunque se encuentren en otro paquete. La idea es permitir que las clases hijas reutilicen o amplíen detalles internos de la superclase sin exponerlos completamente al exterior, manteniendo así un control razonable sobre el estado y el comportamiento.

En Java, el acceso protegido se implementa mediante la palabra clave `protected`. A diferencia de `private`, que limita el acceso exclusivamente a la clase donde se declara, `protected` autoriza el acceso desde las subclases. Conceptualmente, no se está rompiendo la encapsulación, sino **diseñando la clase base pensando explícitamente en su reutilización mediante herencia**. Esto resulta útil cuando un dato forma parte esencial del comportamiento de las subclases, pero no debe ser visible para el resto del programa.

Aplicado al ejemplo de `Soldado`, si el atributo `nombre` se declara como `protected` en lugar de `private`, dicho atributo seguirá formando parte del estado interno del soldado, pero además podrá usarse directamente dentro del código de `Zapador`. De este modo, el zapador puede emplear el nombre heredado para mostrar mensajes relacionados con su acción específica (poner minas), sin necesidad de recurrir a métodos auxiliares.

```java
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}
```

```java
class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }

    public void ponerMina() {
        System.out.println("El zapador " + nombre + " ha puesto una mina.");
    }
}
```

Este ejemplo muestra que el acceso protegido **permite a las subclases interactuar directamente con el estado heredado**, sin hacerlo público al resto del sistema. De esta forma, la herencia se vuelve más flexible, pero sin perder el control sobre qué partes de la clase base están pensadas para ser usadas y ampliadas por sus descendientes.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En los lenguajes orientados a objetos, **no existe una regla universal** que obligue a que todos tengan una única clase base común para todos los objetos. La existencia de una “raíz” de la jerarquía depende del diseño del lenguaje. Algunos lenguajes permiten trabajar sin una superclase implícita, mientras que otros definen explícitamente una clase base de la que derivan todos los objetos. Por tanto, la respuesta general es que **depende del lenguaje y de su modelo de tipos**.

En lenguajes orientados a objetos más flexibles o de bajo nivel, como C++ (cuando se usa orientación a objetos), **no existe una clase base obligatoria**. Una clase solo hereda de otra si el programador lo indica explícitamente. Esto significa que pueden existir clases completamente independientes, sin ningún antepasado común definido por el lenguaje. Este enfoque ofrece mayor control, pero también menos comportamientos compartidos de forma automática entre todos los objetos.

En Java, en cambio, **sí existe una clase base común para todos los objetos: `java.lang.Object`**. Toda clase que no declare explícitamente una superclase hereda automáticamente de `Object`. Esto implica que absolutamente todos los objetos en Java comparten un conjunto mínimo de métodos comunes, como `toString()`, `equals()`, `hashCode()` o `getClass()`. Esta decisión de diseño simplifica el lenguaje y garantiza que cualquier referencia a un objeto tenga unas capacidades básicas comunes.

Como consecuencia práctica, en Java **siempre existe una jerarquía de herencia**, aunque no se declare de forma explícita. Por ejemplo, `Soldado`, `Artillero` y `Zapador` heredan implícitamente de `Object`, y solo a partir de ahí comienza la herencia definida por el programador. Esto permite escribir código genérico que funcione con cualquier objeto y refuerza la coherencia del sistema de tipos, algo que diferencia claramente a Java de otros lenguajes orientados a objetos como C++.
**Clase teoría**
    C++ no tiene una clase base implícita, pero Java si que tiene una clase base implícita la cual es la clase "object".
En esta clase object encontramos metodos como el toString() por eso en todas las clases se puede llamar a este metodo porque esta implementado ya en la clase object. Otro método es el equals() es este caso es equivalente al "==". 



## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La **herencia múltiple** es un mecanismo de la orientación a objetos por el cual **una clase puede heredar directamente de más de una clase base**. Conceptualmente, esto implica que una misma clase combina estado y comportamiento procedentes de varias jerarquías distintas. Por ejemplo, se podría decir que una clase *A* hereda de *B* y de *C*, adquiriendo los atributos y métodos de ambas. Aunque esta idea parece potente, introduce problemas de diseño y de ambigüedad, como el conocido *problema del diamante*, donde no queda claro qué implementación debe heredarse cuando dos superclases comparten métodos con el mismo nombre.

En **Java no existe herencia múltiple de clases**. Una clase solo puede extender directamente de **una única clase base** mediante la palabra clave `extends`. Esta decisión fue tomada de forma deliberada en el diseño del lenguaje para evitar ambigüedades en la resolución de métodos, simplificar el modelo mental del programador y mantener un sistema de tipos más predecible. Por tanto, una clase como `Artillero` solo puede heredar de `Soldado`, y no, por ejemplo, de `Soldado` y `Vehiculo` a la vez.

Sin embargo, Java sí permite una forma controlada de herencia múltiple mediante **interfaces**. Una clase puede implementar **múltiples interfaces**, lo que le permite prometer que proporciona ciertos métodos, sin heredar estado ni implementación concreta (salvo métodos `default`, introducidos más tarde). Este enfoque separa claramente el *qué* (el comportamiento esperado) del *cómo* (la implementación), logrando muchos de los beneficios de la herencia múltiple sin sus principales inconvenientes.

En resumen, Java **rechaza la herencia múltiple de clases**, pero **acepta la herencia múltiple de comportamiento abstracto a través de interfaces**. Esta solución favorece diseños más claros y evita conflictos complejos, a costa de obligar a una planificación más cuidadosa de la jerarquía de clases y del uso de la composición y las interfaces como mecanismos complementarios.

**Clase teoría**
La herencia múltiple es heredar de más de una clase. En java no existe la herencia multiple por problemas como la herencia de diamante. Sin enmbargo en leguajes como C++ si que esta implementada esa herencia múltiple.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

En los lenguajes orientados a objetos, las **excepciones son objetos**, lo que implica que forman parte de una jerarquía de clases y pueden **extenderse y personalizarse**. Esto permite modelar errores del dominio de la aplicación con más precisión que usando excepciones genéricas. Al igual que cualquier otro objeto, una excepción puede contener estado propio y comportarse como una unidad coherente de información sobre el problema ocurrido.

Una **excepción no controlada** en Java es aquella que hereda de `RuntimeException`. Estas excepciones **no obligan al programador a capturarlas ni a declararlas** en la firma de los métodos, y suelen representar errores de lógica, estados invalidos o situaciones excepcionales que no siempre pueden (o deben) gestionarse localmente. Crear una excepción personalizada no controlada resulta apropiado cuando el error forma parte del modelo del problema y no de la infraestructura.

Además, una excepción puede estar **compuesta con otros objetos** del dominio. En este caso, la excepción `UsuarioNoEncontradoException` contiene una referencia a un `Usuario`, lo que permite saber exactamente qué entidad provocó el error. Este enfoque es especialmente útil para depuración, registro de errores o capas superiores de la aplicación, donde se necesita contexto adicional más allá de un simple mensaje de texto.

Por último, es buena práctica permitir que una excepción **encapsule una causa subyacente**. Java lo soporta mediante constructores que aceptan un objeto `Throwable`, lo que permite encadenar excepciones y preservar la información original del fallo. Esto facilita el análisis del error sin perder detalles de bajo nivel, manteniendo al mismo tiempo una abstracción de alto nivel adecuada al dominio.

```java
// Clase de dominio sencilla
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
// Excepción personalizada no controlada
class UsuarioNoEncontradoException extends RuntimeException {
    private final Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

Esta implementación muestra cómo una excepción personalizada puede **modelar un error del dominio**, ser no controlada, almacenar información relevante y mantener la causa original cuando existe.



## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

La herencia **no debe usarse solo como mecanismo de reutilización de código** porque introduce una **relación semántica fuerte** entre clases: la relación *“A es-un B”*. Cuando se aplica herencia sin que esta relación sea conceptualmente cierta, el diseño se vuelve frágil. La subclase queda atada a la implementación y a las decisiones internas de la superclase, de modo que cualquier cambio en la clase base puede tener efectos colaterales inesperados en todas sus descendientes. Es decir, se reutiliza código a costa de perder independencia y flexibilidad.

Desde el punto de vista del diseño, la herencia implica **acoplamiento**. La subclase hereda no solo el código que se desea reutilizar, sino también métodos, atributos y contratos que quizá no encajan bien con su responsabilidad real. Esto puede llevar a jerarquías artificiales, difíciles de entender y mantener, donde aparecen métodos heredados que no tienen sentido en la subclase o que deben sobrescribirse constantemente. En estos casos, la herencia deja de expresar un modelo conceptual y se convierte en una fuente de complejidad.

La **composición**, en cambio, permite reutilizar código sin establecer una relación jerárquica. Al componer, una clase *tiene un* objeto de otra clase, en lugar de *ser un* objeto de esa clase. Esto favorece diseños más flexibles, ya que el comportamiento reutilizado puede cambiarse, sustituirse o ampliarse sin afectar a una jerarquía completa. Además, la composición suele respetar mejor el principio de responsabilidad única, porque cada clase mantiene un papel más claro y acotado.

Por ello, se recomienda pensar primero en composición cuando el objetivo principal es reutilizar funcionalidad. La herencia queda reservada para los casos en los que existe una relación clara de especialización y sustitución, donde realmente tenga sentido que un objeto de la subclase pueda usarse sin problemas allí donde se espera uno de la superclase. Esta distinción es clave para escribir código extensible, mantenible y conceptualmente correcto.



## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

La recomendación de **“favorecer la composición frente a la herencia”** surge porque la composición conduce, en general, a diseños **más flexibles, robustos y fáciles de mantener**. La herencia establece una relación muy fuerte entre clases, tanto conceptual como técnicamente: la subclase queda ligada a la implementación interna de la superclase. Esto significa que cambios en la clase base pueden afectar de manera inesperada a todas las subclases, incluso aunque estas no necesiten directamente la parte modificada.

La composición, en cambio, reduce el **acoplamiento**. Al componer, una clase delega parte de su comportamiento a otros objetos en lugar de heredarlo. Esto permite variar el comportamiento en tiempo de ejecución, sustituir implementaciones sin alterar la jerarquía y combinar funcionalidades de maneras que serían imposibles o muy complejas con herencia. Desde el punto de vista del diseño, se trabaja con relaciones *“tiene-un”* en lugar de *“es-un”*, que suelen ajustarse mejor a muchos problemas reales.

Otro motivo importante es que la herencia tiende a ser **estática**: una vez definida la jerarquía, resulta costoso cambiarla sin romper código existente. La composición favorece diseños más **abiertos a la extensión**, donde nuevas funcionalidades pueden añadirse creando nuevos componentes y combinándolos, en lugar de forzar nuevas subclases. Esto encaja directamente con principios de diseño como el *principio abierto/cerrado*, donde el comportamiento se amplía sin modificar código existente.

En resumen, se favorece la composición porque proporciona mayor flexibilidad, menor dependencia entre clases y un modelo más estable frente al cambio. La herencia sigue siendo una herramienta valiosa, pero debe reservarse para los casos en los que exista una relación clara de especialización y sustitución. Cuando el objetivo principal es compartir comportamiento o variar funcionalidades, la composición suele ser la opción más segura y escalable.



## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

Cuando se dice que **“la herencia rompe la encapsulación”**, no se afirma que la encapsulación desaparezca por completo, sino que **se debilita** respecto a otros mecanismos como la composición. La encapsulación busca ocultar los detalles internos de una clase y exponer solo una interfaz bien definida. Sin embargo, al usar herencia, la subclase **necesita conocer y depender** de aspectos internos de la superclase para funcionar correctamente, lo que crea una dependencia más fuerte de lo deseable.

En particular, la subclase queda acoplada no solo a la **interfaz pública** de la superclase, sino también a su **estructura interna y a sus decisiones de diseño**. Aunque los atributos sean `protected` o se acceda a ellos mediante métodos, la subclase suele asumir cómo y cuándo se inicializan, qué invariantes mantiene la clase base o qué efectos secundarios tienen sus métodos. Esto implica que cambios internos en la superclase —que en teoría no deberían afectar a otras clases— **pueden romper el comportamiento de las subclases**, incluso cuando la interfaz pública no ha cambiado.

Por ejemplo, si `Zapador` hereda de `Soldado` y utiliza directamente un atributo `protected nombre`, el funcionamiento de `Zapador` depende de que ese atributo exista, tenga cierto significado y se gestione de una forma concreta. Si `Soldado` cambia su representación interna (por ejemplo, separando nombre y apellidos, o calculando el nombre dinámicamente), la subclase puede verse afectada. En este sentido, la herencia **expone más de lo necesario** a las subclases, mientras que la composición mantiene una encapsulación más fuerte al interactuar únicamente a través de interfaces bien definidas.

Por ello, se dice que la herencia rompe (o relaja) la encapsulación: **las subclases conocen demasiado de sus superclases**. No significa que la herencia sea incorrecta, sino que debe usarse con cuidado y solo cuando exista una relación clara de especialización. Cuando el objetivo es reutilizar comportamiento sin depender de detalles internos, la composición suele preservar mejor la encapsulación y producir diseños más robustos frente al cambio.

**Clase teoría**
Tanto en la pregunta 10, 11, y 12 se llega a la conclusión de que es mejor no usar herencia **solo** por reutilizar código, debe usarser si se necesita la compatibilidade de tipos.
-Usar herencia implica un fuerte acoplamiento desde la clase derivada hacia la clase base.
 ->L clase derivada depende mucho de la base
    -> Cambios internos en la base podrían llegat a afectar a las derivadas.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

A continuación se muestran **dos formas distintas de modelar el mismo problema**, una mediante **herencia** y otra mediante **composición**, para comparar claramente ambos enfoques. En ambos casos, `Estudiante` y `Trabajador` comparten el DNI y el nombre, pero la relación entre las clases cambia de forma significativa.

En el **modelo por herencia**, se introduce una superclase `Persona` que agrupa los datos comunes. Tanto `Estudiante` como `Trabajador` **son una Persona**, lo que implica una relación *“es‑un”*. Esta solución es adecuada cuando existe una relación semántica clara de especialización y se espera tratar a estudiantes y trabajadores como personas de forma intercambiable (compatibilidad de tipos). Sin embargo, las clases hijas quedan ligadas a la estructura y evolución de `Persona`.

```java
class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

En el **modelo por composición**, los datos comunes se encapsulan en una clase independiente `DatosPersonales`. En este caso, `Estudiante` y `Trabajador` **no son** datos personales, sino que **tienen** datos personales. Esta aproximación evita jerarquías innecesarias, reduce el acoplamiento y permite reutilizar la clase `DatosPersonales` en otros contextos. Cada clase recibe una instancia de `DatosPersonales` en su constructor, lo que refuerza la flexibilidad del diseño.

```java
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
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatosPersonales() {
        return datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatosPersonales() {
        return datos;
    }
}
```

Este ejemplo permite apreciar que **ambas soluciones son válidas**, pero responden a necesidades distintas. La herencia enfatiza la relación conceptual y la sustitución de tipos, mientras que la composición prioriza la reutilización flexible y una encapsulación más fuerte. Elegir una u otra depende del significado real de la relación entre las clases y de cómo se espera que el diseño evolucione en el tiempo.

