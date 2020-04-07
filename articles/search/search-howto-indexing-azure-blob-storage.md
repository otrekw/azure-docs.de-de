---
title: Durchsuchen von Azure Blob Storage-Inhalten
titleSuffix: Azure Cognitive Search
description: Erfahren Sie, wie Sie Azure Blob Storage indizieren und Text aus Dokumenten mit der kognitiven Azure-Suche extrahieren.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 5df1198e6681431738f886eb7c3ad549936eab1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067642"
---
# <a name="how-to-index-documents-in-azure-blob-storage-with-azure-cognitive-search"></a>Indizieren von Dokumenten in Azure Blob Storage mit der kognitiven Azure-Suche

In diesem Artikel wird beschrieben, wie Sie die kognitive Azure-Suche zum Indizieren von Dokumenten (z. B. PDF- oder Microsoft Office-Dokumente und verschiedene andere gängige Formate) verwenden, die in Azure Blob Storage gespeichert sind. Zunächst werden grundlegende Informationen zu Einrichten und Konfigurieren eines Blobindexers erläutert. Anschließend folgt eine ausführlichere Betrachtung der Verhaltensweisen und Szenarien, die Ihnen voraussichtlich begegnen.

<a name="SupportedFormats"></a>

## <a name="supported-document-formats"></a>Unterstützte Dokumentformate
Der Blobindexer kann Text aus den folgenden Dokumentformaten extrahieren:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>Einrichten der Blobindizierung
Sie können einen Azure Blob Storage-Indexer über folgende Elemente einrichten:

