---
title: Azure Security Control – Datenschutz
description: Azure Security Control – Datenschutz
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 5482495f87e87e5d05d8adca6b053810a62dcb4e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934515"
---
# <a name="security-control-data-protection"></a>Security Control: Datenschutz

Die Empfehlungen zum Datenschutz konzentrieren sich auf die Behandlung von Problemen im Zusammenhang mit Verschlüsselung, Zugriffssteuerungslisten, identitätsbasierter Zugriffskontrolle und Überwachungsprotokollen für den Datenzugriff.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Verwalten eines Bestands an vertraulichen Informationen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4,1 | Version 13.1 | Kunde |

Verwenden Sie Tags für die Nachverfolgung von Azure-Ressourcen, die vertrauliche Informationen speichern oder verarbeiten.

Erstellen und Verwenden von Tags:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

## <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Isolieren von Systemen, die vertrauliche Informationen speichern oder verarbeiten

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4,2 | 13.2 | Kunde |

Implementieren Sie separate Abonnements und/oder Verwaltungsgruppen für Entwicklungs-, Test- und Produktionsumgebungen. Ressourcen sollten durch ein VNET/Subnetz getrennt, ordnungsgemäß gekennzeichnet und durch eine NSG oder Azure Firewall gesichert werden. Ressourcen, die vertrauliche Daten speichern oder verarbeiten, müssen ausreichend isoliert werden. Implementieren Sie für VMs, auf denen vertrauliche Informationen gespeichert oder verarbeitet werden, Richtlinien und Verfahren, mit denen Sie sie bei Nichtverwendung deaktivieren können.

Erstellen zusätzlicher Azure-Abonnements:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Erstellen von Verwaltungsgruppen:

https://docs.microsoft.com/azure/governance/management-groups/create

Erstellen und Verwenden von Tags:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Erstellen eines virtuellen Netzwerks:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Erstellen einer NSG mit einer Sicherheitskonfiguration:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Bereitstellen von Azure Firewall:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Konfigurieren von „Warnen“ oder „Warnen und ablehnen“ mit Azure Firewall:

https://docs.microsoft.com/azure/firewall/threat-intel

## <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Überwachen und Blockieren einer nicht autorisierten Übertragung vertraulicher Informationen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4.3 | 13.3 | Kunde |

Stellen Sie ein automatisiertes Tool an den Netzwerkperimetern bereit, das die nicht autorisierte Übertragung von sensiblen Informationen überwacht und derartige Übertragungen blockiert und gleichzeitig die Informationssicherheitsexperten benachrichtigt.

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Verschlüsseln aller vertraulichen Informationen während der Übertragung

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4.4 | 14.4 | Shared |

Verschlüsseln Sie alle vertraulichen Informationen während der Übertragung. Stellen Sie sicher, dass alle Clients, die sich mit Ihren Azure-Ressourcen verbinden, TLS 1.2 oder höher aushandeln können.

Befolgen Sie ggf. die Empfehlungen von Azure Security Center zur Verschlüsselung ruhender Daten und zur Verschlüsselung während der Übertragung.

Grundlegendes zur Verschlüsselung während der Übertragung mit Azure:

https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Verwenden eines aktiven Ermittlungstools zur Bestimmung vertraulicher Daten

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4,5 | 14.5 | Kunde |

Wenn für Ihren spezifischen Dienst in Azure keine Funktion verfügbar ist, verwenden Sie ein aktives Ermittlungstools eines Drittanbieters, um alle vertraulichen Informationen zu bestimmen, die von den Technologiesystemen des Unternehmens gespeichert, verarbeitet oder übertragen werden, einschließlich der Systeme, die sich vor Ort oder bei einem Remotedienstanbieter befinden, und um den Bestand an vertraulichen Informationen des Unternehmens zu aktualisieren.

Verwenden Sie Azure Information Protection, um vertrauliche Informationen in Office 365-Dokumenten zu bestimmen.

Verwenden Sie Azure SQL Information Protection, um die Klassifizierung und Bezeichnung von Informationen zu unterstützen, die in Azure SQL-Datenbanken gespeichert sind.

Implementieren von Azure SQL Data Discovery:

https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification

Implementieren von Azure Information Protection:

https://docs.microsoft.com/azure/information-protection/deployment-roadmap

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Verwenden von Azure RBAC zum Steuern des Zugriffs auf Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4.6 | 14.6 | Kunde |

Verwenden Sie Azure AD-RBAC, um den Zugriff auf Daten und Ressourcen zu steuern, und verwenden Sie andernfalls dienstspezifische Zugriffssteuerungsmethoden.

Grundlegendes zu Azure RBAC:

https://docs.microsoft.com/azure/role-based-access-control/overview

Konfigurieren von RBAC in Azure:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

## <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Verwenden der hostbasierten Verhinderung von Datenverlusten zum Erzwingen der Zugriffssteuerung

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4,7 | 14.7 | Kunde |

Implementieren Sie ein Drittanbietertool, z. B. eine automatisierte Lösung für die Verhinderung von Datenverlust, um Zugriffssteuerungen für Daten zu erzwingen, auch wenn Daten von einem System kopiert werden.

## <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Verschlüsseln vertraulicher, ruhender Informationen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4.8 | 14.8 | Kunde |

Verwenden Sie die Verschlüsselung ruhender Daten für alle Azure-Ressourcen. Microsoft empfiehlt, Azure die Verwaltung Ihrer Verschlüsselungsschlüssel zu erlauben. Es gibt jedoch in einigen Fällen die Möglichkeit, ihre eigenen Schlüssel zu verwalten. 

Grundlegendes zur Verschlüsselung ruhender Daten in Azure:

https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest

Konfigurieren der von Kunden verwalteten Verschlüsselungsschlüssel:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Protokollieren und Warnen bei Änderungen an wichtigen Azure-Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 4,9 | 14.9 | Kunde |

Verwenden Sie Azure Monitor mit dem Azure-Aktivitätsprotokoll, um Warnungen zu erstellen, die bei Änderungen an wichtigen Azure-Ressourcen ausgegeben werden.

Erstellen von Warnungen für Azure-Aktivitätsprotokollereignisse:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der nächsten Sicherheitskontrolle: [Verwaltung von Sicherheitsrisiken](security-control-vulnerability-management.md)
