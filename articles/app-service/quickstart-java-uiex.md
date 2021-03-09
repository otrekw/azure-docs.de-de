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
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 64e4c05e9439c164329dede5d714bec160bc5ae2
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102050374"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service"></a>Schnellstart: Erstellen einer Java-App in Azure App Service

Von [Azure App Service](overview.md) wird ein hochgradig skalierbarer Webhostingdienst mit Self-Patching bereitgestellt.  In dieser Schnellstartanleitung wird gezeigt, wie Sie mit der [Azure CLI](/cli/azure/get-started-with-azure-cli) und dem [Azure-Web-App-Plug-In für Maven](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) eine JAR- oder WAR-Datei bereitstellen. 

Es gibt auch IDE-Versionen dieses Artikels. Sehen Sie sich die Schnellstarts für [Azure-Toolkit für IntelliJ](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app) oder [Azure-Toolkit für Eclipse](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app) an.

## <a name="1-prepare-your-environment"></a>1. Vorbereiten der Umgebung

Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:

+ Eine <abbr title="Das Profil, mit dem Abrechnungsinformationen zur Azure-Nutzung verwaltet werden.">Azure-Konto</abbr> mit einem aktiven <abbr title="Die grundlegende Organisationsstruktur, in der Sie Ressourcen in Azure verwalten. Diese wird in der Regel einer Einzelperson oder Abteilung innerhalb einer Organisation zugeordnet.">Abonnement</abbr>. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ Die [Azure CLI](/cli/azure/install-azure-cli)

+ [Java Developer Kit](/azure/developer/java/fundamentals/java-jdk-long-term-support), Version 8 oder 11.

