---
title: Migration eines lokalen NAS zur Azure-Dateisynchronisierung über Azure DataBox
description: Erfahren Sie, wie Sie Dateien von einem lokalen NAS-Speicherort (Network Attached Storage) mit der Azure-Dateisynchronisierung über Azure DataBox zu einer Hybrid Cloud-Bereitstellung migrieren.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 03/5/2021
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 144b2f23e40f315441c3de2482ae8aeffe77ec75
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102583524"
---
# <a name="use-databox-to-migrate-from-network-attached-storage-nas-to-a-hybrid-cloud-deployment-with-azure-file-sync"></a>Migrieren Sie mit Azure DataBox von Network Attached Storage (NAS) zu einer Hybrid Cloud-Bereitstellung mit der Azure-Dateisynchronisierung.

Dieser Artikel zur Migration ist einer von mehreren Artikeln im Zusammenhang mit den Schlüsselwörtern NAS, der Azure-Dateisynchronisierung und Azure DataBox. Überprüfen Sie, ob dieser Artikel für Ihr Szenario zutrifft:

> [!div class="checklist"]
> * Datenquelle: Network Attached Storage (NAS)
> * Migrationsroute: NAS &rArr; DataBox &rArr; Azure-Dateifreigabe &rArr; Synchronisierung mit Windows Server
> * Lokales Zwischenspeichern von Dateien: Ja, das endgültige Ziel ist eine Azure-Dateisynchronisierung-Bereitstellung.

