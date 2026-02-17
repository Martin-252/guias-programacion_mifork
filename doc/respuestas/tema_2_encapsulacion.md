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

La **encapsulación** en POO busca agrupar dentro de una misma unidad (la clase) tanto los datos como los métodos que operan sobre ellos. Con esto se pretende que el estado interno del objeto quede protegido frente a accesos o modificaciones directas desde el exterior. La **ocultación de información**, por su parte, es el mecanismo que impide que ciertos detalles internos sean visibles para otros componentes del programa. Mientras la encapsulación organiza y agrupa, la ocultación controla *qué* puede verse y modificarse desde fuera. Juntos permiten que un objeto mantenga su integridad y se use únicamente a través de una interfaz bien definida.

En esencia, se busca separar claramente la **interfaz pública** de una clase (lo que se ofrece a otros objetos) de su **implementación interna** (cómo se realiza realmente ese comportamiento). Esto facilita que los atributos permanezcan privados y que cualquier modificación a esos valores se realice mediante métodos controlados. Así se reduce el riesgo de inconsistencias, errores o modificaciones accidentales del estado interno del objeto, algo especialmente útil cuando se viene de programación estructurada, donde las variables suelen estar más expuestas.

Entre las ventajas de la ocultación de información pueden mencionarse varias. En primer lugar, permite **proteger el estado interno** del objeto, impidiendo accesos indebidos y garantizando que las reglas de uso se cumplan siempre. También **favorece el mantenimiento** del código, ya que los detalles internos pueden cambiar sin afectar a quienes utilizan la clase, siempre que la interfaz pública se mantenga. Por último, contribuye a **reducir el acoplamiento** entre módulos, facilitando la reutilización y una mayor robustez del sistema.



## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La **interfaz pública** de un objeto o clase en POO se entiende como el conjunto de métodos y atributos accesibles desde el exterior. Representa aquello que otros objetos pueden utilizar para interactuar con la clase, sin necesidad de conocer cómo están implementados internamente esos comportamientos. En términos prácticos, equivale al “contrato” que la clase ofrece al resto del programa: qué operaciones permite realizar y qué información expone de forma controlada.

Esta interfaz pública suele componerse de métodos marcados como `public`, que actúan como puntos de entrada para acceder a las funcionalidades del objeto. Desde la perspectiva de alguien que viene de C/C++ sin orientación a objetos, podría compararse con las funciones declaradas en un archivo de cabecera (`.h`), mientras que la implementación quedaría protegida en el archivo `.cpp`. Sin embargo, en POO esta separación se integra dentro de la propia clase, reforzando la idea de modularidad.

La relación con la **ocultación de información** es directa: la interfaz pública define *lo que se muestra*, mientras que los detalles internos permanecen escondidos mediante áreas `private` o `protected`. Esto permite que la clase oculte su estructura interna, asegurando que el estado solo se modifique a través de métodos controlados. Gracias a ello, se evita que código externo manipule atributos sensibles de forma incorrecta, y se gana estabilidad y flexibilidad para modificar la implementación sin romper el código que utiliza dicha interfaz.

De esta forma, la interfaz pública actúa como una barrera bien definida entre el exterior y el interior de un objeto. El usuario de la clase solo necesita conocer qué métodos puede llamar y qué comportamiento esperar, mientras que la clase conserva libertad para cambiar cómo realiza internamente esas operaciones. Esto refuerza el principio central de la encapsulación: proteger los datos y exponer únicamente lo necesario para un uso correcto.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

La **interfaz pública** de una clase debe diseñarse con especial cuidado porque actúa como el punto de contacto entre la clase y el resto del programa. Todo aquello que se expone públicamente se convierte en una especie de contrato que otros módulos utilizan y del que dependen. Si la interfaz está mal diseñada —por ejemplo, si expone demasiado, si ofrece métodos confusos o si permite un uso incorrecto del objeto— el mantenimiento del software se vuelve más difícil y aumenta el riesgo de errores. Una interfaz clara, mínima y bien pensada facilita que la clase sea más robusta y más fácil de usar sin conocer su implementación interna.

No es fácil cambiar una interfaz pública una vez que otros componentes dependen de ella. Cualquier modificación puede obligar a actualizar múltiples partes del código que la utilizan, provocando incompatibilidades o introduciendo nuevos fallos. Además, modificar la interfaz rompe la abstracción que proporciona la ocultación de información, ya que obliga a los usuarios de la clase a adaptarse a los cambios. Por este motivo se recomienda exponer únicamente lo necesario y mantener la interfaz estable siempre que sea posible, lo cual reduce el impacto sobre el resto del sistema y facilita la evolución interna de la clase sin consecuencias externas.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las **invariantes de clase** son condiciones que deben cumplirse siempre para garantizar que el estado interno de un objeto sea válido. Se trata de reglas lógicas que describen cómo deben estar relacionados los atributos de la clase en cualquier momento en que el objeto sea visible desde el exterior. Estas condiciones deben mantenerse tras la construcción del objeto y después de ejecutar cualquier método público, de modo que la clase no quede en un estado incoherente. En muchos casos, las invariantes funcionan como una garantía de que el objeto se comportará correctamente si se utiliza a través de su interfaz establecida.