+ [Apache Maven](https://maven.apache.org), Version 3.0 oder höher

Das Maven-Plug-In verwendet bei der Bereitstellung in App Services Kontoanmeldeinformation aus der Azure CLI. [Melden Sie sich über die Azure CLI an](/cli/azure/authenticate-azure-cli), ehe Sie fortfahren. Weitere Informationen finden Sie unter [Authentifizierung mit Maven-Plug-Ins](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication).

```azurecli
az login
```

<br>
<hr/>

## <a name="2-create-a-java-app"></a>2. Erstellen einer Java-App

# <a name="java-se"></a>[Java SE](#tab/javase)

Klonen Sie das Beispielprojekt [Spring Boot Getting Started](https://github.com/spring-guides/gs-spring-boot).

```bash
git clone https://github.com/spring-guides/gs-spring-boot
```

Wechseln Sie in das Verzeichnis mit dem abgeschlossenen Projekt.

```bash
cd gs-spring-boot/complete
```

# <a name="tomcat"></a>[Tomcat](#tab/tomcat)

Führen Sie den folgenden Maven-Befehl über die Eingabeaufforderung der Cloud Shell aus, um eine neue App mit dem Namen `helloworld` zu erstellen:

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp" "-Dversion=1.0-SNAPSHOT"
```

Ändern Sie anschließend Ihr Arbeitsverzeichnis in den Projektordner:

```bash
cd helloworld
```

---

<br>
<hr/>

## <a name="3-configure-the-maven-plugin"></a>3. Konfigurieren des Maven-Plug-Ins

Führen Sie den folgenden Maven-Befehl aus, um die Bereitstellung zu konfigurieren. Dieser Befehl unterstützt Sie beim Einrichten des App Service-Betriebssystems, der Java-Version und der Tomcat-Version.

```bash
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.12.0:config
```

::: zone pivot="platform-windows"

# <a name="java-se"></a>[Java SE](#tab/javase)

1. Wenn die Eingabeaufforderung für die **Abonnementoption** angezeigt wird, wählen Sie das richtige `Subscription` aus, indem Sie die Ziffer am Zeilenanfang eingeben.
1. Wenn die Eingabeaufforderung für die **Web-App-Option** angezeigt wird, übernehmen Sie die Standardoption `<create>`, indem Sie die EINGABETASTE drücken, oder wählen Sie eine vorhandene App aus.
1. Wenn die Eingabeaufforderung für die **Betriebssystemoption** angezeigt wird, wählen Sie **Windows** aus, indem Sie `3` eingeben.
1. Wenn die Eingabeaufforderung für die **Tarifoption** angezeigt wird, wählen Sie **B2** aus, in dem Sie `2` eingeben.
1. Verwenden Sie die Java-Standardversion (**Java 8**), indem Sie die EINGABETASTE drücken.
1. Drücken Sie abschließend in der letzten Eingabeaufforderung die EINGABETASTE, um die Auswahl zu bestätigen.

    Die Zusammenfassungsausgabe sieht in etwa wie der unten gezeigte Codeausschnitt aus.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    Please confirm webapp properties
    Subscription Id : ********-****-****-****-************
    AppName : spring-boot-1599007390755
    ResourceGroup : spring-boot-1599007390755-rg
    Region : westeurope
    PricingTier : Basic_B2
    OS : Windows
    Java : 1.8
    WebContainer : java 8
    Deploy to slot : false
    Confirm (Y/N)? : Y
    [INFO] Saving configuration to pom.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 41.118 s
    [INFO] Finished at: 2020-09-01T17:43:45-07:00
    [INFO] ------------------------------------------------------------------------
    </pre>

# <a name="tomcat"></a>[Tomcat](#tab/tomcat)

1. Wenn die Eingabeaufforderung für die **Abonnementoption** angezeigt wird, wählen Sie das richtige `Subscription` aus, indem Sie die Ziffer am Zeilenanfang eingeben.
1. Wenn die Eingabeaufforderung für die **Web-App-Option** angezeigt wird, übernehmen Sie die Standardoption `<create>`, indem Sie die EINGABETASTE drücken, oder wählen Sie eine vorhandene App aus.
1. Wenn die Eingabeaufforderung für die **Betriebssystemoption** angezeigt wird, wählen Sie **Windows** aus, indem Sie `3` eingeben.
1. Wenn die Eingabeaufforderung für die **Tarifoption** angezeigt wird, wählen Sie **B2** aus, in dem Sie `2` eingeben.
1. Verwenden Sie die Java-Standardversion (**Java 8**), indem Sie die EINGABETASTE drücken.
1. Verwenden Sie den Standardwebcontainer (**Tomcat 8.5**), indem Sie die EINGABETASTE drücken.
1. Drücken Sie abschließend in der letzten Eingabeaufforderung die EINGABETASTE, um die Auswahl zu bestätigen.

    Die Zusammenfassungsausgabe sieht in etwa wie der unten gezeigte Codeausschnitt aus.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    Please confirm webapp properties
    Subscription Id : ********-****-****-****-************
    AppName : helloworld-1599003152123
    ResourceGroup : helloworld-1599003152123-rg
    Region : westeurope
    PricingTier : Basic_B2
    OS : Windows
    Java : 1.8
    WebContainer : tomcat 8.5
    Deploy to slot : false
    Confirm (Y/N)? : Y
    [INFO] Saving configuration to pom.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 03:03 min
    [INFO] Finished at: 2020-09-01T16:35:30-07:00
    [INFO] ------------------------------------------------------------------------
    </pre>

---

::: zone-end
::: zone pivot="platform-linux"

### <a name="java-se"></a>[Java SE](#tab/javase)

1. Wenn die Eingabeaufforderung für die **Abonnementoption** angezeigt wird, wählen Sie das richtige `Subscription` aus, indem Sie die Ziffer am Zeilenanfang eingeben.
1. Wenn die Eingabeaufforderung für die **Web-App-Option** angezeigt wird, übernehmen Sie die Standardoption `<create>`, indem Sie die EINGABETASTE drücken, oder wählen Sie eine vorhandene App aus.
1. Wenn die Eingabeaufforderung für die **Betriebssystemoption** angezeigt wird, wählen Sie **Linux** aus, indem Sie die EINGABETASTE drücken.
1. Wenn die Eingabeaufforderung für die **Tarifoption** angezeigt wird, wählen Sie **B2** aus, in dem Sie `2` eingeben.
1. Verwenden Sie die Java-Standardversion (**Java 8**), indem Sie die EINGABETASTE drücken.
1. Drücken Sie abschließend in der letzten Eingabeaufforderung die EINGABETASTE, um die Auswahl zu bestätigen.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    Please confirm webapp properties
    Subscription Id : ********-****-****-****-************
    AppName : spring-boot-1599007116351
    ResourceGroup : spring-boot-1599007116351-rg
    Region : westeurope
    PricingTier : Basic_B2
    OS : Linux
    RuntimeStack : JAVA 8-jre8
    Deploy to slot : false
    Confirm (Y/N)? : Y
    [INFO] Saving configuration to pom.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 20.925 s
    [INFO] Finished at: 2020-09-01T17:38:51-07:00
    [INFO] ------------------------------------------------------------------------
    </pre>

### <a name="tomcat"></a>[Tomcat](#tab/tomcat)

1. Wenn die Eingabeaufforderung für die **Abonnementoption** angezeigt wird, wählen Sie das richtige `Subscription` aus, indem Sie die Ziffer am Zeilenanfang eingeben.
1. Wenn die Eingabeaufforderung für die **Web-App-Option** angezeigt wird, übernehmen Sie die Standardoption `<create>`, indem Sie die EINGABETASTE drücken, oder wählen Sie eine vorhandene App aus.
1. Wenn die Eingabeaufforderung für die **Betriebssystemoption** angezeigt wird, wählen Sie **Linux** aus, indem Sie die EINGABETASTE drücken.
1. Wenn die Eingabeaufforderung für die **Tarifoption** angezeigt wird, wählen Sie **B2** aus, in dem Sie `2` eingeben.
1. Verwenden Sie die Java-Standardversion (**Java 8**), indem Sie die EINGABETASTE drücken.
1. Verwenden Sie den Standardwebcontainer (**Tomcat 8.5**), indem Sie die EINGABETASTE drücken.
1. Drücken Sie abschließend in der letzten Eingabeaufforderung die EINGABETASTE, um die Auswahl zu bestätigen.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    Please confirm webapp properties
    Subscription Id : ********-****-****-****-************
    AppName : helloworld-1599003744223
    ResourceGroup : helloworld-1599003744223-rg
    Region : westeurope
    PricingTier : Basic_B2
    OS : Linux
    RuntimeStack : TOMCAT 8.5-jre8
    Deploy to slot : false
    Confirm (Y/N)? : Y
    [INFO] Saving configuration to pom.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 50.785 s
    [INFO] Finished at: 2020-09-01T16:43:09-07:00
    [INFO] ------------------------------------------------------------------------
    </pre>

---

::: zone-end

Sie können bei Bedarf die Konfigurationen für App Service direkt in der Datei `pom.xml` ändern. Im Folgenden sind einige gängige Konfigurationen aufgeführt:

Eigenschaft | Erforderlich | BESCHREIBUNG | Version
---|---|---|---
`<schemaVersion>` | false | Gibt die Version des Konfigurationsschemas an. Folgende Werte werden unterstützt: `v1` und `v2`. | 1.5.2
`<subscriptionId>` | false | Geben Sie die Abonnement-ID an. | 0.1.0+
`<resourceGroup>` | true | Azure <abbr title="Ein logischer Container für verwandte Azure-Ressourcen, die Sie als Einheit verwalten können.">Ressourcengruppe</abbr> für Ihre Web-App. | 0.1.0+
`<appName>` | true | Der Name Ihrer Web-App. | 0.1.0+
`<region>` | true | Gibt die Region an, in der Ihre Web-App gehostet wird. Der Standardwert ist **westeurope**. Alle gültigen Regionen finden Sie im Abschnitt [Unterstützten Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=app-service). | 0.1.0+
`<pricingTier>` | false | Der Tarif für Ihre Web-App. Für Produktionsworkloads lautet der Standardwert **P1V2**, für Entwicklungs- und Testworkloads in Java ist der empfohlene Mindestwert hingegen **B2**. [Weitere Informationen](https://azure.microsoft.com/pricing/details/app-service/linux/)| 0.1.0+
`<runtime>` | true | Details zur Konfiguration der Laufzeitumgebung finden Sie [hier](https://github.com/microsoft/azure-maven-plugins/wiki/Azure-Web-App:-Configuration-Details). | 0.1.0+
`<deployment>` | true | Details zur Konfiguration der Bereitstellung finden Sie [hier](https://github.com/microsoft/azure-maven-plugins/wiki/Azure-Web-App:-Configuration-Details). | 0.1.0+

Notieren Sie sich die Werte `<appName>` und `<resourceGroup>` (`helloworld-1590394316693` in `helloworld-1590394316693-rg` in der Beispielausgabe). Sie werden später verwendet.

[Problem melden](https://www.research.net/r/javae2e?tutorial=quickstart-java&step=config)

<br>
<hr/>

## <a name="4-deploy-the-app"></a>4. Bereitstellen der App

Stellen Sie Ihre Java-App mit dem folgenden Befehl in Azure bereit.

```bash
mvn package azure-webapp:deploy
```

Nach Abschluss der Bereitstellung steht Ihre Anwendung unter `http://<appName>.azurewebsites.net/` bereit. Öffnen Sie die URL in Ihrem lokalen Webbrowser. Folgendes sollte angezeigt werden:

![In Azure App Service ausgeführte Beispiel-App](./media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**Glückwunsch!** Sie haben Ihre erste Java-App in App Service bereitgestellt.

[Problem melden](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=deploy)

<br>
<hr/>

## <a name="5-clean-up-resources"></a>5. Bereinigen von Ressourcen

Löschen Sie die Java-App und die zugehörigen Ressourcen, um weitere Kosten zu vermeiden.

```azurecli
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

Die Ausführung dieses Befehls kann eine Minute in Anspruch nehmen.

<br>
<hr/>

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Verwenden von Java zum Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Einzelserver und zum Abfragen von Daten](../postgresql/connect-java.md)

> [!div class="nextstepaction"]
> [Richten Sie CI/CD ein.](deploy-continuous-deployment.md)

> [!div class="nextstepaction"]
> [Preisinformationen](https://azure.microsoft.com/pricing/details/app-service/linux/)

> [!div class="nextstepaction"]
> [Aggregieren von Protokollen und Metriken](troubleshoot-diagnostic-logs.md)

> [!div class="nextstepaction"]
> [Hochskalieren](manage-scale-up.md)

> [!div class="nextstepaction"]
> [Ressourcen: Azure für Java-Entwickler](/java/azure/)

> [!div class="nextstepaction"]
> [Konfigurieren der Java-App](configure-language-java.md)
