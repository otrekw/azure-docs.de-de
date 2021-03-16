---
title: Migration eines lokalen NAS zur Azure-Dateisynchronisierung
description: Erfahren Sie, wie Sie Dateien von einem lokalen NAS-Speicherort (Network Attached Storage) mit Azure-Dateisynchronisierung und Azure-Dateifreigaben zu einer Hybrid Cloud-Bereitstellung migrieren.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 03/19/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 86e79302716fa502d8562dd563b0a5c5fb220a67
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102547549"
---
# <a name="migrate-from-network-attached-storage-nas-to-a-hybrid-cloud-deployment-with-azure-file-sync"></a>Migrieren von Network Attached Storage (NAS) zu einer Hybrid Cloud-Bereitstellung mit der Azure-Dateisynchronisierung

Dieser Artikel zur Migration ist einer von mehreren Artikeln im Zusammenhang mit den Schlüsselwörtern „NAS“ und „Azure-Dateisynchronisierung“. Überprüfen Sie, ob dieser Artikel für Ihr Szenario zutrifft:

> [!div class="checklist"]
> * Datenquelle: Network Attached Storage (NAS)
> * Migrationsroute: NAS &rArr; Windows-Server&rArr; Upload und-Synchronisierung mit Azure-Dateifreigabe(n)
> * Lokales Zwischenspeichern von Dateien: Ja, das endgültige Ziel ist eine Bereitstellung einer Azure-Dateisynchronisierung.

