---
title: Speicheraspekte für Azure Functions
description: Erfahren Sie über die Speicheranforderungen von Azure Functions und über das Verschlüsseln gespeicherter Daten.
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: f094996ca44ec36d46330e54eac56b28794ef22e
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78358180"
---
# <a name="storage-considerations-for-azure-functions"></a>Speicheraspekte für Azure Functions

Azure Functions erfordert ein Azure Storage-Konto, wenn Sie eine Funktions-App-Instanz erstellen. Die folgenden Speicherdienste können von ihrer Funktions-App verwendet werden:


|Speicherdienst  | Verwendung in Functions  |
|---------|---------|
| [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)     | Aufrechterhalten von Bindungszustand und Funktionsschlüsseln.  <br/>Wird auch von [Aufgabenhubs in Durable Functions](durable/durable-functions-task-hubs.md) verwendet. |
| [Azure Files](../storage/files/storage-files-introduction.md)  | Dateifreigabe, die zum Speichern und Ausführen Ihres Funktions-App-Codes in einem [Verbrauchstarif](functions-scale.md#consumption-plan) verwendet wird. |
| [Azure Queue Storage](../storage/queues/storage-queues-introduction.md)     | Wird von [Aufgabenhubs in Durable Functions](durable/durable-functions-task-hubs.md) verwendet.   |
| [Azure Table Storage](../storage/tables/table-storage-overview.md)  |  Wird von [Aufgabenhubs in Durable Functions](durable/durable-functions-task-hubs.md) verwendet.       |

> [!IMPORTANT]
> Wenn der verbrauchsbasierte Hostingplan verwendet wird, werden die Funktionscode- und Bindungskonfigurationsdateien in Azure File Storage im Hauptspeicherkonto gespeichert. Wenn Sie das Hauptspeicherkonto löschen, wird dieser Inhalt ebenfalls gelöscht und kann nicht wiederhergestellt werden.

## <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto

Beim Erstellen einer Funktions-App müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob-, Warteschlangen- und Tabellenspeicher unterstützt. Der Grund hierfür ist, dass Functions für Vorgänge wie das Verwalten von Triggern und das Ausführen von Protokollierfunktionen auf Azure Storage basiert. Einige Speicherkonten unterstützen keine Warteschlangen und Tabellen. Diese Konten umfassen reine Blobspeicherkonten, Azure Storage Premium und allgemeine Speicherkonten mit Replikation von ZRS. Diese nicht unterstützten Konten werden aus dem Blatt „Speicherkonto“ herausgefiltert, wenn eine Funktions-App erstellt wird.

Weitere Informationen zu Speicherkontotypen finden Sie unter [Einführung in die Azure Storage-Dienste](../storage/common/storage-introduction.md#azure-storage-services). 

Sie können zwar ein vorhandenes Speicherkonto mit ihrer Funktions-App verwenden, aber Sie müssen sicherstellen, dass diese Anforderungen erfüllt werden. Für Speicherkonten, die im Rahmen der Erstellung der Funktions-App erstellt werden, ist garantiert, dass sie diese Speicherkontenanforderungen erfüllen.  

## <a name="storage-account-guidance"></a>Speicherkontoanleitung

Jede Funktions-App benötigt für den Betrieb ein Speicherkonto. Wenn dieses Konto gelöscht wird, läuft Ihre Funktions-App nicht. Informationen zur Behandlung speicherbezogener Probleme finden Sie unter [Behandeln speicherbezogener Probleme](functions-recover-storage-account.md). Die folgenden zusätzlichen Überlegungen gelten für das von Funktions-Apps verwendete Speicherkonto.

### <a name="storage-account-connection-setting"></a>Verbindungszeichenfolge für das Speicherkonto

Die Speicherkontoverbindung wird in der Anwendungseinstellung [AzureWebJobsStorage](./functions-app-settings.md#azurewebjobsstorage) verwaltet. 

Die Verbindungszeichenfolgen für das Speicherkonto muss aktualisiert werden, wenn Sie Speicherschlüssel neu generieren. [Erfahren Sie hier mehr über die Verwaltung von Speicherschlüsseln](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

### <a name="shared-storage-accounts"></a>Freigegebene Speicherkonten

Es ist möglich, dass mehrere Funktions-Apps dasselbe Speicherkonto ohne Probleme gemeinsam nutzen. Beispielsweise können Sie in Visual Studio mit dem Azure Storage-Emulator mehrere Apps entwickeln. In diesem Fall verhält sich der Emulator wie ein einzelnes Speicherkonto. Dasselbe Speicherkonto, das von Ihrer Funktions-App verwendet wird, kann auch von zum Speichern Ihrer Anwendungsdaten verwendet werden. Dieser Ansatz ist jedoch in einer Produktionsumgebung nicht immer eine gute Idee.

### <a name="optimize-storage-performance"></a>Optimieren der Speicherleistung

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

## <a name="storage-data-encryption"></a>Speicherdatenverschlüsselung

Azure Storage verschlüsselt alle Daten in einem ruhenden Speicherkonto. Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../storage/common/storage-service-encryption.md).

Standardmäßig werden Daten mit von Microsoft verwalteten Schlüsseln verschlüsselt. Für eine zusätzliche Kontrolle über die Verschlüsselungsschlüssel können Sie vom Kunden verwaltete Schlüssel bereitstellen, mit denen Blob- und Dateidaten verschlüsselt werden können. Diese Schlüssel müssen in Azure Key Vault vorhanden sein, damit Functions auf das Speicherkonto zugreifen kann. Weitere Informationen finden Sie unter [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault über das Azure-Portal](../storage/common/storage-encryption-keys-portal.md).  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Hostingoptionen von Azure Functions.

> [!div class="nextstepaction"]
> [Skalierung und Hosting von Azure Functions](functions-scale.md)


