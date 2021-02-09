---
title: Einrichten einer Stagingumgebung in Azure Spring Cloud | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Blaugrün-Bereitstellung mit Azure Spring Cloud verwenden.
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 991a335207fc29cef7b243d7e520dd5f62ff691f
ms.sourcegitcommit: 2dd0932ba9925b6d8e3be34822cc389cade21b0d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "99226106"
---
# <a name="set-up-a-staging-environment-in-azure-spring-cloud"></a>Einrichten einer Stagingumgebung in Azure Spring Cloud

**Dieser Artikel gilt für:** ✔️ Java

In diesem Artikel wird erläutert, wie Sie unter Verwendung des Blaugrün-Bereitstellungsmusters in Azure Spring Cloud eine Stagingbereitstellung einrichten. Die Blau/Grün-Bereitstellung ist ein Azure DevOps-Muster für Continuous Delivery, bei dem eine vorhandene Version (blau) live bleibt, während eine neue Version (grün) bereitgestellt wird. In diesem Artikel wird außerdem gezeigt, wie Sie diese Stagingbereitstellung in die Produktion überführen, ohne die Produktionsbereitstellung direkt zu ändern.

## <a name="prerequisites"></a>Voraussetzungen

* Azure Spring Cloud-Instanz mit *Standard*-**Tarif**.
* Eine ausgeführte Anwendung.  Weitere Informationen finden Sie unter [Schnellstart: Bereitstellen Ihrer ersten Azure Spring Cloud-Anwendung](spring-cloud-quickstart.md).
* [ASC-Erweiterung](https://docs.microsoft.com/cli/azure/azure-cli-extensions-overview) für die Azure CLI

Wenn Sie eine andere Anwendung für dieses Beispiel verwenden möchten, müssen Sie eine kleine Änderung am öffentlichen Teil der Anwendung vornehmen.  Durch diese Änderung wird die Stagingbereitstellung von der Produktion unterschieden.

>[!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Anweisungen in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie die [Azure Cloud Shell](https://shell.azure.com).  Weitere Informationen finden Sie in der [Übersicht über Azure Cloud Shell](../cloud-shell/overview.md).

Befolgen Sie die Anweisungen in den nächsten Abschnitten, um eine Stagingumgebung in Azure Spring Cloud einzurichten.

## <a name="install-the-azure-cli-extension"></a>Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure CLI zu installieren:

```azurecli
az extension add --name spring-cloud
```
    
## <a name="view-apps-and-deployments"></a>Anzeigen von Apps und Bereitstellungen

Bereitgestellte Apps können wie folgt angezeigt werden:

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Spring Cloud-Instanz.

1. Öffnen Sie im Navigationsbereich auf der linken Seite die Option **Bereitstellungen**.

    [ ![Deployment-deprecate](media/spring-cloud-blue-green-staging/deployments.png)](media/spring-cloud-blue-green-staging/deployments.png)

1. Öffnen Sie das Blatt „Apps“, um Apps für Ihre Dienstinstanz anzuzeigen.

    [ ![Apps-dashboard](media/spring-cloud-blue-green-staging/app-dashboard.png)](media/spring-cloud-blue-green-staging/app-dashboard.png)

1. Sie können auf eine App klicken und Details anzeigen.

    [ ![Apps-overview](media/spring-cloud-blue-green-staging/app-overview.png)](media/spring-cloud-blue-green-staging/app-overview.png)

1. Öffnen Sie das Blatt **Bereitstellungen**, um alle Bereitstellungen der App anzuzeigen. Das Bereitstellungsraster gibt Aufschluss darüber, ob es sich um eine Produktions- oder um eine Stagingbereitstellung handelt.

    [ ![Bereitstellungen: Dashboard](media/spring-cloud-blue-green-staging/deployments-dashboard.png)](media/spring-cloud-blue-green-staging/deployments-dashboard.png)

1. Sie können auf den Namen der Bereitstellung klicken, um die Bereitstellungsübersicht anzuzeigen. In diesem Fall ist nur eine einzelne Bereitstellung namens *Default* (Standard) vorhanden.

    [ ![Bereitstellungen: Übersicht](media/spring-cloud-blue-green-staging/deployments-overview.png)](media/spring-cloud-blue-green-staging/deployments-overview.png)
    

## <a name="create-a-staging-deployment"></a>Erstellen einer Stagingbereitstellung

1. Nehmen Sie in Ihrer lokalen Entwicklungsumgebung eine kleine Änderung an Ihrer Anwendung vor. Auf diese Weise können Sie die beiden Bereitstellungen problemlos unterscheiden. Führen Sie den folgenden Befehl aus, um das JAR-Paket zu erstellen: 

    ```console
    mvn clean package -DskipTests
    ```

1. Erstellen Sie in der Azure CLI eine neue Bereitstellung, und geben Sie Ihr den Stagingbereitstellungsnamen „green“.

    ```azurecli
    az spring-cloud app deployment create -g <resource-group-name> -s <service-instance-name> --app <appName> -n green --jar-path gateway/target/gateway.jar
    ```

1. Navigieren Sie nach erfolgreichem Abschluss der CLI-Bereitstellung über das **Anwendungsdashboard** zur Seite der App. Dort werden alle Ihre Instanzen links auf der Registerkarte **Bereitstellungen** angezeigt.

   [ ![Bereitstellungen: Dashboard nach grüner Bereitstellung](media/spring-cloud-blue-green-staging/deployments-dashboard-2.png)](media/spring-cloud-blue-green-staging/deployments-dashboard-2.png)

  
> [!NOTE]
> Der Ermittlungsstatus lautet *OUT_OF_SERVICE*. Daher wird erst dann Datenverkehr an diese Bereitstellung weitergeleitet, wenn die Überprüfung abgeschlossen ist.

## <a name="verify-the-staging-deployment"></a>Überprüfen der Stagingbereitstellung

So überprüfen Sie, ob die grüne Stagingbereitstellung funktioniert:
1. Navigieren Sie zu **Bereitstellungen**, und klicken Sie auf die grüne (`green`) **Stagingbereitstellung**.
1. Klicken Sie auf der Seite **Übersicht** auf den **Testendpunkt**.
1. Dadurch wird der Stagingbuild mit Ihren Änderungen geöffnet.

>[!TIP]
> * Vergewissern Sie sich, dass der Testendpunkt mit einem Schrägstrich (/) endet, um sicherzustellen, dass die CSS-Datei richtig geladen wird.  
> * Wenn Sie zum Anzeigen der Seite Anmeldeinformationen im Browser eingeben müssen, verwenden Sie [URL-Decodierung](https://www.urldecoder.org/), um den Testendpunkt zu decodieren. Nach der URL-Decodierung wird eine URL im Format „https://\<username>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green“ zurückgegeben.  Verwenden Sie dieses Format für den Zugriff auf Ihren Endpunkt.

>[!NOTE]    
> Die Konfigurationsservereinstellungen gelten sowohl für die Stagingumgebung als auch für die Produktion. Beispiel: Wenn Sie als Kontextpfad (`server.servlet.context-path`) für App Gateway auf dem Konfigurationsserver *somepath* festgelegt haben, ändert sich der Pfad zu Ihrer grünen Bereitstellung in „https://\<username>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green/somepath/...“.
 
 Wenn Sie jetzt die öffentliche App Gateway-Instanz aufrufen, wird die alte Seite ohne die Änderung angezeigt.
    
## <a name="set-the-green-deployment-as-the-production-environment"></a>Festlegen der grünen Bereitstellung als Produktionsumgebung

1. Nachdem Sie die Änderung in Ihrer Stagingumgebung überprüft haben, können Sie sie in die Produktion überführen. Kehren Sie zu **Bereitstellungsverwaltung** zurück, und wählen Sie die Anwendung aus, die sich derzeit in der Produktion (`Production`) befindet.

1. Klicken Sie auf die Ellipse nach **Registrierungsstatus**, und legen Sie den Produktionsbuild auf `staging` fest.

   [ ![Bereitstellungen: Festlegen der Stagingbereitstellung](media/spring-cloud-blue-green-staging/set-staging-deployment.png)](media/spring-cloud-blue-green-staging/set-staging-deployment.png)

1. Kehren Sie zur Seite **Bereitstellungsverwaltung** zurück. Legen Sie die grüne (`green`) Bereitstellung auf `production` fest. Nach Abschluss der Einstellung sollte der Status Ihrer grünen (`green`) Bereitstellung als *Aktiv* angezeigt werden. Dies ist jetzt der aktive Produktionsbuild.

   [ ![Bereitstellungen: Ergebnis des Festlegens der Stagingbereitstellung](media/spring-cloud-blue-green-staging/set-staging-deployment-result.png)](media/spring-cloud-blue-green-staging/set-staging-deployment-result.png)

1. Unter der URL der App sollten Ihre Änderungen angezeigt werden.

>[!NOTE]
> Sobald die grüne Bereitstellung als Produktionsumgebung festgelegt wurde, wird die vorherige Bereitstellung zur Stagingbereitstellung.

## <a name="modify-the-staging-deployment"></a>Ändern der Stagingbereitstellung

Sind Sie mit Ihrer Änderung nicht zufrieden, können Sie über die Azure CLI den Anwendungscode ändern, ein neues JAR-Paket erstellen und dieses in die grüne Bereitstellung hochladen.

```azurecli
az spring-cloud app deploy  -g <resource-group-name> -s <service-instance-name> -n gateway -d green --jar-path gateway.jar
```

## <a name="delete-the-staging-deployment"></a>Löschen der Stagingbereitstellung

Löschen Sie die Stagingbereitstellung aus dem Azure-Portal, indem Sie zur Seite der Stagingbereitstellung wechseln und dort die Schaltfläche **Löschen** auswählen.

Alternativ können Sie die Stagingbereitstellung über die Azure CLI löschen, indem Sie den folgenden Befehl ausführen:

```azurecli
az spring-cloud app deployment delete -n <staging-deployment-name> -g <resource-group-name> -s <service-instance-name> --app gateway
```

## <a name="next-steps"></a>Nächste Schritte

* [CI/CD für Azure Spring Cloud](https://review.docs.microsoft.com/azure/spring-cloud/spring-cloud-howto-cicd?branch=pr-en-us-142929&pivots=programming-language-java)
