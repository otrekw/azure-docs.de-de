---
title: Verschieben von Azure Event Grid-Domänen in eine andere Region
description: In diesem Artikel erfahren Sie, wie Sie Azure Event Grid-Domänen aus der aktuellen Region in eine andere Region verschieben.
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/20/2020
ms.openlocfilehash: fff8638a819511f84f15c52ad0695cdd5759f971
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89082214"
---
# <a name="move-azure-event-grid-domains-to-another-region"></a>Verschieben von Azure Event Grid-Domänen in eine andere Region
Sie möchten Ihre Ressourcen ggf. aus verschiedenen Gründen in eine andere Region verschieben. Beispielsweise, um eine neue Azure-Region zu nutzen, interne Richtlinien- und Governanceanforderungen zu erfüllen oder als Reaktion auf Anforderungen der Kapazitätsplanung. 

Folgende allgemeine Schritte werden in diesem Artikel behandelt: 

- **Exportieren der Domänenressource** in eine Azure Resource Manager-Vorlage. 

    > [!IMPORTANT]
    > Die Domänenressource und die Themen in der Domäne werden in die Vorlage exportiert. Abonnements von Domänenthemen werden nicht exportiert. 
- **Verwenden der Vorlage zum Bereitstellen der Domäne** in der Zielregion. 
- **Manuelles Erstellen von Abonnements für Domänenthemen** in der Zielregion. Wenn Sie die Domäne in eine Vorlage in der aktuellen Region exportiert haben, werden Abonnements für Domänenthemen nicht exportiert. Erstellen Sie diese daher nach der Erstellung der Domäne und der Domänenthemen in der Zielregion. 
- **Überprüfen der Bereitstellung**. Senden Sie ein Ereignis an ein Domänenthema in der Domäne, und vergewissern Sie sich, dass der dem Abonnement zugeordnete Ereignishandler aufgerufen wird. 
- Um den **Verschiebevorgang abzuschließen**, löschen Sie die Domäne aus der Quellregion. 

## <a name="prerequisites"></a>Voraussetzungen
- Stellen Sie sicher, dass der Event Grid-Dienst in der Zielregion verfügbar ist. Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all).

## <a name="prepare"></a>Vorbereiten
Exportieren Sie zunächst eine Resource Manager-Vorlage für die Domäne. 

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an.
2. Geben Sie in der Suchleiste **Event Grid-Domänen** ein, und wählen Sie dann **Event Grid-Domänen** aus der Ergebnisliste aus. 

    :::image type="content" source="./media/move-domains-across-regions/search-domains.png" alt-text="Event Grid-Domänen suchen und auswählen":::
3. Wählen Sie die **Domäne** aus, die Sie in eine Resource Manager-Vorlage exportieren möchten. 

    :::image type="content" source="./media/move-domains-across-regions/select-domain.png" alt-text="Auswählen der Domäne":::   
4. Wählen Sie auf der Seite **Event Grid-Domäne** die Option **Vorlage exportieren** unter **Einstellungen** im linken Menü aus, und wählen Sie dann **Herunterladen** auf der Symbolleiste aus. 

    :::image type="content" source="./media/move-domains-across-regions/export-template-download.png" alt-text="Vorlage exportieren > Herunterladen" lightbox="./media/move-domains-across-regions/export-template-download.png":::   

    > [!IMPORTANT]
    > Domänen und Domänenthemen werden exportiert. Abonnements für Domänenthemen werden nicht exportiert. Daher müssen Sie nach dem Verschieben von Domänenthemen Abonnements für Domänenthemen erstellen. 
5. Suchen Sie die aus dem Portal heruntergeladene **ZIP**-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl. Diese ZIP-Datei enthält die JSON-Dateien mit Vorlagen und Parametern. 
1. Öffnen Sie die Datei **template.json** in einem Editor Ihrer Wahl. 
8. Aktualisieren Sie `location` für die **Domänenressource** in die Zielregion oder den Zielspeicherort. Informationen zum Abrufen von Standortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/). Der Code für eine Region ist der Name der Region ohne Leerzeichen, z. B. ist `West US` gleich `westus`.

    ```json
    "type": "Microsoft.EventGrid/domains",
    "apiVersion": "2020-06-01",
    "name": "[parameters('domains_spegriddomain_name')]",
    "location": "westus",
    ```
