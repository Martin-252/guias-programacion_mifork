<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta
A modo de ejemplo de **composición** en C (relación “tiene-un”), puede definirse una estructura `Punto` con dos coordenadas (`x` e `y`) y una estructura `Linea` que **tiene** dos puntos (`a` y `b`). De este modo, `Linea` se construye componiendo dos `Punto`. Además, se pueden incluir funciones auxiliares para calcular la distancia entre dos puntos y la longitud de una línea como la distancia entre sus extremos. Se usará `double` para ganar precisión y `math.h` para `sqrt`.

El siguiente código ilustra la idea. Se define `struct Punto`, `struct Linea`, una función `distancia` entre dos puntos y `longitud_linea` para una línea compuesta por dos puntos. Opcionalmente, se muestra un `main` mínimo para ejemplificar su uso y verificar resultados.

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;  // extremo 1
    Punto b;  // extremo 2
} Linea;

/* Distancia euclídea entre dos puntos */
double distancia(Punto p1, Punto p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx * dx + dy * dy);
}

/* Longitud de una línea como distancia entre sus extremos */
double longitud_linea(Linea l) {
    return distancia(l.a, l.b);
}

/* Ejemplo de uso */
int main(void) {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};      // distancia 5 (triángulo 3-4-5)
    Linea l = {p1, p2};

    printf("Distancia(p1, p2) = %.2f\n", distancia(p1, p2));
    printf("Longitud de la linea = %.2f\n", longitud_linea(l));
    return 0;
}
```

Obsérvese que la **composición** queda reflejada en que `Linea` contiene directamente a `Punto` como campos (no referencias externas), lo que implica pertenencia fuerte: los puntos `a` y `b` “viven” dentro de la línea. Alternativamente, si se prefieren estructuras más flexibles, podría componerse mediante **punteros** a `Punto` (por ejemplo, para compartir puntos entre varias líneas), pero para fines didácticos la composición por valor es más simple y suficiente.



## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
A continuación se muestra el mismo ejemplo trasladado a **orientación a objetos en Java** usando **composición** (“una línea *tiene* dos puntos”) y **ocultación de información** para garantizar **inmutabilidad**. La clase `Punto` encapsula sus coordenadas como `private final` y ofrece un método de instancia `distanciaA(Punto otro)`. La clase `Linea` mantiene dos extremos `private final` y expone un método `longitud()`. No hay *setters* y, si se aceptaran objetos externos, se harían copias defensivas; aquí se crean internamente inmutables, por lo que basta con exponerlos de forma segura.

Se usa `double` para los cálculos (similar a C) y `Math.hypot(dx, dy)` por estabilidad numérica. La inmutabilidad se asegura con campos `final`, ausencia de mutadores y validaciones en el constructor. En caso de querer exponer los puntos, se puede ofrecer métodos de acceso que devuelvan copias (o bien no exponerlos y sólo proporcionar operaciones derivadas, como la longitud). Así, la línea, una vez creada, **no puede cambiar** sus extremos.

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double x() { return x; }
    public double y() { return y; }

    /** Distancia euclídea a otro punto. */
    public double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto destino no puede ser null");
        }
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        // hypot mejora la estabilidad numérica frente a sqrt(dx*dx + dy*dy)
        return Math.hypot(dx, dy);
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

public final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        if (a == null || b == null) {
            throw new IllegalArgumentException("Los puntos de una línea no pueden ser null");
        }
        this.a = a; // Punto es inmutable; no se necesita copia defensiva
        this.b = b;
    }

    /** Longitud de la línea como distancia entre sus extremos. */
    public double longitud() {
        return a.distanciaA(b);
    }

    /** Accesores seguros: devuelven las mismas referencias inmutables. */
    public Punto extremoA() { return a; }
    public Punto extremoB() { return b; }

    @Override
    public String toString() {
        return "Linea[" + a + " -> " + b + "]";
    }
}

// Ejemplo de uso
class Demo {
    public static void main(String[] args) {
        Punto p1 = new Punto(0.0, 0.0);
        Punto p2 = new Punto(3.0, 4.0); // distancia 5
        Linea l = new Linea(p1, p2);

        System.out.println("Distancia p1->p2: " + p1.distanciaA(p2));
        System.out.println("Longitud de la línea: " + l.longitud());
        System.out.println(l);
    }
}
```

