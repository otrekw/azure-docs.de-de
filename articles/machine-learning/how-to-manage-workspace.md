---
title: Erstellen von Arbeitsbereichen im Portal
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Azure Machine Learning-Arbeitsbereiche im Azure-Portal erstellen, anzeigen und löschen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sgilley
author: sdgilley
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: how-to, fasttrack-edit
ms.openlocfilehash: d0b5d3678c3d9c7e55eede13c630510df89d5128
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92045566"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Erstellen und Verwalten von Azure Machine Learning-Arbeitsbereichen im Azure-Portal


In diesem Artikel wird erläutert, wie Sie [**Azure Machine Learning-Arbeitsbereiche**](concept-workspace.md) im Azure-Portal für [Azure Machine Learning](overview-what-is-azure-ml.md) erstellen, anzeigen und löschen.  Das Portal eignet sich am besten für die ersten Schritte mit Arbeitsbereichen. Wenn sich Ihre Anforderungen ändern oder eine umfassendere Automatisierung erforderlich wird, können Sie Arbeitsbereiche aber auch mit der [CLI](reference-azure-machine-learning-cli.md), mit [Python-Code](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) oder über die [VS Code-Erweiterung](tutorial-setup-vscode-extension.md) erstellen und löschen.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Zum Erstellen eines Arbeitsbereichs benötigen Sie ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Abonnement beim [Azure-Portal](https://portal.azure.com/) an. 

1. Wählen Sie im Azure-Portal oben links die Option **+ Ressource erstellen** aus.

      ![Neue Ressource erstellen](./media/how-to-manage-workspace/create-workspace.gif)

1. Suchen Sie mithilfe der Suchleiste **Machine Learning**.

1. Wählen Sie **Machine Learning** aus.

1. Wählen Sie im Bereich **Machine Learning** die Option **Erstellen** aus, um zu beginnen.

1. Geben Sie die folgenden Informationen an, um den neuen Arbeitsbereich zu konfigurieren:

   Feld|BESCHREIBUNG 
   ---|---
   Arbeitsbereichname |Geben Sie einen eindeutigen Namen ein, der Ihren Arbeitsbereich identifiziert. In diesem Beispiel verwenden wir **docs-ws**. Namen müssen in der Ressourcengruppe eindeutig sein. Verwenden Sie einen Namen, der leicht zu merken ist und sich von den von anderen Benutzern erstellten Arbeitsbereichen unterscheidet. Für den Namen des Arbeitsbereichs wird die Groß-/Kleinschreibung nicht beachtet.
   Subscription |Wählen Sie das gewünschte Azure-Abonnement aus.
   Resource group | Verwenden Sie eine vorhandene Ressourcengruppe in Ihrem Abonnement, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen. Eine Ressourcengruppe enthält verwandte Ressourcen für eine Azure-Lösung. In diesem Beispiel verwenden wir **docs-aml**. Die Rolle *Mitwirkender* oder *Besitzer* ist für die Verwendung einer vorhandenen Ressourcengruppe erforderlich.  Weitere Informationen zum Zugriff finden Sie unter [Verwalten des Zugriffs auf einen Azure Machine Learning-Arbeitsbereich](how-to-assign-roles.md).
   Region | Wählen Sie die Azure-Region aus, die Ihren Benutzern und den Datenressourcen am nächsten ist, um Ihren Arbeitsbereich zu erstellen.

1. Wählen Sie **Überprüfen + erstellen** aus, nachdem die Konfiguration des Arbeitsbereichs abgeschlossen ist. Verwenden Sie optional die Abschnitte [Netzwerk](#networking) und [Erweitert](#advanced), um weitere Einstellungen für den Arbeitsbereich zu konfigurieren.

1. Überprüfen Sie die Einstellungen, und nehmen Sie zusätzliche Änderungen oder Korrekturen vor. Wenn Sie mit den Einstellungen zufrieden sind, wählen Sie **Erstellen** aus.

   > [!Warning] 
   > Die Erstellung des Arbeitsbereichs in der Cloud kann einige Minuten dauern.

   Wenn der Vorgang abgeschlossen ist, wird eine Erfolgsmeldung zur Bereitstellung angezeigt. 
 
 1. Um den neuen Arbeitsbereich anzuzeigen, wählen Sie **Zu Ressource wechseln** aus.

### <a name="networking"></a>Netzwerk  

> [!IMPORTANT]  
> Weitere Informationen zur Verwendung eines privaten Endpunkts und eines virtuellen Netzwerks mit Ihrem Arbeitsbereich finden Sie unter [Netzwerkisolation und Datenschutz](how-to-enable-virtual-network.md).
    
1. Bei der standardmäßigen Netzwerkkonfiguration wird ein __öffentlicher Endpunkt__ verwendet, der über das öffentliche Internet zugänglich ist. Um den Zugriff auf Ihren Arbeitsbereich auf ein von Ihnen erstelltes Azure Virtual Network zu beschränken, können Sie __Privater Endpunkt__ als __Konnektivitätsmethode__ auswählen und dann auf __+ Hinzufügen__ klicken, um den Endpunkt zu konfigurieren. 
    
   :::image type="content" source="media/how-to-manage-workspace/select-private-endpoint.png" alt-text="Auswahl des privaten Endpunkts":::  

1. Legen Sie im Formular __Privaten Endpunkt erstellen__ den Speicherort, den Namen und das zu verwendende virtuelle Netzwerk fest. Wenn Sie den Endpunkt mit einer privaten DNS-Zone verwenden möchten, klicken Sie auf __In private DNS-Zone integrieren__, und wählen Sie im Feld __Private DNS-Zone__ die gewünschte Zone aus. Klicken Sie auf __OK__, um den Endpunkt zu erstellen.   

   :::image type="content" source="media/how-to-manage-workspace/create-private-endpoint.png" alt-text="Auswahl des privaten Endpunkts":::   

1. Wenn Sie die Netzwerkkonfiguration abgeschlossen haben, können Sie auf __Überprüfen und erstellen__ klicken oder mit der optionalen Konfiguration unter __Erweitert__ fortfahren. 

    > [!WARNING]    
    > Wenn Sie einen privaten Endpunkt erstellen, wird eine neue private DNS-Zone namens __privatelink.api.azureml.ms__ erstellt. Diese enthält einen Link zum virtuellen Netzwerk. Wenn Sie mehrere Arbeitsbereiche mit privaten Endpunkten in derselben Ressourcengruppe erstellen, kann nur das virtuelle Netzwerk für den ersten privaten Endpunkt zur DNS-Zone hinzugefügt werden. Um den virtuellen Netzwerken, die von den zusätzlichen Arbeitsbereichen bzw. privaten Endpunkten verwendet werden, Einträge hinzuzufügen, führen Sie folgende Schritte aus: 
    >   
    > 1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Ressourcengruppe aus, die den Arbeitsbereich enthält. Wählen Sie dann die Ressource namens __privatelink.api.azureml.ms__ für die private DNS-Zone aus.    
    > 2. Klicken Sie unter __Einstellungen__ auf __VNET-Verknüpfungen__. 
    > 3. Wählen Sie __Hinzufügen__. Geben Sie auf der Seite __VNET-Verknüpfung hinzufügen__ einen eindeutigen __Linknamen__ an, und wählen Sie dann das __virtuelle Netzwerk__ aus, das hinzugefügt werden soll. Klicken Sie auf __OK__, um die Netzwerkverknüpfung hinzuzufügen.    
    >   
    > Weitere Informationen finden Sie unter [DNS-Konfiguration für private Azure-Endpunkte](/azure/private-link/private-endpoint-dns).   

### <a name="vulnerability-scanning"></a>Überprüfung auf Sicherheitsrisiken

Azure Security Center bietet einheitliche Funktionen für die Sicherheitsverwaltung und den erweiterten Schutz vor Bedrohungen für Hybrid Cloud-Workloads. Sie sollten zulassen, dass Azure Security Center Ihre Ressourcen überprüft und die Empfehlungen befolgt. Weitere Informationen finden Sie unter [Azure Container Registry-Imagescans durch Security Center](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration) und [Azure Kubernetes Service-Integration in Security Center](https://docs.microsoft.com/azure/security-center/azure-kubernetes-service-integration).

### <a name="advanced"></a>Erweitert    

Standardmäßig werden Metriken und Metadaten für den Arbeitsbereich in einer Azure Cosmos DB-Instanz gespeichert, die von Microsoft verwaltet wird. Diese Daten werden mit von Microsoft verwalteten Schlüsseln verschlüsselt.  

Wählen Sie __Arbeitsbereich mit hohen geschäftlichen Auswirkungen__ aus, um die von Microsoft in Ihrem Arbeitsbereich gesammelten Daten zu beschränken. Weitere Informationen zu dieser Einstellung finden Sie unter [Verschlüsselung ruhender Daten](concept-enterprise-security.md#encryption-at-rest).

> [!IMPORTANT]  
> Die Auswahl von „starken geschäftlichen Auswirkungen“ kann nur beim Erstellen eines Arbeitsbereichs erfolgen. Diese Einstellung kann nach dem Erstellen des Arbeitsbereichs nicht mehr geändert werden.   
Wenn Sie eine __Enterprise__-Version von Azure Machine Learning erstellen, können Sie stattdessen einen eigenen Schlüssel angeben. Hierdurch wird die Azure Cosmos DB-Instanz erstellt, die Metriken und Metadaten in Ihrem Azure-Abonnement speichert. Führen Sie die folgenden Schritte aus, um Ihren eigenen Schlüssel zu verwenden:    

> [!IMPORTANT]  
> Vor diesen Schritten müssen Sie zunächst die folgenden Aktionen ausführen:   
>   
> 1. Autorisieren Sie die __Machine Learning-App__ (in der Identitäts- und Zugriffsverwaltung) mit den Berechtigungen für Mitwirkende in Ihrem Abonnement.  
> 1. Befolgen Sie die Schritte in [Konfigurieren von kundenseitig verwalteten Schlüsseln](/azure/cosmos-db/how-to-setup-cmk), um:   
>     * Registrieren des Azure Cosmos DB-Anbieters   
>     * Erstellen und Konfigurieren einer Azure Key Vault-Instanz 
>     * Generieren eines Schlüssels  
>   
>     Sie müssen die Azure Cosmos DB-Instanz nicht manuell erstellen. Es wird eine während der Erstellung des Arbeitsbereichs für Sie erstellt. Diese Azure Cosmos DB-Instanz wird in einer separaten Ressourcengruppe mit einem Namen erstellt, der auf diesem Muster basiert: `<your-workspace-resource-name>_<GUID>`.   
>   
> Diese Einstellung kann nach dem Erstellen des Arbeitsbereichs nicht mehr geändert werden. Wenn Sie die von Ihrem Arbeitsbereich verwendete Azure Cosmos DB löschen, müssen Sie auch den Arbeitsbereich löschen, der sie verwendet.

1. Klicken Sie auf __Kundenseitig verwalteter Schlüssel__ und dann auf __Klicken Sie hier, um einen Schlüssel auszuwählen__.   

    :::image type="content" source="media/how-to-manage-workspace/advanced-workspace.png" alt-text="Auswahl des privaten Endpunkts":::   

1. Wählen Sie im Formular __Schlüssel aus Azure Key Vault auswählen__ eine vorhandene Azure Key Vault-instanz, einen darin enthaltenen Schlüssel sowie die Schlüsselversion aus. Dieser Schlüssel wird zum Verschlüsseln der in Azure Cosmos DB gespeicherten Daten verwendet. Klicken Sie abschließend auf die Schaltfläche __Auswählen__, um diesen Schlüssel zu verwenden. 

   :::image type="content" source="media/how-to-manage-workspace/select-key-vault.png" alt-text="Auswahl des privaten Endpunkts":::

### <a name="download-a-configuration-file"></a>Herunterladen einer Konfigurationsdatei

1. Wenn Sie eine [Compute-Instanz](tutorial-1st-experiment-sdk-setup.md#azure) erstellen, überspringen Sie diesen Schritt.

1. Wenn Sie Code in Ihrer lokalen Umgebung verwenden möchten, der auf diesen Arbeitsbereich verweist, wählen Sie im Abschnitt **Übersicht** des Arbeitsbereichs die Option **„config.json“ herunterladen** aus.  

   ![Herunterladen von „config.json“](./media/how-to-manage-workspace/configure.png)
   
   Legen Sie die Datei in der Verzeichnisstruktur mit Ihren Python-Skripts oder Jupyter Notebooks ab. Sie kann sich im selben Verzeichnis, in einem Unterverzeichnis namens *.azureml* oder in einem übergeordneten Verzeichnis befinden. Bei der Erstellung einer Compute-Instanz wird diese Datei automatisch dem richtigen Verzeichnis auf dem virtuellen Computer hinzugefügt.
## <a name="find-a-workspace"></a><a name="view"></a>Suchen nach einem Arbeitsbereich

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Geben Sie im oberen Suchfeld **Machine Learning** ein.  

1. Wählen Sie **Machine Learning** aus.

   ![Suchen nach Azure Machine Learning-Arbeitsbereich](./media/how-to-manage-workspace/find-workspaces.png)

1. Durchsuchen Sie die Liste der gefundenen Arbeitsbereiche. Sie können basierend auf Abonnement, Ressourcengruppen und Standorten filtern.  

1. Wählen Sie einen Arbeitsbereich aus, um seine Eigenschaften anzuzeigen.

## <a name="delete-a-workspace"></a>Löschen eines Arbeitsbereichs

Wählen Sie im [Azure-Portal](https://portal.azure.com/) am oberen Rand des zu löschenden Arbeitsbereichs die Option **Löschen** aus.

:::image type="content" source="./media/how-to-manage-workspace/delete-workspace.png" alt-text="Auswahl des privaten Endpunkts":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="troubleshooting"></a>Problembehandlung

### <a name="resource-provider-errors"></a>Fehler der Ressourcenanbieter

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="moving-the-workspace"></a>Verschieben des Arbeitsbereichs

> [!WARNING]
> Das Verschieben des Azure Machine Learning-Arbeitsbereichs in ein anderes Abonnement oder das Verschieben des besitzenden Abonnements in einen neuen Mandanten wird nicht unterstützt. Andernfalls können Fehler auftreten.

### <a name="deleting-the-azure-container-registry"></a>Löschen der Azure Container Registry

Der Azure Machine Learning-Arbeitsbereich verwendet für einige Operationen die Azure Container Registry (ACR). Es wird automatisch eine ACR-Instanz erstellt, wenn erstmals eine erforderlich ist.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

## <a name="next-steps"></a>Nächste Schritte

Schließen Sie das gesamte Tutorial ab, um zu erfahren, wie Sie einen Arbeitsbereich zum Erstellen, Trainieren und Bereitstellen von Modellen mit Azure Machine Learning verwenden.

> [!div class="nextstepaction"]
> [Tutorial: Trainieren von Modellen](tutorial-train-models-with-aml.md)
