---
title: 'Einrichten von Dienstwarnungen für Windows Virtual Desktop: Azure'
description: Informationen zum Einrichten von Azure Service Health für den Empfang von Dienstbenachrichtigungen für Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 035fc4840b08a0a9807c228cb5f63c191efc524a
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86256882"
---
# <a name="tutorial-set-up-service-alerts"></a>Tutorial: Einrichten von Dienstwarnungen

>[!IMPORTANT]
>Dieser Artikel gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/set-up-service-alerts-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit Azure Service Health können Sie Dienstprobleme und Integritätsempfehlungen für Windows Virtual Desktop überwachen. Azure Service Health kann Sie mithilfe verschiedener Warnungstypen (z.B. E-Mail oder SMS) benachrichtigen, die Auswirkungen eines Problems verdeutlichen und Sie über die Lösung eines Problems auf dem Laufenden halten. Azure Service Health kann Sie auch bei der Verkürzung von Ausfallzeiten und bei der Vorbereitung auf eine geplante Wartung und Änderungen unterstützen, die sich u. U. auf die Verfügbarkeit Ihrer Ressourcen auswirken.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen und Konfigurieren von Dienstwarnungen

Weitere Informationen zu Azure Service Health finden Sie in der [Azure Service Health-Dokumentation](https://docs.microsoft.com/azure/service-health/).

## <a name="create-service-alerts"></a>Erstellen von Dienstwarnungen

In diesem Abschnitt erfahren Sie, wie Sie Azure Service Health konfigurieren und Benachrichtigungen einrichten, auf die Sie im Azure-Portal zugreifen können. Sie können verschiedene Arten von Warnungen einrichten und diese so planen, dass Sie rechtzeitig benachrichtigt werden.

### <a name="recommended-service-alerts"></a>Empfohlene Dienstwarnungen

Wir empfehlen die Erstellung von Dienstwarnungen für die folgenden Integritätsereignistypen:

- **Dienstproblem:** Sie können Benachrichtigungen zu wichtigen Problemen erhalten, die sich auf die Verbindung Ihrer Benutzer mit dem Dienst oder auf die Verwaltung Ihres Windows Virtual Desktop-Mandanten auswirken.
- **Integritätsempfehlung:** Sie können Benachrichtigungen erhalten, die Ihre Aufmerksamkeit erfordern. Es folgen Beispiele für diese Art von Benachrichtigung:
    - Virtuelle Computer (VMs) mit offenem Port 3389 nicht sicher konfiguriert
    - Unterstützung von Funktionalität eingestellt

### <a name="configure-service-alerts"></a>Konfigurieren von Dienstwarnungen

So konfigurieren Sie Dienstwarnungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Service Health** aus.
3. Befolgen Sie die Anweisungen unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal), um Ihre Warnungen und Benachrichtigungen einzurichten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie mit Azure Service Health Dienstprobleme und Integritätsempfehlungen für Windows Virtual Desktop überwachen. Fahren Sie mit den Anleitungsartikeln zum Herstellen der Verbindung mit Windows Virtual Desktop fort, um mehr zur Anmeldung bei Windows Virtual Desktop zu erfahren.

> [!div class="nextstepaction"]
> [Connect to the Remote Desktop client on Windows 7 and Windows 10](./connect-windows-7-10.md) (Herstellen einer Verbindung mit dem Remotedesktopclient unter Windows 7 und Windows 10)
