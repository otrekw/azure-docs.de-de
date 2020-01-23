---
title: Kopieren und Transformieren von Daten in Azure Blob Storage
description: Hier erfahren Sie, wie Sie Daten mithilfe von Data Factory in und aus Blob Storage kopieren sowie Daten in Blob Storage transformieren.
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: 9001a7837e6106df4e052277168388064e65e7ee
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75893303"
---
# <a name="copy-and-transform-data-in-azure-blob-storage-by-using-azure-data-factory"></a>Kopieren und Transformieren von Daten in Azure Blob Storage mithilfe von Azure Data Factory

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-azure-blob-connector.md)
> * [Aktuelle Version](connector-azure-blob-storage.md)

In diesem Artikel wird beschrieben, wie Sie Daten mithilfe der Kopieraktivität in Azure Data Factory aus und in Azure Blob Storage kopieren sowie Daten mithilfe von Datenfluss in Azure Blob Storage transformieren. Informationen zu Azure Data Factory finden Sie im [Einführungsartikel](introduction.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Dieser Azure-BLOB-Connector wird für die folgenden Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit [unterstützter Quellen/Senken-Matrix](copy-activity-overview.md)
- [Mapping Data Flow](concepts-data-flow-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [GetMetadata-Aktivität](control-flow-get-metadata-activity.md)
- [Delete-Aktivität](delete-activity.md)

Bei der Kopieraktivität unterstützt dieser Blob Storage-Connector Folgendes:

- Kopieren von Daten in bzw. aus Azure Storage-Konten für allgemeine Zwecke und heißen/kalten Blob Storage. 
- Kopieren von Blobs mithilfe des Kontoschlüssels, der Dienst-SAS, des Dienstprinzipals oder verwalteter Identitäten für Azure-Ressourcenauthentifizierungen.
- Kopieren von Blobs aus Block-, Anfüge- oder Seitenblobs und Kopieren von Daten ausschließlich in Blockblobs.
- Kopieren von Blobs im jeweiligen Zustand oder Analysieren bzw. Generieren von Blobs mit den [unterstützten Dateiformaten und Komprimierungscodecs](supported-file-formats-and-compression-codecs.md).
- [Beibehalten von Dateimetadaten beim Kopieren](#preserve-metadata-during-copy)

>[!IMPORTANT]
>Wenn Sie die Option **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben** in den Firewalleinstellungen von Azure Storage aktivieren und Azure Integration Runtime zum Herstellen einer Verbindung mit Blob Storage verwenden möchten, müssen Sie die [Authentifizierung der verwalteten Identität](#managed-identity) verwenden.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Blob Storage verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Der Azure Blob-Connector unterstützt die folgenden Authentifizierungstypen (Details finden Sie im entsprechenden Abschnitt):

- [Kontoschlüsselauthentifizierung](#account-key-authentication)
- [SAS-Authentifizierung (Shared Access Signature)](#shared-access-signature-authentication)
- [Dienstprinzipalauthentifizierung](#service-principal-authentication)
- [Verwaltete Identitäten für Azure-Ressourcenauthentifizierung](#managed-identity)

>[!NOTE]
>Wenn Sie mit PolyBase Daten in SQL Data Warehouse laden und Ihr Quell- oder Stagingblobspeicher mit einem Virtual Network-Endpunkt konfiguriert ist, müssen Sie zur Authentifizierung eine verwaltete Identität und die selbstgehostete Integration Runtime in der Version 3.18 oder höher verwenden. Im Abschnitt [Verwaltete Identitäten für Azure-Ressourcenauthentifizierung](#managed-identity) sind weitere Konfigurationsvoraussetzungen aufgeführt.

>[!NOTE]
>HDInsights- und Azure Machine Learning-Aktivitäten unterstützen nur die Kontoschlüsselauthentifizierung für Azure-Blobspeicher.

### <a name="account-key-authentication"></a>Kontoschlüsselauthentifizierung

Für die Verwendung der Authentifizierung mit dem Speicherkontoschlüssel werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureBlobStorage** (empfohlen) oder **AzureStorage** (siehe Hinweise unten) festgelegt werden. |Ja |
| connectionString | Geben Sie für die connectionString-Eigenschaft die Informationen ein, die zum Herstellen einer Verbindung mit Azure Storage erforderlich sind. <br/> Sie können auch den Kontoschlüssel in Azure Key Vault speichern und die `accountKey`-Konfiguration aus der Verbindungszeichenfolge pullen. Ausführlichere Informationen finden Sie in den folgenden Beispielen und im Artikel [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

>[!NOTE]
>Der sekundäre Blob-Dienstendpunkt wird bei Verwendung der Kontoschlüsselauthentifizierung nicht unterstützt. Sie können andere Authentifizierungstypen verwenden.

>[!NOTE]
>Wenn Sie den verknüpften Dienst vom Typ „AzureStorage“ verwendet haben, wird dies weiterhin unterstützt. Es wird jedoch empfohlen, in Zukunft diesen neuen verknüpften Dienst vom Typ „AzureBlobStorage“ zu verwenden.

**Beispiel:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel: Speichern des Kontoschlüssels in Azure Key Vault**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;",
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }            
    }
}
```

### <a name="shared-access-signature-authentication"></a>SAS-Authentifizierung (Shared Access Signature)

Shared Access Signatures bieten delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Sie können eine SAS verwenden, um einem Client für einen bestimmten Zeitraum eingeschränkte Berechtigungen für Objekte in Ihrem Speicherkonto zu gewähren. Sie müssen die Zugriffsschlüssel für Ihr Konto nicht freigeben. Die SAS ist ein URI, dessen Abfrageparameter alle erforderlichen Informationen für den authentifizierten Zugriff auf eine Speicherressource enthalten. Um mit der SAS auf Speicherressourcen zuzugreifen, muss der Client diese nur an den entsprechenden Konstruktor bzw. die entsprechende Methode übergeben. Weitere Informationen zu Shared Access Signatures finden Sie unter [Shared Access Signatures (SAS): Verstehen des Shared Access Signature-Modells](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
>- Data Factory unterstützt jetzt **SAS (Shared Access Signatures) für Dienste** sowie für **Konten**. Weitere Informationen zu SAS (Shared Access Signatures) finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../storage/common/storage-sas-overview.md).
>- In der späteren Datasetkonfiguration ist der Ordnerpfad der absolute Pfad ab der Containerebene. Sie müssen einen Pfad konfigurieren, der sich am Pfad in Ihrem SAS-URI orientiert.

> [!TIP]
> Um eine Dienst-SAS für Ihr Speicherkonto zu generieren, können Sie die folgenden PowerShell-Befehle ausführen. Ersetzen Sie die Platzhalter, und gewähren Sie die erforderliche Berechtigung.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Für die Verwendung der SAS-Authentifizierung werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureBlobStorage** (empfohlen) oder **AzureStorage** (siehe Hinweise unten) festgelegt werden. |Ja |
| sasUri | Geben Sie den SAS-URI für die Azure Storage-Ressourcen wie Blobs oder Container an. <br/>Markieren Sie dieses Feld als „SecureString“, um es sicher in Data Factory zu speichern. Sie können auch das SAS-Token in Azure Key Vault speichern, um die automatische Rotation zu nutzen und den Tokenabschnitt zu entfernen. Ausführlichere Informationen finden Sie in den folgenden Beispielen und im Artikel [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

>[!NOTE]
>Wenn Sie den verknüpften Dienst vom Typ „AzureStorage“ verwendet haben, wird dies weiterhin unterstützt. Es wird jedoch empfohlen, in Zukunft diesen neuen verknüpften Dienst vom Typ „AzureBlobStorage“ zu verwenden.

**Beispiel:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<container>.blob.core.windows.net/?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel: Speichern des Kontoschlüssels in Azure Key Vault**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<container>.blob.core.windows.net/>"
            },
            "sasToken": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Berücksichtigen Sie beim Erstellen eines SAS-URIs die folgenden Aspekte:

- Legen Sie geeignete Lese-/Schreib-Berechtigungen für Objekte fest, basierend auf der Verwendung des verknüpften Diensts in Ihrer Data Factory (Lesen, Schreiben, Lesen/Schreiben).
- Legen Sie für **Ablaufzeit** einen geeigneten Wert fest. Stellen Sie sicher, dass der Zugriff auf Storage-Objekte nicht während des aktiven Zeitraums der Pipeline abläuft.
- Der URI sollte je nach Bedarf auf der richtigen Container-/Blobebene erstellt werden. Ein SAS-URI zu einem Blob ermöglicht Data Factory den Zugriff auf dieses spezielle Blob. Ein SAS-URI zu einem Blobspeichercontainer ermöglicht Data Factory das Durchlaufen von Blobs in diesem Container. Wenn Sie den Zugriff später auf mehr Objekte ausweiten oder auf weniger Objekte beschränken oder den SAS-URI aktualisieren möchten, denken Sie daran, den verknüpften Dienst mit dem neuen URI zu aktualisieren.

### <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung

Informationen zur Dienstprinzipalauthentifizierung für Azure Storage im Allgemeinen finden Sie unter [Authentifizieren des Zugriffs auf Azure Storage mithilfe von Azure Active Directory](../storage/common/storage-auth-aad.md).

Zum Verwenden der Dienstprinzipalauthentifizierung führen Sie die folgenden Schritte aus:

1. Registrieren Sie eine Anwendungsentität in Azure Active Directory (Azure AD), indem Sie die Anleitungen unter [Registrieren Ihrer Anwendung bei einem Azure AD-Mandanten](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant) befolgen. Notieren Sie sich die folgenden Werte, die Sie zum Definieren des verknüpften Diensts verwenden:

    - Anwendungs-ID
    - Anwendungsschlüssel
    - Mandanten-ID

2. Erteilen Sie dem Dienstprinzipal die entsprechenden Berechtigung in Azure Blob Storage. Weitere Informationen zu den Rollen finden Sie unter [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC](../storage/common/storage-auth-aad-rbac.md).

    - Erteilen Sie ihr **als Quelle** in der Zugriffssteuerung (IAM) mindestens die Rolle **Storage-Blobdatenleser**.
    - Weisen Sie **Als Senke** in der Zugriffssteuerung (IAM) mindestens die Rolle **Mitwirkender an Storage-Blobdaten** zu.

Diese Eigenschaften werden für den mit Azure Blob Storage verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureBlobStorage** festgelegt werden. |Ja |
| serviceEndpoint | Geben Sie den Azure Blob Storage-Dienstendpunkt mit dem Muster `https://<accountName>.blob.core.windows.net/` an. |Ja |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | Ja |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als **SecureString**, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja |
| tenant | Geben Sie die Mandanteninformationen (Domänenname oder Mandanten-ID) für Ihre Anwendung an. Diese können Sie abrufen, indem Sie den Mauszeiger über den rechten oberen Bereich im Azure-Portal bewegen. | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

>[!NOTE]
>Dienstprinzipalauthentifizierung wird nur durch den verknüpften Dienst vom Typ „AzureBlobStorage“unterstützt, nicht jedoch vom vorherigen verknüpften Dienst vom Typ „AzureStorage“.

**Beispiel:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a>Verwaltete Identitäten für Azure-Ressourcenauthentifizierung

Eine Data Factory kann einer [verwalteten Identität für Azure-Ressourcen](data-factory-service-identity.md) zugeordnet werden, die diese spezielle Data Factory darstellt. Sie können diese verwaltete Identität direkt für die Blob Storage-Authentifizierung verwenden, ähnlich wie bei der Verwendung Ihres eigenen Dienstprinzipals. Sie erlaubt dieser bestimmten Factory den Zugriff auf und das Kopieren von Daten aus Ihrem bzw. in Ihren Blob Storage.

Weitere Informationen zur Azure Storage-Authentifizierung im Allgemeinen finden Sie unter [Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](../storage/common/storage-auth-aad.md). Um verwaltete Identitäten für die Azure-Ressourcenauthentifizierung zu verwenden, gehen Sie folgendermaßen vor:

1. [Rufen Sie die verwalteten Data Factory-Identitätsinformationen ab](data-factory-service-identity.md#retrieve-managed-identity), indem Sie den Wert von „DIENSTIDENTITÄTSANWENDUNGS-ID“ kopieren, der zusammen mit der Factory generiert wurde.

2. Erteilen Sie der verwalteten Entität geeignete Berechtigungen in Azure Blob Storage. Weitere Informationen zu den Rollen finden Sie unter [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC](../storage/common/storage-auth-aad-rbac.md).

    - Erteilen Sie ihr **als Quelle** in der Zugriffssteuerung (IAM) mindestens die Rolle **Storage-Blobdatenleser**.
    - Weisen Sie **Als Senke** in der Zugriffssteuerung (IAM) mindestens die Rolle **Mitwirkender an Storage-Blobdaten** zu.

>[!IMPORTANT]
>Wenn Sie mit PolyBase Daten aus einem Quell- oder Stagingblob in SQL Data Warehouse laden und eine verwaltete Identität zur Authentifizierung bei dem Blob verwenden, müssen Sie auch die Schritte 1 und 2 im [zugehörigen Leitfaden](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) ausführen. Registrieren Sie in Schritt 1 zuerst Ihren SQL-Datenbank-Server bei Azure Active Directory (Azure AD). Weisen Sie dann in Schritt 2 Ihrem SQL-Datenbank-Server die Rolle „Mitwirkender an Storage-Blobdaten“ zu. Alles Weitere erledigt Data Factory. Wenn Ihr Blobspeicher mit einem Azure Virtual Network-Endpunkt konfiguriert ist und Sie mit PolyBase Daten daraus laden, müssen Sie zur Authentifizierung eine verwaltete Identität verwenden.

Diese Eigenschaften werden für den mit Azure Blob Storage verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **AzureBlobStorage** festgelegt werden. |Ja |
| serviceEndpoint | Geben Sie den Azure Blob Storage-Dienstendpunkt mit dem Muster `https://<accountName>.blob.core.windows.net/` an. |Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder die selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

> [!NOTE]
> Verwaltete Identitäten für die Azure-Ressourcenauthentifizierung werden nur vom verknüpften Dienst des Typs „AzureBlobStorage“ unterstützt, nicht jedoch vom vorherigen verknüpften Dienst des Typs „AzureStorage“. 

**Beispiel:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Folgende Eigenschaften werden für Azure-BLOB unter den `location`-Einstellungen in formatbasierten Datasets unterstützt:

| Eigenschaft   | Beschreibung                                                  | Erforderlich |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Die „type“-Eigenschaft des Speicherorts im Dataset muss auf **AzureBlobStorageLocation** festgelegt werden. | Ja      |
| Container  | Der BLOB-Container.                                          | Ja      |
| folderPath | Der Pfad zum Ordner unter dem angegebenen Container. Wenn Sie Platzhalter verwenden möchten, um Ordner zu filtern, überspringen Sie diese Einstellung, und geben Sie entsprechende Aktivitätsquelleneinstellungen an. | Nein       |
| fileName   | Der Name der Datei unter dem angegebenen Container und „folderPath“. Wenn Sie Platzhalter verwenden möchten, um Ordner zu filtern, überspringen Sie diese Einstellung, und geben Sie entsprechenden Aktivitätsquelleneinstellungen an. | Nein       |

**Beispiel:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit Eigenschaften, die von der Blob Storage-Quelle und -Senke unterstützt werden.

### <a name="blob-storage-as-a-source-type"></a>Blob Storage als Quelltyp

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Folgende Eigenschaften werden für Azure-BLOB unter den `storeSettings`-Einstellungen in der formatbasierten Kopierquelle unterstützt:

| Eigenschaft                 | Beschreibung                                                  | Erforderlich                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Die type-Eigenschaft unter `storeSettings` muss auf **AzureBlobStorageReadSettings** festgelegt werden. | Ja                                           |
| recursive                | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Beachten Sie Folgendes: Wenn „recursive“ auf „true“ festgelegt ist und es sich bei der Senke um einen dateibasierten Speicher handelt, wird ein leerer Ordner oder Unterordner nicht in die Senke kopiert und dort auch nicht erstellt. Zulässige Werte sind **true** (Standard) und **false**. | Nein                                            |
| wildcardFolderPath       | Der Ordnerpfad mit Platzhalterzeichen unter dem angegebenen Container, der im Dataset für das Filtern von Quellordnern konfiguriert ist. <br>Zulässige Platzhalter sind: `*` (entspricht null oder mehr Zeichen) und `?` (entspricht null oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn Ihr tatsächlicher Dateiname einen Platzhalter oder dieses Escapezeichen enthält. <br>Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). | Nein                                            |
| wildcardFileName         | Der Dateiname mit Platzhalterzeichen unter dem angegebenen Container und „folderPath/wildcardFolderPath“ für das Filtern von Quelldateien. <br>Zulässige Platzhalter sind: `*` (entspricht null oder mehr Zeichen) und `?` (entspricht null oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn Ihr tatsächlicher Dateiname einen Platzhalter oder dieses Escapezeichen enthält.  Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). | Ja, wenn `fileName` nicht im Dataset angegeben ist |
| modifiedDatetimeStart    | Dateifilterung basierend auf dem Attribut: Letzte Änderung. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird.  Wenn `modifiedDatetimeStart` den datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist.  Wenn `modifiedDatetimeEnd` den datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist. | Nein                                            |
| modifiedDatetimeEnd      | Wie oben.                                               | Nein                                            |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit einem Speicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein                                            |

> [!NOTE]
> Beim Parquet-Format/Textformat mit Trennzeichen wird die im nächsten Abschnitt beschriebene Quelle der Kopieraktivität vom Typ **BlobSource** aus Gründen der Abwärtskompatibilität weiterhin unverändert unterstützt. Es wird jedoch empfohlen, in Zukunft das neue Modell zu verwenden, da diese neuen Typen nun von der Benutzeroberfläche für die ADF-Dokumentenerstellung generiert werden.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobStorageReadSettings",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="blob-storage-as-a-sink-type"></a>Blob Storage als Senkentyp

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Folgende Eigenschaften werden für Azure-BLOB unter den `storeSettings`-Einstellungen in der formatbasierten Kopiersenke unterstützt:

| Eigenschaft                 | Beschreibung                                                  | Erforderlich |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Die type-Eigenschaft unter `storeSettings` muss auf **AzureBlobStorageWriteSettings** festgelegt werden. | Ja      |
| copyBehavior             | Definiert das Kopierverhalten, wenn es sich bei der Quelle um Dateien aus einem dateibasierten Datenspeicher handelt.<br/><br/>Zulässige Werte sind:<br/><b>- PreserveHierarchy (Standard)</b>: Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><b>- FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich auf der ersten Ebene des Zielordners. Die Namen für die Zieldateien werden automatisch generiert. <br/><b>- MergeFiles</b>: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Datei- oder Blobname angegeben wurde, entspricht der Name der Zusammenführungsdatei dem angegebenen Namen. Andernfalls wird der Dateiname automatisch generiert. | Nein       |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit einem Speicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein       |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureBlobStorageWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Beispiele für Ordner- und Dateifilter

Dieser Abschnitt beschreibt das sich ergebende Verhalten für den Ordnerpfad und den Dateinamen mit Platzhalterfiltern.

| folderPath | fileName | recursive | Quellordnerstruktur und Filterergebnis (Dateien mit **Fettformatierung** werden abgerufen.)|
|:--- |:--- |:--- |:--- |
| `container/Folder*` | (empty, use default) | false | Container<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `container/Folder*` | (empty, use default) | true | Container<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `container/Folder*` | `*.csv` | false | Container<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `container/Folder*` | `*.csv` | true | Container<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Beispiele für „recursive“ und „copyBehavior“

Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen von rekursiven und CopyBehavior-Werten.

| recursive | copyBehavior | Struktur des Quellordners | Resultierendes Ziel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der gleichen Struktur erstellt wie die Quelle:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei5 |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Die Inhalte von Datei1 + Datei2 + Datei3 + Datei4 + Datei5 werden in einer Datei mit einem automatisch generierten Dateinamen zusammengeführt. |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Die Inhalte von Datei1 + Datei2 werden in einer Datei mit einem automatisch generierten Dateinamen zusammengeführt. Automatisch generierter Name für Datei1<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |

## <a name="preserve-metadata-during-copy"></a>Beibehalten von Metadaten beim Kopieren

Beim Kopieren von Dateien von Amazon S3/Azure Blob/Azure Data Lake Storage Gen2 nach Azure Data Lake Storage Gen2/Azure Blob können Sie festlegen, dass die Dateimetadaten zusätzlich zu den Daten beibehalten werden sollen. Weitere Informationen finden Sie unter [Beibehalten von Metadaten](copy-activity-preserve-metadata.md#preserve-metadata).

## <a name="mapping-data-flow-properties"></a>Eigenschaften von Mapping Data Flow

Beim Transformieren von Daten in den Zuordnungsdatenfluss können Sie Dateien aus Azure Blob Storage im JSON- oder Avro-Format, im Textformat mit Trennzeichen oder im Parquet-Format lesen und schreiben. Weitere Informationen finden Sie unter [Quellentransformation](data-flow-source.md) und [Senkentransformation](data-flow-sink.md) im Zuordnungsdatenfluss-Feature.

### <a name="source-transformation"></a>Quellentransformation

In der Quellentransformation können Sie in Azure Blob Storage Daten aus einem Container, Ordner oder einer einzelnen Datei auslesen. Über die Registerkarte **Quellenoptionen** können Sie verwalten, wie die Dateien gelesen werden. 

![Quelloptionen](media/data-flow/sourceOptions1.png "Quelloptionen")

**Platzhalterpfad**: Mithilfe eines Platzhaltermusters wird ADF angewiesen, die einzelnen übereinstimmenden Ordner und Dateien in einer einzigen Quelltransformation zu durchlaufen. Dies ist eine effektive Methode zur Verarbeitung von mehreren Dateien in einem einzigen Datenfluss. Mit dem Pluszeichen (+), das angezeigt wird, wenn Sie mit dem Cursor auf Ihr vorhandenes Platzhaltermuster zeigen, können Sie weitere Platzhaltermuster hinzufügen.

Wählen Sie in Ihrem Quellcontainer eine Reihe von Dateien aus, die einem Muster entsprechen. Nur der Container kann im Dataset angegeben werden. Daher muss Ihr Platzhalterpfad auch den Ordnerpfad des Stammordners enthalten.

Beispiele für Platzhalter:

* ```*```: Stellt eine beliebige Zeichenfolge dar
* ```**```: Stellt rekursive Verzeichnisschachtelung dar
* ```?```: Ersetzt ein Zeichen
* ```[]```: Stimmt mit einem oder mehreren Zeichen in den Klammern überein

* ```/data/sales/**/*.csv```: Ruft alle CSV-Dateien unter „/data/sales“ ab
* ```/data/sales/20??/**```: Ruft alle Dateien aus dem 20. Jahrhundert ab
* ```/data/sales/2004/*/12/[XY]1?.csv```: Ruft alle CSV-Dateien aus Dezember 2004 ab, die mit X oder Y und einer zweistelligen Zahl als Präfix beginnen

**Partitionsstammpfad**: Wenn Ihre Dateiquelle partitionierte Ordner mit dem Format ```key=value``` (z.B. „Jahr=2019“) enthält, können Sie die oberste Ebene dieser Ordnerstruktur einem Spaltennamen in Ihrem Datenfluss-Datenstrom zuweisen.

Legen Sie zunächst einen Platzhalter fest, um darin alle Pfade, die die partitionierten Ordner sind, sowie die zu lesenden Blattdateien einzuschließen.

![Einstellungen für die Partitionsquelldatei](media/data-flow/partfile2.png "Einstellung der Partitionsdatei")

Verwenden Sie die Einstellung „Partitionsstammpfad“, um zu definieren, was die oberste Ebene der Ordnerstruktur ist. Wenn Sie die Inhalte Ihrer Daten über die Datenvorschau anzeigen, sehen Sie, dass ADF die aufgelösten Partitionen hinzufügen wird, die auf den einzelnen Ordnerebenen gefunden werden.

![Partitionsstammpfad](media/data-flow/partfile1.png "Vorschau des Partitionsstammpfads")

**Liste der Dateien**: Dies ist eine Dateigruppe. Erstellen Sie eine Textdatei mit einer Liste der relativen Pfade der zu verarbeitenden Dateien. Verweisen Sie auf diese Textdatei.

**Spalte für die Speicherung im Dateinamen**: Speichern Sie den Namen der Quelldatei in einer Spalte in den Daten. Geben Sie hier einen neuen Spaltennamen ein, um die Zeichenfolge für den Dateinamen zu speichern.

**Nach der Fertigstellung**: Wählen Sie aus, ob Sie nach dem Ausführen des Datenflusses nichts mit der Quelldatei anstellen, die Quelldatei löschen oder die Quelldateien verschieben möchten. Die Pfade für das Verschieben sind relative Pfade.

Um Quelldateien an einen anderen Speicherort nach der Verarbeitung zu verschieben, wählen Sie zuerst für den Dateivorgang die Option „Verschieben“ aus. Legen Sie dann das Quellverzeichnis („from“/„aus“) fest. Wenn Sie keine Platzhalter für Ihren Pfad verwenden, entspricht die Einstellung „from“ dem Quellordner.

Wenn Sie über einen Quellpfad mit Platzhalter verfügen, sieht Ihre Syntax ähnlich wie hier aus:

```/data/sales/20??/**/*.csv```

Geben Sie „from“ beispielsweise wie folgt an:

```/data/sales```

Und „to“ können Sie wie folgt angeben:

```/backup/priorSales```

In diesem Fall werden alle Dateien, die aus „/Data/Sales“ erstellt wurden, in „/Backup/priorSales“ verschoben.

> [!NOTE]
> Die Dateivorgänge werden nur ausgeführt, wenn der Datenfluss anhand der Aktivität zum Ausführen des Datenflusses in einer Pipeline über eine Pipelineausführung ausgeführt wird (Debuggen der Pipeline oder Ausführung). Dateivorgänge werden *nicht* im Datenfluss-Debugmodus ausgeführt.

**Nach der letzten Änderung filtern**: Sie können einen Datumsbereich angeben, um die zu verarbeitenden Dateien nach der letzten Änderung zu filtern. Alle Zeitangaben sind in UTC. 

### <a name="sink-properties"></a>Senkeneigenschaften

In der Senkentransformation können Sie in Azure Blob Storage in einen Container oder Ordner schreiben. Über die Registerkarte **Einstellungen** können Sie verwalten, wie die Dateien geschrieben werden.

![Senkenoptionen](media/data-flow/file-sink-settings.png "Senkenoptionen")

**Ordner löschen:** Bestimmt, ob der Zielordner vor dem Schreiben der Daten gelöscht wird.

**Dateinamenoption:** Bestimmt, wie die Zieldateien im Zielordner benannt werden. Es gibt folgende Dateinamenoptionen:
   * **Standard:** Lassen Sie zu, dass Spark Dateien basierend auf den PART-Standards benennt.
   * **Muster:** Geben Sie ein Muster ein, das Ihre Ausgabedateien pro Partition auflistet. Zum Beispiel erstellt **loans[n].csv** die Dateien „loans1.csv“, „loans2.csv“ usw.
   * **Pro Partition:** Geben Sie einen Dateinamen pro Partition ein.
   * **Wie Daten in Spalte:** Legen Sie die Ausgabedatei auf den Wert einer Spalte fest. Der Pfad ist relativ zum Datasetcontainer und nicht zum Zielordner.
   * **Ausgabe in eine einzelne Datei:** Mit dieser Option werden die partitionierten Ausgabedateien in einer einzelnen Datei kombiniert. Der Pfad ist relativ zum Datasetordner. Bedenken Sie, dass der Zusammenführungsvorgang je nach Knotengröße zu Fehlern führen kann. Diese Option wird für große Datasets nicht empfohlen.

**Alle in Anführungszeichen:** Bestimmt, ob alle Werte in Anführungszeichen eingeschlossen werden.

## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Eigenschaften der GetMetadata-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [GetMetadata-Aktivität](control-flow-get-metadata-activity.md). 

## <a name="delete-activity-properties"></a>Eigenschaften der Delete-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Delete-Aktivität](delete-activity.md).

## <a name="legacy-models"></a>Legacy-Modelle

>[!NOTE]
>Die folgenden Modelle werden aus Gründen der Abwärtskompatibilität weiterhin unverändert unterstützt. Es wird jedoch empfohlen, in Zukunft das in den obigen Abschnitten erwähnte neue Modell zu verwenden, da das neue Modell nun von der ADF-Benutzeroberfläche für die Erstellung generiert wird.

### <a name="legacy-dataset-model"></a>Legacy-Datasetmodell

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **AzureBlob** festgelegt werden. |Ja |
| folderPath | Der Pfad zum Container und Ordner im Blobspeicher. <br/><br/>Für den Pfad mit Ausnahme des Containernamens werden Platzhalterfilter unterstützt. Zulässige Platzhalter sind: `*` (entspricht null oder mehr Zeichen) und `?` (entspricht null oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn Ihr tatsächlicher Dateiname einen Platzhalter oder dieses Escapezeichen enthält. <br/><br/>Beispiele: „Blobcontainer/Blobordner/“. Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). |Ja für die Copy/Lookup-Aktivität, Nein für die GetMetadata-Aktivität |
| fileName | **Name oder Platzhalterfilter** für die Blobs unter dem angegebenen Wert für „folderPath“. Wenn Sie für diese Eigenschaft keinen Wert angeben, zeigt das Dataset auf alle Blobs im Ordner. <br/><br/>Für Filter sind folgende Platzhalter zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen).<br/>- Beispiel 1: `"fileName": "*.csv"`<br/>- Beispiel 2: `"fileName": "???20180427.txt"`<br/>Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Dateiname einen Platzhalter oder dieses Escapezeichen enthält.<br/><br/>Wenn „fileName“ nicht für ein Ausgabedataset und **preserveHierarchy** nicht in der Aktivitätssenke angegeben ist, generiert die Kopieraktivität den Blobnamen automatisch mit dem folgenden Muster: „*Data.[GUID der Aktivitätsausführungs-ID].[GUID, sofern „FlattenHierarchy“].[Format, sofern konfiguriert].[Komprimierung, sofern konfiguriert]* “, z. B. „Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz“. Wenn Sie Daten aus einer Quelle im Tabellenformat kopieren und dabei anstelle einer Abfrage den Tabellennamen verwenden, lautet das Namensmuster „ *[Tabellenname].[ Format].[Komprimierung, sofern konfiguriert]* “, z.B. „MyTable.csv“. |Nein |
| modifiedDatetimeStart | Dateifilterung basierend auf dem Attribut: Letzte Änderung. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br/><br/> Beachten Sie, dass die generelle Leistung der Datenverschiebung beeinträchtigt wird, wenn Sie diese Einstellung aktivieren und eine Dateifilterung für eine große Zahl von Dateien vornehmen möchten. <br/><br/> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird.  Wenn `modifiedDatetimeStart` den datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist.  Wenn `modifiedDatetimeEnd` den datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.| Nein |
| modifiedDatetimeEnd | Dateifilterung basierend auf dem Attribut: Letzte Änderung. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br/><br/> Beachten Sie, dass die generelle Leistung der Datenverschiebung beeinträchtigt wird, wenn Sie diese Einstellung aktivieren und eine Dateifilterung für eine große Zahl von Dateien vornehmen möchten. <br/><br/> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird.  Wenn `modifiedDatetimeStart` den datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist.  Wenn `modifiedDatetimeEnd` den datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.| Nein |
| format | Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), überspringen Sie den Formatabschnitt in den Definitionen der Eingabe- und Ausgabedatasets.<br/><br/>Für das Analysieren oder Generieren von Dateien mit einem bestimmten Format werden die folgenden Dateiformattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** und **ParquetFormat**. Sie müssen die **type**-Eigenschaft unter **format** auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs-legacy.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs-legacy.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs-legacy.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs-legacy.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| compression | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/>Unterstützte Typen sind **GZip**, **Deflate**, **BZIP2** und **ZipDeflate**.<br/>Unterstützte Grade sind **Optimal** und **Schnellste**. |Nein |

>[!TIP]
>Um alle Blobs in einem Ordner zu kopieren, geben Sie nur **folderPath** an.<br>Um einen einzelnen Blob mit einem angegebenen Namen zu kopieren, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Dateinamen an.<br>Um eine Teilmenge der Blobs in einem Ordner zu kopieren, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Platzhalterfilter an. 

**Beispiel:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Blob storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>Legacy-Kopieraktivität: Quellenmodell

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **BlobSource** festgelegt werden. |Ja |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Beachten Sie Folgendes: Wenn „recursive“ auf „true“ festgelegt ist und es sich bei der Senke um einen dateibasierten Speicher handelt, wird ein leerer Ordner oder Unterordner nicht in die Senke kopiert und dort auch nicht erstellt.<br/>Zulässige Werte sind **true** (Standard) und **false**. | Nein |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit einem Speicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="legacy-copy-activity-sink-model"></a>Legacy-Kopieraktivität – Senkenmodell

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **BlobSink** festgelegt werden. |Ja |
| copyBehavior | Definiert das Kopierverhalten, wenn es sich bei der Quelle um Dateien aus einem dateibasierten Datenspeicher handelt.<br/><br/>Zulässige Werte sind:<br/><b>- PreserveHierarchy (Standard)</b>: Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><b>- FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich auf der ersten Ebene des Zielordners. Die Namen für die Zieldateien werden automatisch generiert. <br/><b>- MergeFiles</b>: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Datei- oder Blobname angegeben wurde, entspricht der Name der Zusammenführungsdatei dem angegebenen Namen. Andernfalls wird der Dateiname automatisch generiert. | Nein |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit einem Speicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