1. **Speichern** Sie die Vorlage. 

## <a name="recreate"></a>Neu erstellen 
Stellen Sie die Vorlage bereit, und die Domäne und Domänenthemen in der Zielregion zu erstellen. 

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.
3. Wählen Sie **Vorlagenbereitstellung** aus.
4. Klicken Sie auf **Erstellen**.
5. Wählen Sie **Eigene Vorlage im Editor erstellen**.
6. Wählen Sie **Datei laden** aus, und befolgen Sie dann die Anweisungen zum Laden der Datei **template.json**, die Sie im vorherigen Abschnitt heruntergeladen haben.
7. Wählen Sie **Speichern** aus, um die Vorlage zu speichern. 
8. Gehen Sie auf der Seite **Benutzerdefinierte Bereitstellung** wie folgt vor:
    1. Wählen Sie ein Azure-**Abonnement** aus. 
    1. Wählen Sie eine vorhandene **Ressourcengruppe** in der Zielregion aus, oder erstellen Sie eine Ressourcengruppe. 
    1. Wählen als **Region** die Zielregion aus. Wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben, ist diese Einstellung schreibgeschützt. 
    1. Geben Sie als **Domänennamen** einen neuen Namen für die Domäne ein. 
    1. Klicken Sie auf **Überprüfen + erstellen**. 
    
        :::image type="content" source="./media/move-domains-across-regions/deploy-template.png" alt-text="Bereitstellen der Vorlage":::        
    1. Nachdem die Überprüfung der Vorlage erfolgreich war, wählen Sie **Erstellen** unten auf der Seite aus, um die Ressource bereitzustellen. 
    1. Nachdem die Bereitstellung erfolgreich war, wählen Sie **Zu Ressourcengruppe wechseln** aus, um zur Seite „Ressourcengruppe“ zu navigieren. Vergewissern Sie sich, dass eine Domäne in der Ressourcengruppe vorhanden ist. Wählen Sie die Domäne aus. Bestätigen Sie, dass Domänenthemen in der Domäne vorhanden sind. 

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen
Um den Verschiebevorgang abzuschließen, löschen Sie die Domäne in der Quellregion.  

Wenn Sie von vorn beginnen möchten, löschen Sie die Domäne in der Zielregion, und wiederholen Sie die Schritte in den Abschnitten [Vorbereiten](#prepare) und [Neu erstellen](#recreate) dieses Artikels.

So löschen Sie eine Domäne über das Azure-Portal:

1. Geben Sie im Suchfenster oben im Azure-Portal **Event Grid-Domänen** ein, und wählen Sie in den Suchergebnissen **Event Grid-Domänen** aus. 
2. Wählen Sie die zu löschende Domäne und dann auf der Symbolleiste **Löschen** aus. 
3. Geben Sie auf der Bestätigungsseite den Namen der Ressourcengruppe ein, und wählen Sie **Löschen** aus.  

So löschen Sie die Ressourcengruppe, die die Domäne enthält, mithilfe des Azure-Portals:

1. Geben Sie im Suchfenster oben im Azure-Portal **Ressourcengruppen** ein, und wählen Sie in den Suchergebnissen **Ressourcengruppen** aus. 
2. Wählen Sie die zu löschende Ressourcengruppe und dann auf der Symbolleiste **Löschen** aus. 
3. Geben Sie auf der Bestätigungsseite den Namen der Ressourcengruppe ein, und wählen Sie **Löschen** aus.  

## <a name="next-steps"></a>Nächste Schritte
Sie haben gelernt, wie eine Event Grid-Domäne aus einer Region in eine andere Region verschoben wird. In den folgenden Artikeln finden Sie Informationen zum regionsübergreifenden Verschieben von Systemthemen, benutzerdefinierten Themen und Partnernamespaces.

- [Regionsübergreifendes Verschieben von Systemthemen](move-system-topics-across-regions.md). 
- [Regionsübergreifendes Verschieben von benutzerdefinierten Themen](move-custom-topics-across-regions.md). 
- [Regionsübergreifendes Verschieben von Partnernamespaces](move-partner-namespaces-across-regions.md).

Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie im folgenden Artikel: [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md).