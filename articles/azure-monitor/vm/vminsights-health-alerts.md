---
title: Gastintegritätswarnungen von VM Insights (Vorschau)
description: In diesem Artikel werden die von VM Insights erstellten Warnungen zur Gastintegrität sowie ihre Aktivierung und das Konfigurieren von Benachrichtigungen beschrieben.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/10/2020
ms.openlocfilehash: a32ba9f1c4cf5d6bb9de69e1a6860c858e3ee2a6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101707505"
---
# <a name="vm-insights-guest-health-alerts-preview"></a>Gastintegritätswarnungen von VM Insights (Vorschau)
Mit dem Feature „Gastintegrität“ von VM Insights können Sie Informationen zur Integrität einer VM anzeigen. Dies wird anhand von verschiedenen Leistungsmessungen definiert, für die in regelmäßigen Abständen Stichproben genommen werden. Eine Warnung kann erstellt werden, wenn ein virtueller Computer oder ein „Monitor“ (Überwachungskomponente) in den Status „Fehlerhaft“ wechselt. Sie können diese Warnungen zusammen mit den Warnungen anzeigen und verwalten, die [mit den Warnungsregeln in Azure Monitor erstellt werden](../alerts/alerts-overview.md), und die proaktive Benachrichtigung über die Erstellung neuer Warnungen auswählen.

## <a name="configure-alerts"></a>Konfigurieren von Warnungen
Sie können keine explizite Warnungsregel für das Feature „Gastintegrität“ von VM Insight erstellen, während sich das Feature in der Vorschauphase befindet. Standardmäßig werden für jeden virtuellen Computer Warnungen erstellt, aber nicht für jeden Monitor.  Dies bedeutet Folgendes: Wenn ein Monitor in einen Status versetzt wird, der keine Auswirkung auf den aktuellen Status des virtuellen Computers hat, wird keine Warnung erstellt, weil sich der Status des virtuellen Computers nicht geändert hat. 

Sie können Warnungen für einen bestimmten virtuellen Computer oder einen Monitor eines virtuellen Computers deaktivieren, indem Sie im Azure-Portal in der Konfiguration für den virtuellen Computer die Einstellung **Warnungsstatus** verwenden. Ausführliche Informationen zum Konfigurieren von Monitoren im Azure-Portal finden Sie unter [Konfigurieren der Überwachung für das Feature „Gastintegrität“ von VM Insights (Vorschau)](vminsights-health-configure.md). Ausführliche Informationen zum Konfigurieren von Monitoren für mehrere virtuelle Computer finden Sie unter [Konfigurieren der Überwachung für das Feature „Gastintegrität“ von VM Insights mit Datensammlungsregeln (Vorschau)](vminsights-health-configure-dcr.md).

## <a name="alert-severity"></a>Schweregrad der Warnung
Der Schweregrad der Warnung, die vom Feature „Gastintegrität“ erstellt wird, entspricht direkt dem Schweregrad des virtuellen Computers oder Monitors, von dem die Warnung ausgelöst wird.

| Monitorstatus | Schweregrad der Warnung |
|:---|:---|
| Kritisch | Sev1 |
| Warnung  | Sev2 |
| Healthy  | Sev4 |

## <a name="alert-lifecycle"></a>Lebenszyklus von Warnungen
Für jeden virtuellen Computer wird immer dann eine [Azure-Warnung](../alerts/alerts-overview.md) erstellt, wenn sich sein Status in **Warnung** oder **Kritisch** ändert. Sie können die Warnung über die Option **Warnungen** im **Azure Monitor**-Menü oder im Menü des virtuellen Computers im Azure-Portal anzeigen.

Falls sich eine Warnung bereits im Status **Ausgelöst** befindet, wenn sich der Status des virtuellen Computers ändert, wird keine zweite Warnung erstellt. Allerdings wird der Schweregrad dieser Warnung so geändert, dass er dem Status des virtuellen Computers entspricht. Falls der virtuelle Computer beispielsweise in den Status **Kritisch** versetzt wird, wenn sich eine Warnung vom Typ **Warnung** bereits im Status **Ausgelöst** befindet, wird der Schweregrad dieser Warnung in **Sev1** geändert. Falls der virtuelle Computer in den Status **Warnung** versetzt wird, wenn sich eine Warnung mit **Sev1** bereits im Status **Ausgelöst** befindet, wird der Schweregrad dieser Warnung in **Sev2** geändert. Wenn der virtuelle Computer dann wieder in den Status **Fehlerfrei** versetzt wird, ist die Warnung behoben, und der Schweregrad wird in **Sev4** geändert.

