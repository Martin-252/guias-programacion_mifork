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

Un **puntero a función** es una variable que almacena la dirección de memoria de una función y permite invocarla de forma indirecta. En C, las funciones no son objetos como en Java, pero sí pueden ser referenciadas mediante punteros, lo que hace posible pasar funciones como parámetros, almacenarlas en variables o elegir dinámicamente qué función ejecutar. Este mecanismo es uno de los fundamentos para introducir un estilo más flexible o “funcional” dentro de un lenguaje imperativo como C.

Desde el punto de vista conceptual, un puntero a función indica dos cosas: el **tipo de datos que devuelve la función** y la **lista de parámetros que recibe**. Esto es esencial para que el compilador pueda verificar que la llamada es correcta. A diferencia de C++ con orientación a objetos o Java, aquí no existe enlace dinámico automático mediante métodos virtuales, por lo que los punteros a función proporcionan una alternativa explícita para lograr comportamientos similares.

En el ejemplo siguiente se define una función que recibe una cadena de caracteres (`char *`), convierte sus letras a mayúsculas y devuelve la misma cadena. Posteriormente, se declara una variable local que es un puntero a dicha función, llamada `aMayusculas`, y se usa ese puntero para invocar la función. Se observa que la llamada mediante el puntero es muy similar a una llamada normal, lo que facilita su uso una vez comprendida la sintaxis.

```c
#include <stdio.h>
#include <ctype.h>

char* convertirAMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper((unsigned char)cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "Hola mundo";

    char* (*aMayusculas)(char *);
    aMayusculas = convertirAMayusculas;

    char *resultado = aMayusculas(texto);
    printf("%s\n", resultado);

    return 0;
}
```

Este uso de punteros a función permite desacoplar el código que decide *qué* operación realizar del código que define *cómo* se realiza, una idea clave en los aspectos funcionales. Aunque la sintaxis puede resultar más compleja que en otros lenguajes, el concepto es directamente comparable a usar referencias a métodos en Java o a pasar callbacks, sentando la base para técnicas más avanzadas.

**Clase teoría**

-Las funciones son "ciudadanos de primera clase". Una función es un tipo más:
--Se puede asignar a una variable
--Se puede pasar como paramatro
--Se puede devolver una función como retorno de otra
-Closure
-Expresiones Lambda
-En el lenguaje con comprobación estática de tipos: ¿Qué tipos tienen?



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una **función lambda** es una función anónima, es decir, una función sin nombre explícito, que puede definirse de forma concisa y asignarse a una variable, pasarse como argumento o devolverse como resultado. Su objetivo principal es permitir tratar el comportamiento como un valor, reforzando los aspectos funcionales del lenguaje. Conceptualmente, una función lambda cumple un papel similar al de un puntero a función en C, pero con una sintaxis más clara y segura, especialmente en lenguajes de más alto nivel.

En lenguajes modernos, las funciones lambda facilitan escribir código más expresivo y compacto, evitando la definición repetida de funciones completas cuando solo se necesita una lógica sencilla en un punto concreto. A diferencia de C, donde se trabaja con direcciones de memoria, aquí se manejan directamente referencias a funciones, lo que reduce errores y mejora la legibilidad. Para alguien con experiencia en Java, una lambda puede entenderse como una implementación directa y abreviada de una interfaz funcional.

En **JavaScript**, las funciones lambda se expresan mediante *arrow functions*. En el siguiente ejemplo se define una función lambda que recibe una cadena y devuelve otra en mayúsculas, se asigna a una variable local llamada `aMayusculas` y se invoca posteriormente:

```javascript
let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let resultado = aMayusculas("Hola mundo");
console.log(resultado);
```

En **Java**, las funciones lambda se introducen a partir de Java 8 y siempre se asocian a una **interfaz funcional**, es decir, una interfaz con un único método abstracto. Usando `Function<String, String>`, la lambda indica que recibe un `String` y devuelve otro `String`. El uso es muy directo y comparable al ejemplo anterior:

```java
import java.util.function.Function;

public class Ejemplo {

    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        String resultado = aMayusculas.apply("Hola mundo");
        System.out.println(resultado);
    }
}
```

