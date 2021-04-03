---
title: Verschieben eines Service Bus-Namespace in eine andere Region | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einen Azure Service Bus-Namespace aus der aktuellen Region in eine andere verschieben.
ms.topic: how-to
ms.date: 06/23/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 573ea96437cf6cb76854ffa417fd3ad3fb86138b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88860427"
---
# <a name="move-an-azure-service-bus-namespace-to-another-region"></a>Verschieben eines Azure Service Bus-Namespace in eine andere Region
Es gibt verschiedene Szenarien, in denen Sie Ihren vorhandenen Azure Service Bus-Namespace aus einer Region in eine andere verschieben möchten. Beispielsweise könnte es sein, dass Sie für Testzwecke einen Namespace mit derselben Konfiguration erstellen möchten. Möglicherweise möchten Sie im Rahmen der [Planung der Notfallwiederherstellung](service-bus-geo-dr.md) auch einen sekundären Namespace in einer anderen Region einrichten.

Die allgemeinen Schritte sind folgende:

1. Exportieren Sie den Service Bus-Namespace in der aktuellen Region in eine Azure Resource Manager-Vorlage. 
1. Aktualisieren Sie den Speicherort für Ressourcen in der Vorlage. Entfernen Sie auch den Standardabonnementfilter aus der Vorlage, da Sie keine Standardregel erstellen können, da diese automatisch für Sie erstellt wird. 
1. Verwenden Sie die Vorlage, um den Service Bus-Namespace in der Zielregion bereitzustellen. 
1. Überprüfen Sie die Bereitstellung, um sicherzustellen, dass der Namespace, die Warteschlangen, Themen und Abonnements für Themen alle in der Zielregion erstellt werden. 
1. Schließen Sie die Verschiebung ab, indem Sie nach der Verarbeitung aller Nachrichten den Namespace aus der Quellregion löschen. 

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass Azure Service Bus und die Features, die von Ihrem Konto verwendet werden, in der Zielregion unterstützt werden.
 
## <a name="prepare"></a>Vorbereiten
Exportieren Sie zunächst eine Resource Manager-Vorlage. Diese Vorlage enthält Einstellungen, die Ihren Service Bus-Namespace beschreiben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Alle Ressourcen** und dann Ihren Service Bus-Namespace aus.
3. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.
4. Wählen Sie **Herunterladen** auf der Seite **Vorlage exportieren** aus.

    :::image type="content" source="./media/move-across-regions/download-template.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::
5. Suchen Sie die aus dem Portal heruntergeladene ZIP-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl. Diese ZIP-Datei enthält die JSON-Dateien mit den Vorlagen und Parametern. 
1. Öffnen Sie die Datei „template.json“ im extrahierten Ordner. 
1. Suchen Sie nach `location`, und ersetzen Sie den Wert für die Eigenschaft durch den neuen Namen für die Region oder den Standort. Informationen zum Abrufen von Standortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/). Der Code für eine Region ist der Name der Region ohne Leerzeichen, z. B. ist `West US` gleich `westus`.
1. Entfernen Sie Definitionen von Ressourcen des folgenden Typs: `Microsoft.ServiceBus/namespaces/topics/subscriptions/rules`. Vergessen Sie nicht, das Komma (`,`) vor diesem Abschnitt zu entfernen, damit der JSON-Code gültig bleibt.  

    > [!NOTE]
    > Sie können keine Standardregel für ein Abonnement erstellen, indem Sie eine Resource Manager-Vorlage verwenden. Die Standardregel wird automatisch erstellt, wenn das Abonnement in der Zielregion erstellt wird. 

## <a name="move"></a>Move
Stellen Sie die Vorlage bereit, um in der Zielregion einen Service Bus-Namespace zu erstellen. 

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Geben Sie in **Marketplace durchsuchen** die Zeichenfolge **Vorlagenbereitstellung** für den Suchtext ein, wählen Sie **Vorlagenbereitstellung (Bereitstellen mit benutzerdefinierten Vorlagen)** aus, und drücken Sie dann die **EINGABETASTE**.

    :::image type="content" source="./media/move-across-regions/new-template-deployment.png" alt-text="Neue Vorlagenbereitstellung":::    
1. Wählen Sie auf der Seite **Vorlagenbereitstellung** die Option **Erstellen** aus.

    :::image type="content" source="./media/move-across-regions/template-deployment-create-button.png" alt-text="Neue Vorlagenbereitstellung – Schaltfläche „Erstellen“":::        
1. Wählen Sie auf der Seite **Benutzerdefinierte Bereitstellung** die Option **Eigene Vorlage im Editor erstellen** aus.

    :::image type="content" source="./media/move-across-regions/build-template-link.png" alt-text="Eigene Vorlage im Editor erstellen – Link":::            
