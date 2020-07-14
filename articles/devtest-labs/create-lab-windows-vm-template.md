---
title: Erstellen eines Labs mit Azure DevTest Labs und einer Resource Manager-Vorlage
description: In diesem Tutorial erstellen Sie in Azure DevTest Labs ein Lab, indem Sie eine Azure Resource Manager-Vorlage verwenden. Ein Lab-Administrator richtet ein Lab ein, erstellt virtuelle Computer im Lab und konfiguriert Richtlinien.
ms.topic: tutorial
ms.date: 06/26/2020
ms.openlocfilehash: 22ba4ffeec22a45c07b096b0a754d08c8230dd8f
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2020
ms.locfileid: "85476205"
---
# <a name="tutorial-set-up-a-lab-by-using-azure-devtest-labs-resource-manager-template"></a>Tutorial: Einrichten eines Labs mit Azure DevTest Labs (Resource Manager-Vorlage)
In diesem Tutorial erstellen Sie ein Lab mit einer Windows Server 2019 Datacenter-VM, indem Sie eine Azure Resource Manager-Vorlage verwenden. 

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Überprüfen der Vorlage 
> * Bereitstellen der Vorlage
> * Überprüfen der Vorlage
> * Bereinigen von Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Keine.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-dtl-create-lab-windows-vm/).

:::code language="json" source="~/quickstart-templates/101-dtl-create-lab-windows-vm/azuredeploy.json" range="1-97" highlight="51-85":::

In der Vorlage sind die folgenden Ressourcen definiert:

- [**Microsoft.DevTestLab/labs**](/azure/templates/microsoft.devtestlab/labs)
- [**Microsoft.DevTestLab labs/virtualnetworks**](/azure/templates/microsoft.devtestlab/labs/virtualnetworks)
- [**Microsoft.DevTestLab labs/virtualmachines**](/azure/templates/microsoft.devtestlab/labs/virtualmachines)

Weitere Beispiele für Vorlagen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Devtestlab).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage
Klicken Sie auf die folgende Schaltfläche, um die Bereitstellung automatisch auszuführen. 

[![In Azure bereitstellen](./media/create-lab-windows-vm-template/deploy-button.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-dtl-create-lab-windows-vm%2Fazuredeploy.json)

1. Erstellen Sie eine **neue Ressourcengruppe**, um die spätere Bereinigung zu erleichtern.
1. Wählen Sie einen **Speicherort** für die Ressourcengruppe aus. 
1. Geben Sie einen **Namen** für das Lab ein. 
1. Geben Sie einen **Namen** für den virtuellen Computer ein. 
1. Geben Sie einen **Benutzernamen** eines Benutzers ein, der auf den virtuellen Computer zugreifen kann. 
1. Geben Sie das **Kennwort** für den Benutzer ein. 
1. Aktivieren Sie **Ich stimme den oben genannten Geschäftsbedingungen zu**. 
1. Wählen Sie anschließend die Option **Kaufen** aus.

    :::image type="content" source="./media/create-lab-windows-vm-template/deploy-template-page.png" alt-text="Seite „Vorlage bereitstellen“":::

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung
1. Wählen Sie oben die Option **Benachrichtigungen** aus, um den Status der Bereitstellung anzuzeigen, und klicken Sie auf den Link **Bereitstellung wird ausgeführt**.

    :::image type="content" source="./media/create-lab-windows-vm-template/deployment-notification.png" alt-text="Bereitstellungsbenachrichtigung":::
2. Warten Sie auf der Seite **Bereitstellung: Übersicht**, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang (vor allem das Erstellen eines virtuellen Computers) nimmt einige Zeit in Anspruch. Wählen Sie anschließend die Option **Zu Ressourcengruppe wechseln** oder den **Namen der Ressourcengruppe** aus. Dies ist in der folgenden Abbildung dargestellt: 

    :::image type="content" source="./media/create-lab-windows-vm-template/navigate-resource-group.png" alt-text="Navigieren zur Ressourcengruppe":::
3. Auf der Seite **Ressourcengruppe** wird die Liste mit den Ressourcen der Ressourcengruppe angezeigt. Vergewissern Sie sich, dass Ihr Lab vom Typ `DevTest Lab` in den Ressourcen angezeigt wird. Außerdem werden die abhängigen Ressourcen angezeigt, z. B. das virtuelle Netzwerk und der virtuelle Computer in der Ressourcengruppe. 

    :::image type="content" source="./media/create-lab-windows-vm-template/resource-group-home-page.png" alt-text="Startseite der Ressourcengruppe":::
4. Wählen Sie Ihr Lab in der Liste mit den Ressourcen aus, um die zugehörige Startseite anzuzeigen. Vergewissern Sie sich, dass die Windows Server 2019 Datacenter-VM in der Liste **Meine virtuellen Computer** angezeigt wird. In der folgenden Abbildung ist der Abschnitt **Grundlagen** minimiert. 

    :::image type="content" source="./media/create-lab-windows-vm-template/lab-home-page.png" alt-text="Startseite für das Lab":::

    > [!IMPORTANT] 
    > Lassen Sie diese Seite geöffnet, und befolgen Sie die Anleitung zum Bereinigen von Ressourcen im nächsten Abschnitt, um Kosten für die Ausführung des Labs und der VM in Azure zu vermeiden. Falls Sie das nächste Tutorial zum Testen des Zugriffs auf die VM im Lab durcharbeiten möchten, sollten Sie die Ressourcen anschließend bereinigen. 

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen

1. Löschen Sie zuerst das Lab, damit Sie die Ressourcengruppe löschen können. Das Löschen der Ressourcengruppe ist nicht möglich, wenn sie ein Lab enthält. Wählen Sie zum Löschen des Labs in der Symbolleiste die Option **Löschen** aus. 

    :::image type="content" source="./media/create-lab-windows-vm-template/delete-lab-button.png" alt-text="Schaltfläche zum Löschen des Labs":::
 2. Geben Sie auf der Bestätigungsseite den **Lab-Namen** ein, und wählen Sie **Löschen** aus. 
 3. Warten Sie, bis das Lab gelöscht wurde. Wählen Sie das **Glockensymbol** aus, um die Benachrichtigung für den Löschvorgang anzuzeigen. Dieser Prozess nimmt einige Zeit in Anspruch. Vergewissern Sie sich, dass das Lab gelöscht wurde, und wählen Sie anschließend im Breadcrumb-Menü die **Ressourcengruppe** aus. 
 
    :::image type="content" source="./media/create-lab-windows-vm-template/confirm-lab-deletion.png" alt-text="Bestätigung des Löschvorgangs der VM in den Benachrichtigungen":::
 1. Wählen Sie auf der Seite **Ressourcengruppe** in der Symbolleiste die Option **Ressourcengruppe löschen** aus. Geben Sie auf der Bestätigungsseite den **Ressourcengruppennamen** ein, und wählen Sie **Löschen** aus. Überprüfen Sie die Benachrichtigungen, um zu bestätigen, dass die Ressourcengruppe gelöscht wurde.
 
    :::image type="content" source="./media/create-lab-windows-vm-template/delete-resource-group-button.png" alt-text="Schaltfläche „Ressourcengruppe löschen“":::

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Lab mit einer VM erstellt. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie auf das Lab zugreifen:

> [!div class="nextstepaction"]
> [Tutorial: Zugreifen auf das Lab](tutorial-use-custom-lab.md)

