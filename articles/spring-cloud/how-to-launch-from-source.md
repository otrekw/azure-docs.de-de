---
title: 'Vorgehensweise: Starten Ihrer Spring Cloud-Anwendung aus dem Quellcode'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Ihre Azure Spring Cloud-Anwendung direkt aus dem Quellcode starten.
author: karlerickson
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 09/03/2020
ms.author: karler
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: a4bfa5687c4b552a99b8d4e5a7e5c8f79807d766
ms.sourcegitcommit: 7f3ed8b29e63dbe7065afa8597347887a3b866b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122014915"
---
# <a name="how-to-launch-your-spring-cloud-application-from-source-code"></a>Starten Ihrer Spring Cloud-Anwendung aus dem Quellcode

**Dieser Artikel gilt für:** ✔️ Java

Azure Spring Cloud ermöglicht die Erstellung von Spring Cloud-basierten Microserviceanwendungen in Azure.

Sie können Anwendungen direkt aus dem Java-Quellcode oder über eine vorgefertigte JAR-Datei starten. In diesem Artikel werden die Bereitstellungsverfahren erläutert.

In dieser Schnellstartanleitung wird Folgendes erläutert:

> [!div class="checklist"]
> * Bereitstellen einer Dienstinstanz
> * Festlegen eines Konfigurationsservers für eine Instanz
> * Lokales Erstellen einer Microserviceanwendung
> * Bereitstellen der einzelnen Microservices
> * Zuweisen eines öffentlichen Endpunkts für Ihre Anwendung

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Beginn sicher, dass Ihr Azure-Abonnement über die erforderlichen Abhängigkeiten verfügt:

