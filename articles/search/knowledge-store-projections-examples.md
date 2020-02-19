---
title: Definieren von Projektionen in einem Wissensspeicher
titleSuffix: Azure Cognitive Search
description: Beispiele für gängige Muster für das Projizieren von angereicherten Dokumenten in den Wissensspeicher für die Verwendung mit Power BI oder Azure Machine Learning.
manager: eladz
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: f29f4b91b85c0027df4be2fd5f26ef8f9749fe33
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77163819"
---
# <a name="knowledge-store-projections-how-to-shape-and-export-enrichments-to-the-knowledge-store"></a>Wissensspeicherprojektionen: Strukturieren und Exportieren von Anreicherungen in den Wissensspeicher

> [!IMPORTANT] 
> „Wissensspeicher“ ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Previewfunktionen werden von der [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Die Portalunterstützung ist momentan eingeschränkt, und das .NET SDK wird nicht unterstützt.

Projektionen sind der physische Ausdruck angereicherter Dokumente in einem Wissensspeicher. Für eine effektive Verwendung Ihrer angereicherten Dokumente ist eine Struktur erforderlich. In diesem Artikel untersuchen Sie sowohl die Struktur als auch die Beziehungen und erfahren, wie Sie Projektionseigenschaften erstellen und wie Sie Daten über die von Ihnen erstellten Projektionstypen hinweg verknüpfen. 

Zum Erstellen einer Projektion müssen Sie die Daten mit einer Shaper-Qualifikation strukturieren, um ein benutzerdefiniertes Objekt zu erstellen, oder die Inline-Strukturierungssyntax verwenden. Eine Datenform enthält alle Daten, die Sie projizieren möchten. Dieses Dokument enthält für jede Option ein Beispiel. Sie können eine der Optionen für die von Ihnen erstellten Projektionen auswählen.


Es stehen drei Typen von Projektionen zur Verfügung:
+ Tabellen
+ Objekte
+ Dateien

Tabellenprojektionen werden in Azure Table Storage gespeichert. Objekt- und Dateiprojektionen werden in Blob Storage geschrieben, Objektprojektionen werden als JSON-Dateien gespeichert und können Inhalte aus dem Dokument sowie Ausgaben oder Anreicherungen von Qualifikationen enthalten. Die Anreicherungspipeline kann auch Binärdateien wie Bilder extrahieren. Diese Binärdateien werden als Dateiprojektionen projiziert. Wenn ein binäres Objekt als Objektprojektion projiziert wird, werden nur die zugeordneten Metadaten als JSON-Blob gespeichert. 

Um die Überschneidungen zwischen Datenformen und Projektionen zu verstehen, verwenden wir das folgende Skillset als Grundlage für die Untersuchung verschiedener Konfigurationen. Dieses Skillset verarbeitet unformatierte Bilder und Textinhalte. Projektionen werden aus dem Inhalt des Dokuments und den Ausgaben der Qualifikationen für die Szenarien definiert, die unterstützt werden sollen.

Alternativ können Sie ein [REST-API-Beispiel](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/projections/Projections%20Docs.postman_collection.json) mit allen Aufrufen in dieser exemplarischen Vorgehensweise herunterladen und verwenden.

> [!IMPORTANT] 
> Beim Experimentieren mit Projektionen empfiehlt es sich, die [Eigenschaft für den Indexercache festzulegen](search-howto-incremental-index.md), um die Kosten unter Kontrolle zu halten. Das Bearbeiten von Projektionen führt dazu, dass das gesamte Dokument erneut angereichert wird, wenn der Indexercache nicht festgelegt ist. Wenn der Cache festgelegt ist und nur die Projektionen aktualisiert wurden, führen Skillsetausführungen für zuvor angereicherte Dokumente nicht zu Cognitive Services-Gebühren.


```json
{
    "name": "azureblob-skillset",
    "description": "Skillset created from the portal. skillsetName: azureblob-skillset; contentField: merged_content; enrichmentGranularity: document; knowledgeStoreStorageAccount: confdemo;",
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "name": "#1",
            "description": null,
            "context": "/document/merged_content",
            "categories": [
                "Person",
                "Quantity",
                "Organization",
                "URL",
                "Email",
                "Location",
                "DateTime"
            ],
            "defaultLanguageCode": "en",
            "minimumPrecision": null,
            "includeTypelessEntities": null,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/language"
                }
            ],
            "outputs": [
                {
                    "name": "persons",
                    "targetName": "people"
                },
                {
                    "name": "organizations",
                    "targetName": "organizations"
                },
                {
                    "name": "locations",
                    "targetName": "locations"
                },
                {
                    "name": "entities",
                    "targetName": "entities"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
            "name": "#2",
            "description": null,
            "context": "/document/merged_content",
            "defaultLanguageCode": "en",
            "maxKeyPhraseCount": null,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/language"
                }
            ],
            "outputs": [
                {
                    "name": "keyPhrases",
                    "targetName": "keyphrases"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
            "name": "#3",
            "description": null,
            "context": "/document",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                }
            ],
            "outputs": [
                {
                    "name": "languageCode",
                    "targetName": "language"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
            "name": "#4",
            "description": null,
            "context": "/document",
            "insertPreTag": " ",
            "insertPostTag": " ",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/content"
                },
                {
                    "name": "itemsToInsert",
                    "source": "/document/normalized_images/*/text"
                },
                {
                    "name": "offsets",
                    "source": "/document/normalized_images/*/contentOffset"
                }
            ],
            "outputs": [
                {
                    "name": "mergedText",
                    "targetName": "merged_content"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
            "name": "#5",
            "description": null,
            "context": "/document/normalized_images/*",
            "textExtractionAlgorithm": "printed",
            "lineEnding": "Space",
            "defaultLanguageCode": "en",
            "detectOrientation": true,
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                }
            ],
            "outputs": [
                {
                    "name": "text",
                    "targetName": "text"
                },
                {
                    "name": "layoutText",
                    "targetName": "layoutText"
                }
            ]
        }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "DemosCS",
        "key": "<COGNITIVE SERVICES KEY>"
    },
    "knowledgeStore": null
}
```

Nun können Sie das `knowledgeStore`-Objekt hinzufügen und die Projektionen für jedes der Szenarien nach Bedarf konfigurieren. 

## <a name="projecting-to-tables-for-scenarios-like-power-bi"></a>Projizieren in Tabellen für Szenarien wie Power BI

> [!NOTE] 
> Da der Wissensspeicher ein Azure Storage-Konto ist, sind Tabellenprojektionen Azure Storage-Tabellen und unterliegen den Speichergrenzwerten für Tabellen. Weitere Informationen finden Sie unter [Table Storage-Grenzwerte](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). Sie sollten auch wissen, dass die Entitätsgröße 1 MB nicht überschreiten und eine einzelne Eigenschaft nicht größer als 64 KB sein darf. Aufgrund dieser Einschränkungen stellen Tabellen eine gute Lösung für die Speicherung einer großen Anzahl kleiner Entitäten dar.

Power BI kann aus Tabellen lesen und Beziehungen basierend auf den Schlüsseln ermitteln, die von den Projektionen des Wissensspeichers erstellt werden. Aus diesem Grund eignen sich Tabellen gut für Projektdaten, wenn Sie versuchen, ein Dashboard für Ihre angereicherten Daten zu erstellen. Angenommen, Sie versuchen, ein Dashboard zu erstellen, in dem Sie die aus Dokumenten extrahierten Schlüsselausdrücke als Begriffswolke visualisieren können. Durch das Hinzufügen einer Shaper-Qualifikation zum Skillset können Sie eine benutzerdefinierte Form erstellen, die alle dokumentspezifischen Details und Schlüsselausdrücke enthält. Fügen Sie dem Skillset die Shaper-Qualifikation hinzu, um eine neue Anreicherung namens ```pbiShape``` im ```document``` zu erstellen.

### <a name="using-a-shaper-skill-to-create-a-custom-shape"></a>Verwenden einer Shaper-Qualifikation zum Erstellen einer benutzerdefinierten Form

Erstellen Sie eine benutzerdefinierte Form, die Sie in Table Storage projizieren können. Ohne eine benutzerdefinierte Form kann eine Projektion nur auf einen einzelnen Knoten verweisen (eine Projektion pro Ausgabe). Durch das Erstellen einer benutzerdefinierten Form können Sie verschiedene Elemente in einer neuen logischen Einheit aggregieren, die als eine einzelne Tabelle projiziert oder in Segmenten auf eine Sammlung von Tabellen verteilt werden kann. In diesem Beispiel kombiniert die benutzerdefinierte Form Metadaten und identifizierte Entitäten und Schlüsselbegriffe. Das Objekt heißt pbiShape und ist `/document` untergeordnet. 

> [!IMPORTANT] 
> Quellpfade für Anreicherungen müssen wohlgeformte JSON-Objekte sein, bevor sie projiziert werden können. Die Anreicherungsstruktur kann Anreicherungen repräsentieren, die keine wohlgeformten JSON-Objekte sind, beispielsweise wenn eine Anreicherung einem einfachen Typ wie einer Zeichenfolge untergeordnet ist. Beachten Sie, dass `KeyPhrases` und `Entities` von einem gültigen JSON-Objekt mit dem `sourceContext` umschlossen werden. Dies ist erforderlich, da `keyphrases` und `entities` Anreicherungen von einfachen Typen sind und in gültige JSON-Objekte konvertiert werden müssen, bevor sie projiziert werden können.

```json
{
            "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
            "name": "ShaperForTables",
            "description": null,
            "context": "/document",
            "inputs": [
                {
                    "name": "metadata_storage_content_type",
                    "source": "/document/metadata_storage_content_type",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "metadata_storage_name",
                    "source": "/document/metadata_storage_name",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "metadata_storage_path",
                    "source": "/document/metadata_storage_path",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "metadata_content_type",
                    "source": "/document/metadata_content_type",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "keyPhrases",
                    "source": null,
                    "sourceContext": "/document/merged_content/keyphrases/*",
                    "inputs": [
                        {
                            "name": "KeyPhrases",
                            "source": "/document/merged_content/keyphrases/*"
                        }

                    ]
                },
                {
                    "name": "Entities",
                    "source": null,
                    "sourceContext": "/document/merged_content/entities/*",
                    "inputs": [
                        {
                            "name": "Entities",
                            "source": "/document/merged_content/entities/*/name"
                        }

                    ]
                }
            ],
            "outputs": [
                {
                    "name": "output",
                    "targetName": "pbiShape"
                }
            ]
        }
```
Fügen Sie die soeben definierte Shaper-Qualifikation der Qualifikationsliste im Skillset hinzu. 

Nachdem Sie nun über alle Daten verfügen, die für das Projizieren in Tabellen erforderlich sind, aktualisieren Sie das knowledgeStore-Objekt mit den Tabellendefinitionen. 

```json

"knowledgeStore" : {
    "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
    "projections": [
        {
            "tables": [
                {
                    "tableName": "pbiDocument",
                    "generatedKeyName": "Documentid",
                    "source": "/document/pbiShape"
                },
                {
                    "tableName": "pbiKeyPhrases",
                    "generatedKeyName": "KeyPhraseid",
                    "source": "/document/pbiShape/keyPhrases/*"
                },
                {
                    "tableName": "pbiEntities",
                    "generatedKeyName": "Entityid",
                    "source": "/document/pbiShape/Entities/*"
                }
            ],
            "objects": [],
            "files": []
        }
    ]
}
```

Legen Sie die ```storageConnectionString```-Eigenschaft auf eine gültige Verbindungszeichenfolge für ein Speicherkonto vom Typ „Universell v2“ fest. In diesem Szenario definieren Sie drei Tabellen im Projektionsobjekt durch Festlegen der Eigenschaften ```tableName```, ```source``` und ```generatedKeyName```. Sie können nun das Skillset durch Ausgabe der PUT-Anforderung aktualisieren.

### <a name="slicing"></a>Aufteilen 

Wenn Sie mit einer konsolidierten Form beginnen, in der sich der gesamte zu projizierende Inhalt in einer einzelnen Form (oder einem Anreicherungsknoten) befindet, bietet das Aufteilen die Möglichkeit, einen einzelnen Knoten in mehrere Tabellen oder Objekte aufzuteilen. Hier wird das ```pbiShape```-Objekt in mehrere Tabellen aufgeteilt. Die Aufteilungsfunktion ermöglicht Ihnen, Teile der Form (```keyPhrases``` und ```Entities```) in separate Tabellen zu pullen. Dies ist nützlich, wenn jedem Dokument mehrere Entitäten und Schlüsselbegriffe zugeordnet sind. Durch das Aufteilen wird implizit eine Beziehung zwischen der übergeordneten und der untergeordneten Tabelle hergestellt. Dabei wird der ```generatedKeyName``` in der übergeordneten Tabelle verwendet, um in der untergeordneten Tabelle eine Spalte mit demselben Namen zu erstellen. 

### <a name="naming-relationships"></a>Namensbeziehungen
Mithilfe der Eigenschaften ```generatedKeyName``` und ```referenceKeyName``` werden Daten über Tabellen hinweg oder sogar über Projektionstypen hinweg miteinander verknüpft. Jede Zeile in der untergeordneten Tabelle/Projektion verfügt über eine Eigenschaft, die zurück auf das übergeordnete Element verweist. Der Name der Spalte oder Eigenschaft im untergeordneten Element ist der ```referenceKeyName``` des übergeordneten Elements. Wenn kein ```referenceKeyName``` bereitgestellt wird, verwendet der Dienst standardmäßig den ```generatedKeyName``` des übergeordneten Elements. Power BI benötigt diese generierten Schlüssel, um Beziehungen innerhalb der Tabellen zu ermitteln. Wenn Sie die Spalte in der untergeordneten Tabelle anders benennen möchten, legen Sie die ```referenceKeyName```-Eigenschaft der übergeordneten Tabelle fest. Ein Beispiel wäre das Festlegen von ```generatedKeyName``` auf die ID in der pbiDocument-Tabelle und von ```referenceKeyName``` auf die DocumentID. Dies würde dazu führen, dass die Spalte in den Tabellen pbiEntities und pbiKeyPhrases die Dokument-ID mit dem Namen DocumentID enthält.

Speichern Sie das aktualisierte Skillset, und führen Sie den Indexer aus. Damit verfügen Sie jetzt über eine funktionierende Projektion mit drei Tabellen. Wenn diese Tabellen in Power BI importiert werden, sollte Power BI die Beziehungen automatisch ermitteln.

## <a name="projecting-to-objects"></a>Projizieren in Objekte

Objektprojektionen unterliegen nicht denselben Einschränkungen wie Tabellenprojektionen und eignen sich besser für das Projizieren großer Dokumente. In diesem Beispiel projizieren Sie das gesamte Dokument in eine Objektprojektion. Objektprojektionen sind auf eine einzelne Projektion in einem Container beschränkt.
Zum Definieren einer Objektprojektion verwenden Sie das ```objects```-Array in den Projektionen. Sie können mithilfe der Shaper-Qualifikation eine neue Form generieren oder die Inline-Strukturierung der Objektprojektion verwenden. Während das Tabellenbeispiel den Ansatz der Erstellung einer Form und der anschließenden Aufteilung zeigt, veranschaulicht dieses Beispiel die Verwendung der Inline-Strukturierung. Inline-Strukturierung ist die Möglichkeit, eine neue Form in der Definition der Eingaben für eine Projektion zu erstellen. Bei der Inline-Strukturierung wird ein anonymes Objekt erstellt, das mit dem Ergebnis einer ähnlichen Shaper-Ausführung identisch ist. Die Inline-Strukturierung ist nützlich, wenn Sie eine Form definieren, die nicht wiederverwendet werden soll.
Die projections-Eigenschaft ist ein Array. In diesem Beispiel fügen wir dem Array eine neue Projektionsinstanz hinzu. Aktualisieren Sie die knowledgeStore-Definition mit den intern definierten Projektionen. Sie benötigen bei der Verwendung von Inline-Projektionen keine Shaper-Qualifikation.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
             {
                "tables": [ ],
                "objects": [
                    {
                        "storageContainer": "sampleobject",
                        "source": null,
                        "generatedKeyName": "myobject",
                        "sourceContext": "/document",
                        "inputs": [
                            {
                                "name": "metadata_storage_name",
                                "source": "/document/metadata_storage_name"
                            },
                            {
                                "name": "metadata_storage_path",
                                "source": "/document/metadata_storage_path"
                            },
                            {
                                "name": "content",
                                "source": "/document/content"
                            },
                            {
                                "name": "keyPhrases",
                                "source": "/document/merged_content/keyphrases/*"
                            },
                            {
                                "name": "entities",
                                "source": "/document/merged_content/entities/*/name"
                            },
                            {
                                "name": "ocrText",
                                "source": "/document/normalized_images/*/text"
                            },
                            {
                                "name": "ocrLayoutText",
                                "source": "/document/normalized_images/*/layoutText"
                            }
                        ]

                    }
                ],
                "files": []
            }
        ]
    }
