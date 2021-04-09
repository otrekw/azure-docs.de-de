---
title: Neues in Azure NetApp Files | Microsoft-Dokumentation
description: Bietet eine Übersicht über die neuesten Features und Verbesserungen in Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/19/2021
ms.author: b-juche
ms.openlocfilehash: f2167159b03cd0387acfccf4bbd0a2e840f739df
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "104955227"
---
# <a name="whats-new-in-azure-netapp-files"></a>Neues in Azure NetApp Files

Azure NetApp Files wird regelmäßig aktualisiert. Dieser Artikel bietet eine Übersicht über die neuesten Features und Verbesserungen. 

## <a name="march-2021"></a>März 2021

* [SMB-Freigaben für fortlaufende Verfügbarkeit (Continuous Availability, CA)](azure-netapp-files-create-volumes-smb.md#add-an-smb-volume) (Vorschau)  

    SMB Transparent Failover ermöglicht Wartungsvorgänge für den Azure NetApp Files-Dienst ohne Unterbrechung der Verbindung mit Serveranwendungen, die Daten auf SMB-Volumes speichern und auf diese Daten zugreifen. Zur Unterstützung von SMB Transparent Failover unterstützt Azure NetApp Files jetzt die Option für SMB-Freigaben für fortlaufende Verfügbarkeit zur Verwendung mit SQL Server-Anwendungen über SMB, die auf virtuellen Azure-Computern ausgeführt werden. Dieses Feature wird derzeit für SQL Server unter Windows unterstützt. SQL Server unter Linux wird zurzeit nicht unterstützt. Die Aktivierung dieses Features steigert die SQL Server-Leistung erheblich und bietet Skalierungs- und Kostenvorteile für [Bereitstellungen von einzelnen Instanzen, Always On-Failoverclusterinstanzen und Always On-Verfügbarkeitsgruppen](azure-netapp-files-solution-architectures.md#sql-server). Weitere Informationen finden Sie unter [Vorteile der Verwendung von Azure NetApp Files für die SQL Server-Bereitstellung](solutions-benefits-azure-netapp-files-sql-server.md).

* [Automatische Größenänderung eines regionsübergreifenden Replikationszielvolumes](azure-netapp-files-resize-capacity-pools-or-volumes.md#resize-a-cross-region-replication-destination-volume)

    In einer regionsübergreifenden Replikationsbeziehung wird die Größe eines Zielvolumes basierend auf der Größe des Quellvolumes automatisch angepasst. Daher müssen Sie die Größe des Zielvolumes nicht separat ändern. Dieses Verhalten zur automatischen Größenänderung gilt, wenn sich die Volumes in einer aktiven Replikationsbeziehung befinden oder wenn das Replikationspeering mit dem Vorgang zur Neusynchronisierung unterbrochen wird. Damit dieses Feature funktioniert, müssen Sie einen ausreichenden Toleranzbereich in den Kapazitätspools für die Quell- und Zielvolumes sicherstellen.

## <a name="december-2020"></a>Dezember 2020

* [Tool für konsistente Momentaufnahmen in Azure-Anwendungen](azacsnap-introduction.md) (Vorschau)    

    Das Tool für konsistente Momentaufnahmen in Azure-Anwendungen (Azure Application Consistent Snapshot Tool, AzAcSnap) ist ein Befehlszeilentool, mit dem Sie den Schutz von Daten für Drittanbieterdatenbanken (SAP HANA) in Linux-Umgebungen (z. B. SUSE und RHEL) vereinfachen können.   

    Für AzAcSnap werden die Funktionen für Volumemomentaufnahmen und die Replikation von Azure NetApp Files und Azure (große Instanz) genutzt. Ihnen bieten sich folgende Vorteile:

    * Anwendungskonsistenter Schutz von Daten 
    * Datenbankkatalog-Verwaltung 
    * *Ad-hoc*-Schutz von Volumes 
    * Klonen von Speichervolumes 
    * Unterstützung der Notfallwiederherstellung 

## <a name="november-2020"></a>November 2020

* [Momentaufnahme wiederherstellen](azure-netapp-files-manage-snapshots.md#revert-a-volume-using-snapshot-revert)

    Mit der Funktion zum Wiederherstellen von Momentaufnahmen können Sie schnell ein Volume wieder in den Zustand versetzen, in dem es sich befand, als eine bestimmte Momentaufnahme erstellt wurde. In den meisten Fällen ist das Wiederherstellen eines Volumes wesentlich schneller als das Wiederherstellen einzelner Dateien aus einer Momentaufnahme in das aktive Dateisystem. Im Vergleich zur Wiederherstellung einer Momentaufnahme auf einem neuen Volume ist es auch wesentliche effizienter hinsichtlich des Speicherplatzes.

## <a name="september-2020"></a>September 2020

* [Regionsübergreifende Replikation in Azure NetApp Files](cross-region-replication-introduction.md) (Vorschau)

  Azure NetApp Files unterstützt jetzt regionsübergreifende Replikation. Mit dieser neuen Notfallwiederherstellungsfunktion können Sie Ihre Azure NetApp Files-Volumes schnell und kostengünstig von einer Azure-Region in eine andere replizieren und so Ihre Daten vor unvorhersehbaren regionalen Ausfällen schützen. Die regionsübergreifende Replikation in Azure NetApp Files nutzt die SnapMirror®-Technologie, bei der nur geänderte Blöcke in einem komprimierten, effizienten Format über das Netzwerk gesendet werden. Diese proprietäre Technologie minimiert die Menge an Daten, die regionsübergreifend repliziert werden muss, wodurch Datenübertragungskosten eingespart werden. Auch die Replikationszeit verkürzt sich dadurch, sodass Sie eine kürzere Recovery Point Objective (RPO) erreichen können.

* [Manueller QoS-Kapazitätspool](manual-qos-capacity-pool-introduction.md) (Vorschau)  

    Bei einem manuellen QoS-Kapazitätspool können Sie die Kapazität und den Durchsatz für ein Volume unabhängig voneinander zuweisen. Der Gesamtdurchsatz aller Volumes, die mit einem Kapazitätspool mit dem QoS-Typ „Manuell“ erstellt werden, ist durch den Gesamtdurchsatz des Pools begrenzt. Dieser wird anhand der Kombination von Poolgröße und Serviceleveldurchsatz ermittelt. Alternativ kann der [QoS-Typ](azure-netapp-files-understand-storage-hierarchy.md#qos_types) eines Kapazitätspools „auto“ (automatisch) sein, wobei es sich um die Standardeinstellung handelt. In einem automatischen QoS-Kapazitätspool wird der Durchsatz den Volumes im Pool automatisch zugewiesen. Dies erfolgt proportional zum Größenkontingent, das den Volumes zugewiesen ist.

* [LDAP-Signatur](azure-netapp-files-create-volumes-smb.md) (Vorschau)   

    Azure NetApp Files unterstützt jetzt LDAP-Signatur für sichere LDAP-Lookups zwischen dem Azure NetApp Files-Dienst und den benutzerseitig angegebenen Active Directory Domain Services-Domänencontrollern. Diese Funktion steht derzeit als Vorschau zur Verfügung.

* [AES-Verschlüsselung für AD-Authentifizierung](azure-netapp-files-create-volumes-smb.md) (Vorschau)

    Azure NetApp Files unterstützt jetzt AES-Verschlüsselung bei LDAP-Verbindungen mit dem Domänencontroller, um die AES-Verschlüsselung für ein SMB-Volume zu ermöglichen. Diese Funktion steht derzeit als Vorschau zur Verfügung. 

* Neue [Metriken](azure-netapp-files-metrics.md):   

    * Neue Volumemetriken: 
        * *Zugeordnete Größe des Volumes*: Die bereitgestellte Größe eines Volumes
    * Neue Poolmetriken: 
        * *Zugeordnete Poolgröße*: Die bereitgestellte Größe des Pools. 
        * *Gesamtgröße der Momentaufnahme für den Pool*: Die Summe der Momentaufnahmegrößen aller Volumes in dem Pool.

## <a name="july-2020"></a>Juli 2020

* [Doppelprotokollvolume (NFSv3 und SMB)](create-volumes-dual-protocol.md)

    Sie können jetzt ein Azure NetApp Files-Volume erstellen, das den gleichzeitigen Doppelprotokollzugriff (NFS v3 und SMB) mit Unterstützung für LDAP-Benutzerzuordnung ermöglicht. Diese Funktion ermöglicht Anwendungsfälle, bei denen Sie eine Linux-basierte Workload haben können, die Daten in einem Azure NetApp Files-Volume generiert und speichert. Gleichzeitig müssen Ihre Mitarbeiter Windows-basierte Clients und Software verwenden, um die neu generierten Daten von demselben Azure NetApp Files-Volume zu analysieren. Durch die Funktion für den gleichzeitigen Doppelprotokollzugriff entfällt die Notwendigkeit, die von der Workload generierten Daten zur Nachanalyse mit einem anderen Protokoll auf ein separates Volume zu kopieren, was Speicherkosten und Betriebszeit spart. Diese Funktion ist kostenlos (die normalen [Azure NetApp Files-Speicherkosten](https://azure.microsoft.com/pricing/details/netapp/) fallen weiterhin an) und allgemein verfügbar. Weitere Informationen finden Sie in der [Dokumentation zum gleichzeitigen Doppelprotokollzugriff](create-volumes-dual-protocol.MD).

* [NFS v4.1 Kerberos-Verschlüsselung während der Übertragung](configure-kerberos-encryption.MD)

    Azure NetApp Files unterstützt jetzt die NFS-Clientverschlüsselung in Kerberos-Modi (krb5, krb5i und krb5p) mit AES-256-Verschlüsselung, was Ihnen zusätzliche Datensicherheit bietet. Diese Funktion ist kostenlos (die normalen [Azure NetApp Files-Speicherkosten](https://azure.microsoft.com/pricing/details/netapp/) fallen weiterhin an) und allgemein verfügbar. Weitere Informationen finden Sie in der [Dokumentation zur NFS v4.1 Kerberos-Verschlüsselung](configure-kerberos-encryption.MD).

* [Dynamische Änderung des Servicelevels von Volumes](dynamic-change-volume-service-level.MD)

    Die Cloud verspricht Flexibilität bei den IT-Ausgaben. Sie können den Servicelevel eines vorhandenen Azure NetApp Files-Volumes jetzt ändern, indem Sie das Volume in einen anderen Kapazitätspool verschieben, der den von Ihnen für das Volume gewünschten Servicelevel verwendet. Diese direkte Änderung des Servicelevels für das Volume erfordert keine Migration von Daten. Auch der Zugriff auf das Volume auf Datenebene wird hiervon nicht beeinträchtigt. Sie können ein vorhandenes Volume so ändern, dass es einen höheren Servicelevel verwendet, um die Leistung zu verbessern, oder so dass es einen niedrigeren Servicelevel verwendet, um die Kosten zu optimieren. Diese Funktion ist kostenlos (die normalen [Azure NetApp Files-Speicherkosten](https://azure.microsoft.com/pricing/details/netapp/) fallen weiterhin an) und befindet sich derzeit in der öffentlichen Vorschauphase. Sie können sich für die Featurevorschau registrieren, indem Sie der [Dokumentation zur dynamischen Änderung des Servicelevels](dynamic-change-volume-service-level.md) folgen.

* [Volumemomentaufnahme-Richtlinie](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) (Vorschau) 

    Mit Azure NetApp Files können Sie Zeitpunktmomentaufnahmen Ihrer Volumes erstellen. Sie können jetzt eine Momentaufnahmerichtlinie erstellen, damit Azure NetApp Files automatisch Volumemomentaufnahmen mit einer Häufigkeit Ihrer Wahl erstellt. Sie können die Erstellung der Momentaufnahmen in stündlichen, täglichen, wöchentlichen oder monatlichen Zyklen planen. Außerdem können Sie als Bestandteil der Momentaufnahmerichtlinie die maximale Anzahl aufzubewahrender Momentaufnahmen angeben. Diese Funktion ist kostenlos (die normalen [Azure NetApp Files-Speicherkosten](https://azure.microsoft.com/pricing/details/netapp/) fallen weiterhin an) und befindet sich derzeit in der Vorschauphase. Sie können sich für die Featurevorschau registrieren, indem Sie der [Dokumentation für Volumemomentaufnahme-Richtlinien](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) folgen.

* [Exportrichtlinie mit NFS-Stammzugriff](azure-netapp-files-configure-export-policy.md)

    Sie können in Azure NetApp Files jetzt angeben, ob das Stammkonto auf das Volume zugreifen kann. 

* [Momentaufnahmepfad ausblenden](azure-netapp-files-manage-snapshots.md#restore-a-file-from-a-snapshot-using-a-client)

    Sie können in Azure NetApp Files jetzt angeben, ob ein Benutzer das Verzeichnis `.snapshot` (NFS-Clients) oder den Ordner `~snapshot` (SMB-Clients) auf einem eingebundenen Volume anzeigen und darauf zugreifen kann.

## <a name="may-2020"></a>Mai 2020

* [Microsoft Azure Backup-Richtlinienbenutzer](create-active-directory-connections.md) (Vorschau)

    Sie können in Azure NetApp Files jetzt weitere Konten einschließen, die erhöhte Rechte für das für Azure NetApp Files erstellte Computerkonto erfordern. Die angegebenen Konten dürfen die NTFS-Berechtigungen auf Datei- oder Ordnerebene ändern. Beispielsweise können Sie ein nicht privilegiertes Dienstkonto angeben, das zum Migrieren von Daten zu einer SMB-Dateifreigabe in Azure NetApp Files verwendet wird. Das Feature Sicherungsrichtlinienbenutzer steht derzeit als Vorschau zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
* [Was ist Azure NetApp Files?](azure-netapp-files-introduction.md)
* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md) 
