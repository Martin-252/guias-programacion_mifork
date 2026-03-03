<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
En C no hay excepciones, así que el patrón habitual consiste en separar **cálculo** y **comunicación del error**. Una primera opción clásica es devolver un **código de estado** (0 = OK, distinto de 0 = error) y pasar el resultado por **parámetro de salida**. De este modo, la función `raiz` no imprime nada: solo valida la entrada, calcula cuando procede y retorna el estado. Quien llama decide cómo informar al usuario (p. ej., mostrando un mensaje). Esta solución es sencilla, explícita y no depende de variables globales.

```c
#include <stdio.h>
#include <math.h>

int raiz(double x, double *out_result) {
    if (x < 0.0) {
        return -1;                 // Código de error (por ejemplo, -1 = argumento inválido)
    }
    *out_result = sqrt(x);
    return 0;                      // 0 = éxito
}

int main(void) {
    double x = -9.0;
    double r = 0.0;

    int status = raiz(x, &r);
    if (status != 0) {
        // Se informa desde fuera de la función
        printf("Error: la raíz solo admite números no negativos (x = %.2f)\n", x);
    } else {
        printf("raiz(%.2f) = %.5f\n", x, r);
    }
    return 0;
}
```

Una segunda opción es aprovechar el convenio de C para **errores de la biblioteca matemática**: usar `errno` (por ejemplo, `EDOM` para argumento fuera de dominio) y devolver un **valor especial** como `NAN`. La función establece `errno` y retorna `NAN` en caso de error; el llamador pone `errno = 0` antes de la llamada y, después, comprueba `errno` (y si se desea, `isnan`) para decidir si informar al usuario. Esta opción encaja bien con código que ya sigue el estilo de `<math.h>`.

```c
#include <stdio.h>
#include <math.h>
#include <errno.h>

double raiz(double x) {
    if (x < 0.0) {
        errno = EDOM;   // Fuera de dominio
        return NAN;     // Valor indicativo de error en magnitud
    }
    return sqrt(x);
}

int main(void) {
    double x = -9.0;

    errno = 0;                 // Limpiar errno antes de la llamada
    double r = raiz(x);

    if (errno == EDOM) {
        // Se informa desde fuera de la función
        printf("Error: argumento fuera de dominio para raiz (x = %.2f)\n", x);
    } else {
        printf("raiz(%.2f) = %.5f\n", x, r);
    }
    return 0;
}
```

**Clase teoría**
Algunas de las formas de "interpretar una excepcion" en C es devolviendo un valor especial como el -1, o con un parametro adicional para almacenar el código de error.



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una **excepción** es un mecanismo que permite a un programa **interrumpir el flujo normal de ejecución** cuando aparece una situación anómala o inesperada, como valores fuera de rango, errores de entrada/salida o fallos lógicos. En lugar de continuar ejecutando instrucciones que podrían provocar resultados incorrectos, una excepción permite detectar el problema de forma estructurada y trasladarlo a otro lugar del programa para que sea manejado adecuadamente.

El propósito principal de usar excepciones al **implementar funciones** es separar el código que realiza el trabajo normal del código que trata con errores. De esta manera, la función puede centrarse únicamente en su comportamiento lógico, delegando el manejo de problemas en quien la llama. Esto permite diseñar funciones más limpias, más fáciles de leer y más coherentes, especialmente cuando los errores no pueden resolverse dentro de la propia función.

Cuando un programador **llama** a funciones que pueden lanzar excepciones, su objetivo es capturar y gestionar esos fallos de forma clara, evitando que el programa complete acciones incorrectas o termine abruptamente. Además, el manejo de excepciones permite decidir qué hacer ante cada tipo de problema: desde mostrar un mensaje al usuario hasta intentar una recuperación o registrar el error.

En conjunto, las excepciones proporcionan un modo formal, flexible y seguro de comunicar errores que no se puede resolver en el punto donde ocurren. Permiten mantener separado el flujo normal del flujo de error y facilitan que distintos niveles del programa colaboren en la detección y resolución de fallos.


**Clase teoría**
Exepción -> surge en situaciones atipicas y esto nos sirve:

Cuando implementamos una función nos permite indicar más claramente el error.

Cuando llamamos, me facilita separar la lógica normal de la reacción o manejo de la situación erronea.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
Una manera idiomática en Java es que el método **lance una excepción** cuando el argumento no cumple el dominio, y que sea el **código llamador** quien decida cómo informar. Así se separan cálculo y notificación del error: la clase `Calculadora` se limita a validar y calcular, mientras que `main` controla el flujo y presenta el mensaje al usuario. Para un argumento negativo, resulta apropiado `IllegalArgumentException`.

