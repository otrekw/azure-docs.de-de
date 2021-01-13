---
title: 'Tutorial: Konfigurieren des Regelmoduls – Azure Front Door'
description: Dieser Artikel bietet ein Tutorial zur Konfiguration des Regelmoduls sowohl im Azure-Portal als auch über die Befehlszeilenschnittstelle.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/09/2020
ms.author: duau
ms.custom: devx-track-azurecli
ms.openlocfilehash: c15b6115c2b07503320cd7b5f3c790be03aeea94
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92278116"
---
# <a name="tutorial-configure-your-rules-engine"></a>Tutorial: Konfigurieren Ihres Regelmoduls

In diesem Tutorial wird erläutert, wie Sie eine Regelmodulkonfiguration und Ihre erste Regel sowohl im Azure-Portal als auch über die Befehlszeilenschnittstelle erstellen. 

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> - Konfigurieren des Regelmoduls über das Portal
> - Konfigurieren des Regelmoduls mithilfe der Azure CLI

## <a name="prerequisites"></a>Voraussetzungen

* Bevor Sie die Schritte in diesem Tutorial ausführen können, müssen Sie zunächst eine Azure Front Door Service-Konfiguration erstellen. Weitere Informationen finden Sie unter [Quickstart: Erstellen einer „Front Door“](quickstart-create-front-door.md).

## <a name="configure-rules-engine-in-azure-portal"></a>Konfigurieren des Regelmoduls im Azure-Portal
1. Wechseln Sie in der Front-Door-Ressource zu **Einstellungen** und wählen Sie die Option zur **Regelmodulkonfiguration** aus. Klicken Sie auf **Hinzufügen**, geben Sie Ihrer Konfiguration einen Namen, und beginnen Sie mit dem Erstellen Ihrer ersten Regelmodulkonfiguration.

    ![Menü für die Front Door-Einstellungen](./media/front-door-rules-engine/rules-engine-tutorial-1.png)

1. Klicken Sie auf **Regel hinzufügen**, um Ihre erste Regel zu erstellen. Dann klicken Sie auf **Bedingung hinzufügen** oder **Aktion hinzufügen**, um Ihre Regel zu definieren.
    
    > [!NOTE]
    >- Zum Löschen einer Bedingung oder Aktion aus einer Regel verwenden Sie den Papierkorb auf der rechten Seite der jeweiligen Bedingung oder Aktion.
    > - Geben Sie keine Bedingungen an, um eine Regel zu erstellen, die für den gesamten eingehenden Datenverkehr gilt.
    > - Um die Auswertung von Regeln zu unterbinden, sobald die erste Übereinstimmungsbedingung erfüllt ist, aktivieren Sie **Auswertung der verbleibenden Regeln beenden**. Wenn diese Option aktiviert ist und alle Übereinstimmungsbedingungen einer bestimmten Regel erfüllt sind, werden die verbleibenden Regeln in der Konfiguration nicht ausgeführt.  

    ![Regelmodulkonfiguration](./media/front-door-rules-engine/rules-engine-tutorial-4.png) 

1. Bestimmen Sie die Priorität der Regeln in der Konfiguration, indem Sie die Schaltflächen zum Verschieben nach oben, nach unten und ganz nach oben verwenden. Die Priorität ist in aufsteigender Reihenfolge, was bedeutet, dass die erste aufgelistete Regel die wichtigste Regel ist.

1. Nachdem Sie eine oder mehrere Regeln erstellt haben, drücken Sie **Speichern**. Mit dieser Aktion wird die Konfiguration des Regelmoduls erstellt.

1. Nachdem Sie mindestens eine Konfiguration erstellt haben, ordnen Sie eine Regelmodulkonfiguration einer Routenregel zu. Eine einzelne Konfiguration kann zwar auf mehrere Routenregeln angewendet werden, aber eine Routenregel darf nur eine Regelmodulkonfiguration enthalten. Um die Zuordnung vorzunehmen, navigieren Sie im **Frontdoor-Designer** zu **Routenregeln**. Wählen Sie die Routenregel aus, der Sie die Regelmodulkonfiguration hinzufügen möchten, navigieren Sie zu **Routendetails** > **Regelmodulkonfiguration**, und wählen Sie die Konfiguration aus, die Sie zuordnen möchten.

    ![Konfigurieren für eine Routingregel](./media/front-door-rules-engine/rules-engine-tutorial-5.png)


