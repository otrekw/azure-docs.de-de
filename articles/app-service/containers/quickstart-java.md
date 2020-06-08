---
title: 'Schnellstart: Erstellen einer Linux-Java-App'
description: Führen Sie erste Schritte mit Linux-Apps in Azure App Service aus, indem Sie Ihre erste Java-App in einem Linux-Container in App Service bereitstellen.
keywords: Azure, App Service, Web-App, Linux, Java, Maven, Schnellstart
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/27/2019
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 1ed7126f2698294ac6706aafcb85e3229a7491bb
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300065"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-linux"></a>Schnellstart: Erstellen einer Java-App in Azure App Service für Linux

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter dem Linux-Betriebssystem. In dieser Schnellstartanleitung wird gezeigt, wie Sie mit der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) und dem [Azure-Web-App-Plug-In für Maven](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) eine Java-Webarchivdatei (WAR) unter Linux bereitstellen.

> [!NOTE]
>
> Dazu können auch gängige IDEs wie IntelliJ, Eclipse und VS Code verwendet werden. Sehen Sie sich unsere ähnlichen Dokumente in den [Schnellstarts für Azure-Toolkit für IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app), [Azure-Toolkit für Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app) oder [VS Code](https://code.visualstudio.com/docs/java/java-webapp) an.
>
![In Azure App Service ausgeführte Beispiel-App](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Erstellen einer Java-App

Führen Sie den folgenden Maven-Befehl über die Eingabeaufforderung der Cloud Shell aus, um eine neue App mit dem Namen `helloworld` zu erstellen:

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp" -Dversion=1.0-SNAPSHOT
```
Ändern Sie anschließend Ihr Arbeitsverzeichnis in den Projektordner:

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>Konfigurieren des Maven-Plug-Ins

Der Prozess zur Bereitstellung in Azure App Service kann Ihre Azure-Anmeldeinformation automatisch aus der Azure CLI übernehmen. Wenn Sie Azure CLI nicht installiert haben, werden Sie vom Maven-Plug-In mit OAuth oder der Geräteanmeldung angemeldet. Überprüfen Sie bei Bedarf die Details zur [Authentifizierung mit Maven-Plug-Ins](https://github.com/microsoft/azure-maven-plugins/wiki/Authenticatio).

Um die Bereitstellung zu konfigurieren, führen Sie den maven-Befehl an der Eingabeaufforderung aus und verwenden die Standardkonfigurationen, indem Sie die **EINGABETASTE** drücken, bis die Eingabeaufforderung **Bestätigen [ja/nein]** angezeigt wird. Drücken Sie dann **‚j‘** , um die Konfiguration abzuschließen. 
```cmd
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```
Ein Beispielprozess sieht wie folgt aus:

```cmd
~@Azure:~/helloworld$ mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------------< example.demo:helloworld >-----------------------
[INFO] Building helloworld Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.9.1:config (default-cli) @ helloworld ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. Java 11
2. Java 8 [*]
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. TOMCAT 8.5 [*]
Enter index to use:
Please confirm webapp properties
AppName : helloworld-1558400876966
ResourceGroup : helloworld-1558400876966-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : TOMCAT 8.5-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

> [!NOTE]
> In diesem Artikel arbeiten wir nur mit Java-Apps, die in WAR-Dateien enthalten sind. Das Plug-In unterstützt auch JAR-Webanwendungen. Dies können Sie unter [Bereitstellen einer Spring Boot-App mit JAR-Datei in Azure App Service unter Linux](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) ausprobieren.

Öffnen Sie `pom.xml`, um die aktualisierte Konfiguration anzuzeigen.

```bash
code pom.xml
```

Sie können bei Bedarf die Konfigurationen für den App Service direkt in Ihrer POM-Datei ändern. Einige gängige sind nachstehend aufgeführt:

 Eigenschaft | Erforderlich | Beschreibung | Version
---|---|---|---
`<schemaVersion>` | false | Gibt die Version des Konfigurationsschemas an. Folgende Werte werden unterstützt: `v1` und `v2`. | 1.5.2
`<resourceGroup>` | true | Azure-Ressourcengruppe für Ihre Web-App. | 0.1.0+
`<appName>` | true | Der Name Ihrer Web-App. | 0.1.0+
`<region>` | true | Gibt die Region an, in der Ihre Web-App gehostet wird. Der Standardwert ist **westeurope**. Alle gültigen Regionen finden Sie im Abschnitt [Unterstützten Regionen](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+
`<pricingTier>` | false | Der Tarif für Ihre Web-App. Der Standardwert ist **P1V2**.| 0.1.0+
`<runtime>` | true | Details zur Konfiguration der Laufzeitumgebung finden Sie [hier](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+
`<deployment>` | true | Details zur Konfiguration der Bereitstellung finden Sie [hier](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=config)

## <a name="deploy-the-app"></a>Bereitstellen der App

Stellen Sie Ihre Java-App mit dem folgenden Befehl in Azure bereit:

```bash
mvn package azure-webapp:deploy
```

Navigieren Sie nach Abschluss der Bereitstellung zur bereitgestellten Anwendung, indem Sie in Ihrem Webbrowser die folgende URL verwenden, z. B. `http://<webapp>.azurewebsites.net`. 

![In Azure App Service ausgeführte Beispiel-App](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**Glückwunsch!** Sie haben Ihre erste Java-App für App Service unter Linux bereitgestellt.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=deploy)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Ressourcengruppe erstellt. Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, löschen Sie die Ressourcengruppe im Portal, indem Sie den folgenden Befehl in Cloud Shell ausführen:

```azurecli-interactive
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

Die Ausführung dieses Befehls kann eine Minute in Anspruch nehmen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Abfragen einer Azure SQL-Datenbank mithilfe von Java](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Schnellstart: Verwenden von Java zum Herstellen einer Verbindung mit Azure Database for MySQL und zum Abfragen von Daten](/azure/mysql/connect-java)

> [!div class="nextstepaction"]
> [Schnellstart: Verwenden von Java zum Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Einzelserver und zum Abfragen von Daten](/azure/postgresql/connect-java)

> [!div class="nextstepaction"]
> [Konfigurieren der Java-App](configure-language-java.md)

> [!div class="nextstepaction"]
> [CI/CD mit Jenkins](/azure/jenkins/deploy-jenkins-app-service-plugin)

> [!div class="nextstepaction"]
> [Weitere Azure-Ressourcen für Java-Entwickler](/java/azure/)

> [!div class="nextstepaction"]
> [Weitere Informationen zu Maven-Plug-Ins für Azure](https://github.com/microsoft/azure-maven-plugins)
