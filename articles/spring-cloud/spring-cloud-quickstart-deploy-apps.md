---
title: 'Schnellstart: Erstellen und Bereitstellen von Apps in Azure Spring Cloud'
description: Hier wird die App-Bereitstellung in Azure Spring Cloud beschrieben.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/03/2020
ms.custom: devx-track-java, devx-track-azurecli
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: 727354593488cd5af77408eaa7c8d9e747a0b071
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102210307"
---
# <a name="quickstart-build-and-deploy-apps-to-azure-spring-cloud"></a>Schnellstart: Erstellen und Bereitstellen von Apps in Azure Spring Cloud

::: zone pivot="programming-language-csharp"
In dieser Schnellstartanleitung erfahren Sie, wie Sie Microserviceanwendungen mithilfe der Azure CLI erstellen und in Azure Spring Cloud bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

* Absolvieren Sie die vorherigen Schnellstarts in dieser Serie:

  * [Bereitstellen des Azure Spring Cloud-Diensts](spring-cloud-quickstart-provision-service-instance.md)
  * [Einrichten des Azure Spring Cloud-Konfigurationsservers](spring-cloud-quickstart-setup-config-server.md)

## <a name="download-the-sample-app"></a>Herunterladen der Beispiel-App

Wenn Sie bis jetzt Azure Cloud Shell verwendet haben, wechseln Sie zu einer lokalen Eingabeaufforderung, um die folgenden Schritte auszuführen:

1. Erstellen Sie einen neuen Ordner, und klonen Sie das Beispiel-App-Repository.

   ```console
   mkdir source-code
   ```

   ```console
   cd source-code
   ```

   ```console
   git clone https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples
   ```

1. Navigieren Sie zum Repositoryverzeichnis.

   ```console
   cd Azure-Spring-Cloud-Samples
   ```

## <a name="deploy-planetweatherprovider"></a>Bereitstellen von PlanetWeatherProvider

1. Erstellen Sie eine App für das Projekt „PlanetWeatherProvider“ in Ihrer Azure Spring Cloud-Instanz.

   ```azurecli
   az spring-cloud app create --name planet-weather-provider --runtime-version NetCore_31
   ```

   Wenn Sie die automatische Dienstregistrierung aktivieren möchten, müssen Sie der App den gleichen Namen geben, der auch in der Datei *appsettings.json* des Projekts als Wert von `spring.application.name` angegeben ist:

   ```json
   "spring": {
     "application": {
       "name": "planet-weather-provider"
     }
   }
   ```

   Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen.

1. Wechseln Sie in das Verzeichnis des Projektordners `PlanetWeatherProvider`.

   ```console
   cd steeltoe-sample/src/planet-weather-provider
   ```

1. Erstellen Sie die Binärdateien und die *ZIP*-Datei, die bereitgestellt werden sollen.

   ```console
   dotnet publish -c release -o ./publish
   ```

   > [!TIP]
   > Die Projektdatei enthält den folgenden XML-Code zum Verpacken der Binärdateien in einer *ZIP*-Datei, nachdem diese in den Ordner *./publish* geschrieben wurden:
   >
   > ```xml
   > <Target Name="Publish-Zip" AfterTargets="Publish">
   >   <ZipDirectory SourceDirectory="$(PublishDir)" DestinationFile="$(MSBuildProjectDirectory)/publish-deploy-planet.zip" Overwrite="true" />
   > </Target>
   > ```

1. Nehmen Sie die Bereitstellung in Azure vor.

   Stellen Sie sicher, dass sich die Eingabeaufforderung im Projektordner befindet, bevor Sie den folgenden Befehl ausführen.

   ```console
   az spring-cloud app deploy -n planet-weather-provider --runtime-version NetCore_31 --main-entry Microsoft.Azure.SpringCloud.Sample.PlanetWeatherProvider.dll --artifact-path ./publish-deploy-planet.zip
   ```

   Die Option `--main-entry` gibt den relativen Pfad vom Stammordner der *ZIP*-Datei zur  *DLL*-Datei an, die den Einstiegspunkt der Anwendung enthält. Nachdem der Dienst die *ZIP*-Datei hochgeladen hat, werden alle Dateien und Ordner extrahiert, und es wird versucht, den Einstiegspunkt in der angegebenen *DLL*-Datei auszuführen.

   Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen.

## <a name="deploy-solarsystemweather"></a>Bereitstellen von SolarSystemWeather

1. Erstellen Sie in Ihrer Azure Spring Cloud-Instanz eine weitere App, dieses Mal für das Projekt „SolarSystemWeather“:

   ```azurecli
   az spring-cloud app create --name solar-system-weather --runtime-version NetCore_31
   ```

   `solar-system-weather` ist der Name, der in der Datei *appsettings.json* des Projekts `SolarSystemWeather` angegeben ist.

   Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen.

