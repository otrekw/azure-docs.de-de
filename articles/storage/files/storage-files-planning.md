---
title: Planung für eine Azure Files-Bereitstellung | Microsoft-Dokumentation
description: Erfahren Sie, was Sie beim Planen einer Azure Files-Bereitstellung berücksichtigen müssen.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 1/3/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d1d36c6f6413a9438063c6fe30403af095ed9a6b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84659635"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planung für eine Azure Files-Bereitstellung
[Azure Files](storage-files-introduction.md) kann auf zwei Arten bereitgestellt werden: durch direktes Einbinden der serverlosen Azure-Dateifreigaben oder durch lokales Zwischenspeichern von Azure-Dateifreigaben mithilfe von Azure-Dateisynchronisierung. Welche Bereitstellungsoption Sie auswählen, ändert die Aspekte, die Sie beim Planen der Bereitstellung berücksichtigen müssen. 

- **Direktes Einbinden einer Azure-Dateifreigabe**: Da Azure Files SMB-Zugriff bietet, können Sie Azure-Dateifreigaben lokal oder in der Cloud mithilfe des standardmäßigen SMB-Clients einbinden, der unter Windows, macOS und Linux verfügbar ist. Da Azure-Dateifreigaben serverlos sind, erfordert die Bereitstellung für Produktionsszenarien keine Verwaltung eines Dateiservers oder NAS-Geräts. Dies bedeutet, dass Sie keine Softwarepatches anwenden oder physische Datenträger austauschen müssen. 

- **Lokales Zwischenspeichern von Azure-Dateifreigaben mit Azure-Dateisynchronisierung**: Die Azure-Dateisynchronisierung ermöglicht das Zentralisieren der Dateifreigaben Ihrer Organisation in Azure Files, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Die Azure-Dateisynchronisierung transformiert Ihre lokalen Windows Server-Computer in einen schnellen Cache für Ihre Azure-Dateifreigabe. 

Dieser Artikel befasst sich hauptsächlich mit Bereitstellungsüberlegungen zum Bereitstellen einer Azure-Dateifreigabe, die direkt von einem lokalen oder cloudbasierten Client eingebunden werden soll. Weitere Informationen zum Planen einer Azure-Dateisynchronisierungsbereitstellung finden Sie unter [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md).

## <a name="management-concepts"></a>Verwaltungskonzepte
[!INCLUDE [storage-files-file-share-management-concepts](../../../includes/storage-files-file-share-management-concepts.md)]

Beim Bereitstellen von Azure-Dateifreigaben in Speicherkonten wird Folgendes empfohlen:

- Ausschließliches Bereitstellen von Azure-Dateifreigaben in Speicherkonten mit anderen Azure-Dateifreigaben. Auch wenn GPv2-Speicherkonten Speicherkonten mit verschiedenen Zwecken ermöglichen, kann das Mischen von Ressourcen das spätere Beheben von Leistungsproblemen erschweren, da für Speicherressourcen wie z. B. Azure-Dateifreigaben und Blobcontainer die Grenzen des Speicherkontos gemeinsam sind. 

- Beachten Sie beim Bereitstellen von Azure-Dateifreigaben die IOPS-Einschränkungen eines Speicherkontos. Im Idealfall würden Sie Dateifreigaben Speicherkonten im Verhältnis 1:1 zuordnen. Dies ist jedoch ggf. aufgrund verschiedener Beschränkungen und Einschränkungen in Ihrer Organisation oder in Azure nicht immer möglich. Wenn es nicht möglich ist, dass nur eine Dateifreigabe in einem Speicherkonto bereitgestellt wird, sollten Sie berücksichtigen, welche Freigaben sehr aktiv sein werden und welche Freigaben weniger aktiv sind, um sicherzustellen, dass die aktivsten Dateifreigaben nicht zusammen im gleichen Speicherkonto gruppiert werden.

