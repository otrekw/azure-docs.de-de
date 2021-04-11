---
title: Verwalten einer Datadog-Ressource – Azure-Partnerlösungen
description: In diesem Artikel wird die Verwaltung einer Datadog-Ressource im Azure-Portal beschrieben. Hier erfahren Sie, wie Sie das einmalige Anmelden einrichten, eine Confluent-Organisation löschen und Support anfordern.
ms.service: partner-services
ms.topic: conceptual
ms.date: 02/19/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: 04aef540bc134e5ec307be6a232ce47f0923e528
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105046350"
---
# <a name="manage-the-datadog-resource"></a>Verwalten der Datadog-Ressource

In diesem Artikel wird beschrieben, wie Sie die Einstellungen für Ihre Azure-Integration in Datadog verwalten.

## <a name="resource-overview"></a>Ressourcenübersicht

Klicken Sie im linken Bereich auf **Übersicht**, um Details zu ihrer Datadog-Ressource anzuzeigen.

:::image type="content" source="media/manage/resource-overview.png" alt-text="Übersicht über die Datadog-Ressource" border="true" lightbox="media/manage/resource-overview.png":::

Die Details umfassen Folgendes:

- Ressourcengruppenname
- Speicherort/Region
- Subscription
- `Tags`
- Link für einmaliges Anmelden bei der Datadog-Organisation
- Datadog-Angebot/-Plan
- Abrechnungszeitraum

Außerdem finden Sie hier Links zu Datadog-Dashboards, -Protokollen und -Hostzuordnungen.

Der Übersichtsbildschirm enthält eine Zusammenfassung der Ressourcen, die Protokolle und Metriken an Datadog senden.

- Ressourcentyp: Der Azure-Ressourcentyp.
- Ressourcen gesamt: Die Anzahl aller Ressourcen für den Ressourcentyp.
- Ressourcen, die Protokolle senden: Die Anzahl der Ressourcen, die Protokolle über die Integration an Datadog senden.
- Ressourcen, die Metriken senden: Die Anzahl der Ressourcen, die Metriken über die Integration an Datadog senden.

## <a name="reconfigure-rules-for-metrics-and-logs"></a>Neukonfigurieren von Regeln für Metriken und Protokolle

Um die Konfigurationsregeln für Metriken und Protokolle zu ändern, wählen Sie im linken Bereich **Metriken und Protokolle** aus.

:::image type="content" source="media/manage/reconfigure-metrics-and-logs.png" alt-text="Ändern der Konfiguration von Protokollen und Metriken für die Datadog-Ressource." border="true":::

