---
title: Erstellen Ihrer ersten Funktion in Azure mit Visual Studio Code
description: Hier wird beschrieben, wie Sie mit der Azure Functions-Erweiterung in Visual Studio Code eine einfache per HTTP ausgelöste Funktion in Azure erstellen und veröffentlichen.
ms.topic: quickstart
ms.date: 01/10/2020
ms.custom: mvc, devcenter
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 0540c7b01d693975f34515c7d13f0477ac74d4a1
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842169"
---
# <a name="quickstart-create-an-azure-functions-project-using-visual-studio-code"></a>Schnellstart: Erstellen eines Azure Functions-Projekts mit Visual Studio Code

In diesem Artikel wird mithilfe von Visual Studio Code eine Funktion erstellt, die auf HTTP-Anforderungen reagiert. Der Code wird lokal getestet und anschließend in der serverlosen Umgebung von Azure Functions bereitgestellt. Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an. 

Es gibt auch eine [CLI-basierte Version](functions-create-first-azure-function-azure-cli.md) dieses Artikels.

## <a name="prerequisites"></a>Voraussetzungen

+ [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms) 
::: zone pivot="programming-language-csharp"
+ [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) für Visual Studio Code
::: zone-end
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/), Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen)
::: zone-end
::: zone pivot="programming-language-python"
+ [Python 3.7](https://www.python.org/downloads/release/python-375/) oder [Python 3.6](https://www.python.org/downloads/release/python-368/) (werden beide von Azure Functions unterstützt). Python 3.8 wird noch nicht unterstützt. 

+ [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python) für Visual Studio Code
::: zone-end
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Core SDK 2.2+](https://www.microsoft.com/net/download)

+ [PowerShell-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) 
::: zone-end

+ [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code 

+ [Azure-Konto](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) mit einem aktiven Abonnement. Sollten Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.

## <a name="create-an-azure-functions-project"></a>Erstellen Ihres lokalen Projekts 

In diesem Abschnitt wird mithilfe von Visual Studio Code ein lokales Azure Functions-Projekt in der gewünschten Sprache erstellt. Weiter unten in diesem Artikel wird der Funktionscode in Azure veröffentlicht. 

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** das Symbol **Neues Projekt erstellen** aus.

    ![Auswählen von „Neues Projekt erstellen“](media/functions-create-first-function-vs-code/create-new-project.png)

1. Wählen Sie einen Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Diese Schritte sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie in diesem Fall keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    ::: zone pivot="programming-language-csharp"

    | Aufforderung | Wert | 
    | ------ | ----- | 
    | „Select a language for your function project“ (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.) | C# |
    | „Select a version“ (Wählen Sie eine Version aus.) | Azure Functions v2 | 
    | Auswählen einer Vorlage für die erste Funktion Ihres Projekts | HTTP-Trigger | 
    | Angeben eines Funktionsnamens | HttpExample | 
    | Angeben eines Namespaces | My.Functions | 
    | Autorisierungsstufe | Anonym | 
    | Auswählen, wie Sie Ihr Projekt öffnen möchten | Hinzufügen zum Arbeitsbereich |

    ::: zone-end

    ::: zone pivot="programming-language-javascript"

    | Aufforderung | Wert | 
    | ------ | ----- | 
    | „Select a language for your function project“ (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.) | JavaScript | 
    | „Select a version“ (Wählen Sie eine Version aus.) | Azure Functions v2 | 
    | Auswählen einer Vorlage für die erste Funktion Ihres Projekts | HTTP-Trigger | 
    | Angeben eines Funktionsnamens | HttpExample | 
    | Autorisierungsstufe | Anonym | 
    | Auswählen, wie Sie Ihr Projekt öffnen möchten | Hinzufügen zum Arbeitsbereich |

    ::: zone-end

    ::: zone pivot="programming-language-typescript"

    | Aufforderung | Wert | 
    | ------ | ----- | 
    | „Select a language for your function project“ (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.) | TypeScript | 
    | „Select a version“ (Wählen Sie eine Version aus.) | Azure Functions v2 | 
    | Auswählen einer Vorlage für die erste Funktion Ihres Projekts | HTTP-Trigger | 
    | Angeben eines Funktionsnamens | HttpExample | 
    | Autorisierungsstufe | Anonym | 
    | Auswählen, wie Sie Ihr Projekt öffnen möchten | Hinzufügen zum Arbeitsbereich |

    ::: zone-end

    ::: zone pivot="programming-language-powershell"

    | Aufforderung | Wert | 
    | ------ | ----- | 
    | „Select a language for your function project“ (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.) | PowerShell | 
    | „Select a version“ (Wählen Sie eine Version aus.) | Azure Functions v2 | 
    | Auswählen einer Vorlage für die erste Funktion Ihres Projekts | HTTP-Trigger | 
    | Angeben eines Funktionsnamens | HttpExample | 
    | Autorisierungsstufe | Anonym | 
    | Auswählen, wie Sie Ihr Projekt öffnen möchten | Hinzufügen zum Arbeitsbereich |

    ::: zone-end

    ::: zone pivot="programming-language-python"

    | Aufforderung | Wert | 
    | ------ | ----- | 
    | „Select a language for your function project“ (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.) | Python | 
    | „Select a version“ (Wählen Sie eine Version aus.) | Azure Functions v2 | 
    | „Select a Python alias to create a virtual environment“ (Wählen Sie einen Python-Alias zum Erstellen einer virtuellen Umgebung aus.) | Python-Alias | 
    | Auswählen einer Vorlage für die erste Funktion Ihres Projekts | HTTP-Trigger | 
    | Angeben eines Funktionsnamens | HttpExample | 
    | Autorisierungsstufe | Anonym | 
    | Auswählen, wie Sie Ihr Projekt öffnen möchten | Hinzufügen zum Arbeitsbereich | 

    ::: zone-end

1. Von Visual Studio Code werden folgende Schritte ausgeführt:

    + Erstellen eines Azure Functions-Projekts in einem neuen Arbeitsbereich mit den Konfigurationsdateien [host.json](functions-host-json.md) und [local.settings.json](functions-run-local.md#local-settings-file) 

    ::: zone pivot="programming-language-csharp"

    + Erstellen der [Klassenbibliotheksdatei „HttpExample.cs“](functions-dotnet-class-library.md#functions-class-library-project) zur Implementierung der Funktion

    ::: zone-end
        
    ::: zone pivot="programming-language-javascript"
    
    + Erstellen der Datei „package.json“ im Stammordner

    + Erstellen des Ordners „HttpExample“ mit der [Definitionsdatei „function.json“](functions-reference-node.md#folder-structure) und der [Datei „index.js“](functions-reference-node.md#exporting-a-function) (Node.js-Datei mit dem Funktionscode)
    
    ::: zone-end
    
    ::: zone pivot="programming-language-typescript"
    
    + Erstellen der Datei „package.json“ und der Datei „tsconfig.json“ im Stammordner

    + Erstellen des Ordners „HttpExample“ mit der [Definitionsdatei „function.json“](functions-reference-node.md#folder-structure) und der [Datei „index.ts“](functions-reference-node.md#typescript) (TypeScript-Datei mit dem Funktionscode)
    
    ::: zone-end
    
    ::: zone pivot="programming-language-powershell"
    
    + Erstellen des Ordners „HttpExample“ mit der [Definitionsdatei „function.json“](functions-reference-python.md#programming-model) und der Datei „run.ps1“ mit dem Funktionscode
    
    ::: zone-end
    
    ::: zone pivot="programming-language-python"
    
    + Erstellen der Datei „requirements.txt“ mit den von Functions benötigten Paketen (auf der Projektebene)
    
    + Erstellen des Ordners „HttpExample“ mit der [Definitionsdatei „function.json“](functions-reference-python.md#programming-model) und der Datei „\_\_init\_\_.py“ mit dem Funktionscode
    
    ::: zone-end

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-python"

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

::: zone-end

::: zone pivot="programming-language-powershell"

[!INCLUDE [functions-run-function-test-local-vs-code-ps](../../includes/functions-run-function-test-local-vs-code-ps.md)]

::: zone-end

Nachdem Sie sich vergewissert haben, dass die Funktion auf Ihrem lokalen Computer korrekt ausgeführt wird, können Sie das Projekt mithilfe von Visual Studio Code direkt in Azure veröffentlichen. 

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>Ausführen der Funktion in Azure

1. Kopieren Sie die URL des HTTP-Triggers im Bereich **Ausgabe**. Fügen Sie auch hier die Abfragezeichenfolge `name` als `?name=<yourname>` am Ende dieser URL hinzu, und führen Sie die Anforderung aus.

    Die URL, über die Ihre per HTTP ausgelöste Funktion aufgerufen wird, sollte das folgende Format haben:

        http://<functionappname>.azurewebsites.net/api/httpexample?name=<yourname> 

1. Fügen Sie diese neue URL für die HTTP-Anforderung in die Adresszeile des Browsers ein. Das folgende Beispiel zeigt die von der Funktion im Browser zurückgegebene Antwort auf die GET-Remoteanforderung: 

    ![Funktionsantwort im Browser](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Schritt ([Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Funktion](functions-add-output-binding-storage-queue-vs-code.md)) fortfahren möchten, müssen alle Ihre Ressourcen erhalten bleiben, um darauf aufbauen zu können.

Andernfalls können Sie die Funktions-App und die zugehörigen Ressourcen wie im Anschluss beschrieben löschen, um weitere Kosten zu vermeiden.

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

Weitere Informationen zu den Kosten von Functions finden Sie unter [Abschätzen der Kosten des Verbrauchstarifs](functions-consumption-costs.md).

## <a name="next-steps"></a>Nächste Schritte

Sie haben Visual Studio Code genutzt, um eine Funktions-App mit einer einfachen Funktion zu erstellen, die über HTTP ausgelöst wird. Im nächsten Artikel erweitern Sie diese Funktion durch Hinzufügen einer Ausgabebindung. Diese Bindung schreibt die Zeichenfolge aus der HTTP-Anforderung in eine Nachricht in einer Azure Queue Storage-Warteschlange. 

> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Funktion](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