- Stellen Sie nur GPv2- und FileStorage-Konten bereit, und aktualisieren Sie GPv1- und klassische Speicherkonten, wenn diese in Ihrer Umgebung vorhanden sind. 

## <a name="identity"></a>Identity
Für den Zugriff auf eine Azure-Dateifreigabe muss der Benutzer der Dateifreigabe authentifiziert sein und über Autorisierung für den Zugriff auf die Freigabe verfügen. Dies erfolgt basierend auf der Identität des Benutzers, der auf die Dateifreigabe zugreift. Azure Files ist in drei Hauptidentitätsanbieter integriert:
- **Lokales Active Directory Domain Services (AD DS oder lokales AD DS)** (Vorschauversion): Azure Storage-Konten können über Domänenbeitritt wie ein Windows Server-Dateiserver oder ein NAS-Gerät mit einem kundeneigenen Active Directory Domain Services verbunden werden. Sie können einen Domänencontroller lokal, auf einer Azure-VM oder sogar als VM bei einem anderen Cloudanbieter bereitstellen. Azure Files ist unabhängig vom Speicherort des Domänencontrollers. Sobald ein Speicherkonto einer Domäne beigetreten ist, kann der Endbenutzer eine Dateifreigabe mit dem Benutzerkonto einbinden, mit dem er sich bei seinem PC angemeldet hat. Bei der AD-basierten Authentifizierung wird das Kerberos-Authentifizierungsprotokoll verwendet.
- **Azure Active Directory Domain Services (Azure AD DS)** : Azure AD DS bietet einen von Microsoft verwalteten Domänencontroller, der für Azure-Ressourcen verwendet werden kann. Der Domänenbeitritt Ihres Speicherkontos zu Azure AD DS bietet ähnliche Vorteile wie der Domänenbeitritt zu einem Active Directory im Besitz eines Kunden. Diese Bereitstellungsoption ist besonders nützlich für Lift-and-Shift-Anwendungsszenarien, die AD-basierte Berechtigungen erfordern. Da Azure AD DS AD-basierte Authentifizierung bereitstellt, verwendet diese Option auch das Kerberos-Authentifizierungsprotokoll.
- **Azure-Speicherkontoschlüssel**: Azure-Dateifreigaben können auch mit einem Azure-Speicherkontoschlüssel bereitgestellt werden. Bei einer solchen Einbindung einer Dateifreigabe wird der Speicherkontoname als Benutzername und der Speicherkontoschlüssel als Kennwort verwendet. Die Verwendung des Speicherkontoschlüssels zum Einbinden der Azure-Dateifreigabe ist praktisch ein Administratorvorgang, da die eingebundene Dateifreigabe über vollständige Berechtigungen für alle Dateien und Ordner auf der Freigabe verfügt, auch wenn diese über ACLs verfügen. Wenn Sie den Speicherkontoschlüssel zum Einbinden über SMB verwenden, wird das NTLMv2-Authentifizierungsprotokoll verwendet.

Für Kunden, die von lokalen Dateiservern migrieren oder neue Dateifreigaben in Azure Files erstellen, die sich wie Windows-Dateiserver oder NAS-Geräte verhalten sollen, ist der Domänenbeitritt des Speicherkontos zum **Active Directory im Besitz des Kunden** die empfohlene Option. Weitere Informationen zum Domänenbeitritt Ihres Speicherkontos zu einem Active Directory im Besitz des Kunden finden Sie unter [Azure Files Active Directory: Übersicht](storage-files-active-directory-overview.md).

