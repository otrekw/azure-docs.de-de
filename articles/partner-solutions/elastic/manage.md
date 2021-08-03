---
title: Verwalten einer Elastic-Integration in Azure – Azure-Partnerlösungen
description: Dieser Artikel beschreibt die Verwaltung von Elastic im Azure-Portal. Hier finden Sie Informationen dazu, wie Sie Diagnoseeinstellungen konfigurieren und die Ressource löschen.
ms.service: partner-services
ms.topic: conceptual
ms.date: 05/20/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: 9feb4c5eb143ced9079bfe9e63b451aecd437212
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111952584"
---
# <a name="manage-the-elastic-integration-with-azure"></a>Verwalten der Elastic-Integration mit Azure

Dieser Artikel beschreibt, wie Sie Ihre Integration von Elastic in Azure verwalten. Es wird gezeigt, wie Sie die Diagnoseeinstellungen konfigurieren und die Elastic-Ressource löschen.

## <a name="reconfigure-rules-for-metrics-and-logs"></a>Neukonfigurieren von Regeln für Metriken und Protokolle

Beim Erstellen der Elastic-Ressource haben Sie konfiguriert, welche Protokolle an Elastic gesendet werden. Wenn Sie diese Einstellungen ändern müssen, wählen Sie im linken Bereich **Metriken und Protokolle** aus. Nehmen Sie die erforderlichen Änderungen an der Art und Weise vor, wie Protokolle an Elastic gesendet werden.

Weitere Informationen zu den beiden Protokolltypen finden Sie unter [Schnellstart: Erste Schritte mit Elastic](create.md).

:::image type="content" source="media/manage/reconfigure-logs.png" alt-text="Ändern der Protokolleinstellungen":::

## <a name="view-monitored-resources"></a>Anzeigen der überwachten Ressourcen

Eine Liste der Ressourcen, die Protokolle an Elastic senden, finden Sie im linken Bereich unter **Überwachte Ressourcen**.

:::image type="content" source="media/manage/monitored-resources.png" alt-text="Anzeigen der überwachten Ressourcen":::

Sie können die Liste nach Ressourcentyp, Namen der Ressourcengruppe, Speicherort und danach filtern, ob die Ressource Protokolle sendet.

Die Spalte **Protokolle an Elastic** gibt an, ob die Ressource Protokolle an Elastic sendet. Wenn die Ressource keine Protokolle sendet, gibt dieses Feld an, warum keine Protokolle gesendet werden. Mögliche Gründe:

* Die Ressource unterstützt das Senden von Protokollen nicht. Nur Azure-Ressourcenprotokolle für alle hier definierten Ressourcentypen und Protokollkategorien können so konfiguriert werden, dass Protokolle an Elastic gesendet werden.
* Das Limit von fünf Diagnoseeinstellungen wurde erreicht. Jede Azure-Ressource kann über maximal fünf [Diagnoseeinstellungen](../../azure-monitor/essentials/diagnostic-settings.md) verfügen.
* Ein Fehler blockiert das Senden von Protokollen an Elastic.
* Für die Ressource sind keine Protokolle konfiguriert. Nur Ressourcen mit den entsprechenden Ressourcentags werden an Elastic gesendet. Sie haben die Tagregeln in der Protokollkonfiguration angegeben. 
* Die Region wird nicht unterstützt. Die Azure-Ressource befindet sich in einer Region, die derzeit keine Protokolle an Elastic sendet. 

## <a name="monitor-virtual-machines-using-elastic-agent"></a>Überwachen von virtuellen Computern mit dem Elastic-Agent

Sie können Elastic-Agents als Erweiterung auf virtuellen Computern installieren. Um die verfügbaren virtuellen Computer in Ihrem Abonnement anzuzeigen, wählen Sie im linken Bereich Ihrer Elastic-Ressource die Option **Virtuelle Computer** aus.

:::image type="content" source="media/manage/vm-agents.png" alt-text="Anzeigen virtueller Computer":::

Die folgenden Daten werden für jeden virtuellen Computer angezeigt:

* Ressourcenname: Der Name des virtuellen Computers.
* Ressourcenstatus: Gibt an, ob der virtuelle Computer beendet wurde oder ausgeführt wird. Der Elastic-Agent kann nur auf virtuellen Computern installiert werden, die ausgeführt werden. Wenn ein virtueller Computer angehalten wird, wird die Installation des Elastic-Agent deaktiviert.
* Agent-Version: Die Versionsnummer des Elastic-Agents.
* Agent-Status: Gibt an, ob der Elastic-Agent auf dem virtuellen Computer ausgeführt wird.
* Aktivierte Integrationen: Die Schlüsselmetriken, die vom Elastic-Agent gesammelt werden.
* Senden von Protokollen: Gibt an, ob der Elastic-Agent Protokolle an Elastic sendet.

Zum Installieren des Elastic-Agents wählen Sie einen virtuellen Computer und dann **Erweiterung installieren** aus.

Sie werden im Portal aufgefordert, die Installation des Agents mit der Standardauthentifizierung zu bestätigen. Wählen Sie **OK** aus, um die Installation zu starten. Das Portal zeigt **Wird installiert** als Status an, bis der Agent installiert und bereitgestellt wurde.

Nachdem der Elastic-Agent installiert wurde, ändert sich der Status in **Installiert**.

Um festzustellen, ob der Elastic-Agent installiert wurde, wählen Sie den virtuellen Computer aus, und navigieren Sie zu **Erweiterungen**.

Zum Deinstallieren des Elastic-Agents von einem virtuellen Computer wählen Sie den virtuellen Computer und dann **Erweiterung deinstallieren** aus.

## <a name="configure-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen

Zum Konfigurieren der Diagnoseeinstellungen für eine Ressource wählen Sie die entsprechende Ressource aus. Wählen Sie dann im linken Bereich **Diagnoseeinstellungen** aus. 

Aktivieren Sie im Abschnitt mit den Details zum Ziel die Option zum Senden an Partnerlösungen, um Elastic als Ziel auszuwählen. Diese Option ist erst verfügbar, nachdem eine Elastic-Ressource erstellt wurde.

:::image type="content" source="media/manage/diagnostic-settings.png" alt-text="Konfigurieren von Diagnoseeinstellungen":::

## <a name="delete-elastic-resource"></a>Löschen einer Elastic-Ressource

Wenn Sie Ihre Elastic-Ressource nicht mehr benötigen, löschen Sie sie im Azure-Portal.

Um die Elastic-Ressource in Azure zu löschen, wählen Sie sie aus. Wählen Sie in der **Übersicht** die Option **Löschen** aus. Bestätigen Sie, dass Sie die Elastic-Ressource löschen möchten.

:::image type="content" source="media/manage/delete-elastic.png" alt-text="Löschen einer Elastic-Ressource":::

Nachdem die Elastic-Ressource gelöscht wurde, werden keine Protokolle mehr an Elastic gesendet. Sämtliche Abrechnungen für Elastic über den Azure Marketplace werden gestoppt.

## <a name="next-steps"></a>Nächste Schritte

Hilfe beim Lösen von Problemen finden Sie unter [Problembehandlung für die Elastic-Integration in Azure](troubleshoot.md).