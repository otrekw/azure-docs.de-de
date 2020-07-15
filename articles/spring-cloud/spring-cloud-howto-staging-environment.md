---
title: Einrichten einer Stagingumgebung in Azure Spring Cloud | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Blaugrün-Bereitstellung mit Azure Spring Cloud verwenden.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: brendm
ms.openlocfilehash: 2e29f6a75b303518ac34ecf9b570bd7638cf0c3a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "79471029"
---
# <a name="set-up-a-staging-environment-in-azure-spring-cloud"></a>Einrichten einer Stagingumgebung in Azure Spring Cloud

In diesem Artikel wird erläutert, wie Sie unter Verwendung des Blaugrün-Bereitstellungsmusters in Azure Spring Cloud eine Stagingbereitstellung einrichten. Die Blau/Grün-Bereitstellung ist ein Azure DevOps-Muster für Continuous Delivery, bei dem eine vorhandene Version (blau) live bleibt, während eine neue Version (grün) bereitgestellt wird. In diesem Artikel wird außerdem gezeigt, wie Sie diese Stagingbereitstellung in die Produktion überführen, ohne die Produktionsbereitstellung direkt zu ändern.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie die PiggyMetrics-Anwendung aus dem [Tutorial zum Starten einer Azure Spring Cloud-Anwendung](spring-cloud-quickstart-launch-app-portal.md) bereits bereitgestellt haben. PiggyMetrics umfasst drei Anwendungen: „gateway“, „account-service“ und „auth-service“.  

Wenn Sie eine andere Anwendung für dieses Beispiel verwenden möchten, müssen Sie eine kleine Änderung am öffentlichen Teil der Anwendung vornehmen.  Durch diese Änderung wird die Stagingbereitstellung von der Produktion unterschieden.

>[!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Anweisungen in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie die [Azure Cloud Shell](https://shell.azure.com).  Weitere Informationen finden Sie in der [Übersicht über Azure Cloud Shell](../cloud-shell/overview.md).

Befolgen Sie die Anweisungen in den nächsten Abschnitten, um eine Stagingumgebung in Azure Spring Cloud einzurichten.

## <a name="install-the-azure-cli-extension"></a>Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure CLI zu installieren:

```azurecli
az extension add --name spring-cloud
```
    
## <a name="view-all-deployments"></a>Anzeigen aller Bereitstellungen

Navigieren Sie im Azure-Portal zu Ihrer Dienstinstanz, und wählen Sie **Bereitstellungsverwaltung** aus, um alle Bereitstellungen anzuzeigen. Wählen Sie bei Bedarf die einzelnen Bereitstellungen aus, um weitere Details anzuzeigen.

## <a name="create-a-staging-deployment"></a>Erstellen einer Stagingbereitstellung

1. Nehmen Sie in Ihrer lokalen Entwicklungsumgebung eine kleine Änderung an der PiggyMetrics-Gatewayanwendung vor. Ändern Sie beispielsweise die Farbe in der Datei *gateway/src/main/resources/static/css/launch.css*. Auf diese Weise können Sie die beiden Bereitstellungen problemlos unterscheiden. Führen Sie den folgenden Befehl aus, um das JAR-Paket zu erstellen: 

    ```console
    mvn clean package
    ```

1. Erstellen Sie in der Azure CLI eine neue Bereitstellung, und geben Sie Ihr den Stagingbereitstellungsnamen „green“.

    ```azurecli
    az spring-cloud app deployment create -g <resource-group-name> -s <service-instance-name> --app gateway -n green --jar-path gateway/target/gateway.jar
    ```

1. Wenn die Bereitstellung erfolgreich abgeschlossen wurde, navigieren Sie über das **Anwendungsdashboard** zur Gatewayseite. Alle Ihre Instanzen werden links auf der Registerkarte **App Instances** (App-Instanzen) angezeigt.
  
> [!NOTE]
> Der Ermittlungsstatus lautet *OUT_OF_SERVICE*. Daher wird erst dann Datenverkehr an diese Bereitstellung weitergeleitet, wenn die Überprüfung abgeschlossen ist.

## <a name="verify-the-staging-deployment"></a>Überprüfen der Stagingbereitstellung

1. Kehren Sie zur Seite **Bereitstellungsverwaltung** zurück, und wählen Sie Ihre neue Bereitstellung aus. Der Bereitstellungsstatus sollte *Wird ausgeführt* lauten. Die Schaltfläche **Assign/Unassign domain** (Domäne zuweisen/Zuweisung für Domäne aufheben) wird grau unterlegt dargestellt, da es sich bei der Umgebung um eine Stagingumgebung handelt.

1. Im Abschnitt **Übersicht** sollte ein **Testendpunkt** angezeigt werden. Kopieren Sie ihn, und fügen Sie ihn in einem neuen Browserfenster ein. Die neue PiggyMetrics-Seite wird angezeigt.

>[!TIP]
> * Vergewissern Sie sich, dass der Testendpunkt mit einem Schrägstrich (/) endet, um sicherzustellen, dass die CSS-Datei richtig geladen wird.  
> * Wenn Sie zum Anzeigen der Seite Anmeldeinformationen im Browser eingeben müssen, verwenden Sie [URL-Decodierung](https://www.urldecoder.org/), um den Testendpunkt zu decodieren. Nach der URL-Decodierung wird eine URL im Format „https://\<username>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green“ zurückgegeben.  Verwenden Sie dieses Format für den Zugriff auf Ihren Endpunkt.

>[!NOTE]    
> Die Konfigurationsservereinstellungen gelten sowohl für die Stagingumgebung als auch für die Produktion. Beispiel: Wenn Sie als Kontextpfad (`server.servlet.context-path`) für App Gateway auf dem Konfigurationsserver *somepath* festgelegt haben, ändert sich der Pfad zu Ihrer grünen Bereitstellung in „https://\<username>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green/somepath/...“.
 
 Wenn Sie jetzt die öffentliche App Gateway-Instanz aufrufen, wird die alte Seite ohne die Änderung angezeigt.
    
## <a name="set-the-green-deployment-as-the-production-environment"></a>Festlegen der grünen Bereitstellung als Produktionsumgebung

1. Nachdem Sie die Änderung in Ihrer Stagingumgebung überprüft haben, können Sie sie in die Produktion überführen. Kehren Sie zur Seite **Bereitstellungsverwaltung** zurück, und aktivieren Sie das Kontrollkästchen für die Anwendung **gateway**.

2. Wählen Sie **Set deployment** (Bereitstellung festlegen) aus.
3. Wählen Sie in der Liste **Produktionsbereitstellung** die Option **Grün** und anschließend **Anwenden** aus.
4. Navigieren Sie zur Seite **Übersicht** der Gatewayanwendung. Wenn Sie bereits eine Domäne für Ihre Gatewayanwendung zugewiesen haben, wird die URL auf der Seite **Übersicht** angezeigt. Wählen Sie URL aus, und navigieren Sie zur Website, um die geänderte PiggyMetrics-Seite anzuzeigen.

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
