---
title: Verwalten und Ermitteln von Daten in Azure Blob Storage mit Blob Index (Vorschau)
description: Erfahren Sie, wie Sie Blob Index-Tags zum Kategorisieren, Verwalten und Abfragen von Daten verwenden, um Blobobjekte zu ermitteln.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/24/2020
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 3e5507069a3e1eeadfaf4c3eeee288b2651e88a1
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996039"
---
# <a name="manage-and-find-data-on-azure-blob-storage-with-blob-index-preview"></a>Verwalten und Ermitteln von Daten in Azure Blob Storage mit Blob Index (Vorschau)

Bei immer größer werdenden Datasets kann die Suche nach einem bestimmten Objekt schwierig und frustrierend sein. Blob Index bietet Funktionen zur Datenverwaltung und -ermittlung anhand von Schlüssel-Wert-Indextagattributen. Mithilfe dieser Attribute können Sie Objekte innerhalb eines einzelnen Containers oder in allen Containern Ihres Speicherkontos kategorisieren und ermitteln. Bei sich ändernden Datenanforderungen können Objekte dynamisch neu kategorisiert werden, indem die entsprechenden Indextags aktualisiert werden. Die vorhandene Containerorganisation bleibt dabei erhalten. Durch die Verwendung von Blob Index kann die Entwicklung vereinfacht werden, indem Ihre Blobdaten und die zugehörigen Indexattribute innerhalb desselben Diensts konsolidiert werden. Auf diese Weise lassen sich mithilfe nativer Funktionen effiziente und skalierbare Anwendungen erstellen. 

Blob Index ermöglicht Folgendes:

- Dynamisches Kategorisieren Ihrer Blobs mithilfe von Schlüssel-Wert-Indextags für die Datenverwaltung
- Schnelles Auffinden bestimmter getaggter Blobs innerhalb eines einzelnen Containers oder innerhalb des gesamten Speicherkontos
- Angeben von bedingtem Verhalten für Blob-APIs basierend auf der Auswertung von Indextags
- Verwenden von Indextags für erweiterte Steuerungsmöglichkeiten bei Blobplattformfunktionen wie der [Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md)

Gehen wir von einem Szenario aus, in dem Sie über Millionen von Blobs in Ihrem Speicherkonto verfügen, die von einer Vielzahl unterschiedlicher Anwendungen geschrieben werden und auf die eine große Anzahl von Anwendungen zugreifen. Sie möchten nach allen zugehörigen Daten eines einzelnen Projekts suchen. Da die Daten auf mehrere Container mit unterschiedlichen Blobnamenskonventionen verteilt sein können, sind Sie unsicher, welche Bereiche Sie in Ihre Suche einschließen sollen. Sie wissen jedoch, dass Ihre Anwendungen alle Daten mit Tags hochladen, die auf dem jeweiligen Projekt und einer entsprechenden Beschreibung basieren. Anstatt Millionen von Blobs zu durchsuchen, um Namen und Eigenschaften zu vergleichen, können Sie ganz einfach `Project = Contoso` als Kriterium für die Ermittlung verwenden. Blob Index filtert alle Container innerhalb Ihres gesamten Speicherkontos, um in kürzester Zeit nur die fünfzig Blobs aus `Project = Contoso` zurückzugeben. 

Beispiele für den Einstieg in Blob Index finden Sie unter [Verwenden von Blob Index zum Verwalten und Suchen von Daten](storage-blob-index-how-to.md).

## <a name="blob-index-tags-and-data-management"></a>Blob Index-Tags und Datenverwaltung

Container- und Blobnamenpräfixe ermöglichen eine eindimensionale Kategorisierung Ihrer gespeicherten Daten. Mit Blob Index können Sie ab sofort eine mehrdimensionale Kategorisierung für all Ihre [Blobdatentypen (Block, Append oder Page)](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) mit angewendeten Attributtags vornehmen. Diese mehrdimensionale Kategorisierung wird nativ indiziert und verfügbar gemacht, damit Sie Ihre Daten in kürzester Zeit finden.

