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

El **polimorfismo** es un principio de la programación orientada a objetos que permite tratar objetos de distintas clases relacionadas por herencia como si fueran del mismo tipo base, dejando que cada uno se comporte según su implementación concreta. Esto significa que una referencia de una clase padre puede apuntar a objetos de diferentes clases hijas y que, al invocar un método, se ejecute la versión adecuada según el objeto real. Su utilidad principal es aumentar la flexibilidad del código y reducir el acoplamiento, ya que no es necesario conocer el tipo concreto del objeto para usarlo correctamente.

Desde un punto de vista práctico, el polimorfismo sirve para escribir código más general y reutilizable. En lugar de programar decisiones explícitas con condicionales que distingan cada caso (algo habitual en programación estructurada), se delega el comportamiento específico a cada subclase. De este modo, añadir nuevas funcionalidades suele implicar crear nuevas clases sin modificar el código existente, lo que facilita el mantenimiento y la extensión de los programas.

La **sobreescritura de métodos** es el mecanismo que hace posible el polimorfismo en tiempo de ejecución. Consiste en que una clase hija proporcione su propia implementación de un método que ya está definido en la clase padre, manteniendo la misma firma (nombre, parámetros y tipo de retorno compatible). Cuando se invoca ese método a través de una referencia del tipo padre, el lenguaje selecciona automáticamente la versión correspondiente a la clase real del objeto, no a la de la referencia.

```java
class Animal {
    void hacerSonido() {
        System.out.println("El animal hace un sonido");
    }
}

class Perro extends Animal {
    @Override
    void hacerSonido() {
        System.out.println("El perro ladra");
    }
}

Animal a = new Perro();
a.hacerSonido(); // Se ejecuta el método de Perro
```



## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La **ligadura dinámica** o **enlace tardío** consiste en que la decisión de qué implementación de un método se ejecuta no se toma en tiempo de compilación, sino en **tiempo de ejecución**, basándose en el tipo real del objeto y no en el tipo de la referencia. Cuando se invoca un método mediante una referencia a una clase base, el lenguaje determina dinámicamente qué versión del método debe ejecutarse según la clase concreta del objeto al que apunta esa referencia.

La relación con el **polimorfismo** es directa y fundamental: el polimorfismo en tiempo de ejecución solo es posible gracias a la ligadura dinámica. Sin enlace tardío, el mismo método llamado sobre referencias del mismo tipo ejecutaría siempre la misma implementación, lo que impediría que cada subclase aportase su propio comportamiento. Por tanto, puede decirse que la ligadura dinámica es el mecanismo interno que permite que el polimorfismo funcione correctamente en lenguajes orientados a objetos.

En **C++**, la ligadura dinámica **no es automática** para todos los métodos. Para que un método se resuelva mediante enlace tardío, debe declararse explícitamente como `virtual` en la clase base. Si no se hace, el enlazado será estático (en tiempo de compilación), incluso aunque haya herencia. En cambio, en **Java**, la ligadura dinámica es el comportamiento **por defecto** para los métodos de instancia: todos los métodos no `static`, no `final` y no `private` usan enlace tardío sin necesidad de indicarlo explícitamente, lo que simplifica su uso y reduce errores comunes.

En **Python**, la ligadura dinámica es aún más flexible y forma parte del propio modelo del lenguaje. Todos los métodos se resuelven dinámicamente y ni siquiera es necesario declarar tipos explícitos para aplicar polimorfismo. El método que se ejecuta depende únicamente de los métodos disponibles en el objeto en tiempo de ejecución, independientemente de su jerarquía de clases. Por ello, en Python el polimorfismo y el enlace tardío son implícitos y naturales, sin requerir ninguna palabra clave especial ni configuración adicional.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

A continuación se muestra un ejemplo sencillo en Java que ilustra el **polimorfismo** mediante la sobreescritura de un método. Se define una clase base `Soldado` con un método `saludar`, y dos subclases `Zapador` y `Artillero`. La clase `Zapador` sobreescribe completamente el comportamiento del método, mientras que `Artillero` hereda la implementación original sin modificarla.

Este ejemplo permite observar que, aunque se utilicen **referencias de tipo `Soldado`**, el método que se ejecuta depende del **tipo real del objeto** en tiempo de ejecución. Esto demuestra el enlace dinámico característico del polimorfismo, donde no es necesario conocer de antemano la subclase concreta para que el método correcto sea invocado.

Finalmente, al recorrer un array de `Soldado`, se comprueba que cada objeto responde de forma distinta a la misma llamada al método `saludar`, sin usar condicionales ni comprobaciones explícitas de tipo. De este modo, se evidencia cómo el polimorfismo simplifica el código y mejora su extensibilidad.

