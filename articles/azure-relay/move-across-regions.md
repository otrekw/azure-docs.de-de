---
title: Verschieben eines Azure Relay-Namespace in eine andere Region
description: In diesem Artikel erfahren Sie, wie Sie einen Azure Relay-Namespace aus der aktuellen Region in eine andere Region verschieben.
ms.topic: how-to
ms.date: 09/03/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 60a182764639341fcda159356dd9fe6c65cfabd9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89463321"
---
# <a name="move-an-azure-relay-namespace-to-another-region"></a>Verschieben eines Azure Relay-Namespace in eine andere Region
In diesem Artikel erfahren Sie, wie Sie einen Azure Relay-Namespace aus der aktuellen Region in eine andere Region verschieben. Die allgemeinen Schritte sind folgende:

1. **Exportieren des Relaynamespace** in eine Azure Resource Manager-Vorlage.
1. **Aktualisieren des Standorts (Region)** für Ressourcen in der Vorlage. Löschen Sie außerdem alle **dynamischen** WCF-Relays aus der Vorlage. 

    WCF-Relays verfügen über zwei Modi. Im ersten Modus wird das WCF-Relay explizit mithilfe des Azure-Portals oder einer Azure Resource Manager-Vorlage erstellt. Auf der Seite **WCF-Relays** im Azure-Portal sehen Sie, dass die Eigenschaft **isDynamic** für ein Relay in diesem Modus auf **FALSE** festgelegt ist. 

    Im zweiten Modus wird das WCF-Relay automatisch generiert, wenn ein Listener (Server) eine Verbindung mit einer bestimmten Endpunktadresse herstellt. Solange der Listener mit dem Relay verbunden ist, wird das Relay in der Liste der WCF-Relays im Azure-Portal angezeigt. Für ein Relay in diesem Modus wird die **isDynamic**-Eigenschaft auf **TRUE** festgelegt, da sie dynamisch generiert wird. Das dynamische WCF-Relay wird nicht mehr angezeigt, wenn der Listener die Verbindung trennt. 
1. Führen Sie die **Bereitstellung** von Ressourcen in der Zielregion mithilfe der Vorlage aus.

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass der Azure Relay-Dienst in der Zielregion verfügbar ist. Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=service-bus&regions=all). 
 
## <a name="prepare"></a>Vorbereiten
Exportieren Sie zunächst eine Resource Manager-Vorlage. Diese Vorlage enthält Einstellungen, die Ihren Azure Relay-Namespace beschreiben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Alle Ressourcen** und dann Ihren Azure Relay-Namespace aus.
3. Wählen Sie im linken Menü unter **Einstellungen** die Option **Vorlage** exportieren aus.
4. Wählen Sie **Herunterladen** auf der Seite **Vorlage exportieren** aus.

    :::image type="content" source="./media/move-across-regions/download-template.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::
5. Suchen Sie die aus dem Portal heruntergeladene ZIP-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl. Diese ZIP-Datei enthält die JSON-Dateien mit den Vorlagen und Parametern. 
1. Öffnen Sie die Datei **template.json** aus dem extrahierten Ordner in einem Editor Ihrer Wahl.
1. Suchen Sie nach `location`, und ersetzen Sie den Wert für die Eigenschaft durch den neuen Namen für die Region. Informationen zum Abrufen von Standortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/). Der Code für eine Region ist der Name der Region ohne Leerzeichen, z. B. ist `West US` gleich `westus`.
1. Entfernen Sie Definitionen von Ressourcen des Typs **Dynamisches WCF Relay** (Typ `Microsoft.Relay/namespaces/WcfRelays`). Dynamische WCF-Relays sind Relays, für die die **isDynamic**-Eigenschaft auf der Seite **Relays** auf **TRUE** festgelegt ist. Im folgenden Beispiel ist **echoservice** ein dynamisches WCF-Relay. Seine Definition sollte aus der Vorlage entfernt werden. 

    :::image type="content" source="./media/move-across-regions/dynamic-relays.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::

## <a name="move"></a>Move
Stellen Sie die Vorlage bereit, um in der Zielregion einen Relaynamespace zu erstellen. 

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Geben Sie in **Marketplace durchsuchen** die Zeichenfolge **Vorlagenbereitstellung** für den Suchtext ein, wählen Sie **Vorlagenbereitstellung (Bereitstellen mit benutzerdefinierten Vorlagen)** aus, und drücken Sie dann die **EINGABETASTE**.

    :::image type="content" source="./media/move-across-regions/new-template-deployment.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::    