## <a name="configure-rules-engine-in-azure-cli"></a>Konfigurieren des Regelmoduls in der Azure CLI

1. Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest), falls Sie dies noch nicht getan haben. Fügen Sie die Front Door-Erweiterung hinzu:- az extension add --name front-door. Melden Sie sich dann an, und wechseln Sie zu Ihrem Abonnement: az account set --subscription <Name_oder_ID>.

1. Beginnen Sie mit dem Erstellen eines Regelmoduls. In diesem Beispiel wird eine Regel mit einer headerbasierten Aktion und einer Übereinstimmungsbedingung gezeigt. 

    ```azurecli-interactive
    az network front-door rules-engine rule create -f {front_door} -g {resource_group} --rules-engine-name {rules_engine} --name {rule1} --priority 1 --action-type RequestHeader --header-action Overwrite --header-name Rewrite --header-value True --match-variable RequestFilenameExtension --operator Contains --match-values jpg png --transforms Lowercase
    ```

1. Listen Sie alle Regeln auf. 

    ```azurecli-interactive
    az network front-door rules-engine rule list -f {front_door} -g {rg} --name {rules_engine}
    ```

1. Fügen Sie eine Aktion zur Außerkraftsetzung der Weiterleitungsroute hinzu. 

    ```azurecli-interactive
    az network front-door rules-engine rule action add -f {front_door} -g {rg} --rules-engine-name {rules_engine} --name {rule1} --action-type ForwardRouteOverride --backend-pool {backend_pool_name} --caching Disabled
    ```

1. Listen Sie alle Aktionen in einer Regel auf. 

    ```azurecli-interactive
    az network front-door rules-engine rule action list -f {front_door} -g {rg} -r {rules_engine} --name {rule1}
    ```

1. Verknüpfen Sie eine Regelmodulkonfiguration mit einer Routingregel.  

    ```azurecli-interactive
    az network front-door routing-rule update -g {rg} -f {front_door} -n {routing_rule_name} --rules-engine {rules_engine}
    ```

1. Heben Sie die Verknüpfung des Regelmoduls auf. 

    ```azurecli-interactive
    az network front-door routing-rule update -g {rg} -f {front_door} -n {routing_rule_name} --remove rulesEngine # case sensitive word ‘rulesEngine’
    ```

Weitere Informationen und die vollständige Liste der Befehle des AFD-Regelmoduls finden Sie [hier](/cli/azure/ext/front-door/network/front-door/rules-engine?preserve-view=true&view=azure-cli-latest).   

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie eine Regelmodulkonfiguration erstellt und den Routingregeln zugeordnet. Wenn die Regelmodulkonfiguration nicht mehr der Front Door-Instanz zugeordnet sein soll, können Sie die Konfiguration entfernen, indem Sie die folgenden Schritte ausführen:

1. Heben Sie die Zuordnung der Routingregeln in der Regelmodulkonfiguration auf, indem Sie neben dem Namen des Regelmoduls auf die drei Punkte klicken.

    :::image type="content" source="./media/front-door-rules-engine/front-door-rule-engine-routing-association.png" alt-text="Zuordnen von Routingregeln":::

1. Deaktivieren Sie alle Routingregeln, denen diese Regelmodulkonfiguration zugeordnet ist, und klicken Sie auf „Speichern“.

    :::image type="content" source="./media/front-door-rules-engine/front-door-routing-rule-association.png" alt-text="Zuordnen von Routingregeln":::

1. Nun können Sie die Regelmodulkonfiguration aus Front Door löschen.

    :::image type="content" source="./media/front-door-rules-engine/front-door-delete-rule-engine-configuration.png" alt-text="Zuordnen von Routingregeln":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen einer Regelmodulkonfiguration
* Ordnen Sie den Front Door-Routingregeln eine Konfiguration zu.

Im nächsten Tutorial erfahren Sie, wie Sie Sicherheitsheader mit dem Regelmodul hinzufügen:

> [!div class="nextstepaction"]
> [Hinzufügen von Sicherheitsheadern mit Regel-Engines](front-door-security-headers.md)