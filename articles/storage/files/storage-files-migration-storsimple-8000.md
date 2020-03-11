---
title: Migration der StorSimple 8000-Serie zur Azure-Dateisynchronisierung
description: Erfahren Sie, wie Sie eine StorSimple 8100- oder 8600-Appliance zur Azure-Dateisynchronisierung migrieren.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 03/02/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 78100a5dd38b211f6b0241d5a0bac10cf86b09f6
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78250945"
---
# <a name="storsimple-8100-and-8600-migration-to-azure-file-sync"></a>StorSimple 8100- und 8600-Migration zur Azure-Dateisynchronisierung

Die StorSimple 8000-Serie stellt zwei separate SKUs dar, und es ist möglich, die Daten von jeder dieser SKUs zu einer Umgebung der Azure-Dateisynchronisierung zu migrieren. In diesem Artikel wird beschrieben, wie Sie beide Appliances zur Azure-Dateisynchronisierung migrieren. Er enthält die erforderlichen Hintergrundinformationen und Migrationsschritte für ein erfolgreiches Migrieren zur Azure-Dateisynchronisierung.

## <a name="azure-file-sync"></a>Azure-Dateisynchronisierung

Die Azure-Dateisynchronisierung ist ein Microsoft-Clouddienst, der auf zwei Hauptkomponenten basiert:

* Dateisynchronisierung und Cloudtiering.
* Dateifreigaben als nativer Speicher in Azure, auf den über verschiedene Protokolle wie SMB und Datei-REST zugegriffen werden kann. Eine Azure-Dateifreigabe ist vergleichbar mit einer Dateifreigabe auf einem Windows-Server, die Sie nativ als Netzwerklaufwerk einbinden können. Sie unterstützt wichtige Genauigkeitsaspekte der Dateiverwaltung wie Attribute, Berechtigungen und Zeitstempel. Mit Azure-Dateifreigaben ist es nicht mehr erforderlich, dass eine Anwendung oder ein Dienst die in der Cloud gespeicherten Dateien und Ordner interpretiert. Sie können darauf nativ über vertraute Protokolle und Clients wie den Windows-Datei-Explorer zugreifen. Dadurch sind Azure-Dateifreigaben die ideale und flexibelste Methode, um allgemeine Dateiserverdaten und einige Anwendungsdaten in der Cloud zu speichern.

In diesem Artikel werden hauptsächlich die Migrationsschritte behandelt. Wenn Sie vor der Migration mehr über die Azure-Dateisynchronisierung erfahren möchten, empfehlen wir die folgenden Artikel:

