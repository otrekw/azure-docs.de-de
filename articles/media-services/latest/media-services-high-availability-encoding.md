---
title: Hochverfügbarkeitscodierung für Azure Media Services
description: Erfahren Sie, wie Sie im Falle eines Ausfalls oder Fehlers des regionalen Rechenzentrums ein Failover auf ein sekundäres Media Services-Konto ausführen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: afaa7545fbcbab016249e73a2247817310c5cdfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78934193"
---
# <a name="media-services-high-availability-encoding"></a>Hochverfügbarkeitscodierung für Media Services 

Der Azure Media Services-Codierungsdienst ist eine regionale Batchverarbeitungsplattform und derzeit nicht für die Hochverfügbarkeit innerhalb einer einzelnen Region konzipiert. Der Codierungsdienst bietet derzeit keinen sofortigen Failover für den Dienst, wenn es einen Ausfall eines regionalen Rechenzentrums oder einen Ausfall der zugrunde liegenden Komponente oder abhängiger Dienste (z. B. Speicher, SQL) gibt. In diesem Artikel wird erläutert, wie Sie Media Services bereitstellen können, um eine Hochverfügbarkeitsarchitektur mit Failover zu verwalten und die optimale Verfügbarkeit Ihrer Anwendungen sicherzustellen.

Wenn Sie die in diesem Artikel beschriebenen Richtlinien und bewährten Methoden befolgen, verringern Sie das Risiko von Codierungsfehlern und Verzögerungen und minimieren die Wiederherstellungszeit, wenn ein Ausfall in einer einzelnen Region auftritt.

## <a name="how-to-build-a-cross-regional-encoding-system"></a>Erstellen eines überregionalen Codierungssystems

* [Erstellen](create-account-cli-how-to.md) Sie zwei (oder mehr) Azure Media Services-Konten.

    Die beiden Konten müssen sich in verschiedenen Regionen befinden. Weitere Informationen finden Sie unter [Regionen, in denen der Azure Media Services-Dienst bereitgestellt wird](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).
* Laden Sie Ihre Medien in dieselbe Region hoch, von der aus Sie den Auftrag übermitteln möchten. Weitere Informationen über den Beginn der Codierung finden Sie unter [Erstellen einer Auftragseingabe aus einer HTTPS-URL](job-input-from-http-how-to.md) oder [Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md).

    Wenn Sie dann den [Auftrag](transforms-jobs-concept.md) erneut in eine andere Region übermitteln müssen, können Sie JobInputHttp oder [Copy-Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) verwenden, um die Daten aus dem Quellcontainer für Medienobjekte in einen Medienobjektcontainer in der alternativen Region zu kopieren.
* Abonnieren Sie die JobStateChange-Nachrichten in den einzelnen Konten über Azure Event Grid. Weitere Informationen finden Sie unter

    * [Beispiel zur Audioanalyse](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer), das zeigt, wie ein Auftrag mit Azure Event Grid überwacht werden kann, einschließlich des Hinzufügens eines Fallbacks für den Fall, dass die Azure Event Grid-Nachrichten aus irgendeinem Grund verzögert werden.
    * [Azure Event Grid-Schemas für Media Services-Ereignisse](media-services-event-schemas.md)
    * [Registrieren für Ereignisse über das Azure-Portal oder die CLI](reacting-to-media-services-events.md) (Sie können dies auch über das EventGrid Management SDK erreichen)
    * [Microsoft.Azure.EventGrid SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/) (das Media Services-Ereignisse nativ unterstützt)

    Sie können Event Grid-Ereignisse auch über Azure Functions nutzen.
