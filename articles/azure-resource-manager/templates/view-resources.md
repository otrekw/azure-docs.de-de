---
title: Ermitteln von Ressourceneigenschaften
description: Beschreibt, wie Sie nach Ressourceneigenschaften suchen.
ms.topic: conceptual
ms.date: 06/10/2020
ms.openlocfilehash: c8bbe0dcb1bc9dc9751a1dc0d0b98a6368473546
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91327747"
---
# <a name="discover-resource-properties"></a>Ermitteln von Ressourceneigenschaften

Bevor Sie Resource Manager-Vorlagen erstellen, müssen Sie wissen, welche Ressourcentypen verfügbar sind und welche Werte in Ihrer Vorlage verwendet werden sollen. In diesem Artikel werden einige Möglichkeiten gezeigt, wie Sie die Eigenschaften finden können, die in Ihre Vorlage eingeschlossen werden sollen.

## <a name="find-resource-provider-namespaces"></a>Suchen von Namespaces für Ressourcenanbieter

Ressourcen in einer ARM-Vorlage werden mit einem Ressourcenanbieter-Namespace und einem Ressourcentyp definiert. Beispielsweise ist „Microsoft.Storage/storageAccounts“ der vollständige Name des Speicherkonto-Ressourcentyps. „Microsoft.Storage“ ist der Namespace. Wenn Sie die Namespaces für die Ressourcentypen, die Sie verwenden möchten, noch nicht kennen, finden Sie weitere Informationen unter [Ressourcenanbieter für Azure-Dienste](../management/azure-services-resource-providers.md).

![Zuordnung von Ressourcenanbieter-Namespaces in Resource Manager](./media/view-resources/resource-provider-namespace-and-azure-service-mapping.png)

## <a name="export-templates"></a>Exportieren von Vorlagen

Die einfachste Möglichkeit zum Abrufen der Vorlageneigenschaften für Ihre vorhandene Ressourcen besteht darin, die Vorlage zu exportieren. Weitere Informationen finden Sie unter [Exportieren von einzelnen und mehreren Ressourcen in eine Vorlage im Azure-Portal](./export-template-portal.md).

## <a name="use-resource-manager-tools-extension"></a>Verwenden der Erweiterung „Resource Manager-Tools“