1. Wechseln Sie in das Verzeichnis des Projekts `SolarSystemWeather`.

   ```console
   cd ../solar-system-weather
   ```

1. Erstellen Sie die Binärdateien und die *ZIP*-Datei, die bereitgestellt werden sollen.

   ```console
   dotnet publish -c release -o ./publish
   ```

1. Nehmen Sie die Bereitstellung in Azure vor.

   ```console
   az spring-cloud app deploy -n solar-system-weather --runtime-version NetCore_31 --main-entry Microsoft.Azure.SpringCloud.Sample.SolarSystemWeather.dll --artifact-path ./publish-deploy-solar.zip
   ```
   
   Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen.

## <a name="assign-public-endpoint"></a>Zuweisen eines öffentlichen Endpunkts

Senden Sie zum Testen der Anwendung im Browser eine HTTP GET-Anforderung an die Anwendung `solar-system-weather`.  Dazu benötigen Sie einen öffentlichen Endpunkt für die Anforderung.

1. Führen Sie zum Zuweisen des Endpunkts den folgenden Befehl aus:

   ```azurecli
   az spring-cloud app update -n solar-system-weather --assign-endpoint true
   ```

1. Führen Sie zum Abrufen der URL des Endpunkts den folgenden Befehl aus:

   Windows:

   ```azurecli
   az spring-cloud app show -n solar-system-weather -o table
   ```

   Linux:

   ```azurecli
   az spring-cloud app show --name solar-system-weather | grep url
   ```

## <a name="test-the-application"></a>Testen der Anwendung

Senden Sie eine GET-Anforderung an die App `solar-system-weather`. Navigieren Sie in einem Browser zu der öffentlichen URL, an die Sie `/weatherforecast` angefügt haben. Beispiel:

```
https://servicename-solar-system-weather.azuremicroservices.io/weatherforecast
```

Die Ausgabe liegt im JSON-Format vor:

```json
[{"Key":"Mercury","Value":"very warm"},{"Key":"Venus","Value":"quite unpleasant"},{"Key":"Mars","Value":"very cool"},{"Key":"Saturn","Value":"a little bit sandy"}]
```

Diese Antwort zeigt, dass beide Microservice-Apps funktionieren. Die App `SolarSystemWeather` gibt Daten zurück, die sie aus der App `PlanetWeatherProvider` abgerufen hat.
::: zone-end

::: zone pivot="programming-language-java"
In diesem Dokument wird erläutert, wie Sie Microserviceanwendungen mithilfe der folgenden Komponenten erstellen und in Azure Spring Cloud bereitstellen:
* Azure CLI
* Maven-Plug-In
* IntelliJ

Führen Sie vor der Bereitstellung mithilfe der Azure CLI oder von Maven die Beispiele aus, die [eine Instanz von Azure Spring Cloud bereitstellen](spring-cloud-quickstart-provision-service-instance.md) und [den Konfigurationsserver einrichten](spring-cloud-quickstart-setup-config-server.md).

## <a name="prerequisites"></a>Voraussetzungen

* [Installation von JDK 8](/java/azure/jdk/)
* [Registrierung für ein Azure-Abonnement](https://azure.microsoft.com/free/)
* (Optional) [Installation der Azure CLI, Version 2.0.67 oder höher](/cli/azure/install-azure-cli) und Installation der Azure Spring Cloud-Erweiterung mit dem Befehl `az extension add --name spring-cloud`
* (Optional) [Installation des Azure-Toolkits für IntelliJ](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/) und [Anmeldung](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in)

## <a name="deployment-procedures"></a>Bereitstellungsverfahren

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)

### <a name="build-the-microservices-applications-locally"></a>Lokales Erstellen der Microserviceanwendungen

1. Klonen Sie das Beispiel-App-Repository in Ihrem Azure Cloud-Konto.  

    ```azurecli
    git clone https://github.com/Azure-Samples/piggymetrics
    ```

2. Ändern Sie das Verzeichnis, und erstellen Sie das Projekt.

    ```azurecli
    cd piggymetrics
    mvn clean package -DskipTests
    ```

Die Kompilierung des Projekts dauert etwa fünf Minuten. Nach Abschluss des Vorgangs sollten die entsprechenden Ordner einzelne JAR-Dateien für jeden Dienst enthalten.

### <a name="create-and-deploy-the-apps"></a>Erstellen und Bereitstellen der Apps

1. Legen Sie den Standardnamen für die Ressourcengruppe und den Clusternamen mithilfe der folgenden Befehle fest:

    ```azurecli
    az configure --defaults group=<resource group name>
    az configure --defaults spring-cloud=<service instance name>
    ```

