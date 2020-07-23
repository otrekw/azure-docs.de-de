---
title: Übersicht über Sicherheitsfeatures
description: Erfahren Sie mehr über die Sicherheitsfunktionen in Azure Backup, die Ihnen helfen, Ihre Sicherungsdaten zu schützen und die Sicherheitsanforderungen Ihres Unternehmens zu erfüllen.
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: ce6d8a43b48be5189f0459c9f82c69354f40689f
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513200"
---
# <a name="overview-of-security-features-in-azure-backup"></a>Übersicht über Sicherheitsfeatures in Azure Backup

Einer der wichtigsten Schritte zum Schutz Ihrer Daten besteht darin, eine zuverlässige Sicherungsinfrastruktur anzuwenden. Es ist jedoch genauso wichtig, dafür zu sorgen, dass Ihre Daten auf sichere Weise gesichert werden und dass Ihre Sicherungen jederzeit geschützt sind. Azure Backup bietet Sicherheit für Ihre Sicherungsumgebung – sowohl während der Übertragung als auch bei ruhenden Daten. In diesem Artikel finden Sie Sicherheitsfunktionen von Azure Backup, die Ihnen helfen, Ihre Sicherungsdaten zu schützen und die Sicherheitsanforderungen Ihres Unternehmens zu erfüllen.

## <a name="management-and-control-of-identity-and-user-access"></a>Verwalten und Steuern von Identitäten und Benutzerzugriff

Von Recovery Services-Tresoren verwendete Speicherkonten sind isoliert, sodass böswillige Akteure keinen Zugriff darauf haben. Der Zugriff ist nur über Azure Backup-Verwaltungsvorgänge, z. B. eine Wiederherstellung, zulässig. Mit Azure Backup können Sie die verwalteten Vorgänge über eine präzise Zugriffsverwaltung mithilfe der [rollenbasierten Zugriffssteuerung in Azure (RBAC)](./backup-rbac-rs-vault.md) steuern. Mithilfe von RBAC können Sie Aufgaben in Ihrem Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen.

Azure Backup bietet drei [integrierte Rollen](../role-based-access-control/built-in-roles.md), um Vorgänge der Sicherungsverwaltung zu steuern:

* Mitwirkender für Sicherungen: Erstellen und Verwalten von Sicherungen, außer Löschen von Recovery Services-Tresoren und Gewähren des Zugriffs für andere Personen
* Sicherungsoperator: alle Berechtigungen eines Mitwirkenden, außer Entfernen von Sicherungen und Verwalten von Sicherungsrichtlinien
* Sicherungsleser: Berechtigungen zum Anzeigen aller Vorgänge für die Sicherungsverwaltung

Weitere Informationen finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup](./backup-rbac-rs-vault.md).

Azure Backup verfügt über mehrere integrierte Sicherheitskontrollen, um Sicherheitsrisiken zu verhindern, zu erkennen und darauf zu reagieren. Erfahren Sie mehr über [Sicherheitskontrollen für Azure Backup](./backup-security-controls.md).

## <a name="separation-between-guest-and-azure-storage"></a>Trennung zwischen Gast und Azure Storage

Bei Azure Backup, das die Sicherung virtueller Computer und SQL und SAP HANA in VM-Sicherungen umfasst, werden die Sicherungsdaten in Azure Storage gespeichert, und ein Gast hat keinen direkten Zugriff auf den Sicherungsspeicher oder dessen Inhalt.  Bei der Sicherung virtueller Computer wird die Erstellung und Speicherung von Sicherungsmomentaufnahmen durch Azure-Fabric ausgeführt, wobei ein Gast nur im Deaktivieren der Workload für anwendungskonsistente Sicherungen involviert ist.  Mit SQL und SAP HANA erhält die Sicherungserweiterung temporären Schreibzugriff auf bestimmte Blobs.  Auf diese Weise können vorhandene Sicherungen auch in einer kompromittierten Umgebung nicht manipuliert oder von einem Gast gelöscht werden.