En este diseño, `Linea` **compone** dos `Punto`: la vida de los extremos está conceptualmente ligada a la línea que los usa, y al ser `Punto` inmutable, no hay riesgo de que cambien desde fuera. La **encapsulación** evita exponer detalles internos que permitan modificación, y la **inmutabilidad** se obtiene marcando campos `private final`, sin *setters* y con validaciones de entrada. Si en otro contexto `Punto` fuera mutable, sería recomendable **copiar defensivamente** en el constructor y en los *getters* para preservar la inmutabilidad observada de `Linea`.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La **multiplicidad** en composición indica cuántas instancias de una clase “forman parte” de otra dentro de una relación de *tiene-un* (**has‑a**). En composición, además, esa relación implica que la parte no puede existir conceptualmente sin el todo, o al menos que su ciclo de vida está fuertemente ligado al objeto contenedor. La multiplicidad permite describir numéricamente esta estructura: cuántos objetos del tipo “parte” contiene exactamente —o puede contener— un objeto del tipo “todo”.

En el ejemplo de `Linea` y `Punto`, una línea está formada **exactamente por dos puntos**. Esto se expresa como multiplicidad **2** desde `Linea` hacia `Punto`. No es opcional ni variable: toda línea debe tener dos puntos, y siempre serán dos. Por tanto, la multiplicidad en este sentido se representaría como:  
**Linea → Punto : 2**.

Mirando en sentido contrario, cada punto concreto, tal como está modelado en el ejemplo, pertenece **a una única línea**; es decir, no se contempla reutilización de puntos entre líneas ni un punto que exista por sí solo fuera de una línea compuesta. Dado el diseño mostrado, la multiplicidad desde `Punto` hacia `Linea` es **1** (cada punto está contenido exactamente en una línea). Por tanto, queda expresado como:  
**Punto → Linea : 1**.

De este modo, la composición completa puede resumirse, en notación habitual, como:  
**Linea (2) —— (1) Punto**,  
siendo una relación “fuerte” donde los puntos forman parte estricta de la línea y su existencia está conceptualmente ligada a ella.



## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
La distinción entre **composición fuerte** y **composición débil** hace referencia al grado de dependencia que existe entre el *todo* y las *partes* dentro de una relación estructural. En una **composición fuerte**, el objeto compuesto es dueño absoluto de las partes: controla su creación y su destrucción, y dichas partes no tienen sentido fuera del todo. Esto implica que el *ciclo de vida* de las partes está completamente ligado al del todo: cuando el objeto contenedor desaparece, las partes dejan de existir también. Es la forma más estricta de la relación “todo–parte”.

Por el contrario, la **composición débil** describe un escenario donde las partes están asociadas al todo, pero no dependen vitalmente de él. En este caso, el todo puede referenciar objetos que existen por sí mismos, cuyo ciclo de vida no está necesariamente ligado al suyo. Si el todo se destruye, las partes pueden seguir existiendo sin problema. Estas partes pueden incluso estar compartidas entre varios objetos contenedores. Debido a esta menor dependencia, la relación no representa una propiedad estructural del objeto, sino una simple conexión lógica.

En terminología habitual de UML y de diseño orientado a objetos, la **composición débil** se conoce como **asociación o agregación**. En este tipo de relación, el todo contiene o utiliza partes, pero no mantiene la propiedad exclusiva sobre su existencia. Por ejemplo, varias clases podrían compartir un mismo objeto sin que ninguna sea su “dueña” en sentido estricto. Esta forma de relación es útil cuando la estructura del sistema requiere flexibilidad.

