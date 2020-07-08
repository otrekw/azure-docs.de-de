---
title: Einrichten einer Verbindung mit einem Speicherkonto mithilfe einer verwalteten Identität (Vorschau)
titleSuffix: Azure Cognitive Search
description: Erfahren Sie, wie Sie mithilfe einer verwalteten Identität eine Indexerverbindung mit einem Azure Storage-Konto einrichten (Vorschau).
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: e6c766008faa6bbe53a4af69f7da9325cb9ff6a8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559869"
---
# <a name="set-up-a-connection-to-an-azure-storage-account-using-a-managed-identity-preview"></a>Einrichten einer Verbindung mit einem Azure Storage-Konto mithilfe einer verwalteten Identität (Vorschau)

> [!IMPORTANT] 
> Das Einrichten einer Verbindung mit einer Datenquelle mithilfe einer verwalteten Identität wird derzeit in einer geschlossenen öffentlichen Vorschauversion unterstützt. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.
> Füllen Sie [dieses Formular](https://aka.ms/azure-cognitive-search/mi-preview-request) aus, wenn Sie Zugriff auf die Vorschauversion anfordern möchten.

Auf dieser Seite wird beschrieben, wie Sie eine Indexerverbindung mit einer Azure-Speicherkonto mithilfe einer verwalteten Identität einrichten, anstatt Anmeldeinformationen in der Verbindungszeichenfolge des Datenquellenobjekts anzugeben.

Bevor Sie mehr über diese Funktion erfahren, sollten Sie wissen, was ein Indexer ist, und wie Sie einen Indexer für Ihre Datenquelle einrichten. Weitere Informationen finden Sie unter den folgenden Links:
* [Indexer (Übersicht)](search-indexer-overview.md)
* [Azure-Blobindexer](search-howto-indexing-azure-blob-storage.md)
* [Azure Data Lake Storage Gen2-Indexer](search-howto-index-azure-data-lake-storage.md)
* [Azure-Tabellenindexer](search-howto-indexing-azure-tables.md)

## <a name="set-up-the-connection"></a>Einrichten der Verbindung

### <a name="1---turn-on-system-assigned-managed-identity"></a>1\. Aktivieren einer systemseitig zugewiesenen verwalteten Identität

Wenn eine systemseitig zugewiesene verwaltete Identität aktiviert ist, wird in Azure eine Identität für den Suchdienst erstellt, die für die Authentifizierung bei anderen Azure-Diensten innerhalb desselben Mandanten und desselben Abonnements verwendet werden kann. Anschließend können Sie diese Identität in RBAC-Zuweisungen (rollenbasierte Zugriffssteuerung) verwenden, die den Zugriff auf Daten während der Indizierung ermöglichen.

![Aktivieren einer systemseitig zugewiesenen verwalteten Identität](./media/search-managed-identities/turn-on-system-assigned-identity.png "Aktivieren einer systemseitig zugewiesenen verwalteten Identität")

Nach dem Auswählen von **Speichern** wird eine Objekt-ID angezeigt, die dem Suchdienst zugewiesen wurde.

![Objekt-ID](./media/search-managed-identities/system-assigned-identity-object-id.png "ObjectID")
 
### <a name="2---add-a-role-assignment"></a>2\. Hinzufügen einer Rollenzuweisung

In diesem Schritt erteilen Sie dem Azure Cognitive Search-Dienst die Berechtigung, Daten von Ihrem Speicherkonto zu lesen.

1. Navigieren Sie im Azure-Portal zu dem Speicherkonto, in dem die Daten gespeichert werden, die indiziert werden sollen.
2. Wählen Sie **Zugriffssteuerung (IAM)** aus.
3. Klicken Sie auf **Hinzufügen** und dann auf **Rollenzuweisung hinzufügen**.

    ![Hinzufügen der Rollenzuweisung](./media/search-managed-identities/add-role-assignment-storage.png "Rollenzuweisung hinzufügen")

4. Wählen Sie die entsprechenden Rollen je nach Speicherkontotyp aus, den Sie indizieren möchten:
    1. Azure Blob Storage erfordert, dass Sie Ihren Suchdienst zu den Rollen **Lese- und Datenzugriff** und **Storage-Blobdatenleser** hinzufügen.
    1. Azure Data Lake Storage Gen2 erfordert, dass Sie Ihren Suchdienst zu den Rollen **Lese- und Datenzugriff** und **Storage-Blobdatenleser** hinzufügen.
    1. Der Azure-Tabellenspeicher erfordert, dass Sie Ihren Suchdienst nur zur Rolle **Lese- und Datenzugriff** hinzufügen.
5.  Behalten Sie unter **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** bei.
6.  Suchen Sie nach Ihrem Suchdienst, wählen Sie ihn aus, und wählen Sie dann **Speichern** aus.

    ![Hinzufügen der Rollenzuweisung „Lese- und Datenzugriff“](./media/search-managed-identities/add-role-assignment-reader-and-data-access.png "Hinzufügen der Rollenzuweisung „Lese- und Datenzugriff“")

Beachten Sie, dass Sie beim Herstellen einer Verbindung mit Azure Blob Storage und Azure Data Lake Storage Gen2 auch die Rollenzuweisung **Storage-Blobdatenleser** hinzufügen müssen.

![Hinzufügen der Rollenzuweisung „Storage-Blobdatenleser“](./media/search-managed-identities/add-role-assignment-storage-blob-data-reader.png "Hinzufügen der Rollenzuweisung „Storage-Blobdatenleser“")

### <a name="3---create-the-data-source"></a>3\. Erstellen der Datenquelle

Bei der Indizierung über ein Speicherkonto muss die Datenquelle über die folgenden erforderlichen Eigenschaften verfügen:

* **name** ist der eindeutige Name der Datenquelle im Suchdienst.
* **type**
    * Azure Blob Storage: `azureblob`
    * Azure-Tabellenspeicher: `azuretable`
    * Azure Data Lake Storage Gen2: der **Typ** wird angegeben, sobald Sie sich über [dieses Formular](https://aka.ms/azure-cognitive-search/mi-preview-request) für die Vorschauversion registrieren.
* **credentials**
    * Beim Verwenden einer verwalteten Identität zur Authentifizierung unterscheidet sich das Format **credentials** von dem für die Authentifizierung ohne verwaltete Identität. Hier geben Sie eine Ressourcen-ID an, der kein Kontoschlüssel und kein Kennwort zugewiesen ist. Die Ressourcen-ID muss die Abonnement-ID des Speicherkontos, die Ressourcengruppe des Speicherkontos und den Speicherkontonamen enthalten.
    * Format für verwaltete Identitäten: 
        * *ResourceId=/subscriptions/**Ihre Abonnement-ID**/resourceGroups/**Ihr Ressourcengruppenname**/providers/Microsoft.Storage/storageAccounts/**Ihr Speicherkontoname**/;*
* Mit **container** wird ein Container oder Tabellenname in Ihrem Speicherkonto festgelegt. Standardmäßig können alle Blobs im Container abgerufen werden. Wenn Sie nur Blobs in einem bestimmten virtuellen Verzeichnis indizieren möchten, können Sie dieses Verzeichnis mit dem optionalen **query**-Parameter angeben.

Beispiel zum Erstellen eines Blob-Datenquellenobjekts mithilfe der [REST-API](https://docs.microsoft.com/rest/api/searchservice/create-data-source):

```
POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
    "name" : "blob-datasource",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "ResourceId=/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name/;" },
    "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
}   
```

Die Verbindungszeichenfolge für verwaltete Identitäten wird auch über das Azure-Portal und das [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet) unterstützt. Im Azure-Portal ist ein Featureflag erforderlich, das Sie bei der Registrierung für die Vorschauversion über den Link oben auf dieser Seite erhalten. 

### <a name="4---create-the-index"></a>4\. Erstellen des Index

Mit dem Index werden die Felder in einem Dokument, Attribute und andere Konstrukte für die Suchoberfläche angegeben.

Hier sehen Sie, wie Sie einen Index mit einem durchsuchbaren `content`-Feld zum Speichern des aus Blobs extrahierten Texts erstellen:   

    POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
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

### <a name="5---create-the-indexer"></a>5\. Erstellen des Indexers

Ein Indexer verbindet eine Datenquelle mit einem Zielsuchindex und stellt einen Zeitplan zur Automatisierung der Datenaktualisierung bereit.

Nach der Erstellung von Index und Datenquelle können Sie den Indexer erstellen.

Beispielindexerdefinition für einen Blobindexer:

    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
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

## <a name="see-also"></a>Weitere Informationen

Weitere Informationen zu Azure Storage-Indexern:
* [Azure-Blobindexer](search-howto-indexing-azure-blob-storage.md)
* [Azure Data Lake Storage Gen2-Indexer](search-howto-index-azure-data-lake-storage.md)
* [Azure-Tabellenindexer](search-howto-indexing-azure-tables.md)