```
## <a name="file-projections"></a>Dateiprojektionen

Dateiprojektionen sind Bilder, die entweder aus dem Quelldokument extrahiert wurden, oder Ausgaben von Anreicherungen darstellen, die aus dem Anreicherungsprozess projiziert werden können. Dateiprojektionen werden ähnlich wie Objektprojektionen als Blobs implementiert und enthalten das Bild. Zum Generieren einer Dateiprojektion verwenden Sie das ```files```-Array im Projektionsobjekt. In diesem Beispiel werden alle aus dem Dokument extrahierten Bilder in den Container `samplefile` projiziert.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
            {
                "tables": [ ],
                "objects": [ ],
                "files": [
                    {
                        "storageContainer": "samplefile",
                        "source": "/document/normalized_images/*"
                    }
                ]
            }
        ]
    }
```

## <a name="projecting-to-multiple-types"></a>Projizieren auf mehrere Typen

Ein komplexeres Szenario erfordert möglicherweise das Projizieren der Inhalte auf verschiedene Projektionstypen. Wenn Sie z. B. einige Daten wie Schlüsselausdrücke und Entitäten in Tabellen projizieren möchten, speichern Sie die OCR-Ergebnisse von Text und Layouttext als Objekte, und projizieren Sie die Bilder als Dateien. Dieses Update für das Skillset ermöglicht Folgendes:

