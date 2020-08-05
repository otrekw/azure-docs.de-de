---
title: Einführung in die „Wirksame Sicherheitsregeln“-Ansicht in Azure Network Watcher | Microsoft-Dokumentation
description: Diese Seite enthält eine Übersicht über die Network Watcher-Funktion zur Ansicht „Wirksame Sicherheitsregeln“.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: damendo
ms.openlocfilehash: f4c601184a060c3dfc4f033bcf983bf773f7167f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87022649"
---
# <a name="introduction-to-effective-security-rules-view-in-azure-network-watcher"></a>Einführung in die „Wirksame Sicherheitsregeln“-Ansicht in Azure Network Watcher

Netzwerksicherheitsgruppen werden auf Subnetzebene oder auf NIC-Ebene zugeordnet. Bei einer Zuordnung auf Subnetzebene gilt sie für alle VM-Instanzen in dem Subnetz. Die Ansicht „Wirksame Sicherheitsregeln“ gibt alle konfigurierten NSGs und Regeln zurück, die auf NIC- und Subnetzebene für einen virtuellen Computer zugeordnet wurden, und bietet dabei Einblicke in die Konfiguration. Darüber hinaus werden die geltenden Sicherheitsregeln für jede Netzwerkkarte auf einem virtuellen Computer zurückgegeben. Mithilfe der Ansicht „Wirksame Sicherheitsregeln“ können Sie Netzwerksicherheitslücken auf einem virtuellen Computer, z. B. geöffnete Ports, beurteilen. Sie können auch basierend auf einem [Vergleich zwischen den konfigurierten und genehmigten Sicherheitsregeln](network-watcher-nsg-auditing-powershell.md) überprüfen, ob die Netzwerksicherheitsgruppe wie erwartet funktioniert.

Einen weiter gefassten Anwendungsfall stellt die Sicherheitskonformität und -überwachung dar. Sie können einen normativen Satz von Sicherheitsregeln als Modell für die Sicherheitsbestimmungen in Ihrer Organisation definieren. Eine regelmäßige Überwachung der Einhaltung kann programmgesteuert durch einen Vergleich der normativen Regeln mit den angewendeten Regeln für die einzelnen virtuellen Computer in Ihrem Netzwerk implementiert werden.

Im Portal werden die Regeln für jedes Netzwerkschnittstelle angezeigt und nach Eingabe und Ausgabe gruppiert. Dies bietet einen einfachen Einblick in die Regeln, die auf einen virtuellen Computer angewendet werden. Über eine Downloadschaltfläche können sämtliche Sicherheitsregeln unabhängig von der Registerkarte in eine CSV-Datei heruntergeladen werden.

![Sicherheitsgruppenansicht][1]

Es können Regeln ausgewählt werden. Damit wird dann ein neues Blatt geöffnet, auf dem die Netzwerksicherheitsgruppe und die Quell- und Zielpräfixe angezeigt werden. Sie können von diesem Blatt aus direkt zu der Ressource der Netzwerksicherheitsgruppe navigieren.

![Drilldown][2]

### <a name="next-steps"></a>Nächste Schritte

Das Feature *Wirksame Sicherheitsgruppen* können Sie auch über die im Folgenden aufgeführten Methoden verwenden:
* [REST-API](https://docs.microsoft.com/rest/api/virtualnetwork/NetworkInterfaces/ListEffectiveNetworkSecurityGroups)
* [PowerShell](https://docs.microsoft.com/powershell/module/az.network/get-azeffectivenetworksecuritygroup?view=azps-4.4.0)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-list-effective-nsg)

Erfahren Sie unter [Überwachen von Netzwerksicherheitsgruppen-Einstellungen mit PowerShell](network-watcher-nsg-auditing-powershell.md), wie Sie die Einstellungen Ihrer Netzwerksicherheitsgruppe überwachen.

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