Este mecanismo acerca a Java y JavaScript a un estilo más funcional, donde las funciones pueden almacenarse en variables y pasarse como datos. Para quien proviene de C, puede verse como una evolución natural del concepto de puntero a función, pero con mayor seguridad de tipos y una sintaxis considerablemente más simple.



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El **paradigma funcional** es un estilo de programación que se basa en el uso de funciones como elemento central del diseño del programa. En este paradigma, el cálculo se concibe como la evaluación de funciones matemáticas, evitando en la medida de lo posible los efectos secundarios y el uso de estados mutables. En lugar de modificar variables, se prioriza la creación de nuevos valores a partir de otros existentes, lo que facilita el razonamiento sobre el comportamiento del programa y reduce errores asociados a cambios inesperados de estado.

A lenguajes como **Java a partir de la versión 8** se los denomina **multi‑paradigma** porque no se limitan a un único enfoque de programación. Java sigue siendo fundamentalmente un lenguaje orientado a objetos, basado en clases, objetos, encapsulación y herencia, pero desde Java 8 incorpora características propias del paradigma funcional, como las funciones lambda, las interfaces funcionales y el tratamiento de funciones como valores. Esto permite elegir el paradigma más adecuado según el problema, o incluso combinarlos dentro de una misma solución.

Decir que las funciones son **“ciudadanos de primera clase”** significa que las funciones se tratan igual que cualquier otro dato del lenguaje. En la práctica, esto implica que una función puede asignarse a una variable, pasarse como argumento a otra función o devolverse como resultado. Este concepto ya aparece de forma limitada en C mediante punteros a función, pero en lenguajes como Java o JavaScript se integra de manera más segura y expresiva, sin necesidad de trabajar con direcciones de memoria.

La incorporación de estas ideas permite escribir código más modular, reutilizable y declarativo, acercando lenguajes tradicionalmente imperativos u orientados a objetos a los beneficios del paradigma funcional. Para quien proviene de C o Java clásico, este enfoque no sustituye lo aprendido, sino que amplía el conjunto de herramientas disponibles para modelar y resolver problemas de forma más flexible.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una **función lambda en Java** se introdujo a partir de Java 8 y permite definir implementaciones de forma concisa para **interfaces funcionales**, es decir, interfaces que contienen un único método abstracto. Una expresión lambda describe *qué* se hace, no *cómo se estructura* la clase que lo hace, eliminando la necesidad de definir clases anónimas completas. Desde un punto de vista sintáctico, una lambda se compone de una lista de parámetros, el operador `->` y una expresión o bloque de código que representa el cuerpo de la función.

La forma general de una lambda en Java es:

```java
(parámetros) -> expresión
```

o bien:

```java
(parámetros) -> { instrucciones; }
```

Si la lambda contiene una sola expresión, el valor de dicha expresión se devuelve implícitamente, sin necesidad de usar `return`. Cuando el cuerpo es un bloque con varias instrucciones, sí es obligatorio usar `return` si el método abstracto de la interfaz devuelve un valor. Los tipos de los parámetros suelen inferirse automáticamente por el compilador, por lo que no es necesario indicarlos explícitamente en la mayoría de los casos.

Por ejemplo, una función lambda que recibe un `String` y devuelve otro `String` en mayúsculas puede escribirse de forma muy compacta. Al asociarse a una interfaz como `Function<String, String>`, el compilador sabe que existe un método `apply` con esa firma, y lo vincula automáticamente a la lambda:

```java
Function<String, String> aMayusculas = s -> s.toUpperCase();
```

Esta sintaxis permite que el código sea más legible y expresivo, especialmente cuando se pasan funciones como parámetros o se usan colecciones y flujos (*streams*). Para alguien con experiencia previa en Java orientado a objetos, una lambda puede verse como una versión abreviada y directa de una clase anónima que implementa una interfaz con un único método, manteniendo el tipado fuerte característico del lenguaje.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Recibir una función como parámetro consiste en permitir que un método no solo reciba datos, sino también **comportamiento** que puede ejecutarse desde su interior. Esta idea es fundamental en el paradigma funcional y ya aparece en C mediante punteros a función, pero en lenguajes como Java 8 y JavaScript se expresa de forma más clara y segura usando funciones lambda. De este modo, el método que recibe la función queda desacoplado de la lógica concreta que se aplica al dato.

En **JavaScript**, las funciones son ciudadanos de primera clase desde el diseño inicial del lenguaje, por lo que pueden pasarse directamente como argumentos. En el ejemplo siguiente se define una función lambda `aMayusculas` y una función `transformar` que recibe una cadena y una función transformadora, la invoca internamente y devuelve el resultado:

```javascript
function transformar(cadena, transformador) {
    return transformador(cadena);
}

let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let resultado = transformar("Hola mundo", aMayusculas);
console.log(resultado);
```