Wenn Ihr Szenario anders ist, sehen Sie sich die [Tabelle mit Migrationsleitfäden](storage-files-migration-overview.md#migration-guides) an.

Die Azure-Dateisynchronisierung funktioniert an Standorten mit direkt angeschlossenem Speicher (Direct Attached Storage, DAS) und unterstützt keine Synchronisierung mit Standorten mit netzwerkverbundenem Speicher (Network Attached Storage, NAS).
Dadurch wird eine Migration Ihrer Dateien notwendig, und dieser Artikel führt Sie durch die Planung und Ausführung einer solchen Migration.

## <a name="migration-goals"></a>Migrationsziele

Das Ziel besteht darin, Freigaben auf Ihrem NAS-Gerät auf einen Windows-Server zu verschieben. Anschließend soll die Azure-Dateisynchronisierung für eine Hybrid Cloud-Bereitstellung verwendet werden. Migrationen müssen allgemein so durchgeführt werden, dass die Integrität der Produktionsdaten und die Verfügbarkeit während der Migration gewährleistet ist. Letzteres erfordert minimale Ausfallzeiten, damit sie in normalen Wartungsfenstern stattfinden kann oder diese nur geringfügig überschreitet.

## <a name="migration-overview"></a>Übersicht zur Migration

Wie im [Artikel mit der Migrationsübersicht](storage-files-migration-overview.md) zu Azure Files beschrieben, ist die Verwendung des richtigen Kopiertools und Ansatzes wichtig. Ihre NAS-Appliance macht SMB-Freigaben direkt in Ihrem lokalen Netzwerk verfügbar. Das in Windows Server integrierte Tool RoboCopy bietet die beste Methode, um Ihre Dateien in diesem Migrationsszenario zu verschieben.

- Phase 1: [Ermitteln der Anzahl der benötigten Azure-Dateifreigaben](#phase-1-identify-how-many-azure-file-shares-you-need)
- Phase 2: [Lokales Bereitstellen eines geeigneten Windows-Servers](#phase-2-provision-a-suitable-windows-server-on-premises)
- Phase 3: [Bereitstellen der Cloudressource für die Azure-Dateisynchronisierung](#phase-3-deploy-the-azure-file-sync-cloud-resource)
- Phase 4: [Bereitstellen von Azure Storage-Ressourcen](#phase-4-deploy-azure-storage-resources)
- Phase 5: [Bereitstellen des Azure-Dateisynchronisierungs-Agents](#phase-5-deploy-the-azure-file-sync-agent)
- Phase 6: [Konfigurieren der Azure-Dateisynchronisierung auf dem Windows-Server](#phase-6-configure-azure-file-sync-on-the-windows-server)
- Phase 7: [RoboCopy](#phase-7-robocopy)
- Phase 8: [Benutzerübernahme](#phase-8-user-cut-over)

## <a name="phase-1-identify-how-many-azure-file-shares-you-need"></a>Phase 1: Ermitteln der Anzahl der benötigten Azure-Dateifreigaben

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="phase-2-provision-a-suitable-windows-server-on-premises"></a>Phase 2: Lokales Bereitstellen eines geeigneten Windows-Servers

* Erstellen Sie einen Server mit Windows Server 2019 (oder mindestens 2012 R2) als virtuellen Computer oder physischen Server. Ein Windows Server-Failovercluster wird ebenfalls unterstützt.
* Stellen Sie direkt angeschlossenen Speicher (DAS, im Gegensatz zum nicht unterstützten NAS) bereit, oder fügen Sie ihn hinzu.

    Die von Ihnen bereitgestellte Speichermenge kann kleiner sein als diejenige, die Sie zurzeit auf Ihrer NAS-Appliance verwenden. Diese Konfigurationsoption erfordert, dass Sie auch das Feature [Cloudtiering](storage-sync-cloud-tiering-overview.md) der Azure-Dateisynchronisierung nutzen.
    Wenn Sie jedoch Ihre Dateien aus dem größeren NAS-Bereich in einer späteren Phase auf das kleinere Windows Server-Volume kopieren, müssen Sie in Batches arbeiten:

    1. Verschieben Sie einen Satz von Dateien, die auf den Datenträger passen.
    2. Lassen Sie die Datei-Synchronisierung und Cloudtiering interagieren.
    3. Wenn mehr freier Speicherplatz auf dem Volume erstellt wurde, fahren Sie mit dem nächsten Batch von Dateien fort. Überprüfen Sie alternativ den RoboCopy-Befehl im [Abschnitt „RoboCopy“](#phase-7-robocopy) weiter unten zur Verwendung des neuen Switches `/LFSM`. Die Verwendung von `/LFSM` kann Ihre RoboCopy-Aufträge erheblich vereinfachen, ist aber mit einigen anderen RoboCopy-Switches nicht kompatibel, von denen Sie möglicherweise abhängig sind.
    
    Sie können diesen Batchverarbeitungsansatz vermeiden, indem Sie auf dem Windows-Server den entsprechenden Speicherplatz bereitstellen, den Ihre Dateien auf der NAS-Appliance belegen. Ziehen Sie die Deduplizierung unter NAS/Windows in Betracht. Wenn Sie diese große Menge an Speicher nicht dauerhaft auf Ihren Windows-Server übertragen möchten, können Sie die Volumegröße nach der Migration und vor dem Anpassen der Cloudtieringrichtlinien verringern. Dadurch wird ein kleinerer lokaler Cache Ihrer Azure-Dateifreigaben erstellt.

Die Ressourcenkonfiguration (Compute und RAM) der von Ihnen bereitgestellten Windows Server-Instanz hängt größtenteils von der Anzahl der Elemente (Dateien und Ordner) ab, die synchronisiert werden sollen. Wenn Sie Bedenken haben, empfiehlt es sich, eine leistungsstärkere Konfiguration zu verwenden.

[Hier erfahren Sie, wie Sie die Größe eines Windows-Servers basierend auf der Anzahl der zu synchronisierenden Elemente (Dateien und Ordner) anpassen.](storage-sync-files-planning.md#recommended-system-resources)

> [!NOTE]
> Der zuvor verknüpfte Artikel enthält eine Tabelle mit einem Bereich für den Serverarbeitsspeicher (RAM). Sie können sich an der geringeren Zahl für Ihren Server orientieren, müssen jedoch davon ausgehen, dass die anfängliche Synchronisierung wesentlich mehr Zeit in Anspruch nehmen kann.

## <a name="phase-3-deploy-the-azure-file-sync-cloud-resource"></a>Phase 3: Bereitstellen der Cloudressource für die Azure-Dateisynchronisierung

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

## <a name="phase-4-deploy-azure-storage-resources"></a>Phase 4: Bereitstellen von Azure Storage-Ressourcen

Sehen Sie sich in dieser Phase die Zuordnungstabelle aus Phase 1 an, und verwenden Sie diese, um die richtige Anzahl von Azure-Speicherkonten und Dateifreigaben darin bereitzustellen.

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

## <a name="phase-5-deploy-the-azure-file-sync-agent"></a>Phase 5: Bereitstellen des Azure-Dateisynchronisierungs-Agents

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

## <a name="phase-6-configure-azure-file-sync-on-the-windows-server"></a>Phase 6: Konfigurieren der Azure-Dateisynchronisierung auf dem Windows-Server

Der registrierte lokale Windows-Server muss für diesen Prozess bereit und mit dem Internet verbunden sein.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!IMPORTANT]
> Cloudtiering ist das AFS-Feature, das es dem lokalen Server ermöglicht, weniger Speicherkapazität als in der Cloud zu haben, aber trotzdem über den vollständigen Namespace zu verfügen. Lokal interessante Daten werden zudem für eine schnelle Zugriffsleistung lokal zwischengespeichert. Cloudtiering ist ein optionales Feature pro „Serverendpunkt“ der Azure-Dateisynchronisierung.

> [!WARNING]
> Wenn Sie auf Ihren Windows Server-Volumes weniger Speicher bereitgestellt haben, als ihre Daten auf der NAS-Appliance verwenden, ist Cloudtiering obligatorisch. Wenn Sie Cloudtiering nicht aktivieren, gibt der Server nicht genügend Speicherplatz zum Speichern aller Dateien frei. Legen Sie Ihre Tieringrichtlinie vorübergehend für die Migration auf 99 % freien Speicherplatz fest. Achten Sie darauf, nach Abschluss der Migration zu Ihren Cloudtiering-Einstellungen zurückkehren und sie auf eine langfristiger nützliche Ebene festzulegen.

Wiederholen Sie die Schritte zum Erstellen einer Synchronisierungsgruppe und zum Hinzufügen des jeweiligen Serverordners als Serverendpunkt für alle Azure-Dateifreigaben/Serverspeicherorte, die für die Synchronisierung konfiguriert werden müssen.

Nach der Erstellung aller Serverendpunkte funktioniert die Synchronisierung. Sie können eine Testdatei erstellen und beobachten, dass sie an Ihrem Serverstandort mit der verbundenen Azure-Dateifreigabe synchronisiert wird (wie vom Cloudendpunkt in der Synchronisierungsgruppe beschrieben).

Beide Speicherorte – die Serverordner und die Azure-Dateifreigaben – sind ansonsten leer und erwarten an einem der beiden Speicherorte Daten. Im nächsten Schritt beginnen Sie mit dem Kopieren von Dateien auf den Windows-Server, damit die Azure-Dateisynchronisierung sie in die Cloud verschieben kann. Für den Fall, dass Sie Cloudtiering aktiviert haben, beginnt der Server dann mit der Aufteilung der Dateien, wenn die Kapazität auf den lokalen Volumes ausgeschöpft ist.

## <a name="phase-7-robocopy"></a>Phase 7: RoboCopy

Der grundlegende Migrationsansatz besteht darin, mit RoboCopy eine Kopie Ihrer NAS-Appliance auf Ihrem Windows-Server anzulegen und diese mit der Azure-Dateisynchronisierung auf Azure-Dateifreigaben zu synchronisieren.

Erstellen Sie die erste lokale Kopie in Ihrem Windows Server-Zielordner:

* Identifizieren Sie den ersten Speicherort auf Ihrer NAS-Appliance.
* Identifizieren Sie den entsprechenden Ordner auf dem Windows-Server, auf dem die Azure-Dateisynchronisierung bereits konfiguriert wurde.
* Starten des Kopiervorgangs mit RoboCopy

Mit dem folgenden RoboCopy-Befehl werden Dateien aus Ihrem NAS-Speicher in den Windows Server-Zielordner kopiert. Der Windows-Server synchronisiert diesen Ordner mit den Azure-Dateifreigaben. 

Wenn Sie auf Ihrer Windows Server-Instanz weniger Speicher bereitgestellt haben, als ihre Daten auf der NAS-Appliance verwenden, haben Sie Cloudtiering konfiguriert. Wenn das lokale Windows Server-Volume voll ist, beginnt das [Cloudtiering](storage-sync-cloud-tiering-overview.md) von Dateien, die bereits erfolgreich synchronisiert wurden. Durch das Cloudtiering wird ausreichend Speicherplatz generiert, um mit dem Kopieren von der NAS-Appliance fortzufahren. Einmal pro Stunde wird überprüft, was bereits im Cloudtiering synchronisiert wurde, und Speicherplatz freigegeben, um auf dem Volume einen freien Speicherplatz von 99 % zu erreichen.
Es ist möglich, dass RoboCopy Dateien schneller verschiebt, als Sie lokal mit der Cloud und der Ebene synchronisieren können. Dann wird der lokale Speicherplatz auf dem Datenträger knapp. RoboCopy schlägt fehl. Es wird empfohlen, dass Sie die Freigaben in einer Sequenz durcharbeiten, die dies verhindert. Beispielsweise können Sie RoboCopy-Aufträge versetzt anstatt gleichzeitig für alle Freigaben starten oder nur Freigaben verschieben, die auf den aktuellen Umfang des freien Speicherplatzes auf dem Windows-Server zugeschnitten sind, um nur einige Möglichkeiten zu nennen.

[!INCLUDE [storage-files-migration-robocopy](../../../includes/storage-files-migration-robocopy.md)]

## <a name="phase-8-user-cut-over"></a>Phase 8: Benutzerübernahme

Wenn Sie den RoboCopy-Befehl zum ersten Mal ausführen, greifen Ihre Benutzer und Anwendungen weiterhin auf Dateien auf dem NAS zu und ändern sie möglicherweise. Es kann vorkommen, dass RoboCopy ein Verzeichnis verarbeitet und mit dem nächsten fortfährt und dann ein Benutzer am Quellspeicherort (NAS) eine Datei hinzufügt, ändert oder löscht. Diese wird dann während dieser aktuellen RoboCopy-Ausführung nicht verarbeitet. Dies ist das erwartete Verhalten.

Der erste Schritt besteht darin, den Großteil der Daten auf Ihren Windows-Server und dann über die Azure-Dateisynchronisierung in die Cloud zu verschieben. Der erste Kopiervorgang kann einige Zeit in Anspruch nehmen, die von folgenden Faktoren abhängig ist:

* Downloadbandbreite
* Uploadbandbreite
* Geschwindigkeit des lokalen Netzwerks und Anzahl der optimalen Übereinstimmungen von RoboCopy-Threads
* Anzahl von Elementen (Dateien und Ordner), die von RoboCopy und der Azure-Dateisynchronisierung verarbeitet werden müssen

Nachdem die erste Ausführung beendet wurde, führen Sie den Befehl erneut aus.

Beim zweiten Mal wird der Vorgang schneller abgeschlossen, da nur Änderungen, die seit der letzten Ausführung aufgetreten sind, übertragen werden müssen. Während dieser zweiten Ausführung können sich auch neue Änderungen ansammeln.

Wiederholen Sie diesen Vorgang, bis Sie der Auffassung sind, dass die bis zum Abschluss eines RoboCopy-Vorgangs für einen bestimmten Speicherort benötigte Zeit ein akzeptables Ausfallzeitfenster darstellt.

Wenn die Ausfallzeit für Sie akzeptabel ist, müssen Sie den Benutzerzugriff auf Ihre NAS-basierten Freigaben aufheben. Dazu können Sie jeden beliebigen Schritt ausführen, mit dem Benutzer daran gehindert werden, die Datei- und Ordnerstruktur sowie den Inhalt zu ändern. Ein Beispiel hierfür: Ihr DFS-Namespace verweist auf einen nicht vorhandenen Speicherort, oder Sie ändern die Stamm-ACLs auf der Freigabe.

Führen Sie einen letzten RoboCopy-Durchgang aus. Dadurch werden alle Änderungen übernommen, die möglicherweise ausgelassen wurden.
Wie lange dieser letzte Schritt dauert, hängt von der Geschwindigkeit des RoboCopy-Scans ab. Sie können die Zeit (gleich der Ausfallzeit) schätzen, indem Sie messen, wie lange die vorherige Ausführung gedauert hat.

Erstellen Sie eine Freigabe für den Windows Server-Ordner, und passen Sie Ihre DFS-N-Bereitstellung ggf. so an, dass sie auf diese zeigt. Stellen Sie sicher, dass Sie die gleichen Berechtigungen auf Freigabeebene wie auf Ihrer NAS-SMB-Freigabe festlegen. Wenn Sie einen NAS hatten, der in eine Domäne eingebunden war, stimmen die Benutzer-SIDs automatisch überein, da die Benutzer in Active Directory vorhanden sind und RoboCopy Dateien und Metadaten in voller Genauigkeit kopiert. Wenn Sie lokale Benutzer in Ihrem NAS verwendet haben, müssen Sie diese Benutzer als lokale Windows Server-Benutzer erstellen und die vorhandenen SIDs, die RoboCopy auf Ihre Windows Server-Instanz verschoben hat, den SIDs der neuen lokalen Benutzer von Windows Server zuordnen.

Sie haben die Migration einer Freigabe/Gruppe von Freigaben zu einem gemeinsamen Stamm oder Volume abgeschlossen. (Abhängig von der Zuordnung in Phase 1)

Sie können einige dieser Kopiervorgänge parallel ausführen. Es wird empfohlen, jeweils eine Azure-Dateifreigabe auf einmal zu verarbeiten.

> [!WARNING]
> Nachdem Sie alle Daten aus Ihrem NAS auf den Windows-Server verschoben haben und die Migration abgeschlossen ist, gehen Sie wie folgt vor: Kehren Sie zu ***allen*** Synchronisierungsgruppen im Azure-Portal zurück, und passen Sie den Wert für den freien Speicherplatz auf dem Cloudtieringvolume auf einen Wert an, der für die Cachenutzung etwas besser geeignet ist, z. B. „20 %“. 

Die Richtlinie für den freien Speicherplatz für das Cloudtiering wirkt sich auf eine Volumeebene aus, von der aus potenziell mehrere Serverendpunkte synchronisiert werden. Wenn Sie vergessen, den freien Speicherplatz auf einem Serverendpunkt anzupassen, wird für die Synchronisierung weiterhin die restriktivste Regel angewandt, und es wird versucht, 99 % freien Speicherplatz beizubehalten. Der lokale Cache funktioniert in diesem Fall nicht wie erwartet. Dies ist nur dann erstrebenswert, wenn Sie lediglich den Namespace für ein Volume erhalten möchten, das ausschließlich selten genutzte Archivdaten enthält, und Sie den restlichen Speicherplatz für ein anderes Szenario reservieren.

## <a name="troubleshoot"></a>Problembehandlung

Das häufigste Problem, auf das Sie stoßen können, besteht darin, dass der RoboCopy-Befehl mit dem Fehler *Volume voll* auf Windows Server-Seite beendet wird. Das Cloudtiering wird einmal stündlich eingesetzt, um Inhalte vom lokalen Windows Server-Datenträger abzurufen, die bereits synchronisiert wurden. Das Ziel besteht darin, den freien Speicherplatz von 99 % auf dem Volume zu erreichen.

Warten Sie, bis durch den Synchronisierungsvorgang und das Cloudtiering Speicherplatz freigegeben wurde. Sie können dies im Datei-Explorer auf Ihrem Windows-Server beobachten.

Wenn Ihr Windows-Server ausreichende Kapazität aufweist, wird das Problem durch erneutes Ausführen des Befehls behoben. Falls Sie auf diese Situation stoßen, treten keine Schäden auf, und Sie können einfach fortfahren. Der zusätzliche Aufwand durch das erneute Ausführen des Befehls ist die einzige Folge.

Unter dem Link im folgenden Abschnitt finden Sie Informationen zur Problembehandlung der Azure-Dateisynchronisierung.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich weiter mit Azure-Dateifreigaben und der Azure-Dateisynchronisierung vertraut. In den folgenden Artikeln werden erweiterte Optionen, bewährte Methoden und auch Ansätze zur Problembehandlung erläutert. Diese Artikel sind mit der entsprechenden [Dokumentation zur Azure-Dateifreigabe](storage-files-introduction.md) verlinkt.

* [AFS-Übersicht](./storage-sync-files-planning.md)
* [AFS-Bereitstellungshandbuch](./storage-how-to-create-file-share.md)
* [AFS-Problembehandlung](storage-sync-files-troubleshoot.md)