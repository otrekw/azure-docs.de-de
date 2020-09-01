---
title: Erstellen von Arbeitsbereichen im Portal
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Azure Machine Learning-Arbeitsbereiche im Azure-Portal erstellen, anzeigen und löschen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sgilley
author: sdgilley
ms.date: 12/27/2019
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: e1f97fddb07e56946e37c04d9b9685412782c560
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88659754"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Erstellen und Verwalten von Azure Machine Learning-Arbeitsbereichen im Azure-Portal
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel wird erläutert, wie Sie [**Azure Machine Learning-Arbeitsbereiche**](concept-workspace.md) im Azure-Portal für [Azure Machine Learning](overview-what-is-azure-ml.md) erstellen, anzeigen und löschen.  Das Portal eignet sich am besten für die ersten Schritte mit Arbeitsbereichen. Wenn sich Ihre Anforderungen ändern oder eine umfassendere Automatisierung erforderlich wird, können Sie Arbeitsbereiche aber auch mit der [CLI](reference-azure-machine-learning-cli.md), mit [Python-Code](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) oder über die [VS Code-Erweiterung](tutorial-setup-vscode-extension.md) erstellen und löschen.

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
   Standort | Wählen Sie den Standort aus, der Ihren Benutzern und den Datenressourcen am nächsten ist, um Ihren Arbeitsbereich zu erstellen.
   Standort | Wählen Sie den Standort aus, der Ihren Benutzern und den Datenressourcen am nächsten ist, um Ihren Arbeitsbereich zu erstellen.
   Arbeitsbereichs-Edition | Wählen Sie **Basic** oder **Enterprise** aus.  Diese Arbeitsbereichs-Edition bestimmt die Features, auf die Sie Zugriff haben, und die Preise. Erfahren Sie mehr über die [Angebote zur Basic und zur Enterprise Edition](overview-what-is-azure-ml.md#sku). 

    ![Konfigurieren des Arbeitsbereichs](./media/how-to-manage-workspace/select-edition.png)

1. Wählen Sie **Überprüfen + erstellen** aus, nachdem die Konfiguration des Arbeitsbereichs abgeschlossen ist.
2. Überprüfen Sie die Einstellungen, und nehmen Sie zusätzliche Änderungen oder Korrekturen vor. Wenn Sie mit den Einstellungen zufrieden sind, wählen Sie **Erstellen** aus.

   > [!Warning] 
   > Die Erstellung des Arbeitsbereichs in der Cloud kann einige Minuten dauern.

   Wenn der Vorgang abgeschlossen ist, wird eine Erfolgsmeldung zur Bereitstellung angezeigt. 
 
 1. Um den neuen Arbeitsbereich anzuzeigen, wählen Sie **Zu Ressource wechseln** aus.

### <a name="download-a-configuration-file"></a>Herunterladen einer Konfigurationsdatei

1. Wenn Sie eine [Compute-Instanz](tutorial-1st-experiment-sdk-setup.md#azure) erstellen, überspringen Sie diesen Schritt.

1. Wenn Sie Code in Ihrer lokalen Umgebung verwenden möchten, der auf diesen Arbeitsbereich verweist, wählen Sie im Abschnitt **Übersicht** des Arbeitsbereichs die Option **„config.json“ herunterladen** aus.  

   ![Herunterladen von „config.json“](./media/how-to-manage-workspace/configure.png)
   
   Legen Sie die Datei in der Verzeichnisstruktur mit Ihren Python-Skripts oder Jupyter Notebooks ab. Sie kann sich im selben Verzeichnis, in einem Unterverzeichnis namens *.azureml* oder in einem übergeordneten Verzeichnis befinden. Bei der Erstellung einer Compute-Instanz wird diese Datei automatisch dem richtigen Verzeichnis auf dem virtuellen Computer hinzugefügt.

## <a name="upgrade-to-enterprise-edition"></a><a name="upgrade"></a>Upgrade auf Enterprise Edition

Sie können Ihren Arbeitsbereich von der Basic Edition auf die Enterprise Edition upgraden, um die erweiterten Features zu nutzen, z. B. weitgehend codefreie Umgebungen und verbesserte Sicherheitsfeatures.

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com) an.

1. Wählen Sie den Arbeitsbereich aus, den Sie upgraden möchten.

1. Wählen Sie oben rechts auf der Seite **Weitere Informationen** aus.

   [ ![Upgraden eines Arbeitsbereichs](./media/how-to-manage-workspace/upgrade.png) ](./media/how-to-manage-workspace/upgrade.png#lightbox)

1. Wählen Sie im angezeigten Fenster **Upgrade** aus.


> [!IMPORTANT]
> Es ist nicht möglich, einen Enterprise Edition-Arbeitsbereich in einen Basic Edition-Arbeitsbereich herabzustufen. 

## <a name="find-a-workspace"></a><a name="view"></a>Suchen nach einem Arbeitsbereich

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Geben Sie im oberen Suchfeld **Machine Learning** ein.  

1. Wählen Sie **Machine Learning** aus.

   ![Suchen nach Azure Machine Learning-Arbeitsbereich](./media/how-to-manage-workspace/find-workspaces.png)

1. Durchsuchen Sie die Liste der gefundenen Arbeitsbereiche. Sie können basierend auf Abonnement, Ressourcengruppen und Standorten filtern.  

1. Wählen Sie einen Arbeitsbereich aus, um seine Eigenschaften anzuzeigen.

## <a name="delete-a-workspace"></a>Löschen eines Arbeitsbereichs

Wählen Sie im [Azure-Portal](https://portal.azure.com/) am oberen Rand des zu löschenden Arbeitsbereichs die Option **Löschen** aus.

:::image type="content" source="./media/how-to-manage-workspace/delete-workspace.png" alt-text="Löschen des Arbeitsbereichs":::

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
