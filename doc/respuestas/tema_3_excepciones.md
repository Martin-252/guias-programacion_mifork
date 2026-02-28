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




## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una **excepción** es un mecanismo que permite a un programa **interrumpir el flujo normal de ejecución** cuando aparece una situación anómala o inesperada, como valores fuera de rango, errores de entrada/salida o fallos lógicos. En lugar de continuar ejecutando instrucciones que podrían provocar resultados incorrectos, una excepción permite detectar el problema de forma estructurada y trasladarlo a otro lugar del programa para que sea manejado adecuadamente.

El propósito principal de usar excepciones al **implementar funciones** es separar el código que realiza el trabajo normal del código que trata con errores. De esta manera, la función puede centrarse únicamente en su comportamiento lógico, delegando el manejo de problemas en quien la llama. Esto permite diseñar funciones más limpias, más fáciles de leer y más coherentes, especialmente cuando los errores no pueden resolverse dentro de la propia función.

Cuando un programador **llama** a funciones que pueden lanzar excepciones, su objetivo es capturar y gestionar esos fallos de forma clara, evitando que el programa complete acciones incorrectas o termine abruptamente. Además, el manejo de excepciones permite decidir qué hacer ante cada tipo de problema: desde mostrar un mensaje al usuario hasta intentar una recuperación o registrar el error.

En conjunto, las excepciones proporcionan un modo formal, flexible y seguro de comunicar errores que no se puede resolver en el punto donde ocurren. Permiten mantener separado el flujo normal del flujo de error y facilitan que distintos niveles del programa colaboren en la detección y resolución de fallos.



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


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta


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

