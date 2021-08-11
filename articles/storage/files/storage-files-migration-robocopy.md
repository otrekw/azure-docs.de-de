---
title: Migrieren zu Azure-Dateifreigaben mit Robocopy
description: Erfahren Sie, wie Sie mit Robocopy Dateien von mehreren Speicherorten zu Azure-Dateifreigaben migrieren.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 04/12/2021
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 53c0ad42e51e8ffc562827e9a67e01b132dafd89
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108776718"
---
# <a name="use-robocopy-to-migrate-to-azure-file-shares"></a>Verwenden von Robocopy zum Migrieren zu Azure-Dateifreigaben

In diesem Migrationsartikel wird die Verwendung von Robocopy zum Verschieben oder Migrieren von Dateien zu einer Azure-Dateifreigabe beschrieben. Robocopy ist ein vertrauenswürdiges und bekanntes Hilfsprogramm zum Kopieren von Dateien, außerdem bietet es Funktionen, durch die es sich gut für Migrationsvorgänge eignet. Es verwendet das SMB-Protokoll, sodass es sehr vielseitig auf jede Kombination aus Quelle und Ziel anwendbar ist, die SMB unterstützt.

> [!div class="checklist"]
> * Datenquellen: Jede Quelle, die das SMB-Protokoll unterstützt, z. B. NAS (Network Attached Storage), Windows- oder Linux-Server, eine andere Azure-Dateifreigabe u. v. m.
> * Migrationsroute: vom Quellspeicherort &rArr; Windows-Computer mit Robocopy &rArr; Azure-Dateifreigabe

