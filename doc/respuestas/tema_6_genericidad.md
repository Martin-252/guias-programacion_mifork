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
Este tema solo se trata en clase e exame teorico puramente, es decir preguntas puras de genericidad solo serán tratadas en teoría.

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

Un ejemplo clásico de estructura de datos que permite almacenar cualquier tipo de dato consiste en implementar un **contenedor genérico usando `Object` en Java**, apoyándose en un array primitivo. Esta técnica se utilizó ampliamente antes de la introducción de la genericidad en Java, y es conceptualmente equivalente al uso de `void*` en C. La idea fundamental es que todas las clases en Java heredan de `Object`, por lo que un array de `Object` puede contener referencias a instancias de cualquier clase.

Esta aproximación permite reutilizar una única estructura de datos para distintos tipos, pero traslada la responsabilidad del control de tipos al programador. En tiempo de compilación no existe garantía de que el objeto recuperado sea del tipo esperado, lo que obliga a realizar conversiones explícitas (casts). Si el tipo no es correcto, el error solo se detecta en tiempo de ejecución mediante una excepción, lo que supone un riesgo adicional respecto a soluciones más seguras.

El siguiente ejemplo muestra una estructura similar a una lista dinámica muy simple, basada internamente en un array de `Object`:

```java
public class ListaGenerica {
    private Object[] datos;
    private int size;

    public ListaGenerica(int capacidad) {
        datos = new Object[capacidad];
        size = 0;
    }

    public void add(Object elemento) {
        datos[size++] = elemento;
    }

    public Object get(int index) {
        return datos[index];
    }
}
```

Gracias al uso de `Object`, en esta lista se pueden almacenar enteros, cadenas, u objetos definidos por el usuario. Sin embargo, al recuperar un elemento es obligatorio realizar un cast explícito al tipo deseado. Este enfoque ilustra claramente la motivación de la genericidad en Java: mantener la flexibilidad de este tipo de estructuras, pero añadiendo **seguridad de tipos en tiempo de compilación**, algo que no se consigue ni con `void*` en C ni con `Object` en Java sin genéricos.

***Clase teoría*** 
Un generico `T` en java es llamado generico de tipo, es decir, no es un valor sino que es una parametro que puede coger cualquier tipo.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La **programación genérica** consiste en definir algoritmos y estructuras de datos de forma **independiente del tipo concreto de datos** con el que van a trabajar, permitiendo reutilizar el mismo código para distintos tipos sin perder seguridad. En lenguajes como Java, esto se materializa mediante **parámetros de tipo** (genéricos), que permiten expresar explícitamente qué tipo se utilizará, y que el compilador verifique su uso correcto. El objetivo principal es combinar reutilización de código con comprobación de tipos en tiempo de compilación.

Desde un punto de vista conceptual, la programación genérica va más allá de “aceptar cualquier cosa”. No solo permite almacenar distintos tipos, sino también **mantener la coherencia de tipos**: lo que se introduce en la estructura es del mismo tipo que se recupera, sin necesidad de conversiones explícitas. Esto la diferencia claramente de enfoques más antiguos basados en tipos universales, como `void*` en C o `Object` en Java, en los que el tipo real se pierde y debe reconstruirse manualmente.

El ejemplo anterior **no es programación genérica en sentido estricto**, sino un antecedente o solución previa a ella. Aunque permite alojar cualquier tipo de dato, no existe información de tipo en la estructura, y por tanto no se ofrece seguridad en tiempo de compilación. El uso de `Object` proporciona flexibilidad, pero introduce riesgos de error en ejecución. Precisamente, la programación genérica surge para solucionar estas limitaciones, manteniendo la generalidad sin renunciar a la seguridad de tipos.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

El principal problema respecto al **chequeo de tipos** al emplear `void*` en C o `Object` en Java es que **la información del tipo concreto se pierde** dentro de la estructura de datos. El compilador no puede verificar si los elementos almacenados y recuperados son coherentes con el uso que se hace de ellos, ya que todos se tratan como un tipo genérico o universal. Como consecuencia, no se detectan errores de tipo en tiempo de compilación, lo que reduce significativamente la seguridad del código.

