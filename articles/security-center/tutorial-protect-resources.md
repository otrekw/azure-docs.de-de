---
title: Tutorial zur Zugriffs- und Anwendungssteuerung in Azure Security Center
description: Dieses Tutorial zeigt, wie Sie eine Richtlinie für den Just-in-Time-VM-Zugriff und eine Anwendungssteuerungsrichtlinie konfigurieren.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/03/2018
ms.author: memildin
ms.openlocfilehash: cc88561f8882b1712648a261d1f0208a18bab26d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095475"
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Tutorial: Schützen Ihrer Ressourcen mit Azure Security Center
Security Center verringert Ihre Gefährdung durch Bedrohungen, indem mithilfe von Zugriffs- und Anwendungssteuerungen böswillige Aktivitäten blockiert werden. Durch den JIT-Zugriff (Just-in-Time) auf einen virtuellen Computer wird die Anfälligkeit für Angriffe verringert, da Sie den dauerhaften Zugriff auf virtuelle Computer verweigern können. Stattdessen bieten Sie einen gesteuerten und überwachten Zugriff auf VMs nur bei Bedarf. Adaptive Anwendungssteuerungen helfen dabei, VMs gegen Schadsoftware abzusichern, indem sie steuern, welche Anwendungen auf Ihren VMs ausgeführt werden können. Security Center nutzt maschinelles Lernen, um die auf dem virtuellen Computer ausgeführten Prozesse zu analysieren, und unterstützt Sie beim Anwenden von Positivlistenregeln unter Verwendung dieser Daten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren einer Richtlinie für den Just-in-Time-VM-Zugriff
> * Konfigurieren einer Anwendungssteuerungsrichtlinie

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchlaufen der in diesem Tutorial behandelten Features muss Azure Defender aktiviert sein. Eine kostenlose Testversion ist verfügbar. Informationen zum Durchführen eines Upgrades finden Sie unter [Aktivieren von Azure Defender](enable-azure-defender.md).

## <a name="manage-vm-access"></a>Verwalten des VM-Zugriffs
Mit JIT Zugriff auf einen virtuellen Computer kann eingehender Datenverkehr auf den virtuellen Azure-Computern gesperrt werden, um die Gefährdung durch Angriffe zu reduzieren und bei Bedarf einen einfachen Zugriff auf Verbindungen mit virtuellen Computern bereitzustellen.

Verwaltungsports müssen nicht jederzeit geöffnet sein. Sie müssen nur geöffnet sein, während eine Verbindung mit dem virtuellen Computer besteht, z.B. zum Ausführen von Verwaltungs- oder Wartungsaufgaben. Wenn Just-In-Time aktiviert ist, verwendet Security Center NSG-Regeln (Netzwerksicherheitsgruppen), die den Zugriff auf Verwaltungsports beschränken, um sie vor Angriffen zu schützen.

Folgen Sie dem Leitfaden unter [Sichern Ihrer Verwaltungsports mit Just-in-Time-Zugriff (JIT)](security-center-just-in-time.md).

## <a name="harden-vms-against-malware"></a>Absichern von VMs gegen Schadsoftware
Mit adaptiven Anwendungssteuerungen können Sie eine Gruppe von Anwendungen definieren, deren Ausführung in konfigurierten Ressourcengruppen zulässig ist. Dadurch können Sie Ihre VMs gegen Schadsoftware absichern. Security Center nutzt maschinelles Lernen, um die auf dem virtuellen Computer ausgeführten Prozesse zu analysieren, und unterstützt Sie beim Anwenden von Positivlistenregeln unter Verwendung dieser Daten.

Folgen Sie dem Leitfaden unter [Verwenden von adaptiven Anwendungssteuerungen zum Verringern der Angriffsfläche Ihres Computers](security-center-adaptive-application.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie erfahren, wie Sie Ihre Angriffsfläche für Bedrohungen wie folgt verringern:

> [!div class="checklist"]
> * Konfigurieren einer Richtlinie für den Just-in-Time-VM-Zugriff, um einen gesteuerten und überwachten Zugriff auf VMs nur bei Bedarf zu ermöglichen
> * Konfigurieren einer adaptiven Anwendungssteuerung, um festzulegen, welche Anwendungen auf Ihren VMs ausgeführt werden können

Im nächsten Tutorial erfahren Sie, wie Sie auf Sicherheitsvorfälle reagieren.

> [!div class="nextstepaction"]
> [Tutorial: Reagieren auf Sicherheitsvorfälle](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png
