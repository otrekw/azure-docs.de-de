---
title: Konvertieren von Branchenstandardmodellen
titleSuffix: Azure Digital Twins
description: Grundlegendes zum Muster der Konvertierung von Branchenstandardmodellen (RDF/OWL) in DTDL
author: baanders
ms.author: baanders
ms.date: 9/28/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: bf39bd0c787ed64d573cc2a257442b4d3bb8ab7d
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92015113"
---
# <a name="convert-industry-standard-models-to-dtdl-for-azure-digital-twins"></a>Konvertieren von Branchenstandardmodellen in DTDL für Azure Digital Twins

Modelle in Azure Digital Twins werden in der auf JSON-LD basierenden Sprache [**DTDL (Digital Twins Definition Language)** ](concepts-models.md) dargestellt. Bei vorhandenen Modellen außerhalb von Azure Digital Twins, die auf einem Branchenstandard (z. B. RDF oder OWL) basieren, müssen Sie **die Modelle in DTDL konvertieren**, um sie mit Azure Digital Twins verwenden zu können. Die DTDL-Version wird dann zur allgemeingültigen Datenbasis für das Modell in Azure Digital Twins.

In diesem Artikel wird ein Muster für die Konvertierung von RDF-basierten Branchenmodellen oder benutzerdefinierten Modellen in DTDL beschrieben. Sie hat folgenden Inhalt:
* **Strategieanleitung**, die auf eine Vielzahl von Standards und Modelltypen angewandt werden kann
* [**Beispielcode** für einen RDF-spezifischen Konverter](#sample-converter-application)

## <a name="industry-models"></a>Branchenmodelle  

Die Verwendung von Branchenmodellen bietet einen guten Ausgangspunkt für den Entwurf Ihres Azure Digital Twins-Modells. Die Verwendung von Branchenmodellen unterstützt außerdem die Standardisierung und Informationsfreigabe. 

Einige gängige Branchenmodelle:  

| Schlüsselbranche | Modell |
| --- | --- | 
| Gebäude- und Facilitymanagement | [RealEstateCore](https://www.realestatecore.io/)<br>[BRICK-Schema](https://brickschema.org/ontology/1.1/)<br>[Building Topology Ontology (BOT)](https://w3c-lbd-cg.github.io/bot/)<br>[Semantic Sensor Network](https://www.w3.org/TR/vocab-ssn/)<br>[buildingSmart Industry Foundation Classes (IFC)](https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/) |
| Smart Citys | [ETSI NGSI-LD](https://www.etsi.org/deliver/etsi_gr/CIM/001_099/008/01.01.01_60/gr_CIM008v010101p.pdf)<br>[Smart Applications REFerence (SAREF)](https://saref.etsi.org/)<br>[Fiware](https://www.fiware.org/)<br>[Open & Agile Smart Cities (OASC)](https://oascities.org/) |
| Stromnetz | [CIM](https://cimug.ucaiug.org/)/[IEC 61968](https://en.wikipedia.org/wiki/IEC_61968)<br>[ADRM-Modelle](http://www.adrm.com/) für den Energie- und Rohstoffhandel | 

Je nach Ihren Anforderungen können Sie mit DTDL auch Branchenmodelle anpassen oder erweitern oder ein eigenes benutzerdefiniertes Modell von Grund auf entwickeln. 

## <a name="create-and-edit-models"></a>Erstellen und Bearbeiten von Modellen

Im ersten Schritt der Modellierung erstellen Sie Ihre Modelle. Sie können eigene Branchenstandardmodelle erstellen und umfassend bearbeiten, bevor Sie sie in DTDL konvertieren, oder Sie können sie in einem frühen Stadium in DTDL konvertieren und dann als DTDL-Dokumente weiter bearbeiten.

### <a name="with-industry-standards"></a>Mit Branchenstandards

Die meisten Branchenmodelle (auch als **Ontologien** bezeichnet) basieren auf semantischen Webstandards wie [OWL](https://www.w3.org/OWL/[), [RDF](https://www.w3.org/2001/sw/wiki/RDF) und [RDFS](https://www.w3.org/2001/sw/wiki/RDFS). 

In manchen Fällen möchten Sie ggf. ein Modell mithilfe von OWL-basierten Modelltools erstellen oder bearbeiten. Ein OWL-basiertes Modell können Sie mit beliebig vielen Modellerstellungstools entwerfen, einschließlich der folgenden.
* [WebProtégé](https://protege.stanford.edu/products.php#web-protege) und [Protégé Desktop](https://protege.stanford.edu/products.php#desktop-protege) sind gängige Beispiele. Sie können Branchenmodelle in mehreren Formaten importieren, ein Modell bearbeiten oder erweitern und das Modell exportieren. 
* [WebVOWL](http://www.visualdataweb.de/webvowl/) ist ein weiteres beliebtes Tool zur Visualisierung von Modellen. 

Wenn Sie ein Modell mit einem anderen Branchenstandard als DTDL erstellen, müssen Sie es in DTDL konvertieren und in Azure Digital Twins hochladen. 

#### <a name="common-model-file-formats"></a>Allgemeine Modelldateiformate 

RDF, OWL und RDFS sind die grundlegenden Bausteine des semantischen Webs. 

**RDF** bietet eine konzeptionelle Struktur zum Beschreiben von Elementen in Form von **Tripeln**. Tripel bestehen aus drei Einheiten: **Subjekt**, **Prädikat** und **Objekt**. Objekte können aus URIs bestehen. 

Beispiele für RDF-Tripel:

```
<LogicalDevice> <hasCapabiity> <Temperature>
<Chiller> <locatedIn> <Level1>
<Asset> <isPartOf> <Asset>  
```

Alle diese Beispiele sind gültige RDF-Anweisungen, die letzte Anweisung ist jedoch semantisch ungültig. In diesem Fall greift die OWL-Spezifikation. **OWL** definiert, was im Hinblick auf eine gültige Ontologie in RDF geschrieben werden kann.

Ein Beispiel für die Verwendung von OWL: 

```
<Asset> <isPartOf> <Building>
<TemperatureSensor> <isType> <Sensor>
```

**RDFS** umfasst zusätzliche Semantik für das Vokabular zur Definition und Beschreibung von Klassen. Eine dieser Klassen ist beispielsweise `rdfs:subClassOf`:

```
<Equipment> <subClassOf> <Asset>
```

Modelle können in vielen Dateiformaten gespeichert, importiert und exportiert werden, z. B. in folgenden Formaten:  
* RDF/XML 
* Turtle (TTL) 
* OWL/XML 

### <a name="with-dtdl"></a>Mit DTDL

In Azure Digital Twins wird die auf JSON-LD basierende Sprache  **DTDL (Digital Twin Definition Language)** für die Modellierung verwendet. Jedes Modell, das mit Azure Digital Twins verwendet wird, muss entweder vollständig in DTDL geschrieben oder in DTDL konvertiert werden.

Bei Verwendung von Modellen in DTDL können Sie die für  [Visual Studio Code](https://code.visualstudio.com/) verfügbare [**DTDL-Erweiterung**](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl)  nutzen, die eine Syntaxüberprüfung und andere Features zum Schreiben von DTDL-Dokumenten umfasst.

Weitere Informationen zu Azure Digital Twins-Modellen und den zugehörigen Komponenten finden Sie unter [*Konzepte: Benutzerdefinierte Modelle*](concepts-models.md) und[ *Verwalten von Azure Digital Twins-Modellen*](how-to-manage-model.md).

## <a name="convert-models-to-dtdl"></a>Konvertieren von Modellen in DTDL

Damit ein Modell mit Azure Digital Twins verwendet werden kann, muss es im DTDL-Format vorliegen. In diesem Abschnitt wird beschrieben, wie Sie RDF-basierte Modelle in DTDL konvertieren, damit sie mit Azure Digital Twins verwendet werden können.

Zur Konvertierung von RDF-basierten Modellen in DTDL können mehrere Bibliotheken von Drittanbietern verwendet werden. Mit einigen dieser Bibliotheken können Sie die Modelldatei in ein Diagramm laden. Sie können das Diagramm durchlaufen, um nach spezifischen RDFS- und OWL-Konstrukten zu suchen und diese in DTDL zu konvertieren.   

In der folgenden Tabelle sind Beispiele dafür aufgeführt, wie RDFS- und OWL-Konstrukte in DTDL zugeordnet werden können. 

| RDFS/OWL-Konzept | RDFS/OWL-Konstrukt | DTDL-Konzept | DTDL-Konstrukt |
| --- | --- | --- | --- |
| Klassen | `owl:Class`<br>IRI-Suffix<br>``rdfs:label``<br>``rdfs:comment`` | Schnittstelle | `@type:Interface`<br>`@id`<br>`displayName`<br>`comment` 
| Unterklassen von  werden erstellt. | `owl:Class`<br>IRI-Suffix<br>`rdfs:label`<br>`rdfs:comment`<br>`rdfs:subClassOf` | Schnittstelle | `@type:Interface`<br>`@id`<br>`displayName`<br>`comment`<br>`extends` 
| Datentypeigenschaften | `owl:DatatypeProperty`<br>`rdfs:label` oder `INode`<br>`rdfs:label`<br>`rdfs:range` | Schnittstelleneigenschaften | `@type:Property`<br>`name`<br>`displayName`<br>`schema` 
| Objekteigenschaften | `owl:ObjectProperty`<br>`rdfs:label` oder `INode`<br>`rdfs:range`<br>`rdfs:comment`<br>`rdfs:label` | Relationship | `type:Relationship`<br>`name`<br>`target` (oder weggelassen, wenn ohne `rdfs:range`)<br>`comment`<br>`displayName`<br>

Im folgenden C#-Codeausschnitt wird eine RDF-Modelldatei unter Verwendung der [**dotNetRDF**](https://www.dotnetrdf.org/)-Bibliothek in ein Diagramm geladen und in DTDL konvertiert. 

```csharp
using VDS.RDF.Ontology; 
using VDS.RDF.Parsing; 
using Microsoft.Azure.DigitalTwins.Parser; 

//...

Console.WriteLine("Reading file..."); 

FileLoader.Load(_ontologyGraph, rdfFile.FullName); 

// Start looping through for each owl:Class 
foreach (OntologyClass owlClass in _ontologyGraph.OwlClasses) 
{ 

    // Generate a DTMI for the owl:Class 
    string Id = GenerateDTMI(owlClass); 

    if (!String.IsNullOrEmpty(Id)) 
    { 

        Console.WriteLine($"{owlClass.ToString()} -> {Id}"); 

        // Create Interface
        DtdlInterface dtdlInterface = new DtdlInterface 
        { 
            Id = Id, 
            Type = "Interface", 
            DisplayName = GetInterfaceDisplayName(owlClass), 
            Comment = GetInterfaceComment(owlClass), 
            Contents = new List<DtdlContents>() 
        }; 

        // Use DTDL 'extends' for super classes 
        IEnumerable<OntologyClass> foundSuperClasses = owlClass.DirectSuperClasses; 

        //... 
     }

     // Add interface to the list of interfaces 
     _interfaceList.Add(dtdlInterface); 
} 

// Serialize to JSON 
var json = JsonConvert.SerializeObject(_interfaceList); 

//...
``` 

## <a name="validate-and-upload-dtdl-models"></a>Überprüfen und Hochladen von DTDL-Modellen

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

Nachdem ein Modell konvertiert und überprüft wurde, können Sie **es in Ihre Azure Digital Twins-Instanz hochladen**. Weitere Informationen zu diesem Prozess finden Sie im Abschnitt [*Hochladen von Modellen*](how-to-manage-model.md#upload-models) unter *Verwalten benutzerdefinierter Modelle*.

## <a name="sample-converter-application"></a>Beispielkonverteranwendung 

Es steht eine Beispielanwendung zur Verfügung, mit der eine RDF-basierte Modelldatei für die Verwendung im Azure Digital Twins-Dienst in [DTDL (Version 2)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) konvertiert werden kann. Dabei handelt es sich um die .NET Core-Befehlszeilenanwendung **RdfToDtdlConverter**.

Hier können Sie die Beispielanwendung herunterladen: [**RdfToDtdlConverter**](https://docs.microsoft.com/samples/azure-samples/rdftodtdlconverter/digital-twins-model-conversion-samples/). 

Um den Code auf Ihren Computer herunterzuladen, klicken Sie auf die Schaltfläche *ZIP herunterladen* unterhalb des Titels auf der Landing Page der Beispielanwendung. Dadurch wird eine *ZIP-Datei* mit dem Namen *RdfToDtdlConverter_sample_application_to_convert_RDF_to_DTDL.zip* heruntergeladen, die Sie dann entzippen und ausprobieren können.

Sie können sich mit dieser Beispielanwendung die Konvertierungsmuster im Kontext ansehen und als Baustein für Ihre eigenen Anwendungen verwenden. Außerdem können Sie entsprechend Ihren spezifischen Anforderungen Modellkonvertierungen durchführen.

## <a name="next-steps"></a>Nächste Schritte 

Erfahren Sie mehr über das Entwerfen und Verwalten von Modellen digitaler Zwillinge:
 
* [*Konzepte: Grundlegendes zu Zwillingsmodellen in Azure Digital Twins*](concepts-models.md)
* [*Verwenden Verwalten von Azure Digital Twins-Modellen*](how-to-manage-model.md)
* [*Verwenden Analysieren und Validieren von Modellen*](how-to-parse-models.md)