En **Java**, recibir una función como parámetro requiere el uso de una **interfaz funcional**, ya que el lenguaje mantiene un tipado estático estricto. En este caso se reutiliza `Function<String, String>` para representar la función transformadora. El método `transformar` recibe el texto y la función, y la ejecuta mediante el método `apply`, que es el método abstracto de dicha interfaz:

```java
import java.util.function.Function;

public class Ejemplo {
    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        String resultado = transformar("Hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}
```

Este enfoque permite escribir código más flexible y reutilizable, ya que el método `transformar` no depende de una transformación concreta. Para quien proviene de Java orientado a objetos tradicional, este mecanismo puede entenderse como una generalización de pasar objetos con un único método, pero expresado de forma mucho más concisa gracias a las funciones lambda y a los conceptos del paradigma funcional.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Invocar un método pasando **una función lambda directamente en la llamada** consiste en definir el comportamiento justo en el punto donde se utiliza, sin necesidad de almacenarlo previamente en una variable. Esto refuerza la idea de que las funciones se tratan como valores y permite mantener el código más compacto y localizado. Este estilo es habitual cuando la transformación es sencilla o solo se va a usar una vez.

En **JavaScript**, esta forma de trabajo es muy natural, ya que las funciones pueden definirse inline sin ninguna estructura adicional. En el siguiente ejemplo, se llama a `transformar` pasando directamente una función lambda que invierte la cadena recibida, sin definirla previamente:

```javascript
function transformar(cadena, transformador) {
    return transformador(cadena);
}

let resultado = transformar("Hola mundo", (cadena) => {
    return cadena.split("").reverse().join("");
});

console.log(resultado);
```

En **Java**, el enfoque es similar desde Java 8, aunque requiere respetar el tipo de la interfaz funcional esperada. El método `transformar` sigue recibiendo un `Function<String, String>`, y la función lambda se define directamente en la llamada. La inversión de la cadena se implementa en el cuerpo de la lambda:

```java
import java.util.function.Function;

public class Ejemplo {
    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }

    public static void main(String[] args) {
        String resultado = transformar("Hola mundo",
            s -> new StringBuilder(s).reverse().toString()
        );

        System.out.println(resultado);
    }
}
```

Este uso de lambdas inline mejora la legibilidad cuando la lógica es breve y contextual, evitando nombres auxiliares innecesarios. Para quien proviene del uso de punteros a función en C o de Java clásico con clases anónimas, este patrón representa una forma más directa y expresiva de pasar comportamiento como parámetro, alineada con los principios del paradigma funcional.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un **cierre** o *closure* es la capacidad de una función lambda para **capturar y utilizar variables definidas en el contexto donde fue creada**, incluso cuando la función se ejecuta en otro lugar. Estas variables no forman parte de los parámetros de la función, pero siguen siendo accesibles porque la lambda “recuerda” el entorno en el que se definió. Este concepto es clave en el paradigma funcional, ya que permite combinar datos y comportamiento de forma flexible.

En **Java**, una función lambda puede acceder a variables locales del método que la contiene siempre que dichas variables sean **efectivamente finales**, es decir, que no se modifiquen después de su inicialización. Esta restricción garantiza seguridad y coherencia, evitando problemas derivados de estados cambiantes. Desde un punto de vista conceptual, la lambda captura el valor de la variable, no la variable en sí, lo que diferencia este mecanismo de los punteros clásicos en C.

Partiendo del ejemplo anterior, se puede definir una variable local fuera de la lambda y utilizarla dentro de una nueva función transformadora. En este caso, la lambda concatena a la cadena de entrada un sufijo definido en el contexto externo, demostrando claramente el funcionamiento de un cierre:

```java
import java.util.function.Function;

public class Ejemplo {
    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }

    public static void main(String[] args) {
        String sufijo = " !!!";

        String resultado = transformar("Hola mundo",
            s -> s + sufijo
        );

        System.out.println(resultado);
    }
}
```

Este ejemplo muestra cómo la lambda accede a `sufijo`, una variable local definida fuera de su cuerpo, sin necesidad de pasarla como parámetro. Este comportamiento es lo que define a un closure y permite escribir código más expresivo y modular, combinando elementos de la programación orientada a objetos con principios propios del paradigma funcional.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Una primera diferencia fundamental es que una **función lambda no es solo una dirección de memoria**, mientras que un **puntero a función en C sí lo es**. Un puntero a función únicamente señala dónde está el código de la función y permite invocarlo, pero no lleva consigo ningún contexto adicional. En cambio, una función lambda es una construcción de más alto nivel que puede encapsular tanto el código como parte del entorno en el que fue definida, lo que permite acceder a variables externas mediante los llamados *closures*.