* [Azure portal](https://ms.portal.azure.com)
* [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations) für die kognitive Azure-Suche
* [.NET SDK](https://aka.ms/search-sdk) für die kognitive Azure-Suche

> [!NOTE]
> Einige Features (z.B. Feldzuordnungen) sind im Portal noch nicht verfügbar und müssen programmgesteuert verwendet werden.
>

Hier wird der Ablauf unter Verwendung der REST-API veranschaulicht.

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle
Eine Datenquelle gibt an, welche Daten indiziert werden müssen. Sie legt außerdem die Anmeldeinformationen für den Zugriff auf die Daten sowie die Richtlinien fest, mit denen Änderungen an den Daten effizient identifiziert werden können (z.B. neue, geänderte oder gelöschte Zeilen). Eine Datenquelle kann von mehreren Indexern im selben Suchdienst verwendet werden.

Für die Blobindizierung muss die Datenquelle über die folgenden erforderlichen Eigenschaften verfügen:

* **name** ist der eindeutige Name der Datenquelle im Suchdienst.
* **type** muss `azureblob` lauten.
* Mit **credentials** wird die Speicherkonto-Verbindungszeichenfolge als `credentials.connectionString`-Parameter angegeben. Details finden Sie weiter unten unter [Angeben von Anmeldeinformationen](#Credentials).
* Mit **container** wird ein Container in Ihrem Speicherkonto angegeben. Standardmäßig können alle Blobs im Container abgerufen werden. Wenn Sie nur Blobs in einem bestimmten virtuellen Verzeichnis indizieren möchten, können Sie dieses Verzeichnis mit dem optionalen **query**-Parameter angeben.

So erstellen Sie eine Datenquelle:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Weitere Informationen über die API zum Erstellen einer Datenquelle finden Sie unter [Datenquelle erstellen](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Angeben von Anmeldeinformationen ####

Sie haben folgende Möglichkeiten zum Angeben der Anmeldeinformationen für den Blobcontainer:

- **Verbindungszeichenfolge für den Vollzugriff auf ein Speicherkonto**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` Sie können die Verbindungszeichenfolge über das Azure-Portal abrufen, indem Sie auf dem Blatt des Speicherkontos zu „Einstellungen“ > „Schlüssel“ (für klassische Speicherkonten) oder zu „Einstellungen“ > „Zugriffsschlüssel“ (für Azure Resource Manager-Speicherkonten) navigieren.
- Verbindungszeichenfolge für eine **Shared Access Signature (SAS) für ein Speicherkonto**: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl` Die SAS muss über Listen- und Leseberechtigungen für Container und Objekte (in diesem Fall Blobs) verfügen.
-  **Shared Access Signature des Containers**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl` Die SAS muss über Listen- und Leseberechtigungen für den Container verfügen.

Weitere Informationen zu Shared Access Signatures von Speichern finden Sie unter [Verwenden von Shared Access Signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Bei Verwendung von SAS-Anmeldeinformationen müssen Sie die Anmeldedaten für die Datenquellen in regelmäßigen Abständen mit erneuerten Signaturen aktualisieren, um den Ablauf zu verhindern. Falls SAS-Anmeldedaten ablaufen, tritt beim Indexer ein Fehler mit ungefähr folgender Fehlermeldung auf: `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Schritt 2: Erstellen eines Index
Mit dem Index werden die Felder in einem Dokument, Attribute und andere Konstrukte für die Suchoberfläche angegeben.

Hier sehen Sie, wie Sie einen Index mit einem durchsuchbaren `content`-Feld zum Speichern des aus Blobs extrahierten Texts erstellen:   

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Weitere Informationen zum Erstellen von Indizes finden Sie unter [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) (Index erstellen).

### <a name="step-3-create-an-indexer"></a>Schritt 3: Erstellen eines Indexers
Ein Indexer verbindet eine Datenquelle mit einem Zielsuchindex und stellt einen Zeitplan zur Automatisierung der Datenaktualisierung bereit.

Nach der Erstellung von Index und Datenquelle können Sie den Indexer erstellen:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Dieser Indexer wird alle zwei Stunden ausgeführt (das Planungsintervall wird auf „PT2H“ festgelegt). Um einen Indexer alle 30 Minuten auszuführen, legen Sie das Intervall auf „PT30M“ fest. Das kürzeste unterstützte Intervall beträgt fünf Minuten. Der Zeitplan ist optional. Ohne Zeitplan wird ein Indexer nur einmal bei seiner Erstellung ausgeführt. Allerdings können Sie ein Indexer bei Bedarf jederzeit ausführen.   

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Erstellen eines Indexers](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Weitere Informationen zum Definieren von Indexerzeitplänen finden Sie unter [Festlegen eines Zeitplans für Indexer in der kognitiven Azure-Suche](search-howto-schedule-indexers.md).

<a name="how-azure-search-indexes-blobs"></a>

## <a name="how-azure-cognitive-search-indexes-blobs"></a>Wie werden Blobs in der kognitiven Azure-Suche indiziert?

Je nach [Indexer-Konfiguration](#PartsOfBlobToIndex), kann der Blobindexer Metadaten und Textinhalt, Speicher- und Inhaltsmetadaten oder nur Speichermetadaten indizieren (nützlich, wenn Sie nur die Metadaten interessieren und den Inhalt des Blobs nicht indizieren müssen). Der Indexer extrahiert standardmäßig sowohl die Metadaten als auch den Inhalt.

> [!NOTE]
> Die Blobs mit strukturiertem Inhalt wie JSON oder CSV werden standardmäßig als ein einzelnes Textsegment indiziert. Wenn Sie JSON- und CSV-Blobs in einem strukturierten Verfahren indizieren möchten, finden Sie weitere Informationen unter [Indizieren von JSON-Blobs](search-howto-index-json-blobs.md) und [Indizieren von CSV-Blobs](search-howto-index-csv-blobs.md).
>
> Ein Verbunddokument oder eingebettetes Dokument (z.B. ein ZIP-Archiv oder ein Word-Dokument mit eingebetteter Outlook-E-Mail mit Anhängen) wird ebenfalls als einzelnes Dokument indiziert.

* Der Textinhalt des Dokuments wird in ein Zeichenfolgefeld mit dem Namen `content` extrahiert.

> [!NOTE]
> Die kognitive Azure-Suche beschränkt die Menge des extrahierten Texts abhängig vom Tarif: 32.000 Zeichen für den Tarif „Free“, 64.000 Zeichen für den Tarif „Basic“, 4 Millionen Zeichen für den Tarif „Standard“, 8 Millionen Zeichen für den Tarif „Standard S2“ und 16 Millionen Zeichen für den Tarif „Standard S3“. Für gekürzte Dokumente wird eine Warnung in die Statusantwort des Indexers einbezogen.  

* Falls für das Blob vom Benutzer angegebene Metadateneigenschaften vorhanden sind, werden diese „Wort für Wort“ extrahiert. Beachten Sie, dass dafür im Index ein Feld mit dem gleichen Namen wie der Metadatenschlüssel des Blobs definiert sein muss. Wenn Ihr Blob beispielsweise über den Metadatenschlüssel `Sensitivity` mit dem Wert `High` verfügt, sollten Sie ein Feld mit dem Namen `Sensitivity` in Ihrem Suchindex definieren, das dann mit dem Wert `High` aufgefüllt wird.
* Standardmäßige Blob-Metadateneigenschaften werden in die folgenden Felder extrahiert:

  * **metadata\_storage\_name** (Edm.String): der Dateiname des Blobs. Für ein Blob mit dem Namen „/my-container/my-folder/subfolder/resume.pdf“ lautet der Wert dieses Felds beispielsweise `resume.pdf`.
  * **metadata\_storage\_path** (Edm.String): der vollständige URI des Blobs, einschließlich Speicherkonto. Zum Beispiel, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **metadata\_storage\_content\_type** (Edm.String): Inhaltstyp, der mit dem Code angegeben wird, den Sie zum Hochladen des Blobs verwendet haben. Beispiel: `application/octet-stream`.
  * **metadata\_storage\_last\_modified** (Edm.DateTimeOffset): Zeitstempel der letzten Änderung für das Blob. In der kognitiven Azure-Suche wird dieser Zeitstempel zum Identifizieren geänderter Blobs verwendet, um die erneute Indizierung aller Elemente nach der ersten Indizierung zu vermeiden.
  * **metadata\_storage\_size** (Edm.Int64): Blob-Größe in Byte.
  * **metadata\_storage\_content\_md5** (Edm.String): MD5-Hash des Blob-Inhalts, falls verfügbar.
  * **metadata\_storage\_sas\_token** (Edm.String): ein temporäres SAS-Token, das von [benutzerdefinierten Qualifikationen](cognitive-search-custom-skill-interface.md) verwendet werden kann, um Zugriff auf den Blob zu erhalten. Dieses Token sollte nicht zur späteren Verwendung gespeichert werden, da es ablaufen kann.

* Die Metadateneigenschaften der einzelnen Dokumentformate werden in die [hier](#ContentSpecificMetadata) aufgeführten Felder extrahiert.

Es ist nicht erforderlich, Felder für alle obigen Eigenschaften in Ihrem Suchindex zu definieren. Erfassen Sie einfach die Eigenschaften, die Sie für Ihre Anwendung benötigen.

> [!NOTE]
> Die Feldnamen im vorhandenen Index unterscheiden sich von den Feldnamen, die während der Dokumentextrahierung generiert werden. Sie können **Feldzuordnungen** verwenden, um die in der kognitiven Azure-Suche bereitgestellten Eigenschaftennamen den Feldnamen in Ihrem Suchindex zuzuordnen. Weiter unten finden Sie ein Beispiel für die Verwendung von Feldzuordnungen.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Definieren von Dokumentschlüsseln und Feldzuordnungen
In der kognitiven Azure-Suche wird ein Dokument mit dem Dokumentschlüssel eindeutig identifiziert. Jeder Suchindex muss über genau ein Schlüsselfeld vom Typ „Edm.String“ verfügen. Das Schlüsselfeld ist für jedes Dokument erforderlich, das dem Index hinzugefügt wird (es ist auch das einzige erforderliche Feld).  

Sie sollten sorgfältig abwägen, welches extrahierte Feld Sie dem Schlüsselfeld für Ihren Index zuordnen. Die Kandidaten lauten:

* **metadata\_storage\_name**: Dies ist gegebenenfalls ein passender Kandidat. Beachten Sie aber, dass 1) die Namen unter Umständen nicht eindeutig sind, falls Blobs mit dem gleichen Namen in unterschiedlichen Ordnern enthalten sind, und 2) der Name Zeichen enthalten kann, die in Dokumentschlüsseln ungültig sind, z.B. Bindestriche. Als Lösung für ungültige Zeichen können Sie die `base64Encode`-[Feldzuordnungsfunktion](search-indexer-field-mappings.md#base64EncodeFunction) verwenden. Denken Sie in diesem Fall daran, die Dokumentschlüssel zu codieren, wenn Sie sie in API-Aufrufen übergeben, z.B. bei einem Lookup. (Unter .NET können Sie hierfür beispielsweise die [UrlTokenEncode-Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) verwenden.)
* **metadata\_storage\_path**: Die Verwendung des vollständigen Pfads sorgt für Eindeutigkeit, aber der Pfad enthält auf jeden Fall `/`-Zeichen, die [in einem Dokumentschlüssel ungültig sind](https://docs.microsoft.com/rest/api/searchservice/naming-rules).  Wie oben auch, haben Sie hierbei die Möglichkeit, die Schlüssel mit der `base64Encode`-[Funktion](search-indexer-field-mappings.md#base64EncodeFunction) zu codieren.
* Falls keine der Optionen oben für Sie geeignet ist, können Sie den Blobs eine benutzerdefinierte Metadateneigenschaft hinzufügen. Bei dieser Option ist es aber erforderlich, dass diese Metadateneigenschaft im Rahmen des Blob-Uploadvorgangs allen Blobs hinzugefügt wird. Da der Schlüssel eine erforderliche Eigenschaft ist, tritt für alle Blobs, die nicht über diese Eigenschaft verfügen, beim Indizieren ein Fehler auf.

> [!IMPORTANT]
> Wenn keine ausdrückliche Zuordnung für das Schlüsselfeld im Index vorhanden ist, wird in der kognitiven Azure-Suche automatisch `metadata_storage_path` als Schlüssel verwendet, und die Schlüsselwerte werden Base64-codiert (zweite Option oben).
>
>

Für dieses Beispiel wählen wir das Feld `metadata_storage_name` als Dokumentschlüssel aus. Nehmen wir außerdem an, dass Ihr Index über ein Schlüsselfeld mit dem Namen `key` und ein Feld `fileSize` zum Speichern der Dokumentgröße verfügt. Um alles wie gewünscht einzurichten, geben Sie die folgenden Feldzuordnungen an, wenn Sie den Indexer erstellen oder aktualisieren:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Hier wird beschrieben, wie Sie Feldzuordnungen hinzufügen und die Base64-Codierung von Schlüsseln für einen Indexer aktivieren, um alle Elemente zu verknüpfen:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> Weitere Informationen zu Feldzuordnungen finden Sie in [diesem Artikel](search-indexer-field-mappings.md).
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Steuern, welche Blobs indiziert werden
Sie können steuern, welche Blobs indiziert und welche übersprungen werden.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Indizieren von Blobs mit bestimmten Dateierweiterungen
Sie können nur die Blobs mit den Dateinamenerweiterungen indizieren, die Sie über den `indexedFileNameExtensions`-Konfigurationsparameter des Indexers angeben. Der Wert ist eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste von Dateierweiterungen (mit einem vorangestellten Punkt). Um beispielsweise nur die PDF- und DOCX-Blobs zu indizieren, gehen Sie folgendermaßen vor:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Ausschließen von Blobs mit bestimmten Dateierweiterungen
Mithilfe des `excludedFileNameExtensions`-Konfigurationsparameters können Sie verhindern, dass Blobs mit bestimmten Dateinamenerweiterungen indiziert werden. Der Wert ist eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste von Dateierweiterungen (mit einem vorangestellten Punkt). Um beispielsweise alle Blobs mit Ausnahme von Blobs mit den Erweiterungen PNG und JPEG zu indizieren, gehen Sie folgendermaßen vor:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Wenn sowohl der `indexedFileNameExtensions`- als auch der `excludedFileNameExtensions`-Parameter vorhanden ist, wird in der kognitiven Azure-Suche zunächst `indexedFileNameExtensions` und danach `excludedFileNameExtensions` überprüft. Das heißt, wenn die gleiche Dateierweiterung in beiden Listen vorhanden ist, wird sie von der Indizierung ausgeschlossen.

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Steuern, welche Teile des Blobs indiziert werden

Sie können mithilfe des Konfigurationsparameters `dataToExtract` steuern, welche Teile der Blobs indiziert werden. Die folgenden Werte sind möglich:

* `storageMetadata` ‒ Gibt an, dass nur die [standardmäßigen Blob-Eigenschaften und benutzerspezifischen Metadaten](../storage/blobs/storage-properties-metadata.md) indiziert werden
* `allMetadata` ‒ Gibt an, dass Speichermetadaten und die [inhaltstypspezifischen Metadaten](#ContentSpecificMetadata), die aus dem Blobinhalt extrahiert wurden, indiziert werden
* `contentAndMetadata` ‒ Gibt an, dass alle Metadaten und Textinhalte, die aus dem Blob extrahiert wurden, indiziert werden Dies ist der Standardwert.

Verwenden Sie z.B. Folgendes, um nur die Speichermetadaten zu indizieren:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Verwenden von Blob-Metadaten, um zu steuern, wie Blobs indiziert werden

Die oben beschriebenen Konfigurationsparameter werden auf alle Blobs angewendet. In manchen Fällen möchten Sie möglicherweise steuern, wie *einzelne Blobs* indiziert werden. Dies ist möglich, indem Sie die folgenden Eigenschaften und Werte der Blob-Metadaten hinzufügen:

| Eigenschaftenname | Eigenschaftswert | Erklärung |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Weist den Blobindexer an, das Blob vollständig zu überspringen. Weder die Metadaten- noch die Inhaltsextraktion werden versucht. Dies ist nützlich, wenn ein bestimmtes Blob wiederholt fehlschlägt und den Indizierungsprozess unterbricht. |
| AzureSearch_SkipContent |"true" |Dies entspricht der [oben](#PartsOfBlobToIndex) beschriebenen Einstellung `"dataToExtract" : "allMetadata"` zu einem bestimmten Blob. |

<a name="DealingWithErrors"></a>
## <a name="dealing-with-errors"></a>Fehlerkorrektur

Der Blobindexer wird standardmäßig beendet, sobald ein Blob mit einem nicht unterstützten Inhaltstyp (z.B. ein Bild) gefunden wird. Natürlich können Sie den Parameter `excludedFileNameExtensions` nutzen, um bestimmte Inhaltstypen zu überspringen. Allerdings müssen Sie möglicherweise Blobs indizieren, ohne im Voraus alle möglichen Inhaltstypen zu kennen. Legen Sie zum Fortsetzen der Indizierung beim Auftreten eines nicht unterstützten Inhaltstyps den Konfigurationsparameter `failOnUnsupportedContentType` auf `false` fest:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

Für einige Blobs kann in der kognitiven Azure-Suche der Inhaltstyp nicht bestimmt oder ein Dokument mit eigentlich unterstütztem Inhaltstyp nicht verarbeitet werden. Um diesen Fehlermodus zu ignorieren, legen Sie den Konfigurationsparameter `failOnUnprocessableDocument` auf FALSE fest:

      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }

Die kognitive Azure-Suche beschränkt die Größe der indizierten Blobs. Diese Grenzwerte sind unter [Dienstgrenzwerte in der kognitiven Azure-Suche](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity) angegeben. Zu große Blobs werden standardmäßig als Fehler behandelt. Sie können jedoch weiterhin Speichermetadaten übergroßer Blobs indizieren, wenn Sie den Konfigurationsparameter `indexStorageMetadataOnlyForOversizedDocuments` auf „true“ setzen: 

    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }

Sie können die Indizierung auch fortsetzen, wenn an einem beliebigen Punkt der Verarbeitung Fehler auftreten, entweder bei der Analyse von Blobs oder beim Hinzufügen von Dokumenten zu einem Index. Um eine bestimmte Anzahl von Fehlern zu ignorieren, legen Sie die Konfigurationsparameter `maxFailedItems` und `maxFailedItemsPerBatch` auf die gewünschten Werte fest. Beispiel:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

## <a name="incremental-indexing-and-deletion-detection"></a>Inkrementelle Indizierung und Erkennung von Löschungen

Wenn Sie einen Blob-Indexer zur Ausführung nach einem Zeitplan einrichten, werden nur die geänderten Blobs neu indiziert. Dies wird anhand des `LastModified`-Zeitstempels der Blobs ermittelt.

> [!NOTE]
> Sie müssen keine Richtlinie zum Erkennen von Änderungen angeben. Die inkrementelle Indizierung wird für Sie automatisch indiziert.

Damit das Löschen von Dokumenten unterstützt wird, sollten Sie die Strategie des „vorläufigen Löschens“ anwenden. Wenn Sie die Blobs direkt löschen, werden die entsprechenden Dokumente nicht aus dem Suchindex entfernt.

Es gibt zwei Möglichkeiten, den Ansatz des vorläufigen Löschens zu implementieren. Beide werden unten beschrieben.

### <a name="native-blob-soft-delete-preview"></a>Natives vorläufiges Löschen von Blobs (Vorschau)

> [!IMPORTANT]
> Die Unterstützung für das native vorläufige Löschen von Blobs ist als Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](https://docs.microsoft.com/azure/search/search-api-preview) bereitgestellt. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

> [!NOTE]
> Bei Verwendung der Richtlinie zum nativen vorläufigen Löschen von Blobs müssen die Dokumentschlüssel für die Dokumente in Ihrem Index entweder eine Blobeigenschaft oder Blobmetadaten sein.

Bei dieser Methode verwenden Sie das Feature zum [nativen vorläufigen Löschen von Blobs](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete), das Azure Blob Storage bietet. Wenn natives vorläufiges Löschen von Blobs für Ihr Speicherkonto aktiviert ist, wird für die Datenquelle eine Richtlinie für natives vorläufiges Löschen festgelegt, und der Indexer sucht ein Blob, das in einen vorläufig gelöschten Zustand versetzt wurde. Der Indexer entfernt dieses Dokument aus dem Index. Die Richtlinie zum nativen vorläufigen Löschen von Blobs wird beim Indizieren von Blobs aus Azure Data Lake Storage Gen2 nicht unterstützt.

Führen Sie die folgenden Schritte durch:
1. Aktivieren Sie [natives vorläufiges Löschen für Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete). Sie sollten für die Aufbewahrungsrichtlinie einen Wert festlegen, der wesentlich höher ist als der Intervallzeitplan Ihres Indexers. Wenn ein Problem beim Ausführen des Indexers vorliegt oder eine große Anzahl von Dokumenten indiziert werden muss, steht dem Indexer viel Zeit für die endgültige Verarbeitung der vorläufig gelöschten Blobs zur Verfügung. Indexer von Azure Cognitive Suche löschen nur dann ein Dokument aus dem Index, wenn sie das Blob verarbeiten, während es sich in einem vorläufig gelöschten Zustand befindet.
1. Konfigurieren Sie für die Datenquelle eine Richtlinie zur Erkennung des nativen vorläufigen Löschens von Blobs. Ein entsprechendes Beispiel ist nachfolgend dargestellt. Da dieses Feature in der Vorschauversion verfügbar ist, müssen Sie die Vorschau-REST-API verwenden.
1. Führen Sie den Indexer aus, oder legen Sie fest, dass der Indexer gemäß einem Zeitplan ausgeführt wird. Wenn der Indexer das Blob ausführt und verarbeitet, wird das Dokument aus dem Index entfernt.

    ```
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.NativeBlobSoftDeleteDeletionDetectionPolicy"
        }
    }
    ```

#### <a name="reindexing-undeleted-blobs"></a>Neuindizieren nicht gelöschter Blobs

Wenn Sie mit aktiviertem nativem vorläufigem Löschen ein Blob aus Azure Blob Storage in Ihrem Speicherkonto löschen, wechselt das Blob in einen vorläufig gelöschten Zustand, sodass Sie die Möglichkeit haben, das Blob innerhalb des Aufbewahrungszeitraums wieder herzustellen machen. Wenn eine Azure Cognitive Search-Datenquelle eine Richtlinie für natives vorläufiges Löschen eines Blobs aufweist und der Indexer ein vorläufig gelöschtes BLOB verarbeitet, wird dieses Dokument aus dem Index entfernt. Wenn dieses Blobs später wieder hergestellt wird, indiziert der Indexer das Blob nicht immer neu. Dies liegt daran, dass der Indexer basierend auf dem `LastModified`-Zeitstempel des Blobs bestimmt, welche Blobs indiziert werden sollen. Wenn ein vorläufig gelöschtes Blob wieder hergestellt wird, wird sein `LastModified`-Zeitstempel nicht aktualisiert. Falls der Indexer bereits Blobs mit `LastModified`-Zeitstempeln verarbeitet hat, die aktueller sind als das wieder hergestellte Blob, wird das wieder hergestellte Blob nicht neu indiziert. Um sicherzustellen, dass ein wieder hergestelltes Blob neu indiziert wird, müssen Sie den `LastModified`-Zeitstempel des Blobs aktualisieren. Eine Möglichkeit hierfür besteht darin, die Metadaten des Blobs erneut zu speichern. Sie müssen die Metadaten nicht ändern, aber durch das erneute Speichern der Metadaten wird der `LastModified`-Zeitstempel des Blobs aktualisiert, sodass der Indexer weiß, dass er dieses Blob neu indizieren muss.

### <a name="soft-delete-using-custom-metadata"></a>Vorläufiges Löschen mithilfe benutzerdefinierter Metadaten

In dieser Methode verwenden Sie die Metadaten eines Blobs, um anzugeben, wann ein Dokument aus dem Suchindex entfernt werden sollte.

Führen Sie die folgenden Schritte durch:

1. Fügen Sie dem Blob ein Metadaten-Schlüssel-Wert-Paar hinzu, um in Azure Cognitive Search anzugeben, dass das Blob logisch gelöscht ist.
1. Konfigurieren Sie für die Datenquelle eine Richtlinie zur Erkennung von Spalten für vorläufiges Löschen. Ein entsprechendes Beispiel ist nachfolgend dargestellt.
1. Nachdem der Indexer das Blob verarbeitet und das Dokument aus dem Index gelöscht hat, können Sie das Blob für Azure Blob Storage löschen.

Bei der folgenden Richtlinie wird ein Blob beispielsweise als gelöscht angesehen, wenn es über die Metadateneigenschaft `IsDeleted` mit dem Wert `true` verfügt:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }

#### <a name="reindexing-undeleted-blobs"></a>Neuindizieren wieder hergestellter Blobs

Wenn Sie für die Datenquelle eine Richtlinie zur Erkennung von Spalten für vorläufiges Löschen festlegen, fügen Sie die benutzerdefinierten Metadaten einem Blob mit dem Markerwert hinzu, und führen Sie dann den Indexer aus. Der Indexer entfernt das Dokument aus dem Index. Wenn Sie dieses Dokument neu indizieren möchten, ändern Sie einfach den Metadatenwert für vorläufiges Löschen für das Blob, und führen Sie den Indexer erneut aus.

## <a name="indexing-large-datasets"></a>Indizieren großer Datasets

Das Indizieren von Blobs kann sehr zeitaufwändig sein. In Fällen, in denen Sie Millionen von Blobs zu indizieren haben, können Sie durch das Partitionieren der Daten und Verwenden mehrerer Indexer zur parallelen Datenverarbeitung die Indizierung beschleunigen. Sie können es folgendermaßen einrichten:

- Partitionieren Sie Ihre Daten in mehrere Blobcontainer oder virtuelle Ordner.
- Richten Sie mehrere Datenquellen der kognitiven Azure-Suche ein – eine pro Container oder Ordner. Verwenden Sie den Parameter `query`, um auf einen Blob-Ordner zu verweisen:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Erstellen Sie einen entsprechenden Indexer für jede Datenquelle. Alle Indexer können auf den gleichen Zielsuchindex zeigen.  

- Eine Sucheinheit in Ihrem Dienst kann einen Indexer zu jeder angegebenen Uhrzeit ausführen. Das Erstellen von mehreren Indexern – wie oben beschrieben – ist nur nützlich, wenn sie tatsächlich parallel ausgeführt werden. Um mehrere Indexer parallel auszuführen, skalieren Sie Ihren Suchdienst auf, indem Sie eine bestimmte Anzahl von Partitionen und Replikate erstellen. Wenn Ihr Suchdienst beispielsweise über 6 Sucheinheiten verfügt (z.B. 2 Partitionen x 3 Replikate), dann können 6 Indexers simultan ausgeführt werden. Dies führt zu einem sechsfachen Anstieg des Indizierungsdurchsatzes. Weitere Informationen zur Skalierung und Kapazitätsplanung finden Sie unter [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in der kognitiven Azure-Suche](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>Indizieren von Dokumenten zusammen mit den zugehörigen Daten

Sie möchten möglicherweise Dokumente aus mehreren Quellen in Ihrem Index „zusammenbauen“. Womöglich möchten Sie z.B. Text aus Blobs mit anderen Metadaten zusammenführen, die in Cosmos DB gespeichert sind. Sie können auch die Indizierungs-API mit Push zusammen mit unterschiedlichen Indexern zum Erstellen von Suchdokumenten aus mehreren Teilen verwenden. 

Damit dies funktioniert, müssen sich alle Indexer und andere Komponenten auf den Dokumentschlüssel einigen. Weitere Informationen zu diesem Thema finden Sie unter [Indizieren mehrerer Azure-Datenquellen](https://docs.microsoft.com/azure/search/tutorial-multiple-data-sources). Eine ausführliche exemplarische Vorgehensweise finden Sie im externen Artikel: [Combine documents with other data in Azure Cognitive Search](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html) (Zusammenführen von Dokumenten mit anderen Daten in der kognitiven Azure-Suche).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Indizierung von Nur-Text 

Wenn alle Ihre Blobs Nur-Text in derselben Codierung enthalten, können Sie die Indizierungsleistung signifikant verbessern, indem Sie den **Textanalysemodus** nutzen. Um den Textanalysemodus zu verwenden, legen Sie die `parsingMode`-Konfigurationseigenschaft auf `text` fest:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Standardmäßig wird von der `UTF-8`-Codierung ausgegangen. Um eine andere Codierung anzugeben, verwenden Sie die `encoding`-Konfigurationseigenschaft. 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Inhaltstypspezifische Metadateneigenschaften
In der folgenden Tabelle sind die Verarbeitungsschritte für jedes Dokumentformat zusammengefasst, und es werden die Metadateneigenschaften beschrieben, die bei der kognitiven Azure-Suche extrahiert werden.

| Dokumentformat/Inhaltstyp | Inhaltstypspezifische Metadateneigenschaften | Verarbeitungsdetails |
| --- | --- | --- |
| HTML (text/html) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Entfernen von HTML-Markup und Extrahieren von Text |
| PDF (application/pdf) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Extrahieren von Text, z. B. eingebettete Dokumente (mit Ausnahme von Bildern) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extrahieren von Text, z. B. eingebettete Dokumente |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extrahieren von Text, z. B. eingebettete Dokumente |
| DOCM (application/vnd.ms-word.document.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extrahieren von Text, z. B. eingebettete Dokumente |
| WORD XML (application/vnd.ms-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Entfernen von XML-Markup und Extrahieren von Text |
| WORD 2003 XML (application/vnd.ms-wordml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |Entfernen von XML-Markup und Extrahieren von Text |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extrahieren von Text, z. B. eingebettete Dokumente |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extrahieren von Text, z. B. eingebettete Dokumente |
| XLSM (application/vnd.ms-excel.sheet.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extrahieren von Text, z. B. eingebettete Dokumente |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extrahieren von Text, z. B. eingebettete Dokumente |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extrahieren von Text, z. B. eingebettete Dokumente |
| PPTM (application/vnd.ms-powerpoint.presentation.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extrahieren von Text, z. B. eingebettete Dokumente |
| MSG (application/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Extrahieren von Text, einschließlich Anlagen. `metadata_message_to_email`, `metadata_message_cc_email` und `metadata_message_bcc_email` sind Zeichenfolgensammlungen, die übrigen Felder sind Zeichenfolgen.|
| ODT (application/vnd.oasis.opendocument.text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extrahieren von Text, z. B. eingebettete Dokumente |
| ODS (application/vnd.oasis.opendocument.spreadsheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extrahieren von Text, z. B. eingebettete Dokumente |
| ODP (application/vnd.oasis.opendocument.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Extrahieren von Text, z. B. eingebettete Dokumente |
| ZIP (application/zip) |`metadata_content_type` |Extrahieren von Text aus allen Dokumenten im Archiv |
| GZ (application/gzip) |`metadata_content_type` |Extrahieren von Text aus allen Dokumenten im Archiv |
| EPUB (application/epub+zip) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Extrahieren von Text aus allen Dokumenten im Archiv |
| XML (application/xml) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |Entfernen von XML-Markup und Extrahieren von Text |
| JSON (application/json) |`metadata_content_type`<br/>`metadata_content_encoding` |Extrahieren von Text<br/>HINWEIS:  Wenn Sie mehrere Felder des Dokuments aus einem JSON-Blob extrahieren möchten, helfen Ihnen die ausführlichen Informationen unter [Indizierung der JSON-Blobs](search-howto-index-json-blobs.md) weiter. |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Extrahieren von Text, einschließlich Anlagen |
| RTF (application/rtf) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Extrahieren von Text|
| Nur-Text (text/plain) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Extrahieren von Text|


## <a name="help-us-make-azure-cognitive-search-better"></a>Helfen Sie uns bei der Verbesserung der kognitiven Azure-Suche
Teilen Sie uns auf unserer [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/) mit, wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben.
