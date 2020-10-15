---
title: Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen in Azure Monitor
description: Erstellen von Aktivitätsprotokollwarnungen über das Azure-Portal mit einer Azure Resource Manager-Vorlage und Azure PowerShell.
ms.topic: conceptual
ms.subservice: alerts
ms.date: 06/25/2019
ms.openlocfilehash: 7914f3ee57d014de2f34fca301b587c6297bebe3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90983088"
---
# <a name="create-view-and-manage-activity-log-alerts-by-using-azure-monitor"></a>Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor  

## <a name="overview"></a>Übersicht

Aktivitätsprotokollwarnungen werden aktiviert, wenn ein neues Aktivitätsprotokollereignis eintritt, das die in der Warnung angegebenen Bedingungen erfüllt.

Diese Warnungen gelten für Azure-Ressourcen und können mit einer Azure Resource Manager-Vorlage erstellt werden. Sie können auch im Azure-Portal erstellt, aktualisiert oder gelöscht werden. Normalerweise erstellen Sie Aktivitätsprotokollwarnungen, um Benachrichtigungen zu erhalten, wenn es für Ressourcen in Ihrem Azure-Abonnement zu spezifischen Änderungen kommt. Warnungen gelten häufig für bestimmte Ressourcengruppen oder Ressourcen. Es kann beispielsweise sein, dass Sie benachrichtigt werden möchten, wenn ein beliebiger virtueller Computer in der Beispielressourcengruppe **myProductionResourceGroup** gelöscht wird. Unter Umständen möchten Sie auch benachrichtigt werden, wenn einem Benutzer unter Ihrem Abonnement neue Rollen zugewiesen werden.

> [!IMPORTANT]
> Warnungen zur Service Health-Benachrichtigung können nicht über die Schnittstelle zur Erstellung von Aktivitätsprotokollwarnungen erstellt werden. Weitere Informationen zum Erstellen und Verwenden von Service Health-Benachrichtigungen finden Sie unter [Empfangen von Aktivitätsprotokollwarnungen zu Service Health-Benachrichtigungen](../../service-health/alerts-activity-log-service-notifications-portal.md).

Stellen Sie beim Erstellen von Warnungsregeln Folgendes sicher:

- Das Abonnement im Bereich unterscheidet sich nicht von dem Abonnement, unter dem die Warnung erstellt wird.
- Die Kriterien, für die die Warnung konfiguriert wird, müssen die Ereigniskategorie „Ebene“, „Status“, „Aufrufer“, „Ressourcengruppe“, „Ressourcen-ID“ oder „Ressourcentyp“ sein.
- Im JSON-Code für die Warnungskonfiguration gibt es keine Bedingung „anyOf“ oder geschaltete Bedingungen. Grundsätzlich ist nur eine „allOf“-Bedingung ohne weitere „allOf“- oder „anyOf“-Bedingungen zulässig.
- Wenn die Kategorie „Verwaltung“ lautet, müssen Sie in Ihrer Warnung mindestens ein Kriterium der oben genannten Kriterien angeben. Eine Warnung, die jedes Mal aktiviert wird, wenn ein Ereignis in den Aktivitätsprotokollen erstellt wird, kann nicht erstellt werden.
- Warnungen können für Ereignisse in der Kategorie „Warnung“ des Aktivitätsprotokolls nicht erstellt werden.

## <a name="azure-portal"></a>Azure-Portal

Sie können das Azure-Portal verwenden, um Warnungsregeln für das Aktivitätsprotokoll zu erstellen und zu ändern. Die Umgebung ist in ein Aktivitätsprotokoll von Azure integriert, um eine nahtlose Erstellung von Warnmeldungen für bestimmte Ereignisse zu gewährleisten, die für Sie von Interesse sind.

### <a name="create-with-the-azure-portal"></a>Erstellen mit dem Azure-Portal

Verwenden Sie das folgende Verfahren.