## <a name="viewing-alerts"></a>Anzeigen von Warnungen
Sie können Warnungen, die vom Feature „Gastintegrität“ von VM Insights erstellt wurden, zusammen mit anderen [Warnungen im Azure-Portal](../platform/alerts-overview.md#alerts-experience) anzeigen. Sie können die Option **Warnungen** im **Azure Monitor**-Menü auswählen, um die Warnungen für alle überwachten Ressourcen anzuzeigen, oder im Menü eines virtuellen Computers die Option **Warnungen** verwenden, um die Warnungen nur für den entsprechenden virtuellen Computer anzuzeigen.

## <a name="alert-properties"></a>Warnungseigenschaften

### <a name="properties-in-the-azure-portal"></a>Eigenschaften im Azure-Portal
Die Eigenschaften der Warnung, die beim Anzeigen im Azure-Portal zu sehen sind, sind in der folgenden Tabelle beschrieben.

| Eigenschaft | BESCHREIBUNG |
|:---|:---|
| Monitorstatus vor der Erstellung der Warnung | Der Status des Monitors oder virtuellen Computers, bevor diese Warnung zum ersten Mal ausgelöst wurde. |
| Monitorstatus nach der Erstellung der Warnung | Der Status des Monitors oder virtuellen Computers, als die Warnung zum ersten Mal ausgelöst wurde. Dies ist der Status, der die Auslösung der Warnung verursacht hat. |
| Weitere Informationen zum Statusübergang während der Erstellung der Warnung | Link zur Seite mit den Informationen zur VM-Integrität, auf der der genaue Verlauf des Statusübergangs angezeigt wird. Dieser Statusübergang steht für den Vorgang, bei dem der Status des Monitors zuerst von **Fehlerfrei** in einen nicht fehlerfreien Status übergegangen ist. |

Beispiel: Der Status eines Monitors ändert sich zum Zeitpunkt t0 von **Fehlerfrei** in **Kritisch**, und eine neue Warnung mit **Sev1** wird ausgelöst. Anschließend erfolgt zum Zeitpunkt t1 der Übergang von **Kritisch** zu **Warnung**, und der Warnungsschweregrad wird in **Sev2** aktualisiert. Zum Zeitpunkt t2 wird zu **Fehlerfrei** gewechselt, und die Warnung gilt als behoben.

Die Warnungseigenschaften weisen diese Werte während der gesamten Sequenz auf.

- Monitorstatus vor der Erstellung der Warnung: Healthy
- Monitorstatus nach der Erstellung der Warnung: Kritisch
- Weitere Informationen zum Statusübergang während der Erstellung der Warnung: Navigationslink zum Statusübergang zum Zeitpunkt t0.


### <a name="properties-in-notifications"></a>Eigenschaften in Benachrichtigungen
Die Eigenschaften der Warnung, die in Benachrichtigungen enthalten sind, sind in der folgenden Tabelle beschrieben.

| Eigenschaft | BESCHREIBUNG |
|:---|:---|
| Vorheriger Monitorstatus | Status des Monitors oder virtuellen Computers vor der Statusänderung. Wenn diese Benachrichtigung bei der Aktualisierung des Warnungsschweregrads ausgelöst wird, gibt diese Eigenschaft den Status an, der direkt vor der Aktualisierung des Schweregrads bestanden hat. |
| Aktueller Monitorstatus | Status des Monitors oder des virtuellen Computers während der Statusänderung. Wenn diese Benachrichtigung durch die Aktualisierung des Warnungsschweregrads ausgelöst wird, repräsentiert diese Eigenschaft den neuen Status. |
| Weitere Informationen zu diesem Statusübergang | Link zur Seite mit den Informationen zur VM-Integrität, auf der der genaue Verlauf des Statusübergangs angezeigt wird. Dieser Statusübergang repräsentiert den Fall, in dem sich der Status des Monitors geändert hat und dadurch diese Benachrichtigung ausgelöst wurde. |

Im obigen Beispiel verfügen die Benachrichtigungen jeweils über die folgenden Eigenschaften.

Empfang einer Benachrichtigung zum Zeitpunkt t0
- Vorheriger Monitorstatus: Healthy
- Aktueller Monitorstatus: Kritisch
- Weitere Informationen zu diesem Statusübergang: Navigationslink zum Statusübergang zum Zeitpunkt t0.

Empfang einer Benachrichtigung zum Zeitpunkt t1
- Vorheriger Monitorstatus: Kritisch
- Aktueller Monitorstatus: Warnung
- Weitere Informationen zu diesem Statusübergang: Navigationslink zum Statusübergang zum Zeitpunkt t1.

Empfang einer Benachrichtigung zum Zeitpunkt t2
- Vorheriger Monitorstatus: Warnung
- Aktueller Monitorstatus: Healthy
- Weitere Informationen zu diesem Statusübergang: Navigationslink zum Statusübergang zum Zeitpunkt t2.

## <a name="configure-notifications"></a>Konfigurieren von Benachrichtigungen
Gehen Sie wie folgt vor, wenn Sie eine proaktive Benachrichtigung über eine Warnung erhalten möchten, die vom Feature „Gastintegrität“ ausgelöst wird: Erstellen Sie eine [Aktionsgruppe](../alerts/action-groups.md), um die verschiedenen durchzuführenden Aktionen zu definieren, z. B. das Senden einer SMS oder das Starten einer Logik-App. Erstellen Sie anschließend eine [Aktionsregel](../alerts/alerts-action-rules.md), mit der der Bereich für die Monitore und virtuellen Computer angegeben und diese Aktionsgruppe verwendet wird.

Wählen Sie im Azure-Portal im Menü **Überwachung** die Option **Warnungen** aus.  Wählen Sie **Aktionen verwalten** und dann **Aktionsregeln (Vorschau)** aus. 

![Neue Aktionsregel](media/vminsights-health-alerts/action-rule-new.png)

Klicken Sie auf **Neue Aktionsregel**, um eine neue Regel zu erstellen. Klicken Sie anschließend neben dem Bereich auf **Auswählen**, und wählen Sie entweder ein Abonnement, eine Ressourcengruppe oder einen oder mehrere bestimmte virtuelle Computer aus. Die Benachrichtigung wird nur für virtuelle Computer ausgelöst, die innerhalb des Bereichs liegen.

![Bereich der Aktionsregel](media/vminsights-health-alerts/action-rule-scope.png)

Klicken Sie neben **Filter** auf **Hinzufügen**. Erstellen Sie einen Filter, für den Folgendes gilt: **Überwachungsdienst ist gleich VM Insights – Integrität**. Fügen Sie weitere Filter hinzu, um die entsprechenden Warnungen anzugeben, von denen die Benachrichtigung ausgelöst werden soll. Beispielsweise können Sie die Option **Schweregrad** nutzen, um die Warnungen aller Monitore abzudecken, die einen bestimmten Schweregrad aufweisen.

![Filter für Aktionsregel](media/vminsights-health-alerts/action-rule-filter.png)

Wählen Sie unter **Für diesen Bereich definieren** die Option **Aktionsgruppe** und dann die Aktionsgruppe aus, die dem Monitor zugeordnet werden soll. Geben Sie der Regel einen Namen, und wählen Sie die Ressourcengruppe aus, in der sie gespeichert werden soll. Klicken Sie auf **Erstellen**, um die Regel zu erstellen.

![Aktionsregel](media/vminsights-health-alerts/action-rule.png)


## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren des Features „Gastintegrität“ von VM Insights und Onboarding von Agents](vminsights-health-enable.md)
- [Konfigurieren von Monitoren mit dem Azure-Portal](vminsights-health-configure.md)
- [Konfigurieren von Monitoren mit Datensammlungsregeln](vminsights-health-configure-dcr.md)