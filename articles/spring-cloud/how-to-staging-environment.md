---
title: Einrichten einer Stagingumgebung in Azure Spring Cloud | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Blaugrün-Bereitstellung mit Azure Spring Cloud verwenden.
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 9b3a5659e91ca90d31500b10526e3e2179d4e7da
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105046112"
---
# <a name="set-up-a-staging-environment-in-azure-spring-cloud"></a>Einrichten einer Stagingumgebung in Azure Spring Cloud

**Dieser Artikel gilt für:** ✔️ Java

In diesem Artikel wird erläutert, wie Sie unter Verwendung des Blau-Grün-Bereitstellungsmusters in Azure Spring Cloud eine Stagingbereitstellung einrichten. Die Blau-Grün-Bereitstellung ist ein Azure DevOps-Muster für Continuous Delivery, bei dem eine vorhandene Version (blau) live bleibt, während eine neue Version (grün) bereitgestellt wird. In diesem Artikel wird außerdem gezeigt, wie Sie diese Stagingbereitstellung in die Produktion überführen, ohne die Produktionsbereitstellung zu ändern.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Azure Spring Cloud-Instanz im Tarif „Standard“
* Die [Azure Spring Cloud-Erweiterung](/cli/azure/azure-cli-extensions-overview) für die Azure CLI

In diesem Artikel wird eine Anwendung verwendet, die mit Spring Initializr erstellt wurde. Wenn Sie für dieses Beispiel eine andere Anwendung verwenden möchten, müssen Sie am öffentlichen Teil der Anwendung eine kleine Änderung vornehmen, um die Stagingbereitstellung von der Produktion zu unterscheiden.

