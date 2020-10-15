---
title: Definieren einer Regelhierarchie mithilfe einer Azure Firewall-Richtlinie
description: Erfahren Sie, wie Sie mithilfe einer Azure Firewall-Richtlinie eine Regelhierarchie definieren und Compliance erzwingen.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: how-to
ms.date: 08/26/2020
ms.author: victorh
ms.openlocfilehash: c290904c9f4bc7dba70dad9351dc45b676e0c236
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88893617"
---
# <a name="use-azure-firewall-policy-to-define-a-rule-hierarchy"></a>Definieren einer Regelhierarchie mithilfe einer Azure Firewall-Richtlinie

Sicherheitsadministratoren müssen Firewalls verwalten und Compliance über lokale und cloudbasierte Bereitstellungen hinweg sicherstellen. Eine wichtige Komponente ist die Möglichkeit, Anwendungsteams bei der Implementierung von CI/CD-Pipelines zum automatischen Erstellen von Firewallregeln mehr Flexibilität zu bieten.

Mithilfe von Azure Firewall-Richtlinien können Sie eine Regelhierarchie definieren und Compliance erzwingen:

- Stellt eine hierarchische Struktur bereit, um eine zentrale Basisrichtlinie auf einer untergeordneten Anwendungsteam-Richtlinie zu überlagern. Die Basisrichtlinie hat eine höhere Priorität und wird vor der untergeordneten Richtlinie ausgeführt.
- Verwenden Sie eine benutzerdefinierte Definition für rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC), um ein unbeabsichtigtes Entfernen der Basisrichtlinie zu verhindern und selektiven Zugriff auf Regelsammlungsgruppen innerhalb eines Abonnements oder einer Ressourcengruppe bereitzustellen. 

## <a name="solution-overview"></a>Lösungsübersicht

Die Hauptschritte für dieses Beispiel lauten wie folgt:

1. Erstellen einer Basis-Firewallrichtlinie in der Ressourcengruppe des Sicherheitsteams 
3. Definieren spezifischer Regeln für die IT-Sicherheit in der Basisrichtlinie. Dadurch wird ein allgemeiner Regelsatz zum Zulassen/Ablehnen von Datenverkehr hinzugefügt.
4. Erstellen von Anwendungsteam-Richtlinien, die die Basisrichtlinie erben 
5. Definieren von spezifischen Regeln für das Anwendungsteam in der Richtlinie. Sie können auch Regeln von bereits vorhandenen Firewalls migrieren.
6. Erstellen von benutzerdefinierten Azure Active Directory-Rollen zum Bereitstellen eines differenzierten Zugriffs auf die Regelsammlungsgruppe und Hinzufügen von Rollen im Bereich einer Firewallrichtlinie Im folgenden Beispiel können Vertriebsteammitglieder Regelsammlungsgruppen für die Firewallrichtlinie „Sales Teams“ bearbeiten. Das gleiche gilt für die Datenbank- und die Entwicklungsteams.
7. Ordnen Sie die Richtlinie der entsprechenden Firewall zu. Eine Azure-Firewall kann nur eine zugewiesene Richtlinie aufweisen. Dazu muss jedes Anwendungsteam über eine eigene Firewall verfügen.



:::image type="content" source="media/rule-hierarchy/contoso-teams.png" alt-text="Teams und Anforderungen" border="false":::

### <a name="create-the-firewall-policies"></a>Erstellen der Firewallrichtlinien

- Eine Basis-Firewallrichtlinie

Erstellen von Richtlinien für jedes Anwendungsteam:

- Eine Firewallrichtlinie „Sales“. Die Firewallrichtlinie „Sales“ erbt die Basis-Firewallrichtlinie.
- Eine Firewallrichtlinie „Database“. Die Firewallrichtlinie „Database“ erbt die Basis-Firewallrichtlinie.
- Eine Firewallrichtlinie „Engineering“. Die Firewallrichtlinie „Engineering“ erbt ebenfalls die Basis-Firewallrichtlinie.

:::image type="content" source="media/rule-hierarchy/policy-hierarchy.png" alt-text="Teams und Anforderungen" border="false":::

### <a name="create-custom-roles-to-access-the-rule-collection-groups"></a>Erstellen von benutzerdefinierten Rollen für den Zugriff auf die Regelsammlungsgruppen 

Benutzerdefinierte Rollen werden für jedes Anwendungsteam definiert. In der Rolle sind Vorgänge und der Gültigkeitsbereich definiert. Die Anwendungsteams dürfen Regelsammlungsgruppen für ihre jeweiligen Anwendungen bearbeiten.

Verwenden Sie die folgende allgemeine Vorgehensweise, um benutzerdefinierte Rollen zu definieren:

1. Rufen Sie das Abonnement ab:

   `Select-AzSubscription -SubscriptionId xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx`
2. Führen Sie den folgenden Befehl aus:

   `Get-AzProviderOperation "Microsoft.Support/*" | FT Operation, Description -AutoSize`
3. Verwenden Sie den Befehl Get-AzRoleDefinition, um die Rolle „Leser“ im JSON-Format auszugeben. 

   `Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole.json`
4. Öffnen Sie die Datei „ReaderSupportRole.json“ in einem Editor.

   Die JSON-Ausgabe sieht wie folgt aus. Informationen zu den verschiedenen Eigenschaften finden Sie unter  [Benutzerdefinierte Rollen für Azure-Ressourcen](../role-based-access-control/custom-roles.md).