1. Erstellen einer Tabelle mit einer Zeile für jedes Dokument
2. Erstellen einer Tabelle, die mit der Dokumenttabelle verknüpft ist, wobei jeder Schlüsselausdruck als Zeile in dieser Tabelle bezeichnet wird
3. Erstellen einer Tabelle, die mit der Dokumenttabelle verknüpft ist, wobei jede Entität als Zeile in dieser Tabelle bezeichnet wird
4. Erstellen einer Objektprojektion mit dem Layouttext für jedes Bild
5. Erstellen einer Dateiprojektion, die jedes extrahierte Bild projiziert
6. Erstellen einer Querverweistabelle mit Verweisen auf die Dokumenttabelle, die Objektprojektion mit dem Layouttext und die Dateiprojektion

Fügen Sie zunächst dem skill-Array, mit dem ein geformtes Objekt erstellt werden soll, eine neue Shaper-Qualifikation hinzu. 

```json
{
            "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
            "name": "ShaperForCross",
            "description": null,
            "context": "/document",
            "inputs": [
                {
                    "name": "metadata_storage_name",
                    "source": "/document/metadata_storage_name",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "keyPhrases",
                    "source": null,
                    "sourceContext": "/document/merged_content/keyphrases/*",
                    "inputs": [
                        {
                            "name": "KeyPhrases",
                            "source": "/document/merged_content/keyphrases/*"
                        }

                    ]
                },
                {
                    "name": "entities",
                    "source": null,
                    "sourceContext": "/document/merged_content/entities/*",
                    "inputs": [
                        {
                            "name": "Entities",
                            "source": "/document/merged_content/entities/*/name"
                        }

                    ]
                },
                {
                    "name": "images",
                    "source": null,
                    "sourceContext": "/document/normalized_images/*",
                    "inputs": [
                        {
                            "name": "image",
                            "source": "/document/normalized_images/*"
                        },
                        {
                            "name": "layoutText",
                            "source": "/document/normalized_images/*/layoutText"
                        },
                        {
                            "name": "ocrText",
                            "source": "/document/normalized_images/*/text"
                        }
                        ]
                }
                
            ],
            "outputs": [
                {
                    "name": "output",
                    "targetName": "crossProjection"
                }
            ]
        }
```

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
             {
                "tables": [
                    {
                        "tableName": "crossDocument",
                        "generatedKeyName": "Id",
                        "source": "/document/crossProjection"
                    },
                    {
                        "tableName": "crossEntities",
                        "generatedKeyName": "EntityId",
                        "source": "/document/crossProjection/entities/*"
                    },
                    {
                        "tableName": "crossKeyPhrases",
                        "generatedKeyName": "KeyPhraseId",
                        "source": "/document/crossProjection/keyPhrases/*"
                    },
                    {
                        "tableName": "crossReference",
                        "generatedKeyName": "CrossId",
                        "source": "/document/crossProjection/images/*"
                    }
                     
                ],
                "objects": [
                    {
                        "storageContainer": "crossobject",
                        "generatedKeyName": "crosslayout",
                        "source": null,
                        "sourceContext": "/document/crossProjection/images/*/layoutText",
                        "inputs": [
                            {
                                "name": "OcrLayoutText",
                                "source": "/document/crossProjection/images/*/layoutText"
                            }
                        ]
                    }
                ],
                "files": [
                     {
                        "storageContainer": "crossimages",
                        "generatedKeyName": "crossimages",
                        "source": "/document/crossProjection/images/*/image"
                    }
                    ]
                
            }
        ]
    }
