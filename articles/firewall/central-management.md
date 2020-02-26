---
title: Zentrale Verwaltung für Azure Firewall
description: Hier finden Sie Informationen zur zentralen Verwaltung mit Azure Firewall Manager.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 58f670f3f55a63f0c1823adc13c98f5863d4d650
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444859"
---
# <a name="azure-firewall-central-management"></a>Zentrale Verwaltung für Azure Firewall

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Bei der Verwaltung mehrerer Firewalls ist es aufgrund von häufigen Änderungen an Firewallregeln bekanntermaßen schwierig, die Firewalls synchron zu halten. Zentrale IT-Teams benötigen eine Möglichkeit, grundlegende Firewallrichtlinien zu definieren und über mehrere Geschäftseinheiten hinweg zu erzwingen. Gleichzeitig möchten die DevOps-Teams eigene lokale, abgeleitete Firewallrichtlinien erstellen, um agiler arbeiten zu können.

Azure Firewall Manager (Vorschau) kann dabei helfen, diese Probleme zu lösen.


## <a name="azure-firewall-manager-preview"></a>Azure Firewall Manager (Vorschau)

Azure Firewall Manager (Vorschau) ist ein Dienst für die Verwaltung der Netzwerksicherheit, der eine zentrale Sicherheitsrichtlinien- und Routenverwaltung für cloudbasierte Sicherheitsperimeter bereitstellt. Mit diesem Dienst können Unternehmens-IT-Teams Regeln für die Datenverkehrsfilterung für mehrere Azure Firewall-Instanzen auf Netzwerk- und Anwendungsebene zentral definieren. Sie können verschiedene Azure-Regionen und -Abonnements in Hub-and-Spoke-Architekturen einbeziehen, um Governance und Schutz des Datenverkehrs sicherzustellen. DevOps-Teams profitieren von mehr Agilität, indem sie lokale, abgeleitete Sicherheitsrichtlinien für Firewalls nutzen können, die organisationsübergreifend implementiert werden.

### <a name="firewall-policy"></a>Firewallrichtlinie

Eine Firewallrichtlinie ist eine Azure-Ressource, die Sammlungen von NAT-, Netzwerk- und Anwendungsregeln sowie Threat Intelligence-Einstellungen enthält. Es handelt sich um eine globale Ressource, die über mehrere Azure Firewall-Instanzen hinweg in *geschützten virtuellen Hubs* und *virtuellen Hubnetzwerken* verwendet werden kann. Richtlinien können von Grund auf neu erstellt oder von vorhandenen Richtlinien geerbt werden. Die Vererbung ermöglicht DevOps-Teams die Erstellung lokaler Firewallrichtlinien zusätzlich zur obligatorischen Basisrichtlinie der Organisation. Richtlinien funktionieren regions- und abonnementübergreifend.
 
Sie können Firewallrichtlinien und -zuordnungen mithilfe von Azure Firewall Manager erstellen. Sie können eine Richtlinie aber auch über die REST-API, mit Vorlagen, in Azure PowerShell und über die Befehlszeilenschnittstelle erstellen und verwalten. Nachdem Sie eine Richtlinie erstellt haben, können Sie sie einer Firewall in einem virtuellen WAN-Hub zuordnen, sodass dieser zu einem *geschützten virtuellen Hub* wird. Sie können die Richtlinie auch einer Firewall in einem virtuellen Netzwerk zuordnen, was dieses zu einem *virtuellen Hubnetzwerk* macht.

### <a name="pricing"></a>Preise

Richtlinien werden basierend auf der Firewallzuordnung abgerechnet. Richtlinien mit bis zu einer Firewallzuordnung sind kostenlos. Richtlinien mit mehreren Firewallzuordnungen werden zu festen Preisen abgerechnet. Weitere Informationen finden Sie unter [Preise für Azure Firewall Manager](https://azure.microsoft.com/pricing/details/firewall-manager/).

## <a name="azure-firewall-management-partners"></a>Partner für die Azure Firewall-Verwaltung

Die folgenden Drittanbieterlösungen unterstützen die zentrale Azure Firewall-Verwaltung über standardmäßige Azure-REST-APIs. Jede Lösung weist ganz eigene Merkmale und Features auf:

- [AlgoSec CloudFlow](https://www.algosec.com/azure/) 
- [Barracuda Cloud Security Guardian](https://www.barracuda.com/products/cloudsecurityguardian/for_azure)
- [Tufin Orca](https://www.tufin.com/products/tufin-orca)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Firewall Manager-Vorschau finden Sie unter [Was ist Azure Firewall Manager (Vorschau)?](../firewall-manager/overview.md).