La **ocultación de información** ayuda a mantener estas invariantes porque impide que código externo modifique directamente el estado interno del objeto. Si los atributos son privados, solo pueden cambiarse a través de métodos controlados, donde la clase puede verificar y asegurar que las reglas internas se cumplen siempre. Esto evita que el estado pueda ser alterado de manera arbitraria o incorrecta, lo que reduciría la validez de las invariantes. Además, permite centralizar las comprobaciones en puntos específicos de la clase.

Cuando el programador controla todas las modificaciones del estado interno, resulta más sencillo garantizar que las invariantes se mantengan a lo largo de la ejecución. La interfaz pública actúa como un filtro que regula qué cambios se pueden realizar y en qué condiciones, mientras que la implementación privada protege el núcleo de datos. Gracias a ello, se consigue una mayor robustez y previsibilidad en el comportamiento del objeto.

De esta forma, las invariantes de clase y la ocultación de información trabajan de manera complementaria. Las invariantes definen cómo debe ser el estado correcto de un objeto, y la ocultación proporciona los medios para evitar que ese estado se rompa accidentalmente. Esto facilita tanto el mantenimiento como la evolución del código, ya que se reduce el riesgo de corromper la lógica interna por accesos incontrolados.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

A continuación se muestra un ejemplo sencillo de una clase `Punto` en Java que aplica **ocultación de información** utilizando atributos privados y métodos públicos:

```java
public class Punto {

    // Atributos privados (ocultación de información)
    private double x;
    private double y;

    // Constructor público
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Métodos de acceso (getters)
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    // Método para calcular la distancia al origen
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

La **interfaz pública** de esta clase está formada por todo aquello marcado como `public`. En este caso, incluye el constructor `Punto(double, double)`, los métodos `getX()`, `getY()` y el método `calcularDistanciaAOrigen()`. Estos elementos constituyen los puntos de acceso que otros objetos pueden utilizar para interactuar con la clase sin conocer cómo se guardan o manejan internamente los datos. La interfaz pública define, por tanto, qué operaciones pueden realizarse sobre un objeto `Punto` de manera segura y controlada.

La palabra clave **`private`** indica que un atributo o método solo puede usarse desde dentro de la propia clase. Gracias a ello, se evita que código externo modifique directamente los valores de `x` e `y`, lo cual protege las invariantes y garantiza un uso coherente del objeto. Por su parte, **`public`** permite que esos elementos sean accesibles desde otras clases, formando los canales legítimos mediante los cuales se consulta o utiliza el comportamiento del objeto. Esta separación entre lo público y lo privado es fundamental para la encapsulación, ya que asegura que el interior del objeto permanezca protegido y que su uso desde fuera se produzca solo mediante operaciones controladas.

Gracias a esta estructura, la clase `Punto` puede cambiar su implementación interna cuando sea necesario sin afectar a quienes la utilizan, siempre que la interfaz pública se mantenga estable. Esto facilita la evolución del código y lo hace más robusto, al tiempo que reduce el riesgo de errores producidos por accesos indebidos a los datos internos. ¿Quieres que prepare otra clase similar, o que incorpore setters y explique cómo afectan a las invariantes?


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores `public` y `private` pueden aplicarse **a clases, atributos, métodos y constructores**, aunque con ciertas restricciones. El modificador `public` puede utilizarse en **clases de nivel superior (top‑level)**, siempre que el archivo tenga el mismo nombre que la clase pública. También puede aplicarse a **métodos, atributos y constructores**, permitiendo que sean accesibles desde cualquier otra clase del programa, incluso desde otros paquetes. Esto convierte a `public` en la base de la interfaz pública de una clase.

Por su parte, el modificador `private` **no puede aplicarse a clases de nivel superior**, pero sí a **clases internas**, además de a **atributos, métodos y constructores**. Cuando algo se declara como `private`, solo es accesible desde dentro de la propia clase, lo que permite reforzar la ocultación de información y proteger el estado interno del objeto frente a usos indebidos. Esto ayuda a que la clase mantenga su coherencia al controlar estrictamente cómo se modifican sus datos internos.

El diseño cuidadoso de estos modificadores permite definir con claridad la frontera entre lo que se expone al exterior y lo que permanece oculto. De esta forma, `public` sirve para establecer los puntos de acceso seguros, mientras que `private` contribuye a garantizar que los detalles internos se mantengan protegidos. Esta separación es fundamental para la encapsulación y para construir clases robustas y fáciles de mantener.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

En POO suelen distinguirse varios niveles de visibilidad más allá de lo **público** y **privado**, aunque los nombres y posibilidades dependen del lenguaje. En términos generales, muchos lenguajes incluyen también niveles de acceso **protegido** y, en ocasiones, niveles intermedios que limitan la visibilidad según módulos, paquetes o espacios de nombres. El objetivo de estos niveles adicionales es ofrecer un control más fino sobre qué elementos pueden verse desde qué partes del programa, permitiendo construir arquitecturas más modulares y seguras.

En **Java**, además de `public` y `private`, existen dos niveles adicionales:

*   **`protected`**: accesible desde la propia clase, sus subclases y clases del mismo paquete.
*   ***default* (o *package‑private*)**: cuando no se especifica ningún modificador; accesible solo desde clases del mismo paquete.

Esto significa que Java define **cuatro** niveles de visibilidad: `public`, `protected`, *package‑private* y `private`. Cada uno determina hasta qué punto un elemento puede formar parte de la interfaz pública o quedar restringido a zonas más internas del proyecto. Esta variedad permite diseñar APIs más limpias y evitar dependencias no deseadas entre componentes.

En otros lenguajes de POO existen variaciones. En **C++**, por ejemplo, se usan también `public`, `protected` y `private`, pero no existe el concepto de paquete; además, todo gira en torno a clases y herencia. En **C#**, además de los tres anteriores, existen modificadores como `internal` y `protected internal`, que controlan el acceso a nivel de ensamblado. Algunos lenguajes modernos, como **Swift** o **Kotlin**, incluyen niveles adicionales como `internal` o `module`, que restringen la visibilidad al módulo de compilación. En todos los casos, el propósito es permitir que el programador ajuste la visibilidad a las necesidades de organización y seguridad del código.

Así, aunque la idea básica de visibilidad pública o privada es común a la mayoría de lenguajes orientados a objetos, cada lenguaje ofrece capas adicionales para controlar la accesibilidad según jerarquía, módulos o empaquetado. Esto da mayor flexibilidad en el diseño de software y facilita aplicar la encapsulación de forma más precisa según el contexto. ¿Quieres que prepare una tabla comparativa entre Java, C++ y C#?


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

**Respuesta breve:** En Java, los **miembros de instancia `private`** están **ocultos para otras clases**, pero **no** están ocultos **entre instancias de la misma clase**. Cualquier método definido dentro de la clase puede acceder a los campos `private` **tanto del propio objeto (`this`) como de otro objeto** del mismo tipo recibido como parámetro. Por tanto, la opción correcta es **(a) otras clases**; **no** se ocultan frente a **(b) otras instancias** si el acceso ocurre **dentro** del código de la propia clase.

Esto ocurre porque la visibilidad `private` se aplica **a nivel de clase**, no a nivel de objeto. La regla indica que *solo el código de la clase* puede acceder a sus miembros privados; si se dispone de una referencia a otro objeto de esa misma clase, ese mismo código puede leer o modificar sus campos privados. Esto permite escribir métodos que comparan o combinan el estado de dos instancias sin romper la encapsulación, ya que el acceso sigue estando controlado y localizado en el interior de la clase.

**Ejemplo con `calcularDistanciaAPunto(Punto otro)`:**

```java
public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    // Distancia al origen
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Distancia a otro punto: se accede a sus campos privados porque
    // el acceso ocurre dentro de la MISMA clase Punto
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;   // acceso legal: 'otro.x' es private pero estamos en Punto
        double dy = this.y - otro.y;   // acceso legal: 'otro.y' es private pero estamos en Punto
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En este código se observa que `calcularDistanciaAPunto` accede a `otro.x` y `otro.y` directamente, aun siendo `private`. Esto es válido porque el acceso se realiza **desde dentro de la clase `Punto`**. Si se intentara leer `p.x` desde **otra clase** (por ejemplo, `Main`), el compilador lo prohibiría. De este modo, se mantiene la **ocultación de información** frente al exterior, al tiempo que se permite la colaboración segura entre instancias dentro de la misma abstracción.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los **métodos *getter* y *setter*** son funciones utilizadas en lenguajes orientados a objetos para **acceder** y **modificar** atributos privados de una clase de manera controlada. Su finalidad principal es mantener la **encapsulación**, permitiendo que los datos internos estén protegidos mediante `private`, mientras se ofrece una interfaz segura para leer o actualizar esos valores. En este sentido, los *getter* permiten obtener el valor de un atributo sin exponerlo directamente, y los *setter* permiten modificarlo verificando, si es necesario, que la modificación sea válida.

