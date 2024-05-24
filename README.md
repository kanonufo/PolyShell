# PolyShell: Ejecución de Shellcode Polimórfico

PolyShell es una técnica avanzada de ejecución de shellcode que utiliza el polimorfismo para evadir la detección de las soluciones de seguridad. Esta técnica aprovecha diversas estrategias, como DInvoke (Dynamic Invocation), descarga de shellcode desde Internet, asignación de memoria no convencional y ejecución de código desde hilos de fibra, para eludir las defensas tradicionales y ejecutar código malicioso de manera sigilosa.

## Características Principales:

- **DInvoke (Dynamic Invocation):** PolyShell utiliza DInvoke para invocar funciones de la API de Windows de manera dinámica.
  
- **Descarga de Shellcode desde Internet:** PolyShell descarga el shellcode de un servidor remoto, dificultando su detección durante el análisis estático.
  
- **Asignación de Memoria No Convencional:** PolyShell utiliza llamadas al sistema menos comunes para asignar memoria en el proceso, evitando las técnicas de detección convencionales.
  
- **Ejecución de Código desde Hilos de Fibra:** PolyShell ejecuta el shellcode en hilos de fibra para camuflar su ejecución y evitar la detección.

## Uso:

Para utilizar PolyShell, sigue estos pasos:

1. **Descargar el Repositorio:** Clona este repositorio en tu máquina local.

2. **Compilar el Código:** Abre el proyecto en tu IDE preferido y compila el código fuente de PolyShell.

3. **Ejecutar el Binario:** Ejecuta el binario compilado en una máquina de prueba o en un entorno controlado.

4. **Observa el Comportamiento:** Observa cómo PolyShell evade las defensas de seguridad y ejecuta el shellcode de manera sigilosa.

## Fiber Shellcode:

Fiber Shellcode es una técnica que implica la ejecución de shellcode en un hilo de fibra en lugar de un hilo tradicional. Esto ayuda a evadir la detección, ya que los hilos de fibra son menos monitoreados por los mecanismos de seguridad convencionales. El código a continuación muestra cómo se ejecuta el shellcode en un hilo de fibra:

```csharp
// Función para ejecutar shellcode en un hilo de fibra
static void FiberShellcode()
{
    // Código para descargar y ejecutar el shellcode...
}


Descargo de Responsabilidad:
Descargo de Responsabilidad: PolyShell es una herramienta de investigación diseñada para demostrar técnicas avanzadas de evasión de seguridad. El uso de PolyShell con fines maliciosos está estrictamente prohibido. Los autores y los contribuyentes no se hacen responsables del uso indebido de esta herramienta.


