---
title: 'Schnellstart: Erstellen einer Java-App in Azure App Service'
description: Stellen Sie in wenigen Minuten Ihre erste Java-App vom Typ „Hallo Welt“ in Azure App Service bereit. Mit dem Azure-Web-App-Plug-In für Maven können Sie Java-Apps bequem bereitstellen.
keywords: Azure, App Service, Web-App, Windows, Linux, Java, Maven, Schnellstart
author: jasonfreeberg
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 08/01/2020
ms.author: jafreebe
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
zone_pivot_groups: app-service-platform-windows-linux
ms.openlocfilehash: 274228ea5aa9ac9de9725176c8b6221ee9e9542e
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88182696"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service"></a>Schnellstart: Erstellen einer Java-App in Azure App Service

Von [Azure App Service](overview.md) wird ein hochgradig skalierbarer Webhostingdienst mit Self-Patching bereitgestellt.  In dieser Schnellstartanleitung wird gezeigt, wie Sie mit der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) und dem [Azure-Web-App-Plug-In für Maven](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) eine Java-Webarchivdatei (WAR) bereitstellen.

> [!NOTE]
> In diesem Artikel arbeiten wir nur mit Java-Apps, die in WAR-Dateien enthalten sind. Das Plug-In unterstützt auch JAR-Webanwendungen. Dies können Sie unter [Bereitstellen einer Spring Boot-App mit JAR-Datei in Azure App Service unter Linux](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) ausprobieren.