1. Erstellen Sie mithilfe der im vorherigen Schritt erstellten JAR-Dateien Azure Spring Cloud-Microservices. Sie erstellen drei Apps: **gateway**, **auth-service** und **account-service**.

    ```azurecli
    az spring-cloud app create --name gateway
    az spring-cloud app create --name auth-service
    az spring-cloud app create --name account-service
    ```

1. Die im vorherigen Schritt erstellten Anwendungen müssen in Azure bereitgestellt werden. Verwenden Sie die folgenden Befehle, um alle drei Anwendungen bereitzustellen:

    ```azurecli
    az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
    az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
    az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
    ```

### <a name="assign-public-endpoint-to-gateway"></a>Zuweisen eines öffentlichen Endpunkts zum Gateway

Wir benötigen eine Methode, um über einen Webbrowser auf die Anwendung zugreifen zu können. Die Gatewayanwendung benötigt einen öffentlichen Endpunkt.

1. Weisen Sie den Endpunkt mit dem folgenden Befehl zu:

    ```azurecli
    az spring-cloud app update -n gateway --assign-endpoint true
    ```

2. Fragen Sie die öffentliche IP-Adresse der Anwendung **gateway** ab, damit Sie überprüfen können, ob die Anwendung ausgeführt wird:

    ```azurecli
    az spring-cloud app show --name gateway --query properties.url
    ```

#### <a name="maven"></a>[Maven](#tab/Maven)

### <a name="clone-and-build-the-sample-application-repository"></a>Klonen und Erstellen des Beispielanwendungsrepositorys

1. Führen Sie den folgenden Befehl aus, um das Git-Repository zu klonen:

    ```
    git clone https://github.com/Azure-Samples/piggymetrics
    ```
  
1. Führen Sie den folgenden Befehl aus, um das Verzeichnis zu ändern und das Projekt zu erstellen:

    ```
    cd piggymetrics
    mvn clean package -DskipTests
    ```

### <a name="generate-configurations-and-deploy-to-the-azure-spring-cloud"></a>Generieren von Konfigurationen und Bereitstellen in Azure Spring Cloud

1. Generieren Sie Konfigurationen, indem Sie den folgenden Befehl im Stammverzeichnis von PiggyMetrics mit dem übergeordneten POM ausführen. Wenn Sie sich bereits bei der Azure CLI angemeldet haben, werden die Anmeldeinformationen automatisch vom Befehl abgerufen. Andernfalls werden Sie mit Anweisungen zur Eingabeaufforderung angemeldet. Weitere Informationen finden Sie auf unserer [Wiki-Seite](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication).

    ```
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:1.3.0:config
    ```
    
    Sie werden zur Auswahl der folgenden Angaben aufgefordert:
    * **Modules:** Wählen Sie `gateway`, `auth-service` und `account-service` aus.
    * **Abonnement:** Dies ist das Abonnement, das zum Erstellen einer Azure Spring Cloud-Instanz verwendet wird.
    * **Dienstinstanz:** Dies ist der Name Ihrer Azure Spring Cloud-Instanz.
    * **Öffentlicher Endpunkt:** Geben Sie in der Liste der angegebenen Projekte die Zahl für `gateway` ein.  Dadurch erhalten Sie öffentlichen Zugriff.

1. Das POM enthält jetzt die Plug-In-Abhängigkeiten und -Konfigurationen. Stellen Sie die Apps mit dem folgenden Befehl bereit: 

    ```
    mvn azure-spring-cloud:deploy
    ```

#### <a name="intellij"></a>[IntelliJ](#tab/IntelliJ)

### <a name="import-sample-project-in-intellij"></a>Importieren eines Beispielprojekts in IntelliJ

1. Laden Sie das Quellrepository für dieses Tutorial herunter, und entpacken Sie es, oder klonen Sie es mithilfe von Git: `git clone https://github.com/Azure-Samples/piggymetrics`. 

1. Öffnen Sie das IntelliJ-Dialogfeld **Willkommens**, und wählen Sie **Projekt importieren** aus, um den Importassistenten zu öffnen.

1. Wählen Sie den Ordner `piggymetric` aus.

    ![Importieren des Projekts](media/spring-cloud-intellij-howto/revision-import-project-1.png)

### <a name="deploy-gateway-app-to-azure-spring-cloud"></a>Bereitstellen der Gateway-App in Azure Spring Cloud
Für die Bereitstellung in Azure müssen Sie sich mit Ihrem Azure-Konto beim Azure-Toolkit für IntelliJ anmelden und Ihr Abonnement auswählen. Anmeldeinformationen finden Sie unter [Installation und Anmeldung](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in).

1. Klicken Sie im Projektexplorer von IntelliJ mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Azure** -> **In Azure Spring Cloud bereitstellen** aus.

    ![Bereitstellen in Azure 1](media/spring-cloud-intellij-howto/revision-deploy-to-azure-1.png)

