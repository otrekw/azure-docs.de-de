---
title: Empfangen von Aktivitätsprotokollwarnungen zu Azure-Dienstbenachrichtigungen über das Azure-Portal
description: Lassen Sie sich per SMS, E-Mail oder Webhook benachrichtigen, wenn Ereignisse beim Azure-Dienst eintreten.
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: a8723698cddfb519687525820475517b93219a4a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567647"
---
# <a name="create-activity-log-alerts-on-service-notifications-using-the-azure-portal"></a>Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen über das Azure-Portal
## <a name="overview"></a>Übersicht

Dieser Artikel zeigt, wie Sie über das Azure-Portal Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen einrichten.  

Dienstintegritätsbenachrichtigung werden im [Azure Aktivitätsprotokoll ](../azure-monitor/platform/platform-logs-overview.md) gespeichert. Angesichts der möglicherweise großen Menge an Informationen, die im Aktivitätsprotokoll gespeichert werden, ist eine separate Benutzeroberfläche vorhanden, um die Anzeige und Einrichtung von Benachrichtigungen zur Dienstintegrität zu erleichtern. 

Sie können eine Warnung erhalten, wenn Azure Benachrichtigungen zur Dienstintegrität an Ihr Azure-Abonnement sendet. Sie können die Warnung konfigurieren auf der Grundlage von:

- Der Klasse der Dienstintegritätsbenachrichtigung (Dientsprobleme, Geplante Wartung, Integritätsempfehlungen).
- Dem betroffenen Abonnement.
- Den betroffenen Diensten.
- Den betroffenen Regionen.

> [!NOTE]
> Dienstintegritätsbenachrichtigungen senden keine Warnung bezüglich Ereignissen der Ressourcenintegrität.

Sie können auch konfigurieren, an wen die Warnung gesendet werden soll:

- Wählen Sie eine vorhandene Aktionsgruppe aus.
- Erstellen Sie eine neue Aktionsgruppe (die für zukünftige Warnungen verwendet werden kann).

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../azure-monitor/platform/action-groups.md).

Informationen zum Konfigurieren von Dienstintegritätsbenachrichtigungs-Warnungen mithilfe von Azure Resource Manager-Vorlagen siehe [Resource Manager-Vorlagen](../azure-monitor/platform/alerts-activity-log.md).

### <a name="watch-a-video-on-setting-up-your-first-azure-service-health-alert"></a>Sehen Sie sich ein Video über das Einrichten der ersten Azure Service Health-Warnung an!

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="alert-and-new-action-group-using-azure-portal"></a>Warnung und neue Aktionsgruppe mit dem Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Dienstintegrität** aus.

    ![Der Dienst „Dienstintegrität“](media/alerts-activity-log-service-notifications/home-servicehealth.png)

1. Wählen Sie im Abschnitt **Warnungen** die Option **Integritätswarnungen** aus.

    ![Die Registerkarte „Integritätswarnungen“](media/alerts-activity-log-service-notifications/alerts-blades-sh.png)

1. Wählen Sie **Dienstintegritätswarnung erstellen** aus, und füllen Sie die Felder aus.

    ![Der Befehl „Dienstintegritätswarnung erstellen“](media/alerts-activity-log-service-notifications/service-health-alert.png)

1. Wählen Sie das **Abonnement**, die **Dienste** und die **Regionen** für Benachrichtigungen aus.

    ![Das Dialogfeld „Aktivitätsprotokollwarnung hinzufügen“](media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png)

    > [!NOTE]
    > Dieses Abonnement wird verwendet, um die Aktivitätsprotokollwarnung zu speichern. Die Warnungsressource wird für dieses Abonnement bereitgestellt und überwacht hierfür Ereignisse im Aktivitätsprotokoll.

1. Wählen Sie die **Ereignistypen** aus, bei denen Sie benachrichtigt werden möchten: *Dienstproblem*, *Geplante Wartung* und *Integritätsempfehlungen* 

1. Definieren Sie die Warnungsdetails durch Eingeben eines **Namens für die Warnungsregel** und einer **Beschreibung**.

1. Wählen Sie die **Ressourcengruppe** aus, in der die Warnung gespeichert werden soll.

1. Erstellen Sie eine neue Aktionsgruppe, indem Sie **Neue Aktionsgruppe** auswählen. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Auf den kurzen Namen wird in den Benachrichtigungen verwiesen, die gesendet werden, wenn diese Warnung ausgelöst wird.

    ![Erstellen einer neuen Aktionsgruppe](media/alerts-activity-log-service-notifications/action-group-creation.png)

1. Definieren Sie eine Liste von Empfängern, indem Sie folgende Daten der Empfänger angeben:

    a. **Name**: Geben Sie Name, Alias oder Bezeichner des Empfängers ein.

    b. **Aktionstyp**: Wählen Sie „SMS“, „E-Mail“, „Webhook“ „Azure-App“ usw. aus.

    c. **Details**: Geben Sie basierend auf dem ausgewählten Aktionstyp eine Telefonnummer, eine E-Mail-Adresse, einen Webhook-URI usw. ein.

1. Wählen Sie **OK** aus, um die Aktionsgruppe zu erstellen, und dann **Warnungsregel erstellen**, um die Warnung abzuschließen.

Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.

Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](service-health-alert-webhook-guide.md). Informationen über das Webhookschema für Aktivitätsprotokollwarnungen finden Sie unter [Webhook für Azure-Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md).

>[!NOTE]
>Die in den folgenden Schritten definierte Aktionsgruppe kann als vorhandene Aktionsgruppe für alle zukünftigen Warnungsdefinitionen wiederverwendet werden.
>

## <a name="alert-with-existing-action-group-using-azure-portal"></a>Warnung mit vorhandener Aktionsgruppe mit dem Azure-Portal

1. Führen Sie die Schritte 1 bis 6 im vorherigen Abschnitt aus, um Ihre Dienstintegritätsbenachrichtigung zu erstellen. 

1. Klicken Sie unter **Aktionsgruppe definieren** auf die Schaltfläche **Aktionsgruppe auswählen**. Wählen Sie die entsprechende Aktionsgruppe aus.

1. Wählen Sie **Hinzufügen** aus, um die Aktionsgruppe hinzuzufügen, und dann **Warnungsregel erstellen**, um die Warnung abzuschließen.

Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [bewährte Methoden zum Einrichten von Azure Service Health-Warnungen](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUa).
- Erfahren Sie, wie Sie [mobile Pushbenachrichtigungen für Azure Service Health einrichten](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUw).
- Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](service-health-alert-webhook-guide.md).
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](service-notifications.md).
- Erfahren Sie mehr über [Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks](../azure-monitor/platform/alerts-rate-limiting.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md).
- Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](../azure-monitor/platform/alerts-overview.md), und erfahren Sie, wie Sie Warnungen empfangen können.
- Weitere Informationen zu [Aktionsgruppen](../azure-monitor/platform/action-groups.md).