1. Wählen Sie im Azure-Portal die Option **Überwachen** > **Warnungen** aus.
2. Wählen Sie **Neue Warnungsregel** in der oberen linken Ecke des Fensters **Warnungen** aus.

     ![Neue Warnungsregel](media/alerts-activity-log/AlertsPreviewOption.png)

     Das Fenster **Regel erstellen** wird angezeigt.

      ![Optionen für neue Warnungsregeln](media/alerts-activity-log/create-new-alert-rule-options.png)

3. Geben Sie unter **Warnungsbedingung definieren** die folgenden Informationen an, und wählen Sie **Fertig** aus:

   - **Warnungsziel:** Um das Ziel für die neue Warnung anzuzeigen und auszuwählen, verwenden Sie **Nach Abonnement filtern** / **Nach Ressourcentyp filtern**. Wählen Sie die Ressource oder die Ressourcengruppe aus der angezeigten Liste aus.

     > [!NOTE]
     > 
     > Sie können nur vom [Azure Resource Manager](../../azure-resource-manager/management/overview.md) überwachte Ressourcen, Ressourcengruppen oder ein ganzes Abonnement für ein Aktivitätsprotokollsignal auswählen. 

     **Beispielansicht für Warnungsziel**

     ![Ziel auswählen](media/alerts-activity-log/select-target.png)

   - Wählen Sie unter **Zielkriterien** die Option **Kriterien hinzufügen** aus. Alle verfügbaren Signale für das Ziel werden angezeigt, einschließlich derjenigen aus verschiedenen Kategorien des **Aktivitätsprotokolls**. Der Kategoriename wird an den Namen des **Überwachungsdiensts** angefügt.

   - Wählen Sie das Signal aus der Liste der verschiedenen möglichen Operationen für den Typ **Aktivitätsprotokoll** aus.

     Sie können die Zeitachse für den Protokollverlauf und die entsprechende Warnungslogik für dieses Zielsignal auswählen:

     **Anzeige „Kriterien hinzufügen“**

     ![Hinzufügen von Kriterien](media/alerts-activity-log/add-criteria.png)
     
     > [!NOTE]
     > 
     >  Um hochwertige und effektive Regeln zu erhalten, sollten Sie den Regeln mindestens eine weitere Bedingung mit dem Signal „Alle administrativen“ hinzufügen. 
     > Als Teil der Definition der Warnung müssen Sie eines der Dropdownfelder ausfüllen: „Ereignisebene“, „Status“ oder „Initiiert von“, und so wird die Regel spezifischer.

     - **Verlaufszeit**: Die für den ausgewählten Vorgang verfügbaren Ereignisse können über die letzten 6, 12 bzw. 24 Stunden oder über die letzte Woche aufgezeichnet werden.

     - **Warnungslogik**:

       - **Ereignisstufe**: Der Schweregrad des Ereignisses: _Ausführlich_, _Information_, _Warnung_, _Fehler_ oder _Kritisch_.
       - **Status:** Der Status des Ereignisses: _Gestartet_, _Fehlgeschlagen_ oder _Erfolgreich_.
       - **Ereignis initiiert von:** Wird auch als „Aufrufer“ bezeichnet. Die E-Mail-Adresse oder der Azure Active Directory-Bezeichner des Benutzers, der den Vorgang durchgeführt hat.

       In diesem Beispielsignaldiagramm wird die Warnungslogik angewendet:

       ![Ausgewählten Kriterien](media/alerts-activity-log/criteria-selected.png)

4. Geben Sie unter **Details der Warnungsregel definieren** die folgenden Details an:

    - **Name der Warnungsregel**: Der Name für die neue Warnungsregel.
    - **Beschreibung**: Die Beschreibung für die Warnungsregel.
    - **Warnung in Ressourcengruppe speichern**: Wählen Sie die Ressourcengruppe aus, in der Sie die neue Regel speichern möchten.

5. Geben Sie unter **Aktionsgruppe** im Dropdownmenü die Aktionsgruppe an, der Sie die neue Warnungsregel zuweisen möchten. Alternativ hierzu können Sie auch [eine neue Aktionsgruppe erstellen](./action-groups.md) und der neuen Regel zuweisen. Klicken Sie zum Erstellen einer neuen Gruppe auf **+ Neue Gruppe**.