Wenn Ihr Szenario anders ist, sehen Sie sich die [Tabelle mit Migrationsleitfäden](storage-files-migration-overview.md#migration-guides) an.

Die Azure-Dateisynchronisierung funktioniert an Standorten mit direkt angeschlossenem Speicher (Direct Attached Storage, DAS) und unterstützt keine Synchronisierung mit Standorten mit netzwerkverbundenem Speicher (Network Attached Storage, NAS).
Dadurch wird eine Migration Ihrer Dateien notwendig, und dieser Artikel führt Sie durch die Planung und Ausführung einer solchen Migration.

## <a name="migration-goals"></a>Migrationsziele

Das Ziel besteht darin, Freigaben auf Ihrem NAS-Gerät auf einen Windows-Server zu verschieben. Anschließend soll die Azure-Dateisynchronisierung für eine Hybrid Cloud-Bereitstellung verwendet werden. Diese Migration muss so erfolgen, dass die Integrität der Produktionsdaten und die Verfügbarkeit während der Migration gewährleistet wird. Letzteres erfordert minimale Ausfallzeiten, damit sie in normalen Wartungsfenstern stattfinden kann oder diese nur geringfügig überschreitet.

## <a name="migration-overview"></a>Übersicht zur Migration

Der Migrationsprozess besteht aus mehreren Phasen. Sie müssen Azure-Speicherkonten und Dateifreigaben bereitstellen, eine lokale Windows Server-Instanz bereitstellen, die Azure-Dateisynchronisierung konfigurieren, mit RoboCopy migrieren und schließlich die Übernahme durchführen. In den folgenden Abschnitten werden die Phasen des Migrationsprozesses ausführlich beschrieben.

> [!TIP]
> Wenn Sie zu diesem Artikel zurückkehren, wechseln Sie anhand der Navigation auf der rechten Seite zu der Migrationsphase, in der Sie aufgehört haben.

## <a name="phase-1-identify-how-many-azure-file-shares-you-need"></a>Phase 1: Ermitteln der Anzahl der benötigten Azure-Dateifreigaben

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="phase-2-deploy-azure-storage-resources"></a>Phase 2: Bereitstellen von Azure Storage-Ressourcen

Sehen Sie sich in dieser Phase die Zuordnungstabelle aus Phase 1 an, und verwenden Sie diese, um die richtige Anzahl von Azure-Speicherkonten und Dateifreigaben darin bereitzustellen.

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

## <a name="phase-3-determine-how-many-azure-databox-appliances-you-need"></a>Phase 3: Bestimmen der Anzahl der benötigten Azure DataBox-Appliances

Starten Sie diesen Schritt nur, wenn Sie die vorherige Phase abgeschlossen haben. Ihre Azure Storage-Ressourcen (Speicherkonten und Dateifreigaben) sollten zu diesem Zeitpunkt erstellt werden. Bei der DataBox-Bestellung müssen Sie angeben, in welche Speicherkonten die DataBox Daten verschieben soll.

In dieser Phase müssen Sie die Ergebnisse des Migrationsplans aus der vorherigen Phase den Grenzwerten der verfügbaren DataBox-Optionen zuordnen. Diese Überlegungen helfen Ihnen bei der Planung, welche DataBox-Optionen Sie auswählen sollten, und wie viele sie davon benötigen, um Ihre NAS-Freigaben in Azure-Dateifreigaben zu verschieben.

Um zu ermitteln, wie viele Geräte welchen Typs Sie benötigen, sollten Sie die folgenden wichtigen Grenzwerte beachten:

* Beliebige Azure DataBox-Geräte können Daten in bis zu 10 Speicherkonten verschieben. 
* Jede DataBox-Option verfügt über eine eigene nutzbare Kapazität. Siehe [DataBox-Optionen](#databox-options).

Entnehmen Sie Ihrem Migrationsplan die Anzahl der Speicherkonten, die Sie erstellen möchten, und die Freigaben in jedem einzelnen. Sehen Sie sich dann die Größe der einzelnen Freigaben in Ihrem NAS an. Die Kombination dieser Informationen erlaubt Ihnen, zu optimieren und zu entscheiden, welche Appliance Daten an welche Speicherkonten senden soll. Sie können zwei DataBox-Geräte verwenden, um Dateien in dasselbe Speicherkonto zu verschieben, aber teilen Sie den Inhalt einer einzelnen Dateifreigabe nicht auf zwei DataBox-Geräte auf.

### <a name="databox-options"></a>DataBox-Optionen

Für eine Standardmigration sollte eine DataBox-Option oder eine Kombination dieser drei DataBox-Optionen ausgewählt werden: 

* DataBox Disks Microsoft sendet Ihnen einen und bis zu fünf SSD-Datenträger mit einer Kapazität von jeweils 8 TiB (maximal 40 TiB) zu. Die nutzbare Kapazität ist aufgrund der Verschlüsselung und des Dateisystemoverheads 20 % niedriger. Weitere Informationen finden Sie unter [Was ist ein Azure DataBox-Datenträger?](../../databox/data-box-disk-overview.md).
* DataBox: Dies ist die gängigste Option. Eine robuste DataBox-Appliance, die ähnlich wie ein NAS funktioniert, wird Ihnen geliefert. Sie verfügt über eine verwendbare Kapazität von 80 TiB. Weitere Informationen finden Sie unter [Was ist Azure DataBox?](../../databox/data-box-overview.md).
* DataBox Heavy: Diese Option bietet eine robuste DataBox-Appliance auf Rädern, die ähnlich wie ein NAS mit einer Kapazität von 1 PiB funktioniert. Die nutzbare Kapazität ist aufgrund der Verschlüsselung und des Dateisystemoverheads 20 % niedriger. Weitere Informationen finden Sie unter [Was ist Azure Data Box Heavy?](../../databox/data-box-heavy-overview.md).

## <a name="phase-4-provision-a-suitable-windows-server-on-premises"></a>Phase 4: Lokales Bereitstellen einer geeigneten Windows Server-Instanz

Während Sie auf das Eintreffen Ihrer Azure DataBox(en) warten, können Sie bereits mit der Überprüfung der Notwendigkeit einer oder mehrerer Windows Server-Instanzen beginnen, die Sie mit der Azure-Dateisynchronisierung verwenden werden.

* Erstellen Sie einen Server mit Windows Server 2019 (oder mindestens 2012 R2) als virtuellen Computer oder physischen Server. Ein Windows Server-Failovercluster wird ebenfalls unterstützt.
* Stellen Sie direkt angeschlossenen Speicher (DAS, im Gegensatz zum nicht unterstützten NAS) bereit, oder fügen Sie ihn hinzu.

Die Ressourcenkonfiguration (Compute und RAM) der von Ihnen bereitgestellten Windows Server-Instanz hängt größtenteils von der Anzahl der Elemente (Dateien und Ordner) ab, die synchronisiert werden sollen. Wenn Sie Bedenken haben, sollten Sie eine leistungsstärkere Konfiguration verwenden.

[Hier erfahren Sie, wie Sie die Größe eines Windows-Servers basierend auf der Anzahl der zu synchronisierenden Elemente (Dateien und Ordner) anpassen.](storage-sync-files-planning.md#recommended-system-resources)

> [!NOTE]
> Der zuvor verknüpfte Artikel enthält eine Tabelle mit einem Bereich für den Serverarbeitsspeicher (RAM). Sie können sich an der geringeren Zahl für Ihren Server orientieren, müssen jedoch davon ausgehen, dass die anfängliche Synchronisierung wesentlich mehr Zeit in Anspruch nehmen kann.

## <a name="phase-5-copy-files-onto-your-databox"></a>Phase 5: Kopieren von Dateien auf Ihre DataBox

Wenn Ihre DataBox eintrifft, müssen Sie sie in Sichtlinie mit Ihrer NAS-Appliance einrichten. Befolgen Sie die Installationsdokumentation für den DataBox-Typ, den Sie bestellt haben.

* [Einrichten von Data Box](../../databox/data-box-quickstart-portal.md)
* [Einrichten von Data Box Disk](../../databox/data-box-disk-quickstart-portal.md)
* [Einrichten von Data Box Heavy](../../databox/data-box-heavy-quickstart-portal.md)

Abhängig vom DataBox-Typ sind möglicherweise DataBox-Kopiertools verfügbar. An diesem Punkt werden sie nicht für Migrationen zu Azure-Dateifreigaben empfohlen, da sie Ihre Dateien nicht mit voller Genauigkeit in die DataBox kopieren. Verwenden Sie stattdessen RoboCopy.

Wenn Ihre DataBox eintrifft, weist sie für jedes Speicherkonto, das Sie zum Zeitpunkt der Bestellung angegeben haben, vorab bereitgestellte SMB-Freigaben auf.

* Wenn Ihre Dateien in einer Azure-Premium-Dateifreigabe gespeichert werden, steht eine SMB-Freigabe pro Premium-File Storage-Speicherkonto zur Verfügung.
* Wenn Ihre Dateien in einem Standardspeicherkonto gespeichert werden, stehen drei SMB-Freigaben pro Standardspeicherkonto (GPv1 und GPv2) bereit. Nur die Dateifreigaben, die mit `_AzFiles` enden, sind für Ihre Migration relevant. Ignorieren Sie alle Block- und Seitenblobfreigaben.

Führen Sie die Schritte in der Azure DataBox-Dokumentation aus:

1. [Herstellen einer Verbindung mit der Data Box](../../databox/data-box-deploy-copy-data.md)
1. Kopieren von Daten auf die Data Box
1. [Vorbereiten Ihrer DataBox für den Versand zu Azure](../../databox/data-box-deploy-picked-up.md)

In der verknüpften DataBox-Dokumentation wird ein RoboCopy-Befehl angegeben. Der Befehl eignet sich jedoch nicht für die Beibehaltung der vollständigen Datei- und Ordnergenauigkeit. Verwenden Sie stattdessen diesen Befehl:

[!INCLUDE [storage-files-migration-robocopy](../../../includes/storage-files-migration-robocopy.md)]


## <a name="phase-6-deploy-the-azure-file-sync-cloud-resource"></a>Phase 6: Bereitstellen der Azure-Dateisynchronisierung-Cloudressource

Bevor Sie mit diesem Handbuch fortfahren, warten Sie, bis alle Dateien in den richtigen Azure-Dateifreigaben eingetroffen sind. Der Prozess des Versands und Erfassens von DataBox-Daten nimmt einige Zeit in Anspruch.

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

## <a name="phase-7-deploy-the-azure-file-sync-agent"></a>Phase 7: Bereitstellen des Azure-Dateisynchronisierung-Agents

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

## <a name="phase-8-configure-azure-file-sync-on-the-windows-server"></a>Phase 8: Konfigurieren der Azure-Dateisynchronisierung auf dem Windows-Server

Der registrierte lokale Windows-Server muss für diesen Prozess bereit und mit dem Internet verbunden sein.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

Aktivieren Sie das Cloudtiering-Feature, und wählen Sie im ursprünglichen Downloadabschnitt „Nur Namespace“ aus.

> [!IMPORTANT]
> Cloudtiering ist das AFS-Feature, das es dem lokalen Server ermöglicht, weniger Speicherkapazität als in der Cloud zu haben, aber trotzdem über den vollständigen Namespace zu verfügen. Lokal interessante Daten werden zudem für eine schnelle Zugriffsleistung lokal zwischengespeichert. Cloudtiering ist ein optionales Feature pro „Serverendpunkt“ der Azure-Dateisynchronisierung. Sie müssen dieses Feature verwenden, wenn Sie nicht über genügend lokale Datenträgerkapazität auf der Windows Server-Instanz verfügen, um alle Clouddaten aufzunehmen, und wenn Sie vermeiden möchten, dass alle Daten aus der Cloud heruntergeladen werden!

Wiederholen Sie die Schritte zum Erstellen einer Synchronisierungsgruppe und zum Hinzufügen des jeweiligen Serverordners als Serverendpunkt für alle Azure-Dateifreigaben/Serverspeicherorte, die für die Synchronisierung konfiguriert werden müssen. Warten Sie, bis die Synchronisierung des Namespace abgeschlossen ist. Im folgenden Abschnitt wird ausführlich erläutert, wie Sie dies sicherstellen können.

> [!NOTE]
> Nach der Erstellung eines Serverendpunkts funktioniert die Synchronisierung. Allerdings muss die Synchronisierung die Dateien und Ordner aufzählen (ermitteln), die Sie über DataBox in die Azure-Dateifreigabe verschoben haben. Abhängig von der Größe des Namespace kann dies einige Zeit in Anspruch nehmen, bis der Namespace der Cloud auf dem Server angezeigt wird.

## <a name="phase-9-wait-for-the-namespace-to-fully-appear-on-the-server"></a>Phase 9: Warten, bis der Namespace vollständig auf dem Server angezeigt wird

Es ist zwingend erforderlich, dass Sie mit den nächsten Schritten der Migration warten, bis der Server den Namespace vollständig von der Cloudfreigabe heruntergeladen hat. Wenn Sie zu früh beginnen, Dateien auf den Server zu verschieben, können Sie unnötige Uploads und sogar Dateisynchronisierungskonflikte riskieren.

Um festzustellen, ob der Server die anfängliche Downloadsynchronisierung abgeschlossen hat, öffnen Sie die Ereignisanzeige auf der synchronisierenden Windows Server-Instanz, und verwenden Sie das Telemetrieereignisprotokoll der Azure-Dateisynchronisierung.
Das Telemetrieereignisprotokoll befindet sich in der Ereignisanzeige unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“.

Suchen Sie nach dem letzten 9102-Ereignis. Nach Abschluss einer Synchronisierungssitzung wird ein Ereignis mit der ID 9102 protokolliert. Der Ereignistext enthält ein Feld für die Downloadsynchronisierungsrichtung. (`HResult` muss 0 (null) sein, die Anzahl heruntergeladener Dateien ebenfalls.)
 
Sie möchten zwei aufeinander folgende Ereignisse dieses Typs und Inhalts anzeigen, um zu erkennen, ob der Server das Herunterladen des Namespace abgeschlossen hat. Es ist in Ordnung, wenn verschiedene Ereignisse zwischen zwei 9102-Ereignissen ausgelöst werden.

## <a name="phase-10-catch-up-robocopy-from-your-nas"></a>Phase 10: Aktualisierung mit NAS-Änderungen mit RoboCopy

Nachdem der Server die anfängliche Synchronisierung des gesamten Namespace von der Cloudfreigabe abgeschlossen hat, können Sie mit diesem Schritt fortfahren. Es ist zwingend erforderlich, dass dieser Vorgang abgeschlossen ist, bevor Sie mit diesem Schritt fortfahren. Details hierzu finden Sie im vorherigen Abschnitt.

In diesem Schritt führen Sie RoboCopy-Aufträge aus, um Ihre Cloudfreigaben mit den neuesten Änderungen Ihres NAS, seitdem Sie Ihre Freigaben auf die DataBox verschoben haben, zu aktualisieren.
Diese Aktualisierung mit RoboCopy kann je nach Menge der Änderungen, die auf Ihren NAS-Freigaben aufgetreten sind, schnell beendet sein oder eine Weile dauern.

> [!WARNING]
> Aufgrund eines rückschrittlichen RoboCopy-Verhaltens in Windows Server 2019 ist die Befehlszeilenoption /MIR von RoboCopy nicht mit einem mehrstufigen Zielverzeichnis kompatibel. Für diese Phase der Migration dürfen Sie keinen Windows Server 2019- oder Windows 10-Client verwenden. Verwenden Sie RoboCopy auf einem mittleren Windows Server 2016-Release.

Der grundlegende Migrationsansatz besteht darin, mit RoboCopy eine Kopie Ihrer NAS-Appliance auf Ihrem Windows-Server anzulegen und diese mit der Azure-Dateisynchronisierung auf Azure-Dateifreigaben zu synchronisieren.

Erstellen Sie die erste lokale Kopie in Ihrem Windows Server-Zielordner:

1. Identifizieren Sie den ersten Speicherort auf Ihrer NAS-Appliance.
1. Identifizieren Sie den entsprechenden Ordner auf dem Windows-Server, auf dem die Azure-Dateisynchronisierung bereits konfiguriert wurde.
1. Starten des Kopiervorgangs mit RoboCopy

Mit dem folgenden RoboCopy-Befehl werden nur die Unterschiede (aktualisierte Dateien und Ordner) aus Ihrem NAS-Speicher in den Windows Server-Zielordner kopiert. Die Windows Server-Instanz synchronisiert sie dann mit der/den Azure-Dateifreigab(en). 

[!INCLUDE [storage-files-migration-robocopy](../../../includes/storage-files-migration-robocopy.md)]

Wenn Sie auf Ihrer Windows Server-Instanz weniger Speicher bereitgestellt haben, als ihre Daten auf der NAS-Appliance verwenden, haben Sie Cloudtiering konfiguriert. Wenn das lokale Windows Server-Volume voll ist, beginnt das [Cloudtiering](storage-sync-cloud-tiering-overview.md) von Dateien, die bereits erfolgreich synchronisiert wurden. Durch das Cloudtiering wird ausreichend Speicherplatz generiert, um mit dem Kopieren von der NAS-Appliance fortzufahren. Einmal pro Stunde wird überprüft, was bereits im Cloudtiering synchronisiert wurde, und Speicherplatz freigegeben, um auf dem Volume einen freien Speicherplatz von 99 % zu erreichen.
Es ist möglich, dass RoboCopy zahlreiche Dateien verschieben muss, deren Kapazität Ihren lokalen Speicher auf dem Windows-Server übersteigt. In der Regel können Sie davon ausgehen, dass RoboCopy viel schneller verschieben kann, als die Azure-Dateisynchronisierung Ihre Dateien hochladen und außerhalb Ihres lokalen Volumes ablegen kann. RoboCopy schlägt fehl. Es wird empfohlen, dass Sie die Freigaben in einer Sequenz durcharbeiten, die dies verhindert. Beispielsweise können Sie RoboCopy-Aufträge versetzt anstatt gleichzeitig für alle Freigaben starten oder nur Freigaben verschieben, die auf den aktuellen Umfang des freien Speicherplatzes auf dem Windows-Server zugeschnitten sind, um nur einige Möglichkeiten zu nennen. Die gute Nachricht ist, dass die /MIR-Befehlszeilenoption nur Deltas verschiebt, und sobald ein Delta verschoben wurde, muss ein neu gestarteter Auftrag diese Datei nicht erneut verschieben.

### <a name="user-cut-over"></a>Benutzerübernahme

Wenn Sie den RoboCopy-Befehl zum ersten Mal ausführen, greifen Ihre Benutzer und Anwendungen weiterhin auf Dateien auf dem NAS zu und ändern sie möglicherweise. Es kann vorkommen, dass RoboCopy ein Verzeichnis verarbeitet und mit dem nächsten fortfährt und dann ein Benutzer am Quellspeicherort (NAS) eine Datei hinzufügt, ändert oder löscht. Diese wird dann während dieser aktuellen RoboCopy-Ausführung nicht verarbeitet. Dies ist das erwartete Verhalten.

Bei der ersten Ausführung geht es darum, den Großteil der abgewanderten Daten auf Ihre Windows Server-Instanz und dann über die Azure-Dateisynchronisierung in die Cloud zu verschieben. Der erste Kopiervorgang kann einige Zeit in Anspruch nehmen, die von folgenden Faktoren abhängig ist:

* Uploadbandbreite
* Geschwindigkeit des lokalen Netzwerks und Anzahl der optimalen Übereinstimmungen von RoboCopy-Threads
* Anzahl von Elementen (Dateien und Ordner), die von RoboCopy und der Azure-Dateisynchronisierung verarbeitet werden müssen

Nachdem die erste Ausführung beendet wurde, führen Sie den Befehl erneut aus.

Wenn Sie RoboCopy zum zweiten Mal für dieselbe Freigabe ausführen, wird der Vorgang schneller abgeschlossen, da nur Änderungen, die seit der letzten Ausführung aufgetreten sind, übertragen werden müssen. Sie können wiederholte Aufträge für dieselbe Freigabe ausführen.

Wenn die Ausfallzeit für Sie akzeptabel ist, müssen Sie den Benutzerzugriff auf Ihre NAS-basierten Freigaben aufheben. Dazu können Sie jeden beliebigen Schritt ausführen, mit dem Benutzer daran gehindert werden, die Datei- und Ordnerstruktur sowie den Inhalt zu ändern. Ein Beispiel hierfür: Ihr DFS-Namespace verweist auf einen nicht vorhandenen Speicherort, oder Sie ändern die Stamm-ACLs auf der Freigabe.

Führen Sie einen letzten RoboCopy-Durchgang aus. Dadurch werden alle Änderungen übernommen, die möglicherweise ausgelassen wurden.
Wie lange dieser letzte Schritt dauert, hängt von der Geschwindigkeit des RoboCopy-Scans ab. Sie können die Zeit (gleich der Ausfallzeit) schätzen, indem Sie messen, wie lange die vorherige Ausführung gedauert hat.

Erstellen Sie eine Freigabe für den Windows Server-Ordner, und passen Sie Ihre DFS-N-Bereitstellung ggf. so an, dass sie auf diese zeigt. Stellen Sie sicher, dass Sie die gleichen Berechtigungen auf Freigabeebene wie auf Ihrer NAS-SMB-Freigabe festlegen. Wenn Sie einen NAS hatten, der in eine Domäne eingebunden war, stimmen die Benutzer-SIDs automatisch überein, da die Benutzer in Active Directory vorhanden sind und RoboCopy Dateien und Metadaten in voller Genauigkeit kopiert. Wenn Sie lokale Benutzer in Ihrem NAS verwendet haben, müssen Sie diese Benutzer als lokale Windows Server-Benutzer erstellen und die vorhandenen SIDs, die RoboCopy auf Ihre Windows Server-Instanz verschoben hat, den SIDs der neuen lokalen Benutzer von Windows Server zuordnen.

Sie haben die Migration einer Freigabe/Gruppe von Freigaben zu einem gemeinsamen Stamm oder Volume abgeschlossen. (Abhängig von der Zuordnung in Phase 1)

Sie können einige dieser Kopiervorgänge parallel ausführen. Es wird empfohlen, jeweils eine Azure-Dateifreigabe auf einmal zu verarbeiten.

## <a name="troubleshoot"></a>Problembehandlung

Das häufigste Problem, auf das Sie stoßen können, besteht darin, dass der RoboCopy-Befehl mit dem Fehler *Volume voll* auf Windows Server-Seite beendet wird. Das Cloudtiering wird einmal stündlich eingesetzt, um Inhalte vom lokalen Windows Server-Datenträger abzurufen, die bereits synchronisiert wurden. Das Ziel besteht darin, den freien Speicherplatz von 99 % auf dem Volume zu erreichen.

Warten Sie, bis durch den Synchronisierungsvorgang und das Cloudtiering Speicherplatz freigegeben wurde. Sie können dies im Datei-Explorer auf Ihrem Windows-Server beobachten.

Wenn Ihr Windows-Server ausreichende Kapazität aufweist, wird das Problem durch erneutes Ausführen des Befehls behoben. Falls Sie auf diese Situation stoßen, treten keine Schäden auf, und Sie können einfach fortfahren. Der zusätzliche Aufwand durch das erneute Ausführen des Befehls ist die einzige Folge.

Unter dem Link im folgenden Abschnitt finden Sie Informationen zur Problembehandlung der Azure-Dateisynchronisierung.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich weiter mit Azure-Dateifreigaben und der Azure-Dateisynchronisierung vertraut. In den folgenden Artikeln werden erweiterte Optionen, bewährte Methoden und auch Ansätze zur Problembehandlung erläutert. Diese Artikel sind mit der entsprechenden [Dokumentation zur Azure-Dateifreigabe](storage-files-introduction.md) verlinkt.

* [Migration overview (Übersicht über die Migration)](storage-files-migration-overview.md)
* [AFS-Übersicht](./storage-sync-files-planning.md)
* [AFS-Bereitstellungshandbuch](./storage-how-to-create-file-share.md)
* [AFS-Problembehandlung](storage-sync-files-troubleshoot.md)