Wenn Sie beabsichtigen, den Speicherkontoschlüssel für den Zugriff auf Ihre Azure-Dateifreigaben zu verwenden, empfehlen wir die Verwendung von Dienstendpunkten, wie im Abschnitt [Netzwerk](#networking) beschrieben.

## <a name="networking"></a>Netzwerk
Auf Azure-Dateifreigaben kann über den öffentlichen Endpunkt des Speicherkontos von überall aus zugegriffen werden. Dies bedeutet, dass authentifizierte Anforderungen (etwa Anforderungen, die durch die Anmeldeidentität eines Benutzer autorisiert wurden) auf sichere Weise verwendet werden können – ganz gleich, ob ihr Ursprung innerhalb oder außerhalb von Azure liegt. In vielen Kundenumgebungen ist die Einbindung der Azure-Dateifreigabe in die lokale Arbeitsstation zunächst nicht erfolgreich, obwohl Einbindungen von virtuellen Azure-Computern problemlos funktionieren. Dies liegt daran, dass viele Organisationen und Internetdienstanbieter (Internet Service Providers, ISPs) Port 445 blockieren, der von SMB für die Kommunikation verwendet wird. Um eine Zusammenfassung der ISPs anzuzeigen, die den Zugang von Port 445 aus zulassen oder verweigern, gehen Sie auf [TechNet](https://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

Zum Aufheben der Blockierung des Zugriffs auf Ihre Azure-Dateifreigabe stehen Ihnen zwei Hauptoptionen zur Verfügung:

- Entsperren von Port 445 für das lokale Netzwerk Ihrer Organisation. Auf Azure-Dateifreigaben kann nur extern über den öffentlichen Endpunkt zugegriffen werden, indem internetsichere Protokolle wie SMB 3.0 und die FileREST-API verwendet werden. Dies ist die einfachste Möglichkeit, vom lokalen Standort aus auf Ihre Azure-Dateifreigabe zuzugreifen, da keine erweiterte Netzwerkkonfiguration erforderlich ist, die über das Ändern von Regeln für den Port für ausgehenden Datenverkehr Ihres Unternehmens hinausgeht. Es wird jedoch empfohlen, Legacy- und veraltete Versionen des SMB-Protokolls zu entfernen (also SMB 1.0). Weitere Informationen hierzu finden Sie unter [Sichern von Windows/Windows Server](storage-how-to-use-files-windows.md#securing-windowswindows-server) und [Sichern von Linux](storage-how-to-use-files-linux.md#securing-linux).

- Zugreifen auf Azure-Dateifreigaben über eine ExpressRoute- oder VPN-Verbindung. Wenn Sie über einen Netzwerktunnel auf Ihre Azure-Dateifreigabe zugreifen, können Sie Ihre Azure-Dateifreigabe wie eine lokale Dateifreigabe einbinden, da der SMB-Datenverkehr Ihre Organisationsgrenze nicht überschreitet.   

Obwohl es aus technischer Sicht wesentlich einfacher ist, ihre Azure-Dateifreigaben über den öffentlichen Endpunkt einzubinden, erwarten wir, dass die meisten Kunden ihre Azure-Dateifreigaben über eine ExpressRoute- oder VPN-Verbindung einbinden. Zu diesem Zweck müssen Sie Folgendes für Ihre Umgebung konfigurieren:  

- **Netzwerktunnelung mithilfe von ExpressRoute, Site-to-Site- oder Point-to-Site-VPN**: Das Tunneln in ein virtuelles Netzwerk ermöglicht den Zugriff auf Azure-Dateifreigaben vom lokalen Standort aus, auch wenn Port 445 blockiert ist.
- **Private Endpunkte**: Private Endpunkte stellen Ihrem Speicherkonto eine dedizierte IP-Adresse innerhalb des Adressraums des virtuellen Netzwerks zur Verfügung. Dies ermöglicht Netzwerktunnelung, ohne dass lokale Netzwerke für alle IP-Adressbereiche, die sich im Besitz der Azure-Speichercluster befinden, geöffnet werden müssen. 
- **DNS--Weiterleitung**: Konfigurieren Sie das lokale DNS, um den Namen Ihres Speicherkontos (also `storageaccount.file.core.windows.net` für die öffentlichen Cloudregionen) aufzulösen, um die IP-Adresse Ihrer privaten Endpunkte aufzulösen.

Informationen zum Planen der Netzwerke, die mit der Bereitstellung einer Azure-Dateifreigabe verbunden sind, finden Sie unter [Überlegungen zum Azure Files-Netzwerk](storage-files-networking-overview.md).

## <a name="encryption"></a>Verschlüsselung
Azure Files unterstützt zwei verschiedene Verschlüsselungstypen: Verschlüsselung während der Übertragung, die sich auf die Verschlüsselung bezieht, die beim Einbinden/Zugreifen auf die Azure-Dateifreigabe verwendet wird, sowie die Verschlüsselung ruhender Daten, die sich darauf bezieht, wie die Daten beim Speichern auf einem Datenträger verschlüsselt werden. 

### <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung
Standardmäßig ist in allen Azure-Speicherkonten die Verschlüsselung während der Übertragung aktiviert. Das bedeutet Folgendes: Wenn Sie eine Dateifreigabe über SMB einbinden oder über FileREST darauf zugreifen (per Azure-Portal, PowerShell/CLI oder Azure-SDKs), lässt Azure Files die Verbindung nur dann zu, wenn sie über SMB 3.0 oder höher mit Verschlüsselung oder über HTTPS hergestellt wird. Clients, die SMB 3.0 nicht unterstützen, oder Clients, die zwar SMB 3.0, aber nicht die SMB-Verschlüsselung unterstützen, können die Azure-Dateifreigabe nicht einbinden, wenn die Verschlüsselung während der Übertragung aktiviert ist. Weitere Informationen dazu, welche Betriebssysteme SMB 3.0 mit Verschlüsselung unterstützen, finden Sie in der ausführlichen Dokumentation zu [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md) und [Linux](storage-how-to-use-files-linux.md). Alle aktuellen PowerShell-, CLI- und SDK-Versionen unterstützen HTTPS.  

Sie können die Verschlüsselung während der Übertragung für ein Azure-Speicherkonto deaktivieren. Wenn die Verschlüsselung deaktiviert ist, lässt Azure Files auch SMB 2.1, SMB 3.0 ohne Verschlüsselung und nicht verschlüsselte FileREST-API-Aufrufe über HTTP zu. Der Hauptgrund für die Deaktivierung der Verschlüsselung während der Übertragung ist die Unterstützung einer älteren Anwendung, die unter einem älteren Betriebssystem wie z. B. Windows Server 2008 R2 oder einer älteren Linux-Distribution ausgeführt werden muss. Azure Files lässt nur SMB 2.1-Verbindungen innerhalb der gleichen Region zu, in der sich auch die Azure-Dateifreigabe befindet. Ein SMB 2.1-Client außerhalb der Azure-Region der Azure-Dateifreigabe – z. B. ein lokales System oder eine andere Azure-Region – kann nicht auf die Dateifreigabe zugreifen.

Wir empfehlen dringend, sicherzustellen, dass die Verschlüsselung von Daten während der Übertragung aktiviert ist.

Weitere Informationen zur Verschlüsselung während der Übertragung finden Sie unter [Vorschreiben einer sicheren Übertragung in Azure Storage](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten
[!INCLUDE [storage-files-encryption-at-rest](../../../includes/storage-files-encryption-at-rest.md)]

## <a name="storage-tiers"></a>Speicherebenen
[!INCLUDE [storage-files-tiers-overview](../../../includes/storage-files-tiers-overview.md)]

Im Allgemeinen sind Azure Files-Features und die Interoperabilität mit anderen Diensten für Premium-Dateifreigaben und Standarddateifreigaben identisch. Es gibt jedoch einige wichtige Unterschiede:
- **Abrechnungsmodell**
    - Premium-Dateifreigaben werden über ein bereitgestelltes Abrechnungsmodell abgerechnet. Dies bedeutet, dass Sie für den Speicherplatz bezahlen, den Sie bereitstellen, und nicht für den Speicherplatz, den Sie tatsächlich anfordern. 
    - Standard-Dateifreigaben werden mit einem Modell mit nutzungsbasierter Bezahlung abgerechnet, das die Grundkosten für den Speicherplatz für den tatsächlich genutzten Speicher und dann zusätzliche Transaktionskosten basierend auf der Nutzung der Freigabe umfasst. Mit Standard-Dateifreigaben erhöht sich Ihre Rechnung, wenn Sie die Azure-Dateifreigabe umfangreicher nutzen (Lesen/Schreiben/Einbinden).
- **Redundanzoptionen**
    - Premium-Dateifreigaben sind nur für lokal redundanten (LRS) und zonenredundanten Speicher (ZRS) verfügbar.
    - Standard-Dateifreigaben sind für lokal redundanten, zonenredundanten, georedundanten (GRS) und georedundanten (GZRS) Speicher verfügbar.
- **Maximale Größe einer Dateifreigabe**
    - Premium-Dateifreigaben können ohne zusätzlichen Aufwand für bis zu 100 TiB bereitgestellt werden.
    - Standardmäßig können Standard-Dateifreigaben nur bis zu 5 TiB umfassen. Das Freigabelimit kann jedoch auf 100 TiB erhöht werden, indem das Featureflag *large file share* (große Dateifreigabe) des Speicherkontos verwendet wird. Standard-Dateifreigaben dürfen nur bis zu 100 TiB für lokal redundante oder zonenredundante Speicherkonten umfassen. Weitere Informationen zum Vergrößern von Dateifreigaben finden Sie unter [Aktivieren und Erstellen großer Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-files-how-to-create-large-file-share).
- **Regionale Verfügbarkeit**
    - Premium-Dateifreigaben sind nicht in allen Regionen verfügbar, und zonenredundante Unterstützung ist in einer kleineren Teilmenge von Regionen verfügbar. Um herauszufinden, ob Premium-Dateifreigaben derzeit in Ihrer Region verfügbar sind, lesen Sie die Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=storage) für Azure. Informationen zu den Regionen, in denen ZRS unterstützt wird, finden Sie unter [Unterstützung von Azure-Verfügbarkeitszonen nach Region](../../availability-zones/az-region.md). Damit wir neue Regionen und Premium-Tarif-Features priorisieren können, füllen Sie bitte das Formular dieser [Umfrage](https://aka.ms/pfsfeedback) aus.
    - Standard-Dateifreigaben sind in allen Azure-Regionen verfügbar.
- Azure Kubernetes Service (AKS) unterstützt Premium-Dateifreigaben ab Version 1.13.

Wenn eine Dateifreigabe als Premium- oder als Standard-Dateifreigabe erstellt wurde, können Sie sie nicht automatisch in den anderen Tarif konvertieren. Wenn Sie zum anderen Tarif wechseln möchten, müssen Sie eine neue Dateifreigabe in diesem Tarif erstellen und die Daten von Ihrer ursprünglichen Freigabe manuell in die von Ihnen erstellte neue Freigabe kopieren. Es wird empfohlen, `robocopy` für Windows oder `rsync` für macOS und Linux zu verwenden, um diesen Kopiervorgang auszuführen.

### <a name="understanding-provisioning-for-premium-file-shares"></a>Grundlegendes zur Bereitstellung für Premium-Dateifreigaben
Premium-Dateifreigaben werden basierend auf einem festen Verhältnis aus GiB/IOPS/Durchsatz bereitgestellt. Für jedes bereitgestellte GiB erhält die Freigabe 1 IOPS und einen Durchsatz von 0,1 MiB/s bis zum maximalen Grenzwert pro Freigabe. Die kleinste zulässige Bereitstellung beträgt 100 GiB mit den minimalen Werten für IOPS/Durchsatz.

Auf einer Best-Effort-Basis können alle Freigaben für 60 Minuten oder länger (je nach Größe der Freigabe) auf bis zu 3 IOPS pro GiB an bereitgestelltem Speicher erhöht werden (Burst). Neue Freigaben beginnen mit dem vollständigen Burstguthaben, basierend auf der bereitgestellten Kapazität.

Freigaben müssen in Schritten von 1GiB bereitgestellt werden. Die Mindestgröße beträgt 100 GiB, die nächste Größe ist 101 GiB usw.

> [!TIP]
> IOPS-Grundwert = 1 + 1 * bereitgestellte GiB. (Bis zu 100.000 IOPS).
>
> Burstgrenzwert = 3 * IOPS-Grundwert. (Bis zu 100.000 IOPS).
>
> Ausgangsrate = 60MiB/s + 0,06 * bereitgestellte GiB
>
> Eingangsrate = 40MiB/s + 0,04 * bereitgestellte GiB

Die Größe bereitgestellter Freigaben wird durch ein Freigabekontingent angegeben. Das Freigabekontingent kann jederzeit heraufgesetzt, jedoch erst 24 Stunden nach der letzten Heraufsetzung herabgesetzt werden. Wenn in einer 24-stündigen Wartezeit keine Heraufsetzung aufgetreten ist, können Sie das Freigabekontingent beliebig oft herabsetzen, bis Sie es erneut heraufsetzen. Änderungen von IOPS/Durchsatz werden innerhalb weniger Minuten nach der Größenänderung wirksam.

Es ist möglich, die Größe Ihrer bereitgestellten Freigabe unter Ihre verbrauchten GiB zu reduzieren. Wenn Sie dies tun, gehen Ihnen keine Daten verloren, sondern es wird Ihnen weiterhin die verwendete Größe in Rechnung gestellt. Sie erhalten die Leistung (IOPS-Grundwert, Durchsatz und Burst-IOPS) der bereitgestellten Freigabe, nicht die der verwendeten Größe.

Die folgende Tabelle zeigt einige Beispiele dieser Formeln für die bereitgestellten Freigabengrößen:

|Kapazität (GiB) | IOPS-Grundwert | Burst-IOPS | Ausgehend (MiB/s) | Eingehend (MiB/s) |
|---------|---------|---------|---------|---------|
|100         | 100     | Bis zu 300     | 66   | 44   |
|500         | 500     | Bis zu 1.500   | 90   | 60   |
|1\.024       | 1\.024   | Bis zu 3.072   | 122   | 81   |
|5\.120       | 5\.120   | Bis zu 15.360  | 368   | 245   |
|10.240      | 10.240  | Bis zu 30.720  | 675 | 450   |
|33.792      | 33.792  | Bis zu 100.000 | 2\.088 | 1\.392   |
|51.200      | 51.200  | Bis zu 100.000 | 3\.132 | 2\.088   |
|102.400     | 100.000 | Bis zu 100.000 | 6\.204 | 4\.136   |

> [!NOTE]
> Die Leistung der Dateifreigabe hängt unter anderem von den Computernetzwerklimits, der verfügbaren Netzwerkbandbreite, den E/A-Größen und der Parallelität ab. Beispielsweise kann ein einzelner virtueller Windows-Computer namens *Standard F16s_v2*, der mit einer Premium-Dateifreigabe über SMB verbunden ist, laut internen Tests mit Lese-/Schreibvorgängen mit einer E/A-Größe von 8 KiB 20 K Lese-IOPS und 15 K Schreib-IOPS erzielen. Bei Lese-/Schreibvorgängen mit einer E/A-Größe von 512 MiB kann derselbe virtuelle Computer einen Durchsatz von 1,1 GiB/s ausgehend und 370 MiB/s eingehend erzielen. Um eine maximale Leistung zu erreichen, können Sie die Last auf mehrere VMs verteilen. Weitere Informationen zu gängigen Leistungsproblemen und deren Lösungen finden Sie im [Handbuch zur Problembehandlung](storage-troubleshooting-files-performance.md).

#### <a name="bursting"></a>Bursting
Premium-Dateifreigaben können ihren IOPS-Wert bis zu Faktor drei erhöhen. Bursting wird automatisiert und funktioniert auf Basis eines Guthabensystems. Die Burstübertragung funktioniert auf Best-Effort-Basis, und der Burstgrenzwert ist keine Garantie; bei Dateifreigaben ist eine Burstübertragung *bis zum* Grenzwert möglich.

Guthaben sammeln sich in einem Burstbucket an, wenn Datenverkehr für Ihre Dateifreigabe unterhalb des IOPS-Grundwerts liegt. Beispielsweise hat eine 100GiB-Freigabe 100 IOPS-Grundwerte. Wenn der eigentliche Datenverkehr auf der Freigabe 40IOPS für ein bestimmtes 1-Sekunden-Intervall betrug, werden die 60 nicht verwendeten IOPS einem Burstbucket gutgeschrieben. Diese Guthaben werden dann später verwendet, wenn Vorgänge die IOPS-Grundwerte überschreiten.

> [!TIP]
> Größe des Burstbuckets = IOPS-Grundwert * 2 * 3600.

Wenn eine Freigabe den IOPS-Grundwert überschreitet und Guthaben in einem Burstbucket hat, führt sie Burstübertragungen durch. Freigaben können solange Burstübertragungen durchführen, wie Guthaben übrig sind, aber Freigaben, die kleiner sind als 50TiB, bleiben nur bis zu einer Stunde auf dem Burstgrenzwert. Freigaben, die größer sind als 50TiB, können dieses einstündige Limit technisch überschreiten, bis zu zwei Stunden, aber dies basiert auf der Anzahl der gesammelten Burstguthaben. Jede EA über dem IOPS-Grundwert verbraucht ein Guthaben, und wenn alle Guthaben verbraucht sind, kehrt die Freigabe zum IOPS-Grundwert zurück.

Freigabeguthaben können drei Zustände aufweisen:

- Anwachsend, wenn die Dateifreigabe weniger als den IOPS-Grundwert verwendet.
- Sinkend, wenn die Dateifreigabe Burstübertragungen durchführt.
- Konstant verbleibend, wenn entweder keine Guthaben vorhanden sind oder der IOPS-Grundwert verwendet wird.

Neue Dateifreigaben beginnen mit der vollen Anzahl von Guthaben im Burstbucket. Burstguthaben werden nicht angesammelt, wenn der Freigabe-IOPS aufgrund einer Einschränkung durch den Server unter den IOPS-Grundwert fällt.

### <a name="enable-standard-file-shares-to-span-up-to-100-tib"></a>Aktivieren von Standard-Dateifreigaben für bis zu 100 TiB
[!INCLUDE [storage-files-tiers-enable-large-shares](../../../includes/storage-files-tiers-enable-large-shares.md)]

#### <a name="limitations"></a>Einschränkungen
[!INCLUDE [storage-files-tiers-large-file-share-availability](../../../includes/storage-files-tiers-large-file-share-availability.md)]

## <a name="redundancy"></a>Redundanz
[!INCLUDE [storage-files-redundancy-overview](../../../includes/storage-files-redundancy-overview.md)]

## <a name="migration"></a>Migration
In vielen Fällen werden Sie keine ganz neue Dateifreigabe für Ihre Organisation einrichten, sondern stattdessen eine vorhandene Dateifreigabe von einem lokalen Dateiserver oder NAS-Gerät zu Azure Files migrieren. Für den Erfolg der Migration ist es wichtig, die richtige Migrationsstrategie und das richtige Tool für Ihr Szenario auszuwählen. 

Der[Artikel zur Migrationsübersicht](storage-files-migration-overview.md) behandelt kurz die Grundlagen und enthält eine Tabelle, die Verweise auf Migrationsleitfäden enthält, die Ihr Szenario wahrscheinlich abdecken.

## <a name="next-steps"></a>Nächste Schritte
* [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
* [Bereitstellen von Azure Files](storage-files-deployment-guide.md)
* [Bereitstellen der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md)
* [Ermitteln des Migrationsleitfadens für Ihr Szenario anhand des Artikels zur Migrationsübersicht](storage-files-migration-overview.md)
