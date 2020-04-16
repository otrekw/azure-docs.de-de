---
title: REST-API für die Medienobjektkonvertierung
description: Beschreibt, wie Sie ein Medienobjekt über die REST-API konvertieren.
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: 38116efc9e87eca8e2514a0a84045a69b8d42326
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887043"
---
# <a name="use-the-model-conversion-rest-api"></a>Verwenden der REST-API für die Modellkonvertierung

Der [Modellkonvertierungsdienst](model-conversion.md) wird über eine [REST-API](https://en.wikipedia.org/wiki/Representational_state_transfer) gesteuert. In diesem Artikel werden die Details der Konvertierungsdienst-API beschrieben.

## <a name="regions"></a>Regions

Informationen zu den Basis-URLs, an die Anforderungen gesendet werden, finden Sie in der [Liste der verfügbaren Regionen](../../reference/regions.md).

## <a name="common-headers"></a>Allgemeine Header

### <a name="common-request-headers"></a>Allgemeine Anforderungsheader

Diese Header müssen für alle Anforderungen angegeben werden:

- Der **Authorization**-Header muss den Wert „Bearer [*TOKEN*]“ aufweisen, wobei [*TOKEN*] ein [Dienstzugriffstoken](../tokens.md) ist.

### <a name="common-response-headers"></a>Allgemeine Antwortheader

Alle Antworten enthalten die folgenden Header:

- Der **MS-CV**-Header enthält eine eindeutige Zeichenfolge, die verwendet werden kann, um den Aufruf innerhalb des Diensts nachzuverfolgen.

## <a name="endpoints"></a>Endpunkte

Der Konvertierungsdienst stellt drei REST-API-Endpunkte für Folgendes bereit:

- Starten der Modellkonvertierung mithilfe eines Speicherkontos, das mit Ihrem Azure Remote Rendering-Konto verknüpft ist. 
- Starten der Modellkonvertierung mithilfe der bereitgestellten *Shared Access Signatures (SAS)* .
- Abfragen des Konvertierungsstatus

### <a name="start-conversion-using-a-linked-storage-account"></a>Starten der Konvertierung mithilfe eines verknüpften Speicherkontos
Ihr Azure Remote Rendering-Konto muss Zugriff auf das angegebene Speicherkonto besitzen, indem Sie die Schritte zum [Verknüpfen von Speicherkonten](../create-an-account.md#link-storage-accounts) ausführen.

| Endpunkt | Methode |
|-----------|:-----------|
| /v1/accounts/**accountID**/conversions/create | POST |

Gibt die ID der aktuell ausgeführten Konvertierung in einem JSON-Dokument als Wrapper zurück. Der Feldname lautet „conversionId“.

#### <a name="request-body"></a>Anforderungstext


```json
{
    "input":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container containing your input asset data>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the input blob container>",
        "inputAssetPath" : "<path to the model in the input blob container relative to the folderPath (or container root if no folderPath is specified)>"
    },
    "output":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container where the converted asset will be copied to>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the output blob container. Will contain the asset and log files>",
        "outputAssetFileName": "<optional: can be omitted or empty. The filename of the converted asset. If provided the filename needs to end in .arrAsset>"
    }
}
```
### <a name="start-conversion-using-provided-shared-access-signatures"></a>Starten der Konvertierung mithilfe der bereitgestellten Shared Access Signatures (SAS)
Wenn Ihr ARR-Konto nicht mit Ihrem Speicherkonto verknüpft ist, können Sie mithilfe dieser REST-Schnittstelle Zugriff über *Shared Access Signatures (SAS)* bereitstellen.

| Endpunkt | Methode |
|-----------|:-----------|
| /v1/accounts/**accountID**/conversions/createWithSharedAccessSignature | POST |

Gibt die ID der aktuell ausgeführten Konvertierung in einem JSON-Dokument als Wrapper zurück. Der Feldname lautet „conversionId“.

#### <a name="request-body"></a>Anforderungstext

Der Anforderungstext ist derselbe wie im oben aufgeführten REST-Befehl „create“, aber die Eingabe und die Ausgabe enthält jeweils *SAS-Token (Shared Access Signature)* . Diese Token ermöglichen den Zugriff auf das Speicherkonto zum Lesen der Eingabe und zum Schreiben des Konvertierungsergebnisses.

> [!NOTE]
> Diese SAS-URI-Token sind die Abfragezeichenfolgen und nicht der vollständige URI. 


```json
{
    "input":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container containing your input asset data>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the input blob container>",
        "inputAssetPath" : "<path to the model in the input blob container relative to the folderPath (or container root if no folderPath is specified)>",
        "containerReadListSas" : "<a container SAS token which gives read and list access to the given input blob container>"
    },
    "output":
    {
        "storageAccountname": "<the name of a connected storage account - this does not include the domain suffix (.blob.core.windows.net)>",
        "blobContainerName": "<the name of the blob container where the converted asset will be copied to>",
        "folderPath": "<optional: can be omitted or empty - a subpath in the output blob container. Will contain the asset and log files>",
        "outputAssetFileName": "<optional: can be omitted or empty. The filename of the converted asset. If provided the filename needs to end in .arrAsset>",
        "containerWriteSas" : "<a container SAS token which gives write access to the given output blob container>"
    }
}
```

### <a name="poll-conversion-status"></a>Abrufen des Konvertierungsstatus
Der Status einer aktuell ausgeführten Konvertierung, die mit einem der oben genannten REST-Aufrufe gestartet wurde, kann mithilfe der folgenden Schnittstelle abgefragt werden:


| Endpunkt | Methode |
|-----------|:-----------|
| /v1/accounts/**accountID**/conversions/**conversionId** | GET |

Gibt ein JSON-Dokument mit einem Feld „status“ zurück, das die folgenden Werte aufweisen kann:

- „Running“ (wird ausgeführt)
- „Success“
- „Failure“ (Fehler)

Wenn der Status „Failure“ lautet, wird ein zusätzliches Feld „error“ mit einem Unterfeld „Message“ angezeigt, das Fehlerinformationen enthält. Zusätzliche Protokolle werden in Ihren Ausgabecontainer hochgeladen.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Azure Blob Storage für die Modellkonvertierung](blob-storage.md)
- [Modellkonvertierung](model-conversion.md)