Es gibt viele verschiedene Migrationsrouten für unterschiedliche Quell- und Bereitstellungskombinationen. Anhand der [Tabelle der Migrationsleitfäden](storage-files-migration-overview.md#migration-guides) können Sie die Migration ermitteln, die Ihren Anforderungen am besten entspricht.

## <a name="azcopy-vs-robocopy"></a>AzCopy im Vergleich zu Robocopy
AzCopy und Robocopy sind zwei grundlegend verschiedene Tools zum Kopieren von Dateien. Robocopy verwendet eine beliebige Version des SMB-Protokolls. AzCopy ist ein cloudnatives Tool, das zum Verschieben von Daten verwendet werden kann, sofern sich das Ziel in Azure Storage befindet. AzCopy ist von einem REST-Protokoll abhängig.

Robocopy ist ein vertrauenswürdiges, Windows-basiertes Kopiertool, das seine Vorteile als natives Tool am besten ausspielen kann, wenn Dateien mit großer Präzision kopiert werden sollen. Robocopy unterstützt aufgrund seiner zahlreichen Funktionen und der Möglichkeit Dateien und Ordner mit vielen Einstellungsmöglichkeiten zu kopieren, eine Vielzahl von Migrationsszenarien. Weitere Informationen zur Wichtigkeit des Kopierens von Dateien mit maximaler Genauigkeit finden Sie im [Abschnitt zur Dateigenauigkeit im Artikel „Migrieren zu Azure-Dateifreigaben“](storage-files-migration-overview.md#migration-basics).

AzCopy hingegen wurde erst kürzlich erweitert, um ebenfalls Dateikopierfunktionen mit einer gewissen Genauigkeit zu unterstützen, und es wurden die ersten Funktionen auf dem Weg zu einem Migrationstool hinzugefügt. Es gibt jedoch weiterhin Lücken, und beim Vergleich von AzCopy-Flags mit Robocopy-Flags kann es leicht zu Verwechslungen bei der Funktionalität kommen.

Beispiel: *Robocopy /MIR* spiegelt die Quelle im Ziel, es werden also hinzugefügte, geänderte und gelöschte Dateien berücksichtigt. Ein wichtiger Unterschied zu *AzCopy -sync* ist, dass an der Quelle gelöschte Dateien nicht am Ziel entfernt werden. Dadurch kann nicht von einem vollständigen Featuresatz für differenzielles Kopieren gesprochen werden. AzCopy wird jedoch stetig weiterentwickelt. Derzeit wird AzCopy nicht als Tool für Migrationsszenarien mit Azure-Dateifreigaben als Ziel empfohlen. 

## <a name="migration-goals"></a>Migrationsziele

Ziel ist es, Daten aus vorhandenen Dateifreigabeorten in Azure zu verschieben. In Azure werden die Daten in nativen Azure-Dateifreigaben gespeichert, die Sie ohne eine Instanz von Windows Server verwenden können. Diese Migration muss so erfolgen, dass die Integrität der Produktionsdaten und die Verfügbarkeit während der Migration gewährleistet wird. Letzteres erfordert minimale Ausfallzeiten, damit sie in normalen Wartungsfenstern stattfinden kann oder diese nur geringfügig überschreitet.

## <a name="migration-overview"></a>Übersicht zur Migration

Der Migrationsprozess besteht aus mehreren Phasen. Sie müssen Azure Storage-Konten und Dateifreigaben bereitstellen. Darüber hinaus konfigurieren Sie Netzwerke, stellen eventuell einen DFS-Namespace bereit (DFS-N) oder aktualisieren Ihren vorhandenen. Wenn der tatsächliche Kopiervorgang für die Daten beginnen kann, sollten Sie wiederholte, differenzielle Robocopy-Durchläufe in Betracht ziehen, um die Downtime zu minimieren. Abschließen müssen Sie Ihre Benutzer auf die neu erstellten Azure-Dateifreigaben umstellen. In den folgenden Abschnitten werden die Phasen des Migrationsprozesses ausführlich beschrieben.

> [!TIP]
> Wenn Sie zu diesem Artikel zurückkehren, wechseln Sie anhand der Navigation auf der rechten Seite zu der Migrationsphase, in der Sie aufgehört haben.

## <a name="phase-1-identify-how-many-azure-file-shares-you-need"></a>Phase 1: Ermitteln der Anzahl der benötigten Azure-Dateifreigaben

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="phase-2-deploy-azure-storage-resources"></a>Phase 2: Bereitstellen von Azure Storage-Ressourcen

Sehen Sie sich in dieser Phase die Zuordnungstabelle aus Phase 1 an, und verwenden Sie diese, um die richtige Anzahl von Azure-Speicherkonten und Dateifreigaben darin bereitzustellen.

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

## <a name="phase-3-preparing-to-use-azure-file-shares"></a>Phase 3: Vorbereiten der Nutzung von Azure-Dateifreigaben

Anhand der Informationen dieser Phase können Sie entscheiden, wie Ihre Server und Benutzer inner- und außerhalb von Azure Ihre Azure-Dateifreigaben nutzen können. Wichtigste Entscheidungen:

- **Netzwerk:** Ermöglichen Sie für Ihre Netzwerke das Weiterleiten von SMB-Datenverkehr.
- **Authentifizierung:** Konfigurieren Sie Azure-Speicherkonten für die Kerberos-Authentifizierung. Wenn AD Connect genutzt und für Ihr Speicherkonto der Domänenbeitritt durchgeführt wird, können Ihre Apps und Benutzer ihre eigene AD-Identität für die Authentifizierung verwenden.
- **Autorisierung:** ACLs auf Freigabeebene für jede Azure-Dateifreigabe ermöglichen AD-Benutzern und -Gruppen den Zugriff auf eine Freigabe, und innerhalb einer Azure-Dateifreigabe werden dann native NTFS-ACLs genutzt. Die auf Datei- und Ordner-ACLs basierende Autorisierung funktioniert anschließend wie bei lokalen SMB-Freigaben.
- **Geschäftskontinuität:** Für die Integration von Azure-Dateifreigaben in eine vorhandene Umgebung ist es häufig erforderlich, vorhandene Freigabeadressen beizubehalten. Falls Sie nicht bereits [DFS-Namespaces](files-manage-namespaces.md) verwenden, sollten Sie dies für Ihre Umgebung erwägen. Sie haben dann die Möglichkeit, Freigabeadressen, die von Ihren Benutzern und Skripts verwendet werden, unverändert beizubehalten. DFS-N stellt einen Namespace-Routingdienst für SMB bereit, indem Clients auf Azure-Dateifreigaben umgeleitet werden.

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
* [Übersicht über DFS-Namespaces](files-manage-namespaces.md)
   :::column-end:::
:::row-end:::

### <a name="mounting-an-azure-file-share"></a>Einbinden einer Azure-Dateifreigabe

Bevor Sie Robocopy verwenden können, müssen Sie die Azure-Dateifreigabe per SMB zugänglich machen. Die einfachste Möglichkeit besteht darin, die Freigabe als lokales Netzlaufwerk für die Windows Server-Instanz einzubinden, die Sie für Robocopy verwenden möchten. 

> [!IMPORTANT]
> Bevor Sie eine Azure-Dateifreigabe erfolgreich auf einer lokalen Windows Server-Instanz einbinden können, müssen Sie Phase 3 „Vorbereiten der Nutzung von Azure-Dateifreigaben“ abgeschlossen haben.

Wenn Sie bereit sind, lesen Sie den Artikel [Verwenden einer Azure-Dateifreigabe mit Windows](storage-how-to-use-files-windows.md). Anschließend können Sie die Azure-Dateifreigabe einbinden, für die Sie Robocopy starten möchten.

## <a name="phase-4-robocopy"></a>Phase 4: Robocopy

Mit dem folgenden Robocopy-Befehl werden nur die Unterschiede (aktualisierte Dateien und Ordner) aus Ihrem Quellspeicher in Ihre Azure-Dateifreigabe kopiert.

[!INCLUDE [storage-files-migration-robocopy](../../../includes/storage-files-migration-robocopy.md)]

> [!TIP]
> Lesen Sie sich den [Abschnitt zur Problembehandlung](#troubleshoot-and-optimize) durch, wenn Robocopy Ihre Produktionsumgebung beeinträchtigt, viele Fehler meldet oder nicht so schnell wie erwartet ausgeführt wird.

## <a name="phase-5-user-cut-over"></a>Phase 5: Benutzerübernahme

Wenn Sie den Robocopy-Befehl zum ersten Mal ausführen, greifen Ihre Benutzer und Anwendungen weiterhin auf Dateien an der Quelle zu und ändern sie möglicherweise. Es kann vorkommen, dass Robocopy ein Verzeichnis verarbeitet und mit dem nächsten fortfährt und dann ein Benutzer am Quellspeicherort eine Datei hinzufügt, ändert oder löscht. Diese wird dann während der derzeitigen Robocopy-Ausführung nicht verarbeitet. Dies ist das erwartete Verhalten.

Bei der ersten Ausführung wird der Großteil der geänderten Daten auf Ihre Azure-Dateifreigabe verschoben. Dieser erste Kopiervorgang kann eine Weile dauern. Weitere Informationen dazu, wie die Robocopy-Geschwindigkeit ggf. beeinträchtigt werden kann, finden Sie im [Abschnitt zur Problembehandlung](#troubleshoot-and-optimize).

Nachdem die erste Ausführung beendet wurde, führen Sie den Befehl erneut aus.

Wenn Sie Robocopy zum zweiten Mal für dieselbe Freigabe ausführen, wird der Vorgang schneller abgeschlossen, da nur Änderungen, die seit der letzten Ausführung aufgetreten sind, übertragen werden müssen. Sie können wiederholte Aufträge für dieselbe Freigabe ausführen.

Wenn die Downtime für Sie akzeptabel ist, müssen Sie den Benutzerzugriff auf Ihre Quellfreigaben aufheben. Dazu können Sie jeden beliebigen Schritt ausführen, mit dem Benutzer daran gehindert werden, die Datei- und Ordnerstruktur sowie den Inhalt zu ändern. Ein Beispiel: Ihr DFS-Namespace verweist auf einen nicht vorhandenen Speicherort, oder Sie ändern die ACLs in jeder Freigabe.

Führen Sie einen letzten Robocopy-Durchgang aus. Dadurch werden alle Änderungen übernommen, die möglicherweise ausgelassen wurden.
Wie lange dieser letzte Schritt dauert, hängt von der Geschwindigkeit des Robocopy-Scans ab. Sie können die Zeit (gleich der Ausfallzeit) schätzen, indem Sie messen, wie lange die vorherige Ausführung gedauert hat.

In einem vorherigen Abschnitt haben Sie Ihre Benutzer für den [Zugriff auf die Freigabe mit ihrer Identität](#phase-3-preparing-to-use-azure-file-shares) konfiguriert und sollten eine Strategie für Ihre Benutzer entwickelt haben, um [eingerichtete Pfade zu Ihren neuen Azure-Dateifreigaben (DFS-N)](files-manage-namespaces.md) zu verwenden.

Sie können versuchen, einige dieser Kopien parallel zwischen verschiedenen Quell- und Zielfreigaben auszuführen. Behalten Sie dabei den Netzwerkdurchsatz und das Verhältnis zwischen Kern und Threadanzahl im Blick, um das System nicht zu überlasten.

## <a name="troubleshoot-and-optimize"></a>Problembehandlung und Optimierung

[!INCLUDE [storage-files-migration-robocopy-optimize](../../../includes/storage-files-migration-robocopy-optimize.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die weiteren Informationen zu Azure-Dateifreigaben. In den folgenden Artikeln werden erweiterte Optionen, bewährte Methoden und auch Ansätze zur Problembehandlung erläutert. Diese Artikel sind mit der entsprechenden [Dokumentation zur Azure-Dateifreigabe](storage-files-introduction.md) verlinkt.

* [Migration overview (Übersicht über die Migration)](storage-files-migration-overview.md)
* [Sicherung: Momentaufnahmen von Azure-Dateifreigaben](storage-snapshots-files.md)
* [Verwenden von DFS-Namespaces mit Azure Files](files-manage-namespaces.md)
