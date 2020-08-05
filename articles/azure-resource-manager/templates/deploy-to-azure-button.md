---
title: Schaltfläche zum Bereitstellen in Azure
description: Verwenden Sie diese Schaltfläche, um Azure Resource Manager-Vorlagen aus einem GitHub-Repository bereitzustellen.
ms.topic: conceptual
ms.date: 07/20/2020
ms.openlocfilehash: 9fe69eba2a91bf19e0662ae071c222905c348666
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079469"
---
# <a name="use-a-deployment-button-to-deploy-templates-from-github-repository"></a>Verwenden einer Bereitstellungsschaltfläche zum Bereitstellen von Vorlagen aus einem GitHub-Repository

In diesem Artikel wird beschrieben, wie Sie die Schaltfläche **In Azure bereitstellen** verwenden, um Vorlagen aus einem GitHub-Repository bereitzustellen. Sie können die Schaltfläche direkt zur README.md-Datei in Ihrem GitHub-Repository oder zu einer Webseite hinzufügen, die auf das Repository verweist. Diese Methode unterstützt nur die Bereitstellung auf Ressourcengruppenebene.

## <a name="use-common-image"></a>Verwenden eines einheitlichen Bilds

Verwenden Sie das folgende Bild, wenn Sie die Schaltfläche zu Ihrer Webseite oder Ihrem Repository hinzufügen:

```html
<img src="https://aka.ms/deploytoazurebutton"/>
```

Das Bild sieht so aus:

![Schaltfläche zum Bereitstellen in Azure](https://aka.ms/deploytoazurebutton)

## <a name="create-url-for-deploying-template"></a>Erstellen einer URL zum Bereitstellen einer Vorlage

Beginnen Sie mit der unformatierten URL für die Vorlage in Ihrem Repository, um die URL für Ihre Vorlage zu erstellen: Klicken Sie auf **Roh**, um die unformatierte URL anzuzeigen.

:::image type="content" source="./media/deploy-to-azure-button/select-raw.png" alt-text="Schaltfläche „Roh“ anklicken":::

Die URL hat folgendes Format:

```html
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Wenden Sie dann eine URL-Codierung an. Sie können einen Onlineencoder verwenden oder einen Befehl ausführen. Das folgende PowerShell-Beispiel zeigt, wie ein Wert URL-codiert wird.

```powershell
[uri]::EscapeDataString($url)
```

Die Beispiel-URL weist mit URL-Codierung folgenden Wert auf.

```html
https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

Jeder Link beginnt mit der gleichen Basis-URL:

```html
https://portal.azure.com/#create/Microsoft.Template/uri/
```

Fügen Sie Ihren URL-codierten Link zur Vorlage am Ende der Basis-URL hinzu.

```html
https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

Damit ist die URL für Ihren Link vollständig.

## <a name="create-deploy-to-azure-button"></a>Erstellen der Schaltfläche für die Bereitstellung in Azure

Fügen Sie zum Schluss Link und Bild zusammen.

Um die Schaltfläche über Markdown in die README.md-Datei in Ihrem GitHub-Repository oder einer Webseite hinzuzufügen, verwenden Sie folgenden Befehl:

```markdown
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)
```

Verwenden Sie für HTML folgenden Code:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json" target="_blank">
  <img src="https://aka.ms/deploytoazurebutton"/>
</a>
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Um die vollständige Lösung zu testen, klicken Sie auf die folgende Schaltfläche:

[![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)

Das Portal zeigt einen Bereich an, indem Sie ganz einfach Parameterwerte angeben können. Die Parameter sind mit den Standardwerten aus der Vorlage vorab ausgefüllt.

![Verwenden des Portals für die Bereitstellung](./media/deploy-to-azure-button/portal.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](template-syntax.md).
