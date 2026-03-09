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
Tres elementos que solemos ver cuando se lanza una execpción
a) Un mensaje (getMessage), que se mostra por pantalla.
b) La traza de la pila (getStackTrace, printstackTrace), traza de llamadas de porque o como se produjo el throw. Esto vale para depurar.
c)Opcionalmente, una "causa" (Pregunta 17) 



## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Sí, en Java se pueden incluir **varios bloques `catch`** después de un mismo bloque `try`. Cada bloque se asocia a un tipo distinto de excepción, lo que permite reaccionar de forma diferente según la naturaleza del error. Esta estructura facilita un manejo más fino y expresivo, ya que cada `catch` actúa como un filtro que solo captura el tipo de excepción indicado (o sus subtipos).

Aunque pueda haber **varios bloques `catch`**, **solo se ejecuta uno**. En cuanto se lanza una excepción, la JVM busca el primer `catch` cuyo tipo sea compatible con el de la excepción lanzada, ejecuta ese bloque y **omita el resto**. Después, la ejecución continúa normalmente tras el último `catch`, o en el bloque `finally` si existe. Es importante escribir los `catch` desde los tipos más específicos a los más generales para evitar que un tipo general capture la excepción antes de llegar al específico.

**Clase teoría**
Sí se puede tener mas de uno.
Se va comprobando por orden hasta el primero que encaje, sólo se ejecuta uno, y se *deben* poner del más especifico al más general, porque sino, los catch para excepciones especificas no se ejecutaran.
```java
try{
    // resto del código
}catch(TipoExcepcion1){ //Aqui debería ser el AccessDeniedException especifico
}catch(TipoExccepcion2){//Aquie el IOException más general

}
```

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
Sí, aunque una excepción rompa el flujo normal, Java garantiza la ejecución del bloque `finally` siempre que se haya alcanzado el `try` (salvo casos extremos como `System.exit`, muerte del proceso, o fallo de la JVM). El patrón consiste en colocar en `finally` el **código de limpieza**: cierre de ficheros/streams, liberación de sockets, desbloqueo de recursos, etc. Este bloque se ejecuta tanto si **no hubo error**, como si hubo y se **capturó**, e incluso si la excepción **sigue propagándose** porque no se capturó en ese nivel.

Con **`try-catch-finally`**, se captura la excepción (para registrar, informar, o transformar) y, en cualquier caso, se limpian los recursos en `finally` antes de continuar:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class DemoCatchFinally {
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("datos.txt"));
            String linea = br.readLine();
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            // Manejo local del error (log, mensaje, etc.)
            System.err.println("No se pudo leer el fichero: " + e.getMessage());
        } finally {
            // Se ejecuta SIEMPRE si se entró al try: haya o no excepción
            if (br != null) {
                try { br.close(); } 
                catch (IOException cierre) { System.err.println("Error al cerrar: " + cierre.getMessage()); }
            }
        }
    }
}
```

Con **`try-finally` sin `catch`**, no se captura la excepción en este nivel: se deja **propagar** a quien llame, pero **antes** se ejecuta `finally` para liberar los recursos. Es útil cuando el método no decide sobre el error, pero sí es responsable de la limpieza:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class DemoTryFinally {
    // Declara que puede lanzar IOException: la excepción se propagará
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta));
            return br.readLine();            // Si aquí salta IOException, se ejecutará finally antes de salir
        } finally {
            // Garantía de liberación aunque la excepción se propague
            if (br != null) {
                try { br.close(); } 
                catch (IOException cierre) { System.err.println("Error al cerrar: " + cierre.getMessage()); }
            }
        }
    }

    public static void main(String[] args) {
        try {
            String linea = leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            // Aquí se maneja la excepción propagada
            System.err.println("Fallo leyendo: " + e.getMessage());
        }
    }
}
```

> Nota práctica: en Java moderno, cuando el recurso implementa `AutoCloseable`, conviene usar **try‑with‑resources** (`try (BufferedReader br = ...) { ... }`) que cierra automáticamente sin necesidad de `finally`. Sin embargo, el concepto clave solicitado aquí es que `finally` actúa como **última barrera de limpieza**: se ejecuta siempre que se haya entrado al `try`, tanto si hay `catch` como si se deja la excepción en propagación.