> [!NOTE]
> Dazu können auch gängige IDEs wie IntelliJ und Eclipse verwendet werden. Sehen Sie sich unsere ähnlichen Dokumente in den Schnellstarts für [Azure-Toolkit für IntelliJ](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app) oder [Azure-Toolkit für Eclipse](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app) an.
>
![In Azure App Service ausgeführte Beispiel-App](./media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Erstellen einer Java-App

Führen Sie den folgenden Maven-Befehl über die Eingabeaufforderung der Cloud Shell aus, um eine neue App mit dem Namen `helloworld` zu erstellen:

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp" "-Dversion=1.0-SNAPSHOT"
```

Ändern Sie anschließend Ihr Arbeitsverzeichnis in den Projektordner:

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>Konfigurieren des Maven-Plug-Ins

Der Prozess zur Bereitstellung in Azure App Service kann Ihre Azure-Anmeldeinformation automatisch aus der Azure CLI übernehmen. Wenn Sie die Azure CLI nicht lokal installiert haben, werden Sie vom Maven-Plug-In mit OAuth oder der Geräteanmeldung angemeldet. Überprüfen Sie bei Bedarf die Details zur [Authentifizierung mit Maven-Plug-Ins](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication).

Sie können den folgenden Maven-Befehl ausführen, um die Bereitstellung zu konfigurieren.
```bash
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```

::: zone pivot="platform-windows" 
Sie werden zur Auswahl der folgenden Angaben aufgefordert: 
* **OS(Default: `linux`)**
* **javaVersion(Default: `1.8`)**
* **webContainer(Default: `tomcat 8.5`)** 
 
Achten Sie darauf, **`2`** einzugeben, wenn Sie im ersten Schritt das Betriebssystem **windows** auswählen. Für die anderen Konfigurationen können die Standardeinstellungen übernommen werden, indem Sie die **EINGABETASTE** drücken. Drücken Sie schließlich in der Eingabeaufforderung **Confirm (Y/N)** (Bestätigen (J/N)) die Taste **`Y`** , um die Konfiguration abzuschließen.

Ein Beispielprozess sieht wie folgt aus:

```console
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
Enter index to use: 2
Define value for javaVersion(Default: 1.8): 
1. 1.7
2. 1.7.0_191_ZULU
3. 1.7.0_51
4. 1.7.0_71
5. 1.7.0_80
6. 1.8 [*]
7. 1.8.0_102
8. 1.8.0_111
9. 1.8.0_144
10. 1.8.0_172
11. 1.8.0_172_ZULU
12. 1.8.0_181
13. 1.8.0_181_ZULU
14. 1.8.0_202
15. 1.8.0_202_ZULU
16. 1.8.0_25
17. 1.8.0_60
18. 1.8.0_73
19. 1.8.0_92
20. 11
21. 11.0.2_ZULU
Enter index to use:
Define value for webContainer(Default: tomcat 8.5): 
1. jetty 9.1
2. jetty 9.1.0.20131115
3. jetty 9.3
4. jetty 9.3.13.20161014
5. tomcat 7.0
6. tomcat 7.0.50
7. tomcat 7.0.62
8. tomcat 8.0
9. tomcat 8.0.23
10. tomcat 8.5 [*]
11. tomcat 8.5.20
12. tomcat 8.5.31
13. tomcat 8.5.34
14. tomcat 8.5.37
15. tomcat 8.5.6
16. tomcat 9.0
17. tomcat 9.0.0
18. tomcat 9.0.12
19. tomcat 9.0.14
20. tomcat 9.0.8
Enter index to use:
Please confirm webapp properties
AppName : helloworld-1590394316693
ResourceGroup : helloworld-1590394316693-rg
Region : westeurope
PricingTier : PremiumV2_P1v2
OS : Windows
Java : 1.8
WebContainer : tomcat 8.5
Deploy to slot : false
Confirm (Y/N)? :
[INFO] Saving configuration to pom.
```
::: zone-end
::: zone pivot="platform-linux"  

Sie werden zur Auswahl der folgenden Angaben aufgefordert: 
* **OS(Default: `linux`)**
* **javaVersion(Default: `Java 8`)**
* **webContainer(Default: `Tomcat 8.5`)** 

Für alle Konfigurationen können die Standardeinstellungen übernommen werden, indem Sie die **EINGABETASTE** drücken. Drücken Sie schließlich in der Eingabeaufforderung **Confirm (Y/N)** (Bestätigen (J/N)) die Taste **`Y`** , um die Konfiguration abzuschließen.
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
::: zone-end

Sie können bei Bedarf die Konfigurationen für App Service direkt in der Datei `pom.xml` ändern. Einige gängige sind nachstehend aufgeführt:

 Eigenschaft | Erforderlich | Beschreibung | Version
---|---|---|---
`<schemaVersion>` | false | Gibt die Version des Konfigurationsschemas an. Folgende Werte werden unterstützt: `v1` und `v2`. | 1.5.2
`<resourceGroup>` | true | Azure-Ressourcengruppe für Ihre Web-App. | 0.1.0+
`<appName>` | true | Der Name Ihrer Web-App. | 0.1.0+
`<region>` | true | Gibt die Region an, in der Ihre Web-App gehostet wird. Der Standardwert ist **westeurope**. Alle gültigen Regionen finden Sie im Abschnitt [Unterstützten Regionen](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+
`<pricingTier>` | false | Der Tarif für Ihre Web-App. Der Standardwert ist **P1V2**.| 0.1.0+
`<runtime>` | true | Details zur Konfiguration der Laufzeitumgebung finden Sie [hier](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+
`<deployment>` | true | Details zur Konfiguration der Bereitstellung finden Sie [hier](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+

Notieren Sie sich die Werte `<appName>` und `<resourceGroup>` (`helloworld-1590394316693` in `helloworld-1590394316693-rg` in der Demo). Sie werden später verwendet.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/javae2e?tutorial=quickstart-java&step=config)

## <a name="deploy-the-app"></a>Bereitstellen der App

Beim Prozess der Bereitstellung in Azure App Service werden Kontoanmeldeinformation aus der Azure CLI verwendet. [Melden Sie sich über die Azure CLI an](/cli/azure/authenticate-azure-cli?view=azure-cli-latest), ehe Sie fortfahren.

```azurecli
az login
```
Dann können Sie Ihre Java-App mit dem folgenden Befehl in Azure bereitstellen:

```bash
mvn package azure-webapp:deploy
```

Nach Abschluss der Bereitstellung steht Ihre Anwendung unter `http://<appName>.azurewebsites.net/` (`http://helloworld-1590394316693.azurewebsites.net` in der Demo) bereit. Öffnen Sie die URL in Ihrem lokalen Webbrowser. Folgendes sollte angezeigt werden:

![In Azure App Service ausgeführte Beispiel-App](./media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**Glückwunsch!** Sie haben Ihre erste Java-App in App Service bereitgestellt.

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
> [Ressourcen: Azure für Java-Entwickler](/java/azure/)

> [!div class="nextstepaction"]
> [Konfigurieren der Java-App](configure-language-java.md)

> [!div class="nextstepaction"]
> [CI/CD mit Jenkins](/azure/jenkins/deploy-jenkins-app-service-plugin)

> [!div class="nextstepaction"]
> [Zuordnen einer benutzerdefinierten Domäne](app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Weitere Informationen zu Maven-Plug-Ins für Azure](https://github.com/microsoft/azure-maven-plugins)
