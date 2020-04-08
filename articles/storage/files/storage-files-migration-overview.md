---
title: Migrieren zu Azure-Dateifreigaben
description: Hier erfahren Sie mehr über Migrationen zu Azure-Dateifreigaben und finden entsprechende Migrationsleitfäden.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 3/18/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 903ce52120fce7c23c6a3754498b81fc6fc2430f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247315"
---
# <a name="migrate-to-azure-file-shares"></a>Migrieren zu Azure-Dateifreigaben

In diesem Artikel werden die grundlegenden Aspekte einer Migration zu Azure-Dateifreigaben behandelt.

Neben den Grundlagen der Migration enthält dieser Artikel eine Liste vorhandener, individualisierter Migrationsleitfäden. Diese Migrationsleitfäden unterstützen Sie beim Verschieben Ihrer Dateien in Azure-Dateifreigaben und sind nach dem derzeitigen Speicherort Ihrer Daten und dem Bereitstellungsmodell (nur Cloud oder Hybrid), zu dem Sie wechseln möchten, organisiert.

## <a name="migration-basics"></a>Grundlagen der Migration

In Azure stehen mehrere verschiedene Typen von Cloudspeicher zur Verfügung. Ein grundlegender Aspekt der Migration von Dateien zu Azure ist das Ermitteln der richtigen Azure-Speicheroption für Ihre Daten.

Azure-Dateifreigaben eignen sich hervorragend für allgemeine Dateidaten. Wirklich für alles, wofür Sie eine lokale SMB- oder NFS-Freigabe verwenden. Mit der [Azure-Dateisynchronisierung](storage-sync-files-planning.md) können Sie den Inhalt mehrerer Azure-Dateifreigaben optional auf mehreren Windows-Servern lokal zwischenspeichern.

Wenn Sie eine Anwendung haben, die derzeit auf einem lokalen Server ausgeführt wird, kann das Speichern von Dateien in Azure-Dateifreigaben je nach Anwendung für Sie geeignet sein. Sie können die Anwendung in Azure ausführen und Azure-Dateifreigaben als freigegebenen Speicher verwenden. Sie können für dieses Szenario auch [Azure-Datenträger](../../virtual-machines/windows/managed-disks-overview.md) in Erwägung ziehen. Für cloudbasierte Anwendungen, die nicht vom Zugriff auf Ihre Daten über SMB oder lokale Computer oder dem gemeinsamen Zugriff abhängen, ist ein Objektspeicher (z. B. [Azure-Blobs](../blobs/storage-blobs-overview.md)) häufig die beste Wahl.

Der Schlüssel bei jeder Migration ist die Erfassung aller anwendbaren Dateigenauigkeit bei der Migration Ihrer Dateien vom aktuellen Speicherort zu Azure. Eine Hilfe bei der Auswahl des richtigen Azure-Speichers ist auch, welcher Genauigkeitsgrad von der Azure-Speicheroption unterstützt wird und für Ihr Szenario erforderlich ist. Allgemeine Dateidaten hängen gewöhnlich von Dateimetadaten ab. Bei Anwendungsdaten ist das möglicherweise nicht der Fall. Es gibt zwei grundlegende Komponenten einer Datei:

- **Datenstrom**: Der Datenstrom einer Datei speichert den Dateiinhalt.
- **Dateimetadaten**: Die Dateimetadaten weisen mehrere Unterkomponenten auf:
   * Dateiattribute: Zum Beispiel „schreibgeschützt“.
   * Dateiberechtigungen: Werden als *NTFS-Berechtigungen* oder *Datei- und Ordner-ACLs* bezeichnet.
   * Zeitstempel: Insbesondere die Zeitstempel der *Erstellung* und *letzten Änderung*.
   * Alternativer Datenstrom: Ein Speicherplatz für größere Mengen nicht standardmäßiger Eigenschaften.

Die Dateigenauigkeit bei einer Migration kann daher als die Fähigkeit definiert werden, alle anwendbaren Dateiinformationen in der Quelle zu speichern, diese mit dem Migrationstool zu übertragen und sie im Zielspeicher der Migration zu speichern.