Otro problema importante es la **necesidad de realizar conversiones explícitas (casts)** al recuperar los elementos. Estas conversiones son responsabilidad del programador y no están garantizadas por el compilador. Si se realiza un cast incorrecto, el error solo se manifestará en tiempo de ejecución, ya sea como comportamiento indefinido en C o como una excepción (`ClassCastException`) en Java. Esto dificulta el mantenimiento del código y hace más propenso a errores difíciles de localizar.

Además, este enfoque **no garantiza homogeneidad de tipos** dentro de la estructura de datos. Es posible almacenar objetos de tipos distintos en la misma estructura sin que el compilador lo impida, incluso cuando conceptualmente debería contener un único tipo. Esto rompe contratos implícitos del diseño y obliga a añadir comprobaciones manuales o documentación adicional para evitar usos incorrectos. Estos problemas son precisamente los que la programación genérica moderna pretende solucionar mediante el chequeo de tipos en tiempo de compilación.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta

Los **parámetros de tipo** son un mecanismo de la programación genérica que permite **definir clases, interfaces o métodos indicando tipos “pendientes de concretar”**, que se especifican en el momento de su uso. En lugar de trabajar con un tipo universal como `Object`, se introduce un identificador de tipo (por convenio `T`, `E`, `K`, etc.) que representa un tipo concreto aún desconocido. Esto permite escribir código reutilizable sin perder información de tipo.

Gracias a los parámetros de tipo, el **compilador conoce y verifica el tipo real** con el que se usa una estructura genérica. De este modo, se garantiza que los objetos insertados y recuperados son coherentes, evitándose conversiones explícitas y errores en tiempo de ejecución. En comparación con el uso de `Object`, el tipo no se pierde, sino que se propaga de forma segura a lo largo de la clase o del método.

Un ejemplo sencillo en Java sería una lista genérica definida con un parámetro de tipo:

```java
public class Lista<T> {
    private T[] datos;

    public void add(T elemento) {
        // ...
    }

    public T get(int index) {
        return datos[index];
    }
}
```

En este caso, `T` actúa como un **marcador de tipo** que será sustituido, por ejemplo, por `Integer` o `String` al crear la lista. Este mecanismo supone una mejora fundamental respecto a `Object`, ya que mantiene la flexibilidad de las estructuras genéricas pero añade **chequeo de tipos en tiempo de compilación**, que es el objetivo central de la programación genérica moderna.



## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, la programación genérica se realiza mediante **generics**, que permiten parametrizar clases y colecciones con un tipo concreto. Un ejemplo habitual es `ArrayList<String>`, que indica explícitamente que la lista solo puede contener objetos de tipo `String`. El compilador garantiza que no se puedan insertar otros tipos y que, al recuperar los elementos, estos sean tratados directamente como `String`, sin necesidad de conversiones explícitas ni riesgos de error en ejecución.

A continuación se muestra un ejemplo sencillo en Java, donde se crea una lista dinámica de cadenas, se insertan valores y se recorren con total seguridad de tipos:

```java
import java.util.ArrayList;

public class EjemploJava {
    public static void main(String[] args) {
        ArrayList<String> lista = new ArrayList<>();

        lista.add("uno");
        lista.add("dos");
        lista.add("tres");

        for (String s : lista) {
            System.out.println(s.toUpperCase());
        }
    }
}
```

En este código, el bucle `for-each` deja claro que cada elemento recuperado es un `String`. El compilador impediría añadir, por ejemplo, un `Integer`, y no es necesario realizar ningún cast al acceder a los elementos, lo que ejemplifica la seguridad proporcionada por los generics.

En C++, la programación genérica se implementa mediante **templates**, que permiten definir clases y funciones independientes del tipo concreto. La biblioteca estándar proporciona el contenedor `std::vector`, que puede instanciarse con un tipo específico, como `std::string`. Al igual que en Java, el tipo se fija en tiempo de compilación y se garantiza que todos los elementos del vector sean homogéneos y seguros.

