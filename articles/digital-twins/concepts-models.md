---
title: DTDL-Modelle
titleSuffix: Azure Digital Twins
description: In diesem Artikel erfahren Sie, wie Azure Digital Twins benutzerdefinierte Modelle verwendet, um Entitäten in Ihrer Umgebung zu beschreiben.
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: d3570a22fdd935237e673ea3e43ab5e463b66456
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104590533"
---
# <a name="understand-twin-models-in-azure-digital-twins"></a>Grundlegendes zu Zwillingsmodellen in Azure Digital Twins

Ein Hauptmerkmal von Azure Digital Twins ist die Fähigkeit, Ihr eigenes Vokabular zu definieren und Ihren Zwillingsgraph in den selbstdefinierten Begriffen Ihres Unternehmens zu erstellen. Diese Funktion wird mittels von Benutzern bereitgestellten **Modellen** ermöglicht. Sie können sich Modelle in einer Beschreibung Ihrer Umgebung als die Nomen vorstellen. 

Ein Modell ähnelt einer **Klasse** einer objektorientierten Programmiersprache und definiert eine Datenform für ein bestimmtes Konzept in Ihrer realen Arbeitsumgebung. Modelle haben Namen (wie z. B. *Raum* oder *Temperatursensor*) und enthalten Elemente wie Eigenschaften, Telemetrie/Ereignisse und Befehle, die beschreiben, welche Möglichkeiten dieser Entitätstyp in Ihrer Umgebung hat. Später nutzen Sie diese Modelle, um [**Digital Twins**](concepts-twins-graph.md) zu erstellen, die bestimmte dieser Typbeschreibung entsprechende Entitäten darstellen.

Azure Digital Twins-Modelle werden in der auf JSON-LD basierenden Sprache **Digital Twin Definition Language (DTDL)** dargestellt.  

## <a name="digital-twin-definition-language-dtdl-for-models"></a>Digital Twin Definition Language (DTDL) für Modelle

Modelle für Azure Digital Twins werden mithilfe von DTDL (Digital Twins Definition Language) definiert. 

Sie können die vollständigen Sprachspezifikationen für DTDL in GitHub anzeigen: [**Digital Twins Definition Language (DTDL) – Version 2**](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) (in englischer Sprache).

DTDL basiert auf JSON-LD und ist von der Programmiersprache unabhängig. DTDL ist nicht ausschließlich für Azure Digital Twins bestimmt, sondern wird auch zur Darstellung von Gerätedaten in anderen IoT-Diensten wie [IoT Plug & Play](../iot-pnp/overview-iot-plug-and-play.md) verwendet. Azure Digital Twins verwendet DTDL, **Version 2** (die Verwendung von DTDL, Version 1, für Azure Digital Twins wird nicht mehr unterstützt). 

Der restliche Artikel bietet eine Übersicht über die Verwendung der Sprache in Azure Digital Twins.