Gehen wir von den folgenden fünf Blobs in Ihrem Speicherkonto aus:
>
> container1/transaction.csv  
> container2/campaign.docx  
> photos/bannerphoto.png  
> archives/completed/2019review.pdf  
> logs/2020/01/01/logfile.txt  
>

Diese Blobs werden derzeit durch ein Präfix des Containernamens/Namens des virtuellen Ordners/Blobnamens getrennt. Bei Blob Index können Sie das Indextagattribut `Project = Contoso` für diese fünf Blobs festlegen, um sie derselben Kategorie zuzuweisen, ohne die aktuelle Präfixorganisation zu ändern. Durch die Möglichkeit, Daten anhand des mehrdimensionalen Index der Speicherplattform zu filtern und zu ermitteln, ist es nicht mehr notwendig, Daten zu verschieben.

## <a name="setting-blob-index-tags"></a>Festlegen von Blob Index-Tags

Bei Blob Index-Tags handelt es sich um Schlüssel-Wert-Attribute, die auf neue oder vorhandene Objekte innerhalb Ihres Speicherkontos angewendet werden können. Sie können die Indextags während des Uploadvorgangs über die Vorgänge PutBlob, PutBlockList oder CopyBlob und den optionalen x-ms-tags-Header angeben. Wenn Sie bereits über Blobs in Ihrem Speicherkonto verfügen, können Sie SetBlobTags mit einem formatierten XML-Dokument aufrufen und dabei die Blobindex-Tagattribute im Text der Anforderung angeben. 

Nachfolgend sind Beispieltags aufgeführt, die festgelegt werden können

Sie können ein einzelnes Tag auf Ihr Blob anwenden, um anzugeben, wann die Verarbeitung Ihrer Daten abgeschlossen wurde.
>
> "processedDate" = '2020-01-01'
>
Sie können mehrere Tags auf Ihr Blob anwenden, um eine ausführlichere Beschreibung für die Daten hinzuzufügen.
>
> "Project" = 'Contoso'  
> "Classified" = 'True'  
> "Status" = 'Unprocessed'  
> "Priority" = '01' 
>

Um die vorhandenen Indextagattribute zu ändern, müssen Sie die vorhandenen Tagattribute zunächst abrufen. Anschließend ersetzen Sie sie mit dem SetBlobTags-Vorgang. Um alle Indextags von einem Blob zu entfernen, rufen Sie den SetBlobTags-Vorgang ohne Tagattribute auf. Da Blobindextags eine Unterressource der Blobdateninhalte sind, ändert SetBlobTags weder die zugrunde liegenden Inhalte, noch die Uhrzeit der letzten Änderung eines Blobs.

Die unten aufgeführten Einschränkungen gelten für Blobindextags:
- Jedes Blob kann über bis zu zehn Blobindextags verfügen
- Tagschlüssel müssen zwischen 1 und 128 Zeichen umfassen
- Tagwerte müssen zwischen 0 und 256 Zeichen umfassen
- Bei Tagschlüsseln und -werten wird die Groß-/Kleinschreibung beachtet
- Tagschlüssel und -werte unterstützen lediglich Zeichenfolgendatentypen. Zahlen, Datumsangaben, Uhrzeitangaben oder Sonderzeichen werden als Zeichenfolgen gespeichert
- Tagschlüssel und -werte müssen den folgenden Benennungsregeln entsprechen:
  - Alphanumerische Zeichen: a-z, A-Z, 0-9
  - Sonderzeichen: Leerzeichen, Pluszeichen, Minuszeichen, Punkt, Doppelpunkt, Gleichheitszeichen, Unterstrich, Schrägstrich

## <a name="getting-and-listing-blob-index-tags"></a>Abrufen und Auflisten von Blob Index-Tags

