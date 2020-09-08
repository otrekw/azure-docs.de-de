---
title: 'Schnellstart: Bereitstellen Ihrer ersten Azure Spring Cloud-Anwendung'
description: In dieser Schnellstartanleitung stellen Sie eine Spring Cloud-Anwendung in Azure Spring Cloud bereit.
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/05/2020
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 245516e0a54865d3a6097c4bb566b850cb738ad6
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89260549"
---
# <a name="quickstart-deploy-your-first-azure-spring-cloud-application"></a>Schnellstart: Bereitstellen Ihrer ersten Azure Spring Cloud-Anwendung

In dieser Schnellstartanleitung erfahren Sie, wie Sie eine einfache Azure Spring Cloud-Microserviceanwendung für die Ausführung in Azure bereitstellen. 

Der in diesem Tutorial verwendete Anwendungscode ist eine einfache App, die mit Spring initializr erstellt wurde. Wenn Sie das Tutorial abgeschlossen haben, kann auf die Anwendung online zugegriffen werden, und sie kann über das Azure-Portal verwaltet werden.

In dieser Schnellstartanleitung wird Folgendes erläutert:

> [!div class="checklist"]
> * Generieren eines grundlegenden Spring Cloud-Projekts
> * Bereitstellen einer Dienstinstanz
> * Erstellen und Bereitstellen der App mit einem öffentlichen Endpunkt
> * Streamen von Protokollen in Echtzeit

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* [Installation von JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
* [Registrierung für ein Azure-Abonnement](https://azure.microsoft.com/free/)
* (Optional) [Installation der Azure CLI, Version 2.0.67 oder höher](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) und der Azure Spring Cloud-Erweiterung mit dem Befehl `az extension add --name spring-cloud`
* (Optional) [Installation des Azure-Toolkits für IntelliJ](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/) und [Anmeldung](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in)

## <a name="generate-a-spring-cloud-project"></a>Generieren eines Spring Cloud-Projekts

Beginnen Sie mit [Spring Initializr](https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.3.3.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=hellospring&name=hellospring&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.hellospring&dependencies=web,cloud-eureka,actuator,cloud-starter-sleuth,cloud-starter-zipkin), um ein Beispielprojekt mit empfohlenen Abhängigkeiten für Azure Spring Cloud zu generieren. Die folgende Abbildung zeigt die für dieses Beispielprojekt eingerichtete Initializr-Instanz:
```url
https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.3.3.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=hellospring&name=hellospring&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.hellospring&dependencies=web,cloud-eureka,actuator,cloud-starter-sleuth,cloud-starter-zipkin
```

  ![Initializr-Seite](media/spring-cloud-quickstart-java/initializr-page.png)

1. Klicken Sie auf **Generieren**, wenn alle Abhängigkeiten festgelegt sind. Laden Sie das Paket herunter, entpacken Sie es, und erstellen Sie dann einen Webcontroller für eine einfache Webanwendung, indem Sie `src/main/java/com/example/hellospring/HelloController.java` wie folgt hinzufügen:

    ```java
    package com.example.hellospring;
    
    import org.springframework.web.bind.annotation.RestController;
    import org.springframework.web.bind.annotation.RequestMapping;
    
    @RestController
    public class HelloController {
    
        @RequestMapping("/")
        public String index() {
            return "Greetings from Azure Spring Cloud!";
        }
    
    }
    ```
## <a name="provision-an-instance-of-azure-spring-cloud"></a>Bereitstellen einer Instanz von Azure Spring Cloud

Mit dem folgenden Verfahren wird eine Azure Spring Cloud-Instanz über das Azure-Portal erstellt:

1. Öffnen Sie das [Azure-Portal](https://ms.portal.azure.com/) in einem neuen Tab. 

2. Suchen Sie im oberen Suchfeld nach *Azure Spring Cloud*.

3. Wählen Sie in den Ergebnissen *Azure Spring Cloud* aus.

    ![ASC-Symbol: Starten](media/spring-cloud-quickstart-launch-app-portal/find-spring-cloud-start.png)

4. Klicken Sie auf der Azure Spring Cloud-Seite auf **+ Hinzufügen**.

    ![ASC-Symbol: Hinzufügen](media/spring-cloud-quickstart-launch-app-portal/spring-cloud-add.png)

5. Füllen Sie das Formular auf der Azure Spring Cloud-Seite **Erstellen** aus.  Berücksichtigen Sie die folgenden Richtlinien:
    - **Abonnement**: Wählen Sie das Abonnement aus, unter dem diese Ressource abgerechnet werden soll.
    - **Ressourcengruppe**: Die Erstellung neuer Ressourcengruppen für neue Ressourcen ist eine bewährte Methode. Sie wird später als **\<resource group name\>** verwendet.
    - **Dienstdetails/Name**: Geben Sie **\<service instance name\>** an.  Der Name muss zwischen 4 und 32 Zeichen lang sein und darf nur Kleinbuchstaben, Ziffern und Bindestriche enthalten.  Das erste Zeichen des Dienstnamens muss ein Buchstabe und das letzte Zeichen entweder ein Buchstabe oder eine Ziffer sein.
    - **Standort**: Wählen Sie die Region für Ihre Dienstinstanz aus.

    ![Startseite des ASC-Portals](media/spring-cloud-quickstart-launch-app-portal/portal-start.png)

6. Klicken Sie auf **Überprüfen und erstellen**.

## <a name="build-and-deploy-the-app"></a>Erstellen und Bereitstellen der App
    
#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)
Im folgenden Verfahren wird die Anwendung mithilfe der Azure CLI erstellt und bereitgestellt. Führen Sie im Stammverzeichnis des Projekts folgenden Befehl aus:

1. Erstellen Sie das Projekt mithilfe von Maven:

    ```console
    mvn clean package -DskipTests
    ```

1. Installieren Sie die Azure Spring Cloud-Erweiterung für die Azure CLI (sofern sie noch nicht installiert ist):

    ```azurecli
    az extension add --name spring-cloud
    ```
    
1. Erstellen Sie die App mit einem zugewiesenen öffentlichen Endpunkt:

    ```azurecli
    az spring-cloud app create -n hellospring -s <service instance name> -g <resource group name> --is-public
    ```

1. Stellen Sie die JAR-Datei für die App bereit:

    ```azurecli
    az spring-cloud app deploy -n hellospring -s <service instance name> -g <resource group name> --jar-path target\hellospring-0.0.1-SNAPSHOT.jar
    ```
    
1. Es dauert einige Minuten, bis die Bereitstellung der Anwendung abgeschlossen ist. Navigieren Sie im Azure-Portal zum Blatt **Apps**, um zu überprüfen, ob die Anwendung bereitgestellt wurde. Der Status der Anwendung sollte angezeigt werden.

#### <a name="intellij"></a>[IntelliJ](#tab/IntelliJ)

Im folgenden Verfahren wird das IntelliJ-Plug-In für Azure Spring Cloud verwendet, um die Beispiel-App in IntelliJ IDEA bereitzustellen.  

### <a name="import-project"></a>Importieren des Projekts

1. Öffnen Sie das IntelliJ-Dialogfeld **Willkommen**, und wählen Sie **Projekt importieren** aus, um den Import-Assistenten zu öffnen.
1. Wählen Sie den Ordner `hellospring` aus.

    ![Importieren des Projekts](media/spring-cloud-quickstart-java/intellij-new-project.png)

### <a name="deploy-the-app"></a>Bereitstellen der App
Für die Bereitstellung in Azure müssen Sie sich bei Ihrem Azure-Konto anmelden und Ihr Abonnement auswählen.  Anmeldeinformationen finden Sie unter [Installation und Anmeldung](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in).

1. Klicken Sie im Projektexplorer von IntelliJ mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Azure** -> **In Azure Spring Cloud bereitstellen** aus.

    [ ![Bereitstellen in Azure 1](media/spring-cloud-quickstart-java/intellij-deploy-azure-1.png) ](media/spring-cloud-quickstart-java/intellij-deploy-azure-1.png#lightbox)

1. Übernehmen Sie den Namen für die App im Feld **Name**. **Name** bezieht sich auf die Konfiguration, nicht auf den App-Namen. Benutzer müssen ihn in der Regel nicht ändern.
1. Wählen Sie im Textfeld **Artefakt** die Datei *hellospring-0.0.1-SNAPSHOT.jar* aus.
1. Überprüfen Sie im Textfeld **Abonnement** Ihr Abonnement.
1. Wählen Sie im Textfeld **Spring Cloud** die Instanz von Azure Spring Cloud aus, die Sie unter [Schnellstart: Bereitstellen des Azure Spring Cloud-Diensts](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-provision-service-instance) erstellt haben.
1. Legen Sie **Öffentlicher Endpunkt** auf *Aktivieren* fest.
1. Wählen Sie im Textfeld **App:** die Option **App erstellen...** aus.
1. Geben Sie *hellospring* ein, und klicken Sie dann auf **OK**.

    [ ![Bereitstellung in Azure: OK](media/spring-cloud-quickstart-java/intellij-deploy-to-azure.png) ](media/spring-cloud-quickstart-java/intellij-deploy-to-azure.png#lightbox)

1. Starten Sie die Bereitstellung, indem Sie unten im Dialogfeld **Deploy Azure Spring Cloud app** (Azure Spring Cloud-App bereitstellen) auf die Schaltfläche **Ausführen** klicken. Das Plug-In führt den Befehl `mvn package` für die App `hellospring` aus, und stellt die vom `package`-Befehl generierte JAR-Datei bereit.
---

Nach Abschluss der Bereitstellung können Sie unter `https://<service instance name>-hellospring.azuremicroservices.io/` auf die App zugreifen.

  [ ![Zugreifen auf die App über den Browser](media/spring-cloud-quickstart-java/access-app-browser.png) ](media/spring-cloud-quickstart-java/access-app-browser.png#lightbox)

## <a name="streaming-logs-in-real-time"></a>Streamingprotokolle in Echtzeit

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)

Verwenden Sie den folgenden Befehl, um Echtzeitprotokolle von der App zu erhalten.

```azurecli
az spring-cloud app logs -n hellospring -s <service instance name> -g <resource group name> --lines 100 -f

```
Protokolle werden in den Ergebnissen angezeigt:

[ ![Streamingprotokolle](media/spring-cloud-quickstart-java/streaming-logs.png) ](media/spring-cloud-quickstart-java/streaming-logs.png#lightbox)

>[!TIP]
> Verwenden Sie `az spring-cloud app logs -h`, um weitere Parameter und Funktionen für Protokolldatenströme zu untersuchen.

#### <a name="intellij"></a>[IntelliJ](#tab/IntelliJ)

1. Wählen Sie **Azure-Explorer** und dann **Spring Cloud** aus.
1. Klicken Sie mit der rechten Maustaste auf die ausgeführte App.
1. Wählen Sie in der Dropdownliste **Streamingprotokolle** aus.
1. Wählen Sie die Instanz aus.

    [ ![Auswählen von Streamingprotokollen](media/spring-cloud-quickstart-java/intellij-get-streaming-logs.png) ](media/spring-cloud-quickstart-java/intellij-get-streaming-logs.png)

1. Das Streamingprotokoll wird im Ausgabefenster angezeigt.

    [ ![Ausgabe des Streamingprotokolls](media/spring-cloud-quickstart-java/intellij-streaming-logs-output.png) ](media/spring-cloud-quickstart-java/intellij-streaming-logs-output.png)
---

Informationen zu erweiterten Protokollanalysefeatures finden Sie im Menü im [Azure-Portal](https://portal.azure.com/) auf der Registerkarte **Protokolle**. Bei den hier aufgeführten Protokollen kommt es zu einer Wartezeit von einigen Minuten.

[ ![Protokollanalyse](media/spring-cloud-quickstart-java/logs-analytics.png) ](media/spring-cloud-quickstart-java/logs-analytics.png#lightbox)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Ressourcengruppe erstellt. Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, löschen Sie die Ressourcengruppe im Portal, indem Sie den folgenden Befehl in Cloud Shell ausführen:
```azurecli
az group delete --name <your resource group name; for example: hellospring-1558400876966-rg> --yes
```

In diesem Schnellstart haben Sie gelernt, wie Sie:

> [!div class="checklist"]
> * Generieren eines grundlegenden Azure Spring Cloud-Projekts
> * Bereitstellen einer Dienstinstanz
> * Erstellen und Bereitstellen der App mit einem öffentlichen Endpunkt
> * Streamingprotokolle in Echtzeit
## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Erstellen und Ausführen von Microservices](spring-cloud-quickstart-sample-app-introduction.md)

Weitere Beispiele finden Sie auf GitHub: [Azure Spring Cloud-Beispiele](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