Otra diferencia importante está en el **sistema de tipos y la seguridad**. En C, los punteros a funciones requieren una sintaxis compleja y el compilador ofrece menos garantías si se cometen errores en las firmas de las funciones. En lenguajes como Java, las lambdas están ligadas a interfaces funcionales con un tipado fuerte, lo que permite que el compilador verifique automáticamente que los parámetros y el tipo de retorno son correctos. Esto reduce errores y hace que el código sea más legible y mantenible.

Además, las funciones lambda están pensadas para integrarse de forma natural en un **modelo declarativo y expresivo**, mientras que los punteros a función son un mecanismo de bajo nivel. En Java o JavaScript es habitual definir lambdas inline, pasarlas como argumentos o devolverlas como resultado sin introducir ruido sintáctico. En C, aunque es posible pasar punteros a función como parámetros, el estilo resultante suele ser más difícil de leer y entender, especialmente para lógicas sencillas.

Por último, conceptualmente las lambdas fomentan un cambio de mentalidad hacia el **tratamiento de funciones como valores de primer nivel**, algo que en C solo se logra de forma parcial. Los punteros a función pueden verse como un precedente técnico, pero las funciones lambda van más allá al combinar claridad, seguridad de tipos y captura de contexto, sentando las bases del paradigma funcional dentro de lenguajes que tradicionalmente no lo eran.



## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Para devolver funciones es necesario que el lenguaje trate a las funciones como valores, permitiendo construirlas dinámicamente y retornarlas como resultado de otros métodos. En Java 8 esto se consigue mediante **interfaces funcionales** y funciones lambda. La idea es que un método cree y configure una función en función de ciertos parámetros, y devuelva esa función para que sea usada más adelante. Este patrón es habitual en programación funcional y se conoce como *función fábrica*.

En este caso, la función `crearDescuento` recibe un porcentaje y devuelve una función de tipo `Function<Double, Double>`. La función devuelta representa un descuento concreto y, cuando se invoca, aplica ese porcentaje a una cantidad dada. El porcentaje no se pasa de nuevo como parámetro, sino que queda almacenado en el contexto de la función devuelta.

```java
import java.util.function.Function;

public class Ejemplo {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad - (cantidad * porcentaje / 100.0);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento20 = crearDescuento(20);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento20.apply(precio)); // 80.0
    }
}
```

En este ejemplo se crean dos funciones de descuento distintas, una del 10 % y otra del 20 %, y se aplican ambas a la misma cantidad. Cada función mantiene su propio comportamiento aunque provengan del mismo método creador. Esto permite separar claramente la lógica de creación de la lógica de uso, favoreciendo reutilización y claridad.

La **closure** aparece porque la función lambda devuelta captura la variable `porcentaje`, que pertenece al contexto del método `crearDescuento`. Aunque ese método ya haya terminado su ejecución, la lambda sigue teniendo acceso al valor capturado. En Java, este valor debe ser efectivo-final, lo que garantiza que cada función de descuento conserve de forma segura el porcentaje con el que fue creada, demostrando cómo las lambdas combinan código y contexto en una única abstracción funcional.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

En Java, una **interfaz funcional** es una interfaz que representa el **tipo** de una función lambda. Dado que Java es un lenguaje con tipado estático, toda expresión lambda debe poder asociarse a un tipo concreto en tiempo de compilación, y ese tipo es precisamente una interfaz funcional. Conceptualmente, una interfaz funcional puede entenderse como la descripción de una función: especifica qué parámetros recibe y qué valor devuelve, sin indicar cómo se implementa.

El requisito fundamental para que una interfaz sea funcional es que contenga **exactamente un método abstracto**. Ese método define la firma de la función que la lambda implementa. Pueden existir otros métodos en la interfaz, pero solo uno de ellos puede ser abstracto. Los métodos `default` y `static`, introducidos también en Java 8, no cuentan como métodos abstractos y, por tanto, no rompen la condición de interfaz funcional.

Además, los métodos heredados de `Object` (`toString`, `equals`, `hashCode`, etc.) tampoco se consideran métodos abstractos a efectos de este cómputo. Esto permite que una interfaz funcional herede de `Object` o redefina alguno de sus métodos sin dejar de ser funcional. Gracias a esta regla, interfaces como `Function`, `Predicate` o `Runnable` pueden utilizarse directamente con expresiones lambda.

