---
title: Verschieben von Azure Event Grid-Partnernamespaces in eine andere Region
description: In diesem Artikel erfahren Sie, wie Sie Azure Event Grid-Partnernamespaces aus der aktuellen Region in eine andere Region verschieben.
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/20/2020
ms.openlocfilehash: 6783db6b9bb1c7d48b308234a179925d6f30e281
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89082211"
---
# <a name="move-azure-event-grid-partner-namespaces-to-another-region"></a>Verschieben von Azure Event Grid-Partnernamespaces in eine andere Region
Sie möchten Ihre Ressourcen ggf. aus verschiedenen Gründen in eine andere Region verschieben. Beispielsweise, um eine neue Azure-Region zu nutzen, interne Richtlinien- und Governanceanforderungen zu erfüllen oder als Reaktion auf Anforderungen der Kapazitätsplanung. 

Folgende allgemeine Schritte werden in diesem Artikel behandelt: 

- **Exportieren der Partnernamespaceressource** in eine Azure Resource Manager-Vorlage. Löschen von Definitionen für Ereigniskanalressourcen in der Vorlage. Ein Ereigniskanal kann einen Verweis auf die Azure Resource Manager-ID des Partnerthemas enthalten, das sich im Besitz eines Kunden befindet. Daher kann er nicht mithilfe der Vorlage in der Zielregion erstellt werden.  
- **Verwenden der Vorlage, um den Partnernamespace in der Zielregion bereitzustellen**. Erstellen Sie dann Ereigniskanäle im neuen Partnernamespace in der Zielregion. 
- Um den **Verschiebevorgang abzuschließen**, löschen Sie den Partnernamespace aus der Quellregion. 

    > [!NOTE]
    > - Das Exportieren von **Partnerthemen** in eine Azure Resource Manager-Vorlage wird nicht unterstützt, da Kunden kein Partnerthema direkt erstellen können. 
    > - **Partnerregistrierungen** sind globale Ressourcen (nicht an eine bestimmte Region gebunden), sodass das Verschieben aus einer Region in eine andere Region nicht anwendbar ist. 

## <a name="prerequisites"></a>Voraussetzungen
- Stellen Sie sicher, dass der Event Grid-Dienst in der Zielregion verfügbar ist. Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all).

## <a name="prepare"></a>Vorbereiten
Exportieren Sie zunächst eine Resource Manager-Vorlage für den Partnernamespace. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Geben Sie oben in der Suchleiste **Event Grid-Partnernamespaces** ein, und wählen Sie **Event Grid-Partnernamespaces** aus der Ergebnisliste aus. 
3. Wählen Sie den **Partnernamespace** aus, den Sie in eine Resource Manager-Vorlage exportieren möchten. 
4. Wählen Sie auf der Seite **Event Grid-Partnernamespace** die Option **Vorlage exportieren** unter **Einstellungen** im linken Menü aus, und wählen Sie dann **Herunterladen** auf der Symbolleiste aus. 

    :::image type="content" source="./media/move-partner-namespaces-across-regions/download-template.png" alt-text="Vorlage exportieren > Herunterladen" lightbox="./media/move-partner-namespaces-across-regions/download-template.png":::   
5. Suchen Sie die aus dem Portal heruntergeladene **ZIP**-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl. Diese ZIP-Datei enthält die JSON-Dateien mit Vorlagen und Parametern. 
1. Öffnen Sie die Datei **template.json** in einem Editor Ihrer Wahl. 
8. Aktualisieren Sie `location` für die **Themaressource** in die Zielregion oder den Zielspeicherort. Informationen zum Abrufen von Standortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/). Der Code für eine Region ist der Name der Region ohne Leerzeichen, z. B. ist `West US` gleich `westus`.

    ```json
    {
        "type": "Microsoft.EventGrid/partnerNamespaces",
        "apiVersion": "2020-04-01-preview",
        "name": "[parameters('partnerNamespace_name')]",
        "location": "westus",
        "properties": {
            "partnerRegistrationFullyQualifiedId": "[parameters('partnerRegistrations_ContosoCorpAccount1_externalid')]"
        }
    }
    ``` 
1. **Speichern** Sie die Vorlage. 

## <a name="recreate"></a>Neu erstellen 
Stellen Sie die Vorlage bereit, um in der Zielregion einen Partnernamespace zu erstellen. 

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
    1. Wählen als **Speicherort** die Zielregion aus. Wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben, ist diese Einstellung schreibgeschützt. 
    1. Geben Sie als **Namen des Partnernamespace** einen Namen für den neuen Partnernamespace ein. 
    1. Geben Sie für die externe ID der Partnerregistrierung die Ressourcen-ID der Partnerregistrierung im folgenden Format ein: `/subscriptions/<Azure subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.EventGrid/partnerRegistrations/<Partner registration name>`.
    1. Aktivieren Sie das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.     
    1. Wählen Sie **Überprüfen und erstellen** aus, um den Bereitstellungsprozess zu starten. 
    1. Überprüfen Sie die Einstellungen auf der Seite **Überprüfen und erstellen**, und wählen Sie **Erstellen** aus. 

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen
Um den Verschiebevorgang abzuschließen, löschen Sie den Partnernamespace in der Quellregion.  

Wenn Sie von vorn beginnen möchten, löschen Sie den Partnernamespace in der Zielregion, und wiederholen Sie die Schritte in den Abschnitten [Vorbereiten](#prepare) und [Neu erstellen](#recreate) dieses Artikels.

So löschen Sie einen Partnernamespace über das Azure-Portal:

1. Geben Sie oben im Suchfenster im Azure-Portal **Event Grid-Partnernamespaces** ein, und wählen Sie **Event Grid-Partnernamespaces** aus der Ergebnisliste aus. 
2. Wählen Sie den zu löschenden Partnernamespace und dann auf der Symbolleiste **Löschen** aus. 
3. **Bestätigen** Sie den Löschvorgang, um den Partnernamespace zu löschen. 

## <a name="next-steps"></a>Nächste Schritte
Sie haben gelernt, wie ein Event Grid-Partnernamespace aus einer Region in eine andere Region verschoben wird. In den folgenden Artikeln finden Sie Informationen zum regionsübergreifenden Verschieben von Systemthemen, benutzerdefinierten Themen und Domänen.

- [Regionsübergreifendes Verschieben von Systemthemen](move-system-topics-across-regions.md). 
- [Regionsübergreifendes Verschieben von benutzerdefinierten Themen](move-custom-topics-across-regions.md). 
- [Regionsübergreifendes Verschieben von Domänen](move-domains-across-regions.md).

Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie im folgenden Artikel: [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md).