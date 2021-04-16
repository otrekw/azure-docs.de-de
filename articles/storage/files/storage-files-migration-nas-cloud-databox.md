---
title: Migration von lokalem NAS zu Azure-Dateifreigaben
description: Es wird beschrieben, wie Sie Dateien aus lokalem Network Attached Storage (NAS) per Azure Data Box zu Azure-Dateifreigaben migrieren.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 04/02/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: a8420d23c8bda29290722975ada2acca6733f0e7
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106491673"
---
# <a name="use-databox-to-migrate-from-network-attached-storage-nas-to-azure-file-shares"></a>Verwenden von Data Box für die Migration von Network Attached Storage (NAS) zu Azure-Dateifreigaben

Dieser Artikel zur Migration ist einer von mehreren Artikeln mit den Schlüsselwörtern „NAS“ und „Azure Data Box“. Überprüfen Sie, ob dieser Artikel für Ihr Szenario zutrifft:

> [!div class="checklist"]
> * Datenquelle: Network Attached Storage (NAS)
> * Migrationsroute: NAS &rArr; Data Box &rArr; Azure-Dateifreigabe
> * Lokales Zwischenspeichern von Dateien: Nein. Das übergeordnete Ziel besteht darin, die Azure-Dateifreigaben direkt in der Cloud zu nutzen. Es ist nicht geplant, die Azure-Dateisynchronisierung zu verwenden.