```java
class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        System.out.println("El zapador saluda llevando equipo de demolición.");
    }
}

class Artillero extends Soldado {
    // No sobreescribe el método saludar
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[2];
        ejercito[0] = new Zapador();
        ejercito[1] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar(); // Polimorfismo en acción
        }
    }
}
```



## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, al **sobreescribir un método** es posible invocar la implementación de la **clase base** y trabajar a partir de su comportamiento. Esto permite reutilizar la lógica ya existente y ampliar o modificar solo aquellas partes que interesan, en lugar de sustituir completamente el método. Esta práctica es habitual cuando se quiere especializar una acción sin perder el comportamiento común definido en la superclase.

En Java, para invocar un método de la clase base desde una subclase se utiliza la palabra clave **`super`**. Mediante `super.metodo()`, se fuerza la llamada a la versión del método definida en la clase padre, independientemente de que haya sido sobreescrito en la clase hija. De este modo, se puede ejecutar primero el comportamiento general y, a continuación, añadir funcionalidad específica.

En el siguiente ejemplo, la clase `Zapador` sobreescribe el método `saludar`, pero comienza llamando al método base de `Soldado`. Tras ello, añade un mensaje adicional propio del zapador. Se observa cómo no se pierde el saludo estándar y, al mismo tiempo, se introduce un comportamiento especializado.

```java
class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar(); // llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

La palabra clave del lenguaje utilizada para invocar el método de la clase base es **`super`**, y su uso resulta fundamental cuando se quiere combinar herencia, sobreescritura y reutilización de código de forma controlada.



## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al **sobreescribir un método en Java**, deben cumplirse varias restricciones relacionadas con su firma. Los **parámetros deben coincidir exactamente** en número, orden y tipo con los del método de la clase base; no es posible cambiarlos, ya que dejaría de considerarse una sobreescritura. En cuanto al **tipo de retorno**, debe ser el mismo o uno **compatible por covarianza**, es decir, una subclase del tipo de retorno original. Además, al sobreescribir no se puede **reducir la visibilidad** del método (por ejemplo, no puede pasar de `public` a `protected`) y tampoco se pueden lanzar excepciones comprobadas más generales que las declaradas en el método base.

La **sobreescritura (overriding)** y la **sobrecarga (overloading)** son conceptos distintos aunque suelen confundirse. La sobreescritura se produce entre una clase base y una subclase, con la misma firma de método, y está ligada al **polimorfismo y al enlace dinámico**, ya que el método que se ejecuta se decide en tiempo de ejecución. En cambio, la sobrecarga consiste en definir varios métodos con el **mismo nombre pero distinta lista de parámetros** dentro de una misma clase (o heredados), y su resolución se hace en **tiempo de compilación**, sin relación directa con el polimorfismo.

La anotación **`@Override`** sirve para indicar explícitamente que un método pretende sobreescribir otro definido en una superclase (o interfaz). Su principal utilidad es que el compilador puede **detectar errores**, como cambios accidentales en la firma del método, errores tipográficos o intentos de sobreescribir un método que en realidad no existe en la clase base. Por este motivo, se recomienda usar `@Override` siempre, ya que mejora la **seguridad del código**, facilita el mantenimiento y hace más clara la intención del programador al leer la clase.



## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, al estudiar Java se empieza a emplear el **polimorfismo desde etapas muy tempranas**, aunque a menudo se haga de forma implícita y sin nombrarlo explícitamente. Todas las clases en Java heredan de `Object`, y métodos como `toString`, `equals` o `hashCode` están pensados precisamente para ser **sobreescritos** por las clases hijas. Cuando se redefinen estos métodos, se está preparando el objeto para que responda correctamente en contextos genéricos donde solo se conoce el tipo `Object`.

Ahora bien, es importante matizar que **sobreescribir un método no es, por sí solo, usar polimorfismo**; el polimorfismo aparece cuando ese método se invoca a través de una **referencia a la clase base** y se ejecuta la versión de la clase concreta en tiempo de ejecución. Por ejemplo, cuando un objeto se imprime con `System.out.println(objeto)`, internamente se llama a `toString()` usando una referencia genérica, y es entonces cuando se manifiesta el polimorfismo al ejecutarse la versión sobreescrita en la clase real del objeto.

Por tanto, al sobreescribir `toString` o `equals` ya se está **participando en el mecanismo del polimorfismo**, aunque muchas veces no se sea consciente de ello. Esto demuestra que el polimorfismo no es un concepto avanzado que aparezca solo en ejemplos complejos, sino una característica fundamental del diseño de Java que se utiliza desde los primeros programas, incluso antes de estudiarlo de forma formal y explícita.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