En cambio, la **composición fuerte** es la que suele denominarse **composición** propiamente dicha. Representa relaciones rígidas de pertenencia y dependencia vital: las partes forman parte integrante y esencial del objeto compuesto. Esta diferencia guía muchas decisiones de diseño, como si deben hacerse copias defensivas, si los objetos deben ser inmutables o si deben ser compartidos entre múltiples entidades.



## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
En los casos en los que una clase utiliza a otra **solo como parte de la implementación de un método**, no se considera que exista composición, sino **dependencia**. La dependencia es la relación más débil entre clases: indica únicamente que una clase necesita a otra *temporalmente* para llevar a cabo una operación, pero sin formar parte de su estructura interna ni de su estado permanente.

Cuando una clase recibe objetos como parámetros de un método, los devuelve como resultado, los crea con `new` dentro de un método o los usa como variables locales, la relación se limita al ámbito del propio método. Esta relación **no afecta al ciclo de vida** del objeto recibido ni implica pertenencia estructural. Los objetos usados podrían existir antes y después sin relación estable con la clase que los emplea. Por tanto, el vínculo se denomina dependencia, ya que la clase depende de esos tipos únicamente para realizar esa funcionalidad puntual.

En cambio, la composición aparece solo cuando el objeto “todo” **posee** a las partes como atributos propios (campos de instancia) y mantiene su ciclo de vida. Si no hay atributos que almacenen esas instancias como parte del estado permanente del objeto, no hablamos de composición sino, nuevamente, de dependencia. Esta distinción resulta esencial para comprender el diseño orientado a objetos, ya que separa el uso meramente operativo de una clase del hecho de formar parte real de su estructura interna.

Por tanto, al usar objetos como parámetros, retornos, variables temporales o instancias creadas en métodos, la relación correcta es **dependencia**, no composición. Esta clasificación ayuda a mantener un modelo conceptual claro y a distinguir entre lo que una clase *usa* para trabajar y lo que una clase *contiene* como parte esencial de su identidad.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
A efectos didácticos, puede implementarse la relación de dos maneras. **Composición fuerte**: los puntos *solo existen dentro* de la línea y no se exponen; su ciclo de vida queda ligado al de `Linea`. Para ello, se crean internamente (p. ej., con coordenadas primitivas en el constructor de `Linea`) y se encapsulan en una clase interna privada, impidiendo referencias externas. La línea ofrece operaciones (como `longitud()`), pero no devuelve los puntos ni acepta referencias a ellos, con lo que las “partes” dejan de existir cuando desaparece el “todo”.

Por contraste, **composición débil (agregación)**: la línea almacena referencias a objetos `Punto` que pueden existir antes, después y al margen de ella. Dichos puntos pueden compartirse entre varias líneas y, si son mutables, sus cambios externos se reflejan en la línea. El todo no controla el ciclo de vida de las partes (no las crea necesariamente ni las destruye); se limita a *usar* referencias. Esta diferencia impacta en invariantes, encapsulación y (de ser necesario) copias defensivas.

En términos de diseño, se elegirá **composición fuerte** cuando los elementos “parte” sean intrínsecos a la identidad del “todo” y no deban circular de forma independiente (p. ej., puntos *propios* de una línea geométrica inmutable). En cambio, se aplicará **agregación** cuando interese el **reuso/compartición** (p. ej., un punto que sirve como vértice común de varias líneas). Incluso si `Punto` fuera inmutable, al pasarlo por referencia y exponerlo se mantendría una relación débil; para lograr semántica fuerte conviene crear internamente las partes y no exponerlas directamente.

***

### 1) Composición **fuerte** (ciclo de vida de los puntos ligado a `Linea`)