El siguiente ejemplo muestra un vector dinámico de cadenas en C++, con inserción y recorrido de sus elementos:

```c++
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> v;

    v.push_back("uno");
    v.push_back("dos");
    v.push_back("tres");

    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }

    return 0;
}
```

En este caso, `std::vector<std::string>` asegura que cada elemento es un `std::string`. El compilador rechazaría la inserción de cualquier otro tipo, y durante el recorrido no existe ambigüedad sobre el tipo de cada elemento. Tanto en Java como en C++, estos ejemplos muestran programación genérica real, ya que combinan reutilización de código con **chequeo estricto de tipos en tiempo de compilación**.



## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase con **parámetros de tipo**, el compilador utiliza esa información para **verificar la corrección de los tipos** en el uso de la clase, pero la forma concreta en la que actúa **no es la misma en Java y en C++**. En ambos lenguajes, el tipo genérico se fija en el punto de instanciación (por ejemplo, `Lista<String>` o `vector<string>`), pero el tratamiento interno que hace el compilador es radicalmente distinto, lo que tiene consecuencias importantes en tiempo de compilación y de ejecución.

En **Java**, el compilador aplica un mecanismo llamado **type erasure** (borrado de tipos). Esto significa que los parámetros de tipo **solo existen en tiempo de compilación** y se eliminan al generar el bytecode. Internamente, el código genérico se transforma en una versión basada en `Object` (o en el límite superior del tipo, si existe), insertando automáticamente casts donde sea necesario. Como resultado, todas las instancias genéricas comparten una única versión del código, y en tiempo de ejecución no se puede saber con qué tipo concreto se instanció una clase genérica.

En **C++**, el compilador emplea un mecanismo completamente distinto conocido como **instanciación de plantillas**. Cada vez que se usa una plantilla con un tipo concreto, el compilador **genera una versión específica del código** para ese tipo. Por ejemplo, `std::vector<int>` y `std::vector<std::string>` producen código distinto. Los tipos genéricos existen plenamente en tiempo de compilación, no se borran, y no hay casts implícitos ni pérdida de información de tipo.

En resumen, Java apuesta por una solución más uniforme y compatible con versiones antiguas mediante el **borrado de tipos**, mientras que C++ prioriza la generación de código especializado mediante **plantillas instanciadas**. Ambas estrategias permiten programación genérica con chequeo de tipos en compilación, pero difieren en rendimiento, capacidad de reflexión sobre tipos y comportamiento interno del compilador.

***Clase teoría***




## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

En Java, una clase con **parámetros de tipo** puede definirse indicando dichos parámetros entre signos `< >` tras el nombre de la clase. En este caso, una clase `Par<A, B>` permite alojar **dos valores de tipos potencialmente distintos**, manteniendo siempre la información de tipo y garantizando seguridad en tiempo de compilación. Cada parámetro de tipo representa un tipo concreto que se fijará al instanciar la clase.

La clase `Par` puede diseñarse de forma sencilla, con un constructor que inicialice ambos valores y un método getter para cada uno. El uso de parámetros de tipo evita recurrir a `Object` y elimina la necesidad de realizar conversiones explícitas al recuperar los valores, lo que supone una mejora clara respecto a enfoques no genéricos.

```java
public class Par<A, B> {
    private A primero;
    private B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}
```

Un uso típico de esta clase consiste en **devolver múltiples resultados** desde un método de forma segura. Por ejemplo, se puede definir una función que calcule la media y la desviación típica de un array de `double` y las devuelva agrupadas en un `Par<Double, Double>`. De este modo, el tipo de ambos valores queda perfectamente especificado.

```java
public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
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
```

Al utilizar este método, el compilador garantiza que el primer valor del `Par` es un `Double` correspondiente a la media y el segundo es un `Double` correspondiente a la desviación típica. Este ejemplo muestra cómo la programación genérica permite estructurar el código de forma clara, reutilizable y con **chequeo estricto de tipos**, sin sacrificar flexibilidad.



## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta

En Java es posible declarar **parámetros de tipo a nivel de método**, incluso aunque la clase no sea genérica. Un **método genérico** define sus propios parámetros de tipo antes del tipo de retorno y permite expresar relaciones de tipo entre sus argumentos y el resultado. Esto resulta especialmente útil cuando el comportamiento genérico es puntual y no pertenece al estado de una clase.

A continuación se muestra una versión **no genérica**, basada en `Object`, del método `seleccionaUno`. Este método acepta dos objetos cualesquiera y devuelve uno al azar. Aunque es flexible, presenta problemas claros desde el punto de vista del chequeo de tipos:

```java
import java.util.Random;

public static Object seleccionaUno(Object a, Object b) {
    Random rnd = new Random();
    return rnd.nextBoolean() ? a : b;
}
```

Con esta definición, el compilador **no garantiza que ambos objetos sean del mismo tipo**, y el valor devuelto es de tipo `Object`. Como consecuencia, el código cliente está obligado a realizar **downcasting** explícito al tipo esperado, lo que introduce riesgo de errores en tiempo de ejecución si el cast es incorrecto o si los objetos pasados no eran homogéneos.

La versión correcta mediante un **método genérico** se define introduciendo un parámetro de tipo `<T>`. De esta forma, se expresa que ambos argumentos y el valor de retorno son del **mismo tipo concreto**, fijado en el punto de llamada:

```java
public static <T> T seleccionaUno(T a, T b) {
    Random rnd = new Random();
    return rnd.nextBoolean() ? a : b;
}
```

Con esta definición se logran dos mejoras fundamentales. En primer lugar, **no es necesario realizar downcasting**, ya que el compilador conoce el tipo exacto de retorno. En segundo lugar, se **fuerza en tiempo de compilación** que ambos argumentos sean del mismo tipo; cualquier intento de pasar objetos de tipos distintos provocará un error de compilación. Esto ejemplifica claramente cómo los métodos genéricos permiten escribir código flexible sin renunciar a la seguridad de tipos.
***Clase teoría***
Ao facer un metodo genérico forzamos que se a ese metodo se devolve un `T` e se lle pasan mais de un parametro todos teñan que ser `T`, e sempre vai devolver un `T`.

```java
    class PruebaGenericos{
        public static <T> T seleccioneUnoDeLosDIos(T primero, T segundo){
            Random rand = new Random();
            return rand.nextBoolean() ? primero : segundo;
        }

    }

```


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, en Java **se pueden establecer restricciones (límites) en los parámetros de tipo** mediante la cláusula `extends`. Esto permite indicar que un parámetro genérico debe ser **un subtipo de una clase determinada o implementar una interfaz concreta**. De este modo, el compilador garantiza que ese tipo dispone, al menos, de los métodos definidos en dicha superclase o interfaz. Para el caso de los números, Java proporciona la clase abstracta `Number`, de la que heredan tipos como `Integer`, `Double` o `Float`.

Una primera solución, **sin programación genérica**, consiste en declarar las coordenadas directamente como `Number`. Esto permite aceptar cualquier tipo numérico, pero se pierde información sobre el tipo concreto y es necesario trabajar siempre a través de métodos genéricos como `doubleValue()` para operar con los valores. El compilador no puede saber si ambos puntos usan el mismo tipo numérico.

```java
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Una segunda solución, más robusta, consiste en **usar genéricos con restricción**, asegurando que el punto trabaja siempre con **un único tipo concreto de número**. Para ello, se define la clase como `Punto<T extends Number>`. Esto refuerza el chequeo de tipos, ya que el compilador garantiza que ambas coordenadas y los puntos comparados usan el mismo tipo `T`.

```java
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Respecto al **type erasure**, en ambos casos el tipo genérico **se elimina tras la compilación**. En la versión genérica, `T` se sustituye por su límite superior, es decir, **`Number`**. Por tanto, el tipo final generado por el compilador es esencialmente equivalente a la primera solución, pero con una diferencia crucial: **el chequeo de tipos se ha realizado en tiempo de compilación**, antes de que la información genérica desaparezca. Esto ilustra cómo los genéricos en Java mejoran la seguridad sin alterar el modelo de ejecución.



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

