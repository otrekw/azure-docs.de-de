---
title: include file
description: include file
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 03/19/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: a00291182059506aeab9cde965fa4cbd5177ecf7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132124"
---
Wenn ein Benutzer nicht über die erforderlichen Zugriffsberechtigungen zum Anwenden von Tags verfügt, können Sie dem Benutzer die Rolle **Tag Contributor** (Tagmitwirkender) zuweisen. Weitere Informationen finden Sie im [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer mit RBAC und dem Azure-Portal](../articles/role-based-access-control/quickstart-assign-role-user-portal.md).

1. Um die Tags für eine Ressource oder Ressourcengruppe anzuzeigen, suchen Sie in der Übersicht nach vorhandenen Tags. Wenn Sie noch keine Tags angewandt haben, ist die Liste leer.

   ![Anzeigen der Tags für eine Ressource oder Ressourcengruppe](./media/resource-manager-tag-resources/view-tags.png)

1. Um ein Tag hinzuzufügen, wählen Sie **Klicken Sie hier, um Markierungen hinzuzufügen.** aus.

1. Geben Sie einen Namen und einen Wert an.

   ![Hinzufügen eines Tags](./media/resource-manager-tag-resources/add-tag.png)

1. Fügen Sie weitere Tags nach Bedarf hinzu. Wählen Sie dann die Option **Schließen**.

   ![Speichern von Tags](./media/resource-manager-tag-resources/save-tags.png)

1. Die Tags werden jetzt in der Übersicht angezeigt.

   ![Anzeigen von Tags](./media/resource-manager-tag-resources/view-new-tags.png)

1. Um ein Tag hinzuzufügen oder zu löschen, wählen Sie **Ändern** aus.

1. Um ein Tag zu löschen, wählen Sie das Papierkorbsymbol aus. Wählen Sie anschließend **Speichern** aus.

   ![Löschen eines Tags](./media/resource-manager-tag-resources/delete-tag.png)

So führen Sie einen Massenzuweisung von Tags zu mehreren Ressourcen durch

1. Aktivieren Sie in einer Liste von Ressourcen das Kontrollkästchen für die Ressourcen, denen Sie das Tag zuweisen möchten. Wählen Sie dann **Tags zuweisen** aus.

   ![Auswählen mehrerer Ressourcen](./media/resource-manager-tag-resources/select-multiple-resources.png)

1. Fügen Sie Namen und Werte hinzu. Wählen Sie dann die Option **Schließen**.

   ![Auswählen von „Zuweisen“](./media/resource-manager-tag-resources/select-assign.png)

So zeigen Sie alle Ressourcen mit einem Tag an

1. Wählen Sie im Menü des Azure-Portals die Option **Tags** aus. Wählen Sie es aus den verfügbaren Optionen aus.

   ![Suchen nach Tag](./media/resource-manager-tag-resources/find-tags-general.png)

1. Wählen Sie das Tag zum Anzeigen von Ressourcen aus.

   ![Auswählen eines Tags](./media/resource-manager-tag-resources/select-tag.png)

1. Alle Ressourcen mit diesem Tag werden angezeigt.

   ![Anzeigen von Ressourcen nach Tag](./media/resource-manager-tag-resources/view-resources-by-tag.png)