Blobindextags werden als Unterressource gemeinsam mit den Blobdaten gespeichert und können unabhängig von den zugrunde liegenden Blobdateninhalten abgerufen werden. Nach der Festlegung können die Blobindextags für ein einzelnes Blob umgehend über den GetBlobTags-Vorgang abgerufen und überprüft werden. Auch der ListBlobs-Vorgang mit dem `include:tags`-Parameter gibt alle Blobs innerhalb eines Containers sowie die angewendeten Blobindextags zurück. 

Für alle Blobs mit mindestens einem Blobindextag wird in den Vorgängen ListBlobs, GetBlob und GetBlobProperties „x-ms-tag-count“ zurückgegeben. Der jeweilige Wert gibt die Anzahl von Blobindextags an, die in einem Blob vorhanden sind.

## <a name="finding-data-using-blob-index-tags"></a>Suchen nach Daten mithilfe von Blob Index-Tags

Wenn für Ihre Blobs Blobindextags festgelegt wurden, macht das Indizierungsmodul diese Schlüssel-Wert-Attribute in einem mehrdimensionalen Index verfügbar. Während Ihre Indextags im Blob vorliegen und umgehend abgerufen werden können, kann es einige Zeit dauern, bis der Blobindex mit den aktualisierten Indextagattributen aktualisiert wurde. Sobald der Blobindex aktualisiert wurde, können Sie die nativen Abfrage- und Ermittlungsfunktionen des Blobspeichers nutzen.

Über den FindBlobsByTags-Vorgang können Sie eine gefilterte Rückgabe mit Blobs abrufen, deren Indextags einem bestimmten Blobindex-Abfrageausdruck entsprechen. Blob Index unterstützt das Filtern von Daten innerhalb aller Container oder innerhalb eines einzelnen Containers in Ihrem Speicherkonto. Da es sich bei allen Tagschlüsseln und -werten des Blobindex um Zeichenfolgen handelt, verwenden die unterstützten relationalen Operatoren eine lexikografische Sortierung für die Indextagwerte.