```java
public class Calculadora {

    /**
     * Devuelve la raíz cuadrada de x si x >= 0.
     * Lanza IllegalArgumentException si x es negativo.
     */
    public static double raiz(double x) {
        if (x < 0.0) {
            throw new IllegalArgumentException("La raíz solo admite números no negativos: x=" + x);
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        double x1 = 9.0;
        double x2 = -9.0;

        // Caso correcto
        try {
            double r1 = Calculadora.raiz(x1);
            System.out.printf("raiz(%.2f) = %.5f%n", x1, r1);
        } catch (IllegalArgumentException e) {
            // El control y la comunicación del error se hacen desde fuera del método
            System.out.println("Error al calcular raiz: " + e.getMessage());
        }

        // Caso que provoca excepción
        try {
            double r2 = Calculadora.raiz(x2);
            System.out.printf("raiz(%.2f) = %.5f%n", x2, r2);
        } catch (IllegalArgumentException e) {
            // Aquí se decide cómo informar (log, mensaje al usuario, etc.)
            System.out.println("Error al calcular raiz: " + e.getMessage());
        }
    }
}
```

Este diseño permite que `Calculadora.raiz` exprese claramente su **contrato**: para `x < 0` se lanza una excepción y no se retorna resultado. El método `main` muestra cómo capturar la excepción con `try/catch` y responsabilizarse de la comunicación (por ejemplo, mostrando un mensaje). La ventaja respecto a códigos de error es que el flujo “normal” (cálculo y uso del resultado) queda limpio, y los caminos de error quedan concentrados en bloques bien delimitados.



## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

“**Lanzar**” una excepción consiste en indicar explícitamente que se ha producido una situación anómala y que el flujo normal de ejecución no puede continuar en ese punto. En Java se hace con la instrucción `throw`. Cuando se lanza una excepción, el método deja de ejecutarse inmediatamente y no retorna ningún valor normal. Esto permite separar la lógica correcta de la lógica de error, de modo que una función como `raiz` no tenga que decidir cómo informar: solo declara la anomalía y delega la responsabilidad en quien la invoque.

“**Controlar**” o **“capturar”** una excepción significa envolver el código que puede fallar en un bloque `try` y proporcionar uno o varios bloques `catch` para interceptar excepciones concretas. En estos bloques se decide qué hacer: informar al usuario, registrar el error, recuperar el estado, etc. Controlar no es obligatorio; si una función no captura la excepción, simplemente la deja pasar hacia arriba. Esa capacidad de dejar que otra parte del programa gestione el error es una ventaja fundamental del sistema de excepciones.

La **“propagación”** de una excepción ocurre cuando ningún método en la cadena de llamadas la captura. Tras lanzarse, Java busca un bloque `catch` adecuado en el método actual; si no existe, el método **termina abruptamente**, se destruyen sus variables locales y la excepción se envía al método que lo llamó. Este proceso se repite hacia arriba por la **pila de llamadas**, haciendo que cada función que no capture la excepción termine de inmediato sin ejecutar el resto de su código. Ninguno de estos métodos se reanuda después: una vez interrumpidos por la propagación, no continúan en el punto donde quedaron, solo pueden terminar o dejar paso a otro `catch` en niveles superiores.

Aplicándolo al ejemplo de `Calculadora.raiz`, si `raiz(x2)` recibe un número negativo, ejecuta `throw new IllegalArgumentException(...)`. El método abandona su ejecución al instante y la excepción sube al `main`. En `main`, el primer bloque `try` no sufre problemas, pero el segundo provoca que la ejecución salte directamente al bloque `catch (IllegalArgumentException e)`. Desde el punto donde se lanza la excepción dentro de `raiz`, el resto del método no se ejecuta, y tampoco se ejecutaría el código restante dentro del `try` de `main` si dicho código existiera después de la llamada. El método que no captura la excepción no vuelve a retomarse; únicamente “cede el control” al manejador que la capture o, en ausencia de tal manejador, el programa finaliza con un error no controlado.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
La **propagación natural de excepciones** ofrece una ventaja fundamental respecto a C: elimina la necesidad de comprobar manualmente códigos de error en cada llamada intermedia. En C, si una función detecta un problema, lo habitual es devolver un valor especial o un código de estado, y cada función que esté por encima en la cadena de llamadas debe comprobar explícitamente ese código y retransmitirlo si no puede resolverlo. Esto llena el programa de comprobaciones repetitivas y obliga a que todos los niveles conozcan detalles del error, incluso cuando no tienen capacidad para manejarlo.

