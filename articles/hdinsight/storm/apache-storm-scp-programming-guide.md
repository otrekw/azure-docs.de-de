---
title: SCP.NET-Programmierleitfaden für Storm in Azure HDInsight
description: Erfahren Sie, wie Sie mit SCP.NET .NET-basierte Storm-Topologien für die Verwendung mit Storm zur Ausführung in Azure HDInsight erstellen.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive, devx-track-csharp
ms.date: 01/13/2020
ms.openlocfilehash: c993b3f70f609fb79c51ba9be08fa3d5dc7e8317
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104864107"
---
# <a name="scp-programming-guide-for-apache-storm-in-azure-hdinsight"></a>SCP-Programmierleitfaden für Apache Storm in Azure HDInsight

SCP ist eine Plattform zur Erstellung zuverlässiger, konsistenter und leistungsfähiger Anwendungen für die Datenverarbeitung in Echtzeit. Sie basiert auf [Apache Storm](https://storm.incubator.apache.org/), einem von den Open-Source-Software-Communitys entwickelten System für die Datenstromverarbeitung. Storm wurde von Nathan Marz erstellt. Es wurde von Twitter als Open-Source veröffentlicht. Storm verwendet [Apache ZooKeeper](https://zookeeper.apache.org/), ein weiteres Apache-Projekt für hochzuverlässige verteilte Koordinierung und Zustandsverwaltung.

Das SCP-Projekt hat nicht nur Storm auf Windows portiert, sondern auch projektbezogene Erweiterungen und Anpassungen für die Windows-Umgebung vorgenommen. Die Erweiterungen umfassen die .NET-Benutzeroberfläche für Entwickler und die .NET-Bibliotheken. Die Anpassung umfasst die Windows-basierte Bereitstellung.

Mit den Erweiterungen und Anpassungen brauchen Sie die Open-Source-Softwareprojekte nicht zu forken. Sie können abgeleitete Umgebungen verwenden, die auf Storm aufgebaut sind.

## <a name="processing-model"></a>Verarbeitungsmodell

Die Daten in SCP sind als fortlaufende Streams von Tupeln modelliert. In der Regel gilt Folgendes für die Tupel:

1. Sie gelangen in eine Warteschlange.
1. Sie werden von der in einer Storm-Topologie gehosteten Geschäftslogik aufgenommen und transformiert.
1. Ihre Ausgabe wird entweder als Tupel an ein anderes SCP-System weitergeleitet oder für sie wird ein Commit ausgeführt wie für verteilte Dateisysteme und Datenbanken wie SQL Server.

:::image type="content" source="./media/apache-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png" alt-text="Diagramm mit einer Warteschlange mit Weiterleitung der Daten zur Verarbeitung, die wiederum einen Datenspeicher versorgt" border="false":::

Eine Anwendungstopologie in Storm definiert einen Berechnungsgraphen. Jeder Knoten in einer Topologie enthält Verarbeitungslogik. Verknüpfungen zwischen Knoten zeigen den Datenfluss an.

Knoten, die Eingabedaten in die Topologie einfügen, werden als _Spout_ bezeichnet. Sie können sie zur Sequenzierung der Daten verwenden. Die Eingabedaten könnten aus einer Quelle wie Dateiprotokollen, einer Transaktionsdatenbank oder einem Systemleistungsindikator stammen.

Knoten, die sowohl Eingabe- als auch Ausgabedatenflüsse aufweisen, werden als _Bolt_ bezeichnet. Sie führen die eigentliche Datenfilterung, Auswahl und Aggregation durch.

SCP unterstützt die Datenverarbeitung nach den Prinzipien „Best-Effort“, „At-Least-Once“ und „Exactly Once“.

In einer verteilten Anwendung zur Datenstromverarbeitung können bei der Datenverarbeitung Fehler auftreten. Zu solchen Fehlern gehören ein Netzwerkausfall, ein Computerfehler oder ein Fehler in Ihrem Code. Mit dem „At-Least-Once“-Modell wird sichergestellt, dass alle Daten mindestens einmal verarbeitet werden, indem im Fehlerfall dieselben Daten automatisch erneut abgespielt werden.

Das „At-Least-Once“-Verarbeitungsmodell ist einfach und zuverlässig und eignet sich für zahlreiche Anwendungen. Wenn aber eine exakte Zählung erforderlich ist, reicht das „At-least-once“-Modell nicht aus, da dieselben Daten mehr als einmal in der Anwendungstopologie erneut wiedergegeben werden könnten. Für diese Fälle stellt das „Exactly-Once“-Verarbeitungsmodell ein korrektes Ergebnis sicher, selbst wenn die Daten mehrfach wiederholt und verarbeitet wurden.

Mit SCP können .NET-Entwickler Anwendungen für die Datenverarbeitung in Echtzeit erstellen, während sie eine Java Virtual Machine (JVM) mit Storm verwenden. Die JVM und .NET kommunizieren über lokale TCP-Sockets. Jeder Spout/Bolt ist genauer gesagt ein .NET-/Java-Prozesspaar, wobei die Benutzerlogik in einem .NET-Prozess als Plug-In ausgeführt wird.

Führen Sie die folgenden Schritte aus, um eine Datenverarbeitungsanwendung mit SCP zu erstellen:

1. Entwurf und Implementierung der Spouts zum Abrufen von Daten aus Warteschlangen.
1. Entwurf und Implementierung der Bolts, die Eingangsdaten verarbeiten und in externen Speichern speichern, z. B. in einer Datenbank.
1. Entwurf der Topologie und anschließendes Übermitteln und Ausführen der Topologie.

Die Topologie definiert Scheitelpunkte und die Daten, die zwischen ihnen übertragen werden. SCP nimmt eine Topologiespezifikation und stellt sie in einem Storm-Cluster bereit, in dem jeder dieser Scheitelpunkte in einem logischen Knoten ausgeführt wird. Der Storm-Taskplaner kümmert sich um Failover und Skalierung.

In diesem Artikel werden einige einfache Beispiele verwendet, um die Erstellung von Datenverarbeitungsanwendungen mit SCP zu veranschaulichen.

## <a name="scp-plug-in-interface"></a>SCP-Plug-In-Schnittstelle

SCP-Plug-Ins sind eigenständige Anwendungen. Sie können während der Entwicklung in Visual Studio ausgeführt und nach der Bereitstellung in der Produktionsumgebung in die Storm-Pipeline eingebunden werden.

Das Schreiben eines SCP-Plug-Ins ist dasselbe wie das Schreiben einer beliebigen anderen Windows-Konsolenanwendung. Die SCP.NET-Plattform deklariert einige Schnittstellen für Spout/Bolt. Ihr Plug-In-Code implementiert diese Schnittstellen. Der Hauptzweck dieses Entwurfs besteht darin, dass Sie sich auf Ihre Geschäftslogik konzentrieren können, während die SCP.NET-Plattform andere Aufgaben übernehmen kann.

Ihr Plug-In-Code implementiert eine der folgenden Schnittstellen. Welche Schnittstelle implementiert wird, hängt davon ab, ob die Topologie transaktional oder nicht transaktional ist und ob die Komponente ein Spout oder Bolt ist.

* **ISCPSpout**
* **ISCPBolt**
* **ISCPTxSpout**
* **ISCPBatchBolt**

### <a name="iscpplugin"></a>ISCPPlugin

**ISCPPlugin** ist die gemeinsame Schnittstelle für viele Plug-Ins. Derzeit ist es eine Dummy-Schnittstelle.

```csharp
public interface ISCPPlugin
{
}
```

### <a name="iscpspout"></a>ISCPSpout

**ISCPSpout** ist die Schnittstelle für nicht transaktionale Spouts.

```csharp
public interface ISCPSpout : ISCPPlugin
{
    void NextTuple(Dictionary<string, Object> parms);
    void Ack(long seqId, Dictionary<string, Object> parms); 
    void Fail(long seqId, Dictionary<string, Object> parms);
}
```

Wenn **NextTuple** aufgerufen wird, kann Ihr C#-Code ein oder mehrere Tupel ausgeben. Wenn nichts auszugeben ist, wird die Methode verlassen, ohne etwas zurückzugeben.

Die Methoden **NextTuple**, **Ack** und **Fail** werden alle in einer engen Schleife in einem einzelnen Thread eines C#-Prozesses aufgerufen. Wenn keine Tupel auszugeben sind, lassen Sie **NextTupel** für eine kurze Zeitspanne von etwa 10 Millisekunden ruhen. Dieser Standby hilft, die CPU-Verfügbarkeit nicht zu verschwenden.

Die Methoden **Ack** und **Fail** werden nur aufgerufen, wenn eine Spezifikationsdatei den Bestätigungsmechanismus aktiviert. Der Parameter *seqId* identifiziert das Tupel, das bestätigt wird oder Fehler aufweist. Wenn die Bestätigung in einer nicht transaktionalen Topologie verwendet wird, sollte die folgende **Emit**-Funktion in einem Spout verwendet werden:

```csharp
public abstract void Emit(string streamId, List<object> values, long seqId);
```

Wenn eine nicht transaktionale Topologie die Bestätigung nicht unterstützt, können **Ack** und **Fail** als leere Funktionen belassen werden.

Der Eingabeparameter *parms* in diesen Funktionen gibt ein leeres Wörterbuch an und ist für den zukünftigen Gebrauch vorgesehen.

### <a name="iscpbolt"></a>ISCPBolt

**ISCPBolt** ist die Schnittstelle für einen nicht transaktionalen Bolt.

```csharp
public interface ISCPBolt : ISCPPlugin
{
void Execute(SCPTuple tuple);
}
```

Wenn ein neues Tupel verfügbar ist, wird die Funktion **Execute** aufgerufen, um es zu verarbeiten.

### <a name="iscptxspout"></a>ISCPTxSpout

**ISCPTxSpout** ist die Schnittstelle für einen transaktionalen Spout.

```csharp
public interface ISCPTxSpout : ISCPPlugin
{
    void NextTx(out long seqId, Dictionary<string, Object> parms);  
    void Ack(long seqId, Dictionary<string, Object> parms);         
    void Fail(long seqId, Dictionary<string, Object> parms);        
}
```

Ebenso wie beim nicht transaktionalen Gegenstück werden **NextTx**, **Ack** und **Fail** in einer engen Schleife in einem Singlethread eines C#-Prozesses aufgerufen. Wenn keine Tupel auszugeben sind, lassen Sie **NextTx** für eine kurze Zeitspanne von etwa 10 Millisekunden ruhen. Dieser Standby hilft, die CPU-Verfügbarkeit nicht zu verschwenden.

Wenn **NextTx** aufgerufen wird, um eine neue Transaktion zu starten, identifiziert der Ausgabeparameter *seqId* die Transaktion. Die Transaktion wird auch in **Ack** und **Fail** verwendet. Ihre **NextTx**-Methode kann Daten an die Java-Seite ausgeben. Die Daten werden in ZooKeeper gespeichert, um erneut abgespielt werden zu können. Da ZooKeeper nur über eine begrenzte Kapazität verfügt, sollte Ihr Code nur Metadaten und keine Massendaten in einem transaktionalen Spout ausgeben.

Da Storm eine fehlerhafte Transaktion automatisch erneut wiedergibt, wird **Fail** normalerweise nicht aufgerufen. Falls SCP jedoch die vom transaktionalen Spout ausgegebenen Metadaten überprüft, kann **Fail** für ungültige Metadaten aufgerufen werden.

Der Eingabeparameter *parms* in diesen Funktionen gibt ein leeres Wörterbuch an und ist für den zukünftigen Gebrauch vorgesehen.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt

**ISCPBatchBolt** ist die Schnittstelle für einen transaktionalen Bolt.

```csharp
public interface ISCPBatchBolt : ISCPPlugin
{
    void Execute(SCPTuple tuple);
    void FinishBatch(Dictionary<string, Object> parms);  
}
```

Die Methode **Execute** wird aufgerufen, wenn ein neues Tupel den Bolt erreicht. Die Methode **FinishBatch** wird aufgerufen, wenn diese Transaktion endet. Der Eingabeparameter *parms* ist für den zukünftigen Gebrauch reserviert.

Für eine transaktionale Topologie ist **StormTxAttempt** eine wichtige Klasse. Sie verfügt über zwei Member: **TxId** und **AttemptId**. Das Member **TxId** identifiziert eine bestimmte Transaktion. Eine Transaktion kann mehrmals wiederholt werden, wenn sie fehlerhaft ist und erneut wiedergegeben wird.

SCP.NET erstellt ein neues **ISCPBatchBolt**-Objekt für jedes **StormTxAttempt**-Objekt, analog zum Verhalten von Storm in Java. Der Zweck dieses Entwurfs ist die Unterstützung der parallelen Transaktionsverarbeitung. Nachdem ein Transaktionsversuch abgeschlossen ist, wird das entsprechende **ISCPBatchBolt**-Objekt zerstört und eine Garbage Collection durchgeführt.

## <a name="object-model"></a>Objektmodell

SCP.NET enthält außerdem einen einfachen Satz von Schlüsselobjekten für die Entwicklung. Die Objekte sind **Context**, **StateStore** und **SCPRuntime**. Sie werden in diesem Abschnitt besprochen.

### <a name="context"></a>Kontext

Das **Context**-Objekt stellt einer Anwendung eine Ausführungsumgebung zur Verfügung. Jede **ISCPPlugin**-Instanz von **ISCPSpout**, **ISCPBolt**, **ISCPTxSpout** oder **ISCPBatchBolt** verfügt über eine entsprechende **Context**-Instanz. Die von **Context** bereitgestellten Funktionen sind in die folgenden beiden Teile unterteilt:

* Der statische Teil, der im gesamten C#-Prozess zur Verfügung steht
* Der dynamische Teil, der nur für die spezifische **Context**-Instanz verfügbar ist

### <a name="static-part"></a>Statischer Teil

```csharp
public static ILogger Logger = null;
public static SCPPluginType pluginType;
public static Config Config { get; set; }
public static TopologyContext TopologyContext { get; set; }  
```

Das **Logger**-Objekt wird für die Protokollierung bereitgestellt.

Das **pluginType**-Objekt gibt den Plug-In-Typ des C#-Prozesses an. Falls der Prozess im lokalen Testmodus ohne Java ausgeführt wird, ist der Plug-In-Typ **SCP_NET_LOCAL**.

```csharp
public enum SCPPluginType 
{
    SCP_NET_LOCAL = 0,
    SCP_NET_SPOUT = 1,
    SCP_NET_BOLT = 2,
    SCP_NET_TX_SPOUT = 3,
    SCP_NET_BATCH_BOLT = 4  
    }
```

Die Eigenschaft **Config** erhält Konfigurationsparameter von der Java-Seite, die sie bei der Initialisierung eines C#-Plug-Ins übergibt. Die **Config**-Parameter sind in zwei Teile unterteilt: **stormConf** und **pluginConf**.

```csharp
public Dictionary<string, Object> stormConf { get; set; }  
public Dictionary<string, Object> pluginConf { get; set; }  
```

Der Teil **stormConf** ist ein durch Storm definierter Parameter, und der Teil **pluginConf** ist ein durch SCP definierter Parameter. Ein Beispiel:

```csharp
public class Constants
{
    … …

    // constant string for pluginConf
    public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

    // constant string for stormConf
    public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";
    public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";
}
```

Der Typ **TopologyContext** ruft den Topologiekontext ab. Es ist am nützlichsten für mehrere parallele Komponenten. Ein Beispiel:

```csharp
//demo how to get TopologyContext info
if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
{
    Context.Logger.Info("TopologyContext info:");
    TopologyContext topologyContext = Context.TopologyContext;
    Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());
    taskIndex = topologyContext.GetThisTaskIndex();
    Context.Logger.Info("taskIndex: {0}", taskIndex);
    string componentId = topologyContext.GetThisComponentId();
    Context.Logger.Info("componentId: {0}", componentId);
    List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
    Context.Logger.Info("taskNum: {0}", componentTasks.Count);
}
```

### <a name="dynamic-part"></a>Dynamischer Teil

Die folgenden Schnittstellen sind für eine bestimmte **Context**-Instanz relevant, die von der SCP.NET-Plattform erstellt und an Ihren Code übergeben wird:

```csharp
// Declare the Output and Input Stream Schemas

public void DeclareComponentSchema(ComponentStreamSchema schema);

// Emit tuple to default stream.
public abstract void Emit(List<object> values);

// Emit tuple to the specific stream.
public abstract void Emit(string streamId, List<object> values);  
```

Für einen nicht transaktionalen Spout, der die Bestätigung unterstützt, wird die folgende Methode bereitgestellt:

```csharp
// for nontransactional spout that supports ack
public abstract void Emit(string streamId, List<object> values, long seqId);  
```

Ein nicht transaktionaler Bolt, der die Bestätigung unterstützt, sollte explizit **Ack** oder **Fail** mit dem empfangenen Tupel aufrufen. Wenn ein neues Tupel ausgegeben wird, muss der Bolt auch die Anker des Tupels angeben. Die folgenden Methoden werden bereitgestellt:

```csharp
public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values);
public abstract void Ack(SCPTuple tuple);
public abstract void Fail(SCPTuple tuple);
```

### <a name="statestore"></a>StateStore

Das **StateStore**-Objekt bietet Metadatendienste, monotone Sequenzgenerierung und Koordinierung ohne Wartezeiten. Sie können Abstraktionen von verteilter Parallelität auf höherer Ebene auf **StateStore** aufbauen. Diese Abstraktionen umfassen verteilte Sperren, verteilte Warteschlangen, Barrieren und Transaktionsdienste.

SCP-Anwendungen können das **State**-Objekt verwenden, um Informationen in [Apache ZooKeeper](https://zookeeper.apache.org/) zu serialisieren. Diese Fähigkeit ist besonders für eine Transaktionstopologie wertvoll. Wenn ein transaktionaler Spout nicht mehr reagiert und neu startet, kann **State** die erforderlichen Informationen von ZooKeeper abrufen und die Pipeline neu starten.

Das **StateStore**-Objekt verfügt über diese Prinzipalmethoden:

```csharp
/// <summary>
/// Static method to retrieve a state store of the given path and connStr 
/// </summary>
/// <param name="storePath">StateStore path</param>
/// <param name="connStr">StateStore address</param>
/// <returns>Instance of StateStore</returns>
public static StateStore Get(string storePath, string connStr);

/// <summary>
/// Create a new state object in this state store instance
/// </summary>
/// <returns>State from StateStore</returns>
public State Create();

/// <summary>
/// Retrieve all states that were previously uncommitted, excluding all exited states
/// </summary>
/// <returns>Uncommitted states</returns>
public IEnumerable<State> GetUnCommitted();

/// <summary>
/// Get all the states in the StateStore
/// </summary>
/// <returns>All the states</returns>
public IEnumerable<State> States();

/// <summary>
/// Get state or registry object
/// </summary>
/// <param name="info">Registry name (registry only)</param>
/// <typeparam name="T">Type, registry or state</typeparam>
/// <returns>Return registry or state</returns>
public T Get<T>(string info = null);

/// <summary>
/// List all the committed states
/// </summary>
/// <returns>Registries containing the committed state </returns>
public IEnumerable<Registry> Committed();

/// <summary>
/// List all the exited states in the StateStore
/// </summary>
/// <returns>Registries containing the exited states</returns>
public IEnumerable<Registry> Aborted();

/// <summary>
/// Retrieve an existing state object from this state store instance 
/// </summary>
/// <returns>State from StateStore</returns>
/// <typeparam name="T">stateId, id of the State</typeparam>
public State GetState(long stateId)
```

Das **State**-Objekt verfügt über diese Prinzipalmethoden:

```csharp
/// <summary>
/// Set the status of the state object to commit
/// </summary>
public void Commit(bool simpleMode = true);

/// <summary>
/// Set the status of the state object to exit
/// </summary>
public void Abort();

/// <summary>
/// Put an attribute value under the given key
/// </summary>
/// <param name="key">Key</param>
/// <param name="attribute">State attribute</param>
    public void PutAttribute<T>(string key, T attribute);

/// <summary>
/// Get the attribute value associated with the given key
/// </summary>
/// <param name="key">Key</param>
/// <returns>State attribute</returns>
    public T GetAttribute<T>(string key);
```

Wenn **simpleMode** auf **true** festgelegt ist, löscht die **Commit**-Methode den entsprechenden ZNode in ZooKeeper. Andernfalls löscht die Methode den aktuellen ZNode und fügt einen neuen Knoten im COMMITTED\_PATH hinzu.

### <a name="scpruntime"></a>SCPRuntime

Die **SCPRuntime**-Klasse stellt die folgenden beiden Methoden bereit:

```csharp
public static void Initialize();

public static void LaunchPlugin(newSCPPlugin createDelegate);  
```

Die **Initialize**-Methode initialisiert die SCP-Laufzeitumgebung. In dieser Methode stellt der C#-Prozess eine Verbindung mit der Java-Komponente her, um Konfigurationsparameter und den Topologiekontext abzurufen.

Die **LaunchPlugin**-Methode startet die Nachrichtenverarbeitungsschleife. In dieser Schleife empfängt das C#-Plug-In Nachrichten von der Java-Seite. Diese Nachrichten umfassen Tupel und Steuersignale. Das Plug-In verarbeitet dann die Nachrichten, eventuell durch Aufruf der von Ihrem Code bereitgestellten Schnittstellenmethode.

Der Eingabeparameter für **LaunchPlugin** ist ein Delegat. Die Methode kann ein Objekt zurückgeben, das die Schnittstelle **ISCPSpout**, **ISCPBolt**, **ISCPTxSpout** oder **ISCPBatchBolt** implementiert.

```csharp
public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary<string, Object> parms);
```

Für **ISCPBatchBolt** können Sie ein **StormTxAttempt**-Objekt aus dem *parms*-Parameter abrufen und damit beurteilen, ob es sich bei dem Versuch um einen wiederholten Versuch handelt. Die Überprüfung auf einen Wiederholungsversuch erfolgt häufig im Commit-Bolt. Das HelloWorldTx-Beispiel weiter unten in diesem Artikel veranschaulicht diese Überprüfung.

SCP-Plug-Ins können normalerweise in zwei Modi ausgeführt werden: lokaler Testmodus und normaler Modus.

#### <a name="local-test-mode"></a>Lokaler Testmodus

In diesem Modus werden die SCP-Plug-Ins in Ihrem C#-Code während der Entwicklungsphase in Visual Studio ausgeführt. In diesem Modus können Sie die Schnittstelle **ILocalContext** verwenden. Die Schnittstelle stellt Methoden zur Verfügung, um die ausgegebenen Tupel zu lokalen Dateien zu serialisieren und sie wieder in den Arbeitsspeicher (RAM) einzulesen.

```csharp
public interface ILocalContext
{
    List<SCPTuple> RecvFromMsgQueue();
    void WriteMsgQueueToFile(string filepath, bool append = false);  
    void ReadFromFileToMsgQueue(string filepath);
}
```

#### <a name="regular-mode"></a>Normaler Modus

In diesem Modus führt der Storm-Java-Prozess die SCP-Plug-ins aus. Es folgt ein Beispiel:

```csharp
namespace Scp.App.HelloWorld
{
public class Generator : ISCPSpout
{
    … …
    public static Generator Get(Context ctx, Dictionary<string, Object> parms)
    {
    return new Generator(ctx);
    }
}

class HelloWorld
{
    static void Main(string[] args)
    {
    /* Setting the environment variable here can change the log file name */
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");

    SCPRuntime.Initialize();
    SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
    }
}
}
```

## <a name="topology-specification-language"></a>Sprache für die Topologiespezifikation

SCP Topology Specification ist eine domänenspezifische Sprache (DSL) zum Beschreiben und Konfigurieren von SCP-Topologien. Sie basiert auf [Clojure DSL von Storm](https://storm.incubator.apache.org/documentation/Clojure-DSL.html) und wird durch SCP erweitert.

Sie können Topologiespezifikationen direkt an einen Storm-Cluster zur Ausführung über den Befehl **runSpec** übermitteln.

In SCP.NET wurden die folgenden Funktionen zum Definieren transaktionaler Topologien hinzugefügt:

| Neue Funktion | Parameter | Beschreibung |
| --- | --- | --- |
| **tx-topolopy** |*topology-name*<br />*spout-map*<br />*bolt-map* |Definiert eine transaktionale Topologie mit dem Topologienamen, der Spouts-Definitionszuordnung und der Bolts-Definitionszuordnung. |
| **scp-tx-spout** |*exec-name*<br />*args*<br />*fields* |Definiert einen transaktionalen Spout. Die Funktion führt die durch *exec-name* angegebene Anwendung aus und verwendet *args*.<br /><br />Der Parameter *fields* gibt die Ausgabefelder für den Spout an. |
| **scp-tx-batch-bolt** |*exec-name*<br />*args*<br />*fields* |Definiert einen transaktionalen Batch-Bolt. Die Funktion führt die durch *exec-name* angegebene Anwendung aus und verwendet *args*.<br /><br />Der Parameter *fields* gibt die Ausgabefelder für den Bolt an. |
| **scp-tx-commit-bolt** |*exec-name*<br />*args*<br />*fields* |Definiert einen transaktionalen Commit-Bolt. Die Funktion führt die durch *exec-name* angegebene Anwendung aus und verwendet *args*.<br /><br />Der Parameter *fields* gibt die Ausgabefelder für den Bolt an. |
| **nontx-topology** |*topology-name*<br />*spout-map*<br />*bolt-map* |Definiert eine nicht transaktionale Topologie mit dem Topologienamen, der Spouts-Definitionszuordnung und der Bolts-Definitionszuordnung. |
| **scp-spout** |*exec-name*<br />*args*<br />*fields*<br />*parameters* |Definiert einen nicht transaktionalen Spout. Die Funktion führt die durch *exec-name* angegebene Anwendung aus und verwendet *args*.<br /><br />Der Parameter *fields* gibt die Ausgabefelder für den Spout an.<br /><br />Der *parameters*-Parameter ist optional. Verwenden Sie ihn, um Parameter wie „nontransactional.ack.enabled“ anzugeben. |
| **scp-bolt** |*exec-name*<br />*args*<br />*fields*<br />*parameters* |Definiert einen nicht transaktionalen Bolt. Die Funktion führt die durch *exec-name* angegebene Anwendung aus und verwendet *args*.<br /><br />Der Parameter *fields* gibt die Ausgabefelder für den Bolt an.<br /><br />Der *parameters*-Parameter ist optional. Verwenden Sie ihn, um Parameter wie „nontransactional.ack.enabled“ anzugeben. |

SCP.NET definiert die folgenden Schlüsselwörter:

| Schlüsselwort | Beschreibung |
| --- | --- |
| **:name** |Der Topologiename. |
| **:topology** |Die Topologie unter Verwendung der Funktionen in der vorherigen Tabelle und der integrierten Funktionen. |
| **:p** |Der Parallelitätshinweis für die einzelnen Spouts oder Bolts. |
| **:config** |Gibt an, ob Parameter konfiguriert oder die vorhandenen Parameter aktualisiert werden. |
| **:schema** |Das Schema des Datenstroms. |

SCP.NET definiert auch diese häufig verwendeten Parameter:

| Parameter | Beschreibung |
| --- | --- |
| „plugin.name“ |Der .exe-Dateiname des C#-Plug-Ins. |
| „plugin.args“ |Die Plug-In-Argumente. |
| „output.schema“ |Das Ausgabeschema. |
| „nontransactional.ack.enabled“ |Gibt an, ob die Bestätigung für eine nicht transaktionale Topologie aktiviert ist. |

Der **runSpec**-Befehl wird zusammen mit den Bits bereitgestellt. Befehlssyntax:

```csharp
.\bin\runSpec.cmd
usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target
```

Der Parameter *resource-dir* ist optional. Geben Sie sie an, wenn Sie eine C#-Anwendung einbinden möchten. Das angegebene Verzeichnis enthält die Anwendung, Abhängigkeiten und Konfigurationen.

Der *classpath*-Parameter ist ebenfalls optional. Er gibt den Java-Klassenpfad an, wenn die Spezifikationsdatei einen Java-Spout oder -Bolt enthält.

## <a name="miscellaneous-features"></a>Verschiedene Features

### <a name="input-and-output-schema-declarations"></a>Deklarieren von Ein- und Ausgabeschema

Ihre C#-Prozesse können Tupel ausgeben. Dazu serialisiert die Plattform Tupel in **byte[]**-Objekte und überträgt die Objekte auf die Java-Seite. Storm überträgt diese Tupel dann an die Ziele.

In Downstreamkomponenten erhalten die C#-Prozesse Tupel von der Java-Seite zurück und konvertieren sie in die ursprünglichen Typen der Plattform. All diese Vorgänge werden von der Plattform ausgeblendet.

Um Serialisierung und Deserialisierung zu unterstützen, muss Ihr Code das Schema für Ein- und Ausgabe deklarieren. Das Schema ist als Wörterbuch definiert. Die Datenstrom-ID ist der Wörterbuchschlüssel. Der Schlüsselwert sind die Typen der Spalten. Eine Komponente kann mehrere Datenströme deklarieren.

```csharp
public class ComponentStreamSchema
{
    public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
    public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
    public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
    {
        InputStreamSchema = input;
        OutputStreamSchema = output;
    }
}
```

Die folgende Funktion wird zu einem **Context**-Objekt hinzugefügt:

```csharp
public void DeclareComponentSchema(ComponentStreamSchema schema)
```

Die Entwickler müssen sicherstellen, dass die ausgegebenen Tupel dem für einen Datenstrom definierten Schema entsprechen. Andernfalls löst das System eine Laufzeitausnahme aus.

### <a name="multistream-support"></a>Unterstützung mehrerer Datenströme

SCP unterstützt Benutzercode, der mit mehreren separaten Datenströmen gleichzeitig kommunizieren kann. Das **Context**-Objekt spiegelt diese Unterstützung als optionalen Datenstrom-ID-Parameter der **Emit**-Methode wider.

Im SCP.NET-**Context**-Objekt wurden zwei Methoden hinzugefügt. Sie geben mindestens ein Tupel an bestimmte Datenströme aus. Der Parameter *streamId* ist eine Zeichenfolge. Sein Wert muss sowohl im C#-Code als auch in der Spezifikation der Topologiedefinition gleich sein.

```csharp
/* Emit tuple to the specific stream. */
public abstract void Emit(string streamId, List<object> values);

/* for nontransactional spout only */
public abstract void Emit(string streamId, List<object> values, long seqId);
```

Das Ausgeben an einen nicht vorhandenen Datenstrom führt zu Laufzeitausnahmen.

### <a name="fields-grouping"></a>Gruppieren von Feldern

Das integrierte Gruppieren von Feldern in Storm funktioniert in SCP.NET nicht ordnungsgemäß. Auf der Java-Proxyseite ist der Datentyp aller Felder tatsächlich **byte[]**. Die Feldgruppierung verwendet den Hashcode des **byte[]**-Objekts, um die Gruppierung vorzunehmen. Der Hashcode ist die Adresse dieses Objekts im RAM. Die Gruppierung ist also für Multibyte-Objekte, die zwar denselben Inhalt, jedoch nicht dieselbe Adresse haben, falsch.

SCP.NET fügt eine angepasste Gruppierungsmethode hinzu und verwendet den Inhalt des **byte[]**-Objekts für die Gruppierung. In einer Spezifikationsdatei sieht die Syntax wie in diesem Beispiel aus:

```csharp
(bolt-spec
    {
        "spout_test" (scp-field-group :non-tx [0,1])
    }
    …
)
```

In der vorhergehenden Spezifikationsdatei:

* `scp-field-group` gibt an, dass es sich bei der Gruppierung um eine benutzerdefinierte Feldgruppierung handelt, die von SCP implementiert wurde.
* `:tx` oder `:non-tx` gibt an, ob die Topologie transaktional ist. Sie benötigen diese Informationen, da der Startindex zwischen transaktionalen und nicht transaktionalen Topologien unterschiedlich ist.
* `[0,1]` gibt einen Hashset von Feld-IDs an, die mit Null beginnen.

### <a name="hybrid-topology"></a>Hybride Topologie

Nativer Storm-Code wird in Java geschrieben. SCP.NET hat Storm so erweitert, dass Sie C#-Code für Ihre Geschäftslogik schreiben können. SCP.NET unterstützt aber auch Hybridtopologien, die neben C#-Spouts/-Bolts auch Java-Spouts/-Bolts enthalten.

### <a name="specify-java-spoutbolt-in-a-specification-file"></a>Angeben von Java-Spout/Bolt in einer Spezifikationsdatei

Sie können **scp-spout** und **scp-bolt** in einer Spezifikationsdatei verwenden, um Java-Spouts und -Bolts anzugeben. Ein Beispiel:

```csharp
(spout-spec 
  (microsoft.scp.example.HybridTopology.Generator.)
  :p 1)
```

Hier ist `microsoft.scp.example.HybridTopology.Generator` der Klassenname des Java-Spouts.

### <a name="specify-the-java-classpath-in-a-runspec-command"></a>Angeben des Java-Klassenpfads in einem runSpec-Befehl

Wenn Sie eine Topologie übermitteln möchten, die Java-Spouts oder -Bolts enthält, kompilieren Sie diese zunächst, um JAR-Dateien zu erzeugen. Anschließend geben Sie den Java-Klassenpfad mit den JAR-Dateien an, wenn Sie die Topologie übermitteln. Ein Beispiel:

```csharp
bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*
```

Hier ist `examples\HybridTopology\java\target\` der Ordner, der die JAR-Datei für den Java-Spout/-Bolt enthält.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Serialisierung und Deserialisierung zwischen Java und C#

Eine SCP-Komponente umfasst die Java-Seite und die C#-Seite. Um mit den nativen Java-Spouts/-Bolts zu interagieren, müssen Serialisierung und Deserialisierung zwischen der Java-Seite und der C#-Seite erfolgen, wie in der folgenden Grafik dargestellt:

:::image type="content" source="./media/apache-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png" alt-text="Diagramm der Java-Komponente, die an die SCP-Komponente sendet, die dann an eine andere Java-Komponente sendet" border="false":::

#### <a name="serialization-in-the-java-side-and-deserialization-in-the-c-side"></a>Serialisierung auf der Java-Seite und Deserialisierung auf der C#-Seite

Stellen Sie zunächst die Standardimplementierung für die Serialisierung auf der Java-Seite und die Deserialisierung auf der C#-Seite bereit.

Geben Sie die Serialisierungsmethode der Java-Seite in einer Spezifikationsdatei an.

```csharp
(scp-bolt
    {
        "plugin.name" "HybridTopology.exe"
        "plugin.args" ["displayer"]
        "output.schema" {}
        "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
    })
```

Geben Sie die Deserialisierungsmethode der C#-Seite in Ihrem C#-Code an.

```csharp
Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
inputSchema.Add("default", new List<Type>() { typeof(Person) });
this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());
```  

Wenn der Datentyp nicht zu komplex ist, sollte diese Standardimplementierung die meisten Fälle abdecken. Hier folgen Fälle, in denen Sie Ihre eigene Implementierung einbinden können:

* Ihr Datentyp ist für die Standardimplementierung zu komplex.
* Die Leistung Ihrer Standardimplementierung entspricht nicht Ihren Anforderungen.

Die Serialisierungsschnittstelle ist auf der Java-Seite wie folgt definiert:

```csharp
public interface ICustomizedInteropJavaSerializer {
    public void prepare(String[] args);
    public List<ByteBuffer> serialize(List<Object> objectList);
}
```

Die Deserialisierungsschnittstelle ist auf der C#-Seite wie folgt definiert:

```csharp
public interface ICustomizedInteropCSharpDeserializer
{
    List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
}
```

#### <a name="serialization-in-the-c-side-and-deserialization-in-the-java-side"></a>Serialisierung auf der C#-Seite und Deserialisierung auf der Java-Seite

Geben Sie die Serialisierungsmethode der C#-Seite in Ihrem C#-Code an.

```csharp
this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
```

Geben Sie die Deserialisierungsmethode der Java-Seite in einer Spezifikationsdatei an.

```csharp
(scp-spout
   {
     "plugin.name" "HybridTopology.exe"
     "plugin.args" ["generator"]
     "output.schema" {"default" ["person"]}
     "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
   }
)
```

Dabei ist `"microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer"` der Name des Deserialisierers und `"microsoft.scp.example.HybridTopology.Person"` ist die Zielklasse, in die die Daten deserialisiert werden.

Sie können auch Ihre eigene Implementierung eines C#-Serialisierungsmoduls und eines Java-Deserialisierers einbinden.

Dieser Code entspricht der Schnittstelle für das C#-Serialisierungsmodul:

```csharp
public interface ICustomizedInteropCSharpSerializer
{
    List<byte[]> Serialize(List<object> dataList);
}
```

Dieser Code entspricht der Schnittstelle für den Java-Deserialisierer:

```csharp
public interface ICustomizedInteropJavaDeserializer {
    public void prepare(String[] targetClassNames);
    public List<Object> Deserialize(List<ByteBuffer> dataList);
}
```

## <a name="scp-host-mode"></a>SCP-Hostmodus

In diesem Modus können Sie Ihren Code als DLL kompilieren und SCPHost.exe wie von SCP bereitgestellt verwenden, um eine Topologie zu übermitteln. Eine Spezifikationsdatei sieht wie der folgende Code aus:

```csharp
(scp-spout
  {
    "plugin.name" "SCPHost.exe"
    "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
    "output.schema" {"default" ["sentence"]}
  })
```

Hier wird `"plugin.name"` als `"SCPHost.exe"` angegeben, was durch das SCP SDK bereitgestellt wird. SCPHost.exe akzeptiert drei Parameter in der folgenden Reihenfolge:

1. Der Name der DLL, der in diesem Beispiel `"HelloWorld.dll"` ist.
1. Der Name der Klasse, der in diesem Beispiel `"Scp.App.HelloWorld.Generator"` ist.
1. Der Name einer öffentlichen statischen Methode, die aufgerufen werden kann, um eine Instanz von **ISCPPlugin** zu erhalten.

Kompilieren Sie im Hostmodus Ihren Code als DLL für den Aufruf durch die SCP-Plattform. Da die Plattform dann die volle Kontrolle über die gesamte Verarbeitungslogik erhalten kann, empfehlen wir Ihnen, die Topologie im SCP-Hostmodus zu übermitteln. Dies vereinfacht die Entwicklung. Außerdem bietet Sie mehr Flexibilität und eine bessere Abwärtskompatibilität für spätere Releases.

## <a name="scp-programming-examples"></a>SCP-Programmierbeispiele

### <a name="helloworld"></a>HelloWorld

Das folgende einfache HelloWorld-Beispiel bietet einen Eindruck von SCP.NET. Verwendet werden eine nicht transaktionale Topologie mit einem Spout namens **generator** und zwei Bolts namens **splitter** und **counter**. Der Spout **generator** erzeugt zufällige Sätze und übermittelt diese Sätze an **splitter**. Der Bolt **splitter** teilt die Sätze in Wörter auf und übermittelt diese Wörter an den Bolt **counter**. Der Bolt **counter** verwendet ein Wörterbuch, um das Auftreten der einzelnen Wörter aufzuzeichnen.

Dieses Beispiel hat zwei Spezifikationsdateien: HelloWorld.spec und HelloWorld \_EnableAck.spec. Der C#-Code kann herausfinden, ob Acknowledgment aktiviert ist, indem er das `pluginConf`Objekt von der Java-Seite holt.

```csharp
/* demo how to get pluginConf info */
if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
{
    enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
}
Context.Logger.Info("enableAck: {0}", enableAck);
```

Wenn die Bestätigung im Spout aktiviert ist, werden die nicht bestätigten Tupel in einem Wörterbuch zwischengespeichert. Wenn `Fail` aufgerufen wird, wird das fehlgeschlagene Tupel erneut wiedergegeben.

```csharp
public void Fail(long seqId, Dictionary<string, Object> parms)
{
    Context.Logger.Info("Fail, seqId: {0}", seqId);
    if (cachedTuples.ContainsKey(seqId))
    {
        /* get the cached tuple */
        string sentence = cachedTuples[seqId];

        /* replay the failed tuple */
        Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
    }
    else
    {
        Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
    }
}
```

### <a name="helloworldtx"></a>HelloWorldTx

Das folgende Beispiel „HelloWorldTx“ verdeutlicht die Implementierung einer transaktionalen Topologie. Das Beispiel enthält einen Spout mit dem Namen **generator**, einen Batch-Bolt mit dem Namen **partial-count** und einen Commit-Bolt mit dem Namen **count-sum**. Das Beispiel enthält auch drei bereits vorhandene Textdateien: „DataSource0.txt“, „DataSource1.txt“ und „DataSource2.txt“.

Für jede Transaktion wählt der Spout **generator** nach dem Zufallsprinzip zwei der drei vorhandenen Dateien aus und übermittelt die beiden Dateinamen an den Bolt **partial-count**. Der Bolt **partial-count**:

1. Ruft einen Dateinamen aus dem empfangenen Tupel ab.
1. Öffnet die entsprechende Datei.
1. Zählt die Anzahl der Wörter in der Datei.
1. Gibt die Wortanzahl an den Bolt **count-sum** aus.

Der Bolt **count-sum** erstellt die Gesamtzahl.

Um „Exactly-Once“-Semantik zu erzielen, muss der Commit-Bolt **count-sum** beurteilen können, ob es sich um eine wiederholte Transaktion handelt. In diesem Beispiel enthält er die folgende statische Membervariable:

```csharp
public static long lastCommittedTxId = -1; 
```

Wenn eine **ISCPBatchBolt**-Instanz erstellt wird, erhält sie den Wert des `txAttempt`-Objekts aus Eingabeparametern.

```csharp
public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
{
    /* for transactional topology, we can get txAttempt from the input parms */
    if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
    {
        StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
        return new CountSum(ctx, txAttempt);
    }
    else
    {
        throw new Exception("null txAttempt");
    }
}
```

Beim Aufrufen von `FinishBatch` wird `lastCommittedTxId` aktualisiert, wenn es sich nicht um eine wiederholte Transaktion handelt.

```csharp
public void FinishBatch(Dictionary<string, Object> parms)
{
    /* judge whether it is a replayed transaction */
    bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

    if (!replay)
    {
        /* If it is not replayed, update the totalCount and lastCommittedTxId value */
        totalCount = totalCount + this.count;
        lastCommittedTxId = this.txAttempt.TxId;
    }
    … …
}
```

### <a name="hybridtopology"></a>HybridTopology

Diese Topologie enthält einen Java-Spout und einen C#-Bolt. Sie verwendet die Standardimplementierungen der SCP-Plattform für Serialisierung und Deserialisierung. Details zur Spezifikationsdatei finden Sie in der Datei „HybridTopology.spec“ im Ordner „examples\\HybridTopology“. Weitere Informationen zur Angabe des Java-Klassenpfads finden Sie auch in der Datei „SubmitTopology.bat“.

### <a name="scphostdemo"></a>SCPHostDemo

Dieses Beispiel ist dem HelloWorld-Beispiel sehr ähnlich. Der einzige Unterschied ist, dass Ihr Code als DLL kompiliert und die Topologie mit „SCPHost.exe“ übermittelt wird. Eine ausführlichere Erläuterung finden Sie im Abschnitt über den SCP-Hostmodus.

## <a name="next-steps"></a>Nächste Schritte

Beispiele für Apache Storm-Topologien, die mit SCP erstellt wurden, finden Sie in den folgenden Artikeln:

* [Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)
* [Verarbeitung von Ereignissen von Azure Event Hubs mit Apache Storm auf HDInsight](apache-storm-develop-csharp-event-hub-topology.md)
* [Verarbeiten von Fahrzeugsensordaten von Event Hubs mit Apache Storm in HDInsight](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Extrahieren, Transformieren und Laden (ETL) von Azure Event Hubs auf Apache HBase](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