**Clase toería**
Se garantiza mediante un bloque *finally* el cual es un bloque especial que se ejecuta *SIEMPRE* que se entre en el bloque *try*, y hace que se ejecuten todas las líneas que estan ese bloque.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí, en Java puede usarse **`try` con `finally` sin `catch`**. Ese patrón (`try { ... } finally { ... }`) es útil cuando el método **no maneja** la excepción en ese nivel (la deja propagar con `throws`), pero **sí debe garantizar** la liberación de recursos o la restauración de invariantes. El bloque `finally` se ejecuta siempre que se haya **entrado** en el `try`, tanto si no ocurre excepción como si ocurre y se propaga, o si se captura en otro nivel. Las únicas excepciones prácticas son situaciones anómalas como `System.exit(...)`, fallo de la JVM, apagado del proceso o si el propio `finally` lanza otra excepción que interrumpe su ejecución.

Además, **`finally` se ejecuta incluso si hay `return` dentro de `try` (o de `catch`)**. El flujo es: se evalúa el `return`, se ejecuta `finally`, y **después** se completa el retorno. Por ello, poner `return` en `finally` es una mala práctica: **anula** la excepción que estaba propagándose o **sobrescribe** el valor que se iba a devolver, dificultando el razonamiento del programa. También conviene evitar que `finally` lance nuevas excepciones; si algo puede fallar allí (p. ej., `close()`), suele envolverse en un `try/catch` local para registrar el problema sin ocultar el error original.

```java
// try-finally SIN catch: la excepción (si ocurre) se propaga, pero antes se limpia
public static String leerPrimeraLinea(Path ruta) throws IOException {
    BufferedReader br = Files.newBufferedReader(ruta);
    try {
        return br.readLine();            // Aunque haya return aquí, se ejecutará finally antes de devolver
    } finally {
        try { br.close(); } catch (IOException e) { /* registrar si procede */ }
    }
}

// Demostración de que finally se ejecuta con return en try
public static int demo() {
    try {
        return 1;                        // Valor preparado para devolver
    } finally {
        System.out.println("Siempre paso por finally");
        // ¡Evitar return aquí! Sobrescribiría el 1 y complica el control de flujo.
    }
}
```
**Clase toería**
Si el bloque *finally* se ejecuta sin el catch, puesto que es catch. Si hubo excepción, como no hay catch, la excepción se propaga.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java, las **excepciones controladas** (*checked exceptions*) son aquellas que **el compilador obliga a declarar o capturar**. Si un método puede producir una de estas excepciones, debe incluirla en su firma con `throws`, y quien llame debe rodearla con `try‑catch` o volver a declararla. Representan situaciones que *razonablemente pueden esperarse* y que el programador debería prever. Por el contrario, las **excepciones no controladas** (*unchecked exceptions*) no requieren ser declaradas ni capturadas: heredan de `RuntimeException` y normalmente indican errores de programación, condiciones lógicas incorrectas o fallos que no tiene sentido forzar a manejar en cada llamada.

`RuntimeException` es, por tanto, la **raíz de todas las excepciones no controladas**. Su papel es separar los errores que se deben manejar explícitamente de los errores debidos a fallos en el código (como accesos fuera de rango, nulos inesperados o precondiciones violadas). Cualquier excepción que herede de `RuntimeException` se considera un problema que debe arreglarse mediante pruebas y corrección de código, no mediante un manejo obligatorio. Por ello, el compilador no exige declarar ni capturar estas excepciones, aunque pueden manejarse si se desea.

Entre las excepciones **controladas** habituales están `IOException`, `SQLException` o `ClassNotFoundException`, que representan fallos externos al programa. Nosotros mismos podríamos crear una clase `DatosInvalidosException extends Exception` para modelar un error de validación externa. Entre las **no controladas** típicas aparecen `NullPointerException`, `IllegalArgumentException`, `IndexOutOfBoundsException` o `ArithmeticException`. De forma similar, podríamos definir `ParametroVacioException extends RuntimeException` para señalar un error lógico del programador.

**Situaciones típicas donde se prefiere una excepción *controlada*:**