Visual Studio Code und die Erweiterung „Azure Resource Manager-Tools“ helfen Ihnen dabei, exakt herauszufinden, welche Eigenschaften für den jeweiligen Ressourcentyp erforderlich sind. Sie bieten IntelliSense und Codeausschnitte, wodurch sich die Definition einer Ressource in Ihrer Vorlage vereinfacht. Weitere Informationen finden Sie unter [Quickstart: Erstellen von Azure Resource Manager-Vorlagen mit Visual Studio Code](./quickstart-create-templates-use-visual-studio-code.md#add-an-azure-resource).

Der folgende Screenshot zeigt eine einer Vorlage hinzugefügte Speicherkontoressource:

![Codeausschnitte der Erweiterung „Resource Manager-Tools“](./media/view-resources/resource-manager-tools-extension-snippets.png)

Die Erweiterung bietet außerdem eine Liste der Optionen für die Konfigurationseigenschaften.

![Konfigurierbare Werte er Erweiterung „Resource Manager-Tools“](./media/view-resources/resource-manager-tools-extension-configurable-properties.png)

## <a name="use-template-reference"></a>Verwenden der Vorlagenreferenz

Die Azure Resource Manager-Vorlage ist die umfassendste Ressource für das Vorlagenschema. Darin finden Sie API-Versionen, Vorlagenformat und Eigenschaftsinformationen.

1. Wechseln Sie zu [Azure Resource Manager-Vorlagenreferenz](/azure/templates/).
1. Wählen Sie im linken Navigationsbereich **Speicher** und dann **Alle Ressourcen** aus. Auf der Seite „Alle Ressourcen“ werden die Ressourcentypen und -versionen zusammengefasst.

    ![Vorlagenreferenz Ressourcenversionen](./media/view-resources/resource-manager-template-reference-resource-versions.png)

    Wenn Sie den Ressourcentyp kennen, können Sie mit dem folgenden URL-Format direkt zu dieser Seite wechseln: `https://docs.microsoft.com/azure/templates/{provider-namespace}/{resource-type}`.

1. Wählen Sie die neueste Version aus. Es wird immer empfohlen, die neueste API-Version zu verwenden.

    Im Abschnitt **Vorlagenformat** werden alle Eigenschaften für ein Speicherkonto aufgelistet. **sku** ist in der Liste enthalten:

    ![Vorlagenreferenz Speicherkontoformat](./media/view-resources/resource-manager-template-reference-storage-account-sku.png)

    Scrollen Sie nach unten, um **SKU-Objekt** im Abschnitt **Eigenschaftswerte** anzuzeigen. Im Artikel werden die zulässigen Werte für den SKU-Namen gezeigt:

    ![Vorlagenreferenz Speicherkonto SKU-Werte](./media/view-resources/resource-manager-template-reference-storage-account-sku-values.png)

    Am Ende der Seite werden im Abschnitt **Schnellstartvorlagen** einige Azure-Schnellstartvorlagen aufgelistet, die den Ressourcentyp enthalten:

    ![Vorlagenreferenz Speicherkonto Schnellstartvorlagen](./media/view-resources/resource-manager-template-reference-quickstart-templates.png)

Auf jeder der Azure-Dienstdokumentationssites gibt es einen Link zur Vorlagenreferenz.  Beispielsweise die [Key Vault-Dokumentationssite](../../key-vault/general/overview.md):

![Resource Manager-Vorlagenreferenz Key Vault](./media/view-resources/resource-manager-template-reference-key-vault.png)

## <a name="use-resource-explorer"></a>Verwenden von Ressourcen-Explorer

Der Ressourcen-Explorer ist in das Azure-Portal eingebettet. Bevor Sie diese Methode verwenden, benötigen Sie ein Speicherkonto. Wenn Sie keins haben, wählen Sie die folgende Schaltfläche aus, um eins zu erstellen:

[![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Geben Sie im Suchfeld **Ressourcen-Explorer** ein, und wählen Sie dann **Ressourcen-Explorer** aus.

    ![Screenshot, der das Suchen nach dem Ressourcen-Explorer im Azure-Portal zeigt.](./media/view-resources/azure-portal-resource-explorer.png)

1. Erweitern Sie auf der linken Seite **Abonnements** und dann Ihr Azure-Abonnement. Sie finden das Speicherkonto entweder unter **Providers** (Anbieter) oder unter **ResourceGroups** (Ressourcengruppen).

    ![Azure-Portal Ressourcen-Explorer](./media/view-resources/azure-portal-resource-explorer-home.png)

    - **Providers**: Erweitern Sie **Providers** -> **Microsoft.Storage** -> **storageaccounts**, und wählen Sie dann Ihr Speicherkonto aus.
    - **ResourceGroups**: Wählen Sie die Ressourcengruppe aus, die das Speicherkonto enthält, wählen Sie **Ressourcen** aus, und wählen Sie dann das Speicherkonto aus.

    Auf der rechten Seite wird die SKU-Konfiguration für das vorhandene Speicherkonto ähnlich dem folgenden angezeigt:

    ![Azure-Portal Ressourcen-Explorer Speicherkonto-SKU](./media/view-resources/azure-portal-resource-explorer-sku.png)

## <a name="use-resourcesazurecom"></a>Verwenden von „Resources.azure.com“

„Resources.Azure.com“ ist eine öffentliche Website, auf die jeder mit einem Azure-Abonnement zugreifen kann. Die Site befindet sich in der Vorschauversion.  Erwägen Sie stattdessen die Verwendung des [Ressourcen-Explorers](#use-resource-explorer). Dieses Tool bietet diese Funktionen:

- Ermitteln der APIs für die Azure-Ressourcenverwaltung.
- Abrufen der API-Dokumentation und von Schemainformationen.
- Vornehmen von direkten API-Aufrufen in Ihren eigenen Abonnements.

Um zu veranschaulichen, wie Sie Schemainformationen mithilfe dieses Tools abrufen, benötigen Sie ein Speicherkonto. Wenn Sie keins haben, wählen Sie die folgende Schaltfläche aus, um eins zu erstellen:

[![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json)

1. Wechseln Sie zu [resources.azure.com](https://resources.azure.com/). Es dauert ein wenig, bis das Tool den linken Bereich aufgefüllt hat.
1. Wählen Sie **Abonnements** aus.

    ![resource.azure.com API-Zuordnung](./media/view-resources/resources-azure-com-api-mapping.png)

    Der linke Knoten entspricht dem API-Aufruf auf der rechten Seite. Sie können den API-Aufruf ausführen, indem Sie die Schaltfläche **GET** auswählen.
1. Erweitern Sie auf der linken Seite **Abonnements** und dann Ihr Azure-Abonnement. Sie finden das Speicherkonto entweder unter **Providers** (Anbieter) oder unter **ResourceGroups** (Ressourcengruppen).

    - **Providers**: Erweitern Sie **Providers** -> **Microsoft.Storage** -> **storageaccounts**, und wechseln Sie dann zu dem Speicherkonto.
    - **ResourceGroups**: Wählen Sie die Ressourcengruppe aus, die das Speicherkonto enthält, und wählen Sie dann **Ressourcen** aus.

    Auf der rechten Seite wird die SKU-Konfiguration für das vorhandene Speicherkonto ähnlich dem folgenden angezeigt:

    ![Azure-Portal Ressourcen-Explorer Speicherkonto-SKU](./media/view-resources/azure-portal-resource-explorer-sku.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Vorlagenschemainformationen finden. Weitere Informationen zum Erstellen von Resource Manager-Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](./template-syntax.md).