## <a name="internet-connectivity-not-required-for-azure-vm-backup"></a>Keine Internetkonnektivität für Azure-VM-Sicherungen erforderlich

Die Sicherung von virtuellen Azure-Computern erfordert das Verschieben von Daten vom Datenträger Ihres virtuellen Computers in den Recovery Services-Tresor. Die gesamte dazu erforderliche Kommunikation und Datenübertragung erfolgt jedoch ausschließlich im Azure-Backbone-Netzwerk, ohne dass auf Ihr virtuelles Netzwerk zugegriffen werden muss. Daher müssen Sie bei der Sicherung von virtuellen Azure-Computern, die sich in geschützten Netzwerken befinden, keinen Zugriff auf IP-Adressen oder FQDNs gewähren.

## <a name="private-endpoints-for-azure-backup"></a>Private Endpunkte für Azure Backup

Sie können nun [private Endpunkte](../private-link/private-endpoint-overview.md) verwenden, um Ihre Daten von Servern in einem virtuellen Netzwerk sicher in Ihrem Recovery Services-Tresor zu sichern. Der private Endpunkt verwendet eine IP-Adresse aus dem VNET-Adressraum für Ihren Tresor, sodass Sie die virtuellen Netzwerke nicht für öffentliche IP-Adressen verfügbar machen müssen. Private Endpunkte können zum Sichern und Wiederherstellen von SQL- und SAP HANA-Datenbanken verwendet werden, die auf Ihren virtuellen Azure-Computern ausgeführt werden. Sie können auch für lokale Server mit dem MARS-Agent verwendet werden.

Weitere Informationen zu privaten Endpunkten für Azure Backup finden Sie [hier](./private-endpoints.md).

## <a name="encryption-of-data-in-transit-and-at-rest"></a>Verschlüsselung von Daten während der Übertragung und im Ruhezustand

Die Verschlüsselung schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. In Azure werden Daten bei der Übertragung zwischen Azure Storage und dem Tresor per HTTPS geschützt. Diese Daten bleiben im Azure-Backbone-Netzwerk.