> [!NOTE] 
> Nicht alle Dienste, die DTDL verwenden, implementieren genau die gleichen Features von DTDL. Beispielsweise verwendet IoT Plug & Play keine DTDL-Features, die für Graphen gedacht sind, während Azure Digital Twins derzeit keine DTDL-Befehle implementiert.
>
> Weitere Informationen zu den DTDL-Features, die für Azure Digital Twins spezifisch sind, finden Sie in diesem Artikel im Abschnitt [ Besonderheiten der DTDL-Implementierung für Azure Digital Twins](#azure-digital-twins-dtdl-implementation-specifics).

## <a name="elements-of-a-model"></a>Elemente eines Modells

Innerhalb einer Modelldefinition ist das Element auf oberster Codeebene eine **Schnittstelle**. Damit wird das gesamte Modell gekapselt, während der Rest des Modells innerhalb der Schnittstelle definiert wird. 

Eine DTDL-Modellschnittstelle kann keines, eines oder mehrere der folgenden Felder enthalten:
* **Eigenschaft**: Eigenschaften sind Datenfelder, die den Zustand einer Entität repräsentieren (wie die Eigenschaften in vielen objektorientierten Programmiersprachen). Eigenschaften haben Sicherungsspeicher und können jederzeit gelesen werden.
* **Telemetrie**: Telemetriefelder stellen Messungen oder Ereignisse dar und dienen häufig zur Beschreibung von Sensormesswerten von Geräten. Im Gegensatz zu Eigenschaften wird Telemetrie nicht in einem digitalen Zwilling gespeichert. Telemetrie ist eine Reihe von zeitgebundenen Datenereignissen, die bei ihrem Auftreten verarbeitet werden müssen. Weitere Informationen zu den Unterschieden zwischen Eigenschaft und Telemetrie finden Sie unten im Abschnitt [*Eigenschaften im Vergleich zu Telemetrie*](#properties-vs-telemetry).
* **Komponenten**: Komponenten ermöglichen Ihnen nach Wunsch das Erstellen Ihrer Modellschnittstelle als Zusammensetzung anderer Schnittstellen. Ein Beispiel einer Komponente ist die Schnittstelle *Frontkamera* (und die weitere Komponentenschnittstelle *Rückkamera*), die bei der Definition eines Modells für ein *Smartphone* verwendet werden. Sie müssen zunächst eine Schnittstelle für *Frontkamera* so definieren, als wäre sie ihr eigenes Modell. Danach können Sie beim Definieren von *Smartphone* darauf verweisen.

    Beschreiben Sie mit einer Komponente etwas, das ein integraler Bestandteil Ihrer Lösung ist, aber keine separate Identität benötigt und im Digital Twin-Graph nicht unabhängig erstellt, gelöscht oder neu angeordnet werden muss. Wenn Sie möchten, dass Entitäten im Zwillingsgraphen unabhängig voneinander existieren, stellen Sie sie als separate Digital Twins verschiedener Modelle dar, die durch *Beziehungen* verbunden sind (siehe den nächsten Punkt).
    
    >[!TIP] 
    >Komponenten können auch zur Organisation verwendet werden, um Sätze verwandter Eigenschaften innerhalb einer Modellschnittstelle zu gruppieren. In diesem Fall können Sie sich jede Komponente als einen Namespace oder „Ordner“ innerhalb der Schnittstelle vorstellen.
* **Beziehung**: Anhand von Beziehungen können Sie darstellen, wie ein Digital Twin mit anderen Digital Twins zusammenarbeiten kann. Beziehungen können verschiedene semantische Bedeutungen darstellen, wie z. B. *enthält* („Raum enthält Boden“), *kühlt* („Klimaanlage kühlt Raum“), *Rechnungsempfänger* („Kompressor wird Benutzer in Rechnung gestellt“) usw. Beziehungen ermöglichen der Lösung, einen Graphen miteinander verbundener Entitäten zu erstellen.

> [!NOTE]
> Die [Spezifikation für DTDL](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) definiert auch **Befehle**, d. h. Methoden, die auf einem Digital Twin ausgeführt werden können (z. B. ein Rücksetzbefehl oder ein Befehl zum Ein- oder Ausschalten eines Lüfters). Allerdings werden *Befehle in Azure Digital Twins derzeit nicht unterstützt*.

### <a name="properties-vs-telemetry"></a>Eigenschaften im Vergleich zu Telemetrie

Hier finden Sie einige zusätzliche Anleitungen zum Unterscheiden zwischen den DTDL-Feldern **Eigenschaft** und **Telemetrie** in Azure Digital Twins.

Zwischen Eigenschaften und Telemetrie für Azure Digital Twins-Modelle gibt es folgenden Unterschied:
* Bei **Eigenschaften** muss es Sicherungsspeicher geben. Dies bedeutet, dass Sie eine Eigenschaft jederzeit lesen und deren Wert abrufen können. Wenn die Eigenschaft schreibbar ist, können Sie darin auch einen Wert speichern.  
* **Telemetrie** ähnelt eher einem Datenstrom von Ereignissen. Es handelt sich dabei um eine Reihe von Datennachrichten mit nur kurzer Lebensdauer. Wenn Sie kein Lauschen für das Ereignis und keine Maßnahmen einrichten, die in diesem Fall zu ergreifen sind, gibt es zu einem späteren Zeitpunkt keine Ablaufverfolgung für das Ereignis. Sie können dann nicht mehr zur Ablaufverfolgung zurückkehren und diese lesen. 
  - In C#-Begriffen ist Telemetrie wie ein C#-Ereignis. 
  - In IoT-Begriffen ist Telemetrie normalerweise eine einzelne Messung, die von einem Gerät gesendet wird.

**Telemetrie** wird oft bei IoT-Geräten verwendet, weil viele Geräte die von ihnen generierten Messwerte nicht speichern können (oder an Speichern nicht interessiert sind). Sie senden die Messwerte einfach als Datenstrom von „Telemetrie“ereignissen. In diesem Fall können Sie auf dem Gerät nicht jederzeit nach dem neuesten Wert des Telemetriefelds fragen. Stattdessen müssen Sie auf die Nachrichten vom Gerät lauschen und bei deren Eintreffen Maßnahmen ergreifen. 

Wenn Sie ein Modell in Azure Digital Twins entwerfen, verwenden Sie deshalb in den meisten Fällen wahrscheinlich **Eigenschaften** zum Modellieren Ihrer Zwillinge. Dies erlaubt Ihnen die Nutzung des Sicherungsspeichers und die Möglichkeit zum Lesen und Abfragen der Datenfelder.

Telemetrie und Eigenschaften arbeiten oft zusammen, um den Dateneingang von Geräten zu verarbeiten. Da der gesamte Eingang bei Azure Digital Twins über [APIs](how-to-use-apis-sdks.md) abgewickelt wird, verwenden Sie normalerweise deren Eingangsfunktion zum Lesen der Telemetrie oder der Eigenschaftsereignisse von Geräten und legen als Reaktion darauf eine Eigenschaft in Azure Digital Twins fest. 

Sie können ein Telemetrieereignis auch über die Azure Digital Twins-API veröffentlichen. Wie bei anderer Telemetrie handelt es sich dabei um ein kurzlebiges Ereignis, das zur Verarbeitung einen Listener benötigt.

### <a name="azure-digital-twins-dtdl-implementation-specifics"></a>Besonderheiten der DTDL-Implementierung für Azure Digital Twins

Damit ein DTDL-Modell mit Azure Digital Twins kompatibel ist, muss es diese Anforderungen erfüllen.

* Alle DTDL-Elemente auf oberster Ebene in einem Modell müssen den Typ *Schnittstelle* haben. Dies liegt daran, dass die APIs des Azure Digital Twins-Modells JSON-Objekte empfangen können, die entweder eine Schnittstelle oder ein Array von Schnittstellen darstellen. Infolgedessen sind auf der obersten Ebene keine anderen DTDL-Elementtypen zulässig.
* DTDL für Azure Digital Twins darf keine *Befehle* definieren.
* Azure Digital Twins ermöglicht für Komponenten nur eine einzelne Schachtelungsebene. Das bedeutet, dass eine Schnittstelle, die als Komponente verwendet wird, selbst keine Komponenten haben darf. 
* Schnittstellen können nicht inline innerhalb anderer DTDL-Schnittstellen definiert werden, sondern müssen als separate Entitäten auf oberster Ebene mit eigenen IDs definiert werden. Wenn dann eine andere Schnittstelle diese Schnittstelle als Komponente oder durch Vererbung einbinden möchte, kann sie auf ihre ID verweisen.

Azure Digital Twins beobachtet auch nicht das `writable`-Attribut für Eigenschaften oder Beziehungen. Obwohl dies gemäß DTDL-Spezifikationen festgelegt werden kann, wird der Wert von Azure Digital Twins nicht verwendet. Stattdessen werden diese von externen Clients, die über allgemeine Schreibberechtigungen für den Azure Digital Twins-Dienst verfügen, immer als beschreibbar behandelt.

## <a name="example-model-code"></a>Beispielmodellcode

Modelle von Digital Twin-Typen können ein einem beliebigen Text-Editor geschrieben werden. Die Sprache DTDL folgt der JSON-Syntax, weshalb Sie Modelle mit der Erweiterung *.json* speichern müssen. Durch Verwendung der JSON-Erweiterung wird vielen Text-Editoren zur Programmierung ermöglicht, eine grundlegende Syntaxprüfung und Hervorhebung für Ihre DTDL-Dokumente bereitzustellen. Es ist auch eine [DTDL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl) für [Visual Studio Code](https://code.visualstudio.com/) verfügbar.

Dieser Abschnitt enthält ein Beispiel für ein typisches Modell, das als DTDL-Schnittstelle geschrieben wurde. Das Modell beschreibt **Planeten** mit jeweils Name, Masse und Temperatur.
 
Beachten Sie, dass die-Planeten auch mit **Monden** interagieren können, die ihre Satelliten sind und **Krater** aufweisen können. Im folgenden Beispiel drückt das `Planet`-Modell Verbindungen mit diesen anderen Entitäten aus, indem auf zwei externe Modelle verwiesen wird: `Moon` und `Crater`. Diese Modelle werden auch im folgenden Beispielcode definiert, sind jedoch sehr einfach gehalten, sodass Sie nicht vom primären `Planet`-Beispiel ablenken.

:::code language="json" source="~/digital-twins-docs-samples/models/Planet-Crater-Moon.json":::

Die Felder des Modells sind wie folgt:

| Feld | Beschreibung |
| --- | --- |
| `@id` | Ein Bezeichner für das Modell. Muss das Format `dtmi:<domain>:<unique model identifier>;<model version number>` haben. |
| `@type` | Gibt die Art der beschriebenen Informationen an. Bei einer Schnittstelle ist der Typ *Schnittstelle*. |
| `@context` | Legt den [Kontext](https://niem.github.io/json/reference/json-ld/context/) für das JSON-Dokument fest. Für Modelle muss `dtmi:dtdl:context;2` verwendet werden. |
| `displayName` | [optional] Ermöglicht Ihnen, dem Modell auf Wunsch einen Anzeigenamen zu geben. |
| `contents` | Alle übrigen Schnittstellendaten werden hier als ein Array von Attributdefinitionen platziert. Jedes Attribut muss einen `@type` (eine *Eigenschaft*, *Telemetrie*, einen *Befehl*, eine *Beziehung* oder *Komponente*) enthalten, um die Art der Schnittstelleninformationen zu bestimmen, die beschrieben werden, und dann eine Reihe von Eigenschaften, die das eigentliche Attribut definieren (z. B. `name` und `schema` zur Definition einer *Eigenschaft*). |

> [!NOTE]
> Beachten Sie, dass die Komponentenschnittstelle (in diesem Beispiel *Crater*) im gleichen Array definiert ist wie die Schnittstelle, die sie verwendet (*Planet*). Komponenten müssen auf diese Weise in API-Aufrufen definiert werden, damit die Schnittstelle gefunden werden kann.

### <a name="possible-schemas"></a>Mögliche Schemas

Gemäß DTDL kann das Schema für die Attribute *Eigenschaft* und *Telemetrie* aus einfachen Standardtypen (`integer`, `double`, `string` und `Boolean`) und anderen Typen wie `DateTime` und `Duration` bestehen. 

Zusätzlich zu den einfachen Typen können die Felder *Eigenschaft* und *Telemetrie* diese komplexen Typen enthalten:
* `Object`
* `Map`
* `Enum`

*Telemetrie* felder unterstützen auch `Array`.

### <a name="model-inheritance"></a>Vererbung im Modell

Mitunter möchten Sie ein Modell eventuell weiter spezialisieren. Beispielsweise kann es sinnvoll sein, mit einem generischen Modell wie *Raum* und spezialisierte Varianten *Konferenzraum* und *Fitnessraum* zu arbeiten. Um Spezialisierung auszudrücken, unterstützt DTDL Vererbung. Schnittstellen können von einer oder mehreren anderen Schnittstellen erben. 

Im folgenden Beispiel wird das Modell *Planet* aus dem vorherigen DTDL-Beispiel als Untertyp des größeren Modells *CelestialBody* neu konzipiert. Das übergeordnete Modell wird zuerst definiert. Anschließend baut das untergeordnete Modell unter Verwendung des Felds `extends` darauf auf.

:::code language="json" source="~/digital-twins-docs-samples/models/CelestialBody-Planet-Crater.json":::

In diesem Beispiel trägt *CelestialBody* einen Namen, eine Masse und eine Temperatur zu *Planet* bei. Der Abschnitt `extends` ist ein Schnittstellenname oder Array von Schnittstellennamen (der es der erweiternden Schnittstelle ermöglicht, falls gewünscht, von mehreren übergeordneten Modellen zu erben).

Nach Anwendung der Vererbung macht die erweiternde Schnittstelle alle Eigenschaften in der gesamten Vererbungskette verfügbar.

Die erweiternde Schnittstelle kann keine Definitionen der übergeordneten Schnittstellen ändern, sondern sie nur ergänzen. Sie kann auch keine Fähigkeit neu definieren, die bereits in einer ihrer übergeordneten Schnittstellen definiert ist (selbst wenn die Fähigkeiten als gleich definiert sind). Wenn beispielsweise eine übergeordnete Schnittstelle die `double`-Eigenschaft *Masse* definiert, kann die erweiternde Schnittstelle keine Deklaration von *Masse* enthalten, selbst wenn es sich ebenfalls um eine `double`-Eigenschaft handelt.

## <a name="best-practices-for-designing-models"></a>Bewährte Methoden für das Entwerfen von Modellen

Wenn Sie beim Entwerfen von Modellen die Entitäten in Ihrer Umgebung wiedergeben möchten, kann es hilfreich sein, vorauszuschauen und die Implikationen zu betrachten, die Ihr Entwurf hinsichtlich [Abfragen](concepts-query-language.md) beinhaltet. Sie können Eigenschaften so entwerfen, dass Diagrammdurchläufe bei großen Resultsets vermieden werden. Sie können Beziehungen, die in einer einzelnen Abfrage beantwortet werden müssen, auch als einstufige Beziehungen modellieren.

### <a name="validating-models"></a>Validieren von Modellen

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

## <a name="tools-for-models"></a>Tools für Modelle 

Es gibt mehrere verfügbare Beispieltools, die die Arbeit mit Modellen und Ontologien vereinfachen. Diese finden Sie im folgenden Repository: [Tools für DTDL (Digital Twins Definition Language)](https://github.com/Azure/opendigitaltwins-tools).

In diesem Abschnitt werden die aktuellen Beispiele ausführlicher beschrieben.

### <a name="model-uploader"></a>Modelluploader 

_**Zum Hochladen von Modellen in Azure Digital Twins**_

Sobald Sie Ihre Modelle erstellt, erweitert oder ausgewählt haben, können Sie diese in Ihre Azure Digital Twins-Instanz hochladen, um sie zur Verwendung in Ihrer Lösung zur Verfügung zu stellen. Hierzu verwenden Sie die [Azure Digital Twins-APIs](how-to-use-apis-sdks.md) wie unter [*Exemplarische Vorgehensweise: Verwalten von DTDL-Modellen*](how-to-manage-model.md#upload-models) beschrieben.

Wenn Sie jedoch viele Modelle hochladen müssen oder viele gegenseitige Abhängigkeiten vorliegen, die einzelne Uploads erschweren würden, können Sie das folgende Beispiel verwenden, um mehrere Modelle gleichzeitig hochzuladen: [**Azure Digital Twins-Modelluploader**](https://github.com/Azure/opendigitaltwins-building-tools/tree/master/ModelUploader). Befolgen Sie die im Beispiel bereitgestellten Anweisungen, um dieses Projekt zu konfigurieren und zum Hochladen von Modellen in Ihre eigene Instanz zu verwenden.

### <a name="model-visualizer"></a>Modellvisualisierer 

_**Zum Visualisieren von Modellen**_

Nachdem Sie Modelle in Ihre Azure Digital Twins-Instanz hochgeladen haben, können Sie mithilfe des [**Azure Digital Twins-Modellvisualisierers**](https://github.com/Azure/opendigitaltwins-building-tools/tree/master/AdtModelVisualizer) die Modelle in Ihrer Azure Digital Twins-Instanz anzeigen (einschließlich ggf. vorhandener Vererbungen und Modellbeziehungen). Dieses Beispiel befindet sich derzeit in der Entwurfsphase. Die Digital Twins-Entwicklercommunity kann das Beispiel erweitern und dazu beitragen. 

## <a name="next-steps"></a>Nächste Schritte

* Hier erfahren Sie mehr über das Erstellen von Modellen basierend auf Branchenstandardontologien: [*Konzepte: Was ist eine Ontologie?*](concepts-ontologies.md)

* Ausführliche Informationen zur Verwaltung von Modellen mit API-Vorgängen: [*Vorgehensweise: Verwalten von DTDL-Modellen*](how-to-manage-model.md)

* Hier erfahren Sie, wie Modelle zum Erstellen digitaler Zwillinge verwendet werden: [*Konzepte: Digital Twins und der Digital Twins-Graph*](concepts-twins-graph.md)