6. Klicken Sie zum Aktivieren der Regeln nach der Erstellung für **Regel beim Erstellen aktivieren** auf **Ja**.
7. Wählen Sie **Warnungsregel erstellen** aus.

    Die neue Warnungsregel für das Aktivitätsprotokoll wird erstellt, und oben rechts im Fenster wird eine Bestätigungsmeldung angezeigt.

    Sie können eine Regel aktivieren, deaktivieren, bearbeiten oder löschen. Erfahren Sie mehr über die Verwaltung von Aktivitätsprotokollregeln.


Eine einfache Analogie zum Verständnis der Bedingungen, unter denen Warnungsregeln in einem Aktivitätsprotokoll erstellt werden können, ist die Untersuchung oder Filterung von Ereignissen über das [Aktivitätsprotokoll im Azure-Portal](./activity-log.md#view-the-activity-log). Auf dem Bildschirm **Azure Monitor – Aktivitätsprotokoll** können Sie erforderliche Ereignisse filtern oder finden und dann eine Warnung mit der Schaltfläche **Aktivitätsprotokollwarnung hinzufügen** erstellen. Befolgen Sie dann die Schritte 4 bis 7 wie zuvor gezeigt.
    
 ![Hinzufügen einer Warnung aus dem Aktivitätsprotokoll](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-the-azure-portal"></a>Anzeigen und Verwalten im Azure-Portal

1. Wählen Sie im Azure-Portal die Option **Überwachen** > **Warnungen** aus. Wählen Sie **Warnungsregeln verwalten** in der oberen linken Ecke des Fensters aus.

    ![Screenshot: Aktivitätsprotokoll, in dem das Suchfeld hervorgehoben ist](media/alerts-activity-log/manage-alert-rules.png)

    Die Liste mit den verfügbaren Regeln wird angezeigt.

2. Suchen Sie nach der zu ändernden Aktivitätsprotokollregel.

    ![Suchen nach Regeln für Aktivitätsprotokollwarnungen](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    Sie können die verfügbaren Filter _Abonnement_, _Ressourcengruppe_, _Ressource_, _Signaltyp_ oder _Status_ verwenden, um die Aktivitätsregel zu ermitteln, die Sie bearbeiten möchten.

   > [!NOTE]
   > 
   > Sie können nur **Beschreibung**, **Zielkriterien** und **Aktionsgruppen** bearbeiten.

3. Wählen Sie die Regel aus, und doppelklicken Sie darauf, um die Regeloptionen zu bearbeiten. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie dann **Speichern** aus.

   ![Verwalten von Warnungsregeln](media/alerts-activity-log/activity-log-rule-edit-page.png)

4. Sie können eine Regel aktivieren, deaktivieren oder löschen. Wählen Sie oben im Fenster die entsprechende Option aus, nachdem Sie die Regel wie in Schritt 2 beschrieben ausgewählt haben.


## <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage
Um eine Aktivitätsprotokoll-Warnungsregel mithilfe einer Azure Resource Manager-Vorlage zu erstellen, müssen Sie eine Ressource des Typs `microsoft.insights/activityLogAlerts` erstellen. Anschließend tragen Sie alle zugehörigen Eigenschaften ein. Hier sehen Sie eine Vorlage, mit der eine Aktivitätsprotokoll-Warnungsregel erstellt wird:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
Der vorherige JSON-Beispielcode kann für diese exemplarische Vorgehensweise z.B. als „sampleActivityLogAlert.json“ gespeichert und mit [Azure Resource Manager im Azure-Portal](../../azure-resource-manager/templates/deploy-portal.md) bereitgestellt werden.

  > [!NOTE]
  > 
  > Beachten Sie, dass die höchste Ebene, auf der Aktivitätsprotokollwarnungen definiert werden können, das Abonnement ist.
  > Das bedeutet, dass es keine Möglichkeit gibt, eine Warnung für mehrere Abonnements zu definieren. Daher sollte die Definition eine Warnung pro Abonnement umfassen.

Bei den folgenden Feldern handelt es sich um die Optionen, die Sie in der Azure Resource Manager-Vorlage für die Bedingungsfelder verwenden können. Beachten Sie, dass „Resource Health“, „Advisor“ und „Service Health“ zusätzliche Eigenschaftenfelder für die jeweiligen speziellen Felder umfassen. 
1. resourceId:  Die Ressourcen-ID der betroffenen Ressource in dem Aktivitätsprotokollereignis, für das die Warnung generiert werden soll.
2. category: Die Kategorie des Aktivitätsprotokollereignisses. Beispiel: „Administrative“ (Verwaltung), „ServiceHealth“ (Dienstintegrität), „ResourceHealth“ (Ressourcenintegrität), „Autoscale“ (Autoskalierung), „Security“ (Sicherheit), „Recommendation“ (Empfehlung), „Policy“ (Richtlinie).
3. caller: Die E-Mail-Adresse oder der Azure Active Directory-Bezeichner des Benutzers, der den Vorgang des Aktivitätsprotokollereignisses durchgeführt hat.
4. level: Die Ebene der Aktivität in dem Aktivitätsprotokollereignis, für das die Warnung generiert werden soll. Beispiel: „Critical“ (Kritisch), „Error“ (Fehler), „Warning“ (Warnung), „Informational“ (Information) oder „Verbose“ (Ausführlich).
5. operationName: Der Name des Vorgangs im Aktivitätsprotokollereignis. Beispiel: Microsoft.Resources/deployments/write
6. resourceGroup: Der Name der Ressourcengruppe für die betroffene Ressource im Aktivitätsprotokollereignis.
7. resourceProvider: [Erläuterung zu Azure-Ressourcenanbietern und -typen](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-resource-manager%2Fmanagement%2Fresource-providers-and-types&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373543634&sdata=4RjpTkO5jsdOgPdt%2F%2FDOlYjIFE2%2B%2BuoHq5%2F7lHpCwQw%3D&reserved=0). Eine Liste, die Ressourcenanbieter zu Azure-Diensten zuordnet, finden Sie unter [Ressourcenanbieter für Azure-Dienste](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-resource-manager%2Fmanagement%2Fazure-services-resource-providers&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373553639&sdata=0ZgJPK7BYuJsRifBKFytqphMOxMrkfkEwDqgVH1g8lw%3D&reserved=0).
8. status: Eine Zeichenfolge, die den Status des Vorgangs im Aktivitätsereignis beschreibt. Beispiel: „Started“ (Gestartet), „In Progress“ (In Bearbeitung), „Succeeded“ (Erfolgreich), „Failed“ (Fehler), „Active“ (Aktiv), „Resolved“ (Gelöst)
9. subStatus: Üblicherweise der HTTP-Statuscode des entsprechenden REST-Aufrufs, kann aber auch weitere Zeichenfolgen zur Beschreibung eines untergeordneten Status enthalten.   Beispiel: OK (HTTP-Statuscode: 200), Erstellt (HTTP-Statuscode: 201), Akzeptiert (HTTP-Statuscode: 202), Kein Inhalt (HTTP-Statuscode: 204), Ungültige Anforderung (HTTP-Statuscode: 400), Nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode: 409), Interner Serverfehler (HTTP-Statuscode: 500), Dienst nicht verfügbar (HTTP-Statuscode: 503), Gatewaytimeout (HTTP-Statuscode: 504).
10. resourceType: Der Typ der Ressource, die vom Ereignis betroffen war. Beispiel: Microsoft.Resources/deployments

Beispiel:

```json
"condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        }

```
Weitere Informationen zu den Aktivitätsprotokollfeldern finden Sie [hier](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-monitor%2Fplatform%2Factivity-log-schema&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373563632&sdata=6QXLswwZgUHFXCuF%2FgOSowLzA8iOALVgvL3GMVhkYJY%3D&reserved=0).



> [!NOTE]
> Es kann bis zu 5 Minuten dauern, bis die neue Warnungsregel des Aktivitätsprotokolls aktiv wird.

## <a name="rest-api"></a>REST-API 
Die [Azure Monitor-Aktivitätsprotokollwarnungen-API](/rest/api/monitor/activitylogalerts) ist eine Rest-API. Sie ist vollständig mit der Azure Resource Manager-Rest-API kompatibel. Sie kann über PowerShell mit dem Resource Manager-Cmdlet oder der Azure CLI verwendet werden.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="deploy-the-resource-manager-template-with-powershell"></a>Bereitstellen der Resource Manager-Vorlage mit PowerShell
Um PowerShell zum Bereitstellen der Resource Manager-Beispielvorlage zu verwenden, die im vorherigen Abschnitt [Azure Resource Manager-Vorlage](#azure-resource-manager-template) gezeigt wurde, verwenden Sie den folgenden Befehl:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```

Dabei enthält die Datei „sampleActivityLogAlert.parameters.json“ die Werte für die Parameter, die für die Erstellung der Warnungsregeln erforderlich sind.

### <a name="use-activity-log-powershell-cmdlets"></a>Verwenden von PowerShell-Aktivitätsprotokoll-Cmdlets

In Aktivitätsprotokollwarnungen stehen folgende dedizierte PowerShell-Cmdlets zur Verfügung:

- [Set-AzActivityLogAlert](/powershell/module/az.monitor/set-azactivitylogalert): Erstellt eine neue Aktivitätsprotokollwarnung oder aktualisiert eine vorhandene Aktivitätsprotokollwarnung.
- [Get-AzActivityLogAlert](/powershell/module/az.monitor/get-azactivitylogalert): Ruft mindestens eine Aktivitätsprotokoll-Warnungsressource ab.
- [Enable-AzActivityLogAlert](/powershell/module/az.monitor/enable-azactivitylogalert): Aktiviert eine vorhandene Aktivitätsprotokollwarnung und legt deren Tags fest.
- [Disable-AzActivityLogAlert](/powershell/module/az.monitor/disable-azactivitylogalert): Deaktiviert eine vorhandene Aktivitätsprotokollwarnung und legt deren Tags fest.
- [Remove-AzActivityLogAlert](/powershell/module/az.monitor/remove-azactivitylogalert): Entfernt eine Aktivitätsprotokollwarnung.

## <a name="azure-cli"></a>Azure CLI

Unter [az monitor activity-log alert](/cli/azure/monitor/activity-log/alert) stehen dedizierte Azure CLI-Befehle zum Verwalten von Aktivitätsprotokoll-Warnungsregeln zur Verfügung.

Verwenden Sie für die Erstellung einer neuen Warnungsregel des Aktivitätsprotokolls die folgenden Befehle in der angegebenen Reihenfolge:

1. [az monitor activity-log alert create:](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create) Erstellen einer neuen Ressource für Warnungsregeln des Aktivitätsprotokolls.
1. [az monitor activity-log alert scope:](/cli/azure/monitor/activity-log/alert/scope) Hinzufügen des Bereichs für die erstellte Warnungsregel des Aktivitätsprotokolls.
1. [az monitor activity-log alert action-group:](/cli/azure/monitor/activity-log/alert/action-group) Hinzufügen einer Aktionsgruppe zur Warnungsregel des Aktivitätsprotokolls.

Zum Abrufen einer Ressource für Warnungsregeln des Aktivitätsprotokolls verwenden Sie den Azure CLI-Befehl [az monitor activity-log alert show](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
). Zum Anzeigen aller Ressourcen für Warnungsregeln des Aktivitätsprotokolls in einer Ressourcengruppe verwenden Sie den Befehl [az monitor activity-log alert list](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list).
Ressourcen für Warnungsregeln des Aktivitätsprotokolls können mit dem Azure CLI-Befehl [az monitor activity-log alert delete](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete) entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Webhookschema für Aktivitätsprotokolle](./activity-log-alerts-webhook.md).
- Lesen Sie eine [Übersicht über Aktivitätsprotokolle](./activity-log-alerts.md).
- Weitere Informationen zu [Aktionsgruppen](./action-groups.md).  
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](../../service-health/service-notifications.md).

