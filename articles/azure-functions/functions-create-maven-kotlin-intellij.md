---
title: Erstellen einer Azure-Funktion mit Kotlin und IntelliJ
description: Hier erfahren Sie, wie Sie mit Kotlin und IntelliJ eine einfache, über HTTP ausgelöste serverlose App in Azure erstellen und veröffentlichen.
author: dglover
ms.service: azure-functions
ms.topic: quickstart
ms.date: 03/25/2020
ms.author: dglover
ms.openlocfilehash: 09dd868dc9e05241943899654d7c8bb427a8f268
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92104834"
---
# <a name="quickstart-create-your-first-http-triggered-function-with-kotlin-and-intellij"></a>Schnellstart: Erstellen Ihrer ersten, über HTTP ausgelösten Funktion mit Kotlin und IntelliJ

In diesem Artikel erfahren Sie, wie Sie mithilfe von IntelliJ IDEA und Apache Maven ein [serverloses](https://azure.microsoft.com/overview/serverless-computing/) Funktionsprojekt erstellen. Außerdem wird gezeigt, wie Sie Ihren Funktionscode lokal in der integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) debuggen und das Funktionsprojekt anschließend in Azure bereitstellen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Installieren Sie die folgende Software, um eine Funktion mit Kotlin und IntelliJ zu entwickeln:

- [Java Developer Kit](/azure/developer/java/fundamentals/java-jdk-long-term-support) (JDK), Version 8
- [Apache Maven](https://maven.apache.org), Version 3.0 oder höher
- [IntelliJ IDEA](https://www.jetbrains.com/idea/download), Community- oder Ultimate-Version mit Maven
- [Azure-Befehlszeilenschnittstelle](/cli/azure)
- [Version 2.x](functions-run-local.md#v2) der Azure Functions Core Tools. Sie stellen eine lokale Entwicklungsumgebung zum Schreiben, Ausführen und Debuggen von Azure Functions bereit.

> [!IMPORTANT]
> Damit Sie die Schritte in diesem Artikel ausführen können, muss die Umgebungsvariable JAVA_HOME auf den Installationsspeicherort des JDK festgelegt sein.

## <a name="create-a-functions-project"></a>Erstellen eines Functions-Projekts

1. Wählen Sie in IntelliJ IDEA **Neues Projekt erstellen** aus.  
1. Wählen Sie im Fenster **Neues Projekt** im linken Bereich **Maven** aus.
1. Aktivieren Sie das Kontrollkästchen **Create from archetype** (Aus Archetyp erstellen), und wählen Sie dann für [azure-functions-kotlin-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-kotlin-archetype) die Option **Add Archetype** (Archetyp hinzufügen) aus.
1. Füllen Sie im Fenster **Add Archetype** (Archetyp hinzufügen) die Felder wie folgt aus:
    - _GroupId_: com.microsoft.azure
    - _ArtifactId_: azure-functions-kotlin-archetype
    - _Version_: Verwenden Sie die neueste Version aus [dem zentralen Repository.](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-kotlin-archetype)
    ![Erstellung eines Maven-Projekts aus einem Archetyp in IntelliJ IDEA](media/functions-create-first-kotlin-intellij/functions-create-intellij.png)  
1. Wählen Sie **OK** und anschließend **Weiter** aus.
1. Geben Sie Ihre Details für das aktuelle Projekt ein, und wählen Sie **Fertig stellen** aus.

Maven erstellt die Projektdateien in einem neuen Ordner mit dem Wert von _ArtifactId_ als Name. Bei dem generierten Code des Projekts handelt es sich um eine einfache [durch HTTP ausgelöste](./functions-bindings-http-webhook.md) Funktion, die den Hauptteil der über HTTP ausgelösten Anforderung wiederholt.

## <a name="run-functions-locally-in-the-ide"></a>Lokales Ausführen von Funktionen in der IDE

> [!NOTE]
> Um Funktionen lokal ausführen und debuggen zu können, achten Sie darauf, dass die [Azure Functions Core Tools, Version 2](functions-run-local.md#v2), installiert sind.

1. Importieren Sie Änderungen manuell, oder aktivieren Sie den [automatischen Import](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html).
1. Öffnen Sie die Symbolleiste **Maven-Projekte**.
1. Erweitern Sie **Lifecycle** (Lebenszyklus), und öffnen Sie dann **Paket**. Die Lösung wird erstellt und in einem neu erstellten Zielverzeichnis verpackt.
1. Erweitern Sie **Plug-Ins** > **azure-functions**, und öffnen Sie **azure-functions:run**, um die Azure Functions-Runtime lokal zu starten.  
  ![Maven-Symbolleiste für Azure Functions](media/functions-create-first-kotlin-intellij/functions-intellij-kotlin-maven-toolbar.png)  

1. Schließen Sie das Dialogfeld „Ausführen“, wenn Sie den Test der Funktion beendet haben. Es kann nicht mehr als ein Funktionshost gleichzeitig aktiv sein und lokal ausgeführt werden.

## <a name="debug-the-function-in-intellij"></a>Debuggen der Funktion in IntelliJ

1. Um den Funktionshost im Debugmodus zu starten, fügen Sie **-DenableDebug** als Argument hinzu, wenn Sie Ihre Funktion ausführen. Sie können entweder die Konfiguration in [maven goals](https://www.jetbrains.com/help/idea/maven-support.html#run_goal) ändern oder den folgenden Befehl in einem Terminalfenster ausführen:  

   ```
   mvn azure-functions:run -DenableDebug
   ```

   Dieser Befehl bewirkt, dass der Funktionshost einen Debugport an 5005 öffnet.

1. Wählen Sie im Menü **Run** (Ausführen) **Edit Configurations** (Konfigurationen bearbeiten) aus.
1. Wählen Sie das Pluszeichen **+** aus, um einen **Remote** hinzuzufügen.
1. Füllen Sie die Felder _Name_ und _Einstellungen_ aus, und wählen Sie dann **OK** aus, um die Konfiguration zu speichern.
1. Wählen Sie nach dem Setup **Debug <Name der Remotekonfiguration>** aus, oder drücken Sie auf der Tastatur UMSCHALT+F9, um mit dem Debuggen zu beginnen.

   ![Debuggen von Funktionen in IntelliJ](media/functions-create-first-kotlin-intellij/debug-configuration-intellij.PNG)

1. Wenn Sie das Debuggen abgeschlossen haben, beenden Sie den Debugger und den laufenden Prozess. Es kann nicht mehr als ein Funktionshost gleichzeitig aktiv sein und lokal ausgeführt werden.

## <a name="deploy-the-function-to-azure"></a>Bereitstellen der Funktion in Azure

1. Bevor Sie Ihre Funktion in Azure bereitstellen können, müssen Sie sich [mithilfe der Azure CLI anmelden](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

   ``` azurecli
   az login
   ```

1. Stellen Sie Ihren Code mit `azure-functions:deploy` als Maven-Ziel in einer neuen Funktion bereit. Sie können auch die Option **azure-functions:deploy** im Maven Projects-Fenster aktivieren.

   ```
   mvn azure-functions:deploy
   ```

1. Suchen Sie die URL für Ihre Funktion in der Azure CLI-Ausgabe nach der erfolgreichen Bereitstellung der Funktion.

   ``` output
   [INFO] Successfully deployed Function App with package.
   [INFO] Deleting deployment package from Azure Storage...
   [INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
   [INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   ```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre erste Kotlin-Funktion in Azure bereitgestellt haben, können Sie sich im [Java-Entwicklerhandbuch für Azure Functions](functions-reference-java.md) mit weiteren Informationen zur Entwicklung von Java- und Kotlin-Funktionen vertraut machen.
- Fügen Sie mit `azure-functions:add` als Maven-Ziel zusätzliche Funktionen mit verschiedenen Triggern zu Ihrem Projekt hinzu.