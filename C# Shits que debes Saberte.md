# C# Shits que debes Saberte

Esto puede volverse algo extenso, así que lo dividiré en secciones organizadas, resaltando el propósito, las características clave y cuándo usar cada concepto.

---

### **1. `class` (Clases)**

- **Qué es:** Es una referencia (reference type), lo que significa que vive en el **heap** y se accede a ella a través de un puntero.
- **Características clave:**
  - Permite mutabilidad e inmutabilidad.
  - Ideal para datos complejos o de larga duración.
  - Puede ser **null** (a diferencia de un `struct` que no puede ser null directamente).
  - Soporta herencia, lo cual es fundamental para extender la funcionalidad.
- **Cuándo usar:**
  - Cuando necesitas encapsular lógica compleja junto a tus datos.
  - Si trabajas con grandes cantidades de datos o estructuras que requieren referencias compartidas.

---

### **2. `struct` (Estructuras)**

- **Qué es:** Un tipo por valor (value type) que generalmente vive en el **stack** (a menos que esté contenido en otro objeto, entonces será "boxed" en el heap).
- **Características clave:**
  - Más eficiente para datos pequeños e inmutables.
  - Copiado por valor (lo cual puede generar overhead si es demasiado grande).
  - No admite herencia (pero puede implementar interfaces).
- **Cuándo usar:**
  - Para datos pequeños y de corta duración (por ejemplo, coordenadas, fechas, valores como `Point` o `TimeSpan`).
  - Cuando necesitas evitar el garbage collector y maximizar la eficiencia.

---

### **3. `record` (Registros)**

- **Qué es:** Introducido en C# 9, un `record` es un tipo inmutable por defecto, diseñado para modelar datos.
- **Características clave:**
  - Soporta comparación por valores (`Equals` y `GetHashCode` preconfigurados).
  - Inmutabilidad con `init` para propiedades, aunque puedes hacerlo mutable si lo necesitas.
  - Puede ser referencia (`record`) o por valor (`record struct`).
- **Cuándo usar:**
  - Cuando necesitas modelar datos inmutables y deseas comparar instancias fácilmente (ideal para DTOs, por ejemplo).

---

### **4. `partial` (Parcial)**

- **Qué es:** Permite dividir la implementación de una clase, struct o record en múltiples archivos.
- **Características clave:**
  - Útil para separar código generado automáticamente del código manual.
  - Se unifica en el ensamblado final.
- **Cuándo usar:**
  - Cuando trabajas en equipos grandes o necesitas separar responsabilidades (por ejemplo, al trabajar con frameworks como Entity Framework o Blazor).

---

### **5. `interface` (Interfaz)**

- **Qué es:** Un contrato que especifica los métodos, propiedades y eventos que un tipo debe implementar.
- **Características clave:**
  - Soporta múltiples implementaciones en una clase.
  - Facilita el polimorfismo y el desacoplamiento.
- **Cuándo usar:**
  - Cuando deseas proporcionar abstracciones claras y reutilizables.
  - Ideal en patrones como el de inyección de dependencias.

---

### **6. Herencia**

- **Qué es:** Permite a una clase derivada heredar métodos, propiedades y campos de una clase base.
- **Características clave:**
  - Fomenta la reutilización de código.
  - Soporta métodos y propiedades virtuales (pueden sobrescribirse).
- **Cuándo usar:**
  - En jerarquías claras, como un modelo `Animal -> Mamífero -> Perro`.
  - **Evita usarla excesivamente**. En muchos casos, la composición es una mejor alternativa.

---

### **7. `sealed` (Sellado)**

- **Qué es:** Evita que una clase sea heredada o que un método virtual sea sobrescrito.
- **Características clave:**
  - Mejora el rendimiento al evitar llamadas virtuales para métodos en clases selladas.
- **Cuándo usar:**
  - Cuando deseas proteger la funcionalidad contra cambios futuros.
  - Para optimizar clases o métodos que sabes que no necesitan ser extendidos.

---

### **Combinaciones posibles**

1. **`class` + `partial`:** Ideal para dividir grandes clases (por ejemplo, frameworks como EF Core).
2. **`struct` + `interface`:** Define contratos para datos pequeños (como `IEquatable` en structs).
3. **`record` + herencia:** Muy útil para manejar DTOs extendidos, ej. `UserRecord : PersonRecord`.
4. **Herencia + `sealed`:** Herencia controlada, sellando las clases base que no deben extenderse.
5. **`struct` + `readonly`:** Datos inmutables, ideales para manejar pequeñas estructuras constantes.

---

# 1).- ¡Yupi.....! Ahora vamos a integrar las especificaciones de **public**, **private** y el uso del **guion bajo (_)**

#### *(En los conceptos previamente explicados.)*

---

### **1. `public` y `private`**

Los modificadores de acceso controlan quién puede acceder a los miembros de una clase, struct o interface.

#### **`public`**

- **Qué es:** Permite que un miembro sea accesible desde cualquier lugar del código.
- **Cuándo usarlo:**
  - Para propiedades y métodos que deben ser accesibles desde fuera de la clase o estructura (por ejemplo, una API pública).
- **Ejemplo:**
  
  ```csharp
  public class MiClase
  {
    public string Nombre { get; set; } // Accesible desde cualquier lugar
  }
  ```

#### **`private`**

- **Qué es:** Restringe el acceso solo al mismo tipo (clase o struct) donde está definido.
- **Cuándo usarlo:**
  - Para datos internos o auxiliares que no deberían ser visibles fuera de la clase o estructura.
  - Ayuda a proteger la encapsulación y evitar la manipulación accidental de datos.
- **Ejemplo:**
  
  ```csharp
  public class MiClase
  {
    private string _nombre; // Solo accesible dentro de la clase
  }
  ```

#### **Otras palabras clave relacionadas:**

- `protected`: Accesible en la misma clase y en clases derivadas.
- `internal`: Accesible solo dentro del mismo ensamblado.
- `protected internal`: Una combinación de `protected` e `internal`.
- `private protected`: Accesible en la misma clase y en clases derivadas dentro del mismo ensamblado.

---

### **2. El uso del guion bajo (`_`)**

El guion bajo es una convención común en C# para nombrar **campos privados**. No es obligatorio, pero mejora la claridad del código y evita confusiones con las propiedades públicas.

#### **Convención de uso:**

- Se utiliza para diferenciar **campos privados** de **propiedades públicas**.
- Generalmente, los campos privados se usan dentro de la clase, mientras que las propiedades públicas actúan como una interfaz para exponer esos datos.

**Ejemplo:**

```csharp
public class MiClase
{
    private string _nombre; // Campo privado con guion bajo

    public string Nombre // Propiedad pública para acceder al campo
    {
        get => _nombre;
        set => _nombre = value;
    }
}
```

En este ejemplo, la propiedad `Nombre` controla el acceso al campo privado `_nombre`.

#### **Ventajas del guion bajo:**

- Claridad: Puedes identificar rápidamente que `_nombre` es un campo privado.
- Consistencia: Sigue las convenciones estándar de C# y frameworks como ASP.NET.

---

### **3. Aplicando todo junto:**

Ahora lo combinamos con las estructuras mencionadas anteriormente. Aquí tienes cómo aplicar `private` y `public` con `class`, `struct`, `record`, interfaces, y herencia, usando guion bajo:

#### **Ejemplo combinado:**

```csharp
public class Persona
{
    private int _edad; // Campo privado con guion bajo

    public string Nombre { get; set; } // Propiedad pública
    public int Edad // Propiedad pública para acceder a _edad
    {
        get => _edad;
        set => _edad = value > 0 ? value : throw new ArgumentException("La edad debe ser positiva");
    }
}

public class Empleado : Persona // Herencia
{
    public string Puesto { get; set; } // Propiedad adicional
}

public interface ITrabajador // Interfaz para contrato
{
    void Trabajar(); // Método público obligatorio en la implementación
}

public struct Coordenada // struct para datos pequeños
{
    public double X { get; init; }
    public double Y { get; init; }
}
```

#### **`sealed` en este contexto:**

```csharp
public sealed class Administrador : Empleado // No puede ser heredada
{
    public void Gestionar() { /* Lógica */ }
}
```

### **Mejores prácticas para combinar guion bajo, private/public y estructuras:**

1. **Encapsulación:** Usa `private` para proteger la lógica interna y expón datos solo con `public` si es necesario.
2. **Guion bajo (`_`) en campos privados:** Mejora la legibilidad y diferencia los campos internos de las propiedades.
3. **Herencia con acceso controlado:** Usa `protected` para permitir acceso en clases derivadas.