Un **método *getter*** suele simplemente **devolver el valor de un atributo privado**, sin permitir que el exterior modifique ese dato. En Java, por ejemplo, se nombra normalmente como `getAtributo()`. Este mecanismo garantiza que otros objetos puedan consultar la información sin acceder directamente al campo interno, lo que preserva la ocultación de información. El *getter* forma parte de la interfaz pública y permite mantener la lectura del estado del objeto bajo control.

Por otro lado, un **método *setter*** se utiliza para **modificar el valor de un atributo privado**, normalmente implementando comprobaciones o restricciones antes de asignar el nuevo valor. En Java se nombra habitualmente como `setAtributo(...)`. Gracias a este diseño, la clase decide cómo y cuándo pueden cambiar sus datos internos, lo que ayuda a mantener las **invariantes de clase** y a evitar estados inválidos. Un *setter* mal diseñado podría romper la encapsulación si permite valores incoherentes, por lo que su uso debe plantearse con cautela.

En conjunto, los *getter* y *setter* constituyen una parte importante de la **interfaz pública** de una clase cuando se desea exponer ciertos datos sin sacrificar la encapsulación. No obstante, en un diseño orientado a objetos bien estructurado, suelen recomendarse solo cuando es necesario: si la clase puede ofrecer un comportamiento más abstracto en vez de exponer atributos directamente mediante *getters* y *setters*, se preserva mejor la encapsulación y la autonomía del objeto.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, cuando en POO se dice que la **ocultación de información mejora la “seguridad”**, no se está hablando de seguridad en el sentido de evitar *hackeos* o ataques informáticos. En este contexto, la palabra *seguridad* se usa en un sentido **interno al diseño del software**, no en un sentido de ciberseguridad. El objetivo es evitar que el propio código de la aplicación manipule datos internos de forma incorrecta, accidental o incoherente.