1.  **Errores de entrada/salida**: apertura de ficheros, lectura de red, fallos en sockets.
2.  **Errores externos y recuperables**: servidor inaccesible, permisos insuficientes.
3.  **Operaciones que dependen del entorno**: carga dinámica de clases, acceso a recursos externos.
4.  **Validaciones obligatorias que deben obligar al llamador a reaccionar**: datos de usuario incorrectos.

**Situaciones típicas donde se prefiere una excepción *no controlada*:**

1.  **Fallos de programación**: nulos inesperados, parámetros inválidos (uso de `IllegalArgumentException`).
2.  **Precondiciones violadas**: estados incorrectos en objetos (`IllegalStateException`).
3.  **Errores lógicos irrecuperables**: índices fuera de rango, desbordamientos aritméticos.
4.  **Comprobaciones que serían ruido si se obligara a tratarlas en cada llamada**: errores que deben corregirse, no manejarse.

En conjunto, la distinción permite decidir si un error forma parte del *contrato* de un método (controlado) o si es un indicio de un fallo del propio código (no controlado). Esto permite escribir APIs más claras y programas más robustos.

**Clase teoría**
Hay dos ramas de Exceptions:
*RunTimeException*: `IllegalArgumentException, NullPointerExecption, ArrayIndexOutOfBoundsException`. Estas se conocen en java como las no contraladas, es decir, no estás obligado a poner bloque ``try catch o throws``. Estas excepciones saltan por problemas de programación.

*IOException*: `AccesDeniedException` y otras... . Estas son las si controladas, que al cotrario que las otras si que te que obligan a poner bloque ``try catch o throws``. Estos saltan cuando hay errores que el programador no puede solucionar como el internet,

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
En Java, la palabra clave **`throws`** se utiliza en la *declaración* de un método para indicar que dicho método **puede lanzar una o varias excepciones controladas** (*checked exceptions*) y que **no las maneja internamente**. Con esto, el método transfiere la responsabilidad de manejar esas excepciones a quien lo llame. De este modo, la firma del método pasa a formar parte explícita del “contrato”: informa al programador de que debe prever y tratar esos posibles fallos.

El uso de `throws` es una **alternativa a capturar** una excepción controlada (*checked*) porque el compilador exige que tales excepciones se traten *de alguna manera*. Las dos opciones posibles son:

1.  **Capturarla** en un bloque `try-catch`, gestionando ahí el error.
2.  **Declararla con `throws`** y dejar que se *propague* a niveles superiores.

Si un método elige no capturarla, entonces **debe declararla con `throws`** para que el código que lo llame sea consciente de que esa excepción puede producirse y tome una decisión: manejarla, volverla a propagar o transformarla en otra. Este mecanismo obliga a diseñar APIs claras donde las excepciones previstas están documentadas en la propia firma, lo que mejora la robustez y la legibilidad del código.

Ejemplo breve:

```java
public static String leerLinea(String ruta) throws IOException {
    // No se captura IOException aquí: se deja propagar
    return Files.readAllLines(Path.of(ruta)).get(0);
}

public static void main(String[] args) {
    try {
        System.out.println(leerLinea("datos.txt"));
    } catch (IOException e) {
        System.err.println("Error leyendo el fichero: " + e.getMessage());
    }
}
```

En conjunto, `throws` se usa para **delegar** el tratamiento de excepciones controladas y para especificar formalmente qué errores puede provocar un método, permitiendo así un manejo coherente y estructurado en niveles superiores del programa.




## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
Una forma típica es declarar en la **firma** del método que puede producirse una `IOException` y que **no se maneja localmente**, de modo que la excepción se **propague** al llamador. Aun así, el método debe ser responsable de **liberar recursos**; para ello se usa `finally`, que se ejecuta tanto si hay retorno normal como si salta la excepción. En el ejemplo, se abre un fichero y se devuelve su primera línea; si el fichero no existe (o hay otro problema de E/S), **no** se captura ahí: la excepción sube gracias a `throws`.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Ficheros {

    // Firma que declara la propagación: no se maneja IOException aquí
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta));
            return br.readLine();             // Si aquí falla, la IOException se propagará
        } finally {
            // Limpieza garantizada: se ejecuta haya o no excepción o return
            if (br != null) {
                try { br.close(); }
                catch (IOException cierre) {
                    // Registrar si procede; evitar eclipsar la excepción original
                    System.err.println("Error al cerrar: " + cierre.getMessage());
                }
            }
        }
    }

    // Ejemplo de uso: aquí sí se decide cómo manejar la excepción propagada
    public static void main(String[] args) {
        try {
            String linea = leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            System.err.println("No se pudo abrir/leer el fichero: " + e.getMessage());
        }
    }
}
```

Este patrón separa responsabilidades: el método que **abre** el recurso se asegura de **cerrarlo** pase lo que pase, pero **no decide** cómo reaccionar ante la ausencia del fichero; esa decisión queda en manos del nivel superior (quien llama). Alternativamente, en Java moderno podría preferirse *try‑with‑resources* para evitar el `finally` manual, pero para el objetivo pedido —firma con `throws` y garantía de liberación— el `try` + `finally` es suficiente y explícito.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Sí, en Java **puede declararse** en la cláusula `throws` una excepción **no controlada** (por ejemplo, `RuntimeException` o cualquiera de sus subclases), pero **no es obligatorio**. Al ser *unchecked*, el compilador **no exige** que quien llama la capture ni que la declare a su vez. Por tanto, añadir `throws RuntimeException` en la firma **no cambia** las obligaciones del llamador frente al compilador; su efecto es principalmente **documental**: comunica de forma explícita que el método puede fallar por una condición de programación o de precondición violada.

El método llamador **no tiene por qué** poner `try-catch` por el hecho de que se declare una *unchecked* en `throws`. Solo debería capturarla si **tiene una estrategia significativa** para recuperarse o registrar el problema de forma útil. En muchos casos, una `RuntimeException` representa un **error de programación** (parámetros inválidos, estado ilegal, índice fuera de rango), y lo correcto es **no capturarla** localmente, dejar que se propague y **corregir el bug** o reforzar las precondiciones. Capturar *unchecked* “por sistema” suele ocultar fallos y dificulta su detección.

¿Entonces qué sentido tiene declararlas? Principalmente, **documentación de contrato** y **coherencia en APIs**. Declarar (o al menos documentar en Javadoc con `@throws`) que un método puede lanzar, por ejemplo, `IllegalArgumentException` ayuda a que quien llama entienda las **precondiciones**. También hay escenarios donde conviene capturar *unchecked* en el borde de la aplicación (p. ej., en una capa de presentación o un “filtro global”) para **unificar el tratamiento** de errores y devolver respuestas coherentes sin volcar trazas a usuario. Otro caso de uso es el **encapsulamiento/reenvoltorio**: capturar una excepción (checked o unchecked) y re-lanzar una `RuntimeException` del dominio que **conserve la causa** (`throw new MiRuntime(e)`), evitando contaminar todas las firmas con *checked* en capas que no deben conocerlas.

Ejemplo ilustrativo (se declara una *unchecked* solo como contrato explícito, sin obligación para el llamador):

```java
public class Validador {
    /**
     * @throws IllegalArgumentException si texto es nulo o vacío
     */
    public static void exigeNoVacio(String texto) throws IllegalArgumentException {
        if (texto == null || texto.isBlank()) {
            throw new IllegalArgumentException("Se requiere un texto no vacío");
        }
    }
}

public class Uso {
    public static void main(String[] args) {
        // No es obligatorio usar try-catch; si falla, es un error de uso que debe corregirse
        Validador.exigeNoVacio("");  // lanzará IllegalArgumentException
    }
}
```

En resumen: **sí** se puede poner *unchecked* en `throws`, **no** obliga al llamador a capturarla, y su valor está en **comunicar** precondiciones o políticas de error. Conviene usarlo con mesura; a menudo basta con **documentar en Javadoc** y lanzar la *unchecked* adecuada (`IllegalArgumentException`, `IllegalStateException`, etc.), reservando `try-catch` para los puntos donde realmente tenga sentido **manejar o traducir** el fallo.

**Clase teoría**
Por poder podemos pero el compilador no va a obligar al bloque `try catch`, esto no es habitual, pero a veces se pone por documentación.
Poner un throws en una excepción controlada que no tenía throws hace que no haya compatibilidad sin el ``try catch``.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta`
Las **excepciones controladas** (*checked*) se recomiendan cuando el error forma parte del **contrato normal** del método y el programador llamador **debe** reaccionar. Suelen representar fallos externos y previsibles: problemas de entrada/salida, errores de red, recursos no disponibles o permisos insuficientes. La idea es que el método *no puede garantizar* que la operación funcione y la API quiere obligar a quien llama a decidir qué hacer. Por eso `IOException`, `SQLException` o `ClassNotFoundException` son controladas: son fallos esperables que deben manejarse de forma explícita para evitar comportamientos incorrectos.