1. Wählen Sie auf der Seite **Vorlage bearbeiten** in der Symbolleiste **Datei laden**, und folgen Sie dann den Anweisungen, um die Datei **vorlage.json** zu laden, die Sie im letzten Abschnitt heruntergeladen haben.

    :::image type="content" source="./media/move-across-regions/select-template.png" alt-text="Auswählen einer Vorlage":::                
1. Wählen Sie **Speichern** aus, um die Vorlage zu speichern. 

    :::image type="content" source="./media/move-across-regions/save-template.png" alt-text="Vorlage speichern":::                    
1. Gehen Sie auf der Seite **Benutzerdefinierte Bereitstellung** wie folgt vor: 
    1. Wählen Sie ein Azure-**Abonnement** aus. 
    2. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine. 
    3. Wählen Sie den **Zielspeicherort** oder die Zielregion aus. Wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben, ist diese Einstellung schreibgeschützt. 
    4. Geben Sie einen neuen **Namen für den Namespace** ein.
    1. Klicken Sie auf **Überprüfen + erstellen**. 

        :::image type="content" source="./media/move-across-regions/deploy-template.png" alt-text="Bereitstellen der Resource Manager-Vorlage":::
    1. Wählen Sie unten auf der Seite **Überprüfen + erstellen** die Option **Erstellen** aus. 
    
## <a name="verify"></a>Überprüfung
1. Nachdem die Bereitstellung erfolgreich war, wählen Sie **Zu Ressourcengruppe wechseln** aus.

    :::image type="content" source="./media/move-across-regions/resource-group-navigation-link.png" alt-text="Zu Ressourcengruppe wechseln – Link":::    
1. Wählen Sie auf der Seite **Ressourcengruppe** den Service Bus-Namespace aus. 

    :::image type="content" source="./media/move-across-regions/select-namespace.png" alt-text="Service Bus-Namespace auswählen":::    
1. Überprüfen Sie auf der Seite **Service Bus-Namespace**, ob die Warteschlangen, Themen und Abonnements aus der Quellregion angezeigt werden. 
    1. **Warteschlangen** im Namespace werden am unteren Rand des rechten Bereichs angezeigt.         
    
        :::image type="content" source="./media/move-across-regions/queue-namespace.png" alt-text="Warteschlangen im Namespace":::
    2. Wechseln Sie zur Registerkarte **Themen**, um Themen im Namespace anzuzeigen.
    
        :::image type="content" source="./media/move-across-regions/topics-namespace.png" alt-text="Themen im Namespace":::
    3. Wählen Sie das Thema aus, um zu überprüfen, ob Abonnements erstellt wurden. 

        :::image type="content" source="./media/move-across-regions/topic-subscriptions.png" alt-text="Themenabonnements":::      
    
    

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen
Wenn Sie nach der Bereitstellung von vorne beginnen möchten, können Sie den **Service Bus-Zielnamespace** löschen und die in den Abschnitten [Vorbereiten](#prepare) und [Verschieben](#move) dieses Artikels beschriebenen Schritte wiederholen.

Um die Änderungen zu committen und das Verschieben eines Service Bus-Namespace abzuschließen, löschen Sie den **Service Bus-Quellnamespace**. Stellen Sie sicher, dass Sie alle Nachrichten verarbeiten, bevor Sie den Namespace löschen. 

So löschen Sie einen Service Bus-Quell- oder -Zielnamespace im Azure-Portal

1. Geben Sie im Suchfenster oben im Azure-Portal **Service Bus** ein, und wählen Sie in den Suchergebnissen **Service Bus** aus. Die Service Bus-Namespaces werden in einer Liste angezeigt.
2. Wählen Sie den zu löschenden Zielnamespace und dann auf der Symbolleiste **Löschen** aus. 

    ![Schaltfläche „Namespace löschen“](./media/move-across-regions/delete-namespace-button.png)
3. Überprüfen Sie auf der Seite **Ressourcen löschen** die ausgewählten Ressourcen, und bestätigen Sie den Löschvorgang durch Eingabe von **Ja**. Wählen Sie dann **Löschen** aus. 

    Eine andere Möglichkeit besteht darin, die Ressourcengruppe zu löschen, die über den Service Bus-Namespace verfügt. Wählen Sie auf der Seite **Ressourcengruppe** die Option **Ressourcengruppe löschen** in der Symbolleiste aus, und bestätigen Sie dann den Löschvorgang. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Service Bus-Namespace aus einer Region in eine andere verschoben und die Quellressourcen bereinigt.  Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:

- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md)