```

Objektprojektionen erfordern einen Containernamen für jede Projektion. Objektprojektionen oder Dateiprojektionen dürfen nicht im selben Container verwendet werden. 

### <a name="relationships"></a>Beziehungen

In diesem Beispiel wird auch ein weiteres Feature von Projektionen verdeutlicht, indem mehrere Projektionstypen innerhalb desselben Projektionsobjekts definiert werden. Es gibt eine Beziehung innerhalb und zwischen den verschiedenen Projektionstypen (Tabellen, Objekte, Dateien), die Ihnen ermöglicht, mit einer Tabellenzeile für ein Dokument zu beginnen und den gesamten OCR-Text für die Bilder in diesem Dokument in der Objektprojektion zu suchen. Wenn Sie nicht möchten, dass die Daten in Beziehung zueinander gesetzt werden, definieren Sie die Projektionen in unterschiedlichen Projektionsobjekten. Der folgende Codeausschnitt führt z. B. dazu, dass zwar die Tabellen miteinander in Beziehung stehen, aber nicht die Tabellen und die OCR-Textprojektionen. Projektionsgruppen sind nützlich, wenn Sie dieselben Daten in unterschiedlichen Formen für verschiedene Anforderungen projizieren möchten. Beispiel: eine Projektionsgruppe für das Power BI-Dashboard und eine weitere Projektionsgruppe für die Verwendung der Daten zum Trainieren eines KI-Modells für eine Qualifikation.
Wenn Sie Projektionen für unterschiedliche Typen erstellen, werden zunächst die Datei- und Objektprojektionen generiert und die Pfade den Tabellen hinzugefügt.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
            {
                "tables": [
                    {
                        "tableName": "unrelatedDocument",
                        "generatedKeyName": "Documentid",
                        "source": "/document/pbiShape"
                    },
                    {
                        "tableName": "unrelatedKeyPhrases",
                        "generatedKeyName": "KeyPhraseid",
                        "source": "/document/pbiShape/keyPhrases"
                    }
                ],
                "objects": [
                    
                ],
                "files": []
            }, 
            {
                "tables": [],
                "objects": [
                    {
                        "storageContainer": "unrelatedocrtext",
                        "source": null,
                        "sourceContext": "/document/normalized_images/*/text",
                        "inputs": [
                            {
                                "name": "ocrText",
                                "source": "/document/normalized_images/*/text"
                            }
                        ]
                    },
                    {
                        "storageContainer": "unrelatedocrlayout",
                        "source": null,
                        "sourceContext": "/document/normalized_images/*/layoutText",
                        "inputs": [
                            {
                                "name": "ocrLayoutText",
                                "source": "/document/normalized_images/*/layoutText"
                            }
                        ]
                    }
                ],
                "files": []
            }
        ]
    }
```

