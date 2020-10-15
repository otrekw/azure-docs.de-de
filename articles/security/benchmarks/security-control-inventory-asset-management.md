---
title: Azure-Sicherheitskontrolle – Bestands- und Ressourcenverwaltung
description: Azure-Sicherheitskontrolle – Bestands- und Ressourcenverwaltung
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: aece7278d134145e00b027c184f3817d5566e1e5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81408360"
---
# <a name="security-control-inventory-and-asset-management"></a>Sicherheitskontrolle: Bestands- und Ressourcenverwaltung

Die Empfehlungen für die Bestands- und Ressourcenverwaltung konzentrieren sich auf die Bewältigung von Problemen im Zusammenhang mit der aktiven Verwaltung (Inventarisierung, Nachverfolgung und Korrektur) aller Azure-Ressourcen, sodass nur autorisierte Ressourcen Zugriff erhalten und nicht autorisierte und nicht verwaltete Ressourcen identifiziert und entfernt werden.

## <a name="61-use-automated-asset-discovery-solution"></a>6.1: Verwenden der automatisierten Asset Discovery-Lösung

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.1 | 1.1, 1.2, 1.3, 1.4, 9.1, 12.1 | Kunde |

Verwenden Sie Azure Resource Graph, um alle Ressourcen (z. B. Computeressourcen, Speicher, Netzwerke, Ports und Protokolle usw.) in ihren Abonnements abzufragen bzw. zu ermitteln.  Stellen Sie entsprechende (Lese-) Berechtigungen in Ihrem Mandanten sicher, und zählen Sie alle Azure-Abonnements sowie Ressourcen in ihren Abonnements auf.

Obwohl klassische Azure-Ressourcen über das Resource Graph ermittelt werden können, wird dringend empfohlen, Azure Resource Manager-Ressourcen zu erstellen und zu verwenden.