* Sicherungsdaten werden automatisch mit von Microsoft verwalteten Schlüsseln verschlüsselt. Sie können auch die gesicherten virtuellen Computer mit verwaltetem Datenträger im Recovery Services-Tresor verschlüsseln, indem Sie [vom Kunden verwaltete Schlüssel](backup-encryption.md#encryption-of-backup-data-using-customer-managed-keys) verwenden, die in Azure Key Vault gespeichert sind. Sie müssen keine besonderen Maßnahmen ergreifen, um diese Verschlüsselung zu aktivieren. Dies gilt für alle Workloads, die in Ihrem Recovery Services-Tresor gesichert werden.

* Azure Backup unterstützt die Sicherung und Wiederherstellung von Azure-VMs, deren Datenträger für Betriebssystem/Daten mit Azure Disk Encryption (ADE) verschlüsselt wurden. [Erfahren Sie mehr über verschlüsselte Azure-VMs und Azure Backup](./backup-azure-vms-encryption.md).

## <a name="protection-of-backup-data-from-unintentional-deletes"></a>Schutz von Sicherungsdaten vor unbeabsichtigten Löschungen

Azure Backup verfügt über Sicherheitsfeatures für den Schutz von Sicherungsdaten auch nach dem Löschen. Beim vorläufigen Löschen werden die Sicherungsdaten 14 Tage länger aufbewahrt, damit das jeweilige Sicherungselement auch dann ohne Datenverluste wiederhergestellt werden kann, wenn ein Benutzer die Sicherung einer VM löscht. Für die zusätzlichen 14 Tage der Aufbewahrung von Sicherungsdaten mit dem Status „Vorläufiges Löschen“ fallen für Kunden keine Kosten an. [Erfahren Sie mehr über das vorläufige Löschen](backup-azure-security-feature-cloud.md).

## <a name="monitoring-and-alerts-of-suspicious-activity"></a>Überwachung und Warnungen zu verdächtigen Aktivitäten

Azure Backup bietet [integrierte Überwachungs- und Warnungsfunktionen](./backup-azure-monitoring-built-in-monitor.md) zum Anzeigen und Konfigurieren von Aktionen für Ereignisse im Zusammenhang mit Azure Backup. [Sicherungsberichte](./configure-reports.md) stellen den zentralen Ort für die Nachverfolgung der Nutzung, das Überwachen von Sicherungen und Wiederherstellungen und die Identifizierung wichtiger Trends mit unterschiedlicher Granularität dar. Mithilfe der Überwachungs- und Berichterstellungstools von Azure Backup können Sie über alle nicht autorisierten, verdächtigen oder bösartigen Aktivitäten benachrichtigt werden, sobald sie auftreten.

## <a name="security-features-to-help-protect-hybrid-backups"></a>Sicherheitsfeatures für den Schutz von Hybridsicherungen

Azure Backup-Dienst verwendet den Microsoft Azure Recovery Services-Agent (MARS) zum Sichern und Wiederherstellen von Dateien, Ordnern und des Volume- oder Systemstatus von einem lokalen Computer in Azure. MARS bietet nun Sicherheitsfeatures für den Schutz von Hybridsicherungen. Zu den Features zählen:

* Es wurde eine zusätzliche Authentifizierungsebene hinzugefügt, die bei allen kritischen Vorgängen wie Passphrasenänderungen zum Tragen kommt. Durch diese Überprüfung wird gewährleistet, dass solche Vorgänge nur von Benutzern durchgeführt werden können, die über gültige Azure-Anmeldeinformationen verfügen. [Erfahren Sie mehr über die Features zur Verhinderung von Angriffen](./backup-azure-security-feature.md#prevent-attacks).

* Gelöschte Sicherungsdaten werden für zusätzliche 14 Tage ab dem Löschdatum aufbewahrt. So wird gewährleistet, dass die Daten innerhalb des vorgegebenen Zeitraums wiederhergestellt werden können und auch bei Angriffen keine Datenverluste auftreten. Zum Schutz vor beschädigten Daten werden darüber hinaus auch mehr Mindestwiederherstellungspunkte gepflegt. [Erfahren Sie mehr über das Wiederherstellen von gelöschten Sicherungsdaten](./backup-azure-security-feature.md#recover-deleted-backup-data).

* Für Daten, die mit dem Microsoft Azure Recovery Services-Agent (MARS) gesichert werden, wird eine Passphrase verwendet, um sicherzustellen, dass die Daten vor dem Hochladen in Azure Backup verschlüsselt und erst nach dem Herunterladen von Azure Backup entschlüsselt werden. Die Informationen zur Passphrase sind nur für den Benutzer, der die Passphrase erstellt hat, und den Agent verfügbar, der mit ihr konfiguriert wurde. Es werden keinerlei Informationen an den Dienst übertragen oder für diesen freigegeben. Dadurch wird die umfassende Sicherheit Ihrer Daten gewährleistet, da alle Daten, die versehentlich offengelegt werden (z. B. bei einem Man-in-the-Middle-Angriff im Netzwerk), ohne die Passphrase unbrauchbar sind und die Passphrase nicht im Netzwerk übertragen wird.

## <a name="compliance-with-standardized-security-requirements"></a>Compliance mit standardisierten Sicherheitsanforderungen

Um Organisationen bei der Einhaltung nationaler und branchenspezifischer Anforderungen hinsichtlich der Erfassung und Nutzung personenbezogener Daten zu helfen, bieten Microsoft Azure und Azure Backup das umfassendste Angebot an Zertifizierungen und Nachweisen aller Clouddienstanbieter. [Liste der Compliancezertifizierungen](compliance-offerings.md)

## <a name="next-steps"></a>Nächste Schritte

* [Sicherheitsfeatures für den Schutz von Cloudworkloads mit Azure Backup](backup-azure-security-feature-cloud.md)
* [Sicherheitsfeatures für den Schutz von Hybridsicherungen mit Azure Backup](backup-azure-security-feature.md)
