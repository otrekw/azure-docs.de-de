---
title: Erstellen von Ressourcenintegritätswarnungen über das Azure-Portal
description: Erstellen Sie über das Azure-Portal Warnungen, die Sie informieren, wenn Ihre Azure-Ressourcen nicht mehr verfügbar sind.
ms.topic: conceptual
ms.date: 6/23/2020
ms.openlocfilehash: e48c400e5be3516b08496db7a4cb6a19e45d6c97
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100594634"
---
# <a name="configure-resource-health-alerts-using-azure-portal"></a>Konfigurieren von Ressourcenintegritätswarnungen mithilfe des Azure-Portals

Dieser Artikel zeigt, wie Sie Aktivitätsprotokollwarnungen für Ressourcenintegritätsbenachrichtigungen im Azure-Portal einrichten.

Azure Resource Health informiert Sie über den aktuellen und den vergangenen Integritätsstatus Ihrer Azure-Ressourcen. Azure Resource Health-Warnungen können Sie nahezu in Echtzeit informieren, wenn sich der Integritätsstatus dieser Ressourcen ändert. Die programmgesteuerte Erstellung von Resource Health-Warnungen ermöglicht Benutzern das Massenerstellen und -anpassen von Warnungen.

Ressourcenintegritätsbenachrichtigungen werden im [Azure-Aktivitätsprotokoll](../azure-monitor/essentials/platform-logs-overview.md) gespeichert. Angesichts der möglicherweise großen Menge von Informationen, die im Aktivitätsprotokoll gespeichert werden, ist eine separate Benutzeroberfläche vorhanden, um die Anzeige und Einrichtung von Benachrichtigungen zur Ressourcenintegrität zu vereinfachen.
Sie können eine Warnung erhalten, wenn eine Azure-Ressource Benachrichtigungen zur Ressourcenintegrität an Ihr Azure-Abonnement sendet. Sie können die Warnung konfigurieren auf der Grundlage von:

* Dem betroffenen Abonnement.
* Den betroffenen Ressourcentypen.
* Den betroffenen Ressourcengruppen.
* Den betroffenen Ressourcen.
* Dem Ereignisstatus der betroffenen Ressourcen.
* Den betroffenen Status der Ressourcen.
* Den Ursachentypen der betroffenen Ressourcen.

Sie können auch konfigurieren, an wen die Warnung gesendet werden soll:

* Wählen Sie eine vorhandene Aktionsgruppe aus.
* Erstellen Sie eine neue Aktionsgruppe (die für zukünftige Warnungen verwendet werden kann).

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../azure-monitor/alerts/action-groups.md).

Informationen zum Konfigurieren von Ressourcenintegritätsbenachrichtigungs-Warnungen mithilfe von Azure Resource Manager-Vorlagen finden Sie unter [Resource Manager-Vorlagen](./resource-health-alert-arm-template-guide.md).
Ressourcenintegritätswarnung über das Azure-Portal

## <a name="resource-health-alert-using-azure-portal"></a>Ressourcenintegritätswarnung über das Azure-Portal

1. Wählen Sie im Azure-[Portal](https://portal.azure.com/) die Option **Service Health** aus.

    ![Auswahl von „Service Health“](./media/resource-health-alert-monitor-guide/service-health-selection.png)
2. Wählen Sie im Abschnitt **Resource Health** die Option **Service Health** aus.
3. Wählen Sie **Add resource health alert** (Ressourcenintegritätswarnung hinzufügen) aus, und füllen Sie die Felder aus.
4. Wählen Sie unter „Warnungsziel“ Angaben für **Abonnement**, **Ressourcentypen**, **Ressourcengruppen** und **Ressourcen** aus, für die Sie benachrichtigt werden möchten.

    ![Auswahl des Ziels](./media/resource-health-alert-monitor-guide/alert-target.png)

5. Wählen Sie unter „Warnungsbedingung“ Folgendes aus:
    1. Den **Ereignistyp**, für den Sie benachrichtigt werden möchten. Der Schweregrad des Ereignisses: „Aktiv“, „Gelöst“, „In Bearbeitung“, „Aktualisiert“.
    2. Den **Ressourcenstatus**, für den Sie benachrichtigt werden möchten. Der Ressourcenstatus des Ereignisses: „Verfügbar“, „Nicht verfügbar“, „Unbekannt“, „Beeinträchtigt“
    3. Den **Ursachentyp**, für den Sie benachrichtigt werden möchten. Die Ursache des Ereignisses: „Plattformseitig ausgelöst“, „Benutzerseitig ausgelöst“. ![Auswahl der Warnungsbedingung, Auswahl der Integrität](./media/resource-health-alert-monitor-guide/alert-condition.png)
6. Geben Sie unter „Details der Warnungsregel definieren“ die folgenden Details an:
    1. **Name der Warnungsregel**: Der Name für die neue Warnungsregel.
    2. **Beschreibung**: Die Beschreibung für die Warnungsregel.
    3. **Warnung in Ressourcengruppe speichern**: Wählen Sie die Ressourcengruppe aus, in der Sie die neue Regel speichern möchten.
7. Geben Sie unter **Aktionsgruppe** im Dropdownmenü die Aktionsgruppe an, der Sie die neue Warnungsregel zuweisen möchten. Alternativ hierzu können Sie auch [eine neue Aktionsgruppe erstellen](../azure-monitor/alerts/action-groups.md) und der neuen Regel zuweisen. Wählen Sie zum Erstellen einer neuen Gruppe **+ Neue Gruppe** aus.
8. Klicken Sie zum Aktivieren der Regeln nach der Erstellung für **Regel beim Erstellen aktivieren** auf **Ja**.
9. Wählen Sie **Warnungsregel erstellen** aus.

Die neue Warnungsregel für das Aktivitätsprotokoll wird erstellt, und oben rechts im Fenster wird eine Bestätigungsmeldung angezeigt.
Sie können eine Regel aktivieren, deaktivieren, bearbeiten oder löschen. Erfahren Sie mehr über das [Verwalten von Aktivitätsprotokollregeln](../azure-monitor/alerts/alerts-activity-log.md#view-and-manage-in-the-azure-portal).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Resource Health:

* [Übersicht über Azure Resource Health](Resource-health-overview.md)
* [Über Azure Resource Health verfügbare Ressourcentypen und Integritätsüberprüfungen](resource-health-checks-resource-types.md)

Erstellen von Service Health-Warnungen:

* [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](./alerts-activity-log-service-notifications-portal.md) 
* [Ereignisschema des Azure-Aktivitätsprotokolls](../azure-monitor/essentials/activity-log-schema.md)
* [Konfigurieren von Ressourcenintegritätswarnungen mithilfe von Resource Manager-Vorlagen](./resource-health-alert-arm-template-guide.md)