- [Erstellen von Abfragen mit Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

- [Anzeigen Ihrer Azure-Abonnements](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Grundlegendes zu Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)

## <a name="62-maintain-asset-metadata"></a>6.2: Verwalten von Ressourcenmetadaten

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.2 | 1.5 | Kunde |

Wenden Sie Tags auf Ihre Azure-Ressourcen an, die Metadaten erzeugen, um sie logisch in einer Taxonomie zu organisieren.

- [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="63-delete-unauthorized-azure-resources"></a>6.3: Löschen nicht autorisierter Azure-Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.3 | 1.6 | Kunde |

Verwenden Sie nach Bedarf Tagging, Verwaltungsgruppen und separate Abonnements zum Organisieren und Nachverfolgen von Ressourcen. Stimmen Sie den Bestand regelmäßig ab, und stellen Sie sicher, dass nicht autorisierte Ressourcen rechtzeitig aus dem Abonnement gelöscht werden.

- [Erstellen zusätzlicher Azure-Abonnements](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Erstellen von Verwaltungsgruppen](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Erstellen und Verwenden von Tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4: Definieren und Verwalten eines Bestands an genehmigten Azure-Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.4 | 2.1 | Kunde |

Erstellen Sie einen Bestand an genehmigten Azure-Ressourcen und genehmigter Software für Computeressourcen entsprechend den Anforderungen Ihrer Organisation.

## <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Überwachung auf nicht genehmigte Azure-Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6,5 | 2.3, 2.4 | Kunde |

Verwenden Sie Azure Policy, um den Typ der Ressourcen, die in ihren Abonnements erstellt werden können, einzuschränken.

Verwenden Sie Azure Resource Graph, um Ressourcen in ihren Abonnements abzufragen und zu ermitteln.  Stellen Sie sicher, dass alle in der Umgebung vorhandenen Azure-Ressourcen genehmigt sind.

- [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Erstellen von Abfragen mit Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

## <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Überwachen auf nicht genehmigte Softwareanwendungen innerhalb von Computeressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.6 | 2.3, 2.4 | Kunde |

Verwenden Sie den Bestand virtueller Azure-Computer, um die Erfassung von Informationen zur gesamten Software auf virtuellen Computern zu automatisieren. Softwarename, Version, Herausgeber und Aktualisierungszeit sind im Azure-Portal verfügbar. Um Zugriff auf das Installationsdatum und andere Informationen zu erhalten, aktivieren Sie die Diagnose auf Gastebene, und fügen Sie die Windows-Ereignisprotokolle einem Log Analytics-Arbeitsbereich hinzu.

- [Aktivieren des Bestands virtueller Azure-Computer](https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software)

## <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Entfernen nicht genehmigter Azure-Ressourcen und Softwareanwendungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.7 | 2.5 | Kunde |

Verwenden Sie in Azure Security Center die Dateiintegritätsüberwachung (Änderungsnachverfolgung) und den VM-Bestand, um die gesamte auf virtuellen Computern installierte Software zu identifizieren. Sie können einen eigenen Prozess zum Entfernen nicht autorisierter Software implementieren. Sie können auch eine Drittanbieterlösung verwenden, um nicht genehmigte Software zu identifizieren.

- [Verwenden der Dateiintegritätsüberwachung](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring#using-file-integrity-monitoring)

- [Grundlegendes zur Azure-Änderungsnachverfolgung](https://docs.microsoft.com/azure/automation/change-tracking)

- [Aktivieren des Bestands an virtuellen Azure-Computern](https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software)

## <a name="68-use-only-approved-applications"></a>6.8: Ausschließliche Verwendung genehmigter Anwendungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6,8 | 2.6 | Kunde |

Verwenden Sie die adaptiven Anwendungssteuerungen in Azure Security Center, um sicherzustellen, dass nur autorisierte Software ausgeführt und die Ausführung jeglicher nicht autorisierter Software in Azure Virtual Machines blockiert wird.

- [Verwenden der adaptiven Anwendungssteuerungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

## <a name="69-use-only-approved-azure-services"></a>6.9: Ausschließliche Verwendung genehmigter Azure-Dienste

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.9 | 2.6 | Kunde |

Verwenden Sie Azure Policy, um einzuschränken, welche Dienste in Ihrer Umgebung bereitgestellt werden können.

- [Konfigurieren und Verwalten von Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Ablehnen eines bestimmten Ressourcentyps mit Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

## <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10: Verwalten eines Bestands an genehmigten Softwaretiteln

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.10 | 2.7 | Kunde |

Verwenden Sie die adaptiven Anwendungssteuerungen in Azure Security Center, um festzulegen, auf welche Dateitypen eine Regel angewendet oder nicht angewendet werden kann.

Implementieren Sie andernfalls eine Drittanbieterlösung, wenn dies die Anforderung nicht erfüllt.

- [Verwenden der adaptiven Anwendungssteuerungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

## <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: Einschränken der Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.11 | 2,9 | Kunde |

Verwenden Sie den bedingten Azure-Zugriff, um die Möglichkeiten von Benutzern zur Interaktion mit Azure Resource Manager einzuschränken, indem Sie „Zugriff blockieren“ für die App zur „Verwaltung von Microsoft Azure“ konfigurieren.

- [Konfigurieren des bedingten Zugriffs, um den Zugriff auf Azure Resource Manager zu blockieren](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

## <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Einschränken der Möglichkeiten der Benutzer, Skripte innerhalb von Computeressourcen auszuführen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.12 | 2,9 | Kunde |

Abhängig vom Skripttyp können Sie betriebssystemspezifische Konfigurationen oder Ressourcen von Drittanbietern verwenden, um die Möglichkeit der Benutzer zur Skriptausführung innerhalb von Azure-Computeressourcen einzuschränken.  Sie können auch die adaptiven Anwendungssteuerungen in Azure Security Center nutzen, um sicherzustellen, dass nur autorisierte Software ausgeführt und die Ausführung jeglicher nicht autorisierter Software in Azure Virtual Machines blockiert wird.

- [Steuern der PowerShell-Skriptausführung in Windows-Umgebungen](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

- [Verwenden der adaptiven Anwendungssteuerungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

## <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Physische oder logische Trennung von Anwendungen mit hohem Risiko

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 6.13 | 2,9 | Kunde |

Software, die für den Geschäftsbetrieb erforderlich ist, aber ein höheres Risiko für das Unternehmen darstellen kann, sollte innerhalb des eigenen virtuellen Computers und/oder virtuellen Netzwerks isoliert und entweder mit einer Azure Firewall oder einer Netzwerksicherheitsgruppe ausreichend gesichert werden.

- [Erstellen eines virtuellen Netzwerks](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Erstellen einer NSG mit einer Sicherheitskonfiguration](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der nächsten Sicherheitskontrolle: [Sichere Konfiguration](security-control-secure-configuration.md)