>[!TIP]
> [Azure Cloud Shell](https://shell.azure.com) ist eine kostenlose interaktive Shell, mit der Sie die Anweisungen in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie Ihre Cloud Shell-Instanz. Weitere Informationen finden Sie in der [Übersicht über Azure Cloud Shell](../cloud-shell/overview.md).

Befolgen Sie die Anweisungen in den nächsten Abschnitten, um die Blau-Grün-Bereitstellungen in Azure Spring Cloud einzurichten.

## <a name="install-the-azure-cli-extension"></a>Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure CLI zu installieren:

```azurecli
az extension add --name spring-cloud
```
## <a name="prepare-the-app-and-deployments"></a>Vorbereiten der App und der Bereitstellungen
Führen Sie die folgenden Schritte aus, um die Anwendung zu erstellen:

1. Generieren Sie den Code für die Beispiel-App mithilfe von Spring Initializr mit [dieser Konfiguration](https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.3.4.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=hellospring&name=hellospring&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.hellospring&dependencies=web,cloud-eureka,actuator,cloud-starter-sleuth,cloud-starter-zipkin,cloud-config-client).

2. Laden Sie den Code herunter.
3. Fügen Sie die Quelldatei „HelloController.java“ mit dem folgenden Inhalt zum Ordner `\src\main\java\com\example\hellospring\` hinzu:

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
4. Erstellen Sie die JAR-Datei:

   ```azurecli
   mvn clean packge -DskipTests
   ```
5. Erstellen Sie die App in Ihrer Azure Spring Cloud-Instanz:

   ```azurecli
   az spring-cloud app create -n demo -g <resourceGroup> -s <Azure Spring Cloud instance> --assign-endpoint
   ```
6. Stellen Sie die App in Azure Spring Cloud bereit:

   ```azurecli
   az spring-cloud app deploy -n demo -g <resourceGroup> -s <Azure Spring Cloud instance> --jar-path target\hellospring-0.0.1-SNAPSHOT.jar
   ```
7. Ändern Sie den Code für die Stagingbereitstellung:

   ```java
   package com.example.hellospring; 
   import org.springframework.web.bind.annotation.RestController; 
   import org.springframework.web.bind.annotation.RequestMapping; 

   @RestController 

   public class HelloController { 

   @RequestMapping("/") 

     public String index() { 

         return "Greetings from Azure Spring Cloud! THIS IS THE GREEN DEPLOYMENT"; 
     } 

   } 
   ```
8. Erstellen Sie die JAR-Datei neu:

   ```azurecli
   mvn clean packge -DskipTests
   ```
9. Erstellen Sie die Grün-Bereitstellung: 

   ```azurecli
   az spring-cloud app deployment create -n green --app demo -g <resourceGroup> -s <Azure Spring Cloud instance> --jar-path target\hellospring-0.0.1-SNAPSHOT.jar 
   ```

## <a name="view-apps-and-deployments"></a>Anzeigen von Apps und Bereitstellungen

Zeigen Sie die bereitgestellten Apps mithilfe des folgenden Verfahrens an:

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Spring Cloud-Instanz.

1. Öffnen Sie im linken Bereich den Bereich **Apps**, um die Apps für Ihre Dienstinstanz anzuzeigen.

   ![Screenshot: Bereich „Apps“](media/spring-cloud-blue-green-staging/app-dashboard.png)

1. Sie können eine App auswählen und Details anzeigen.

   ![Screenshot: Details zu einer App](media/spring-cloud-blue-green-staging/app-overview.png)

1. Öffnen Sie **Bereitstellungen**, um alle Bereitstellungen der App anzuzeigen. Das Raster zeigt die Produktions- und Stagingbereitstellungen.

   ![Screenshot: Auflistung der App-Bereitstellungen](media/spring-cloud-blue-green-staging/deployments-dashboard.png)

1. Klicken Sie auf die URL, um die aktuell bereitgestellte Anwendung zu öffnen.
    
   ![Screenshot: URL für die bereitgestellte Anwendung](media/spring-cloud-blue-green-staging/running-blue-app.png)

1. Klicken Sie in der Spalte **Zustand** auf **Produktion**, um die Standard-App anzuzeigen.
    
   ![Screenshot: URL für die Standard-App](media/spring-cloud-blue-green-staging/running-default-app.png)

1. Klicken Sie in der Spalte **Zustand** auf **Staging**, um die Staging-App anzuzeigen.
    
   ![Screenshot: URL für die Staging-App](media/spring-cloud-blue-green-staging/running-staging-app.png)

>[!TIP]
> * Vergewissern Sie sich, dass der Testendpunkt mit einem Schrägstrich (/) endet, um sicherzustellen, dass die CSS-Datei richtig geladen wird.  
> * Wenn Sie zum Anzeigen der Seite Anmeldeinformationen im Browser eingeben müssen, verwenden Sie [URL-Decodierung](https://www.urldecoder.org/), um den Testendpunkt zu decodieren. Nach der URL-Decodierung wird eine URL im folgenden Format zurückgegeben: *https://\<username>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green*. Verwenden Sie dieses Format für den Zugriff auf Ihren Endpunkt.

>[!NOTE]    
> Die Konfigurationsservereinstellungen gelten sowohl für Ihr Stagingumgebung als auch für Ihre Produktionsumgebung. Wenn Sie beispielsweise für den Kontextpfad (*server.servlet.context-path*) für Ihr App-Gateway im Konfigurationspfad *somepath* festlegen, wird der Pfad zu Ihrer grünen Bereitstellung in *https://\<username>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green/somepath/...* geändert.
 
Wenn Sie jetzt die öffentliche App Gateway-Instanz aufrufen, wird die alte Seite ohne die Änderung angezeigt.

## <a name="set-the-green-deployment-as-the-production-environment"></a>Festlegen der grünen Bereitstellung als Produktionsumgebung

1. Nachdem Sie die Änderung in Ihrer Stagingumgebung überprüft haben, können Sie sie in die Produktion überführen. Wählen Sie auf der Seite **Apps** > **Bereitstellungen** die Anwendung aus, die sich derzeit in der **Produktion** befindet.

1. Klicken Sie auf die Auslassungspunkte nach dem **Registrierungsstatus** der grünen Bereitstellung, und klicken Sie dann auf **Als Produktion festlegen**. 

   ![Screenshot: Auswahl zum Festlegen des Stagingbuilds für die Produktion](media/spring-cloud-blue-green-staging/set-staging-deployment.png)

1. Stellen Sie sicher, dass die URL der App Ihre Änderungen widerspiegelt.

   ![Screenshot: URL der App, die sich nun in der Produktionsumgebung befindet.](media/spring-cloud-blue-green-staging/new-production-deployment.png)

>[!NOTE]
> Sobald die grüne Bereitstellung als Produktionsumgebung festgelegt wurde, wird die vorherige Bereitstellung zur Stagingbereitstellung.

## <a name="modify-the-staging-deployment"></a>Ändern der Stagingbereitstellung

Wenn Sie mit Ihrer Änderung nicht zufrieden sind, können Sie Ihren Anwendungscode ändern, ein neues JAR-Paket erstellen und dieses mithilfe der Azure CLI in Ihre grüne Bereitstellung hochladen:

```azurecli
az spring-cloud app deploy  -g <resource-group-name> -s <service-instance-name> -n gateway -d green --jar-path gateway.jar
```

## <a name="delete-the-staging-deployment"></a>Löschen der Stagingbereitstellung

Rufen Sie die Seite für Ihre Stagingbereitstellung auf, und klicken Sie auf **Löschen**, um Ihre Stagingbereitstellung über das Azure-Portal zu löschen.

Alternativ können Sie die Stagingbereitstellung über die Azure CLI löschen, indem Sie den folgenden Befehl ausführen:

```azurecli
az spring-cloud app deployment delete -n <staging-deployment-name> -g <resource-group-name> -s <service-instance-name> --app gateway
```

## <a name="next-steps"></a>Nächste Schritte

* [CI/CD für Azure Spring Cloud](./spring-cloud-howto-cicd.md?pivots=programming-language-java)