Diese Beispiele veranschaulichen die allgemeinen Muster bei der Verwendung von Projektionen. Sie sollten nun auch über ein gutes Verständnis der Konzepte beim Erstellen einer Projektion für Ihr spezielles Szenario verfügen.

## <a name="common-issues"></a>Häufige Probleme

Beim Definieren einer Projektion treten einige Probleme häufiger auf, die zu unerwarteten Ergebnissen führen können.

1. Keine Zeichenfolgenanreicherungen für die Strukturierung. Wenn Zeichenfolgen angereichert werden (z. B. ```merged_content``` mit Schlüsselausdrücken), wird die angereicherte Eigenschaft als untergeordnetes Element von merged_content innerhalb der Anreicherungsstruktur dargestellt. Zur Projektionszeit muss diese jedoch in ein gültiges JSON-Objekt mit einem Namen und einem Wert transformiert werden.
2. Auslassen von ```/*``` am Ende eines Quellpfads. Wenn die Quelle einer Projektion z. B. ```/document/pbiShape/keyPhrases``` ist, wird das Array mit den Schlüsselausdrücken als einzelnes Objekt/einzelne Zeile projiziert. Wenn Sie den Quellpfad auf ```/document/pbiShape/keyPhrases/*``` festlegen, wird für jeden Schlüsselausdruck eine einzelne Zeile oder ein einzelnes Objekt erzeugt.
3. Bei Pfadselektoren wird die Groß-/Kleinschreibung berücksichtigt. Dies kann zu Warnungen über fehlende Eingaben führen, wenn Sie nicht die richtige Schreibung für den Selektor verwenden.