La ocultación de información aumenta la **seguridad lógica del programa**, porque impide que partes externas modifiquen atributos sensibles sin pasar por métodos controlados. Gracias a ello, la clase puede mantener sus invariantes, evitar estados inconsistentes y protegerse de errores provocados por un mal uso involuntario. Se trata de prevenir fallos de programación, no ataques maliciosos.

Por tanto, la “seguridad” en este contexto significa que el objeto se **protege frente a usos indebidos dentro del propio código del desarrollador**, no frente a amenazas externas. La encapsulación ayuda a que el software sea más robusto, fácil de mantener y menos propenso a errores, pero no constituye un mecanismo de defensa ante vulnerabilidades de seguridad informática.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un **miembro de instancia** es un atributo o método que pertenece a cada objeto individual creado a partir de una clase. Esto significa que cada instancia mantiene su propia copia de esos datos, y las operaciones que actúan sobre ellos afectan solo al objeto concreto. Por ejemplo, en una clase `Punto`, las coordenadas `x` e `y` son miembros de instancia: cada punto tiene sus propios valores. Esta idea resulta familiar viniendo de C/C++, donde cada estructura (`struct`) tiene su propio conjunto de campos para cada variable creada.

En contraste, un **miembro de clase** es un atributo o método que pertenece a la clase en sí y no a cada objeto. En Java, se declaran con la palabra clave `static`. La clase mantiene una única copia del miembro, compartida por todas las instancias. Esto permite almacenar información común o implementar utilidades independientes del estado de cualquier objeto concreto. Por ejemplo, si se quisiera contar cuántos `Punto` se han creado, ese contador sería un miembro de clase.

Respecto a la visibilidad, **los miembros de clase también pueden ocultarse**. Un atributo o método estático puede declararse como `private`, lo que impide que otras clases accedan directamente a él, del mismo modo que ocurre con los miembros de instancia. La ocultación sigue funcionando igual, porque el modificador de acceso se aplica al elemento dentro de la clase, sin importar si es estático o no. Esto permite proteger datos globales compartidos y evitar modificaciones externas que puedan comprometer el funcionamiento de la clase.

Así, tanto los miembros de instancia como los de clase pueden beneficiarse de la encapsulación. La diferencia está en su ámbito: los primeros pertenecen a cada objeto y los segundos son compartidos por todos ellos. Pero ambos pueden ser públicos o privados según lo que convenga al diseño, permitiendo controlar la interfaz pública y preservar la coherencia del estado interno, ya sea individual o compartido.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, **tiene sentido que los constructores sean privados**, aunque solo en situaciones específicas. Un constructor privado impide que otras clases creen instancias directamente, lo que puede resultar útil cuando la clase desea **controlar completamente cómo y cuándo se crean sus objetos**. De esta forma, se puede forzar que todas las instancias pasen por métodos estáticos especializados, o incluso impedir que existan múltiples instancias.

Un caso típico es el **patrón Singleton**, donde se desea que exista una única instancia en todo el programa. Al hacer el constructor privado, se evita que otros componentes creen nuevos objetos, y la propia clase proporciona una instancia única mediante un método público estático. Esto ayuda a mantener invariantes y a garantizar que solo exista un objeto que represente ese recurso compartido.

Otra situación donde un constructor privado tiene sentido es cuando se quiere ofrecer **métodos de fábrica (factory methods)** para crear objetos de maneras controladas. En estos casos, la clase oculta los detalles de construcción y expone métodos con nombres más expresivos, como `crearDesdeCoordenadasPolaras(...)`, evitando que el usuario deba conocer la representación interna. Esta técnica mejora la encapsulación y facilita la evolución de la implementación.

En resumen, aunque la mayoría de clases utiliza constructores públicos, declarar un constructor como privado puede ser una herramienta útil para controlar el proceso de creación de instancias. Esto permite reforzar la encapsulación, proteger invariantes y guiar al usuario hacia formas de construcción más claras y seguras. ¿Quieres que prepare un ejemplo de un Singleton o una clase con factory methods?


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los **miembros de clase** se indican con la palabra clave `static`. Pertenecen a la **clase** y no a cada objeto, por lo que existe **una única copia compartida** entre todas las instancias. Esto permite mantener información global o utilidades independientes del estado concreto de un objeto. Se accede a ellos preferentemente mediante el **nombre de la clase** (p. ej., `Punto.getMaxX()`), lo que refuerza que no dependen de una instancia.

A continuación, se muestra la clase `Punto` extendida para incluir **miembros de clase** que registran los valores máximos de `x` e `y` que se han establecido en **cualquier** punto creado hasta el momento. Se actualizan en el **constructor** (y también en los *setters*, si existen), y se exponen mediante **métodos estáticos** de solo lectura. De este modo se preserva la encapsulación: los máximos no pueden modificarse desde fuera arbitrariamente, solo evolucionan a través del uso normal de la clase.

```java
public class Punto {

    // --- Estado de instancia (ocultación de información) ---
    private double x;
    private double y;

    // --- Estado de clase (compartido por todas las instancias) ---
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // Constructor de instancia
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    // Getters de instancia
    public double getX() { return x; }
    public double getY() { return y; }

    // (Opcional) Setters de instancia que mantienen invariantes y actualizan máximos
    public void setX(double x) {
        this.x = x;
        actualizarMaximos(this.x, this.y);
    }

    public void setY(double y) {
        this.y = y;
        actualizarMaximos(this.x, this.y);
    }

    // Método de instancia
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // --- Interfaz pública de clase (miembros estáticos) ---
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // (Opcional) Método de clase para reiniciar los máximos si interesa
    public static void reiniciarMaximos() {
        maxX = Double.NEGATIVE_INFINITY;
        maxY = Double.NEGATIVE_INFINITY;
    }

    // --- Implementación privada de clase ---
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}
```

