---
title: 'Tutorial: Erstellen und Bereitstellen von Azure Resource Manager-Bicep-Dateien'
description: Erstellen Sie Ihre erste Bicep-Datei zum Bereitstellen von Azure-Ressourcen. In diesem Tutorial lernen Sie die Syntax der Bicep-Datei kennen und erfahren, wie Sie ein Speicherkonto bereitstellen.
author: mumian
ms.date: 03/03/2021
ms.topic: tutorial
ms.author: jgao
ms.custom: ''
ms.openlocfilehash: 6a335b554fa0cfc2e12c8ddbe3e24a50fdedec0f
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102036309"
---
# <a name="tutorial-create-and-deploy-first-azure-resource-manager-bicep-file"></a>Tutorial: Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Bicep-Datei

In diesem Tutorial lernen Sie [Bicep](./bicep-overview.md) kennen. Es wird gezeigt, wie Sie eine Bicep-Startdatei erstellen und in Azure bereitstellen. Sie erfahren mehr über die Struktur der Bicep-Datei und die Tools, die Sie für die Arbeit mit Bicep-Dateien benötigen. Dieses Tutorial dauert ungefähr **12 Minuten**, die tatsächliche Zeit variiert jedoch je nach Anzahl der Tools, die Sie installieren müssen.

Dieses Tutorial ist das erste einer Reihe. Im Laufe der Reihe ändern Sie die Bicep-Datei Schritt für Schritt, bis Sie alle wichtigen Bestandteile einer Bicep-Datei erkundet haben. Diese Elemente sind die Bausteine für weitaus komplexere Bicep-Dateien. Wir hoffen, dass Sie am Ende der Reihe Ihre eigenen Bicep-Dateien erstellen können und bereit sind, Ihre Bereitstellungen mit Bicep-Dateien zu automatisieren.

Weitere Informationen zu den Vorteilen der Verwendung von Bicep und der Automatisierung von Bereitstellungen mit Bicep-Dateien finden Sie unter [Was ist Bicep (Vorschau)?](./bicep-overview.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="get-tools"></a>Erforderliche Tools

Zunächst sorgen wir dafür, dass Sie über die Tools verfügen, die Sie zum Erstellen und Bereitstellen von Bicep-Dateien benötigen. Installieren Sie diese Tools auf Ihrem lokalen Computer.

### <a name="editor"></a>Editor

Zum Erstellen von Bicep-Dateien benötigen Sie einen guten Editor. Wir empfehlen Visual Studio Code mit der Bicep-Erweiterung. Informationen zum Installieren dieser Tools finden Sie bei Bedarf unter [Schnellstart: Erstellen von Bicep-Dateien mit Visual Studio Code](quickstart-create-bicep-use-visual-studio-code.md).

### <a name="command-line-deployment"></a>Befehlszeilenbereitstellung

Zum Bereitstellen der Bicep-Datei benötigen Sie außerdem die aktuelle Version von Azure PowerShell oder der Azure CLI. Die Installationsanweisungen finden Sie unter:

- [Installieren von Azure PowerShell](/powershell/azure/install-az-ps)
- [Installieren der Azure CLI unter Windows](/cli/azure/install-azure-cli-windows)
- [Installieren der Azure CLI unter Linux](/cli/azure/install-azure-cli-linux)
- [Installieren der Azure CLI unter macOS](/cli/azure/install-azure-cli-macos)

Melden Sie sich nach der Installation von Azure PowerShell oder der Azure CLI zum ersten Mal an. Hilfe finden Sie unter [Anmelden –PowerShell](/powershell/azure/install-az-ps#sign-in) oder [Anmelden – Azure CLI](/cli/azure/get-started-with-azure-cli#sign-in).

Nun sind Sie bereit, mehr über Bicep zu erfahren.

## <a name="create-your-first-bicep-file"></a>Erstellen Ihrer ersten Bicep-Datei

1. Öffnen Sie Visual Studio Code mit der installierten Bicep-Erweiterung.
1. Wählen Sie im Menü **Datei** zum Erstellen einer neuen Datei **Neue Datei** aus.
1. Wählen Sie im Menü **Datei** die Option **Speichern unter**.
1. Nennen Sie die Datei _azuredeploy_, und wählen Sie die Dateierweiterung _bicep_ aus. Der vollständige Name der Datei lautet _azuredeploy.bicep_.
1. Speichern Sie die Datei auf Ihrer Arbeitsstation. Wählen Sie einen einprägsamen Pfad aus, da Sie diesen Pfad später beim Bereitstellen der Bicep-Datei angeben.
1. Kopieren Sie den folgenden Inhalt in die Datei:

    ```bicep
    resource stg 'Microsoft.Storage/storageAccounts@2019-06-01' = {
      name: '{provide-unique-name}'
      location: 'eastus'
      sku: {
        name: 'Standard_LRS'
      }
      kind: 'StorageV2'
      properties: {
        supportsHttpsTrafficOnly: true
      }
    }
    ```

    Die Visual Studio Code-Umgebung sieht wie folgt aus:

    ![Visual Studio Code: erste Bicep-Datei](./media/Bicep-tutorial-create-first-bicep/resource-manager-visual-studio-code-first-bicep.png)

    Die Ressourcendeklaration umfasst vier Komponenten:

    - **resource:** Hierbei handelt es sich um das Schlüsselwort.
    - **symbolic name** (stg): Ein symbolischer Name ist ein Bezeichner zum Referenzieren der Ressource in Ihrer Bicep-Datei. Dabei handelt es sich nicht um den Namen der Ressource, der nach der Bereitstellung verwendet wird. Der Name der Ressource wird von der Eigenschaft **name** definiert.  Weitere Informationen finden Sie in der vierten Komponente in dieser Liste. Damit die Tutorials einfach befolgt werden können, wird **stg** als symbolischer Name für die Speicherkontoressource in dieser Tutorialreihe verwendet.
    - **resource type** (Microsoft.Storage/storageAccounts@2019-06-01): Diese Komponente besteht aus dem Ressourcenanbieter (Microsoft.Storage), dem Ressourcentyp (storageAccounts) und der API-Version (apiVersion: 2019-06-01). Jeder Ressourcenanbieter veröffentlicht seine eigenen API-Versionen. Dieser Wert unterscheidet sich daher je nach Typ. Weitere Typen und API-Versionen für verschiedene Azure-Ressourcen finden Sie in der [ARM-Vorlagenreferenz](/azure/templates/).
    - **properties** (alles innerhalb von „= {...}“): Dies sind die spezifischen Eigenschaften, die Sie für den angegebenen Ressourcentyp festlegen möchten. Dies sind genau die gleichen Eigenschaften, die Ihnen auch in einer ARM-Vorlage zur Verfügung stehen. Jede Ressource umfasst eine `name`-Eigenschaft. Die meisten Ressourcen verfügen auch über eine `location`-Eigenschaft, mit der die Region festgelegt wird, in der die Ressource bereitgestellt wird. Die anderen Eigenschaften variieren je nach Ressourcentyp und API-Version. Das Verständnis der Beziehung zwischen der API-Version und den verfügbaren Eigenschaften ist wichtig. Wir werden daher ausführlicher darauf eingehen.

        Für dieses Speicherkonto können Sie diese API-Version unter [storageAccounts 2019-06-01](/azure/templates/microsoft.storage/2019-06-01/storageaccounts) anzeigen. Bedenken Sie, dass Sie Ihrer Bicep-Datei nicht alle Eigenschaften hinzugefügt haben. Viele der Eigenschaften sind optional. Der `Microsoft.Storage`-Ressourcenanbieter könnte eine neue API-Version veröffentlichen, ohne dass sich die von Ihnen bereitgestellte Version ändert. Sie können diese Version weiterhin verwenden und dabei sicher sein, dass die Ergebnisse Ihrer Bereitstellung immer konsistent sind.

        Wenn Sie eine ältere API-Version anzeigen (z. B. [storageAccounts 2016-05-01](/azure/templates/microsoft.storage/2016-05-01/storageaccounts)), werden Sie feststellen, dass dort weniger Eigenschaften verfügbar sind.

        Falls Sie sich entscheiden, die API-Version für eine Ressource zu ändern, müssen Sie die Eigenschaften für diese Version prüfen und die Bicep-Datei entsprechend anpassen.

1. Ersetzen Sie `{provide-unique-name}` einschließlich der geschweiften Klammern `{}` durch einen eindeutigen Speicherkontonamen.

    > [!IMPORTANT]
    > Der Name des Speicherkontos muss innerhalb von Azure eindeutig sein. Der Name darf nur Kleinbuchstaben und Ziffern enthalten, und er darf maximal 24 Zeichen lang sein. Es sind verschiedene Benennungsmuster möglich. Sie können beispielsweise **store1** als Präfix verwenden und danach Ihre Initialen und das aktuelle Datum hinzufügen. Der Name könnte z. B. **store1abc09092019** lauten.

    Das Erraten des eindeutigen Namens für ein Speicherkonto ist nicht einfach und keine gute Lösung, wenn Sie umfangreiche Bereitstellungen automatisieren möchten. In einem späteren Teil dieser Tutorialreihe verwenden Sie Bicep-Funktionen, die das Erstellen eines eindeutigen Namens vereinfachen.

1. Speichern Sie die Datei .

Herzlichen Glückwunsch, Sie haben Ihre erste Bicep-Datei erstellt.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit Ihren Azure-Anmeldeinformationen an, um Azure PowerShell/Azure CLI zu verwenden.

Wählen Sie die Registerkarten in den folgenden Codeabschnitten aus, um zwischen Azure PowerShell und der Azure CLI zu wählen. Die CLI-Beispiele in diesem Artikel sind für die Bash-Shell geschrieben.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Connect-AzAccount
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az login
```

---

Wenn Sie über mehrere Azure-Abonnements verfügen, wählen Sie das Abonnement aus, das Sie verwenden möchten. Ersetzen Sie `[SubscriptionID/SubscriptionName]` und die eckigen Klammern `[]` durch Ihre Abonnementinformationen:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Set-AzContext [SubscriptionID/SubscriptionName]
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az account set --subscription [SubscriptionID/SubscriptionName]
```

---

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Beim Bereitstellen einer Bicep-Datei geben Sie eine Ressourcengruppe an, die die Ressourcen enthalten soll. Erstellen Sie die Ressourcengruppe vor dem Ausführen des Bereitstellungsbefehls entweder mit der Azure CLI oder Azure PowerShell.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroup `
  -Name myResourceGroup `
  -Location "Central US"
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group create \
  --name myResourceGroup \
  --location "Central US"
```

---

## <a name="deploy-bicep-file"></a>Bereitstellen der Bicep-Datei

Die Sprache ist eine transparente Abstraktion über Azure Resource Manager-Vorlagen (ARM-Vorlagen). Jede Bicep-Datei wird vor ihrer Bereitstellung in eine ARM-Standardvorlage kompiliert. Sie können die Bicep-Datei vor ihrer Bereitstellung in eine ARM-Vorlage kompilieren oder die Bicep-Datei direkt bereitstellen. Zum Bereitstellen der Bicep-Datei verwenden Sie entweder die Azure CLI oder Azure PowerShell. Verwenden Sie die Ressourcengruppe aus, die Sie erstellt haben. Geben Sie der Bereitstellung einen Namen, damit Sie sie im Bereitstellungsverlauf problemlos identifizieren können. Erstellen Sie der Einfachheit halber auch eine Variable, in der der Pfad zur Bicep-Datei gespeichert wird. Diese Variable erleichtert es Ihnen, die Bereitstellungsbefehle auszuführen, da Sie den Pfad nicht bei jeder Bereitstellung erneut eingeben müssen. Ersetzen Sie `{provide-the-path-to-the-bicep-file}` einschließlich der geschweiften Klammern `{}` durch den Pfad zu Ihrer Bicep-Datei mit dem Dateierweiterungsnamen _.bicep_.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Für die Ausführung dieses Bereitstellungs-Cmdlets benötigen Sie die [aktuelle Version](/powershell/azure/install-az-ps) von Azure PowerShell.

```azurepowershell
$bicepFile = "{provide-the-path-to-the-bicep-file}"
New-AzResourceGroupDeployment `
  -Name firstbicep `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $bicepFile
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für die Ausführung dieses Bereitstellungsbefehls müssen Sie über die [aktuelle Version](/cli/azure/install-azure-cli) der Azure CLI verfügen.

```azurecli
bicepFile="{provide-the-path-to-the-bicep-file}"
az deployment group create \
  --name firstbicep \
  --resource-group myResourceGroup \
  --template-file $bicepFile
```

---

Der Bereitstellungsbefehl gibt Ergebnisse zurück. Suchen Sie nach `ProvisioningState`, um zu sehen, ob die Bereitstellung erfolgreich war.

> [!NOTE]
> Wenn bei der Bereitstellung ein Fehler aufgetreten ist, verwenden Sie den Schalter `verbose`, um Informationen zu den erstellten Ressourcen abzurufen. Verwenden Sie den Schalter `debug`, um weitere Informationen zum Debuggen zu erhalten.

## <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppe im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Menü **Ressourcengruppen** aus.

1. Wählen Sie die im letzten Verfahren bereitgestellte Ressourcengruppe aus. Der Standardname ist **myResourceGroup**. In der Ressourcengruppe sollte keine Ressource bereitgestellt sein.

1. Hinweis: In der Übersicht wird rechts oben der Status der Bereitstellung angezeigt. Wählen Sie **1 Erfolgreich** aus.

   ![Anzeigen des Bereitstellungsstatus](./media/bicep-tutorial-create-first-bicep/deployment-status.png)

1. Der Verlauf der Bereitstellungen für die Ressourcengruppe wird angezeigt. Wählen Sie **firstbicep** aus.

   ![Auswählen der Bereitstellung](./media/bicep-tutorial-create-first-bicep/select-from-deployment-history.png)

1. Sie sehen eine Zusammenfassung der Bereitstellung. Ein Speicherkonto wurde bereitgestellt. Hinweis: Auf der linken Seite können Sie Eingaben, Ausgaben und die während der Bereitstellung verwendete Vorlage anzeigen.

   ![Zusammenfassung der Bereitstellungen anzeigen](./media/bicep-tutorial-create-first-bicep/view-deployment-summary.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Wenn Sie jetzt aufhören, sollten Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine einfache Bicep-Datei zum Bereitstellen in Azure erstellt.  In den nächsten Tutorials erfahren Sie, wie Sie einer Bicep-Datei Parameter, Variablen, Ausgaben und Module hinzufügen. Diese Features sind die Bausteine für weitaus komplexere Bicep-Dateien.

> [!div class="nextstepaction"]
> [Hinzufügen von Parametern](bicep-tutorial-add-parameters.md)