Las **excepciones no controladas** (*unchecked*), como `IllegalArgumentException` o `IllegalStateException`, se usan cuando el error se debe a un **mal uso de la API** o a una **precondición violada**. En estos casos no se pretende obligar a capturar nada, porque el problema debe corregirse en el código, no manejarse en tiempo de ejecución. Errores como pasar un índice fuera de rango, un argumento nulo inesperado o invocar un método en un estado inválido no deberían requerir un `try-catch` constante. Por eso se dejan como *unchecked*: si ocurren, es preferible que la excepción se propague y revele un fallo lógico.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. De hecho, **Java es una excepción histórica**: la mayoría de lenguajes modernos (C++, Python, C#, JavaScript, Go —aunque Go usa otro mecanismo—) **solo tienen excepciones no controladas**, o directamente no obligan a declararlas. En estos lenguajes, las excepciones se tratan siempre como *unchecked*, y se considera responsabilidad del programador decidir dónde capturarlas. La práctica común es usar siempre excepciones no controladas como mecanismo principal, ya que evita firmas de métodos demasiado pesadas y reduce el “ruido” de capturas obligatorias.

En los lenguajes donde solo existe una opción, la opción habitual es la de las **excepciones no controladas**, porque encaja mejor con flujos de ejecución flexibles y APIs más limpias. La filosofía predominante es que las excepciones representan situaciones anómalas, no rutas normales del programa, y por tanto no deben requerir comprobaciones obligatorias en cada llamada. En ese sentido, el enfoque mayoritario en la industria se acerca más al modelo de las excepciones *unchecked* que al diseño dual de Java.


**Clase teoría**
Se recomienda usar las excepciones controladas cuando son problemas que nosostros no podemos arreglar, y las controladas cuando el problema es de programación.

No en todos los leguajes existen estas opciones, la mayoria de lenguajes suelen ir solo con las no controladas. 


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, **tiene sentido lanzar excepciones dentro de un `catch`** cuando el manejador detecta que **no puede resolver completamente el problema** o cuando desea **traducir** una excepción baja a otra más apropiada para la capa actual del programa. Lanzar una nueva excepción desde el `catch` permite ajustar el tipo, el mensaje y el contexto del error, evitando que capas superiores tengan que conocer detalles internos de otras capas.

También es posible **relanzar la misma excepción capturada** usando simplemente `throw;` o `throw e;` (equivalentes en Java). Esto conserva la información original del error —tipo, mensaje y traza de pila— y permite que la excepción siga **propagándose** hacia arriba. Esta técnica es útil cuando una capa necesita registrar, limpiar o anotar algo, pero **no es la responsable final** del manejo del error. Así, no se pierde contexto ni se altera la semántica original del fallo.

Un caso típico de lanzar una **nueva excepción dentro del `catch`** es cuando se quiere *ocultar* excepciones de una capa baja convirtiéndolas en excepciones de negocio más significativas. Por ejemplo, se captura una `IOException` en la capa de acceso a datos y se lanza una `DatosNoDisponiblesException` que refleja mejor el contexto de la aplicación, sin exponer tecnicismos al resto del sistema.

```java
try {
    BufferedReader br = new BufferedReader(new FileReader("config.txt"));
    // ...
} catch (IOException e) {
    // Traducción del error
    throw new ConfiguracionException("No se pudo cargar el fichero de configuración", e);
}
```

En cambio, **relanzar la misma excepción capturada** es apropiado cuando la capa actual desea hacer alguna tarea auxiliar —como registrar un mensaje, liberar recursos adicionales o anotar el punto donde ocurrió— pero sin modificar la naturaleza del error. Tras realizar esta acción, se permite que la excepción siga su camino normal.

```java
try {
    procesarDatos();
} catch (IllegalArgumentException e) {
    // Registrar el error, pero dejar que siga fluyendo hacia arriba
    System.err.println("Argumento inválido detectado en procesarDatos(): " + e.getMessage());
    throw e;  // relanzamiento de la misma excepción
}
```

En conjunto, ambas técnicas permiten ajustar el manejo de errores a la arquitectura: **lanzar una nueva excepción** sirve para traducir o encapsular fallos entre capas, mientras que **relanzar la misma** permite añadir información o limpieza sin interferir con la propagación natural. Ambas forman parte de un estilo profesional de diseño de excepciones y ayudan a mantener APIs limpias, capas bien aisladas y diagnósticos más claros.


**Clase teoría**
Sí, si que tiene sentido en algunos casos.
Sí que se puede relanzar la misma excepción, para envolver otra excepcion nueva , por ejemplo para pasar de una controlada a una no controlada (será causa (ver 17)), o para lanzar otra excepcion totalmente nueva.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Cuando se dice que una excepción es la **“causa”** de otra, significa que una excepción de nivel inferior (más técnica) se **encapsula** dentro de otra excepción de nivel superior (más significativa para la capa actual). Este mecanismo permite **no perder la información original**, incluida la traza de la pila, mientras se ofrece al nivel superior un tipo de error más coherente con la lógica de la aplicación. En Java, esto se hace mediante un constructor que recibe un objeto `Throwable` como causa o usando `initCause()`.

Esta práctica es fundamental en diseños por capas: una capa baja (p. ej., acceso a ficheros) puede lanzar una excepción técnica (`IOException`), pero la capa de negocio puede transformar ese error en una excepción más específica de su dominio sin perder la traza original. Así se consigue ocultar detalles internos sin perjudicar la capacidad de diagnóstico cuando sea necesario analizar el problema en profundidad.

Cuando una excepción **tiene una causa** y se imprime con `printStackTrace()` o aparece en consola, Java muestra **ambas pilas**: primero la de la excepción externa (de alto nivel) y después una sección con `Caused by:` seguida de la traza original de la excepción interna. Esto permite reconstruir completamente el origen del fallo.

***

### **Ejemplo en Java: encapsulación con causa**

Supongamos una excepción personalizada de alto nivel:

```java
public class LecturaDatosException extends Exception {
    public LecturaDatosException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}
```

Y ahora un método que captura una excepción de bajo nivel y la encapsula:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ServicioDatos {

    public static String cargarConfiguracion() throws LecturaDatosException {
        try {
            BufferedReader br = new BufferedReader(new FileReader("config.txt"));
            return br.readLine();
        } catch (IOException e) {
            // Encapsulamos la IOException para no exponer detalles del bajo nivel
            throw new LecturaDatosException("Error cargando configuración", e);
        }
    }

    public static void main(String[] args) {
        try {
            System.out.println(cargarConfiguracion());
        } catch (LecturaDatosException e) {
            e.printStackTrace(); // Aquí se verá la excepción externa y, debajo, su causa
        }
    }
}
```

***

### **¿Qué se ve cuando la excepción aparece en pantalla?**

Al ejecutarse `printStackTrace()`, Java muestra algo como:

    LecturaDatosException: Error cargando configuración
        at ServicioDatos.cargarConfiguracion(ServicioDatos.java:10)
        at ServicioDatos.main(ServicioDatos.java:18)
    Caused by: java.io.FileNotFoundException: config.txt (No such file or directory)
        at java.io.FileInputStream.open0(Native Method)
        at java.io.FileInputStream.open(FileInputStream.java:195)
        ...

Es decir:

1.  **La excepción de alto nivel**, con su propio mensaje y traza.
2.  La línea `Caused by:`
3.  **La excepción original**, con toda su traza de bajo nivel.

***

En conjunto, usar la “causa” permite conectar las capas del programa sin perder información, manteniendo encapsulación y diagnosticabilidad al mismo tiempo.


**Clase teoría**
Consiste en capturar una excepción para devolver otra la cual se va a combertir en la causa principal del problema. Se usa tambien para envolver excepciones del lenguaje de programacion en excepciones creadas por el programador.

Causa de excepción:
Se ve cuando la excepción se muestra por pantalla.
"Excepcion externa (NetFluxException)"

Caused by excepcion interna (IOException)
se puede obtener con el metodo getCause()