La **interfaz pública** relacionada con los miembros de clase está formada por `getMaxX()`, `getMaxY()` (y, si se desea, `reiniciarMaximos()`), todos marcados como `static`. Los campos `maxX` y `maxY` están **ocultos** (`private`) para impedir modificaciones externas y solo cambian a través de `actualizarMaximos(...)`, que también es **privado y estático**. Este diseño mantiene la encapsulación y separa claramente el **estado por objeto** del **estado compartido**. Si se necesitara concurrencia real, podría considerarse sincronización o tipos atómicos; para uso básico de un solo hilo, la solución mostrada es suficiente.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta



```java
public static Punto crearRedondeando(double x, double y) {
    double rx = Math.round(x);
    double ry = Math.round(y);
    return new Punto(rx, ry);
}
```

Sí, se ha usado `static`, porque un método factoría **no depende de una instancia concreta**: su función es crear objetos nuevos, por lo que tiene sentido que pertenezca a la **clase**, no a un objeto ya existente.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


Se han preservado los nombres y las signaturas públicas (`Punto(double,double)`, `getX()`, `getY()`, `setX(...)`, `setY(...)`, `calcularDistanciaAOrigen()`, `calcularDistanciaAPunto(Punto)`, `getMaxX()`, `getMaxY()`, `reiniciarMaximos()`, y `crearRedondeando(...)`). Internamente, `coords[0]` representa `x` y `coords[1]` representa `y`. Como antes, los máximos globales se mantienen con miembros de **clase** `static` y se actualizan en el constructor y los setters.

```java
public class Punto {

    // --- Estado interno oculto mediante array ---
    private final double[] coords = new double[2]; // coords[0] = x, coords[1] = y

    // --- Estado de clase (compartido) ---
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // Constructor público (interfaz inalterada)
    public Punto(double x, double y) {
        this.coords[0] = x;
        this.coords[1] = y;
        actualizarMaximos(x, y);
    }

    // Getters públicos (interfaz inalterada)
    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    // Setters públicos (interfaz inalterada)
    public void setX(double x) {
        this.coords[0] = x;
        actualizarMaximos(this.coords[0], this.coords[1]);
    }

    public void setY(double y) {
        this.coords[1] = y;
        actualizarMaximos(this.coords[0], this.coords[1]);
    }

    // Método de instancia existente: distancia al origen
    public double calcularDistanciaAOrigen() {
        double x = coords[0], y = coords[1];
        return Math.sqrt(x * x + y * y);
    }

    // Método de instancia existente: distancia a otro punto
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coords[0] - otro.coords[0]; // acceso permitido dentro de la misma clase
        double dy = this.coords[1] - otro.coords[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Miembros de clase para consultar máximos globales
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // Opción para reiniciar los máximos
    public static void reiniciarMaximos() {
        maxX = Double.NEGATIVE_INFINITY;
        maxY = Double.NEGATIVE_INFINITY;
    }

    // Implementación privada y estática para mantener los máximos
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    // Método factoría existente que redondea al entero más cercano
    public static Punto crearRedondeando(double x, double y) {
        double rx = Math.round(x);
        double ry = Math.round(y);
        return new Punto(rx, ry);
    }
}
```

Esta sustitución ilustra cómo la **encapsulación** permite cambiar la **representación interna** (dos campos vs. un array) sin afectar al código cliente, siempre que la interfaz pública permanezca estable. Si se quisiera reforzar aún más la inmutabilidad del array, podría añadirse documentación y evitar exponerlo jamás; incluso podría considerarse una clase inmutable eliminando setters, aunque eso sí modificaría la interfaz actual.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Declarar un atributo **público** solo porque va a tener *getter* y *setter* no es una buena práctica en POO. Aunque funcionalmente parezca equivalente, exponer directamente el atributo rompe la **encapsulación**, porque cualquier parte del programa puede modificarlo sin pasar por una capa de control. En cambio, los métodos *getter* y *setter* permiten que la clase mantenga un punto centralizado donde validar valores, actualizar otros atributos dependientes o preservar ciertas reglas internas. Esa capacidad de control se pierde si el atributo es público.

La **convención más habitual** en prácticamente todos los lenguajes orientados a objetos modernos es declarar los **atributos como privados** y exponer solo los métodos necesarios mediante la interfaz pública. Esto evita que el código externo dependa de cómo están representados internamente los datos y permite cambiar dicha representación sin afectar al resto del programa. Por ejemplo, si un par de coordenadas pasan de ser dos `double` a un array interno —como en tu ejercicio anterior—, nada cambia para quien usa la clase porque no accede directamente a los atributos.

Este tema está íntimamente relacionado con las **invariantes de clase**. Una invariante es una condición lógica que siempre debe cumplirse para que el objeto sea válido. Si los atributos fuesen públicos, el código externo podría saltarse las reglas de la clase y dejar el objeto en un estado incoherente. En cambio, si los atributos son privados y solo pueden modificarse a través de *setters*, la clase puede comprobar siempre que el nuevo valor respeta sus invariantes antes de aceptarlo. Así, los métodos sirven tanto para exponer funcionalidad como para proteger la consistencia interna del objeto.

