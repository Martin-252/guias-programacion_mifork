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


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