Weitere Informationen finden Sie unter [Konfigurieren von Metriken und Protokollen](create.md#configure-metrics-and-logs).

## <a name="view-monitored-resources"></a>Anzeigen der überwachten Ressourcen

Um die Liste der Ressourcen anzuzeigen, die Protokolle an Datadog ausgeben, wählen Sie im linken Bereich **Überwachte Ressourcen** aus.

:::image type="content" source="media/manage/view-monitored-resources.png" alt-text="Anzeigen der von Datadog überwachten Ressourcen" border="true":::

Sie können die Liste der Ressourcen nach Ressourcentyp, Namen der Ressourcengruppe, Speicherort und danach filtern, ob die Ressource Protokolle und Metriken sendet.

In der Spalte **Protokolle für Datadog** ist angegeben, ob die Ressource Protokolle an Datadog sendet. Wenn die Ressource keine Protokolle sendet, gibt dieses Feld an, warum keine Protokolle an Datadog gesendet werden. Mögliche Gründe:

- Die Ressource unterstützt das Senden von Protokollen nicht. Nur Ressourcentypen mit Überwachungsprotokollkategorien können so konfiguriert werden, dass Protokolle an Datadog gesendet werden.
- Das Limit von fünf Diagnoseeinstellungen wurde erreicht. Jede Azure-Ressource kann über maximal fünf Diagnoseeinstellungen verfügen. Weitere Informationen finden Sie unter [Diagnoseeinstellungen](../../azure-monitor/essentials/diagnostic-settings.md).
- Fehler. Die Ressource ist so konfiguriert, dass Protokolle an Datadog gesendet werden, diese werden jedoch von einem Fehler blockiert.
- Protokolle sind nicht konfiguriert. Nur Azure-Ressourcen, die über die entsprechenden Ressourcentags verfügen, werden so konfiguriert, dass Protokolle an Datadog gesendet werden.
- Die Region wird nicht unterstützt. Die Azure-Ressource befindet sich in einer Region, die das Senden von Protokollen an Datadog derzeit nicht unterstützt.
- Der Datadog-Agent ist nicht konfiguriert. Virtuelle Computer, auf denen der Datadog-Agent nicht installiert ist, geben keine Protokolle an Datadog aus.

## <a name="api-keys"></a>API-Schlüssel

Um die Liste der API-Schlüssel für Ihre Datadog-Ressource anzuzeigen, wählen Sie im linken Bereich **Schlüssel** aus. Informationen zu den Schlüsseln werden angezeigt.

:::image type="content" source="media/manage/keys.png" alt-text="API-Schlüssel für die Datadog-Organisation." border="true":::

Der Azure-Portal stellt eine schreibgeschützte Ansicht der API-Schlüssel bereit. Um die Schlüssel zu verwalten, wählen Sie den Link zum Datadog-Portal aus. Aktualisieren Sie die Azure-Portal-Ansicht, nachdem Sie Änderungen im Datadog-Portal vorgenommen haben.

Die Azure-Datadog-Integration ermöglicht Ihnen die Installation des Datadog-Agents auf einem virtuellen Computer oder in einem App-Dienst. Wenn kein Standardschlüssel ausgewählt ist, kann die Datadog-Agent-Installation nicht ausgeführt werden.

## <a name="monitor-virtual-machines-using-the-datadog-agent"></a>Überwachen von virtuellen Computern mit dem Datadog-Agent

Sie können Datadog-Agents als Erweiterung auf virtuellen Computern installieren. Wechseln Sie im linken Bereich unter der Datadog-Organisationskonfigurationen zum **Agent für virtuelle Computer**. Auf diesem Bildschirm wird die Liste aller virtuellen Computer im Abonnement angezeigt.

Die folgenden Daten werden für jeden virtuellen Computer angezeigt:

- Ressourcenname: Der Name des virtuellen Computers
- Ressourcenstatus: Gibt an, ob der virtuelle Computer beendet wurde oder ausgeführt wird. Der Datadog-Agent kann nur auf virtuellen Computern installiert werden, die ausgeführt werden. Wenn der virtuelle Computer beendet wurde, ist die Installation des Datadog-Agents deaktiviert.
- Agent-Version: Die Versionsnummer des Datadog-Agents.
- Agent-Status: Gibt an, ob der Datadog-Agent auf dem virtuellen Computer ausgeführt wird.
- Aktivierte Integrationen: Die Schlüsselmetriken, die vom Datadog-Agent gesammelt werden.
- Installationsmethode: Das spezifische Tool, das zum Installieren des Datadog-Agents verwendet wird, z. B. Chef oder Script.
- Protokolle werden gesendet: Gibt an, ob der Datadog-Agent Protokolle an Datadog sendet.

Wählen Sie den virtuellen Computer aus, auf dem der Datadog-Agent installiert werden soll. Wählen Sie **Agent installieren** aus.

Sie werden im Portal aufgefordert, die Installation des Agents mit dem Standardschlüssel zu bestätigen. Wählen Sie **OK** aus, um die Installation zu starten. Azure zeigt als Status **Installieren** an, bis der Agent installiert und bereitgestellt wurde.

Nachdem der Datadog-Agent installiert wurde, ändert sich der Status in „Installiert“.

Um festzustellen, ob der Datadog-Agent installiert wurde, wählen Sie den virtuellen Computer aus, und navigieren Sie zum Fenster „Erweiterungen“.

Sie können Datadog-Agents auf einem virtuellen Computer deinstallieren, indem Sie zu **Agent für virtuelle Computer** wechseln. Wählen Sie den virtuellen Computer und dann **Agent deinstallieren** aus.

## <a name="monitor-app-services-using-the-datadog-agent-as-an-extension"></a>Überwachen von App Services mithilfe des Datadog-Agents als Erweiterung

Sie können Datadog-Agents als Erweiterung für App Services installieren. Wechseln Sie im linken Bereich zu **App Service-Erweiterung**. Auf diesem Bildschirm wird die Liste aller App Services im Abonnement angezeigt.

Für jeden App Service werden die folgenden Datenelemente angezeigt:

- Ressourcenname: Der Name des virtuellen Computers.
- Ressourcenstatus: Gibt an, ob der App Service beendet wurde oder ausgeführt wird. Der Datadog-Agent kann nur für App Services installiert werden, die ausgeführt werden. Wenn der App Service beendet wurde, ist die Installation des Datadog-Agents deaktiviert.
- App Service-Plan: Der spezifische Plan, der für den App Service konfiguriert ist.
- Agent-Version: Die Versionsnummer des Datadog-Agents.

Wählen Sie zum Installieren des Datadog-Agents den App Service und dann **Erweiterung installieren** aus. Der aktuelle Datadog-Agent wird als Erweiterung für den App Service installiert.

Im Portal wird bestätigt, dass Sie den Datadog-Agent installieren möchten. Außerdem werden die Anwendungseinstellungen für den jeweiligen App Service mit dem Standardschlüssel aktualisiert. Der App Service wird neu gestartet, nachdem die Installation des Datadog-Agents abgeschlossen wurde.

Wählen Sie **OK** aus, um den Installationsvorgang für den Datadog-Agent zu starten. Im Portal wird als Status **Installieren** angezeigt, bis der Agent installiert wurde. Nachdem der Datadog-Agent installiert wurde, ändert sich der Status in „Installiert“.

Wenn Sie Datadog-Agents für den App Service deinstallieren möchten, wechseln Sie zu **App Service-Erweiterung**. Wählen Sie den App Service und dann **Erweiterung deinstallieren** aus.

## <a name="reconfigure-single-sign-on"></a>Neukonfigurieren des einmaligen Anmeldens

Wenn Sie einmaliges Anmelden neu konfigurieren möchten, wählen Sie im linken Bereich **Einmaliges Anmelden (SSO)** aus.

Wählen Sie **Einmaliges Anmelden über Azure Active Directory aktivieren** aus, um das einmalige Anmelden über Azure Active Directory einzurichten.

Der Portal ruft die entsprechende Datadog-Anwendung aus Azure Active Directory ab. Die App entspricht dem Unternehmens-App-Namen, den Sie beim Einrichten der Integration ausgewählt haben. Wählen Sie den Namen der Datadog-App aus, wie unten dargestellt:
 
:::image type="content" source="media/manage/reconfigure-single-sign-on.png" alt-text="Neukonfigurieren der Anwendung für einmaliges Anmelden." border="true":::
 
## <a name="disable-or-enable-integration"></a>Deaktivieren oder Aktivieren der Integration

Sie können das Senden von Protokollen und Metriken von Azure an Datadog beenden. Sonstige Datadog-Dienste, die sich nicht auf die Überwachung von Metriken und Protokollen beziehen, werden weiterhin in Rechnung gestellt.

Weitere Informationen zum Deaktivieren der Azure-Integration mit Datadog finden Sie unter **Übersicht**. Wählen Sie **Deaktivieren** und **OK** aus.
 
:::image type="content" source="media/manage/disable.png" alt-text="Deaktivieren der Datadog-Ressource." border="true":::

Weitere Informationen zum Aktivieren der Azure-Integration mit Datadog finden Sie unter **Übersicht**. Wählen Sie **Aktivieren** und **OK** aus. Wenn Sie **Aktivieren** auswählen, wird eine vorherige Konfiguration für Metriken und Protokolle abgerufen. Die Konfiguration bestimmt, welche Azure-Ressourcen Metriken und Protokolle an Datadog ausgeben. Nach Abschluss dieses Schritts werden die Metriken und Protokolle an Datadog gesendet.
 
:::image type="content" source="media/manage/enable.png" alt-text="Aktivieren der Datadog-Ressource." border="true":::

## <a name="delete-datadog-resource"></a>Löschen der Datadog-Ressource

Wechseln Sie im linken Bereich zu **Übersicht**, und wählen Sie **Löschen** aus. Bestätigen Sie, dass Sie die Datadog-Ressource löschen möchten. Klicken Sie auf **Löschen**.

:::image type="content" source="media/manage/delete.png" alt-text="Löschen der Datadog-Ressource" border="true":::

Wenn einer Datadog-Organisation nur eine Datadog-Ressource zugeordnet ist, werden keine Protokolle und Metriken mehr an Datadog gesendet. Die gesamte Abrechnung für Datadog über Azure Marketplace wird beendet.

Wenn der Datadog-Organisation mehrere Datadog-Ressourcen zugeordnet sind, wird beim Löschen der Datadog-Ressource nur das Senden von Protokollen und Metriken für diese Datadog-Ressource beendet. Da die Datadog-Organisation mit weiteren Azure-Ressourcen verknüpft ist, wird die Abrechnung über Azure Marketplace weitergeführt.

## <a name="getting-support"></a>Anfordern von Support

Wenn Sie sich bezüglich der Azure-Datadog-Integration an den Support wenden möchten, wählen Sie im linken Bereich **Neue Supportanfrage** aus. Wählen Sie den Link zum Datadog-Portal aus.

:::image type="content" source="media/manage/support-request.png" alt-text="Erstellen einer neuen Datadog-Supportanfrage" border="true":::

## <a name="next-steps"></a>Nächste Schritte

Hilfestellungen für die Problembehandlung finden Sie unter [Problembehandlung bei Datadog-Lösungen](troubleshoot.md).