En resumen, aunque pueda parecer más simple hacer un atributo público cuando existe un *getter* y un *setter*, lo adecuado es mantenerlo **privado**. Esto protege la estructura interna del objeto, permite asegurar las invariantes y garantiza que la clase conserve el control sobre cómo se utiliza su propio estado.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase **inmutable** es aquella cuyo estado no puede cambiar una vez que el objeto ha sido construido. Esto implica que todos sus atributos son privados y no existen métodos que permitan modificarlos después de la inicialización. En lugar de alterar el estado interno, cualquier operación que conceptualmente “cambie” algo devuelve **un nuevo objeto** con los valores actualizados. Este enfoque hace que los objetos sean más simples de razonar y evita que queden en estados incoherentes, ya que sus valores permanecen constantes durante toda su vida.

Un **método modificador** es cualquier método que **alteré el estado interno** del objeto, es decir, que cambie el valor de uno o más atributos privados. El ejemplo más típico es un *setter*, pero no es el único caso. Existen métodos modificadores que no se llaman “setX(...)" y que realizan cambios internos como ajustar varios atributos simultáneamente, reiniciar estados, incrementar contadores o actualizar estructuras internas. Por tanto, aunque todos los *setters* son métodos modificadores, **no todos los métodos modificadores son setters**. Lo que define a un modificador es que cambie el estado, no su nombre.

Las clases inmutables tienen varias ventajas importantes. En primer lugar, **evitan problemas de concurrencia**, porque si el estado no cambia, varios hilos pueden usar el mismo objeto sin necesidad de sincronización. Esto también las hace más sencillas de analizar y depurar, ya que no hay que buscar dónde o cuándo se modificó un atributo. Además, las invariantes de clase son más fáciles de mantener en objetos inmutables: si los atributos sólo se establecen en el constructor, no existen escenarios en los que el objeto quede en un estado inválido debido a cambios posteriores. Esa estabilidad también favorece la reutilización y evita efectos secundarios inesperados.

En resumen, una clase inmutable ofrece un comportamiento más predecible, seguro y fácil de usar en entornos complejos. Aunque su coste sea crear más objetos en lugar de modificar los existentes, en la práctica este coste suele ser pequeño comparado con los beneficios en claridad, robustez y protección de invariantes. Por ese motivo, muchos lenguajes modernos y librerías estándar emplean ampliamente la inmutabilidad como patrón de diseño recomendado.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No, **no es recomendable incluir métodos “setter” siempre ni como convención**. En la práctica de la programación orientada a objetos moderna, añadir setters “por defecto” suele considerarse una **mala práctica**, porque expone más de lo necesario la representación interna de la clase y debilita la encapsulación. La idea de “tengo atributos → genero getters y setters para todos” es tentadora, pero convierte la clase en una simple estructura de datos sin protección ni control.

La recomendación general es que **solo existan setters cuando realmente hagan falta**, es decir, cuando modificar un atributo sea parte esencial del comportamiento correcto del objeto. Si el objeto no necesita que su estado cambie después de ser creado, entonces no necesita setters. En muchos diseños bien estructurados —especialmente en clases que representan valores, como puntos, fechas o coordenadas— es preferible mantenerlas inmutables y ofrecer métodos que devuelvan nuevos objetos antes que permitir modificaciones arbitrarias.

Además, evitar setters innecesarios **protege las invariantes de clase**. Si los atributos pueden cambiarse en cualquier momento desde fuera, la clase pierde la capacidad de garantizar que su estado siempre sea válido. Al limitar o eliminar setters, la clase controla con firmeza cómo se modifican sus datos internos, verificando condiciones, manteniendo coherencia y evitando estados incorrectos. De esta forma, la interfaz pública expresa solo el comportamiento realmente permitido y no expone operaciones que puedan romper la lógica interna.

En resumen, los setters deben incluirse **solo cuando sean necesarios** desde el punto de vista del diseño y del comportamiento del objeto, no como una convención automática. Evitarlos cuando no se requieren conduce a clases más seguras, fáciles de mantener, coherentes y robustas.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase **`String`** en Java es **inmutable**. Esto significa que, una vez creada una cadena, su contenido no puede modificarse. Cada operación que conceptualmente “cambia” una cadena —como concatenar, reemplazar o cortar— en realidad **crea un nuevo objeto `String`** con el nuevo contenido, dejando intacto el original. Esta decisión de diseño ayuda a mantener la seguridad lógica, evitar efectos laterales y facilitar el uso de cadenas en estructuras como *hash tables*.

Cuando se *concatenan* dos cadenas con el operador `+`, lo que ocurre internamente es que se crea **un nuevo objeto `String`** que contiene el resultado de la concatenación. El contenido de las cadenas originales no se modifica en ningún caso. Esto es cómodo para el programador, pero puede ser poco eficiente si se realizan muchas concatenaciones dentro de un bucle, ya que cada operación implica crear objetos adicionales y copiar datos de forma repetida.

Si es necesario realizar una operación que **construye paso a paso una cadena larga mediante múltiples concatenaciones**, lo recomendable es utilizar clases específicas como **`StringBuilder`** (o `StringBuffer` si se necesita sincronización). Estas clases son **mutables**, por lo que permiten añadir, modificar o eliminar partes del texto sin crear objetos intermedios. Cuando se completa la construcción de la cadena, se puede convertir el resultado final a un `String` mediante el método `toString()`. Esta aproximación reduce significativamente el coste en tiempo y memoria cuando se realizan muchas operaciones de concatenación.

