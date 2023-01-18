# CS_LecturaYEscrituraDeArc.NET-E1
Ejercicio: Lectura de archivos y escritura en ellos

## **Adición de Json.NET al proyecto**

1. Use el terminal para agregar *Json.NET* al proyecto.

```bash
dotnet add package Newtonsoft.Json
```

## **Preparación para los datos de ventas**

1. En la parte superior de `Program.cs`, agregue `using Newtonsoft.Json`:

```csharp
using Newtonsoft.Json;
```

En `Program.cs`
, directamente debajo del método `FindFiles`
, [agregue un nuevo objeto `record`](https://learn.microsoft.com/es-es/dotnet/csharp/language-reference/builtin-types/record/)
 que modelará los datos de *sales.json*
:

```csharp
record SalesData (double Total);
```

## **Creación de un método para calcular totales de ventas**

1. En `Program.cs`, justo antes de la línea `record` que agregó en el paso anterior, cree una función que calculará el total de ventas. Este método debe tomar un `IEnumerable<string>` de rutas de acceso de archivo en el que pueda iterar.

```csharp
double CalculateSalesTotal(IEnumerable<string> salesFiles)
{
    double salesTotal = 0;

    // READ FILES LOOP

    return salesTotal;
}
```

En este método, reemplace `// READ FILES LOOP`
 por un bucle que itere en `salesFiles`
, lea el archivo, analice el contenido como JSON y, después, aumente la variable `salesTotal`
 con el valor `total`
 del archivo:

```csharp
double CalculateSalesTotal(IEnumerable<string> salesFiles)
{
    double salesTotal = 0;

    // Loop over each file path in salesFiles
    foreach (var file in salesFiles)
    {      
        // Read the contents of the file
        string salesJson = File.ReadAllText(file);

        // Parse the contents as JSON
        SalesData? data = JsonConvert.DeserializeObject<SalesData?>(salesJson);

        // Add the amount found in the Total field to the salesTotal variable
        salesTotal += data?.Total ?? 0;
    }

    return salesTotal;
}
```

## **Llamada al método CalculateSalesTotals**

1. En el archivo `Program.cs`, agregue una llamada a la función `CalculateSalesTotal` justo encima de la llamada a `File.WriteAllText`:

```csharp
var currentDirectory = Directory.GetCurrentDirectory();
var storesDir = Path.Combine(currentDirectory, "stores");

var salesTotalDir = Path.Combine(currentDirectory, "salesTotalDir");
Directory.CreateDirectory(salesTotalDir);

var salesFiles = FindFiles(storesDir);

var salesTotal = CalculateSalesTotal(salesFiles); // Add this line of code

File.WriteAllText(Path.Combine(salesTotalDir, "totals.txt"), String.Empty);
```

## **Escritura del total en el archivo totals.txt**

1. En el archivo `Program.cs`, modifique el bloque `File.WriteAllText` para escribir el valor de la variable `salesTotal` en el archivo *totals.txt*. Durante el proceso, cambie la llamada `File.WriteAllText` a `File.AppendAllText` para que no se sobrescriba nada en el archivo.

```csharp
var currentDirectory = Directory.GetCurrentDirectory();            
var storesDir = Path.Combine(currentDirectory, "stores");

var salesTotalDir = Path.Combine(currentDirectory, "salesTotalDir");
Directory.CreateDirectory(salesTotalDir);            

var salesFiles = FindFiles(storesDir);

var salesTotal = CalculateSalesTotal(salesFiles);

File.AppendAllText(Path.Combine(salesTotalDir, "totals.txt"), $"{salesTotal}{Environment.NewLine}");
```

## **Ejecución del programa**

1. Ejecute el programa desde el terminal:

```csharp
dotnet run
```

Vuelva a ejecutar el programa desde el terminal.

```csharp
dotnet run
```

1. Seleccione el archivo *salesTotalDir/totals.txt*.
    
    El archivo *totals.txt* ahora tiene una segunda línea. Cada vez que el programa se ejecute, los totales se volverán a sumar y se escribirá una nueva línea en el archivo.

[![Lectura-Yescritura1.png](https://i.postimg.cc/52WF1PwG/Lectura-Yescritura1.png)](https://postimg.cc/N5b0kkC7)

[![Lectura-Yescritura2.png](https://i.postimg.cc/kgNR5xs5/Lectura-Yescritura2.png)](https://postimg.cc/mPDgX10x)

![image](https://user-images.githubusercontent.com/104856701/213238578-1cbdd84e-9327-4c74-9b39-f00c51f8b39d.png)