Wenn Ihr Szenario anders ist, sehen Sie sich die [Tabelle mit Migrationsleitfäden](storage-files-migration-overview.md#migration-guides) an.

In diesem Artikel wird Schritt für Schritt der gesamte Prozess für die Planung, Bereitstellung und Netzwerkkonfiguration beschrieben, der erforderlich ist, um die Migration von Ihrer NAS-Appliance zu funktionierenden Azure-Dateifreigaben durchzuführen. In dieser Anleitung wird Azure Data Box für den Massentransport von Daten (Offlinetransport) genutzt.

## <a name="migration-goals"></a>Migrationsziele

Das Ziel besteht darin, die Freigaben auf Ihrer NAS-Appliance nach Azure zu verschieben und als native Azure-Dateifreigaben bereitzustellen, die Sie ohne Windows Server nutzen können. Diese Migration muss so erfolgen, dass die Integrität der Produktionsdaten und die Verfügbarkeit während der Migration gewährleistet wird. Letzteres erfordert minimale Ausfallzeiten, damit sie in normalen Wartungsfenstern stattfinden kann oder diese nur geringfügig überschreitet.

## <a name="migration-overview"></a>Übersicht zur Migration

Der Migrationsprozess besteht aus mehreren Phasen. Sie müssen Azure-Speicherkonten und -Dateifreigaben bereitstellen, das Netzwerk konfigurieren, die Migration mit Azure Data Box durchführen, die Änderungen mit Robocopy bereitstellen und abschließend für Ihre Benutzer die Umstellung auf die neu erstellten Azure-Dateifreigaben vornehmen. In den folgenden Abschnitten werden die Phasen des Migrationsprozesses ausführlich beschrieben.

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

## <a name="phase-4-provision-a-temporary-windows-server"></a>Phase 4: Bereitstellen einer temporären Windows Server-Instanz

Während Sie auf das Eintreffen Ihrer Azure Data Box warten (eine oder auch mehrere), können Sie bereits die Windows Server-Instanzen bereitstellen, die Sie zum Ausführen von Robocopy-Aufträgen benötigen. 

- Die erste Aufgabe für diese Server ist das Kopieren von Dateien auf die Data Box.
- Die zweite Aufgabe dieser Server ist die Bereitstellung der Änderungen, die auf der NAS-Appliance während des Data Box-Transports vorgenommen wurden. Mit diesem Ansatz wird die Downtime aufseiten der Quelle auf ein Mindestmaß reduziert.

Die Geschwindigkeit, mit der Ihre Robocopy-Aufträge ausgeführt werden, hängt hauptsächlich von den folgenden Faktoren ab:

* IOPS im Quell- und Zielspeicher
* Verfügbare Bandbreite zwischen Quelle und Ziel </br> Ausführlichere Informationen finden Sie im Abschnitt zur Problembehandlung unter [IOPS- und Bandbreitenaspekte](#iops-and-bandwidth-considerations).
* Schnelle Verarbeitung von Dateien und Ordnern in einem Namespace </br> Ausführlichere Informationen finden Sie im Abschnitt zur Problembehandlung unter [Verarbeitungsgeschwindigkeit](#processing-speed).
* Anzahl von Änderungen zwischen Robocopy-Ausführungen </br> Ausführlichere Informationen finden Sie im Abschnitt zur Problembehandlung unter [Vermeiden von unnötigem Arbeitsaufwand](#avoid-unnecessary-work).

Es ist wichtig, die obigen Details zu berücksichtigen, wenn Sie die Entscheidungen in Bezug auf den RAM und die Threadanzahl für Ihre temporären Windows Server-Instanzen treffen.

## <a name="phase-5-preparing-to-use-azure-file-shares"></a>Phase 5: Vorbereiten der Nutzung von Azure-Dateifreigaben

Um Zeit zu sparen, sollten Sie mit der Arbeit an dieser Phase bereits beginnen, während Sie auf das Eintreffen Ihrer Data Box warten. Anhand der Informationen dieser Phase können Sie entscheiden, wie Ihre Server und Benutzer inner- und außerhalb von Azure Ihre Azure-Dateifreigaben nutzen können. Wichtigste Entscheidungen:

- **Netzwerk:** Ermöglichen Sie für Ihre Netzwerke das Weiterleiten von SMB-Datenverkehr.
- **Authentifizierung:** Konfigurieren Sie Azure-Speicherkonten für die Kerberos-Authentifizierung. Wenn AD Connect genutzt und für Ihr Speicherkonto der Domänenbeitritt durchgeführt wird, können Ihre Apps und Benutzer ihre eigene AD-Identität für die Authentifizierung verwenden.
- **Autorisierung:** ACLs auf Freigabeebene für jede Azure-Dateifreigabe ermöglichen AD-Benutzern und -Gruppen den Zugriff auf eine Freigabe, und innerhalb einer Azure-Dateifreigabe werden dann native NTFS-ACLs genutzt. Die auf Datei- und Ordner-ACLs basierende Autorisierung funktioniert anschließend wie bei lokalen SMB-Freigaben.
- **Geschäftskontinuität:** Für die Integration von Azure-Dateifreigaben in eine vorhandene Umgebung ist es häufig erforderlich, vorhandene Freigabeadressen beizubehalten. Falls Sie DFS-Namespaces nicht bereits verwenden, sollten Sie dies für Ihre Umgebung erwägen. Sie haben dann die Möglichkeit, Freigabeadressen, die von Ihren Benutzern und Skripts verwendet werden, unverändert beizubehalten. Sie nutzen hierbei DFS-N als Namespace-Routingdienst für SMB, indem Sie DFS-Namespaceziele nach deren Migration auf Azure-Dateifreigaben umleiten.

:::row:::
    :::column:::
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/jd49W33DxkQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    :::column-end:::
    :::column:::
        Dieses Video ist eine Anleitung und Demo dazu, wie Azure-Dateifreigaben in fünf einfachen Schritte für Information-Worker und Apps auf sichere Weise direkt verfügbar gemacht werden können.</br>
        In diesem Video wird auf dedizierte Dokumentation für einige Themen verwiesen:

* [Übersicht über Identitäten](storage-files-active-directory-overview.md)
* [Übersicht – lokale Active Directory Domain Services-Authentifizierung über SMB für Azure-Dateifreigaben](storage-files-identity-auth-active-directory-enable.md)
* [Azure Files – Überlegungen zum Netzwerkbetrieb](storage-files-networking-overview.md)
* [Konfigurieren von Azure Files-Netzwerkendpunkten](storage-files-networking-endpoints.md)
* [Konfigurieren eines Site-to-Site-VPN zur Verwendung mit Azure Files](storage-files-configure-s2s-vpn.md)
* [Konfigurieren eines P2S-VPN (Point-to-Site) unter Windows zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-windows.md)
* [Konfigurieren eines P2S-VPN (Point-to-Site) unter Linux zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-linux.md)
* [Konfigurieren der DNS-Weiterleitung für Azure Files](storage-files-networking-dns.md)
* [Übersicht über DFS-Namespaces](/windows-server/storage/dfs-namespaces/dfs-overview)
   :::column-end:::
:::row-end:::

## <a name="phase-6-copy-files-onto-your-databox"></a>Phase 6: Kopieren von Dateien auf Ihre Data Box

Nachdem Ihre Data Box eingetroffen ist, müssen Sie diese mit „Sichtverbindung“ zu Ihrer NAS-Appliance einrichten. Befolgen Sie die Installationsdokumentation für den DataBox-Typ, den Sie bestellt haben.

* [Einrichten von Data Box](../../databox/data-box-quickstart-portal.md)
* [Einrichten von Data Box Disk](../../databox/data-box-disk-quickstart-portal.md)
* [Einrichten von Data Box Heavy](../../databox/data-box-heavy-quickstart-portal.md)

Abhängig vom DataBox-Typ sind möglicherweise DataBox-Kopiertools verfügbar. An diesem Punkt werden sie nicht für Migrationen zu Azure-Dateifreigaben empfohlen, da sie Ihre Dateien nicht mit voller Genauigkeit in die DataBox kopieren. Verwenden Sie stattdessen Robocopy.

Wenn Ihre DataBox eintrifft, weist sie für jedes Speicherkonto, das Sie zum Zeitpunkt der Bestellung angegeben haben, vorab bereitgestellte SMB-Freigaben auf.

* Wenn Ihre Dateien in einer Azure-Premium-Dateifreigabe gespeichert werden, steht eine SMB-Freigabe pro Premium-File Storage-Speicherkonto zur Verfügung.
* Wenn Ihre Dateien in einem Standardspeicherkonto gespeichert werden, stehen drei SMB-Freigaben pro Standardspeicherkonto (GPv1 und GPv2) bereit. Nur die Dateifreigaben, die mit `_AzFiles` enden, sind für Ihre Migration relevant. Ignorieren Sie alle Block- und Seitenblobfreigaben.

Führen Sie die Schritte in der Azure DataBox-Dokumentation aus:

1. [Herstellen einer Verbindung mit der Data Box](../../databox/data-box-deploy-copy-data.md)
1. Kopieren von Daten auf die Data Box
1. [Vorbereiten Ihrer DataBox für den Versand zu Azure](../../databox/data-box-deploy-picked-up.md)

In der verknüpften DataBox-Dokumentation wird ein Robocopy-Befehl angegeben. Der Befehl eignet sich jedoch nicht für die Beibehaltung der vollständigen Datei- und Ordnergenauigkeit. Verwenden Sie stattdessen diesen Befehl:

```console
Robocopy /MT:32 /NP /NFL /NDL /B /MIR /IT /COPY:DATSO /DCOPY:DAT /UNILOG:<FilePathAndName> <SourcePath> <Dest.Path> 
```
* Weitere Informationen zu den Details der einzelnen Robocopy-Flags finden Sie in der Tabelle, die unten im [Robocopy-Abschnitt](#robocopy) enthalten ist.
* Weitere Informationen zum richtigen Dimensionieren der Threadanzahl `/MT:n`, Optimieren der Robocopy-Geschwindigkeit und Einfügen von Robocopy als problemlose Komponente in Ihrem Rechenzentrum finden Sie im [Abschnitt zur Problembehandlung für Robocopy](#troubleshoot).


## <a name="phase-7-catch-up-robocopy-from-your-nas"></a>Phase 7: Bereitstellen der Änderungen in Ihrem NAS mit Robocopy

Nachdem für Ihre Data Box gemeldet wurde, dass alle Dateien und Ordner auf den geplanten Azure-Dateifreigaben platziert wurden, können Sie mit dieser Phase fortfahren.
Der Vorgang zum Bereitstellen von Änderungen mit Robocopy ist nur erforderlich, wenn sich die Daten im NAS seit dem Start des Data Box-Kopiervorgangs ggf. geändert haben. In bestimmten Szenarien, in denen Sie eine Freigabe für Archivierungszwecke verwenden, können Sie das Vornehmen von Änderungen auf der Freigabe Ihres NAS unter Umständen so lange zurückhalten, bis die Migration abgeschlossen ist. Möglicherweise können Sie Ihre geschäftlichen Anforderungen auch erfüllen, indem Sie für die NAS-Freigaben während der Migration den Schreibschutz aktivieren.

In Fällen, in denen bei Ihnen für eine Freigabe während der Migration der Schreibschutz aktiviert sein muss und nur ein kleines Downtimefenster zur Verfügung steht, muss die Ausführung dieses Robocopy-Schritts zum Bereitstellen der Änderungen vollständig abgeschlossen sein, bevor für den Benutzerzugriff das Failover direkt auf die Azure-Dateifreigabe erfolgt.

In diesem Schritt führen Sie Robocopy-Aufträge aus, um Ihre Cloudfreigaben mit den neuesten Änderungen Ihres NAS, seitdem Sie Ihre Freigaben auf die DataBox verschoben haben, zu aktualisieren.
Diese Aktualisierung mit Robocopy kann je nach Menge der Änderungen, die auf Ihren NAS-Freigaben aufgetreten sind, schnell beendet sein oder eine Weile dauern.

Erstellen Sie die erste lokale Kopie in Ihrem Windows Server-Zielordner:

1. Identifizieren Sie den ersten Speicherort auf Ihrer NAS-Appliance.
1. Ermitteln Sie die übereinstimmende Azure-Dateifreigabe.
1. Binden Sie die Azure-Dateifreigabe als lokales Netzlaufwerk auf Ihrer temporären Windows Server-Instanz ein.
1. Starten Sie den Kopiervorgang wie beschrieben mit Robocopy.

### <a name="mounting-an-azure-file-share"></a>Einbinden einer Azure-Dateifreigabe

Bevor Sie Robocopy verwenden können, müssen Sie die Azure-Dateifreigabe per SMB zugänglich machen. Die einfachste Möglichkeit besteht darin, die Freigabe als lokales Netzlaufwerk für die Windows Server-Instanz einzubinden, die Sie für Robocopy verwenden möchten. 

> [!IMPORTANT]
> Bevor Sie eine Azure-Dateifreigabe erfolgreich auf einer lokalen Windows Server-Instanz einbinden können, müssen Sie die Phase „Vorbereiten der Nutzung von Azure-Dateifreigaben“ abgeschlossen haben!

Lesen Sie sich anschließend den Artikel [Verwenden einer Azure-Dateifreigabe mit Windows](storage-how-to-use-files-windows.md) durch, und binden Sie die Azure-Dateifreigabe ein, für die Sie das Bereitstellen der NAS-Änderungen mit Robocopy starten möchten.

### <a name="robocopy"></a>Robocopy

Mit dem folgenden Robocopy-Befehl werden nur die Unterschiede (aktualisierte Dateien und Ordner) aus Ihrem NAS-Speicher auf Ihre Azure-Dateifreigabe kopiert. 

[!INCLUDE [storage-files-migration-robocopy](../../../includes/storage-files-migration-robocopy.md)]

> [!TIP]
> Lesen Sie sich den [Abschnitt zur Problembehandlung](#troubleshoot) durch, wenn Robocopy Ihre Produktionsumgebung beeinträchtigt, viele Fehler meldet oder nicht so schnell wie erwartet ausgeführt wird.

### <a name="user-cut-over"></a>Benutzerübernahme

Wenn Sie den Robocopy-Befehl zum ersten Mal ausführen, greifen Ihre Benutzer und Anwendungen weiterhin auf Dateien auf dem NAS zu und ändern sie möglicherweise. Es kann vorkommen, dass Robocopy ein Verzeichnis verarbeitet und mit dem nächsten fortfährt und dann ein Benutzer am Quellspeicherort (NAS) eine Datei hinzufügt, ändert oder löscht. Diese wird dann während dieser aktuellen Robocopy-Ausführung nicht verarbeitet. Dies ist das erwartete Verhalten.

Bei der ersten Ausführung wird der Großteil der geänderten Daten auf Ihre Azure-Dateifreigabe verschoben. Dieser erste Kopiervorgang kann eine Weile dauern. Weitere Informationen dazu, wie die Robocopy-Geschwindigkeit ggf. beeinträchtigt werden kann, finden Sie im [Abschnitt zur Problembehandlung](#troubleshoot).

Nachdem die erste Ausführung beendet wurde, führen Sie den Befehl erneut aus.

Wenn Sie Robocopy zum zweiten Mal für dieselbe Freigabe ausführen, wird der Vorgang schneller abgeschlossen, da nur Änderungen, die seit der letzten Ausführung aufgetreten sind, übertragen werden müssen. Sie können wiederholte Aufträge für dieselbe Freigabe ausführen.

Wenn die Ausfallzeit für Sie akzeptabel ist, müssen Sie den Benutzerzugriff auf Ihre NAS-basierten Freigaben aufheben. Dazu können Sie jeden beliebigen Schritt ausführen, mit dem Benutzer daran gehindert werden, die Datei- und Ordnerstruktur sowie den Inhalt zu ändern. Ein Beispiel hierfür: Ihr DFS-Namespace verweist auf einen nicht vorhandenen Speicherort, oder Sie ändern die Stamm-ACLs auf der Freigabe.

Führen Sie einen letzten Robocopy-Durchgang aus. Dadurch werden alle Änderungen übernommen, die möglicherweise ausgelassen wurden.
Wie lange dieser letzte Schritt dauert, hängt von der Geschwindigkeit des Robocopy-Scans ab. Sie können die Zeit (gleich der Ausfallzeit) schätzen, indem Sie messen, wie lange die vorherige Ausführung gedauert hat.

Erstellen Sie eine Freigabe für den Windows Server-Ordner, und passen Sie Ihre DFS-N-Bereitstellung ggf. so an, dass sie auf diese zeigt. Stellen Sie sicher, dass Sie die gleichen Berechtigungen auf Freigabeebene wie auf Ihrer NAS-SMB-Freigabe festlegen. Wenn Sie einen NAS hatten, der in eine Domäne eingebunden war, stimmen die Benutzer-SIDs automatisch überein, da die Benutzer in Active Directory vorhanden sind und Robocopy Dateien und Metadaten in voller Genauigkeit kopiert. Wenn Sie lokale Benutzer in Ihrem NAS verwendet haben, müssen Sie diese Benutzer als lokale Windows Server-Benutzer erstellen und die vorhandenen SIDs, die Robocopy auf Ihre Windows Server-Instanz verschoben hat, den SIDs der neuen lokalen Benutzer von Windows Server zuordnen.

Sie haben die Migration einer Freigabe/Gruppe von Freigaben zu einem gemeinsamen Stamm oder Volume abgeschlossen. (Abhängig von der Zuordnung in Phase 1)

Sie können einige dieser Kopiervorgänge parallel ausführen. Es wird empfohlen, jeweils eine Azure-Dateifreigabe auf einmal zu verarbeiten.

## <a name="troubleshoot"></a>Problembehandlung

[!INCLUDE [storage-files-migration-robocopy-optimize](../../../includes/storage-files-migration-robocopy-optimize.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die weiteren Informationen zu Azure-Dateifreigaben. In den folgenden Artikeln werden erweiterte Optionen, bewährte Methoden und auch Ansätze zur Problembehandlung erläutert. Diese Artikel sind mit der entsprechenden [Dokumentation zur Azure-Dateifreigabe](storage-files-introduction.md) verlinkt.

* [Migration overview (Übersicht über die Migration)](storage-files-migration-overview.md)
* [Microsoft Azure-Speicher: Überwachung, Diagnose und Problembehandlung](../common/storage-monitoring-diagnosing-troubleshooting.md)
* [Netzwerküberlegungen für den Direktzugriff](storage-files-networking-overview.md)
* [Sicherung: Momentaufnahmen von Azure-Dateifreigaben](storage-snapshots-files.md)