1. [Installation von Git](https://git-scm.com/)
2. [Installation von JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
3. [Installation von Maven 3.0 oder höher](https://maven.apache.org/download.cgi)
4. [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli)
5. [Registrierung für ein Azure-Abonnement](https://azure.microsoft.com/free/)

> [!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie [Azure Cloud Shell](https://shell.azure.com) über shell.azure.com.  Weitere Informationen zu Azure Cloud Shell finden Sie in der [Dokumentation](../cloud-shell/overview.md).

## <a name="install-the-azure-cli-extension"></a>Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure-Befehlszeilenschnittstelle zu installieren:

```azurecli
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-using-the-azure-cli"></a>Bereitstellen einer Dienstinstanz mithilfe der Azure-Befehlszeilenschnittstelle

Melden Sie sich bei der Azure CLI an, und wählen Sie Ihr aktives Abonnement aus.

```azurecli
az login
az account list -o table
az account set --subscription
```

Erstellen Sie eine Ressourcengruppe, die Ihren Azure Spring Cloud-Dienst enthält. Informieren Sie sich weiter über [Azure-Ressourcengruppen](../azure-resource-manager/management/overview.md).

```azurecli
az group create --location eastus --name <resource group name>
```

Führen Sie die folgenden Befehle aus, um eine Instanz von Azure Spring Cloud bereitzustellen. Bereiten Sie einen Namen für Ihren Azure Spring Cloud-Dienst vor. Der Name muss zwischen 4 und 32 Zeichen lang sein und darf nur Kleinbuchstaben, Ziffern und Bindestriche enthalten. Das erste Zeichen des Dienstnamens muss ein Buchstabe und das letzte Zeichen entweder ein Buchstabe oder eine Ziffer sein.

```azurecli
az spring-cloud create -n <resource name> -g <resource group name>
```

Die Bereitstellung der Dienstinstanz dauert ungefähr fünf Minuten.

Legen Sie den Standardnamen für die Ressourcengruppe und für die Azure Spring Cloud-Instanz mithilfe der folgenden Befehle fest:

```azurecli
az config set defaults.group=<service group name>
az config set defaults.spring-cloud=<service instance name>
```

## <a name="create-the-azure-spring-cloud-application"></a>Erstellen der Azure Spring Cloud-Anwendung

Mit dem folgenden Befehl erstellen Sie eine Azure Spring Cloud-Anwendung in Ihrem Abonnement.  Damit wird ein leerer Dienst erstellt, in den Sie in Ihre Anwendung hochladen können.

```azurecli
az spring-cloud app create -n <app-name>
```

## <a name="deploy-your-spring-cloud-application"></a>Bereitstellen der Spring Cloud-Anwendung

Sie können Ihre Anwendung über eine vorkonfigurierte JAR-Datei oder ein Gradle- oder Maven-Repository bereitstellen.  Im Folgenden finden Sie Anweisungen für die einzelnen Fälle.

### <a name="deploy-a-built-jar"></a>Bereitstellen einer erstellten JAR-Datei

Stellen Sie zum Bereitstellen aus einer auf Ihrem lokalen Computer erstellten JAR-Datei sicher, dass der Build eine [fat-JAR](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-build.html#howto-create-an-executable-jar-with-maven)-Datei erzeugt.

So stellen Sie die fat-JAR-Datei für eine aktive Bereitstellung bereit

```azurecli
az spring-cloud app deploy -n <app-name> --jar-path <path-to-fat-JAR, for example "target\hellospring-0.0.1-SNAPSHOT.jar">
```

So stellen Sie die fat-JAR-Datei für eine bestimmte Bereitstellung bereit

```azurecli
az spring-cloud app deployment create --app <app-name> -n <deployment-name> --jar-path <path-to-fat-JAR, for example "target\hellospring-0.0.1-SNAPSHOT.jar">
```

### <a name="deploy-from-source-code"></a>Bereitstellen aus dem Quellcode

Azure Spring Cloud verwendet [kpack](https://github.com/pivotal/kpack), um Ihr Projekt zu erstellen.  Sie können mithilfe der Azure-Befehlszeilenschnittstelle Ihren Quellcode hochladen, das Projekt mit kpack erstellen und es in der Zielanwendung bereitstellen.

> [!WARNING]
> Das Projekt darf nur eine JAR-Datei mit einem `main-class`-Eintrag in der Datei `MANIFEST.MF` in `target` (für Maven-Bereitstellungen) oder `build/libs` (für Gradle-Bereitstellungen) erzeugen.  Wenn mehrere JAR-Dateien mit `main-class`-Einträgen vorhanden sind, führt die Bereitstellung zu einem Fehler.

Für Maven-/Gradle-Projekte mit einem Modul:

```azurecli
cd <path-to-maven-or-gradle-source-root>
az spring-cloud app deploy -n <app-name>
```

Für Maven-/Gradle-Projekte mit mehreren Modulen wiederholen Sie diesen Schritt für jedes Modul:

```azurecli
cd <path-to-maven-or-gradle-source-root>
az spring-cloud app deploy -n <app-name> --target-module <relative-path-to-module>
```

### <a name="show-deployment-logs"></a>Anzeigen von Bereitstellungsprotokollen

Sie überprüfen die kpack-Buildprotokolle mit dem folgenden Befehl:

```azurecli
az spring-cloud app show-deploy-log -n <app-name> [-d <deployment-name>]
```

> [!NOTE]
> In den kpack-Protokollen wird nur die letzte Bereitstellung angezeigt, wenn diese Bereitstellung aus dem Quellcode mithilfe von kpack erstellt wurde.

## <a name="assign-a-public-endpoint-to-gateway"></a>Zuweisen eines öffentlichen Endpunkts zum Gateway

1. Öffnen Sie die Seite **Anwendungs-Dashboard**.
2. Wählen Sie die `gateway`-Anwendung aus, um die Seite **Anwendungsdetails** anzuzeigen.
3. Wählen Sie **Endpunkt zuweisen** aus, um dem Gateway einen öffentlichen Endpunkt zuzuweisen. Dies kann einige Minuten dauern.
4. Geben Sie in Ihrem Browser die zugewiesene öffentliche IP-Adresse ein, um die laufende Anwendung anzuzeigen.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/javae2e?tutorial=asc-source-quickstart&step=public-endpoint)

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen einer Dienstinstanz
> * Festlegen eines Konfigurationsservers für eine Instanz
> * Lokales Erstellen einer Microserviceanwendung
> * Bereitstellen der einzelnen Microservices
> * Bearbeiten der Umgebungsvariablen für Anwendungen
> * Zuweisen einer öffentlichen IP-Adresse für Ihr Anwendungsgateway

> [!div class="nextstepaction"]
> [Schnellstart: Überwachen von Azure Spring Cloud-Apps mit Protokollen, Metriken und Ablaufverfolgung](./quickstart-logs-metrics-tracing.md)

Weitere Beispiele finden Sie auf GitHub: [Azure Spring Cloud-Beispiele](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
