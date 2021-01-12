---
title: 'Azure Container Instances: Anleitung'
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie Cognitive Services-Container für Azure Container Instances bereitstellen.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: aahi
ms.openlocfilehash: 003b4411ac791898f4a7467b9b03f29aadba2fc7
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2020
ms.locfileid: "97704837"
---
# <a name="deploy-and-run-container-on-azure-container-instance"></a>Bereitstellen und Ausführen eines Containers unter Azure Container Instances

Führen Sie die folgenden Schritte aus, um Azure Cognitive Services-Anwendungen problemlos in der Cloud zu skalieren, indem Sie [Azure Container Instances](../../container-instances/index.yml) verwenden. Mithilfe von Containern können Sie sich auf die Erstellung von Anwendungen konzentrieren und müssen sich nicht um die Verwaltung der Infrastruktur kümmern. Weitere Informationen zur Verwendung von Containern finden Sie unter [Features und Vorteile](../cognitive-services-container-support.md#features-and-benefits).

## <a name="prerequisites"></a>Voraussetzungen

Diese Lösung funktioniert für alle Cognitive Services-Container. Die Cognitive Services-Ressource muss vor der Verwendung der Lösung erstellt werden. Jede Cognitive Services-Instanz mit Containerunterstützung verfügt über einen Artikel mit einer Installationsanleitung, in dem die Installation und Konfiguration des Diensts für einen Container beschrieben wird. Da für einige Dienste eine Datei oder eine Gruppe von Dateien als Eingabe für den Container benötigt wird, sollten Sie unbedingt mit dem Container vertraut sein und diesen bereits erfolgreich verwendet haben, bevor Sie diese Lösung einsetzen.

* Eine Azure-Ressource für die von Ihnen verwendete Azure Cognitive Services-Instanz.
* Cognitive Services-**Endpunkt-URL**: Lesen Sie die entsprechende Installationsanleitung Ihres Diensts für den Container, um über das Azure-Portal zu ermitteln, wo sich die Endpunkt-URL befindet, und wie ein korrektes Beispiel für die URL aussieht. Das genaue Format kann sich von Dienst zu Dienst unterscheiden.
* Cognitive Services-**Schlüssel**: Die Schlüssel befinden sich auf der Seite **Schlüssel** für die Azure-Ressource. Sie benötigen nur einen der beiden Schlüssel. Der Schlüssel ist eine Zeichenfolge mit 32 alphanumerischen Zeichen.

* Ein einzelner Cognitive Services-Container auf Ihrem lokalen Host (Ihrem Computer). Stellen Sie sicher, dass Folgendes möglich ist:
  * Rufen Sie das Image mit einem `docker pull`-Befehl ab.
  * Führen Sie den lokalen Container erfolgreich mit allen erforderlichen Konfigurationseinstellungen aus, indem Sie einen `docker run`-Befehl verwenden.
  * Rufen Sie den Endpunkt des Containers auf, um eine HTTP 2xx-Antwort und eine JSON-Antwort zu erhalten.

Sie müssen alle Variablen in spitzen Klammern (`<>`) durch Ihre eigenen Werte ersetzen. Hierbei müssen auch die spitzen Klammern ersetzt werden.

> [!IMPORTANT]
> Der LUIS-Container erfordert eine `.gz`-Modelldatei, die zur Laufzeit abgerufen wird. Der Container muss auf diese Modelldatei über eine Volumeeinbindung aus der Containerinstanz zugreifen können. Gehen Sie zum Hochladen einer Modelldatei wie folgt vor:
> 1. [Erstellen Sie eine Azure-Dateifreigabe.](../../storage/files/storage-how-to-create-file-share.md) Notieren Sie sich den Namen des Azure Storage-Kontos, den Schlüssel und den Namen der Dateifreigabe, da Sie diese Angaben später benötigen.
> 2. [Exportieren Sie Ihr LUIS-Modell (App-Paket) über das LUIS-Portal.](../LUIS/luis-container-howto.md#export-packaged-app-from-luis) 
> 3. Navigieren Sie im Azure-Portal zur Seite **Übersicht** Ihrer Speicherkontoressource, und wählen Sie **Dateifreigaben** aus. 
> 4. Wählen Sie den Namen der kürzlich erstellten Dateifreigabe und anschließend **Hochladen** aus. Laden Sie dann Ihr App-Paket hoch. 

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

[!INCLUDE [Portal instructions for creating an ACI instance](includes/create-container-instances-resource.md)]

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli)

[!INCLUDE [CLI instructions for creating an ACI instance](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

---


## <a name="use-the-container-instance"></a>Verwenden der Containerinstanz

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

1. Wählen Sie **Übersicht** aus, und kopieren Sie die IP-Adresse. Dies ist eine numerische IP-Adresse, z. B. `55.55.55.55`.
1. Öffnen Sie eine neue Browserregisterkarte, und verwenden Sie die IP-Adresse, z. B. `http://<IP-address>:5000 (http://55.55.55.55:5000`). Sie sehen die Homepage des Containers und erkennen daran, dass der Container ausgeführt wird.

    ![Homepage des Containers](../../../includes/media/cognitive-services-containers-api-documentation/container-webpage.png)

1. Wählen Sie **Dienst-API-Beschreibung**, um die Swagger-Seite für den Container anzuzeigen.

1. Wählen Sie eine der **POST**-APIs und dann **Ausprobieren** aus.  Die Parameter werden einschließlich der Eingabe angezeigt. Fügen Sie die Parameter ein.

1. Wählen Sie **Ausführen**, um die Anforderung an Ihre Containerinstanz zu senden.

    Sie haben Cognitive Services-Container in Azure Container Instances erfolgreich erstellt und verwendet.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli)

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

> [!NOTE]
> Wenn Sie den Container für die Textanalyse im Gesundheitsbereich ausführen, verwenden Sie die folgende URL, um Abfragen zu übermitteln: `http://localhost:5000/text/analytics/v3.2-preview.1/entities/health`

---