1. Fügen Sie im Feld **Name** die Zeichenfolge *:gateway* an den vorhandenen **Namen** an.
1. Wählen Sie im Textfeld **Artefakt** die Option *com.piggymetrics:gateway:1.0-SNAPSHOT* aus.
1. Überprüfen Sie im Textfeld **Abonnement** Ihr Abonnement.
1. Wählen Sie im Textfeld **Spring Cloud** die Instanz von Azure Spring Cloud aus, die Sie unter [Schnellstart: Bereitstellen des Azure Spring Cloud-Diensts](./spring-cloud-quickstart-provision-service-instance.md) erstellt haben.
1. Legen Sie **Öffentlicher Endpunkt** auf *Aktivieren* fest.
1. Wählen Sie im Textfeld **App:** die Option **App erstellen...** aus.
1. Geben Sie *gateway* ein, und klicken Sie dann auf **OK**.

    ![In Azure bereitstellen OK](media/spring-cloud-intellij-howto/revision-deploy-to-azure-2.png)

1. Doppelklicken Sie im Abschnitt **Before launch** (Vor dem Start) des Dialogfelds auf *Run Maven Goal* (Maven-Ziel ausführen).
1. Navigieren Sie im Textfeld **Arbeitsverzeichnis** zum Ordner *piggymetrics/gateway*.
1. Geben Sie im Textfeld **Befehlszeile** die Zeichenfolge *package -DskipTests* ein. Klicken Sie auf **OK**.
1. Starten Sie die Bereitstellung, indem Sie unten im Dialogfeld **Deploy Azure Spring Cloud app** (Azure Spring Cloud-App bereitstellen) auf die Schaltfläche **Ausführen** klicken. Das Plug-In führt den Befehl `mvn package` für die App `gateway` aus, und stellt die vom `package`-Befehl generierte JAR-Datei bereit.

### <a name="deploy-auth-service-and-account-service-apps-to-azure-spring-cloud"></a>Bereitstellen der Apps „auth-service“ und „account-service“ in Azure Spring Cloud
Sie können die oben beschriebenen Schritte wiederholen, um die Apps `auth-service` und `account-service` in Azure Spring Cloud bereitzustellen:

1. Ändern Sie die Werte für **Name** und **Artefakt** zur Identifizierung der App `auth-service`.
1. Wählen Sie im Textfeld **App:** die Option **App erstellen...** aus, um die App `auth-service` zu erstellen.
1. Vergewissern Sie sich, dass die Option **Öffentlicher Endpunkt** auf *Deaktiviert* festgelegt ist.
1. Stellen Sie im Abschnitt **Before launch** (Vor dem Start) des Dialogfelds das **Arbeitsverzeichnis** auf den Ordner *piggymetrics/auth-service* um.
1. Starten Sie die Bereitstellung, indem Sie unten im Dialogfeld **Deploy Azure Spring Cloud app** (Azure Spring Cloud-App bereitstellen) auf die Schaltfläche **Ausführen** klicken. 
1. Wiederholen Sie diese Schritte, um `account-service` zu konfigurieren und bereitzustellen.
---

Navigieren Sie zu der URL, die in der Ausgabe der vorherigen Schritte zurückgegeben wurde, um auf die PiggyMetrics-Anwendung zuzugreifen. Beispiel: `https://<service instance name>-gateway.azuremicroservices.io`

![Zugreifen auf PiggyMetrics](media/spring-cloud-quickstart-launch-app-cli/launch-app.png)

Sie können auch im Azure-Portal navigieren, um die URL zu suchen. 
1. Navigieren Sie zum Dienst.
2. Wählen Sie **Apps** aus.
3. Wählen Sie **Gateway** aus.

    ![Navigieren zur App](media/spring-cloud-quickstart-launch-app-cli/navigate-app1.png)
    
4. Suchen Sie die URL auf der Seite **Gateway | Übersicht**.

    ![Navigieren zur App 2](media/spring-cloud-quickstart-launch-app-cli/navigate-app2-url.png)

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Azure-Ressourcen erstellt, für die weiterhin Gebühren anfallen, falls sie in Ihrem Abonnement verbleiben. Wenn Sie nicht mit der nächsten Schnellstartanleitung fortfahren möchten, lesen Sie die Informationen unter [Bereinigen von Ressourcen](spring-cloud-quickstart-logs-metrics-tracing.md#clean-up-resources). Fahren Sie andernfalls mit der nächsten Schnellstartanleitung fort:

> [!div class="nextstepaction"]
> [Protokolle, Metriken und Ablaufverfolgung](spring-cloud-quickstart-logs-metrics-tracing.md)