Finalmente, Java proporciona la anotación `@FunctionalInterface`, cuyo uso no es obligatorio pero sí recomendable. Esta anotación indica explícitamente la intención de que la interfaz sea funcional y hace que el compilador verifique que se cumple el requisito de tener un único método abstracto. De este modo, se mejora la claridad del diseño y se evitan errores accidentales que podrían impedir el uso de la interfaz con funciones lambda.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Una **interfaz funcional** definida “a mano” permite crear un tipo propio que represente una función concreta del dominio del problema, en lugar de reutilizar interfaces genéricas como `Function<T, R>`. Esto mejora la legibilidad del código y hace explícita la intención semántica de la función. En este caso, se desea modelar una transformación de una cadena de texto en otra, lo que encaja perfectamente con el concepto de interfaz funcional.

Para que una interfaz sea funcional debe cumplir el requisito de tener **un único método abstracto**, que será el que implemente la función lambda. Ese método define la firma de la función: qué parámetros recibe y qué devuelve. Puede incluir métodos `default` o `static`, pero estos no cuentan para el cómputo del método abstracto. Declarar este tipo de interfaz es especialmente útil cuando se quiere pasar comportamiento como parámetro de forma clara y tipada.

A continuación se define la interfaz funcional `Transformador`, cuya responsabilidad es convertir un `String` en otro `String`. Se utiliza la anotación `@FunctionalInterface` para dejar constancia explícita de su propósito y permitir que el compilador valide que cumple los requisitos:

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

Una interfaz de este tipo puede utilizarse directamente con funciones lambda o referencias a métodos, del mismo modo que `Function<String, String>`, pero con una semántica más clara. De este modo, se refuerza el carácter multi‑paradigma de Java, combinando tipado estático y orientación a objetos con conceptos propios del paradigma funcional.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Puede definirse una versión más genérica de la interfaz funcional `Transformador` empleando **genericidad**, de forma que no quede limitada únicamente a transformar cadenas. Mediante parámetros de tipo, la interfaz describe una transformación desde un tipo de entrada a un tipo de salida, manteniendo el tipado estático característico de Java. Conceptualmente, esta generalización es similar a `Function<T, R>`, pero permite usar un nombre más expresivo y adaptado al dominio del problema.

Para que la interfaz siga siendo funcional, debe mantenerse el requisito de **un único método abstracto**. En este caso, dicho método recibe un valor de tipo genérico `T` y devuelve otro de tipo `R`. El uso de la anotación `@FunctionalInterface` sigue siendo recomendable para garantizar que la interfaz se usa correctamente con funciones lambda.

La interfaz genérica `Transformador` puede definirse de la siguiente manera:

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```

A partir de esta interfaz, es posible definir un transformador concreto que convierta un `Double` en un `Integer`, por ejemplo redondeando el valor. Este transformador puede implementarse mediante una función lambda de forma directa y clara:

```java
public class Ejemplo {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
            d -> (int) Math.round(d);

