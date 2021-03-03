---
title: Empfangen von Aktivitätsprotokollwarnungen zu Azure-Dienstbenachrichtigungen über das Azure-Portal
description: Erfahren Sie, wie Sie über das Azure-Portal Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen einrichten.
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 48126d923cb0baa33058c6fd55e48f31d793fade
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100570186"
---
# <a name="create-activity-log-alerts-on-service-notifications-using-the-azure-portal"></a>Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen über das Azure-Portal
## <a name="overview"></a>Übersicht

Dieser Artikel zeigt, wie Sie über das Azure-Portal Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen einrichten.  

Benachrichtigungen zur Dienstintegrität werden im [Azure-Aktivitätsprotokoll](../azure-monitor/essentials/platform-logs-overview.md) gespeichert. Angesichts der großen Menge an Informationen, die im Aktivitätsprotokoll gespeichert werden, ist eine separate Benutzeroberfläche vorhanden, um die Anzeige und Einrichtung von Benachrichtigungen zur Dienstintegrität zu erleichtern. 

Sie können eine Warnung erhalten, wenn Azure Benachrichtigungen zur Dienstintegrität an Ihr Azure-Abonnement sendet. Sie können die Warnung konfigurieren auf der Grundlage von:

- Der Klasse der Dienstintegritätsbenachrichtigung (Dientsprobleme, Geplante Wartung, Integritätsempfehlungen, Sicherheitsempfehlungen).
- Dem betroffenen Abonnement.
- Den betroffenen Diensten.
- Den betroffenen Regionen.

> [!NOTE]
> Dienstintegritätsbenachrichtigungen senden keine Warnungen für Ressourcenintegritätsereignisse.

Sie können auch konfigurieren, an wen die Warnung gesendet werden soll:

- Wählen Sie eine vorhandene Aktionsgruppe aus.
- Erstellen Sie eine neue Aktionsgruppe (die für zukünftige Warnungen verwendet werden kann).

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../azure-monitor/alerts/action-groups.md).

Informationen zum Konfigurieren von Dienstintegritätsbenachrichtigungs-Warnungen mithilfe von Azure Resource Manager-Vorlagen siehe [Resource Manager-Vorlagen](../azure-monitor/alerts/alerts-activity-log.md).

### <a name="watch-a-video-on-setting-up-your-first-azure-service-health-alert"></a>Sehen Sie sich ein Video über das Einrichten der ersten Azure Service Health-Warnung an!

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="create-service-health-alert-using-azure-portal"></a>Erstellen einer Service Health-Warnung über das Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Dienstintegrität** aus.

    ![Der Dienst „Dienstintegrität“](media/alerts-activity-log-service-notifications/home-servicehealth.png)

1. Wählen Sie im Abschnitt **Warnungen** die Option **Integritätswarnungen** aus.

    ![Die Registerkarte „Integritätswarnungen“](media/alerts-activity-log-service-notifications/alerts-blades-sh.png)

1. Wählen Sie **Service Health-Warnung hinzufügen** aus, und füllen Sie die Felder aus.

    ![Der Befehl „Dienstintegritätswarnung erstellen“](media/alerts-activity-log-service-notifications/service-health-alert.png)

1. Wählen Sie das **Abonnement**, die **Dienste** und die **Regionen** aus, für die Sie Benachrichtigungen erhalten möchten.

    [![Das Dialogfeld „Aktivitätsprotokollwarnung hinzufügen“](./media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png)](./media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png#lightbox)

> [!NOTE]
>Dieses Abonnement wird verwendet, um die Aktivitätsprotokollwarnung zu speichern. Die Warnungsressource wird für dieses Abonnement bereitgestellt und überwacht hierfür Ereignisse im Aktivitätsprotokoll.

5. Wählen Sie die **Ereignistypen** aus, bei denen Sie benachrichtigt werden möchten: *Dienstproblem*, *Geplante Wartung*, *Integritätsempfehlungen* und *Sicherheitsempfehlung*.

6. Klicken Sie auf **Aktionsgruppe auswählen**, um eine vorhandene Aktionsgruppe auszuwählen oder eine neue Aktionsgruppe zu erstellen. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../azure-monitor/alerts/action-groups.md).


7. Definieren Sie die Warnungsdetails durch Eingeben eines **Namens für die Warnungsregel** und einer **Beschreibung**.

8. Wählen Sie die **Ressourcengruppe** aus, in der die Warnung gespeichert werden soll.



Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.

Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](service-health-alert-webhook-guide.md). Informationen über das Webhookschema für Aktivitätsprotokollwarnungen finden Sie unter [Webhook für Azure-Aktivitätsprotokollwarnungen](../azure-monitor/alerts/activity-log-alerts-webhook.md).


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [bewährte Methoden zum Einrichten von Azure Service Health-Warnungen](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUa).
- Erfahren Sie, wie Sie [mobile Pushbenachrichtigungen für Azure Service Health einrichten](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUw).
- Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](service-health-alert-webhook-guide.md).
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](service-notifications.md).
- Erfahren Sie mehr über [Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks](../azure-monitor/alerts/alerts-rate-limiting.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../azure-monitor/alerts/activity-log-alerts-webhook.md).
- Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](../azure-monitor/alerts/alerts-overview.md), und erfahren Sie, wie Sie Warnungen empfangen können.
- Weitere Informationen zu [Aktionsgruppen](../azure-monitor/alerts/action-groups.md).