```json
   { 
     "Name": "Reader", 
     "Id": "acdd72a7-3385-48ef-bd42-f606fba81ae7", 
     "IsCustom": false, 
     "Description": "Lets you view everything, but not make any changes.", 
     "Actions": [ 
      "*/read" 
     ], 
     "NotActions": [], 
     "DataActions": [], 
     "NotDataActions": [], 
     "AssignableScopes": [ 
       "/" 
     ] 
   } 
```
5. Bearbeiten Sie die JSON-Datei, und fügen Sie den 

   `*/read", "Microsoft.Network/*/read", "Microsoft.Network/firewallPolicies/ruleCollectionGroups/write` 

   Vorgang der Eigenschaft  **Aktionen** hinzu. Achten Sie darauf, ein Komma nach dem Lesevorgang einzufügen. Diese Aktion ermöglicht dem Benutzer das Erstellen und Aktualisieren von Regelsammlungsgruppen.
6. Fügen Sie in  **AssignableScopes** Ihre Abonnement-ID im folgenden Format ein: 

   `/subscriptions/xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx`

   Sie müssen explizite Abonnement-IDs hinzufügen. Andernfalls können Sie die Rolle nicht in Ihr Abonnement importieren.
7. Löschen Sie die Zeile mit der  **Id**-Eigenschaft, und ändern Sie die  **IsCustom**-Eigenschaft in TRUE.
8. Ändern Sie die Eigenschaften  **Name** und  **Description** in *Autoren der AZFM-Regelsammlungsgruppe* bzw. *Benutzer in dieser Rolle können Firewallrichtlinien-Regelsammlungsgruppen bearbeiten*.

Ihre JSON-Datei sollte dem folgenden Beispiel ähneln:

```
{ 

    "Name":  "AZFM Rule Collection Group Author", 
    "IsCustom":  true, 
    "Description":  "Users in this role can edit Firewall Policy rule collection groups", 
    "Actions":  [ 
                    "*/read", 
                    "Microsoft.Network/*/read", 
                     "Microsoft.Network/firewallPolicies/ruleCollectionGroups/write" 
                ], 
    "NotActions":  [ 
                   ], 
    "DataActions":  [ 
                    ], 
    "NotDataActions":  [ 
                       ], 
    "AssignableScopes":  [ 
                             "/subscriptions/xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx"] 
} 
```
9. Zum Erstellen der neuen benutzerdefinierten Rolle verwenden Sie den Befehl New-AzRoleDefinition und geben die JSON-Rollendefinitionsdatei an. 

   `New-AzRoleDefinition -InputFile "C:\CustomRoles\RuleCollectionGroupRole.json`

### <a name="list-custom-roles"></a>Auflisten benutzerdefinierter Rollen

Zum Auflisten aller benutzerdefinierten Rollen können Sie den Befehl Get-AzRoleDefinition verwenden:

   `Get-AzRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom`

Sie können die benutzerdefinierten Rollen auch im Azure-Portal anzeigen. Wechseln Sie zu Ihrem Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** , **Rollen** aus.

:::image type="content" source="media/rule-hierarchy/sales-app-policy.png" alt-text="Teams und Anforderungen":::

:::image type="content" source="media/rule-hierarchy/sales-app-policy-read.png" alt-text="Teams und Anforderungen":::

Weitere Informationen finden Sie im [Tutorial: Erstellen einer benutzerdefinierten Azure-Rolle mithilfe von Azure PowerShell](../role-based-access-control/tutorial-custom-role-powershell.md).

### <a name="add-users-to-the-custom-role"></a>Hinzufügen von Benutzern zur benutzerdefinierten Rolle

Im Portal können Sie der Rolle „Autoren der AZFM-Regelsammlungsgruppe“ Benutzer hinzufügen und Zugriff auf die Firewallrichtlinien bereitstellen.

1. Wählen Sie im Portal die Firewallrichtlinie für das Anwendungsteam (z. B. SalesAppPolicy) aus.
2. Wählen Sie **Zugriffssteuerung** aus.
3. Wählen Sie **Rollenzuweisung hinzufügen** aus.
4. Fügen Sie der Rolle Benutzer/Benutzergruppen (z. B. das Vertriebsteam) hinzu.

Wiederholen Sie diesen Vorgang für die anderen Firewallrichtlinien.

### <a name="summary"></a>Zusammenfassung

Die Firewallrichtlinie mit benutzerdefinierter RBAC bietet jetzt selektiven Zugriff auf Firewallrichtlinien-Regelsammlungsgruppen.

Benutzer haben keine Berechtigungen für Folgendes:
- Löschen der Azure Firewall- oder Firewallrichtlinie
- Aktualisieren der Firewallrichtlinien-Hierarchie oder der DNS-Einstellungen oder der Threat Intelligence
- Aktualisieren der Firewallrichtlinie, wenn sie dort nicht Mitglied der Gruppe „Autoren der AZFM-Regelsammlungsgruppe“ sind.

Sicherheitsadministratoren können die Basisrichtlinie verwenden, um entsprechend den Anforderungen ihres Unternehmens Integritätsschutz zu erzwingen und bestimmte Arten von Datenverkehr (z. B. ICMP) zu blockieren. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Azure Firewall-Richtlinien](policy-overview.md).

