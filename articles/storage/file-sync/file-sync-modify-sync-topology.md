---
title: Ändern der Topologie für die Azure-Dateisynchronisierung | Microsoft-Dokumentation
description: Anleitung zum Ändern der Topologie für die Azure-Dateisynchronisierung
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 4/23/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 4977b56265f542f8f8ff13693abb468bc695a384
ms.sourcegitcommit: 18cd3c1c8cc47258c6a1a04e0e03d6248c52ef24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107992729"
---
# <a name="modify-your-azure-file-sync-topology"></a>Ändern der Topologie für die Azure-Dateisynchronisierung

In diesem Artikel werden die gängigsten Methoden beschrieben, die Kunden zum Ändern der Topologie für die Azure-Dateisynchronisierung zur Verfügung stehen. Außerdem werden Empfehlungen für die Vorgehensweise vorgestellt. Wenn Sie Ihre Topologie für die Azure-Dateisynchronisierung ändern möchten, lesen Sie die nachstehenden Best Practices, um Fehler und/oder einen potenziellen Datenverlust zu vermeiden.

## <a name="migrate-a-server-endpoint-to-a-different-azure-file-sync-storage-sync-service"></a>Migrieren eines Serverendpunkts zu einem anderen Speichersynchronisierungsdienst für die Azure-Dateisynchronisierung

Nachdem Sie sichergestellt haben, dass Ihre Daten auf dem lokalen Server auf dem neuesten Stand sind, heben Sie die Bereitstellung Ihres Serverendpunkts auf. Anweisungen hierzu finden Sie unter [Aufheben der Bereitstellung Ihres Serverendpunkts für die Azure-Dateisynchronisierung](./file-sync-deprovision-server-endpoint.md#scenario-2-you-intend-to-delete-your-server-endpoint-and-stop-using-this-specific-azure-file-share). Führen Sie anschließend eine erneute Bereitstellung in der gewünschten Synchronisierungsgruppe und im Speichersynchronisierungsdienst durch.

Wenn Sie alle Serverendpunkte, die einem Server zugeordnet sind, zu einer anderen Synchronisierungsgruppe oder einem anderen Speichersynchronisierungsdienst migrieren möchten, finden Sie weitere Informationen unter [Aufheben der Bereitstellung aller Serverendpunkte, die einem registrierten Server zugeordnet sind](#deprovision-all-server-endpoints-associated-with-a-registered-server).

## <a name="change-the-granularity-of-a-server-endpoint"></a>Ändern der Granularität eines Serverendpunkts

Nachdem Sie sich vergewissert haben, dass Ihre Daten auf dem lokalen Server auf dem neuesten Stand sind (siehe [Aufheben der Bereitstellung Ihres Serverendpunkts für die Azure-Dateisynchronisierung](./file-sync-deprovision-server-endpoint.md#scenario-2-you-intend-to-delete-your-server-endpoint-and-stop-using-this-specific-azure-file-share)), heben Sie die Bereitstellung Ihres Serverendpunkts auf. Führen Sie dann eine erneute Bereitstellung mit der gewünschten Granularität durch.

## <a name="deprovision-azure-file-sync-topology"></a>Aufheben der Bereitstellung der Topologie für die Azure-Dateisynchronisierung

Die Bereitstellung von Ressourcen für die Azure-Dateisynchronisierung muss in einer bestimmten Reihenfolge aufgehoben werden: Serverendpunkte, Synchronisierungsgruppe, Speicherdienst. Nachstehend wird der gesamte Ablauf dokumentiert ist, Sie können den Vorgang jedoch an einem beliebigen Punkt beenden. 

Navigieren Sie zunächst im Azure-Portal zur Ressource für den Speichersynchronisierungsdienst, und wählen Sie im Speichersynchronisierungsdienst eine Synchronisierungsgruppe aus. Führen Sie die Schritte unter [Aufheben der Bereitstellung Ihres Serverendpunkts für die Azure-Dateisynchronisierung](./file-sync-deprovision-server-endpoint.md) aus, um Datenintegrität und -verfügbarkeit beim Löschen von Serverendpunkten sicherzustellen. Um die Bereitstellung Ihrer Synchronisierungsgruppe oder des Speichersynchronisierungsdiensts aufzuheben, müssen alle Serverendpunkte gelöscht werden. Wenn Sie nur bestimmte Serverendpunkte löschen möchten, können Sie den Vorgang hier beenden. 

Nachdem Sie alle Serverendpunkte in der Synchronisierungsgruppe gelöscht haben, löschen Sie den Cloudendpunkt. 

Löschen Sie anschließend die Synchronisierungsgruppe. 

Wiederholen Sie diese Schritte für alle Synchronisierungsgruppen im Speichersynchronisierungsdienst, die Sie löschen möchten. Nachdem alle Synchronisierungsgruppen in diesem Speichersynchronisierungsdienst gelöscht wurden, löschen Sie die Ressource für den Speichersynchronisierungsdienst.

## <a name="rename-a-server-endpoint-path-or-sync-group"></a>Umbenennen eines Serverendpunktpfads oder einer Synchronisierungsgruppe

Dies wird derzeit nicht unterstützt. 

Wenn Sie derzeit Laufwerk D verwenden und eine Migration zur Cloud planen, finden Sie weitere Informationen unter [Verwenden des Laufwerks „D:“ einer VM als Datenlaufwerk](https://docs.microsoft.com/azure/virtual-machines/windows/change-drive-letter).

## <a name="deprovision-all-server-endpoints-associated-with-a-registered-server"></a>Aufheben der Bereitstellung aller Serverendpunkte, die einem registrierten Server zugeordnet sind

Um vor dem Aufheben der Bereitstellung sicherzustellen, dass Ihre Daten sicher sind und vollständig aktualisiert wurden, lesen Sie den Artikel [Aufheben der Bereitstellung Ihres Serverendpunkts für die Azure-Dateisynchronisierung](./file-sync-deprovision-server-endpoint.md).

Navigieren Sie zur Ressource Ihres Speichersynchronisierungsdiensts und dann zur Registerkarte „Registrierte Server“. Wählen Sie den Server aus, für den Sie die Registrierung aufheben möchten, und klicken Sie auf **Registrierung des Servers aufheben**. Hierdurch wird umgehend die Bereitstellung für alle Serverendpunkte aufgehoben, die diesem Server zugeordnet sind.

## <a name="next-steps"></a>Nächste Schritte
* [Aufheben der Bereitstellung Ihres Serverendpunkts für die Azure-Dateisynchronisierung](./file-sync-deprovision-server-endpoint.md)