---

# 2).- ¡Changos! `abstract` y su wtf.

Ahora vamos a explorar el concepto de `abstract` en C# y las combinaciones más importantes relacionadas con este modificador.

---

### **¿Qué es `abstract`?**

El modificador `abstract` en C# indica que un elemento (clase o miembro) es incompleto y debe ser implementado o sobreescrito en una clase derivada. Básicamente, define el "esqueleto" de un comportamiento, dejando los detalles de su implementación a las clases que heredan de él.

- **Clases abstractas**: Sirven como modelos base y no pueden ser instanciadas directamente.
- **Métodos abstractos**: Declaran la firma del método, pero no su implementación.

---

### **1. Clases Abstractas**

Una clase con el modificador `abstract` **no puede ser instanciada directamente** y suele contener tanto métodos abstractos (sin implementación) como concretos (con implementación).

**Ejemplo**:

```csharp
public abstract class Vehiculo
{
    public string Marca { get; set; }
    public string Modelo { get; set; }

    // Método abstracto: no tiene implementación.
    public abstract void Moverse();

    // Método concreto: tiene implementación.
    public void Encender()
    {
        Console.WriteLine("El vehículo está encendido.");
    }
}
```

> En este caso, `Vehiculo` es una clase abstracta. Tiene un método abstracto `Moverse` (que cada clase derivada deberá implementar) y un método concreto `Encender` que está listo para usarse.

---

### **2. Métodos Abstractos**

Un método abstracto **solo puede existir dentro de una clase abstracta**. Obliga a las clases derivadas a proporcionar una implementación.

**Ejemplo**:

```csharp
public abstract class Animal
{
    public abstract void HacerSonido(); // Método abstracto, sin implementación.
}
```

**Clase derivada que implementa el método abstracto**:

```csharp
public class Perro : Animal
{
    public override void HacerSonido()
    {
        Console.WriteLine("Guau guau");
    }
}
```

Si no implementas el método abstracto en la clase derivada, el compilador arrojará un error.

---

### **3. Clases Concretas que Heredan de Clases Abstractas**

Una clase concreta debe implementar todos los métodos abstractos de su clase base abstracta, pero puede optar por sobrescribir (o no) los métodos concretos heredados.

**Ejemplo Completo**:

```csharp
public abstract class Figura
{
    public abstract double CalcularArea(); // Método abstracto
    public abstract double CalcularPerimetro();

    public void MostrarTipo()
    {
        Console.WriteLine("Soy una figura geométrica.");
    }
}

public class Circulo : Figura
{
    public double Radio { get; set; }

    public Circulo(double radio)
    {
        Radio = radio;
    }

    public override double CalcularArea()
    {
        return Math.PI * Radio * Radio;
    }

    public override double CalcularPerimetro()
    {
        return 2 * Math.PI * Radio;
    }
}
```

Uso de la clase concreta:

```csharp
Figura circulo = new Circulo(5);
Console.WriteLine($"Área: {circulo.CalcularArea()}");
Console.WriteLine($"Perímetro: {circulo.CalcularPerimetro()}");
```

---

### **4. Combinaciones con Otros Modificadores**

1. **`abstract` + `sealed`**: No es válido. Una clase sellada (`sealed`) no puede ser heredada, por lo que no puede ser abstracta (porque necesita ser implementada por una clase derivada).

2. **`abstract` + `override`**:
   Una clase derivada puede usar `override` para implementar un método abstracto definido en la clase base.

**Ejemplo**:

```csharp
public abstract class Persona
{
    public abstract void Saludar();
}

public class Estudiante : Persona
{
    public override void Saludar()
    {
        Console.WriteLine("Hola, soy un estudiante.");
    }
}
```

3. **`abstract` + `partial`**:
   Puedes dividir una clase abstracta en múltiples archivos usando el modificador `partial`.

**Ejemplo**:

```csharp
public abstract partial class Operacion
{
    public abstract double Calcular();
}

public abstract partial class Operacion
{
    public string Descripcion { get; set; }
}
```

4. **`abstract` + `interface`**:
   Las interfaces también definen contratos, pero una clase abstracta puede implementar parcialmente una interfaz. Las clases derivadas deben completar el contrato.

**Ejemplo**:

```csharp
public interface IMedible
{
    double CalcularArea();
    double CalcularPerimetro();
}

public abstract class Figura : IMedible
{
    public abstract double CalcularArea(); // Se deja sin implementación
    public abstract double CalcularPerimetro();
}
```

---

### **5. Métodos Abstractos en Jerarquías**

Si una clase abstracta tiene métodos abstractos y concretos, puedes diseñar jerarquías complejas. Cada nivel puede agregar su propia funcionalidad o implementación.

**Ejemplo**:

```csharp
public abstract class Transporte
{
    public abstract void Mover();
    public virtual void Cargar()
    {
        Console.WriteLine("Cargando transporte genérico.");
    }
}

public abstract class Vehiculo : Transporte
{
    public override void Cargar()
    {
        Console.WriteLine("Cargando un vehículo.");
    }
}

public class Camion : Vehiculo
{
    public override void Mover()
    {
        Console.WriteLine("El camión se está moviendo.");
    }

    public override void Cargar()
    {
        Console.WriteLine("Cargando un camión de mercancías pesadas.");
    }
}
```

Uso:

```csharp
Transporte miCamion = new Camion();
miCamion.Mover();
miCamion.Cargar();
```

---

### **Diferencia entre `abstract` y `interface`**

- Una `interface` solo puede contener la firma de los métodos, propiedades o eventos. Todo debe ser implementado por las clases que la implementan.
- Una clase `abstract` puede incluir tanto métodos abstractos (sin implementación) como concretos (con implementación).

> **Cuándo usar `abstract`**:
> 
> - Cuando deseas proporcionar una implementación base común con métodos o propiedades compartidas.
> - Para escenarios donde varias clases comparten lógica específica.

> **Cuándo usar `interface`**:
> 
> - Para definir un contrato que múltiples tipos deben cumplir, sin necesidad de lógica compartida.



---

# 3).- **Resumen de los Modificadores y Conceptos**

1. **`abstract` (Clases y Métodos Abstractos):**
   
   - Define un "esqueleto" o modelo que otras clases deben implementar.
   - Una **clase abstracta** no puede ser instanciada directamente.
   - Un **método abstracto** no tiene implementación y obliga a las clases derivadas a sobrescribirlo.