Aunque ambas soluciones permiten definir puntos reutilizables sin duplicar la clase `Punto`, **no refuerzan el chequeo de tipos del mismo modo**. En la solución sin genéricos, donde las coordenadas son de tipo `Number`, **sí es posible crear un punto con una coordenada entera y otra real**, por ejemplo `new Punto(3, 4.5)`. Esto es válido porque tanto `Integer` como `Double` heredan de `Number`, y el compilador no impone ninguna restricción adicional sobre la homogeneidad de los tipos de las coordenadas.

En cambio, en la solución con genéricos `Punto<T extends Number>`, **no es posible mezclar tipos numéricos distintos en un mismo punto**. Al fijar el parámetro de tipo `T`, ambas coordenadas deben ser exactamente del mismo tipo concreto. Por ejemplo, `Punto<Integer>` obliga a que `x` e `y` sean `Integer`, y un intento de pasar un `Double` provocaría un error de compilación. Esto supone un refuerzo claro del chequeo de tipos, ya que se captura en compilación un posible uso incoherente del modelo.

Respecto al tipo devuelto por los métodos `getX`, también existe una diferencia importante. En la solución sin genéricos, el método `getX` devuelve siempre un valor de tipo **`Number`**, lo que obliga al código cliente a tratar el resultado de forma genérica o a realizar conversiones si necesita un tipo más específico. El compilador no puede deducir más información que esa superclase común.

En la solución con genéricos, el método `getX` devuelve un valor de tipo **`T`**, es decir, el tipo concreto con el que se instanció el `Punto` (`Integer`, `Double`, etc.). Esto permite escribir código más preciso y seguro, ya que el tipo exacto se conserva en el punto de uso, aunque se elimine después por *type erasure*. En resumen, los genéricos no cambian lo que es posible hacer en tiempo de ejecución, pero **mejoran de forma significativa la detección anticipada de errores en tiempo de compilación**.



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

A continuación se muestra **la versión genérica del diseño**, partiendo exactamente del problema planteado, y aplicando **genéricos auto-referenciados** para garantizar en **tiempo de compilación** que la distancia solo se calcula entre puntos del **mismo tipo concreto**, eliminando por completo `instanceof` y *downcasting*.

***


Se añade un parámetro de tipo `T` que representa **el propio subtipo concreto** del punto. Esto obliga a que cualquier implementación especifique consigo misma el tipo válido.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

Este patrón expresa que “un punto solo sabe calcular distancias a **su mismo tipo**”.

***



```java
public class Punto2D implements Punto<Punto2D> {
    private final double x;
    private final double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2)
        );
    }
}
```

Aquí el método **solo acepta `Punto2D`**, por lo que:

*   No es necesario `instanceof`
*   No existe *downcasting*
*   El compilador garantiza el uso correcto

***



```java
public class Punto3D implements Punto<Punto3D> {
    private final double x;
    private final double y;
    private final double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }



    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2) +
            Math.pow(z - p.z, 2)
        );
    }
}
```

***



```java
public class Main {
    public static void main(String[] args) {
        Punto2D a2 = new Punto2D(1, 2);
        Punto2D b2 = new Punto2D(4, 6);

        Punto3D a3 = new Punto3D(1, 2, 3);
        Punto3D b3 = new Punto3D(4, 6, 8);

        System.out.println(a2.distanciaA(b2)); // ✅ OK
        System.out.println(a3.distanciaA(b3)); // ✅ OK

        // a2.distanciaA(a3); ❌ ERROR de compilación
    }
}
```

***


Este ejemplo ilustra un uso **avanzado y realista de genéricos**, donde no solo se parametrizan datos, sino que se **expresan invariantes del diseño** directamente en el tipo, algo imposible de lograr con herencia tradicional sin genéricos.



## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