1. Wählen Sie auf der Seite **Vorlagenbereitstellung** die Option **Erstellen** aus.

    :::image type="content" source="./media/move-across-regions/template-deployment-create-button.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::        
1. Wählen Sie auf der Seite **Benutzerdefinierte Bereitstellung** die Option **Eigene Vorlage im Editor erstellen** aus.

    :::image type="content" source="./media/move-across-regions/build-template-link.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::            
1. Wählen Sie auf der Seite **Vorlage bearbeiten** in der Symbolleiste **Datei laden**, und folgen Sie dann den Anweisungen, um die Datei **vorlage.json** zu laden, die Sie im letzten Abschnitt heruntergeladen haben.

    :::image type="content" source="./media/move-across-regions/select-template.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::                
1. Wählen Sie **Speichern** aus, um die Vorlage zu speichern. 

    :::image type="content" source="./media/move-across-regions/save-template.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::                    
1. Gehen Sie auf der Seite **Benutzerdefinierte Bereitstellung** wie folgt vor: 
    1. Wählen Sie ein Azure-**Abonnement** aus. 
    2. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine. 
    3. Wählen Sie den **Zielspeicherort** oder die Zielregion aus. Wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben, ist diese Einstellung schreibgeschützt. 
    4. Geben Sie einen neuen **Namen für den Namespace** ein.
    1. Klicken Sie auf **Überprüfen + erstellen**. 

        :::image type="content" source="./media/move-across-regions/deploy-template.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::
    1. Wählen Sie unten auf der Seite **Überprüfen + erstellen** die Option **Erstellen** aus. 
    
## <a name="verify"></a>Überprüfung
1. Nachdem die Bereitstellung erfolgreich war, wählen Sie **Zu Ressourcengruppe wechseln** aus.

    :::image type="content" source="./media/move-across-regions/resource-group-navigation-link.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::    
1. Wählen Sie auf der Seite **Ressourcengruppe** den Azure Relay-Namespace aus. 

    :::image type="content" source="./media/move-across-regions/select-namespace.png" alt-text="Herunterladen einer Resource Manager-Vorlage":::    
1. Wählen Sie auf der Seite **Azure Relay-Namespace** im linken Menü **Hybridverbindungen** oder **WCF Relays** aus, um zu bestätigen, dass Hybridverbindungen und WCF-Relays erstellt werden. Wenn Sie vergessen haben, Definitionen für dynamische WCF-Relays vor dem Importieren der Vorlage zu löschen, löschen Sie diese auf der Seite **WCF-Relays**. Die dynamischen WCF-Relays werden automatisch erstellt, wenn Clients eine Verbindung mit dem Relaynamespace herstellen. 

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen
Wenn Sie nach der Bereitstellung neu beginnen möchten, können Sie den **Azure Relay-Zielnamespace** löschen und die in den Abschnitten [Vorbereiten](#prepare) und [Verschieben](#move) dieses Artikels beschriebenen Schritte wiederholen.

Um die Änderungen zu committen und das Verschieben eines Namespace abzuschließen, löschen Sie den **Azure Relay-Namespace** in der Quellregion. 

So löschen Sie einen Azure Relay-Namespace (Quelle oder Ziel) im Azure-Portal:

1. Geben Sie im Suchfenster oben im Azure-Portal **Relays** ein, und wählen Sie in den Suchergebnissen **Relays** unter **Dienste** aus. Alle Azure Relay-Namespaces werden in einer Liste angezeigt.
2. Wählen Sie den Zielnamespace aus, der gelöscht werden soll, um die Seite **Relay** zu öffnen. 
1. Wählen Sie auf der Seite **Relay** auf der Symbolleiste **Löschen** aus. 

    ![Schaltfläche „Namespace löschen“](./media/move-across-regions/delete-namespace-button.png)
3. Geben Sie auf der Seite **Namespace löschen** den Namen des Azure Relay-Namespace ein, um den Löschvorgang zu bestätigen, und wählen Sie dann **Löschen** aus. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie einen Azure Relay-Namespace aus einer Region in eine andere Region verschoben. Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:

- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md)