2. **`interface` (Interfaces):**
   
   - Contratos que definen qué métodos o propiedades deben implementarse en clases o structs.
   - No puede contener implementación (excepto métodos con cuerpo por defecto desde C# 8.0).

3. **`class` (Clases):**
   
   - Tipo de referencia usado para encapsular datos y comportamiento.
   - Puede contener métodos, propiedades, constructores, etc.
   - Soporta herencia y polimorfismo.

4. **`struct` (Estructuras):**
   
   - Tipo por valor diseñado para datos pequeños e inmutables.
   - No soporta herencia, pero puede implementar interfaces.

5. **`sealed` (Clases Selladas):**
   
   - Impide que una clase pueda heredarse.
   - Mejora el rendimiento al evitar llamadas virtuales.

6. **Modificadores de Acceso (`public`, `private`, etc.):**
   
   - **`public`**: Accesible desde cualquier lugar.
   - **`private`**: Accesible solo dentro de la clase o struct que lo declara.
   - **`protected`**: Accesible desde la clase base y las clases derivadas.
   - **`internal`**: Accesible dentro del mismo ensamblado.
   - **`protected internal`**: Accesible desde clases en el mismo ensamblado o derivadas.
   - **`private protected`**: Restricción a clases derivadas dentro del mismo ensamblado.

7. **`partial` (Clases Parciales):**
   
   - Divide la implementación de una clase en múltiples archivos.
   - Útil para separar código generado automáticamente y código escrito manualmente.

8. **`override` (Métodos Sobrescritos):**
   
   - Permite cambiar la implementación de un método virtual o abstracto definido en una clase base.

9. **Uso del Guion Bajo (`_`):**
   
   - Convención común para nombrar **campos privados** y diferenciarlos de propiedades públicas.

---

### **Orden y Jerarquía en la Construcción**

En C#, existe un **orden lógico y jerárquico** para construir elementos. Aquí está la guía:

1. **Modificador de Acceso (siempre va primero):**
   
   - `public`, `private`, `protected`, etc. 
   - Ejemplo válido: `public abstract class`.

2. **Abstract o Sealed (en segundo lugar, si aplica):**
   
   - Solo una clase puede ser `abstract` o `sealed`, nunca ambas.
   - Ejemplo válido: `public abstract class`.

3. **Palabra Clave del Tipo (class, struct, interface, etc.):**
   
   - Siempre debe especificar qué tipo se declara.
   - Ejemplo válido: `public abstract class`.

4. **Nombre de la Clase, Interface o Struct:**
   
   - Define el identificador del tipo.
   - Ejemplo válido: `public abstract class Vehiculo`.

5. **Implementación o Herencia (si aplica):**
   
   - Especifica interfaces implementadas o clases base heredadas.
   - Ejemplo válido: `public abstract class Vehiculo : Transporte, IMovible`.

6. **Métodos y Propiedades:**
   
   - Aplica modificadores y define la implementación dentro del cuerpo de la clase.

**Ejemplo Final de Jerarquía:**

```csharp
// Orden jerárquico correcto
public abstract class Vehiculo : Transporte, IMovible
{
    private string _marca; // Campo privado

    public string Marca // Propiedad pública
    {
        get => _marca;
        set => _marca = value;
    }

    public abstract void Moverse(); // Método abstracto
}
```

**Ejemplo Incorrecto:**

```csharp
// Esto no es válido porque el orden es incorrecto
class interface public Vehiculo { ... }
```

---

### **Conclusión**

La jerarquía es: **Modificador de acceso** → **Tipo de abstracción (`abstract`, `sealed`)** → **Tipo (`class`, `interface`, `struct`)** → **Nombre** → **Herencia/Implementación** → **Cuerpo de Métodos y Propiedades**.

---

# 4)- ¡A por el tio coño!

 Desarrollaremos un caso práctico que integre `abstract`, clases concretas, interfaces y el orden jerárquico correcto ***(pero sin mucho guón bajo, así notas la diferencia)***.

---

### **Caso Práctico: Sistema de Gestión de Vehículos**

Construiremos un sistema que maneja diferentes tipos de vehículos (automóviles, bicicletas, camiones) utilizando clases abstractas, interfaces y modificadores de acceso en el orden correcto.

#### **Código Completo**

```csharp
// Interface: Define un contrato para vehículos motorizados
public interface IVehiculoMotorizado
{
    void EncenderMotor();
    void ApagarMotor();
}

// Clase abstracta: Modelo base para cualquier tipo de vehículo
public abstract class Vehiculo
{
    private string _marca; // Campo privado

    // Propiedad pública que accede al campo privado
    public string Marca 
    {
        get => _marca;
        set => _marca = value ?? throw new ArgumentNullException(nameof(value));
    }

    public int AnioFabricacion { get; set; } // Propiedad pública

    public Vehiculo(string marca, int anioFabricacion)
    {
        _marca = marca;
        AnioFabricacion = anioFabricacion;
    }

    // Método abstracto que debe implementarse en las clases derivadas
    public abstract void Mover();

    // Método concreto que ya tiene implementación
    public virtual void Detener()
    {
        Console.WriteLine("El vehículo se ha detenido.");
    }
}

// Clase concreta: Automóvil (hereda de Vehiculo y usa la interfaz)
public class Automovil : Vehiculo, IVehiculoMotorizado
{
    public int NumeroPuertas { get; set; }

    public Automovil(string marca, int anioFabricacion, int numeroPuertas) 
        : base(marca, anioFabricacion)
    {
        NumeroPuertas = numeroPuertas;
    }

    // Implementación del método abstracto
    public override void Mover()
    {
        Console.WriteLine($"{Marca} se está desplazando sobre cuatro ruedas.");
    }

    // Implementación de la interfaz
    public void EncenderMotor()
    {
        Console.WriteLine("Motor del automóvil encendido.");
    }

    public void ApagarMotor()
    {
        Console.WriteLine("Motor del automóvil apagado.");
    }
}

// Clase concreta: Bicicleta (hereda de Vehiculo)
public class Bicicleta : Vehiculo
{
    public string Tipo { get; set; } // Propiedad adicional para bicicletas (ej., Montaña, Urbana)

    public Bicicleta(string marca, int anioFabricacion, string tipo) 
        : base(marca, anioFabricacion)
    {
        Tipo = tipo;
    }

    // Implementación del método abstracto
    public override void Mover()
    {
        Console.WriteLine($"{Marca} se está desplazando con pedales.");
    }
}

// Clase concreta sellada: Camión
public sealed class Camion : Vehiculo, IVehiculoMotorizado
{
    public double CapacidadCarga { get; set; }

    public Camion(string marca, int anioFabricacion, double capacidadCarga) 
        : base(marca, anioFabricacion)
    {
        CapacidadCarga = capacidadCarga;
    }

    // Implementación del método abstracto
    public override void Mover()
    {
        Console.WriteLine($"{Marca} transporta carga pesada.");
    }

    // Implementación de la interfaz
    public void EncenderMotor()
    {
        Console.WriteLine("Motor del camión encendido.");
    }

    public void ApagarMotor()
    {
        Console.WriteLine("Motor del camión apagado.");
    }

    // Método sobrescrito con funcionalidad específica para camiones
    public override void Detener()
    {
        Console.WriteLine("El camión se ha detenido y ha activado el freno de mano.");
    }
}

// Programa principal para demostrar su uso
public static class ProgramaPrincipal
{
    public static void Main()
    {
        // Crear instancias de los vehículos
        Vehiculo miAuto = new Automovil("Toyota", 2023, 4);
        Vehiculo miBici = new Bicicleta("Giant", 2022, "Montaña");
        Vehiculo miCamion = new Camion("Mercedes-Benz", 2020, 18.5);

        // Usar los vehículos
        miAuto.Mover();
        miAuto.Detener();

        miBici.Mover();
        miBici.Detener();

        miCamion.Mover();
        miCamion.Detener();

        // Operaciones específicas para vehículos motorizados
        IVehiculoMotorizado autoMotorizado = (IVehiculoMotorizado)miAuto;
        autoMotorizado.EncenderMotor();
        autoMotorizado.ApagarMotor();

        IVehiculoMotorizado camionMotorizado = (IVehiculoMotorizado)miCamion;
        camionMotorizado.EncenderMotor();
        camionMotorizado.ApagarMotor();
    }
}
```

---

### **Explicación Jerárquica y Orden de Construcción**

1. **Primero, definimos el contrato o la interfaz (`IVehiculoMotorizado`):**
   
   - Se asegura de que todas las clases que implementen esta interfaz deben definir los métodos `EncenderMotor()` y `ApagarMotor()`.

2. **Segundo, creamos la clase abstracta base (`Vehiculo`):**
   
   - Contiene tanto métodos abstractos (`Mover`) como métodos concretos (`Detener`).
   - Proporciona propiedades comunes a todos los vehículos, como `Marca` y `AnioFabricacion`.

3. **Tercero, creamos clases concretas que heredan de la clase abstracta:**
   
   - `Automovil` hereda de `Vehiculo` e implementa la interfaz `IVehiculoMotorizado`.
   - `Bicicleta` hereda de `Vehiculo` y agrega una propiedad específica (`Tipo`).
   - `Camion` hereda de `Vehiculo`, implementa la interfaz, y sobrescribe el método `Detener`.

4. **Cuarto, usamos el programa principal para demostrar:**
   
   - Polimorfismo al usar `Vehiculo` como tipo base para `Automovil`, `Bicicleta` y `Camion`.
   - Implementaciones específicas de la interfaz para vehículos motorizados.

---

### **Reglas de Orden y Jerarquía al Construir**

- El **modificador de acceso** (`public`, `private`, etc.) debe ir **siempre al principio**.
- Si usas `abstract`, `sealed` o `static`, este va **antes de `class` o `interface`**.
- El **nombre** de la clase o interfaz viene después.
- Cualquier **herencia o implementación de interfaces** sigue el nombre.

**Ejemplo correcto (orden jerárquico):**

```csharp
public abstract class Vehiculo : Transporte, IMovible
{
    // Cuerpo de la clase
}
```

---

## ¡Anda la osa!

Ahora vamos a desarrollar el mismo caso práctico del sistema de gestión de vehículos, pero **incorporando el uso del guion bajo (`_`)** como convención para los **campos privados**. Esto mejora la claridad del código al diferenciar entre campos privados y propiedades públicas.

---

```csharp
// Interface: Define un contrato para vehículos motorizados
public interface IVehiculoMotorizado
{
    void EncenderMotor();
    void ApagarMotor();
}

// Clase abstracta: Modelo base para cualquier tipo de vehículo
public abstract class Vehiculo
{
    // Campo privado con guion bajo
    private string _marca;

    // Propiedad pública que accede al campo privado
    public string Marca
    {
        get => _marca;
        set => _marca = value ?? throw new ArgumentNullException(nameof(value));
    }

    private int _anioFabricacion; // Campo privado con guion bajo

    public int AnioFabricacion // Propiedad pública que accede al campo
    {
        get => _anioFabricacion;
        set => _anioFabricacion = value > 0 ? value : throw new ArgumentException("El año debe ser positivo.");
    }

    // Constructor protegido que inicializa los campos
    protected Vehiculo(string marca, int anioFabricacion)
    {
        _marca = marca;
        _anioFabricacion = anioFabricacion;
    }

    // Método abstracto que debe ser implementado por las clases derivadas
    public abstract void Mover();

    // Método concreto ya implementado
    public virtual void Detener()
    {
        Console.WriteLine("El vehículo se ha detenido.");
    }
}

// Clase concreta: Automóvil (hereda de Vehiculo y usa la interfaz)
public class Automovil : Vehiculo, IVehiculoMotorizado
{
    private int _numeroPuertas; // Campo privado con guion bajo

    public int NumeroPuertas // Propiedad pública que accede al campo privado
    {
        get => _numeroPuertas;
        set => _numeroPuertas = value > 0 ? value : throw new ArgumentException("El número de puertas debe ser positivo.");
    }

    public Automovil(string marca, int anioFabricacion, int numeroPuertas)
        : base(marca, anioFabricacion)
    {
        _numeroPuertas = numeroPuertas;
    }

    // Implementación del método abstracto
    public override void Mover()
    {
        Console.WriteLine($"{Marca} se está desplazando sobre cuatro ruedas.");
    }

    // Implementación de la interfaz
    public void EncenderMotor()
    {
        Console.WriteLine("Motor del automóvil encendido.");
    }

    public void ApagarMotor()
    {
        Console.WriteLine("Motor del automóvil apagado.");
    }
}

// Clase concreta: Bicicleta (hereda de Vehiculo)
public class Bicicleta : Vehiculo
{
    private string _tipo; // Campo privado con guion bajo

    public string Tipo // Propiedad pública que accede al campo privado
    {
        get => _tipo;
        set => _tipo = value ?? throw new ArgumentNullException(nameof(value));
    }

    public Bicicleta(string marca, int anioFabricacion, string tipo)
        : base(marca, anioFabricacion)
    {
        _tipo = tipo;
    }

    // Implementación del método abstracto
    public override void Mover()
    {
        Console.WriteLine($"{Marca} se está desplazando con pedales.");
    }
}

// Clase concreta sellada: Camión
public sealed class Camion : Vehiculo, IVehiculoMotorizado
{
    private double _capacidadCarga; // Campo privado con guion bajo

    public double CapacidadCarga // Propiedad pública que accede al campo privado
    {
        get => _capacidadCarga;
        set => _capacidadCarga = value >= 0 ? value : throw new ArgumentException("La capacidad de carga debe ser no negativa.");
    }

    public Camion(string marca, int anioFabricacion, double capacidadCarga)
        : base(marca, anioFabricacion)
    {
        _capacidadCarga = capacidadCarga;
    }

    // Implementación del método abstracto
    public override void Mover()
    {
        Console.WriteLine($"{Marca} transporta carga pesada.");
    }

    // Implementación de la interfaz
    public void EncenderMotor()
    {
        Console.WriteLine("Motor del camión encendido.");
    }

    public void ApagarMotor()
    {
        Console.WriteLine("Motor del camión apagado.");
    }

    // Sobrescritura del método para funcionalidad específica del camión
    public override void Detener()
    {
        Console.WriteLine("El camión se ha detenido y ha activado el freno de mano.");
    }
}

// Programa principal para demostrar el uso
public static class ProgramaPrincipal
{
    public static void Main()
    {
        // Crear instancias de los vehículos
        Vehiculo miAuto = new Automovil("Toyota", 2023, 4);
        Vehiculo miBici = new Bicicleta("Giant", 2022, "Montaña");
        Vehiculo miCamion = new Camion("Mercedes-Benz", 2020, 18.5);

        // Usar los vehículos
        miAuto.Mover();
        miAuto.Detener();

        miBici.Mover();
        miBici.Detener();

        miCamion.Mover();
        miCamion.Detener();

        // Operaciones específicas para vehículos motorizados
        IVehiculoMotorizado autoMotorizado = (IVehiculoMotorizado)miAuto;
        autoMotorizado.EncenderMotor();
        autoMotorizado.ApagarMotor();

        IVehiculoMotorizado camionMotorizado = (IVehiculoMotorizado)miCamion;
        camionMotorizado.EncenderMotor();
        camionMotorizado.ApagarMotor();
    }
}
```

---

### **Explicación del Uso del Guion Bajo**

1. **Para campos privados (`_`)**:
   
   - Se usa el guion bajo como prefijo para identificar rápidamente que el campo es privado.
   - Mejora la legibilidad al diferenciar entre los campos privados (`_marca`, `_anioFabricacion`) y las propiedades públicas (`Marca`, `AnioFabricacion`).

2. **Ventajas del guion bajo**:
   
   - Evita colisiones de nombres entre campos y propiedades.
   - Hace más legible el código en proyectos grandes.

---

### **Conclusión**

Con este enfoque, el uso del guion bajo en los campos privados ayuda a mantener un código más organizado y fácil de seguir, mientras que la jerarquía correcta garantiza que todo esté en orden lógico: **modificadores de acceso** → **`abstract/sealed`** → **tipo (class/interface)** → **nombre** → **herencia/implementación y bla bla blas**.

---

# 5).- ¡Ya casi! (Mentira, falta una banda).

- Aquí tienes un resumen completo de las **convenciones de sintaxis en C#**, enfocado en mantener un código limpio, legible y consistente. 

- Estas convenciones son muy importantes tanto para proyectos personales como para trabajo en equipo.

---

### **1. Nomenclatura y Convenciones de Estilo**

#### **1.1. Nombres de Clases, Métodos y Propiedades**

- **Clases y tipos:** Usan **PascalCase** (cada palabra empieza con mayúscula).
  - Ejemplo: `Cliente`, `EmpleadoTiempoCompleto`
- **Métodos y propiedades:** También usan **PascalCase**.
  - Ejemplo: `CalcularSalario()`, `NombreCompleto`
- **Interfaces:** Empiezan con `I` seguido de PascalCase.
  - Ejemplo: `IEmpleado`, `IVehiculoMotorizado`

#### **1.2. Campos Privados**

- Usar **camelCase** con **prefijo de guion bajo** (`_`) para campos privados.
  - Ejemplo: `_nombre`, `_edad`
- Esto ayuda a diferenciar los campos de las propiedades públicas.

#### **1.3. Variables Locales**

- Usan **camelCase**, sin guion bajo.
  - Ejemplo: `var resultado = CalcularArea();`

#### **1.4. Constantes**

- Usar **PascalCase** para constantes públicas o estáticas.
  - Ejemplo: `const int MaximoIntentos = 3;`

---

### **2. Modificadores de Acceso**

El modificador de acceso siempre debe ir al **principio de la declaración**:

- Ejemplo para una clase pública:
  
  ```csharp
  public class Vehiculo
  {
      private string _marca; // Campo privado
      public string Marca { get; set; } // Propiedad pública
  }
  ```

---

### **3. Orden de Declaración dentro de una Clase**

El orden recomendado es:

1. **Campos privados.**
2. **Propiedades públicas.**
3. **Constructores.**
4. **Métodos públicos.**
5. **Métodos privados.**
6.  **En Visual Estudio puedes usar #REGION y #END REGION**

Ejemplo:

```csharp
public class Empleado
{
    #region Campos Privados
    private string _nombre; // Campo privado
    private int _edad; // Otro campo privado
    #endregion

    #region Propiedades Públicas
    public string Nombre
    {
        get => _nombre;
        set => _nombre = value ?? throw new ArgumentNullException(nameof(value));
    }

    public int Edad
    {
        get => _edad;
        set => _edad = value > 0 ? value : throw new ArgumentException("La edad debe ser positiva.");
    }
    #endregion

    #region Constructores
    public Empleado(string nombre, int edad)
    {
        _nombre = nombre;
        _edad = edad;
    }
    #endregion

    #region Métodos Públicos
    public void MostrarInformacion()
    {
        Console.WriteLine($"Empleado: {Nombre}, Edad: {Edad}");
    }
    #endregion

    #region Métodos Privados
    private void Log(string mensaje)
    {
        Console.WriteLine($"Log: {mensaje}");
    }
    #endregion
}

```

---

### **4. Uso de Espacios y Sangría**

- **Espacios:**
  - Entre palabras clave y paréntesis.
    - Correcto: `if (condicion)`.
    - Incorrecto: `if(condicion)`.
- **Llaves:** Siempre deben ir en una **nueva línea**.
  - Correcto:
    
    ```csharp
    if (condicion)
    {
        Console.WriteLine("Condición verdadera.");
    }
    ```
  - Incorrecto:
    
    ```csharp
    if (condicion) {
        Console.WriteLine("Condición verdadera.");
    }
    ```
- **Sangría:** Usar 4 espacios para cada nivel de indentación (no usar tabuladores).

---

### **5. Declaraciones de Clases y Métodos**

- Declarar una clase de manera clara con modificadores, nombre y herencia/interfaz en el orden correcto.
  
  ```csharp
  public abstract class Vehiculo : Transporte, IMovible
  {
      // Cuerpo de la clase
  }
  ```

---

### **6. Comentarios**

- **Comentarios de una sola línea:** Usar `//`.
  
  ```csharp
  // Este es un comentario de una sola línea
  ```
- **Comentarios multilínea:** Usar `/* */`.
  
  ```csharp
  /* Este es un comentario 
     de múltiples líneas */
  ```
- **Documentación:** Usar `///` para generar comentarios XML.
  
  ```csharp
  /// <summary>
  /// Calcula el área de una figura.
  /// </summary>
  /// <param name="lado">El lado de la figura.</param>
  /// <returns>El área calculada.</returns>
  public double CalcularArea(double lado)
  {
      return lado * lado;
  }
  ```

---

### **7. Uso de `this` y el Guion Bajo**

- Usar `this` para hacer referencia explícita a miembros de la clase si hay conflicto con nombres de parámetros.
  
  ```csharp
  public class Persona
  {
      private string _nombre;
  
      public Persona(string nombre)
      {
          this._nombre = nombre; // Se usa this para distinguir entre campo y parámetro
      }
  }
  ```

---

### **8. Uso de `var`**

- Usar `var` solo cuando el tipo es obvio.
  
  ```csharp
  var lista = new List<int>(); // Correcto
  int numero = 5;             // Correcto
  var numero = 5;             // También aceptable
  ```

---

### **9. Convenciones para Nombres de Archivos**

- Un archivo debe llamarse igual que la clase, struct o interfaz que contiene.
  - Ejemplo: Si la clase es `Empleado`, el archivo debe llamarse `Empleado.cs`.

---

### **10. Manejo de Espacios de Nombres**

- Los namespaces deben estar organizados jerárquicamente y reflejar la estructura del proyecto.
  
  ```csharp
  namespace Proyecto.Sistema.Vehiculos
  {
      public class Vehiculo
      {
          // Definición de la clase
      }
  }
  ```

---

### **11. Principios Generales**

- **Mantén el código limpio:** Sigue el principio de responsabilidad única (cada clase debe tener un único propósito).
- **Evita métodos largos:** Divide la lógica en métodos más pequeños y reutilizables.
- **Usa nombres significativos:** Los nombres de variables, métodos y clases deben describir claramente su propósito.

---

### **Conclusión**

Las convenciones de sintaxis en C# mejoran la **legibilidad**, **mantenibilidad** y la **calidad** del código. Siguiendo estas reglas, construirás proyectos más claros y profesionales, además de facilitar el trabajo en equipo. 

---

# 6).- Metodos de clases.

En C#, hay ciertas reglas sobre cómo puedes combinar modificadores en la definición de un método. Algunos modificadores son compatibles entre sí, mientras que otros no pueden combinarse debido a las restricciones del lenguaje. Vamos a desglosar cómo funcionan y cuáles son las posibles combinaciones:

---

### **1. Modificadores comunes de métodos**
- **`public`/`private`/`protected`/`internal`:** Controlan la accesibilidad del método.
- **`static`:** Indica que el método pertenece a la clase y no a una instancia de la clase.
- **`partial`:** Permite que la definición del método esté dividida en varios archivos (usualmente usado con clases `partial`).
- **`virtual`:** Permite que el método sea sobrescrito en una clase derivada.
- **`override`:** Indica que el método sobrescribe una implementación base.
- **`abstract`:** Declara un método que no tiene implementación y debe ser implementado en clases derivadas.
- **`sealed`:** Impide que un método sobrescrito sea sobrescrito nuevamente en clases derivadas.
- **`async`:** Permite que un método utilice la programación asincrónica con `await`.
- **`extern`:** Declara que el método está definido fuera del código administrado de C#, por ejemplo, en una biblioteca nativa.
- **`dynamic` (retorno):** Permite que el valor de retorno sea resuelto dinámicamente en tiempo de ejecución.

---

### **2. Combinaciones válidas**
Algunas combinaciones comunes que puedes usar en métodos son:

1. **`public static`:** Método de clase accesible globalmente.
   ```csharp
   public static void MiMetodo() { }
   ```

2. **`public partial`:** Métodos dentro de clases `partial`, combinados cuando se compila el código.
   ```csharp
   public partial void MiMetodo();
   ```

3. **`public async`:** Métodos que soportan operaciones asincrónicas.
   ```csharp
   public async Task MiMetodoAsync() { await Task.Delay(1000); }
   ```

4. **`protected override`:** Métodos sobrescritos en una clase derivada.
   ```csharp
   protected override void MiMetodo() { }
   ```

5. **`private static`:** Métodos estáticos que solo son accesibles dentro de la clase.
   ```csharp
   private static int Calcular() { return 42; }
   ```

6. **`public virtual`:** Métodos que pueden ser sobrescritos.
   ```csharp
   public virtual void MetodoVirtual() { }
   ```

7. **`internal static`:** Métodos de clase accesibles solo dentro del ensamblado.
   ```csharp
   internal static void MetodoInterno() { }
   ```

---

### **3. Combinaciones no válidas**
1. **`static` con `virtual`:**
   - No puedes combinar estos dos, ya que `static` indica que el método pertenece a la clase, mientras que `virtual` requiere que el método sea sobrescrito, lo cual depende de una instancia.

2. **`abstract` con `static`:**
   - Un método `abstract` no puede ser `static` porque `abstract` requiere una implementación en una clase derivada, pero `static` pertenece a la clase base.

3. **`partial` sin clase `partial`:**
   - Los métodos `partial` solo funcionan dentro de clases marcadas como `partial`.

4. **`sealed` sin `override`:**
   - `sealed` solo tiene sentido cuando estás impidiendo sobrescribir un método previamente `override`.

5. **`dynamic` como modificador:**
   - `dynamic` no es un modificador, es un tipo de retorno. No puedes usarlo como modificador de método, pero sí como tipo:
     ```csharp
     public dynamic MiMetodo() { return "Hola"; }
     ```

---

### **4. Ejemplo válido inspirado en una pregunta**
¿Puedo hacer esto?: `public static partial dynamic MyMethod()` 

Aunque no es válido como está escrito, algo similar pero válido sería:

```csharp
public partial class MiClase
{
    public static dynamic MiMetodo()
    {
        return "Valor dinámico";
    }
}
```

Aquí:
- **`public` y `static`:** El método es accesible globalmente y pertenece a la clase.
- **`partial`:** Si `MiClase` es una clase parcial, este método puede estar definido en un archivo parcial.
- **`dynamic`:** El tipo de retorno puede ser resuelto en tiempo de ejecución.

---

### **5. Consideraciones finales**
Combinar modificadores en métodos puede ser muy útil para expresar las intenciones del diseño de tu código. Sin embargo, es importante seguir las reglas de C# y usar combinaciones que reflejen la funcionalidad deseada.

---
# 7).- Overview de los metodos.

Utilizando la **analogía** donde una Clase es una tabla en una base de datos (BDD), los métodos en C# pueden entenderse según cómo actúan en relación con las "filas" (instancias de objetos) y la "tabla" (la clase en su conjunto). Aquí va la explicación:

---

### **Tipos de métodos en el contexto de una tabla de BDD**

1. **Métodos Estáticos: Trabajan en la tabla completa**
   - Los métodos estáticos pertenecen a la "tabla" o clase completa. Estos métodos no necesitan una instancia de la tabla para ejecutarse, ya que operan directamente sobre la "estructura global" o el conjunto completo de registros.

   - Ejemplo de método estático:
     - **`ContarFilas`**: Este método podría contar cuántas filas hay en la tabla.
     - **`BuscarFilasConNulos`**: Identificar qué registros tienen valores faltantes.

   **Ejemplo en código:**
   ```csharp
   public static class Tabla
   {
       private static List<Fila> filas = new List<Fila>();

       public static int ContarFilas()
       {
           return filas.Count; // Retorna la cantidad de registros en la tabla
       }

       public static List<Fila> BuscarFilasConNulos()
       {
           return filas.Where(f => f.TieneNulos()).ToList(); // Encuentra registros con valores nulos
       }
   }
   ```

   - **Paralelismo en la BDD:** Estos métodos son como hacer consultas generales, por ejemplo: `SELECT COUNT(*) FROM tabla;` o `SELECT * FROM tabla WHERE columna IS NULL;`.

---

2. **Métodos de Instancia: Operan sobre una fila específica**
   - Los métodos de instancia están diseñados para trabajar en un registro individual (fila). Estos métodos necesitan ser llamados desde una instancia de la clase, lo que significa que necesitas "seleccionar" una fila para operar en ella.

   - Ejemplo de método de instancia:
     - **`ActualizarColumna`**: Cambiar un valor en una columna específica de esa fila.
     - **`TieneNulos`**: Verificar si alguna columna de la fila tiene valores nulos.

   **Ejemplo en código:**
   ```csharp
   public class Fila
   {
       public string Columna1 { get; set; }
       public string Columna2 { get; set; }

       public bool TieneNulos()
       {
           return string.IsNullOrEmpty(Columna1) || string.IsNullOrEmpty(Columna2); // Verifica si hay nulos
       }

       public void ActualizarColumna(int columna, string valor)
       {
           if (columna == 1) Columna1 = valor;
           else if (columna == 2) Columna2 = valor;
       }
   }
   ```

   - **Paralelismo en la BDD:** Esto sería como hacer algo específico para una fila: `UPDATE tabla SET columna1 = 'valor' WHERE id = 1;`.

---

### **Colaboración entre métodos estáticos e instancias**

La combinación de métodos estáticos y de instancia crea un ecosistema robusto. Por ejemplo:
- Un **método estático** podría identificar todas las filas con datos faltantes.
- Luego, un experto (simbolizado aquí como un usuario) podría usar métodos de instancia para actualizar cada fila de forma específica.

---

### **Ejemplo completo del ecosistema**

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

public class Fila
{
    public string Columna1 { get; set; }
    public string Columna2 { get; set; }

    public Fila(string columna1 = null, string columna2 = null)
    {
        Columna1 = columna1;
        Columna2 = columna2;
    }

    public bool TieneNulos()
    {
        return string.IsNullOrEmpty(Columna1) || string.IsNullOrEmpty(Columna2);
    }

    public void ActualizarColumna(int columna, string valor)
    {
        if (columna == 1) Columna1 = valor;
        else if (columna == 2) Columna2 = valor;
    }
}

public static class Tabla
{
    private static List<Fila> filas = new List<Fila>();

    public static void AgregarFila(Fila fila)
    {
        filas.Add(fila);
    }

    public static int ContarFilas()
    {
        return filas.Count;
    }

    public static List<Fila> BuscarFilasConNulos()
    {
        return filas.Where(f => f.TieneNulos()).ToList();
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Agregar filas (registros) iniciales
        Tabla.AgregarFila(new Fila());
        Tabla.AgregarFila(new Fila("Valor 1", null));
        Tabla.AgregarFila(new Fila("Valor 1", "Valor 2"));

        Console.WriteLine($"Total de filas: {Tabla.ContarFilas()}");

        // Encontrar filas con datos faltantes
        var filasConNulos = Tabla.BuscarFilasConNulos();
        Console.WriteLine($"Filas con nulos: {filasConNulos.Count}");

        // Actualizar una fila específica
        if (filasConNulos.Count > 0)
        {
            filasConNulos[0].ActualizarColumna(2, "Valor actualizado");
            Console.WriteLine("Se actualizó una fila con valores nulos.");
        }

        Console.WriteLine($"Total de filas tras la actualización: {Tabla.ContarFilas()}");
    }
}
```

---

### **Resumen del paralelismo**
- **Métodos estáticos:** Actúan a nivel de "tabla". Son útiles para trabajar con el conjunto completo de registros.
- **Métodos de instancia:** Operan a nivel de "fila". Son ideales para modificar o analizar un registro en particular.
- **Combinación:** Usar ambos permite manejar la base de datos tanto global como individualmente, creando un sistema flexible y escalable.

---
# 9).- Sobre Funciones

En C#, las **funciones** son bloques de código reutilizables que realizan una tarea específica y, en la mayoría de los casos, devuelven un valor. 

En términos más técnicos, las funciones en C# se implementan como **métodos** dentro de clases o estructuras, porque todo en C# está contenido dentro de una clase o estructura. Vamos a explorar cómo funcionan, sus tipos y cómo se pueden usar.

---

### **1. Partes de una función en C#**
Cada función (o método) tiene las siguientes partes principales:

```csharp
[modificadores] tipoDeRetorno NombreDeLaFuncion([parámetros])
{
    // Cuerpo de la función: conjunto de instrucciones
    return valor; // Opcional si el tipo de retorno no es void
}
```

#### **Ejemplo básico:**
```csharp
public int Sumar(int a, int b)
{
    return a + b;
}
```

---

### **2. Tipos de funciones en C#**
Las funciones pueden clasificarse según varios criterios:

#### **a) Según el tipo de retorno:**
- **Funciones que devuelven un valor:** Estas usan un tipo de retorno específico como `int`, `string`, etc.
  ```csharp
  public double CalcularPromedio(int suma, int total)
  {
      return (double)suma / total;
  }
  ```

- **Funciones que no devuelven valor (`void`):** Usadas cuando no es necesario retornar algo.
  ```csharp
  public void MostrarMensaje(string mensaje)
  {
      Console.WriteLine(mensaje);
  }
  ```

#### **b) Según los modificadores de acceso:**
- **`public` (Públicas):** Accesibles desde cualquier parte del programa.
- **`private` (Privadas):** Solo accesibles dentro de la clase donde están definidas.
- **`protected` (Protegidas):** Accesibles dentro de la clase y sus derivadas.
- **`internal` (Internas):** Accesibles dentro del mismo ensamblado.

#### **c) Según si pertenecen a la clase o a una instancia:**
- **Métodos estáticos (`static`):** Asociados con la clase, no con una instancia.
  ```csharp
  public static int ObtenerNumeroAleatorio()
  {
      return new Random().Next();
  }
  ```

- **Métodos de instancia:** Operan en objetos específicos de la clase.
  ```csharp
  public string ObtenerDescripcion()
  {
      return $"Este es un libro.";
  }
  ```

#### **d) Según si son síncronas o asíncronas:**
- **Síncronas:** Ejecución estándar, una instrucción tras otra.
  ```csharp
  public int Sumar(int a, int b)
  {
      return a + b;
  }
  ```

- **Asíncronas (`async`):** Usadas para ejecutar código sin bloquear el hilo principal.
  ```csharp
  public async Task DescargarArchivo(string url)
  {
      await Task.Delay(2000); // Simula una descarga
      Console.WriteLine("Archivo descargado.");
  }
  ```

---

### **3. Cómo llamar a una función**
Para usar una función, simplemente la llamamos por su nombre. Dependiendo de si es estática o de instancia, la forma de llamarla será distinta:

#### **Llamada a un método de instancia:**
Se requiere un objeto de la clase:
```csharp
var libro = new Libro();
libro.ObtenerDescripcion();
```

#### **Llamada a un método estático:**
Se llama directamente usando el nombre de la clase:
```csharp
var numero = Random.ObtenerNumeroAleatorio();
```

---

### **4. Beneficios de usar funciones**
- **Reutilización:** Puedes evitar repetir código, reduciendo la redundancia.
- **Mantenimiento:** El código es más fácil de leer y actualizar.
- **Modularidad:** Divide un programa en partes más manejables.
- **Claridad:** Mejoran la comprensión al asignar nombres descriptivos a los bloques de código.

---

### **5. Paralelismo con la tabla en la BDD**
Si volvemos a la analogía de la tabla en una base de datos:

- **Funciones con retornos específicos (`SELECT`):**
  ```csharp
  public static int ContarFilas()
  {
      return filas.Count; // Equivalente a SELECT COUNT(*) FROM tabla;
  }
  ```

- **Funciones que no devuelven valores (`INSERT`/`UPDATE`):**
  ```csharp
  public void InsertarFila(string valor)
  {
      filas.Add(valor);
      // Similar a INSERT INTO tabla VALUES ('valor');
  }
  ```

- **Funciones asíncronas (`Procesos grandes o remotos`):**
  ```csharp
  public async Task ObtenerDatosRemotos()
  {
      await Task.Delay(1000); // Simula una llamada a una API o servicio remoto
      Console.WriteLine("Datos obtenidos.");
  }
  ```

---

### **6. Ejemplo práctico de funciones en un sistema**
Combina funciones estáticas y de instancia para manejar operaciones generales y específicas:

```csharp
using System;
using System.Collections.Generic;

public class Fila
{
    public int Id { get; set; }
    public string Columna { get; set; }

    public void ActualizarColumna(string nuevoValor)
    {
        Columna = nuevoValor;
    }
}

public static class Tabla
{
    private static List<Fila> filas = new List<Fila>();

    public static void InsertarFila(Fila fila)
    {
        filas.Add(fila);
    }

    public static Fila ObtenerFilaPorId(int id)
    {
        return filas.Find(f => f.Id == id);
    }

    public static void MostrarFilas()
    {
        foreach (var fila in filas)
        {
            Console.WriteLine($"Id: {fila.Id}, Columna: {fila.Columna}");
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        Tabla.InsertarFila(new Fila { Id = 1, Columna = "Valor 1" });
        Tabla.InsertarFila(new Fila { Id = 2, Columna = "Valor 2" });

        Console.WriteLine("Todas las filas:");
        Tabla.MostrarFilas();

        var fila = Tabla.ObtenerFilaPorId(1);
        fila.ActualizarColumna("Nuevo Valor");

        Console.WriteLine("Filas después de actualizar:");
        Tabla.MostrarFilas();
    }
}
```

---

### **Conclusión**
Las funciones en C# permiten organizar el código de manera eficiente, combinando lógica reutilizable con estructuras robustas. En el contexto de tu analogía, las funciones pueden actuar como **consultas** o **acciones** sobre la tabla (manejando filas específicas o trabajando en la tabla completa).

---
# 10).- Resumen de clases y metodos.

¡Claro que si John, la tengo re clara! 

Para entender la diferencia entre **clases** y **métodos**, pensemos en cómo trabajan juntos en la programación orientada a objetos, como si estuvieran relacionados dentro de un sistema jerárquico.

---

### **1. ¿Qué es una Clase?**
Una clase es como un **molde o plano** que define las propiedades (atributos) y los comportamientos (métodos) de un objeto. Es una abstracción que agrupa datos y funciones relacionadas en un único concepto.

- **Analogía con una tabla en la BDD:**
  Una clase es como la estructura de una tabla: define las **columnas** (propiedades) y puede incluir métodos que interactúan con los registros. Cada fila en la tabla es un **objeto** basado en esta clase.

- **Características clave de una Clase:**
  - Define datos (propiedades/atributos).
  - Define comportamientos (métodos).
  - Se utiliza para crear objetos.
  - Ejemplo:
    ```csharp
    public class Libro
    {
        // Propiedades: columnas de la tabla
        public string Nombre { get; set; }
        public string Autor { get; set; }

        // Método: comportamiento para los libros
        public void MostrarDetalles()
        {
            Console.WriteLine($"Nombre: {Nombre}, Autor: {Autor}");
        }
    }
    ```

---

### **2. ¿Qué es un Método?**
Un método es una **función** que está definida dentro de una clase y describe un comportamiento o acción que puede realizarse en los objetos de esa clase.

- **Analogía con la tabla en la BDD:**
  Un método es como una operación que puedes realizar en una tabla o fila. Por ejemplo, puedes "seleccionar" datos específicos de una tabla o "actualizar" una fila. Aquí, los métodos trabajan sobre las **propiedades** de las filas (objetos).

- **Características clave de un Método:**
  - Define una acción.
  - Puede trabajar con los datos (propiedades) de un objeto o de la clase.
  - Puede ser llamado para realizar tareas específicas.
  - Ejemplo:
    ```csharp
    public void ActualizarAutor(string nuevoAutor)
    {
        Autor = nuevoAutor; // Cambiar valor de la propiedad Autor
    }
    ```

---

### **3. Diferencias principales**
| **Característica**      | **Clase**                                               | **Método**                                               |
|--------------------------|--------------------------------------------------------|---------------------------------------------------------|
| **Definición**           | Es un molde para crear objetos que agrupa datos y acciones. | Es una acción o comportamiento definido dentro de la clase. |
| **Función principal**    | Contiene las propiedades y los métodos que definen el objeto. | Realiza operaciones sobre las propiedades o datos del objeto. |
| **Estructura**           | Se define como un conjunto de propiedades y métodos.    | Es un bloque de código dentro de la clase que realiza una tarea. |
| **Ejemplo en la BDD**    | Estructura de una tabla con columnas y registros.        | Operaciones como `INSERT`, `SELECT` o `UPDATE` en la tabla. |
| **Reusabilidad**         | Crea múltiples instancias (objetos) basadas en ella.     | Puede llamarse para realizar tareas repetitivas o específicas. |

---

### **4. Ejemplo práctico en conjunto**
Imagina que tienes una biblioteca y necesitas trabajar con libros:

#### Clase (estructura de la tabla):
Define cómo son los libros, con columnas como `Nombre` y `Autor`:

```csharp
public class Libro
{
    public string Nombre { get; set; }
    public string Autor { get; set; }
}
```

#### Métodos (operaciones en la tabla):
Añade acciones que puedes realizar, como mostrar los detalles de un libro o actualizar su autor:

```csharp
public void MostrarDetalles()
{
    Console.WriteLine($"Nombre: {Nombre}, Autor: {Autor}");
}

public void ActualizarAutor(string nuevoAutor)
{
    Autor = nuevoAutor;
}
```

#### Uso:
Con la clase y sus métodos, puedes crear libros y realizar operaciones en ellos:
```csharp
Libro libro1 = new Libro { Nombre = "1984", Autor = "George Orwell" };
libro1.MostrarDetalles(); // Acción: muestra los detalles
libro1.ActualizarAutor("Eric Arthur Blair"); // Actualiza el autor
libro1.MostrarDetalles(); // Verifica los cambios
```

---

### **Conclusión**
- **La clase** es como la plantilla o plano que define cómo se estructuran y funcionan los objetos.
- **El método** es como una herramienta dentro de la clase que describe las acciones que puedes realizar sobre esos objetos.

Ambos son fundamentales para construir aplicaciones orientadas a objetos, y trabajan juntos como el "qué" y el "cómo".

---
# 11).- Funciones vs Métodos en C#: Similaridades y Diferencias

Aunque a menudo las palabras "función" y "método" se usan de manera intercambiable, en C# tienen matices importantes que marcan su diferencia:

---

### **1. ¿Qué son las Funciones?**
- **Definición general:** Una función es un bloque de código reutilizable que realiza una tarea específica y normalmente devuelve un valor. En muchos lenguajes de programación, las funciones pueden existir de forma independiente.
- **En C#:** Todas las funciones se definen dentro de una **clase** o estructura, y por lo tanto se consideran métodos. Sin embargo, cuando se usa "función", a menudo se refiere al concepto general (como en matemáticas o lenguajes funcionales).

---

### **2. ¿Qué son los Métodos?**
- **Definición:** En C#, un método es una **función** asociada a una clase o estructura. Puede operar sobre los datos (propiedades) de una clase, realizar una acción o devolver un valor.
- **C#:** Todos los métodos son funciones, pero no todas las funciones en el sentido matemático o genérico se consideran métodos (porque las funciones siempre están dentro de clases en C#).

---

### **3. Diferencias clave**

| **Característica**          | **Funciones**                            | **Métodos**                              |
|-----------------------------|------------------------------------------|------------------------------------------|
| **Independencia**           | En algunos lenguajes (como C o Python), pueden existir fuera de una clase. | En C#, siempre están dentro de una clase o estructura. |
| **Enfoque en C#**           | Se refiere a un concepto genérico (cálculos, tareas específicas). | Implementación de funciones dentro del paradigma orientado a objetos. |
| **Asociación**              | No necesariamente tienen un contexto asociado (fuera de C#). | Están asociadas a una clase (contexto de instancia o estático). |
| **Acceso a propiedades**    | En otros lenguajes, no manipulan directamente propiedades de objetos. | Pueden operar directamente sobre propiedades y datos de la clase. |
| **Ejemplo en C#: Retorno**  | Una función puede devolver un valor calculado: `return resultado;`. | Un método puede hacer esto y además operar en el estado de un objeto o clase. |

---

### **4. Ejemplo práctico de la relación entre ambos**

#### **Definición como "función" (concepto):**
La siguiente función suma dos números y devuelve el resultado:

```csharp
public static int Sumar(int a, int b)
{
    return a + b;
}
```

#### **Definición como "método" (en una clase):**
Esta misma función puede definirse como un método dentro de una clase, y puede operar con los datos de la clase si no es estática:

```csharp
public class Calculadora
{
    public int Resultado { get; set; }

    public void Sumar(int a, int b)
    {
        Resultado = a + b; // Modifica una propiedad de la clase
    }
}
```

---

### **5. Relaciones y uso en C#**

1. **Funciones dentro de métodos:**

   En C#, las funciones siempre están dentro de clases y, por tanto, son métodos. Sin embargo, puedes pensar en un "método estático" como una función clásica en otros lenguajes.

   Ejemplo:
   ```csharp
   public static double CalcularPromedio(int suma, int total)
   {
       return (double)suma / total; // Esto se comporta como una función tradicional
   }
   ```

2. **Métodos con datos de objetos:**
   Los métodos no estáticos (de instancia) operan en datos específicos del objeto al que pertenecen:

   ```csharp
   public class Libro
   {
       public string Titulo { get; set; }

       public void ActualizarTitulo(string nuevoTitulo)
       {
           Titulo = nuevoTitulo; // Cambia el estado del objeto
       }
   }
   ```

3. **Métodos estáticos como funciones globales:**
   Los métodos estáticos no dependen de una instancia de clase y se comportan más como funciones clásicas:

   ```csharp
   public static class Utils
   {
       public static int CalcularEdad(DateTime fechaNacimiento)
       {
           return DateTime.Now.Year - fechaNacimiento.Year;
       }
   }
   ```

---

### **6. Analizando la analogía de la tabla**
Volviendo a la analogía con la base de datos (BDD):

- Una **función (estática):** Podría contar cuántas filas tiene la tabla:
  ```csharp
  public static int ContarFilas(List<Fila> tabla)
  {
      return tabla.Count;
  }
  ```

- Un **método (de instancia):** Actuaría sobre un registro específico:
  ```csharp
  public void ActualizarColumna(string nuevaColumna)
  {
      Columna = nuevaColumna;
  }
  ```

---

### **Conclusión**
En C#, la distinción entre funciones y métodos radica principalmente en el contexto en el que operan:
- Las **funciones** (en un sentido general) realizan una tarea específica y devuelven un resultado.
- Los **métodos**, en el contexto de C#, son funciones que están asociadas a una clase o estructura y operan dentro del paradigma orientado a objetos.

Ambos son herramientas esenciales, y su elección depende del diseño del sistema y el propósito de la lógica que se implementa.

---
# 12).- Ejemplo vago de un poco de todo lo **algo** que se hablo.

Acá se combina **interfaz**, **herencia**, **métodos**, y **funciones** siguiendo un esquema paso a paso. Este diseño ilustra cómo se conectan los componentes en C# y cómo implementar lo que pediste, incluyendo una función que llama a un método que no devuelve valor.

---

### **Esquema del desarrollo**

#### **Paso 1: Crear una interfaz**
Define los comportamientos comunes que serán implementados por las clases que hereden de ella.

#### **Paso 2: Crear clases parciales**
Permite dividir la implementación de una clase en múltiples archivos para facilitar su gestión.

#### **Paso 3: Métodos y funciones dentro de las clases**
Usamos modificadores como `public`, `private`, `static`, `virtual`, etc., para mostrar diferentes posibilidades de personalización.

#### **Paso 4: Implementar herencia**
Define una jerarquía entre clases para reutilizar y extender funcionalidades.

---

### **Implementación detallada**

#### **1. Crear la interfaz**
Aquí definimos los métodos que las clases deben implementar.

```csharp
public interface IBaseDatos
{
    void Insertar(string registro); // Método sin retorno, para insertar datos
    List<string> ObtenerRegistros(); // Función con retorno, para devolver todos los registros
}
```

---

#### **2. Crear una clase base (usando herencia)**
La clase base implementa la interfaz y proporciona funcionalidades comunes.

```csharp
public abstract class BaseDatos : IBaseDatos
{
    protected List<string> registros = new List<string>(); // Lista para almacenar datos

    // Método de la interfaz para insertar
    public virtual void Insertar(string registro)
    {
        registros.Add(registro);
    }

    // Función para devolver los registros almacenados
    public List<string> ObtenerRegistros()
    {
        return registros;
    }
}
```

---

#### **3. Crear clases parciales**
Dividimos una clase para extenderla más adelante.

Archivo 1 (`ArchivoParcial1.cs`):
```csharp
public partial class BaseDatosAvanzada : BaseDatos
{
    // Método estático para contar registros (a nivel de tabla)
    public static int ContarRegistros(List<string> registros)
    {
        return registros.Count; // Devuelve el número de registros
    }
}
```

Archivo 2 (`ArchivoParcial2.cs`):
```csharp
public partial class BaseDatosAvanzada
{
    // Método sellado que sobrescribe el método de la clase base
    public sealed override void Insertar(string registro)
    {
        base.Insertar(registro); // Llama al método original
    }

    // Función que incluye un método que no devuelve valor
    public string InsertarYConfirmar(string registro)
    {
        Insertar(registro); // Llama al método Insertar (sin retorno)
        return "Se insertó el registro: " + registro; // Retorna un mensaje
    }
}
```

---

#### **4. Crear una clase derivada (herencia avanzada)**
Extendemos la funcionalidad de la clase avanzada.

```csharp
public class BaseDatosEspecializada : BaseDatosAvanzada
{
    // Método asíncrono para operaciones largas (ejemplo: simulación de sincronización)
    public async Task SincronizarConServidor()
    {
        await Task.Delay(2000); // Simula una espera de sincronización
        Console.WriteLine("Sincronización completada con el servidor.");
    }
}
```

---

### **Flujo completo en el programa principal**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        // Paso 1: Crear una instancia de la clase especializada
        var baseDatos = new BaseDatosEspecializada();

        // Paso 2: Insertar datos y confirmar la operación
        string resultado = baseDatos.InsertarYConfirmar("Libro: El Quijote");
        Console.WriteLine(resultado);

        // Paso 3: Contar registros (usando el método estático)
        int totalRegistros = BaseDatosAvanzada.ContarRegistros(baseDatos.ObtenerRegistros());
        Console.WriteLine($"Total de registros: {totalRegistros}");

        // Paso 4: Sincronizar con el servidor (asíncrono)
        await baseDatos.SincronizarConServidor();
    }
}
```

---

### **Análisis del flujo**
1. **Interfaz (`IBaseDatos`):**
   - Define los contratos que deben cumplir las clases (`Insertar` y `ObtenerRegistros`).

2. **Clase base (`BaseDatos`):**
   - Implementa la interfaz y proporciona una implementación básica de los métodos.

3. **Clases parciales (`BaseDatosAvanzada`):**
   - Divide la funcionalidad en múltiples archivos para mayor claridad.
   - Implementa métodos estáticos, sellados, y una función que incluye un método sin retorno.

4. **Clase especializada (`BaseDatosEspecializada`):**
   - Agrega funcionalidades avanzadas, como métodos asíncronos para tareas más complejas.

5. **Método en una función:**
   - El método `Insertar` (sin retorno) se utiliza dentro de la función `InsertarYConfirmar` (que tiene un retorno).

---

### **Resultado esperado** (ponele)
Al ejecutar el programa:
1. Se inserta un registro y se imprime un mensaje confirmando la inserción.
2. Se cuenta el número de registros en la base de datos.
3. Se simula una sincronización con el servidor.

---

Este ejemplo combina **interfaz, herencia, métodos y funciones** siguiendo un flujo lógico y mostrando las posibilidades de diseño.

---
## 13).- Ahora estás listo para decir que no quieres programar en C#

# :rofl:
