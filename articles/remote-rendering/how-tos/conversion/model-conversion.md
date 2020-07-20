---
title: Modellkonvertierung
description: Informationen zum Prozess der Konvertierung eines Modells für das Rendering
author: jakrams
ms.author: jakras
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: db05c7a5a45221485ecb62c2c90d56be52d5ef48
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84808611"
---
# <a name="convert-models"></a>Konvertieren von Modellen

Mit Azure Remote Rendering können Sie sehr komplexe Modelle rendern. Zur Erzielung einer maximalen Leistung müssen die Daten vorverarbeitet werden, damit sie in einem optimalen Format vorliegen. Je nach Datenmenge kann dieser Schritt eine Weile dauern. Es wäre unpraktisch, wenn dieser zeitliche Aufwand beim Laden des Modells in Anspruch anfallen würde. Außerdem wäre es verschwenderisch, diesen Prozess für mehrere Sitzungen zu wiederholen. Aus diesen Gründen bietet der ARR-Dienst einen dedizierten *Konvertierungsdienst*, den Sie im Vorfeld ausführen können.
Nach der Konvertierung kann ein Modell aus einem Azure Storage-Konto geladen werden.

## <a name="supported-source-formats"></a>Unterstützte Quellformate

Der Konvertierungsdienst unterstützt folgende Formate:

- **FBX** (ab Version 2011)
- **GLTF** (Version 2.x)
- **GLB** (Version 2.x)

Es gibt geringfügige Unterschiede zwischen den Formaten im Hinblick auf die Konvertierung von Materialeigenschaften, was unter [Materialzuordnung für Modellformate](../../reference/material-mapping.md) erläutert wird.

## <a name="the-conversion-process"></a>Der Konvertierungsprozess

1. [Bereiten Sie zwei Azure Blob Storage-Container vor](blob-storage.md): einen für die Eingabe und einen für die Ausgabe.
1. Laden Sie Ihr Modell in den Eingabecontainer hoch (optional unter einem Unterpfad).
1. Lösen Sie den Konvertierungsprozess über [die REST-API zur Modellkonvertierung](conversion-rest-api.md) aus.
1. Fragen Sie den Dienst nach dem Status der Konvertierung ab.
1. Laden Sie nach Abschluss ein Modell
    - entweder aus einem verknüpften Speicherkonto (siehe die Schritte unter „Verknüpfen von Speicherkonten“ unter [Erstellen eines Kontos](../create-an-account.md#link-storage-accounts) zum Herstellen einer Verknüpfung mit Ihrem Speicherkonto).
    - oder durch Angeben einer *Shared Access Signature (SAS)* .

Alle Modelldaten (Ein- und Ausgabe) werden im vom Benutzer bereitgestellten Azure-Blobspeicher gespeichert. Azure Remote Rendering bietet Ihnen die volle Kontrolle über die Verwaltung Ihrer Objekte.

## <a name="pricing"></a>Preise

Informationen zu den Preisen für die Konvertierung finden Sie auf der Seite [Remote Rendering – Preise](https://azure.microsoft.com/pricing/details/remote-rendering).


## <a name="conversion-parameters"></a>Konvertierungsparameter

Informationen zu den verschiedenen Konvertierungsoptionen finden Sie in [diesem Kapitel](configure-model-conversion.md).

## <a name="examples"></a>Beispiele

- [Schnellstart: Konvertieren eines Modells für das Rendering](../../quickstarts/convert-model.md) bietet eine detaillierte Einführung in das Konvertieren eines Modells.
- [PowerShell-Beispielskripts](../../samples/powershell-example-scripts.md), die die Verwendung des Konvertierungsdiensts veranschaulichen, finden Sie im [Repository mit den ARR-Beispielen](https://github.com/Azure/azure-remote-rendering) im Ordner *Scripts*.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Azure Blob Storage für die Modellkonvertierung](blob-storage.md)
- [REST-API für die Modellkonvertierung](conversion-rest-api.md)
- [Konfigurieren der Modellkonvertierung](configure-model-conversion.md)
- [Materialzuordnung für Modellformate](../../reference/material-mapping.md)