* Wenn Sie einen [Auftrag](transforms-jobs-concept.md) erstellen:

    * Wählen Sie ein zufälliges Konto aus der Liste der derzeit verwendeten Konten aus (diese Liste enthält normalerweise beide Konten, aber wenn Probleme festgestellt werden, kann sie nur ein Konto enthalten). Wenn die Liste leer ist, geben Sie eine Warnung aus, damit ein Operator dies untersuchen kann.
    * Als allgemeine Richtlinie gilt, dass Sie eine [reservierte Einheit für Medien](media-reserved-units-cli-how-to.md) pro [JobOutput](https://docs.microsoft.com/rest/api/media/jobs/create#joboutputasset) benötigen (es sei denn, Sie verwenden [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md), wobei drei reservierte Einheiten für Medien pro „JobOutput“ empfohlen werden).
    * Rufen Sie die Anzahl der für das gewählte Konto reservierten Einheiten für Medien (MRUs) ab. Wenn die aktuelle Anzahl der **reservierten Einheiten für Medien** noch nicht den maximalen Wert erreicht hat, fügen Sie die Anzahl der für den Auftrag benötigten MRUs hinzu, und aktualisieren Sie den Dienst. Wenn Ihre Auftragsübermittlungsrate hoch ist und Sie die MRUs häufig abfragen, um herauszufinden, dass Sie das Maximum erreicht haben, verwenden Sie einen verteilten Cache für den Wert mit einem angemessenen Zeitlimit.
    * Zählen Sie die Anzahl der Inflight-Aufträge.

* Wenn Ihr JobStateChange-Handler eine Benachrichtigung erhält, dass ein Auftrag den geplanten Zustand erreicht hat, notieren Sie die Zeit, zu der er in den geplanten Zustand eintritt, sowie die verwendete Region bzw. das verwendete Konto.
* Wenn Ihr JobStateChange-Handler eine Benachrichtigung erhält, dass ein Auftrag den Verarbeitungszustand erreicht hat, markieren Sie den Datensatz für den Auftrag als verarbeitet.
* Wenn Ihr JobStateChange-Handler eine Benachrichtigung erhält, dass ein Auftrag den Zustand Beendet/Fehlerhaft/Abgebrochen erreicht hat, markieren Sie den Datensatz für den Auftrag als abgeschlossen, und dekrementieren Sie die Anzahl der Inflight-Aufträge. Holen Sie sich die Anzahl der für das gewählte Konto reservierten Einheiten für Medien, und vergleichen Sie die aktuelle MRU-Nummer mit der Anzahl der Inflight-Aufträge. Wenn Ihre Inflight-Anzahl niedriger ist als die Anzahl der MRUs, dann dekrementieren Sie diese, und aktualisieren Sie anschließend den Dienst.
* Führen Sie einen separaten Prozess durch, der regelmäßig Ihre Datensätze zu den Aufträgen prüft.
    
    * Wenn Sie über Aufträge im Zustand „Geplant“ verfügen, die nicht innerhalb einer angemessenen Zeitspanne für eine bestimmte Region in den Verarbeitungszustand übergegangen sind, entfernen Sie diese Region aus Ihrer Liste der derzeit verwendeten Konten.  Je nach Ihren geschäftlichen Anforderungen könnten Sie beschließen, diese Aufträge sofort abzubrechen und sie erneut an die andere Region zu übermitteln. Oder Sie könnten ihnen etwas mehr Zeit geben, um in den nächsten Zustand zu wechseln.
    * Abhängig von der Anzahl der auf dem Konto konfigurierten reservierten Einheiten für Medien (MRUs) und der Übermittlungsrate kann es auch Aufträge in der Warteschlange geben, die das System noch nicht zur Verarbeitung abgerufen hat.  Wenn die Liste der Aufträge mit dem Zustand „In Warteschlange“ in einer Region über eine akzeptable Grenze hinaus zunimmt, können diese Aufträge abgebrochen und in der anderen Region übermittelt werden.  Dies kann jedoch ein Symptom dafür sein, dass nicht genügend reservierte Einheiten für Medien auf dem Konto für die aktuelle Last konfiguriert sind.  Bei Bedarf können Sie über Azure-Support ein höheres Kontingent an reservierten Einheiten für Medien anfordern.
    * Wenn eine Region von der Kontoliste entfernt wurde, überwachen Sie sie auf die Wiederherstellung, bevor Sie sie wieder in die Liste aufnehmen.  Die regionale Integrität kann über die bestehenden Aufträge in der Region überwacht werden (falls diese nicht abgebrochen und erneut übermittelt wurden), indem das Konto nach einer gewissen Zeit wieder in die Liste aufgenommen wird und indem die Operatoren die Azure-Kommunikation hinsichtlich Ausfällen überwachen, die Azure Media Services beeinträchtigen könnten.
    
Wenn Sie feststellen, dass die MRU-Anzahl sehr stark steigt und fällt, verschieben Sie die Logik der Dekrementierung in die periodische Aufgabe. Lassen Sie die Logik vor der Auftragsübermittlung die Inflight-Anzahl mit der aktuellen MRU-Anzahl vergleichen, um zu prüfen, ob sie die MRUs aktualisieren muss.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von regionsübergreifendem Video on Demand-Streaming](media-services-high-availability-streaming.md)
* Schauen Sie sich [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-media-services) an
