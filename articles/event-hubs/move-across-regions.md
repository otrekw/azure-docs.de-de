---
title: Verschieben eines Azure Event Hubs-Namespace in eine andere Region | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einen Azure Event Hubs-Namespace aus der aktuellen Region in eine andere verschieben.
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: a70397772d22a65046f87877deab6263d4b2104f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85312963"
---
# <a name="move-an-azure-event-hubs-namespace-to-another-region"></a>Verschieben eines Azure Event Hubs-Namespace in eine andere Region
Es gibt verschiedene Szenarien, in denen Sie Ihren vorhandenen Azure Event Hubs-Namespace aus einer Region in eine andere verschieben möchten. Beispielsweise könnte es sein, dass Sie für Testzwecke einen Namespace mit derselben Konfiguration erstellen möchten. Möglicherweise möchten Sie im Rahmen der [Planung der Notfallwiederherstellung](event-hubs-geo-dr.md#setup-and-failover-flow) auch einen sekundären Namespace in einer anderen Region einrichten.

> [!NOTE]
> In diesem Artikel erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage für einen vorhandenen Event Hubs-Namespace exportieren und anschließend mithilfe der Vorlage einen Namespace mit denselben Konfigurationseinstellungen in einer anderen Region erstellen können. Bei diesem Vorgang werden jedoch keine Ereignisse verschoben, die noch nicht verarbeitet wurden. Sie müssen die Ereignisse im ursprünglichen Namespace verarbeiten, bevor Sie ihn löschen.

## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass die Dienste und Features, die von Ihrem Konto verwendet werden, in der Zielregion unterstützt werden.
- Stellen Sie für Previewfunktionen sicher, dass Ihr Abonnement für die Zielregion auf der Whitelist steht.
- Wenn Sie im Namespace das **Feature für die Erfassung** für Event Hubs aktiviert haben, verschieben Sie die Konten [Azure Storage oder Azure Data Lake Store Gen 2](../storage/common/storage-account-move.md) oder [Azure Data Lake Store Gen 1](../data-lake-store/data-lake-store-migration-cross-region.md), bevor Sie den Event Hubs-Namespace verschieben. Sie können außerdem die Ressourcengruppe, die sowohl Azure Storage- als auch Event Hubs-Namespaces enthält, in die andere Region verschieben, indem Sie die in diesem Artikel beschriebenen Schritte ausführen. 
- Wenn sich der Event Hubs-Namespace in einem **Event Hubs-Cluster** befindet, erstellen Sie in der **Zielregion** einen [dedizierten Cluster](event-hubs-dedicated-cluster-create-portal.md), bevor Sie die Schritte in diesem Artikel ausführen. Sie können auch die [Schnellstartvorlage auf GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-cluster-namespace-eventhub/) verwenden, um einen Event Hubs-Cluster zu erstellen. Entfernen Sie in der Vorlage den Namespaceteil des JSON-Codes, um nur den Cluster zu erstellen. 

## <a name="prepare"></a>Vorbereiten
Exportieren Sie zunächst eine Resource Manager-Vorlage. Diese Vorlage enthält Einstellungen, die Ihren Event Hubs-Namespace beschreiben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Alle Ressourcen** und dann Ihren Event Hubs-Namespace aus.

3. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.

4. Wählen Sie **Herunterladen** auf der Seite **Vorlage exportieren** aus.

    ![Herunterladen einer Resource Manager-Vorlage](./media/move-across-regions/download-template.png)

5. Suchen Sie die aus dem Portal heruntergeladene ZIP-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl.

   Diese ZIP-Datei enthält die JSON-Dateien, in denen die Vorlage und Skripts zum Bereitstellen der Vorlage enthalten sind.


## <a name="move"></a>Move

Stellen Sie die Vorlage bereit, um in der Zielregion einen Event Hubs-Namespace zu erstellen. 


1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.

2. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.

3. Wählen Sie **Vorlagenbereitstellung** aus.

4. Klicken Sie auf **Erstellen**.

5. Wählen Sie **Eigene Vorlage im Editor erstellen**.

6. Wählen Sie **Datei laden** aus, und befolgen Sie dann die Anweisungen zum Laden der Datei **template.json**, die Sie im vorherigen Abschnitt heruntergeladen haben.

7. Wählen Sie **Speichern** aus, um die Vorlage zu speichern. 

8. Gehen Sie auf der Seite **Benutzerdefinierte Bereitstellung** wie folgt vor: 

    1. Wählen Sie ein Azure-**Abonnement** aus. 

    2. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine. Wenn sich der Quellnamespace in einem Event Hubs-Cluster befindet, wählen Sie die Ressourcengruppe aus, die den Cluster in der Zielregion enthält. 

    3. Wählen Sie den **Zielspeicherort** oder die Zielregion aus. Wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben, ist diese Einstellung schreibgeschützt. 

    4. Führen Sie im Abschnitt **EINSTELLUNGEN** die folgenden Schritte aus:
    
        1. Geben Sie den neuen **Namen des Namespace** ein. 

            ![Bereitstellen der Resource Manager-Vorlage](./media/move-across-regions/deploy-template.png)

        2. Wenn sich der Quellnamespace in einem **Event Hubs-Cluster** befand, geben Sie die Namen der **Ressourcengruppe** und des **Event Hubs-Clusters** als Teil der **externen ID** ein. 

              ```
              /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<CLUSTER'S RESOURCE GROUP>/providers/Microsoft.EventHub/clusters/<CLUSTER NAME>
              ```   
        3. Wenn der Event Hub in Ihrem Namespace ein Azure Storage-Konto für das Erfassen von Ereignissen verwendet, geben Sie den Namen der Ressourcengruppe und des Speicherkontos im Feld `StorageAccounts_<original storage account name>_external` ein. 
            
            ```
            /subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/<STORAGE'S RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>
            ```    
    5. Aktivieren Sie das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**. 
    
    6. Wählen Sie nun **Kaufen** aus, um den Bereitstellungsprozess zu starten. 

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen
Wenn Sie nach der Bereitstellung von vorne beginnen möchten, können Sie den **Event Hubs-Zielnamespace** löschen und die in den Abschnitten [Vorbereiten](#prepare) und [Verschieben](#move) dieses Artikels beschriebenen Schritte wiederholen.

Um die Änderungen zu committen und das Verschieben eines Event Hubs-Namespace abzuschließen, löschen Sie den **Event Hubs-Quellnamespace**. Vergewissern Sie sich, dass Sie alle Ereignisse im Namespace verarbeitet haben, bevor Sie den Namespace löschen. 

So löschen Sie einen Event Hubs-Quell- oder -Zielnamespace im Azure-Portal

1. Geben Sie im Suchfenster oben im Azure-Portal **Event Hubs** ein, und wählen Sie in den Suchergebnissen **Event Hubs** aus. Die Event Hubs-Namespaces werden in einer Liste angezeigt.

2. Wählen Sie den zu löschenden Zielnamespace und dann auf der Symbolleiste **Löschen** aus. 

    ![Schaltfläche „Namespace löschen“](./media/move-across-regions/delete-namespace-button.png)

3. Überprüfen Sie auf der Seite **Ressourcen löschen*** die ausgewählten Ressourcen, und bestätigen Sie den Löschvorgang durch Eingabe von **Ja**. Wählen Sie dann **Löschen** aus. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Event Hubs-Namespace aus einer Region in eine andere verschoben und die Quellressourcen bereinigt.  Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:


- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Verschieben virtueller Azure-Computer in eine andere Region](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