Um sicherzustellen, dass die Migration möglichst reibungslos verläuft, ermitteln Sie [das optimale Kopiertool für Ihre Anforderungen](#migration-toolbox) und passen ein Speicherziel an Ihre Quelle an.

Unter Berücksichtigung der vorherigen Informationen wird deutlich, welcher Zielspeicher für allgemeine Dateien in Azure zu verwenden ist: [Azure-Dateifreigaben](storage-files-introduction.md). Im Vergleich zur Objektspeicherung in Azure-Blobs können Dateimetadaten für Dateien in einer Azure-Dateifreigabe nativ gespeichert werden.

Azure-Dateifreigaben bewahren außerdem die Datei- und Ordnerhierarchie. Darüber hinaus gilt:
* NTFS-Berechtigungen können für Dateien und Ordnern gespeichert werden, da sie lokal vorhanden sind.
* AD-Benutzer (oder Azure AD DS-Benutzer) können nativ auf eine Azure-Dateifreigabe zugreifen. 
    Sie verwenden ihre aktuelle Identität und erhalten Zugriff auf Grundlage von Freigabeberechtigungen sowie Datei- und Ordner-ACLs. Ein Verhalten, das demjenigen beim Herstellen einer Verbindung mit einer lokalen Dateifreigabe durch die Benutzer ähnelt.
*  Der alternative Datenstrom ist der primäre Aspekt der Dateigenauigkeit, der derzeit nicht für eine Datei in einer Azure-Dateifreigabe gespeichert werden kann.
   Bei Einbeziehung der Azure-Dateisynchronisierung bleibt er lokal erhalten.

* [Weitere Informationen zur AD-Authentifizierung für Azure-Dateifreigaben](storage-files-identity-auth-active-directory-enable.md)
* [Weitere Informationen zur Azure Active Directory Domain Services-Authentifizierung (Azure AD DS) für Azure-Dateifreigaben](storage-files-identity-auth-active-directory-domain-service-enable.md)

## <a name="migration-guides"></a>Migrationsleitfäden

In der folgenden Tabelle sind detaillierte Migrationsleitfäden aufgelistet.

Hinweis zur Navigation:
1. Suchen Sie die Zeile für das Quellsystem, in dem Ihre Dateien aktuell gespeichert sind.
2. Entscheiden Sie, ob Sie eine Hybridbereitstellung wünschen, bei der Sie die Azure-Dateisynchronisierung zum Zwischenspeichern der Inhalte einer oder mehrerer lokaler Azure-Dateifreigaben verwenden, oder ob Sie Azure-Dateifreigaben direkt in der Cloud verwenden möchten. Wählen Sie die Spalte für das Ziel aus, das Ihrer Auswahl entspricht.
3. In der Tabellenzelle am Schnittpunkt von Quelle und Ziel sind verfügbare Migrationsszenarios aufgelistet. Wählen Sie eines davon als direkten Link zum detaillierten Migrationsleitfaden aus.

Für ein Szenario ohne Link ist derzeit noch kein Migrationsleitfaden veröffentlicht. Prüfen Sie diese Tabelle gelegentlich auf Aktualisierungen. Neue Leitfäden werden veröffentlicht, sobald sie verfügbar sind.

| Quelle | Ziel: </br>Hybridbereitstellung | Ziel: </br>Reine Cloudbereitstellung |
|:---|:--|:--|
| | Toolkombination:| Toolkombination: |
| Windows Server 2012 R2 und höher | <ul><li>[Azure-Dateisynchronisierung ](storage-sync-files-deployment-guide.md)</li><li>[Azure-Dateisynchronisierung + DataBox](storage-sync-offline-data-transfer.md)</li><li>Speichermigrationsdienst + Azure-Dateisynchronisierung</li></ul> | <ul><li>Azure-Dateisynchronisierung</li><li>Azure-Dateisynchronisierung + DataBox</li><li>Speichermigrationsdienst + Azure-Dateisynchronisierung</li><li>RoboCopy</li></ul> |
| Windows Server 2012 und niedriger | <ul><li>Azure-Dateisynchronisierung + DataBox</li><li>Speichermigrationsdienst + Azure-Dateisynchronisierung</li></ul> | <ul><li>Speichermigrationsdienst + Azure-Dateisynchronisierung</li><li>RoboCopy</li></ul> |
| Network Attached Storage (NAS) | <ul><li>[Azure-Dateisynchronisierung + RoboCopy](storage-files-migration-nas-hybrid.md)</li></ul> | <ul><li>RoboCopy</li></ul> |
| Linux/Samba | <ul><li>[RoboCopy + Azure-Dateisynchronisierung](storage-files-migration-linux-hybrid.md)</li></ul> | <ul><li>RoboCopy</li></ul> |
| StorSimple 8100/8600 | <ul><li>[Azure-Dateisynchronisierung + virtuelle 8020-Appliance](storage-files-migration-storsimple-8000.md)</li></ul> | |
| StorSimple 1200 | <ul><li>[Azure-Dateisynchronisierung ](storage-files-migration-storsimple-1200.md)</li></ul> | |
| | | |

## <a name="migration-toolbox"></a>Toolbox für die Migration

### <a name="file-copy-tools"></a>Tools zum Kopieren von Dateien

Es sind mehrere Microsoft- und Nicht-Microsoft-Tools zum Kopieren von Dateien verfügbar. Um das richtige Tool für Ihr Migrationsszenario auszuwählen, müssen Sie drei grundlegende Fragen stellen:

* Unterstützt das Kopiertool den Quell- und Zielspeicherort für eine bestimmte Dateikopie? 
    * Unterstützt es Ihren Netzwerkpfad und/oder die verfügbaren Protokolle (z. B. REST/SMB/NFS) zu und von den Quell- und Zielspeicherorten?
* Bewahrt das Kopiertool die erforderliche Dateigenauigkeit, die vom Quell-/Zielspeicherort unterstützt wird? In einigen Fällen unterstützt der Zielspeicher nicht dieselbe Genauigkeit wie die Quelle. Sie haben bereits entschieden, dass der Zielspeicher für Ihre Anforderungen ausreichend ist. Daher muss das Kopiertool nur den Dateigenauigkeitsfunktionen des Ziels entsprechen.
* Weist das Kopiertool Funktionen auf, durch die es in meine Migrationsstrategie passt? 
    * Beachten Sie beispielsweise, ob Optionen zum Minimieren der Ausfallzeit enthalten sind. Eine gute Frage ist: Kann ich diese Kopie mehrmals für denselben Speicherort ausführen, auf den von Benutzern aktiv zugegriffen wird? Kann ich in diesem Fall die Ausfallzeit wesentlich verringern. Vergleichen Sie dies mit einer Situation, in der Sie die Kopie nur starten können, wenn sich die Quelle nicht mehr ändert, um eine vollständige Kopie zu gewährleisten.

In der folgenden Tabelle sind Microsoft-Tools und deren aktuelle Eignung für Azure-Dateifreigaben klassifiziert:

| Empfohlen | Tool | Unterstützt Azure-Dateifreigaben | Bewahrt Dateigenauigkeit |
| :-: | :-- | :---- | :---- |
|![Ja, empfohlen.](media/storage-files-migration-overview/circle-green-checkmark.png)| RoboCopy | Unterstützt. Azure-Dateifreigaben können als Netzwerklaufwerke eingebunden werden. | Vollständige Genauigkeit* |
|![Ja, empfohlen.](media/storage-files-migration-overview/circle-green-checkmark.png)| Azure-Dateisynchronisierung | Nativ in Azure-Dateifreigaben integriert. | Vollständige Genauigkeit* |
|![Ja, empfohlen.](media/storage-files-migration-overview/circle-green-checkmark.png)| Speichermigrationsdienst (SMS) | Indirekt unterstützt. Azure-Dateifreigaben können als Netzwerklaufwerke auf einem SMS-Zielserver eingebunden werden. | Vollständige Genauigkeit* |
|![Nicht vollständig empfohlen.](media/storage-files-migration-overview/triangle-yellow-exclamation.png)| Azure DataBox | Unterstützt. | Kopiert keine Metadaten. [Kann in Kombination mit der Azure-Dateisynchronisierung verwendet werden](storage-sync-offline-data-transfer.md). |
|![Nicht empfohlen.](media/storage-files-migration-overview/circle-red-x.png)| AzCopy | Unterstützt. | Kopiert keine Metadaten. |
|![Nicht empfohlen.](media/storage-files-migration-overview/circle-red-x.png)| Azure Storage-Explorer | Unterstützt. | Kopiert keine Metadaten. |
|![Nicht empfohlen.](media/storage-files-migration-overview/circle-red-x.png)| Azure Data Factory | Unterstützt. | Kopiert keine Metadaten. |
|||||

*\* Vollständige Genauigkeit: Entspricht oder übertrifft die Azure-Dateifreigabefunktionen.*

### <a name="migration-helper-tools"></a>Hilfstools für die Migration

In diesem Abschnitt sind Tools aufgelistet, die Ihnen beim Planen und Ausführen von Migrationen helfen.

* **RoboCopy (Microsoft Corporation)**

    Eines der am besten geeigneten Kopiertools für Dateimigrationen ist Bestandteil von Microsoft Windows. Aufgrund der vielen Optionen in diesem Tool ist die [Hauptdokumentation zu RoboCopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) eine hilfreiche Quelle.

* **TreeSize (JAM Software GmbH)**

    Die Azure-Dateisynchronisierung wird in erster Linie entsprechend der Anzahl der Elemente (Dateien und Ordner) und weniger entsprechend der TiB-Gesamtmenge skaliert. Mit dem Tool kann die Anzahl der Dateien und Ordner auf Ihren Windows Server-Volumes bestimmt werden. Darüber hinaus kann damit eine Perspektive vor einer [Bereitstellung der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md) erstellt werden, aber auch danach, wenn das Cloudtiering eingebunden ist und Sie nicht nur die Anzahl der Elemente sehen möchten, sondern auch in welchen Verzeichnissen der Servercache am meisten genutzt wird.
    Dieses Tool (Version 4.4.1 getestet) ist mit mehrstufigen Dateien kompatibel. Während des normalen Betriebs wird kein Rückruf von mehrstufigen Dateien ausgelöst.


## <a name="next-steps"></a>Nächste Schritte

1. Erstellen Sie einen Plan für die beabsichtigte Bereitstellung von Azure-Dateifreigaben (nur Cloud oder Hybrid).
2. Suchen Sie in der Liste der verfügbaren Migrationsleitfäden nach der detaillierten Anleitung, die Ihrer Quelle und Bereitstellung von Azure-Dateifreigaben entspricht.

Weitere Informationen zu den in diesem Artikel erwähnten Azure Files-Technologien finden Sie in den folgenden Artikeln:

* [Übersicht über Azure-Dateifreigaben](storage-files-introduction.md)
* [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
* [Azure-Dateisynchronisierung: Cloudtiering](storage-sync-cloud-tiering.md)