Für die Filterung des Blobindex gelten folgende Kriterien:
-   Tagschlüssel müssen in doppelte Anführungszeichen (") eingeschlossen werden
-   Tagwerte und Containernamen müssen in einfache Anführungszeichen (') eingeschlossen werden
-   Das @-Zeichen ist nur für das Filtern nach einem bestimmten Containernamen zulässig (d. h. @container = 'ContainerName')
- Filter werden mit lexikografischer Sortierung auf Zeichenfolgen angewendet
-   Vorgänge für einseitige Bereiche sind für ein und denselben Schlüssel nicht unzulässig (d. h. "Rank" > '10' AND "Rank" >= '15')
- Bei Verwendung von REST zum Erstellen von Filterausdrücken müssen Zeichen URI-codiert sein

In der Tabelle unten sind alle zulässigen Operatoren für FindBlobsByTags aufgeführt:

|  Operator  |  BESCHREIBUNG  | Beispiel |
|------------|---------------|---------|
|     =      |     Gleich     | "Status" = 'In Progress' | 
|     >      |  Größer als |  "Date" > '2018-06-18' |
|     >=     |  Größer oder gleich | "Priority" >= '5' | 
|     <      |  Kleiner als    | "Age" < '32' |
|     <=     |  Kleiner oder gleich  | "Company" <= 'Contoso' |
|    AND     |  Logisches AND  | "Rank" >= '010' AND "Rank" < '100' |
| @container |  Eingrenzung auf einen bestimmten Container   | @container = 'videofiles' AND "status" = 'done' |

> [!NOTE]
> Wenn Sie Tags festlegen und abfragen, sollten Ihnen die lexikografische Reihenfolge bekannt sein.
> - Zahlen kommen vor Buchstaben. Zahlen werden basierend auf der ersten Ziffer sortiert.
> - Großbuchstaben kommen vor Kleinbuchstaben.
> - Symbole sind keine Standardzeichen. Einige Symbole kommen vor numerischen Werten. Andere Symbole kommen vor oder nach Buchstaben.
>

## <a name="conditional-blob-operations-with-blob-index-tags"></a>Bedingte Blobvorgänge mit Blob Index-Tags
In REST-Versionen ab 2019-10-10 unterstützen die meisten [Blob-Dienst-APIs](https://docs.microsoft.com/rest/api/storageservices/operations-on-blobs) jetzt den bedingten Header „x-ms-if-tags“. Mit diesem Header ist ein Vorgang nur dann erfolgreich, wenn die angegebene Blobindexbedingung erfüllt ist. Wenn die Bedingung nicht erfüllt ist, wird folgender Fehler ausgegeben: `error 412: The condition specified using HTTP conditional header(s) is not met`.

Der Header „x-ms-if-tags“ kann mit den übrigen vorhandenen bedingten HTTP-Headern (If-Match, If-None-Match usw.) kombiniert werden.  Wenn in einer Anforderung mehrere bedingte Header angegeben werden, müssen die Bedingungen aller Header erfüllt sein, damit der Vorgang erfolgreich ist.  Alle bedingten Header werden effektiv mit einem logischen AND kombiniert. 

In der Tabelle unten sind alle zulässigen Operatoren für Bedingungsvorgänge aufgeführt:

|  Operator  |  BESCHREIBUNG  | Beispiel |
|------------|---------------|---------|
|     =      |     Gleich     | "Status" = 'In Progress' |
|     <>     |   Ungleich   | "Status" <> 'Done'  | 
|     >      |  Größer als |  "Date" > '2018-06-18' |
|     >=     |  Größer oder gleich | "Priority" >= '5' | 
|     <      |  Kleiner als    | "Age" < '32' |
|     <=     |  Kleiner oder gleich  | "Company" <= 'Contoso' |
|    AND     |  Logisches AND  | "Rank" >= '010' AND "Rank" < '100' |
|     oder     |  Logisches OR   | "Status" = 'Done' OR "Priority" >= '05' |

> [!NOTE]
> Es gibt zwei zusätzliche Operatoren („Ungleich“ und „logisches OR“), die im bedingten Header „x-ms-if-tags“ für Blobvorgänge zulässig, im FindBlobsByTags-Vorgang jedoch nicht vorhanden sind.

## <a name="platform-integrations-with-blob-index-tags"></a>Plattformintegration bei Blob Index-Tags

Blob Index-Tags sind nicht nur zum Kategorisieren, Verwalten und Durchsuchen Ihrer Blobdaten nützlich, sondern bieten zudem auch Integrationen in andere Blob-Dienstfeatures wie die [Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md). 

### <a name="lifecycle-management"></a>Lebenszyklusverwaltung

Mithilfe des neuen blobIndexMatch-Vorgangs als Regelfilter in der Lebenszyklusverwaltung können Sie Daten in kältere Ebenen verschieben oder basierend auf den Indextags löschen, die auf Ihre Blobs angewendet wurden. Dadurch können Sie Ihre Regeln präziser definieren und Daten nur dann verschieben oder löschen, wenn sie den angegebenen Tagkriterien entsprechen.

Sie können einen Blobindexabgleich als eigenständigen Filtersatz in einer Lebenszyklusregel festlegen, um Aktionen auf getaggte Daten anzuwenden. Alternativ können Sie einen Präfixabgleich und einen Blobindexabgleich kombinieren, um Übereinstimmungen für spezifischere Datasets zu ermitteln. Wenn mehrere Filter auf eine Lebenszyklusregel angewendet werden, wird dies wie ein Vorgang mit logischem AND behandelt. Die Aktion wird also nur dann angewendet, wenn alle Filterkriterien erfüllt sind. 

Das folgende Beispiel einer Lebenszyklusverwaltungs-Regel gilt für Blockblobs im Container „videofiles“. Blobs werden nur dann in die Archivspeicherebene verschoben, wenn die Daten die Blobindextag-Kriterien ```"Status" = 'Processed' AND "Source" == 'RAW'``` erfüllen.

# <a name="portal"></a>[Portal](#tab/azure-portal)
![Beispiel einer Blobindexabgleichs-Regel für die Lebenszyklusverwaltung im Azure-Portal](media/storage-blob-index-concepts/blob-index-lifecycle-management-example.png)
   
# <a name="json"></a>[JSON](#tab/json)
```json
{
    "rules": [
        {
            "enabled": true,
            "name": "ArchiveProcessedSourceVideos",
            "type": "Lifecycle",
            "definition": {
                "actions": {
                    "baseBlob": {
                        "tierToArchive": {
                            "daysAfterModificationGreaterThan": 0
                        }
                    }
                },
                "filters": {
                    "blobIndexMatch": [
                        {
                            "name": "Status",
                            "op": "==",
                            "value": "Processed"
                        },
                        {
                            "name": "Source",
                            "op": "==",
                            "value": "RAW"
                        }
                    ],
                    "blobTypes": [
                        "blockBlob"
                    ],
                    "prefixMatch": [
                        "videofiles/"
                    ]
                }
            }
        }
    ]
}
```
---

## <a name="permissions-and-authorization"></a>Berechtigungen und Autorisierung

Sie können Zugriffsberechtigungen für Blob Index über eine der folgenden Methoden erteilen:

- Über die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC), um Berechtigungen für einen Azure Active Directory-Sicherheitsprinzipal (Azure AD) zu erteilen. Microsoft empfiehlt die Verwendung von Azure AD für mehr Sicherheit und Benutzerfreundlichkeit. Weitere Informationen zur Verwendung von Azure AD mit Blobvorgängen finden Sie unter [Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory](../common/storage-auth-aad.md).
- Durch Delegieren des Zugriffs auf den Blobindex mithilfe einer Shared Access Signature (SAS). Weitere Informationen zu SAS (Shared Access Signatures) finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../common/storage-sas-overview.md).
- Durch Verwendung der Kontozugriffsschlüssel zur Autorisierung von Vorgängen mit gemeinsam verwendetem Schlüssel. Weitere Informationen finden Sie unter [Authentifizieren mit gemeinsam verwendetem Schlüssel](/rest/api/storageservices/authorize-with-shared-key).

Blob Index-Tags sind eine Unterressource der Blobdaten. Ein Benutzer mit Berechtigungen oder SAS-Token für das Lesen oder Schreiben von Blobs kann möglicherweise nicht auf die Blobindextags zugreifen. 

### <a name="role-based-access-control"></a>Rollenbasierte Access Control 
Benutzern, die eine [AAD-Identität](../common/storage-auth-aad.md) verwenden, können die folgenden Berechtigungen für die Arbeit mit Blobindextags zugewiesen werden. 

|   Blobvorgänge   |  RBAC-Aktion   |
|---------------------|----------------|
| Suchen nach Blobs anhand von Tags  | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/filter |
| Festlegen von Blobtags         | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/write | 
| Abrufen von Blobtags         | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/read |

Für die Arbeit mit Tags sind zusätzliche Berechtigungen erforderlich, die über die Berechtigungen für die zugrunde liegenden Blobdaten hinausgehen. Der Rolle „Mitwirkender an Storage-Blobdaten“ werden alle drei Berechtigungen erteilt. Der Rolle „Storage-Blobdatenleser“ werden nur die Berechtigungen „Suchen nach Blobs anhand von Tags“ und „Abrufen von Blobtags“ erteilt.

### <a name="sas-permissions"></a>SAS-Berechtigungen 
Benutzern, die eine [Shared Access Signature (SAS)](../common/storage-sas-overview.md) verwenden, können bereichsbezogene Berechtigungen für das Arbeiten mit Blobtags erteilt werden.
#### <a name="blob-sas"></a>Blob-SAS
Die folgenden Berechtigungen können in einer Blob-Dienst-SAS erteilt werden, um den Zugriff auf Blobindextags zu gestatten. Lese- und Schreibberechtigungen für Blobs allein reichen nicht aus, um die zugehörigen Indextags lesen oder schreiben zu können.

|  Berechtigung  |  URI-Symbol  | Zulässige Vorgänge |
|--------------|--------------|--------------------|
|  Indextags  |      t         | Abrufen und Festlegen von Blobindextags für ein Blob |

#### <a name="container-sas"></a>Container-SAS
Die folgenden Berechtigungen können in einer Containerdienst-SAS erteilt werden, um das Filtern anhand von Blobtags zu gestatten.  Die Berechtigung zum Auflisten von Blobs reicht nicht aus, um Blobs nach ihren Indextags filtern zu können.

|  Berechtigung  |  URI-Symbol  | Zulässige Vorgänge |
|--------------|--------------|--------------------|
| Indextags     |      f       | Suchen nach Blobs anhand von Blobindextags | 

## <a name="choosing-between-metadata-and-blob-index-tags"></a>Unterschiede zwischen Metadaten und Blob Index-Tags 
Sowohl Blob Index-Tags als auch Metadaten ermöglichen das Speichern beliebiger benutzerdefinierter Schlüssel-Wert-Eigenschaften mit einer Blobressource. Beide können direkt abgerufen und festgelegt werden, ohne den Inhalt des Blobs zurückzugeben oder zu ändern. Sie können sowohl Metadaten als auch Indextags verwenden.

Allerdings werden nur Blob Index-Tags automatisch indiziert und durch den nativen Blobdienst abfragbar. Metadaten können nicht nativ indiziert und abgefragt werden, es sei denn, Sie verwenden einen separaten Dienst wie z. B. [Azure Search](../../search/search-blob-ai-integration.md). Darüber hinaus verfügen Blob Index-Tags über zusätzliche Berechtigungen zum Lesen/Filtern und Schreiben, die unabhängig von den zugrunde liegenden Blobdaten erteilt werden. Bei Metadaten werden dieselben Berechtigungen verwendet wie beim Blob. Sie werden als HTTP-Header in den Vorgängen GetBlob oder GetBlobProperties zurückgegeben. Blob Index-Tags werden im Ruhezustand mithilfe eines [von Microsoft verwalteten Schlüssels](../common/storage-service-encryption.md) verschlüsselt. Metadaten hingegen werden im Ruhezustand mithilfe desselben Verschlüsselungsschlüssels verschlüsselt, der für Blobdaten angegeben wurde. 

In der folgenden Tabelle werden die Unterschiede zwischen Metadaten und Blob Index-Tags veranschaulicht:

|              |   Metadaten   |   Blob Index-Tags  |
|--------------|--------------|--------------------|
| **Einschränkungen**         | Kein numerisches Limit; 8 KB insgesamt; keine Beachtung der Groß-/Kleinschreibung | Maximal 10 Tags pro Blob; 768 Bytes pro Tag; Beachtung der Groß-/Kleinschreibung |
| **Updates**      | Auf Archivebene nicht zulässig; SetBlobMetadata ersetzt alle vorhandenen Metadaten; SetBlobMetadata ändert die Uhrzeit der letzten Änderung des Blobs | Für alle Zugriffsebenen zulässig; SetBlobTags ersetzt alle vorhandenen Tags; SetBlobTags ändert nicht die Uhrzeit der letzten Änderung des Blobs |
| **Storage**        | Speicherung mit den Blobdaten |  Unterressource der Blobdaten | 
| **Indizierung & Abfragen** | Nativ nicht verfügbar, Verwendung eines separaten Diensts wie Azure Search erforderlich | Ja, native Indizierungs- und Abfragefunktionen im Blobspeicher integriert |
| **Verschlüsselung** | Verschlüsselung im Ruhezustand mit demselben Verschlüsselungsschlüssel, der auch für Blobdaten verwendet wird |  Verschlüsselung im Ruhezustand mit einem von Microsoft verwalteten Verschlüsselungsschlüssel |
| **Preise**   | Die Größe der Metadaten ist in den Speicherkosten für ein Blob enthalten |    Fixkosten pro Indextag | 
| **Headerantwort** | Metadaten werden als Header in GetBlob und GetBlobProperties zurückgegeben | Rückgabe von TagCount in GetBlob oder GetBlobProperties; Tags werden nur in GetBlobTags und ListBlobs zurückgegeben |
| **Berechtigungen**  |    Lese- oder Schreibberechtigungen für Blobdaten gelten auch für Metadaten |    Zusätzliche Berechtigungen zum Lesen, Filtern oder Schreiben von Tags erforderlich |

## <a name="pricing"></a>Preise
Die aktuellen Preise für Blob Index gelten für die öffentliche Vorschau und können sich bei allgemeiner Verfügbarkeit ändern. Kunden wird die Gesamtzahl von Blobindextags innerhalb eines Speicherkontos in Rechnung gestellt (Durchschnitt pro Monat). Für das Indizierungsmodul fallen keine Kosten an. Bei Anforderungen an SetBlobTags, GetBlobTags und FindBlobsByTags fallen die Kosten für den jeweiligen Vorgangstyp an. Weitere Informationen finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="regional-availability-and-storage-account-support"></a>Regionale Verfügbarkeit und Unterstützung von Speicherkonten

Blob Index ist derzeit nur für GPv2-Knoten (General Purpose v2) verfügbar, bei denen der hierarchische Namespace (HNS) deaktiviert ist. GPV1-Konten (General Purpose v1) werden nicht unterstützt. Sie können ein GPv1-Konto jedoch auf ein GPv2-Konto aktualisieren. Weitere Informationen zu Speicherkonten finden Sie unter [Azure-Speicherkonto – Übersicht](../common/storage-account-overview.md).

Die öffentliche Vorschau von Blob Index ist derzeit nur in den folgenden Regionen verfügbar:
- Kanada, Mitte
- Kanada, Osten
- Frankreich, Mitte
- Frankreich, Süden

Weitere Informationen zum Einstieg in Blob Index finden Sie unter [Verwenden von Blob Index zum Verwalten und Suchen von Daten](storage-blob-index-how-to.md).

> [!IMPORTANT]
> Informationen hierzu finden Sie im Abschnitt zu Bedingungen dieses Artikels. Informationen zum Registrieren für die Vorschau finden Sie im Abschnitt zum Registrieren Ihres Abonnements dieses Artikels. Sie müssen Ihr Abonnement registrieren, bevor Sie Blob Index für Ihre Speicherkonten verwenden können.

### <a name="register-your-subscription-preview"></a>Registrieren Ihres Abonnements (Vorschau)
Da Blob Index nur in der öffentlichen Vorschau verfügbar ist, müssen Sie Ihr Abonnement registrieren, um das Feature verwenden zu können. Um eine Anforderung zu senden, führen Sie die folgenden PowerShell- oder CLI-Befehle aus.

#### <a name="register-by-using-powershell"></a>Registrieren über PowerShell
```powershell
Register-AzProviderFeature -FeatureName BlobIndex -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

#### <a name="register-by-using-azure-cli"></a>Registrieren mithilfe der Azure CLI
```azurecli
az feature register --namespace Microsoft.Storage --name BlobIndex
az provider register --namespace 'Microsoft.Storage'
```

## <a name="conditions-and-known-issues-preview"></a>Bedingungen und bekannte Probleme (Vorschau)
In diesem Abschnitt sind bekannte Probleme und Bedingungen in der aktuellen öffentlichen Vorschauversion von Blob Index beschrieben. Wie bei den meisten Vorschauversionen sollte dieses Feature bis zum Erreichen der allgemeinen Verfügbarkeit nicht für Produktionsworkloads verwendet werden, da sich Verhaltensweisen möglicherweise ändern.

-   Um die Vorschauversion von Blob Index in den unterstützten Regionen zu verwenden, müssen Sie zuerst Ihr Abonnement registrieren.
-   Aktuell werden nur GPv2-Konten mit der Vorschau unterstützt. Blob-, BlockBlobStorage- und HNS-aktivierte DataLake Gen2-Konten werden derzeit nicht mit Blob Index unterstützt. GPv1-Konten werden nicht unterstützt.
-   Beim Hochladen von Seitenblobs mit Indextags werden die Tags derzeit nicht persistent gespeichert. Sie müssen die Tags nach dem Hochladen eines Seitenblobs festlegen.
-   Wenn die Filterung auf einen einzelnen Container beschränkt ist, kann @container nur übergeben werden, wenn alle Indextags im Filterausdruck Gleichheitsprüfungen sind (Schlüssel=Wert). 
-   Wenn der Bereichsoperator mit der AND-Bedingung verwendet wird, muss derselbe Indextag-Schlüsselname angeben werden (Age > ‘013’ AND Age < ‘100’).
-   Die Versionsverwaltung wird mit Blob Index derzeit nicht unterstützt. Blob Index-Tags werden für verschiedene Versionen beibehalten, derzeit jedoch nicht an das Blobindexmodul übermittelt.
-   Ein Kontofailover wird derzeit nicht unterstützt. Der Blobindex wird nach einem Failover möglicherweise nicht ordnungsgemäß aktualisiert.
-   Die Lebenszyklusverwaltung unterstützt beim Blobindexabgleich derzeit nur Gleichheitsprüfungen.
-   CopyBlob kopiert keine Blobindextags vom Quellblob in das neue Zielblob. Sie können während des Kopiervorgangs die Tags angeben, die auf das Zielblob angewendet werden sollen. 
-   Tags werden beim Erstellen von Momentaufnahmen persistent gespeichert. Das Höherstufen von Momentaufnahmen wird derzeit jedoch nicht unterstützt und kann einen leeren Tagsatz zur Folge haben.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="can-blob-index-help-me-filter-and-query-content-inside-my-blobs"></a>Kann ich Blob Index zum Filtern und Abfragen von Inhalten in meinen Blobs verwenden? 
Nein, Blob Index-Tags können verwendet werden, um nach bestimmten Blobs zu suchen. Wenn Sie die Inhalte Ihrer Blobs durchsuchen müssen, verwenden Sie die Abfragebeschleunigung oder Azure Search.

### <a name="are-there-any-special-considerations-regarding-blob-index-tag-values"></a>Gibt es spezielle Überlegungen in Bezug auf Blob Index-Tagwerte?
Blob Index-Tags unterstützen nur String-Datentypen, und bei Abfragen werden Ergebnisse in lexikografischer Reihenfolge zurückgegeben. Zahlen sollten mit Nullen aufgefüllt werden. Datums- und Uhrzeitangaben sollten in einem mit ISO 8601 kompatiblen Format gespeichert werden.

### <a name="are-blob-index-tags-and-azure-resource-manager-tags-related"></a>Sind Blob Index-Tags mit Azure Resource Manager-Tags verwandt?
Nein, mit Azure Resource Manager-Tags lassen sich Ressourcen der Steuerungsebene organisieren (Abonnements, Ressourcengruppen und Speicherkonten). Blob Index-Tags bieten Funktionen zur Objektverwaltung und -ermittlung für Ressourcen der Datenebene (z. B. Blobs innerhalb eines Speicherkontos).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich ein Beispiel zur Verwendung von Blob Index an. Weitere Informationen finden Sie unter [Verwenden von Blob Index zum Verwalten und Suchen von Daten](storage-blob-index-how-to.md)