```java
// Los puntos se crean y viven DENTRO de LineaFuerte: no hay referencias externas posibles.
public final class LineaFuerte {
    // Clase interna privada: nadie fuera puede referenciar estos puntos.
    private static final class Punto {
        private final double x;
        private final double y;
        private Punto(double x, double y) { this.x = x; this.y = y; }
        double distanciaA(Punto otro) {
            double dx = otro.x - this.x;
            double dy = otro.y - this.y;
            return Math.hypot(dx, dy);
        }
    }

    private final Punto a;
    private final Punto b;

    // Se reciben coordenadas y se construyen internamente las "partes".
    public LineaFuerte(double ax, double ay, double bx, double by) {
        this.a = new Punto(ax, ay);
        this.b = new Punto(bx, by);
    }

    public double longitud() {
        return a.distanciaA(b);
    }

    @Override
    public String toString() {
        return "LineaFuerte[(a)=(" + a.x + ", " + a.y + "), (b)=(" + b.x + ", " + b.y + ")]";
    }

    // No se exponen los puntos; si acaso, operaciones derivadas o copias de datos escalares.
    public double ax() { return a.x; }
    public double ay() { return a.y; }
    public double bx() { return b.x; }
    public double by() { return b.y; }
}

// Demostración rápida
class DemoFuerte {
    public static void main(String[] args) {
        LineaFuerte lf = new LineaFuerte(0, 0, 3, 4);
        System.out.println(lf);
        System.out.println("Longitud (fuerte): " + lf.longitud()); // 5.0
        // No es posible modificar ni acceder a los puntos internos: quedan ligados a la línea.
    }
}
```

### 2) Composición **débil** / **agregación** (los puntos NO dependen del ciclo de vida de `Linea`)

```java
// Punto mutable y reutilizable: puede compartirse entre varias líneas.
public class PuntoAgregado {
    private double x;
    private double y;

    public PuntoAgregado(double x, double y) { this.x = x; this.y = y; }

    public double getX() { return x; }
    public double getY() { return y; }

    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }

    public double distanciaA(PuntoAgregado otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy);
    }

    @Override
    public String toString() { return "PuntoAgregado(" + x + ", " + y + ")"; }
}

public class LineaDebil {
    private final PuntoAgregado a; // referencias externas (no hay copias defensivas)
    private final PuntoAgregado b;

    public LineaDebil(PuntoAgregado a, PuntoAgregado b) {
        if (a == null || b == null) {
            throw new IllegalArgumentException("Los puntos no pueden ser null");
        }
        this.a = a;
        this.b = b;
    }

    public double longitud() { return a.distanciaA(b); }

    // Se exponen las mismas referencias: cambios externos afectan a la línea.
    public PuntoAgregado extremoA() { return a; }
    public PuntoAgregado extremoB() { return b; }

    @Override
    public String toString() { return "LineaDebil[" + a + " -> " + b + "]"; }
}

// Demostración de agregación y compartición
class DemoDebil {
    public static void main(String[] args) {
        PuntoAgregado p = new PuntoAgregado(0, 0);
        PuntoAgregado q = new PuntoAgregado(3, 4);

        LineaDebil l1 = new LineaDebil(p, q);
        LineaDebil l2 = new LineaDebil(p, q); // mismos puntos compartidos (agregación)

        System.out.println("Longitud l1 (débil): " + l1.longitud()); // 5.0
        System.out.println("Longitud l2 (débil): " + l2.longitud()); // 5.0

        // Mutación externa de un punto compartido: ambas líneas "ven" el cambio.
        q.setX(6); q.setY(8);
        System.out.println("Tras mover q a (6,8):");
        System.out.println("Longitud l1 (débil): " + l1.longitud()); // 10.0
        System.out.println("Longitud l2 (débil): " + l2.longitud()); // 10.0
    }
}
```

**Resumen clave**: en la variante **fuerte**, los puntos se crean y viven dentro de `Linea`, no se exponen y desaparecen con ella; se materializa la propiedad y el control del ciclo de vida. En la variante **débil**, `Linea` mantiene **referencias compartibles** a puntos externos, que pueden mutar o sobrevivir sin la línea; su relación es de **agregación**. Si se desea debilidad *sin* mutabilidad, basta con hacer `Punto` inmutable y seguir pasando referencias (sin copias internas), lo que mantiene la independencia del ciclo de vida.



## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
