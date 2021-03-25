---
title: 'Erstellen einer Java-Funktion mit Visual Studio Code: Azure Functions'
description: Erfahren Sie, wie Sie eine Java-Funktion erstellen und dann das lokale Projekt für serverloses Hosting in Azure Functions unter Verwendung der Azure Functions-Erweiterung in Visual Studio Code veröffentlichen.
ms.topic: quickstart
ms.date: 11/03/2020
adobe-target: true
adobe-target-activity: DocsExp–386541–A/B–Enhanced-Readability-Quickstarts–2.19.2021
adobe-target-experience: Experience B
adobe-target-content: ./create-first-function-vs-code-java-uiex
ms.openlocfilehash: 10ac677fb65428e8b8855c43b319bbebe7bbbb71
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101701431"
---
# <a name="quickstart-create-a-java-function-in-azure-using-visual-studio-code"></a>Schnellstart: Erstellen einer Java-Funktion in Azure mit Visual Studio Code

> [!div class="op_single_selector" title1="Wählen Sie Ihre Funktionssprache aus: "]
> - [Java](create-first-function-vs-code-java.md)
> - [Python](create-first-function-vs-code-python.md)
> - [C#](create-first-function-vs-code-csharp.md)
> - [JavaScript](create-first-function-vs-code-node.md)
> - [PowerShell](create-first-function-vs-code-powershell.md)
> - [TypeScript](create-first-function-vs-code-typescript.md)
> - [Sonstiges (Go/Rust)](create-first-function-vs-code-other.md)

In diesem Artikel wird mithilfe von Visual Studio Code eine Java-Funktion erstellt, die auf HTTP-Anforderungen reagiert. Der Code wird lokal getestet und anschließend in der serverlosen Umgebung von Azure Functions bereitgestellt.

Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an.

> [!NOTE]
> Sollte Visual Studio Code nicht Ihr bevorzugtes Entwicklungstool sein, stehen ähnliche Tutorials mit [Maven](create-first-function-cli-java.md), [Gradle](./functions-create-first-java-gradle.md) und [IntelliJ IDEA](/azure/developer/java/toolkit-for-intellij/quickstart-functions) für Java-Entwickler zur Verfügung.

## <a name="configure-your-environment"></a>Konfigurieren Ihrer Umgebung

Vergewissern Sie sich zunähst, dass Folgendes vorhanden ist:

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Java Developer Kit](/azure/developer/java/fundamentals/java-jdk-long-term-support), Version 8 oder 11.

+ [Apache Maven](https://maven.apache.org), Version 3.0 oder höher

+ [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)

+ [Java-Erweiterungspaket](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)  

+ [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code 

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Erstellen Ihres lokalen Projekts

In diesem Abschnitt wird mithilfe von Visual Studio Code ein lokales Azure Functions-Projekt in Java erstellt. Weiter unten in diesem Artikel wird der Funktionscode in Azure veröffentlicht. 

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** das Symbol **Neues Projekt erstellen** aus.

    ![Auswählen von „Neues Projekt erstellen“](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Wählen Sie einen Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Diese Schritte sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie in diesem Fall keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Select a language for your function project** (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.): Wählen Sie die Option `Java`.

    + **Select a version of Java** (Wählen Sie eine Java-Version aus): Wählen Sie `Java 8` oder `Java 11` aus, die Java-Version, mit der Ihre Funktionen in Azure ausgeführt werden. Wählen Sie eine Java-Version aus, die Sie lokal überprüft haben.

    + **Provide a group ID** (Geben Sie eine Gruppen-ID an.): Wählen Sie die Option `com.function`.

    + **Provide an artifact ID** (Geben Sie eine Artefakt-ID an.): Wählen Sie die Option `myFunction`.

    + **Provide a version** (Geben Sie eine Version an.): Wählen Sie die Option `1.0-SNAPSHOT`.

    + **Provide a package name** (Geben Sie einen Paketnamen an.): Wählen Sie die Option `com.function`.

    + **Provide an app name** (Geben Sie einen App-Namen an.): Wählen Sie die Option `myFunction-12345`.

    + **Autorisierungsstufe:** Wählen Sie `Anonymous` aus, damit Ihr Funktionsendpunkt von jedem Benutzer aufgerufen werden kann. Weitere Informationen zur Autorisierungsstufe finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Select how you would like to open your project** (Wählen Sie aus, wie Sie Ihr Projekt öffnen möchten.): Wählen Sie die Option `Add to workspace`.

1. Auf der Grundlage dieser Informationen generiert Visual Studio Code ein Azure Functions-Projekt mit einem HTTP-Trigger. Die lokalen Projektdateien können im Explorer angezeigt werden. Weitere Informationen zu den erstellten Dateien finden Sie unter [Generierte Projektdateien](functions-develop-vs-code.md#generated-project-files). 

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Nachdem Sie sich vergewissert haben, dass die Funktion auf Ihrem lokalen Computer korrekt ausgeführt wird, können Sie das Projekt mithilfe von Visual Studio Code direkt in Azure veröffentlichen.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

[!INCLUDE [functions-vs-code-run-remote](../../includes/functions-vs-code-run-remote.md)]

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben [Visual Studio Code](functions-develop-vs-code.md?tabs=java) genutzt, um eine Funktions-App mit einer einfachen Funktion zu erstellen, die über HTTP ausgelöst wird. Im nächsten Artikel erweitern Sie diese Funktion, indem Sie eine Verbindung mit Azure Storage herstellen. Weitere Informationen zum Herstellen einer Verbindung mit anderen Azure-Diensten finden Sie unter [Hinzufügen von Bindungen zu einer vorhandenen Funktion in Azure Functions](add-bindings-existing-function.md?tabs=java). 

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einer Azure Storage-Warteschlange](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-java)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
