---
title: Entwickeln von .NET Standard-Funktionen für Azure Stream Analytics-Aufträge (Vorschau)
description: Erfahren Sie, wie Sie benutzerdefinierte C#-Funktionen für Stream Analytics-Aufträge schreiben.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/10/2020
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 9cf929a3a6f5b3752b030f449b3b24b2bdc941a1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90907337"
---
# <a name="develop-net-standard-user-defined-functions-for-azure-stream-analytics-jobs-preview"></a>Entwickeln von benutzerdefinierten .NET Standard-Funktionen für Azure Stream Analytics-Aufträge (Vorschau)

Azure Stream Analytics bietet eine SQL-ähnliche Abfragesprache zum Durchführen von Transformationen und Berechnungen über Datenströme von Ereignisdaten. Es gibt viele integrierte Funktionen, für einige komplexere Szenarien ist jedoch zusätzliche Flexibilität erforderlich. Mit benutzerdefinierten Funktionen (UDF) in .NET Standard können Sie Ihre eigenen Funktionen in einer beliebigen .NET Standard-Sprache (C#, F# usw.) schreiben, um die Stream Analytics-Abfragesprache zu erweitern. UDFs ermöglichen das Durchführen komplexer mathematischer Berechnungen, das Importieren benutzerdefinierter ML-Modelle mit ML.NET und das Verwenden von benutzerdefinierter eingefügter Logik für die fehlenden Daten. Das UDF-Feature für Stream Analytics-Aufträge befindet sich derzeit in der Vorschauphase und sollte nicht für Produktionsworkloads verwendet werden.

Das .NET-UDF-Feature für Cloudaufträge ist in den folgenden Regionen verfügbar:
* USA, Westen-Mitte
* Nordeuropa
* East US
* USA (Westen)
* USA (Ost) 2
* Europa, Westen

Wenn Sie dieses Feature in einer anderen Region verwenden möchten, können Sie den [Zugriff anfordern](https://aka.ms/ccodereqregion).

## <a name="package-path"></a>Paketpfad

Der Pfad aller UDF-Pakete hat das Format `/UserCustomCode/CLR/*`. Dynamic Link Libraries (DLLs) und Ressourcen werden in den Ordner `/UserCustomCode/CLR/*` kopiert. Damit werden die Benutzer-DLLs von den System- und Azure Stream Analytics-DLLs isoliert. Dieser Paketpfad wird für alle Funktionen genutzt, unabhängig davon, mit welcher Methode sie verwendet werden.

## <a name="supported-types-and-mapping"></a>Unterstützte Typen und Zuordnungen

Damit Azure Stream Analytics-Werte in C# verwendet werden können, müssen sie von einer Umgebung in die andere übertragen werden. Das Marshalling erfolgt für alle Eingabeparameter einer UDF. Jeder Azure Stream Analytics-Typ verfügt über einen entsprechenden Typ in C#, der in der nachstehenden Tabelle aufgeführt ist:

|**Azure Stream Analytics-Typ** |**C#-Typ** |
|---------|---------|
|BIGINT | long |
|float | double |
|nvarchar(max) | Zeichenfolge |
|datetime | Datetime |
|Datensatz | Wörterbuch\<string, object> |
|Array | Objekt[] |

Dasselbe gilt, wenn Daten von C# nach Azure Stream Analytics gemarshallt werden müssen, was beim Ausgabewert einer UDF erfolgt. Die folgende Tabelle zeigt, welche Typen unterstützt werden:

|**C#-Typ**  |**Azure Stream Analytics-Typ**  |
|---------|---------|
|long  |  BIGINT   |
|double  |  float   |
|Zeichenfolge  |  nvarchar(max)   |
|Datetime  |  dateTime   |
|struct  |  Datensatz   |
|Objekt (object)  |  Datensatz   |
|Objekt[]  |  Array   |
|Wörterbuch\<string, object>  |  Datensatz   |

## <a name="develop-a-udf-in-visual-studio-code"></a>Entwickeln einer UDF in Visual Studio Code

[Visual Studio Code-Tools für Azure Stream Analytics](quick-create-visual-studio-code.md) vereinfachen das Verfassen von UDFs, das lokale Testen Ihrer Aufträge (sogar offline) und das Veröffentlichen Ihres Stream Analytics-Auftrags in Azure.

Es gibt zwei Möglichkeiten, .NET Standard-UDFs in Visual Studio Code-Tools zu implementieren.

* UDFs aus lokalen DLLs
* Benutzerdefinierte Funktion aus einem lokalen Projekt

### <a name="local-project"></a>Lokales Projekt

Benutzerdefinierte Funktionen können in einer Assembly geschrieben werden, auf die später in einer Azure Stream Analytics-Abfrage verwiesen wird. Dies ist die empfohlene Option für komplexe Funktionen, die das ganze Potenzial einer .NET Standard-Sprache über die Ausdruckssprache erfordern, z.B. bei prozeduraler Logik oder Rekursion. Sie können benutzerdefinierte Funktionen aus einem lokalen Projekt auch verwenden, wenn Sie die Logik der Funktion für mehrere Azure Stream Analytics-Abfragen verwenden möchten. Durch das Hinzufügen von UDFs zu Ihrem lokalen Projekt haben Sie die Möglichkeit, Ihre Funktionen lokal zu debuggen und zu testen.

So verweisen Sie auf ein lokales Projekt

1. Erstellen Sie eine neue .NET Standard-Klassenbibliothek auf dem lokalen Computer.
2. Schreiben Sie Code in Ihrer Klasse. Beachten Sie, dass die Klassen als *öffentlich* und die Objekte als *statisch öffentlich* definiert werden müssen.
3. Fügen Sie in Ihrem Azure Stream Analytics-Projekt eine neue CSharp-Funktionskonfigurationsdatei hinzu, und verweisen Sie auf das CSharp-Klassenbibliotheksprojekt.
4. Konfigurieren Sie den Assemblypfad im Abschnitt **CustomCodeStorage** der Auftragskonfigurationsdatei `JobConfig.json`. Dieser Schritt ist für lokale Tests nicht erforderlich.

### <a name="local-dlls"></a>Lokale DLLs

Sie können auch auf lokale DLLs verweisen, die die benutzerdefinierten Funktionen enthalten.

### <a name="example"></a>Beispiel

In diesem Beispiel ist **CSharpUDFProject** ein C#-Klassenbibliotheksprojekt, und **ASAUDFDemo** ist das Azure Stream Analytics-Projekt, das auf **CSharpUDFProject** verweist.

:::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/visual-studio-code-csharp-udf-demo.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

Die folgende UDF multipliziert eine ganze Zahl mit sich selbst, um das Quadrat der ganzen Zahl zu erzeugen. Die Klassen müssen als *öffentlich* und die Objekte als *statisch öffentlich* definiert werden.

```csharp
using System;

namespace CSharpUDFProject
{
    // 
    public class Class1
    {
        public static Int64 SquareFunction(Int64 a)
        {
            return a * a;
        }
    }
}
```

In den folgenden Schritten wird gezeigt, wie Sie Ihrem Stream Analytics-Projekt eine C#-UDF hinzufügen.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **Funktionen**, und wählen Sie **Element hinzufügen** aus.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/visual-studio-code-csharp-udf-add-function.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

2. Fügen Sie Ihrem Azure Stream Analytics-Projekt die C#-Funktion **SquareFunction** hinzu.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/visual-studio-code-csharp-udf-add-function-2.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/visual-studio-code-csharp-udf-add-function-name.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

3. Wählen Sie in der Konfiguration der C#-Funktion **Choose library project path** (Bibliotheksprojektpfad auswählen) aus, um Ihr C#-Projekt in der Dropdownliste auszuwählen, und wählen Sie **Projekt erstellen** aus, um das Projekt zu erstellen. Wählen Sie dann **Klasse auswählen** und **Methode auswählen** aus, um den zugehörigen Klassennamen und Methodennamen aus der Dropdownliste auszuwählen. Für Verweise auf die Methoden, Typen und Funktionen in der Stream Analytics-Abfrage müssen die Klassen als *öffentlich* und die Objekte als *statisch öffentlich* definiert sein.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/visual-studio-code-csharp-udf-choose-project.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

    Wenn Sie die C#-UDF aus einer DLL verwenden möchten, wählen Sie **Choose library dll path** (DLL-Bibliothekspfad auswählen) aus, um die DLL auszuwählen. Wählen Sie dann **Klasse auswählen** und **Methode auswählen** aus, um den zugehörigen Klassennamen und Methodennamen aus der Dropdownliste auszuwählen.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/visual-studio-code-csharp-udf-choose-dll.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

4. Rufen Sie die UDF in der Azure Stream Analytics-Abfrage auf.

   ```sql
    SELECT price, udf.SquareFunction(price)
    INTO Output
    FROM Input 
   ```

5. Bevor Sie den Auftrag an Azure übermitteln, konfigurieren Sie den Paketpfad im Abschnitt **CustomCodeStorage** der Auftragskonfigurationsdatei `JobConfig.json`. Verwenden Sie **Aus Ihrem Abonnement auswählen**  in CodeLens, um Ihr Abonnement auszuwählen, und wählen Sie in der Dropdownliste den Speicherkonto- und Containernamen aus. Übernehmen Sie den Standardwert von **Pfad**. Dieser Schritt ist für lokale Tests nicht erforderlich.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/visual-studio-code-csharp-udf-configure-storage-account.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

## <a name="develop-a-udf-in-visual-studio"></a>Entwickeln einer UDF in Visual Studio

Zum Implementieren von UDFs in Visual Studio-Tools stehen drei Methoden zur Auswahl.

* CodeBehind-Dateien in einem ASA-Projekt
* Benutzerdefinierte Funktion aus einem lokalen Projekt
* Vorhandenes Paket aus einem Azure Storage-Konto

### <a name="codebehind"></a>CodeBehind

Sie können benutzerdefinierte Funktionen im CodeBehind **Script.asql** schreiben. Visual Studio-Tools kompilieren automatisch die CodeBehind-Datei in eine Assemblydatei. Die Assemblys werden als ZIP-Datei verpackt und in Ihr Speicherkonto hochgeladen, wenn Sie Ihren Auftrag an Azure übermitteln. Im Tutorial [C#-UDF für Stream Analytics-Edge-Aufträge](stream-analytics-edge-csharp-udf.md) erfahren Sie, wie Sie eine UDF in C# mit CodeBehind schreiben. 

### <a name="local-project"></a>Lokales Projekt

So verweisen Sie auf ein lokales Projekt in Visual Studio

1. Erstellen einer neuen .NET Standard-Klassenbibliothek in Ihrer Projektmappe
2. Schreiben Sie Code in Ihrer Klasse. Beachten Sie, dass die Klassen als *öffentlich* und die Objekte als *statisch öffentlich* definiert werden müssen. 
3. Erstellen Sie Ihr Projekt. Die Tools verpacken alle Artefakte im Papierkorbordner als ZIP-Datei und laden die ZIP-Datei im Speicherkonto hoch. Verwenden Sie für externe Referenzen die Assemblyreferenz anstelle des NuGet-Pakets.
4. Verweisen Sie in Ihrem Azure Stream Analytics-Projekt auf die neue Klasse.
5. Fügen Sie in Ihrem Azure Stream Analytics-Projekt eine neue Funktion hinzu.
6. Konfigurieren Sie den Pfad der Assembly in der Konfigurationsdatei des Auftrags (`JobConfig.json`). Legen Sie den Assemblypfad auf **Verweis auf lokales Projekt oder CodeBehind** fest.
7. Erstellen Sie sowohl das Funktionsprojekt als auch das Azure Stream Analytics-Projekt neu.  

### <a name="example"></a>Beispiel

In diesem Beispiel ist **UDFTest** ein C#-Klassenbibliotheksprojekt, und **ASAUDFDemo** ist das Azure Stream Analytics-Projekt, das auf **UDFTest** verweist.

:::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-demo.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

1. Erstellen Sie Ihr C#-Projekt, mit dem Sie einen Verweis auf Ihre benutzerdefinierte C#-Funktion aus der Azure Stream Analytics-Abfrage hinzufügen können.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-build-project.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

2. Fügen Sie den Verweis auf das C#-Projekt im ASA-Projekt hinzu. Klicken Sie mit der rechten Maustaste auf den Knoten „Verweise“, und wählen Sie „Verweis hinzufügen“ aus.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-reference.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

3. Wählen Sie in der Liste den Namen des C#-Projekts aus.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-choose-project-name.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

4. **UDFTest** sollte unter **Verweise** im **Projektmappen-Explorer** aufgeführt werden.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-added-reference.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

5. Klicken Sie mit der rechten Maustaste auf den Ordner **Funktionen**, und wählen Sie **Neues Element** aus.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

6. Fügen Sie Ihrem Azure Stream Analytics-Projekt die C#-Funktion **SquareFunction.json** hinzu.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-add-csharp-function-2.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

7. Doppelklicken Sie im **Projektmappen-Explorer** auf die Funktion, um das Konfigurationsdialogfeld zu öffnen.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-csharp-function-config.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

8. Wählen Sie in der Konfiguration der C#-Funktion **Aus ASA-Projektverweis laden** und dann in der Dropdownliste die zugehörigen Assembly-, Klassen- und Methodennamen aus. Für Verweise auf die Methoden, Typen und Funktionen in der Stream Analytics-Abfrage müssen die Klassen als *öffentlich* und die Objekte als *statisch öffentlich* definiert sein.

   :::image type="content" source="media/stream-analytics-edge-csharp-udf-methods/stream-analytics-edge-udf-asa-csharp-function-config.png" alt-text="Azure Stream Analytics-Projektdateien in Visual Studio Code":::

## <a name="existing-packages"></a>Vorhandene Pakete

Sie können .NET Standard-UDFs in einer IDE Ihrer Wahl erstellen und sie dann über Ihre Azure Stream Analytics-Abfrage aufrufen. Kompilieren Sie zunächst Ihren Code, und packen Sie alle DLLs. Der Pfad des Pakets hat das Format `/UserCustomCode/CLR/*`. Laden Sie dann `UserCustomCode.zip` in das Stammverzeichnis des Containers in Ihrem Azure Storage-Konto hoch.

Nachdem die ZIP-Pakete der Assembly in Ihr Azure Storage-Konto hochgeladen wurden, können Sie die Funktionen in Azure Stream Analytics-Abfragen verwenden. Sie müssen lediglich die Storage-Informationen in der Konfiguration des Stream Analytics-Auftrags einfügen. Sie können die Funktion mit dieser Option nicht lokal testen, da Ihr Paket von den Visual Studio-Tools nicht herunterladen wird. Der Paketpfad wird vom Dienst analysiert. 

Konfigurieren Sie den Assemblypfad in der Konfigurationsdatei des Auftrags (`JobConfig.json`).

Erweitern Sie den Abschnitt mit der **benutzerdefinierten Codekonfiguration**, und geben Sie die folgenden vorgeschlagenen Werte für die Konfiguration an:

   |**Einstellung**|**Empfohlener Wert**|
   |-------|---------------|
   |Global Storage Settings Resource (Ressource für globale Speichereinstellungen)|Datenquelle aus aktuellem Konto wählen|
   |Global Storage Settings Subscription (Abonnement für globale Speichereinstellungen)| <Ihr Abonnement>|
   |Global Storage Settings Storage Account (Speicherkonto für globale Speichereinstellungen)| <Ihr Speicherkonto>|
   |Custom Code Storage Settings Resource (Ressource für Speichereinstellungen für benutzerdefinierten Code)|Datenquelle aus aktuellem Konto wählen|
   |Custom Code Storage Settings Storage Account (Speicherkonto für Speichereinstellungen für benutzerdefinierten Code)|<Ihr Speicherkonto>|
   |Custom Code Storage Settings Container (Container für Speichereinstellungen für benutzerdefinierten Code)|<Ihr Speichercontainer>|
   |Custom Code Assembly Source (Assemblyquelle für benutzerdefinierten Code)|Vorhandene Assemblypakete aus der Cloud|
   |Custom Code Assembly Source (Assemblyquelle für benutzerdefinierten Code)|UserCustomCode.zip|

## <a name="user-logging"></a>Benutzerprotokollierung

Mit dem Protokollierungsmechanismus können Sie benutzerdefinierte Informationen erfassen, während ein Auftrag ausgeführt wird. Protokolldaten können bspw. verwendet werden, um in Echtzeit den benutzerdefinierten Code zu debuggen oder dessen die Richtigkeit zu bewerten.

Die Klasse `StreamingContext` ermöglicht es Ihnen, Diagnoseinformationen mithilfe der Funktion `StreamingDiagnostics.WriteError` zu veröffentlichen. Der folgende Code zeigt die Schnittstelle, die von Azure Stream Analytics verfügbar gemacht wird.

```csharp
public abstract class StreamingContext
{
    public abstract StreamingDiagnostics Diagnostics { get; }
}

public abstract class StreamingDiagnostics
{
    public abstract void WriteError(string briefMessage, string detailedMessage);
}
```

`StreamingContext` wird als Eingabeparameter an die UDF-Methode übergeben und kann darin zum Veröffentlichen benutzerdefinierter Protokollinformationen verwendet werden. Im folgenden Beispiel definiert `MyUdfMethod` eine Dateneingabe (**data**), die von der Abfrage bereitgestellt wird, und eine Kontexteingabe (**context**) als `StreamingContext`, die von der Runtime-Engine bereitgestellt wird. 

```csharp
public static long MyUdfMethod(long data, StreamingContext context)
{
    // write log
    context.Diagnostics.WriteError("User Log", "This is a log message");
    
    return data;
}
```

Der Wert `StreamingContext` muss nicht von der SQL-Abfrage übergeben werden. Azure Stream Analytics stellt automatisch ein Kontextobjekt bereit, wenn ein Eingabeparameter vorhanden ist. Die Verwendung von `MyUdfMethod` ändert sich nicht. Dies wird in der folgenden Abfrage veranschaulicht:

```sql
SELECT udf.MyUdfMethod(input.value) as udfValue FROM input
```

Sie können über die [Diagnoseprotokolle](data-errors.md) auf Protokollmeldungen zugreifen.

## <a name="limitations"></a>Einschränkungen

Für die Vorschauversion für benutzerdefinierte Funktionen gelten derzeit folgende Einschränkungen:

* .NET Standard-UDFs können nur in Visual Studio Code oder Visual Studio erstellt und in Azure veröffentlicht werden. Schreibgeschützte Versionen von .NET Standard-UDFs können im Azure-Portal unter **Funktionen** angezeigt werden. Das Erstellen von .NET Standard-Funktionen wird im Azure-Portal nicht unterstützt.

* Der Abfrage-Editor im Azure-Portal zeigt einen Fehler an, wenn Sie eine .NET Standard-UDF im Portal verwenden. 

* Da der benutzerdefinierte Code den Kontext mit der Azure Stream Analytics-Engine gemeinsam nutzt, kann benutzerdefinierter Code nicht auf Elemente verweisen, deren Namespace oder DLL-Name in Konflikt mit Azure Stream Analytics-Code steht. Sie können beispielsweise nicht auf *Newtonsoft Json* verweisen.

* Im Projekt enthaltene unterstützende Dateien werden in die ZIP-Datei mit dem benutzerdefinierten Code kopiert, die verwendet wird, wenn Sie den Auftrag in der Cloud veröffentlichen. Alle Dateien in Unterordnern werden beim Entpacken direkt in den Stammordner für den benutzerdefinierten Code in der Cloud kopiert. Die ZIP-Datei wird beim Dekomprimieren „vereinfacht“.

* Benutzerdefinierter Code unterstützt keine leeren Ordner. Fügen Sie den unterstützenden Dateien im Projekt keine leeren Ordner hinzu.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Schreiben einer benutzerdefinierten C#-Funktion für einen Azure Stream Analytics-Auftrag (Vorschauversion)](stream-analytics-edge-csharp-udf.md)
* [Tutorial: Azure Stream Analytics – benutzerdefinierte JavaScript-Funktionen](stream-analytics-javascript-user-defined-functions.md)
* [Erstellen eines Azure Stream Analytics-Auftrags in Visual Studio Code](quick-create-visual-studio-code.md)