        Integer resultado = redondear.transformar(3.6);
        System.out.println(resultado); // 4
    }
}
```

Este enfoque muestra cómo la genericidad y las interfaces funcionales permiten crear abstracciones reutilizables y seguras, combinando la flexibilidad del paradigma funcional con el tipado fuerte de Java. De este modo, se facilita la creación de transformaciones específicas sin renunciar a claridad ni robustez en el diseño.



## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java proporciona un **conjunto de interfaces funcionales predefinidas** en el paquete `java.util.function`, introducido a partir de Java 8, con el objetivo de cubrir los casos de uso más habituales al trabajar con funciones lambda. Estas interfaces evitan la necesidad de definir tipos propios para transformaciones, predicados u operaciones comunes, y permiten un uso directo, tipado y estandarizado de programación funcional dentro del lenguaje.

El grupo más básico lo forman las **interfaces que representan funciones**. La más general es `Function<T, R>`, que modela una transformación de un tipo `T` en otro `R`. A partir de ella existen variantes especializadas como `UnaryOperator<T>` (misma entrada y salida) y `BinaryOperator<T>` (dos entradas del mismo tipo y una salida). Estas interfaces son equivalentes a transformadores genéricos y cubren directamente el caso del `Transformador<T, R>` definido anteriormente.

Otro conjunto importante es el de las **interfaces predicado**, utilizadas para expresar condiciones lógicas. `Predicate<T>` representa una función que recibe un valor y devuelve un `boolean`, y `BiPredicate<T, U>` hace lo mismo con dos parámetros. Estas interfaces son muy usadas en operaciones de filtrado, por ejemplo en colecciones y *streams*, y sustituyen patrones clásicos basados en métodos condicionales o clases auxiliares.

También existen interfaces para **consumir valores sin devolver resultado**, como `Consumer<T>` y `BiConsumer<T, U>`, que representan operaciones con efectos laterales, y para **producir valores sin recibir parámetros**, como `Supplier<T>`. Además, Java define numerosas versiones especializadas para tipos primitivos (`IntFunction`, `DoublePredicate`, `LongConsumer`, etc.), cuyo objetivo es evitar el coste del *boxing* y *unboxing*. En conjunto, estas interfaces hacen que Java pueda considerarse un lenguaje multi‑paradigma, al integrar de forma nativa y segura los conceptos fundamentales del paradigma funcional.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método **`List.forEach`** es una forma funcional de recorrer una colección en Java, introducida a partir de Java 8, que sustituye al bucle `for` tradicional cuando el objetivo es aplicar una acción a cada elemento. En lugar de controlar explícitamente el índice o el iterador, se expresa directamente *qué operación* debe ejecutarse para cada elemento de la lista. Esto encaja con un estilo más declarativo y reduce el ruido sintáctico del código.

Desde el punto de vista funcional, `forEach` recibe una **función** (normalmente una lambda) que se aplica a cada elemento de la colección. El tipo de dicha función es `Consumer<T>`, ya que recibe un elemento de tipo `T` y no devuelve ningún resultado. De esta forma, el control del recorrido queda en manos de la colección, y el programador se centra únicamente en la lógica asociada a cada elemento.

En el siguiente ejemplo se recorre una lista de enteros y se muestra un mensaje únicamente cuando el valor es positivo. La condición se expresa dentro de la lambda pasada a `forEach`, sin necesidad de un bucle explícito:

```java
import java.util.Arrays;
import java.util.List;

public class Ejemplo {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 0, 5, 7, -1);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("Número positivo: " + n);
            }
        });
    }
}
```

Este enfoque ilustra cómo Java permite combinar estructuras clásicas como listas con mecanismos del paradigma funcional. Para quien proviene del uso de bucles `for` en C o Java tradicional, `forEach` puede verse como una abstracción de ese patrón, donde se delega el *cómo recorrer* y se enfatiza el *qué hacer* con cada elemento.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

En la firma de `forEach` se utiliza `Consumer<? super T>` en lugar de `Consumer<T>` para **aumentar la flexibilidad del sistema de tipos** sin perder seguridad. El método `forEach` consume elementos de la lista, es decir, *entrega* elementos de tipo `T` a la función que se pasa como parámetro. Permitir `<? super T>` significa que se admite cualquier consumidor que sea capaz de aceptar valores de tipo `T` **o de uno de sus supertipos**, lo que hace el método más reutilizable en jerarquías de tipos.

Este criterio se resume en la regla conocida como **PECS** (*Producer Extends, Consumer Super*). PECS indica que:

*   Si un parámetro genérico **produce** valores (se leen), debe usarse `? extends T`.
*   Si un parámetro genérico **consume** valores (se escriben), debe usarse `? super T`.

En el caso de `forEach`, la función **consume** elementos de la lista, por lo que resulta correcto usar `Consumer<? super T>`. Gracias a ello, una `List<Integer>` puede aceptar sin problema un `Consumer<Number>` u otro consumidor más general, algo que no sería posible si la firma exigiera exactamente `Consumer<Integer>`.

Este mismo razonamiento puede aplicarse para mejorar la definición del método `transformar`. En su versión básica, se suele escribir:

```java
static <T, R> R transformar(T valor, Function<T, R> f)
```

Sin embargo, aplicando PECS, la versión más flexible sería:

```java
static <T, R> R transformar(T valor, Function<? super T, ? extends R> f)
```

Aquí la función **consume** valores de tipo `T`, por lo que se permite `? super T`, y **produce** valores de tipo `R`, por lo que se permite `? extends R`.

El uso de comodines con PECS no cambia el comportamiento del programa, pero sí amplía los usos válidos desde el punto de vista del compilador. Esto es especialmente importante en librerías y APIs generales, donde pequeñas decisiones en las firmas de los métodos marcan una gran diferencia en extensibilidad y reutilización, manteniendo al mismo tiempo la comprobación estática de tipos que caracteriza a Java.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