* [Azure-Dateisynchronisierung – Übersicht](https://aka.ms/AFS "Übersicht")
* [Azure-Dateisynchronisierung – Bereitstellungsleitfaden](storage-sync-files-deployment-guide.md)

## <a name="migration-goals"></a>Migrationsziele

Das Ziel ist, die Integrität der Produktionsdaten sowie deren Verfügbarkeit zu gewährleisten. Letzteres erfordert minimale Ausfallzeiten, damit sie in normalen Wartungsfenstern stattfinden kann oder diese nur geringfügig überschreitet.

## <a name="storsimple-8000-series-migration-path-to-azure-file-sync"></a>Migrationspfad der StorSimple 8000-Serie zur Azure-Dateisynchronisierung

Zum Ausführen eines Azure-Dateisynchronisierungs-Agents ist ein lokaler Windows-Server erforderlich. Auf dem Windows-Server muss mindestens Windows Server 2012 R2 ausgeführt werden, idealerweise jedoch Windows Server 2019.

Es gibt zahlreiche alternative Migrationspfade, und der Artikel würde zu lang werden, wenn wir alle dokumentieren und veranschaulichen würden, warum sie Risiken oder Nachteile gegenüber dem von uns in diesem Artikel als bewährte Methode empfohlenen Pfad aufweisen.

![Übersicht über die Migrationsphasen der StorSimple 8000-Serie](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-overview.png "Migrationspfad für die StorSimple 8000-Serie: Übersicht über die Phasen weiter unten in diesem Artikel")

Die Abbildung oben zeigt Phasen, die Abschnitten in diesem Artikel entsprechen.
Wir verwenden eine cloudseitige Migration, um den unnötigen Rückruf von Dateien auf die lokale StorSimple-Appliance zu vermeiden. Mit dieser Methode werden Auswirkungen auf das lokale Zwischenspeicherverhalten oder die Nutzung der Netzwerkbandbreite vermieden, die Ihre Produktionsworkloads beeinflussen können.
Eine cloudseitige Migration erfolgt anhand einer Momentaufnahme (eines Volumeklons) Ihrer Daten. Somit sind Ihre Produktionsdaten bis zur Übernahme am Ende der Migration von diesem Prozess isoliert. Da im Wesentlichen mit einer Sicherung gearbeitet wird, ist die Migration sicher und leicht wiederholbar, falls Schwierigkeiten auftreten sollten.

## <a name="considerations-around-existing-storsimple-backups"></a>Überlegungen zu vorhandenen StorSimple-Sicherungen

StorSimple ermöglicht das Erstellen von Sicherungen in Form von Volumeklonen. In diesem Artikel wird ein neuer Volumeklon zum Migrieren Ihrer Livedateien verwendet.
Alle Anweisungen in diesem Artikel gelten auch dann, wenn Sie zusätzlich zu Ihren Livedaten auch Sicherungen migrieren müssen. Der einzige Unterschied besteht darin, dass Sie nicht mit einem neuen Volumeklon, sondern mit dem ältesten Sicherungsvolumeklon beginnen, den Sie migrieren müssen.

Der Ablauf ist wie folgt:

* Legen Sie den Mindestsatz an Volumenklonen fest, den Sie migrieren müssen. Es wird empfohlen, diese Liste möglichst kurz zu halten, da der gesamte Migrationsprozess umso länger dauert, je mehr Sicherungen migriert werden.
* Beim Durchlaufen des Migrationsprozesses beginnen Sie mit dem ältesten zu migrierenden Volumeklon und verwenden dann bei jeder nachfolgenden Migration den jeweils nächstältesten.
* Nach Abschluss jeder Migration eines Volumeklons müssen Sie eine Azure-Dateifreigabemomentaufnahme erstellen. [Azure-Dateifreigabemomentaufnahmen](storage-snapshots-files.md) dienen zur Aufbewahrung von Zeitpunktsicherungen der Dateien und der Ordnerstruktur für Ihre Azure-Dateifreigaben. Sie benötigen diese Momentaufnahmen nach Abschluss der Migration, um sicherzustellen, dass Sie während der Migration Versionen aller Ihrer Volumeklone beibehalten haben.
* Stellen Sie sicher, dass Sie Azure-Dateifreigabemomentaufnahmen für alle Azure-Dateifreigaben erstellen, die vom gleichen StorSimple-Volume bedient werden. Volumeklone befinden sich auf der Volumeebene. Azure-Dateifreigabemomentaufnahmen befinden sich auf der Freigabeebene. Nachdem die Migration eines Volumeklons abgeschlossen ist, müssen Sie eine Freigabemomentaufnahme (auf jeder Azure-Dateifreigabe) erstellen.
* Wiederholen Sie den Migrationsprozess für einen Volumeklon, und erstellen Sie Freigabemomentaufnahmen nach jedem Volumeklon, bis Sie zu einer Momentaufnahme der Livedaten gelangt sind. Der Migrationsprozess für einen Volumeklon wird in den folgenden Phasen beschrieben. 

Wenn Sie keine Sicherungen verschieben müssen und eine neue Sicherungskette auf der Azure-Dateifreigabe beginnen können, nachdem nur die Livedaten migriert wurden, kann dadurch die Komplexität der Migration und deren Zeitdauer verringert werden. Sie können entscheiden, ob und wie viele Sicherungen für jedes Volume (nicht für jede Freigabe) in StorSimple verschoben werden.

## <a name="phase-1-get-ready"></a>Phase 1: Vorbereiten

:::row:::
    :::column:::
        ![Abbildung eines Teils des vorherigen Übersichtsbilds, der den Schwerpunkt dieses Unterabschnitts des Artikels darstellt.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-1.png)
    :::column-end:::
    :::column:::
        Die Basis für die Migration ist ein Volumeklon und eine virtuelle Cloudappliance, die als StorSimple 8020 bezeichnet wird.
Diese Phase konzentriert sich auf die Bereitstellung dieser Ressourcen in Azure.
    :::column-end:::
:::row-end:::

### <a name="deploy-a-storsimple-8020-virtual-appliance"></a>Bereitstellen einer virtuellen StorSimple 8020-Appliance

Das Bereitstellen einer Cloudappliance ist ein Prozess, der Sicherheit, Netzwerkbetrieb und einige andere Aspekte erfordert.

> [!IMPORTANT]
> Der folgende Leitfaden enthält einige nicht benötige Abschnitte. Lesen und befolgen Sie den Artikel vom Anfang bis zu Schritt 3. Dann kehren Sie zu diesem Artikel zurück. Zu diesem Zeitpunkt ist es nicht erforderlich, Schritt 3 oder darüber hinausgehende Anweisungen in diesem Leitfaden auszuführen.

[Bereitstellung einer virtuellen StorSimple 8020-Appliance](../../storsimple/storsimple-8000-cloud-appliance-u2.md)

### <a name="determine-a-volume-clone-to-use"></a>Festlegen eines zu verwendenden Volumeklons

Wenn Sie mit der Migration beginnen möchten, erstellen Sie im ersten Schritt einen neuen Volumeklon (wie es auch bei einer Sicherung der Fall wäre), mit dem der aktuelle Zustand des StorSimple-Cloudspeichers erfasst wird. Erstellen Sie für jedes vorhandene StorSimple-Volume einen Klon.
Wenn Sie Sicherungen verschieben müssen, handelt es sich beim ersten verwendeten Volumeklon nicht um einen neu erstellten Klon, sondern um den ältesten Volumeklon (älteste Sicherung), den Sie migrieren müssen.
Ausführliche Anleitungen finden Sie im Abschnitt [Überlegungen zu vorhandenen StorSimple-Sicherungen](#considerations-around-existing-storsimple-backups).

> [!IMPORTANT]
> Der folgende Leitfaden enthält einige nicht benötige Abschnitte. Lesen und befolgen Sie nur die Schritte, die in dem über den Link verknüpften Abschnitt beschrieben sind. Dann kehren Sie zu diesem Artikel zurück. Dem Abschnitt „Nächste Schritte“ müssen Sie nicht folgen.

[Erstellen von Klonen von einem Volume](../../storsimple/storsimple-8000-clone-volume-u2.md#create-a-clone-of-a-volume)

### <a name="use-the-volume-clone"></a>Verwenden des Volumeklons

Im letzten Schritt von Phase 1 machen Sie den ausgewählten Volumeklon auf der virtuellen 8020-Appliance in Azure verfügbar.

> [!IMPORTANT]
> Im folgenden Leitfaden sind die erforderlichen Schritte angegeben, doch er enthält am Ende auch eine Anweisung zum Formatieren des Volumes. **FORMATIEREN SIE DAS VOLUME NICHT.** Lesen und befolgen Sie den über den Link verknüpften Abschnitt 7 vom Anfang bis zur Anweisung „10. Führen Sie die folgenden Schritte aus, um ein einfaches Volume zu formatieren“.  Führen Sie diesen Schritt nicht aus, und kehren Sie zu diesem Artikel zurück.

[Bereitstellen eines Volumeklons auf der virtuellen 8020-Appliance in Azure](../../storsimple/storsimple-8000-deployment-walkthrough-u2.md#step-7-mount-initialize-and-format-a-volume)

### <a name="phase-1-summary"></a>Zusammenfassung von Phase 1

Sie haben Phase 1 abgeschlossen und dabei die folgenden Schritte ausgeführt:

* Sie haben eine virtuelle StorSimple 8020-Appliance in Azure bereitgestellt.
* Sie haben festgelegt, mit welchem Volumeklon die Migration beginnen soll.
* Sie haben Ihre Volumeklone (einen für jedes Livevolume) für die virtuelle StorSimple-Appliance in Azure bereitgestellt, wodurch die Daten zur weiteren Verwendung verfügbar sind.

## <a name="phase-2-cloud-vm"></a>Phase 2: Cloud-VM

:::row:::
    :::column:::
        ![Abbildung eines Teils des vorherigen Übersichtsbilds, der den Schwerpunkt dieses Unterabschnitts des Artikels darstellt.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-2.png)
    :::column-end:::
    :::column:::
        Da nun der anfängliche Klon auf der virtuellen StorSimple 8020-Appliance in Azure zur Verfügung steht, stellen Sie jetzt eine VM bereit und machen den Volumeklon (oder mehrere Volumenklone) für diese VM über iSCSI verfügbar.
    :::column-end:::
:::row-end:::

### <a name="deploy-an-azure-vm"></a>Bereitstellen einer Azure-VM

Bei der Windows Server-VM in Azure handelt es sich wie bei StorSimple 8020 um ein temporäres Teil der Infrastruktur, das nur während der Migration benötigt wird.
Die Konfiguration der von Ihnen bereitgestellten VM hängt größtenteils von der Anzahl der Elemente (Dateien und Ordner) ab, die synchronisiert werden sollen. Wenn Sie Bedenken haben, empfiehlt es sich, eine leistungsstärkere Konfiguration zu verwenden.

Mit einem einzelnen Windows-Server können bis zu 30 Azure-Dateifreigaben synchronisiert werden.
Die Spezifikationen, für die Sie sich entscheiden, müssen jede Freigabe/jeden Pfad oder den Stamm des StorSimple-Volumes sowie die Anzahl der Elemente (Dateien und Ordner) umfassen.

Die Gesamtgröße der Daten stellt weniger einen Engpass dar, sondern die Anzahl der Elemente, an die Sie die Computerspezifikationen anpassen müssen.

* [Hier erfahren Sie, wie Sie die Größe eines Windows-Servers basierend auf der Anzahl der zu synchronisierenden Elemente (Dateien und Ordner) anpassen.](storage-sync-files-planning.md#recommended-system-resources)
* [Hier erfahren Sie, wie Sie eine Windows Server-VM bereitstellen.](../../virtual-machines/windows/quick-create-portal.md)

> [!IMPORTANT]
> Stellen Sie sicher, dass die VM in derselben Azure-Region wie die virtuelle StorSimple 8020-Appliance bereitgestellt wird. Wenn Sie im Rahmen dieser Migration auch die Region Ihrer Clouddaten in eine andere Region als die ändern müssen, in der sie derzeit gespeichert werden, können Sie dies in einem späteren Schritt beim Bereitstellen von Azure-Dateifreigaben vornehmen.

### <a name="expose-the-storsimple-8020-volumes-to-the-vm"></a>Verfügbarmachen der StorSimple 8020-Volumes für die VM

In dieser Phase verbinden Sie ein oder mehrere StorSimple-Volumes von der virtuellen 8020-Appliance über iSCSI mit der bereitgestellten Windows Server-VM.

> [!IMPORTANT]
> Führen Sie nur die Anweisungen in den Abschnitten **Abrufen der privaten IP für das Cloudgerät** und **Herstellen einer Verbindung über iSCSI** in den folgenden Artikeln aus, und kehren Sie dann zu diesem Artikel zurück.

1. [Abrufen der privaten IP für das Cloudgerät](../../storsimple/storsimple-8000-cloud-appliance-u2.md#get-private-ip-for-the-cloud-appliance)
2. [Herstellen einer Verbindung über iSCSI](../../storsimple/storsimple-8000-deployment-walkthrough-u2.md#step-7-mount-initialize-and-format-a-volume)

### <a name="phase-2-summary"></a>Zusammenfassung von Phase 2

Sie haben Phase 2 abgeschlossen und dabei die folgenden Schritte ausgeführt: 

* Sie haben eine Windows Server-VM in derselben Region wie die virtuelle StorSimple 8020-Appliance bereitgestellt.
* Sie haben alle anwendbaren Volumes von der 8020-Appliance über iSCSI für die Windows Server-VM verfügbar gemacht.
* Wenn Sie den Datei-Explorer auf der Server-VM für die bereitgestellten Volumes verwenden, sollten Ihnen nun Datei- und Ordnerinhalte angezeigt werden.

Fahren Sie erst dann mit Phase 3 fort, wenn Sie diese Schritte für alle Volumes ausgeführt haben, die eine Migration erfordern.

## <a name="phase-3-set-up-azure-file-shares-and-get-ready-for-azure-file-sync"></a>Phase 3: Einrichten von Azure-Dateifreigaben und Vorbereiten auf die Azure-Dateisynchronisierung

:::row:::
    :::column:::
        ![Abbildung eines Teils des vorherigen Übersichtsbilds, der den Schwerpunkt dieses Unterabschnitts des Artikels darstellt.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-3.png)
    :::column-end:::
    :::column:::
        In dieser Phase bestimmen Sie eine Reihe von Azure-Dateifreigaben und stellen diese bereit, erstellen einen lokalen Windows-Server als Ersatz für die StorSimple-Appliance und konfigurieren diesen Server für die Azure-Dateisynchronisierung. 
    :::column-end:::
:::row-end:::

### <a name="map-your-existing-namespaces-to-azure-file-shares"></a>Zuordnen vorhandener Namespaces zu Azure-Dateifreigaben

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="deploy-azure-file-shares"></a>Bereitstellen von Azure-Dateifreigaben

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

> [!TIP]
> Wenn Sie die Azure-Region von der aktuellen Region, in der sich Ihre StorSimple-Daten befinden, in eine andere Region ändern müssen, stellen Sie die Azure-Dateifreigaben in der neuen Region bereit, die Sie verwenden möchten. Sie legen die Region fest, indem Sie sie beim Bereitstellen der Speicherkonten auswählen, die Ihre Azure-Dateifreigaben enthalten. Stellen Sie sicher, dass sich auch die Ressource für die Azure-Dateisynchronisierung, die Sie nachfolgend bereitstellen, in derselben neuen Region befindet.

### <a name="deploy-the-azure-file-sync-cloud-resource"></a>Bereitstellen der Cloudressource für die Azure-Dateisynchronisierung

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

> [!TIP]
> Wenn Sie die Azure-Region von der aktuellen Region, in der sich Ihre StorSimple-Daten befinden, in eine andere Region ändern müssen, dann haben Sie die Speicherkonten für Ihre Azure-Dateifreigaben in der neuen Region bereitgestellt. Stellen Sie sicher, dass Sie bei der Bereitstellung dieses Speichersynchronisierungsdiensts dieselbe Region ausgewählt haben.

### <a name="deploy-an-on-premises-windows-server"></a>Bereitstellen eines lokalen Windows-Servers

* Erstellen Sie einen Server mit Windows Server 2019 (oder mindestens 2012 R2) als virtuellen Computer oder physischen Server. Ein Windows Server-Failovercluster wird ebenfalls unterstützt. Verwenden Sie nicht wieder den Server, den Sie möglicherweise StorSimple 8100 oder 8600 vorgeschaltet haben.
* Stellen Sie direkt angeschlossenen Speicher (DAS, im Gegensatz zum nicht unterstützten NAS) bereit, oder fügen Sie ihn hinzu.

Es wird empfohlen, dem neuen Windows-Server eine gleich große oder größere Menge an Speicher bereitzustellen als die, die Ihrer StorSimple 8100- oder 8600-Appliance lokal zum Zwischenspeichern zur Verfügung steht. Sie verwenden den Windows-Server auf die gleiche Weise wie die StorSimple-Appliance, und wenn er über die gleiche Menge an Speicher wie die Appliance verfügt, sollte die Zwischenspeicherung ähnlich sein, wenn nicht sogar identisch.
Sie können nach Wunsch Speicher zu Ihrem Windows-Server hinzufügen oder daraus entfernen. Dadurch können Sie die Größe des lokalen Volumes und die Menge des für die Zwischenspeicherung verfügbaren lokalen Speichers anpassen.

### <a name="prepare-the-windows-server-for-file-sync"></a>Vorbereiten des Windows-Servers für die Dateisynchronisierung

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

### <a name="configure-azure-file-sync-on-the-windows-server"></a>Konfigurieren der Azure-Dateisynchronisierung auf dem Windows-Server

Der registrierte lokale Windows-Server muss für diesen Prozess bereit und mit dem Internet verbunden sein.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!WARNING]
> **Aktivieren Sie unbedingt das Cloudtiering!** Cloudtiering ist das AFS-Feature, das es dem lokalen Server ermöglicht, weniger Speicherkapazität als in der Cloud zu haben, aber trotzdem über den vollständigen Namespace zu verfügen. Lokal interessante Daten werden zudem für eine schnelle, lokale Zugriffsleistung lokal zwischengespeichert. Ein weiterer Grund für das Aktivieren von Cloudtiering in diesem Schritt ist, dass der Dateiinhalt in dieser Phase nicht synchronisiert werden soll, sondern nur der Namespace verschoben werden sollte.

## <a name="phase-4-configure-the-azure-vm-for-sync"></a>Phase 4: Konfigurieren der Azure-VM für die Synchronisierung

:::row:::
    :::column:::
        ![Abbildung eines Teils des vorherigen Übersichtsbilds, der den Schwerpunkt dieses Unterabschnitts des Artikels darstellt.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-4.png)
    :::column-end:::
    :::column:::
        Diese Phase bezieht sich auf Ihre Azure-VM mit den über iSCSI bereitgestellten ersten Volumeklonen. In dieser Phase wird die VM über die Azure-Dateisynchronisierung verbunden, und es wird eine erste Runde zum Verschieben von Dateien aus Ihren StorSimple-Volumeklonen gestartet.
        
    :::column-end:::
:::row-end:::

Sie haben bereits den lokalen Server, der die StorSimple 8100- oder 8600-Appliance ersetzt, für die Azure-Dateisynchronisierung konfiguriert. 

Die Vorgehensweise zum Konfigurieren der Azure-VM ist fast identisch, umfasst jedoch einen zusätzlichen Schritt. Mit den folgenden Schritten werden Sie durch den Prozess geführt.

> [!IMPORTANT]
> Es ist wichtig, dass die Azure-VM **nicht mit aktiviertem Cloudtiering konfiguriert ist!** Sie tauschen das Volume dieses Servers während der Migration durch neuere Volumeklone aus. Das Cloudtiering bietet keinen Vorteil und bedeutet eine höhere CPU-Auslastung, die Sie vermeiden sollten.

1. [Bereitstellen des AFS-Agents (siehe vorheriger Abschnitt)](#prepare-the-windows-server-for-file-sync)
2. [Vorbereiten der VM für die Azure-Dateisynchronisierung](#get-the-vm-ready-for-azure-file-sync)
3. [Konfigurieren der Synchronisierung](#configure-azure-file-sync-on-the-azure-vm)

### <a name="get-the-vm-ready-for-azure-file-sync"></a>Vorbereiten der VM für die Azure-Dateisynchronisierung

Mit der Azure-Dateisynchronisierung werden die Dateien von den über iSCSI bereitgestellten StorSimple-Volumes in die Azure-Zieldateifreigaben verschoben.
Während dieses Migrationsprozesses stellen Sie mehrere Volumeklone auf der VM unter dem gleichen Laufwerkbuchstaben bereit. Die Azure-Dateisynchronisierung muss so konfiguriert werden, dass der nächste bereitgestellte Volumeklon als eine neuere Version der Dateien und Ordner gesehen wird und die über die Azure-Dateisynchronisierung verbundenen Azure-Dateifreigaben aktualisiert werden. 

> [!IMPORTANT]
> Damit dies funktioniert, muss vor dem Konfigurieren der Azure-Dateisynchronisierung ein Registrierungsschlüssel auf dem Server festgelegt werden.

1. Erstellen Sie ein neues Verzeichnis auf dem Systemlaufwerk der VM. Azure-Dateisynchronisierungsinformationen müssen dort statt auf den bereitgestellten Volumeklonen dauerhaft gespeichert werden. Beispiel: `“C:\syncmetadata”`
2. Öffnen Sie den Registrierungs-Editor, und suchen Sie nach der folgenden Registrierungsstruktur: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync`
3. Erstellen Sie einen neuen Schlüssel vom Zeichenfolgentyp mit folgendem Namen: ***MetadataRootPath***
4. Legen Sie den vollständigen Pfad zu dem Verzeichnis fest, das Sie auf dem Systemvolume erstellt haben, z. B.: `C:\syncmetadata”`

### <a name="configure-azure-file-sync-on-the-azure-vm"></a>Konfigurieren der Azure-Dateisynchronisierung auf der Azure-VM

Dieser Schritt ähnelt dem vorherigen Abschnitt, in dem das Konfigurieren von AFS auf dem lokalen Server erläutert wird.

Der Unterschied besteht darin, dass Sie Cloudtiering auf diesem Server nicht aktivieren dürfen und sicherstellen müssen, dass die richtigen Ordner mit den richtigen Azure-Dateifreigaben verbunden sind. Andernfalls stimmt die Benennung von Azure-Dateifreigaben und Dateninhalten nicht überein, und es gibt keine Möglichkeit, die Cloudressourcen oder lokalen Ordner umzubenennen, ohne die Synchronisierung neu zu konfigurieren.

Informationen finden Sie im [vorherigen Abschnitt zum Konfigurieren der Azure-Dateisynchronisierung auf einem Windows-Server](#configure-azure-file-sync-on-the-windows-server).

### <a name="step-4-summary"></a>Zusammenfassung von Schritt 4

An diesem Punkt haben Sie die Azure-Dateisynchronisierung auf der Azure-VM, auf der Sie die StorSimple-Volumeklone über iSCSI bereitgestellt haben, erfolgreich konfiguriert.
Die Daten werden jetzt von der Azure-VM an die verschiedenen Azure-Dateifreigaben übertragen, und von dort wird ein mehrstufiger Namespace auf Ihrem lokalen Windows-Server angezeigt.

> [!IMPORTANT]
> Stellen Sie sicher, dass zu diesem Zeitpunkt weder Änderungen vorgenommen werden noch Benutzerzugriff auf den Windows-Server erteilt wird.

Das anfängliche Verschieben von Volumeklondaten über die Azure-VM in die Azure-Dateifreigaben kann einige Zeit in Anspruch nehmen, möglicherweise sogar Wochen. Das Schätzen der benötigten Zeit ist schwierig und hängt von vielen Faktoren ab. Insbesondere von der Geschwindigkeit, mit der die Azure-VM auf Dateien auf den StorSimple-Volumes zugreifen kann, und wie schnell die Azure-Dateisynchronisierung die zu synchronisierenden Dateien und Ordner verarbeiten kann. 

Die Erfahrung hat gezeigt, dass die Bandbreite – und somit die tatsächliche Datengröße – nur eine untergeordnete Rolle spielt. Die Zeit, die diese oder eine nachfolgende Migrationsrunde in Anspruch nimmt, hängt größtenteils von der Anzahl der Elemente ab, die pro Sekunde verarbeitet werden können. So wird beispielsweise 1 TiB mit 100.000 Dateien und Ordnern höchstwahrscheinlich langsamer ausgeführt als 1 TiB mit nur 50.000 Dateien und Ordnern.

## <a name="phase-5-iterate-through-multiple-volume-clones"></a>Phase 5: Durchlaufen mehrerer Volumeklone

:::row:::
    :::column:::
        ![Abbildung eines Teils des vorherigen Übersichtsbilds, der den Schwerpunkt dieses Unterabschnitts des Artikels darstellt.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-5.png)
    :::column-end:::
    :::column:::
        Wie bereits in der vorherigen Phase erläutert, kann die anfängliche Synchronisierung einige Zeit in Anspruch nehmen. Die Benutzer und Anwendungen greifen weiterhin auf die lokale StorSimple 8100- oder 8600-Appliance zu. Das bedeutet, dass sich Änderungen anhäufen und mit jedem Tag größere Abweichungen zwischen den Livedaten und dem anfänglichen Volumenklon, den Sie gerade migrieren, entstehen. In diesem Abschnitt erfahren Sie, wie Sie Ausfallzeiten minimieren können, indem Sie mehrerer Volumeklone verwenden und angeben, wann die Synchronisierung abgeschlossen ist.
    :::column-end:::
:::row-end:::

Leider erfolgt der Migrationsprozess nicht sofort. Das bedeutet, dass die zuvor erwähnten Abweichungen gegenüber den Livedaten eine unvermeidliche Folge sind. Die gute Nachricht ist, dass Sie den Prozess der Bereitstellung neuer Volumeklone wiederholen können. Die jeweiligen Abweichungen bei den einzelnen Volumeklonen werden zunehmend geringer. So kann letztendlich eine Synchronisierung in einem Zeitraum durchgeführt werden, der für Sie akzeptabel ist, um Benutzer und Apps für die Übernahme auf den lokalen Windows-Server offline zu schalten.

Wiederholen Sie die folgenden Schritte, bis die Synchronisierung innerhalb einer Zeitspanne abgeschlossen wird, über die das Offlineschalten von Benutzern und Apps für Sie akzeptabel ist:

1. [Ermitteln, ob die Synchronisierung für einen bestimmten Volumeklon abgeschlossen ist](#determine-when-sync-is-done)
2. [Erstellen eines neuen Volumeklons und Bereitstellen auf der virtuellen 8020-Appliance](#the-next-volume-clones)
3. [Bestimmen, wann die Synchronisierung abgeschlossen ist](#determine-when-sync-is-done)
4. [Übernahmestrategie](#cut-over-strategy)

### <a name="the-next-volume-clones"></a>Nächster Volumeklon

Das Erstellen eines Volumenklons wurde bereits zuvor in diesem Artikel erläutert.
Diese Phase umfasst zwei Aktionen:

1. [Erstellen eines Volumeklons](../../storsimple/storsimple-8000-clone-volume-u2.md#create-a-clone-of-a-volume)
2. [Bereitstellen dieses Volumeklons (siehe oben)](#use-the-volume-clone)

### <a name="determine-when-sync-is-done"></a>Bestimmen, wann die Synchronisierung abgeschlossen ist

Wenn die Synchronisierung abgeschlossen ist, können Sie die Zeitmessung beenden und ermitteln, ob Sie das Erstellen und Bereitstellen eines Volumeklons wiederholen müssen oder ob die Zeit, die für die Synchronisierung mit dem letzten Volumeklon benötigt wurde, kurz genug war.

Führen Sie die folgenden Schritte aus, um zu ermitteln, ob die Synchronisierung abgeschlossen ist:

1. Öffnen Sie die Ereignisanzeige, und navigieren Sie zu **Anwendungen und Dienste**.
2. Navigieren Sie zu **Microsoft\FileSync\Agent\Telemetry**, und öffnen Sie es.
3. Suchen Sie nach dem neuesten **Ereignis 9102**, das einer abgeschlossenen Synchronisierungssitzung entspricht.
4. Wählen Sie **Details** aus, und vergewissern Sie sich, dass für **SyncDirection** der Wert **Upload** angegeben ist.
5. Überprüfen Sie **HResult**, und vergewissern Sie sich, dass **0** angezeigt wird. Das bedeutet, dass die Synchronisierungssitzung erfolgreich war. Wenn HResult einen Wert ungleich 0 (null) aufweist, ist während der Synchronisierung ein Fehler aufgetreten. Wenn **PerItemErrorCount** größer als 0 ist, wurden einige Dateien oder Ordner nicht ordnungsgemäß synchronisiert. Es kann vorkommen, dass ein HResult von „0“ angibt, aber eine PerItemErrorCount angegeben wird, die größer als 0 ist. An diesem Punkt müssen Sie sich keine Gedanken über den PerItemErrorCount-Wert machen. Um diese Dateien kümmern wir uns später. Falls diese Fehleranzahl sehr hoch ist (Tausende von Elementen), wenden Sie sich an den Kundensupport, und bitten Sie um eine Verbindung mit der Produktgruppe für die Azure-Dateisynchronisierung, um direkte Anleitungen zur besten Vorgehensweise in den nächsten Phasen zu erhalten.
6. Prüfen Sie, ob mehrere 9102-Ereignisse mit dem HResult-Wert „0“ in einer Zeile angezeigt werden. Dies weist darauf hin, dass die Synchronisierung für diesen Volumeklon abgeschlossen ist.

### <a name="cut-over-strategy"></a>Übernahmestrategie

1. Feststellen, ob die Synchronisierung von einem Volumeklon jetzt schnell genug ausgeführt wird (Abweichungen sind gering genug)
2. Offlineschalten der StorSimple-Appliance
3. Abschließende RoboCopy

Messen Sie die Zeit, und stellen Sie fest, ob die Synchronisierung von einem aktuellen Volumeklon innerhalb eines Zeitfensters abgeschlossen werden kann, das klein genug ist und das Sie als Ausfallzeit in Ihrem System akzeptieren können.

Jetzt ist es Zeit, den Benutzerzugriff auf die StorSimple-Appliance zu deaktivieren. Keine weiteren Änderungen. Die Ausfallzeit hat begonnen.
Die Appliance muss online und verbunden bleiben, doch müssen jetzt Änderungen verhindert werden.

In Phase 6 werden jegliche Abweichungen zwischen den Livedaten und dem letzten Volumeklon aufgehoben.

## <a name="phase-6-a-final-robocopy"></a>Phase 6: Abschließende RoboCopy

An diesem Punkt gibt es zwei Unterschiede zwischen Ihrem lokalen Windows-Server und der StorSimple 8100- oder 8600-Appliance:

1. Es können Dateien vorhanden sein, die nicht synchronisiert wurden (siehe **PerItemErrors** im oben genannten Ereignisprotokoll).
2. Die StorSimple-Appliance weist einen gefüllten Cache auf, wohingegen der Windows-Server zu diesem Zeitpunkt nur über einen Namespace ohne lokal gespeicherten Dateiinhalt verfügt.

![Abbildung eines Teils des vorherigen Übersichtsbilds, der den Schwerpunkt dieses Unterabschnitts des Artikels darstellt.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-6.png)

Mit einer abschließenden RoboCopy können Sie den Cache des Windows-Servers auf den Stand der Appliance bringen und sicherstellen, dass keine Datei ausgelassen wird.

> [!CAUTION]
> Es ist unbedingt erforderlich, dass der von Ihnen verwendete RoboCopy-Befehl genau wie unten beschrieben ist. Es sollen nur lokale Dateien und Dateien, die mit der Volumeklon- und Synchronisierungsmethode nicht verschoben wurden, kopiert werden. Das Problem, warum diese nicht synchronisiert wurden, kann zu einem späteren Zeitpunkt nach Abschluss der Migration gelöst werden. (Informationen finden Sie unter der [Problembehandlung für die Azure-Dateisynchronisierung](storage-sync-files-troubleshoot.md#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing). Höchstwahrscheinlich sind nicht druckbare Zeichen in Dateinamen enthalten, die genauso gut gelöscht werden können.)

RoboCopy-Befehl:

```console
Robocopy /MT:32 /UNILOG:<file name> /TEE /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Hintergrund:

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      Ermöglicht RoboCopy die Multithread-Ausführung. Der Standardwert ist 8, der Höchstwert 128.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /UNILOG:<file name>
   :::column-end:::
   :::column span="1":::
      Gibt den Status als UNICODE in die LOG-Datei aus (überschreibt vorhandenes Protokoll).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /TEE
   :::column-end:::
   :::column span="1":::
      Ausgabe an das Konsolenfenster. Wird in Verbindung mit der Ausgabe in eine Protokolldatei verwendet.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /MIR
   :::column-end:::
   :::column span="1":::
      Ermöglicht RoboCopy, nur Abweichungen zwischen der Quelle (StorSimple-Appliance) und dem Ziel (Windows Server-Verzeichnis) zu berücksichtigen.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY:Kopierflag[s]
   :::column-end:::
   :::column span="1":::
      Genauigkeit der Dateikopie (Standard: „/COPY:DAT“), Kopierflags: D = Daten, A = Attribute, T = Zeitstempel, S = Sicherheit = NTFS-ACLs, O = Besitzerinformationen, U = Überprüfungsinformationen
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPYALL
   :::column-end:::
   :::column span="1":::
      Kopieren aller Dateiinformationen (entspricht „/COPY:DATSOU“)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /DCOPY:Kopierflag[s]
   :::column-end:::
   :::column span="1":::
      Genauigkeit für Kopien von Verzeichnissen (Standard: „/DCOPY:DA“), Kopierflags: D = Daten, A = Attribute, T = Zeitstempel
   :::column-end:::
:::row-end:::

Sie sollten diesen RoboCopy-Befehl für jedes Verzeichnis auf dem Windows-Server als Ziel ausführen, das Sie mit Dateisynchronisierung in eine Azure-Datei konfiguriert haben.

Sie können mehrere dieser Befehle parallel ausführen.
Nachdem dieser RoboCopy-Schritt abgeschlossen ist, können Sie den Benutzern und Apps den Zugriff auf den Windows-Server gestatten, wie es zuvor bei der StorSimple-Appliance der Fall war.

Überprüfen Sie die RoboCopy-Protokolldateien, um festzustellen, ob Dateien ausgelassen wurden. Falls Probleme vorliegen, können Sie diese in den meisten Fällen lösen, nachdem die Migration abgeschlossen ist und die Benutzer und Apps wieder zum Windows-Server zurückgekehrt sind. Wenn Sie Probleme beheben müssen, tun Sie dies vor Phase 7.

Wahrscheinlich ist es erforderlich, die SMB-Freigaben auf dem Windows-Server zu erstellen, die zuvor für die StorSimple-Daten vorhanden waren. Sie können diesen Schritt vorverlegen und zu einem früheren Zeitpunkt ausführen, um hier keine Zeit zu verlieren, doch müssen Sie sicherstellen, dass vor dem jetzigen Zeitpunkt keine Änderungen an Dateien auf dem Windows-Server erfolgen.

Wenn Sie über eine DFS-N-Bereitstellung verfügen, können Sie die DFN-Namespaces auf die neuen Speicherorte für Serverordner verweisen. Wenn keine DFS-N-Bereitstellung vorliegt und Sie der 8100/8600-Appliance lokal einen Windows-Server vorangestellt hatten, können Sie diesen Server aus der Domäne entfernen und den neuen Windows-Server mit AFS in die Domäne einbinden. Dabei geben Sie dem Server denselben Namen wie dem alten Server und dieselben Freigabenamen, damit der Übergang zum neuen Server für Benutzer, Gruppenrichtlinien oder Skripts transparent bleibt.

## <a name="phase-7-deprovision"></a>Phase 7: Aufheben der Bereitstellung

In der letzten Phase haben Sie mehrere Volumeklone durchlaufen und konnten schließlich den Benutzerzugriff auf den neuen Windows-Server übertragen, nachdem Sie die StorSimple-Appliance offline geschaltet haben.

Jetzt können Sie damit beginnen, die Bereitstellung nicht benötigter Ressourcen aufzuheben.
Bevor Sie damit beginnen, wird empfohlen, die neue Bereitstellung der Azure-Dateisynchronisierung in der Produktionsumgebung eine Weile zu beobachten. Dadurch haben Sie die Möglichkeit, eventuelle Probleme beheben.

Wenn Sie zufrieden sind und die AFS-Bereitstellung mindestens einige Tage lang beobachtet haben, können Sie damit beginnen, die Bereitstellung von Ressourcen in folgender Reihenfolge aufzuheben:

1. Deaktivieren Sie die Azure-VM, die verwendet wurde, um Daten mithilfe der Dateisynchronisierung von den Volumenklonen in die Azure-Dateifreigaben zu verschieben.
2. Wechseln Sie zur Ressource für den Speichersynchronisierungsdienst in Azure, und heben Sie die Registrierung der Azure-VM auf. Dadurch wird sie aus allen Synchronisierungsgruppen entfernt.

    > [!WARNING]
    > **Vergewissern Sie sich, dass Sie den richtigen Computer auswählen.** Da Sie die Cloud-VM deaktiviert haben, sollte sie in der Liste der registrierten Server als einziger Offlineserver angezeigt werden. Sie dürfen in diesem Schritt nicht den lokalen Windows-Server auswählen, da sonst dessen Registrierung aufgehoben wird.

3. Löschen Sie die Azure-VM und deren Ressourcen.
4. Deaktivieren Sie die virtuelle StorSimple 8020-Appliance.
5. Heben Sie die Bereitstellung aller StorSimple-Ressourcen in Azure auf.
6. Trennen Sie die physische StorSimple-Appliance von Ihrem Rechenzentrum.

Die Migration ist abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit der Azure-Dateisynchronisierung vertraut. Dies gilt insbesondere für die Flexibilität von Richtlinien für das Cloudtiering.

Wenn Sie im Azure-Portal oder durch frühere Ereignisse feststellen, dass einige Dateien dauerhaft nicht synchronisiert werden, lesen Sie den Leitfaden zur Problembehandlung, um diese Probleme zu beheben.

* [Azure-Dateisynchronisierung – Übersicht: aka.ms/AFS](https://aka.ms/AFS)
* [Cloudtiering](storage-sync-cloud-tiering.md) 
* [Leitfaden zur Problembehandlung bei der Azure-Dateisynchronisierung](storage-sync-files-troubleshoot.md)