En conjunto, la inmutabilidad de `String` aporta seguridad y claridad al código, pero para situaciones donde la eficiencia en concatenaciones es importante, las alternativas mutables como `StringBuilder` representan una solución más adecuada.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En POO, la comparación entre objetos puede hacerse por **identidad** o por **contenido**, pero depende del lenguaje y del diseño de la clase. En Java, si se compara usando `==`, se compara **la identidad**, es decir, si ambas referencias apuntan al **mismo objeto** en memoria. Esto no tiene en cuenta el contenido interno del objeto; dos objetos distintos pero con el mismo estado darán *false*. Para comparar por **contenido**, cada clase debe definir sus propias reglas, normalmente sobrescribiendo el método `equals`. Esto permite decidir cuándo dos objetos deben considerarse equivalentes según sus atributos internos.

El método **`equals`** en Java es un método heredado de la clase base `Object`. Su propósito es comparar **contenido lógico**, es decir, si dos objetos representan la misma información. Sin embargo, **por defecto**, el método `equals` de `Object` simplemente realiza la misma comparación que `==`: compara identidades, no contenido. Por ello, clases como `String`, `Integer` o las colecciones de Java sobrescriben `equals` para que compare sus valores internos. Si no se sobrescribe, `equals` no comparará atributos, sino únicamente si ambos objetos son exactamente la misma instancia.

En el caso concreto de las cadenas, Java sobrescribe `equals` en la clase `String` para que la comparación sea por **contenido**, no por identidad. Esto significa que dos cadenas con los mismos caracteres en el mismo orden son consideradas iguales aunque procedan de lugares distintos del código o hayan sido construidas en momentos diferentes. Por este motivo, **las cadenas en Java deben compararse siempre usando `equals`**, no con `==`, salvo que se quiera saber explícitamente si ambas referencias apuntan al mismo objeto. Usar `==` para comparar textos es un error común que lleva a resultados inesperados.

En resumen, en POO la comparación puede hacerse según la identidad o según el contenido, pero en Java se reserva `==` para la primera y el método `equals` para la segunda. La implementación por defecto de `equals` no compara contenido, por lo que conviene sobrescribirlo cuando la clase representa un valor lógico. En el caso de las cadenas, la comparación debe hacerse mediante `equals`, ya que ha sido cuidadosamente redefinido para comprobar carácter a carácter.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las **clases *wrapper*** son clases que permiten representar un **tipo primitivo** como un **objeto** en lenguajes orientados a objetos. Su función es envolver (to wrap) un valor primitivo dentro de una estructura de clase, de modo que pueda utilizarse en contextos donde solo se permiten objetos, como colecciones genéricas (`ArrayList`, `HashMap`) o métodos que requieren referencias. En Java, los ejemplos típicos son `Integer`, `Double`, `Boolean`, `Character`, etc., que corresponden a `int`, `double`, `boolean` y `char`. Esta separación existe porque Java no es puramente orientado a objetos y mantiene tipos primitivos por razones de eficiencia.

En Java, convertir un tipo primitivo en su wrapper se llama **autoboxing**, y convertir el objeto wrapper en un tipo primitivo se llama **unboxing**. Ambos procesos pueden hacerse de forma explícita, creando manualmente un objeto wrapper, o de forma automática, dejando que el compilador lo haga al asignar valores o al pasarlos como argumentos. Por ejemplo, al escribir `Integer x = 5;` el compilador realiza autoboxing sin que el programador deba usar `new Integer(5)`. De igual forma, cuando se usa un `Integer` en una expresión aritmética, Java realiza unboxing automáticamente. Aunque puede parecer transparente, estos procesos implican creación de objetos o conversiones, por lo que conviene ser consciente de ellos.

Las clases wrapper ofrecen varias ventajas. Permiten almacenar valores primitivos en estructuras de datos basadas en objetos, utilizar métodos asociados al valor (como `Integer.parseInt`), y trabajar con valores nulos cuando se necesite distinguir entre “no hay dato” y “valor cero”. Además, favorecen la integración con APIs que operan exclusivamente con objetos. Sin embargo, el uso excesivo de wrappers en operaciones intensivas puede tener un coste de rendimiento debido al autoboxing y a la creación de objetos adicionales.

No todos los lenguajes orientados a objetos tienen tipos primitivos separados y, por tanto, no todos necesitan wrappers. Lenguajes como **Python**, **Ruby** o **Scala** tratan todos sus valores como objetos, por lo que no requieren esta distinción. En cambio, lenguajes como **Java**, **C#** o **Kotlin** mantienen tipos primitivos por eficiencia y aportan wrappers para integrarlos en el mundo de los objetos. Así, la necesidad de wrappers depende del diseño del lenguaje y del equilibrio que busque entre rendimiento y pureza orientada a objetos.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un **tipo de dato enumerado** en POO representa un conjunto finito y cerrado de valores posibles. Se utiliza cuando una variable solo puede adoptar uno de varios valores predefinidos y lógicos dentro del dominio del problema, como los días de la semana, los colores básicos o los puntos cardinales. Esta restricción explícita evita errores y proporciona mayor claridad semántica, ya que el propio tipo describe sus valores válidos, en lugar de depender de números o cadenas sueltas sin significado intrínseco.