Con excepciones, en cambio, un método puede limitarse a **lanzar** la anomalía y dejar que el runtime de Java busque automáticamente un manejador adecuado. La propagación recorre la pila sin necesidad de escribir ningún código adicional en las funciones intermedias. Esto permite que funciones que solo colaboran en el cálculo —pero que no tienen sentido para informar o decidir qué hacer ante un error— permanezcan limpias y centradas en su tarea, dejando el manejo a un nivel superior que sí tiene contexto para reaccionar correctamente.

Otra ventaja es que la propagación asegura que, cuando se produce un error, las funciones que no lo controlan **no continúan en un estado inconsistente**. En C, si el programador olvida comprobar un código de error o lo comprueba mal, el programa puede seguir ejecutándose con datos incorrectos, lo que hace más difícil detectar fallos. Con excepciones, la interrupción inmediata de la ejecución evita estos problemas silenciosos, ya que el flujo normal solo continúa si no ha ocurrido ninguna anomalía.

Finalmente, la separación entre flujo normal y flujo de error conduce a programas más legibles y más fáciles de mantener. La lógica de éxito se expresa sin ruido, mientras que la lógica de error se concentra en puntos claros (`try/catch`). En conjunto, la propagación automática reduce duplicación, evita errores por omisión de comprobaciones y facilita estructurar el programa de forma más ordenada que en el enfoque tradicional de C.



## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
En la mayoría de lenguajes orientados a objetos, como Java, las excepciones **son objetos** que derivan de una jerarquía común (por ejemplo, `Throwable`). Esto significa que encapsulan información relevante sobre el error: el tipo de problema, un mensaje descriptivo e incluso la traza de la pila. En lugar de limitarse a un simple código numérico, permiten representar el fallo como una entidad estructurada, con datos y comportamiento asociados, lo que encaja naturalmente en el paradigma orientado a objetos.

En términos de **encapsulación**, esta estrategia ofrece la ventaja de agrupar todos los detalles del error dentro del propio objeto‑excepción. Así, los métodos que lanzan la excepción no necesitan exponer internamente cómo detectan o representan el fallo; basta con lanzar un tipo concreto. A su vez, el código que captura la excepción puede acceder únicamente a la información que realmente necesita (mensaje, causa interna, etc.) sin depender del estado interno del método que la generó. Esto reduce el acoplamiento y permite interfaces más limpias.

Además, el hecho de que una excepción sea un objeto hace posible **crear excepciones personalizadas** simplemente definiendo nuevas clases que hereden de `Exception` o de otra clase de la jerarquía. Este mecanismo es útil cuando se quiere clasificar con más precisión el tipo de error o distinguir semánticamente diferentes categorías dentro de un mismo dominio de aplicación. Por ejemplo, se podrían definir excepciones como `ExcepcionEntradaInvalida` o `ExcepcionCalculoMatematico`, cada una con su propio propósito.

En conjunto, tratar las excepciones como objetos favorece claridad, extensibilidad y un estilo de programación más modular. Permite diseñar un sistema de errores que represente fielmente los posibles problemas de la aplicación y que pueda ampliarse conforme crezca el programa, manteniendo siempre el control encapsulado y consistente.

**Clase teoría**
En java las execpciones si que suelen ser objetos.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Una excepción en Java llega a un manejador acompañada de **información esencial que queda encapsulada dentro del propio objeto**, algo que no existe de forma tan completa en C. El elemento más útil es la **traza de la pila** (*stack trace*), que recoge la secuencia exacta de llamadas que condujeron al error. Esta información permite localizar con precisión dónde ocurrió el problema y en qué contexto, algo especialmente valioso en programas grandes o con muchas capas de funciones.

Además, cada excepción incluye un **mensaje descriptivo** que explica la causa del fallo, así como una posible **causa anidada** (*cause*) si la excepción fue desencadenada por otra. Esta capacidad de transportar contexto detallado evita depender de códigos numéricos ambiguos o convenciones externas, como ocurre en C. El desarrollador que captura la excepción puede distinguir no solo el tipo de error, sino también su origen y la información que el método que la lanzó decidió considerar relevante.

Finalmente, el propio **tipo concreto** del objeto excepción ya actúa como información esencial: permite diferenciar con claridad categorías de errores mediante la jerarquía de clases (`IllegalArgumentException`, `ArithmeticException`, etc.). Esto simplifica los manejadores y permite capturar únicamente aquellas situaciones que realmente interesan. En conjunto, disponer de tipo, mensaje y traza encapsulados dentro de la excepción proporciona una capacidad de diagnóstico muy superior a la disponible en C.

**Clase teoría**
a) Un mensaje (getMessage).
b) La traza de la pila (getStackTrace, printstackTrace).
c)Opcionalmente las "causa", es otra execpción que es la verdadera causa.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