En **Java**, un tipo enumerado (`enum`) es realmente una **clase especial**. Aunque tiene una sintaxis compacta, el lenguaje genera internamente una clase con instancias únicas e inmutables para cada valor del enumerado. Esto permite que un `enum` tenga métodos, atributos, constructores privados y comportamiento propio, funcionando como un tipo de dato robusto y orientado a objetos. A diferencia de los enumerados de lenguajes más antiguos como C, los de Java poseen identidad, comportamiento y capacidad de encapsulación, lo que los convierte en objetos de primera categoría dentro del sistema de tipos.

Los enumerados de Java ofrecen ventajas importantes en términos de **encapsulación**. Al definir un conjunto cerrado de valores, impiden que el código externo introduzca valores inválidos, eliminando errores comunes asociados a constantes numéricas o cadenas. Además, al tratarse de clases, permiten ocultar detalles internos, añadir métodos que encapsulen comportamiento relacionado y mantener invariantes entre los valores. Todo el uso externo se realiza a través de la interfaz pública bien definida del `enum`, reforzando la idea de que el interior se mantiene controlado y protegido.

En consecuencia, los enumerados en Java combinan la seguridad de tipos con una encapsulación fuerte y una representación clara del dominio del problema. Esto los hace especialmente útiles en diseños orientados a objetos donde se desea expresar modelos precisos y evitar valores no válidos desde el propio tipo de dato. ¿Quieres que prepare un ejemplo completo de `enum` con métodos y encapsulación interna?


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

Un `enum` en Java es una **clase especial** con instancias predefinidas; permite encapsular datos y comportamiento. A continuación se define `Mes` con doce instancias, atributos privados y constructor, incluyendo métodos para conocer sus **días** (con y sin tener en cuenta bisiesto), el **ordinal 1–12** y cuatro predicados estacionales dependientes del **hemisferio**. Para mantener el diseño claro, cada mes guarda su estación en el hemisferio norte y en el sur, y los métodos simplemente comparan contra ese dato interno.

Se incluye un método `dias()` que devuelve los días “normales” (28 para febrero) y un método `dias(boolean esBisiesto)` para contemplar años bisiestos (29 en febrero). El ordinal **1–12** se expone como `ordinalEnAnio()` (nótese que `enum.ordinal()` de Java es 0‑basado, por eso se almacena el orden explícitamente). Los métodos `esDePrimavera/Verano/Otoño/Invierno(boolean esHemisferioNorte)` responden si el mes pertenece a esa estación en el hemisferio indicado.

```java
public enum Mes {
    ENERO(1, 31, Estacion.INVIERNO, Estacion.VERANO),
    FEBRERO(2, 28, Estacion.INVIERNO, Estacion.VERANO),
    MARZO(3, 31, Estacion.PRIMAVERA, Estacion.OTOÑO),
    ABRIL(4, 30, Estacion.PRIMAVERA, Estacion.OTOÑO),
    MAYO(5, 31, Estacion.PRIMAVERA, Estacion.OTOÑO),
    JUNIO(6, 30, Estacion.VERANO, Estacion.INVIERNO),
    JULIO(7, 31, Estacion.VERANO, Estacion.INVIERNO),
    AGOSTO(8, 31, Estacion.VERANO, Estacion.INVIERNO),
    SEPTIEMBRE(9, 30, Estacion.OTOÑO, Estacion.PRIMAVERA),
    OCTUBRE(10, 31, Estacion.OTOÑO, Estacion.PRIMAVERA),
    NOVIEMBRE(11, 30, Estacion.OTOÑO, Estacion.PRIMAVERA),
    DICIEMBRE(12, 31, Estacion.INVIERNO, Estacion.VERANO);

    private final int orden;               // 1..12
    private final int diasNoBisiesto;      // 28 para FEBRERO, resto según mes
    private final Estacion estacionNorte;  // estación en hemisferio norte
    private final Estacion estacionSur;    // estación en hemisferio sur

    Mes(int orden, int diasNoBisiesto, Estacion estacionNorte, Estacion estacionSur) {
        this.orden = orden;
        this.diasNoBisiesto = diasNoBisiesto;
        this.estacionNorte = estacionNorte;
        this.estacionSur = estacionSur;
    }

    public int ordinalEnAnio() {
        return orden;
    }

    public int dias() {
        return diasNoBisiesto;
    }

    public int dias(boolean esBisiesto) {
        return (this == FEBRERO && esBisiesto) ? 29 : diasNoBisiesto;
    }

    public boolean esDePrimavera(boolean esHemisferioNorte) {
        return (esHemisferioNorte ? estacionNorte : estacionSur) == Estacion.PRIMAVERA;
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        return (esHemisferioNorte ? estacionNorte : estacionSur) == Estacion.VERANO;
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        return (esHemisferioNorte ? estacionNorte : estacionSur) == Estacion.OTOÑO;
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        return (esHemisferioNorte ? estacionNorte : estacionSur) == Estacion.INVIERNO;
    }

    private enum Estacion { PRIMAVERA, VERANO, OTOÑO, INVIERNO }
}
```

Si se prefiriese evitar identificadores con `ñ` por cuestiones de codificación en ciertos entornos, bastaría con renombrar internamente la `enum Estacion` (p. ej., `OTONIO`) sin cambiar las firmas públicas pedidas. ¿Se desea además añadir un método utilitario `esBisiesto(int anio)` para computar bisiestos desde el `enum`?
