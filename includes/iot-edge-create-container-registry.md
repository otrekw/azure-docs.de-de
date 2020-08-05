---
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/30/2019
ms.author: kgremban
ms.openlocfilehash: 0c43c6dcced94225e9ab9ae903535ce74286ad9a
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87406712"
---
## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

In diesem Tutorial verwenden Sie die Azure IoT Tools-Erweiterung zum Entwickeln eines Moduls und zum Erstellen eines **Containerimages** aus den Dateien. Danach pushen Sie dieses Image in ein **Repository**, in dem Ihre Images gespeichert und verwaltet werden. Abschließend stellen Sie Ihr Image aus der Registrierung zur Ausführung auf dem IoT Edge-Gerät bereit.

Sie können eine beliebige Docker-kompatible Registrierung zum Speichern Ihrer Containerimages verwenden. Zwei beliebte Docker-Registrierungsdienste sind [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) und [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In diesem Tutorial wird Azure Container Registry verwendet.

Sollten Sie noch keine Containerregistrierung besitzen, führen Sie die folgenden Schritte aus, um eine neue Containerregistrierung in Azure zu erstellen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **Ressource erstellen** > **Container** > **Container Registry** aus.

2. Geben Sie die folgenden Werte an, um Ihre Containerregistrierung zu erstellen:

   | Feld | value |
   | ----- | ----- |
   | Subscription | Wählen Sie ein Abonnement aus der Dropdownliste aus. |
   | Resource group | Es wird empfohlen, die gleiche Ressourcengruppe für alle Testressourcen zu verwenden, die Sie während der IoT Edge-Schnellstarts und -Tutorials erstellen. Beispielsweise **IoTEdgeResources**. |
   | Registrierungsname | Geben Sie einen eindeutigen Namen an. |
   | Location | Wählen Sie einen Standort in Ihrer Nähe aus. |
   | SKU | Wählen Sie **Basic** aus. |

3. Klicken Sie auf **Erstellen**.

4. Navigieren Sie nach der Erstellung der Containerregistrierung zu ihr, und wählen Sie im Menü unter **Einstellungen** im linken Bereich **Zugriffsschlüssel** aus. 

5. Klicken Sie zum Aktivieren des Administratorbenutzers, um den **Benutzernamen** und das **Kennwort** für Ihre Containerregistrierung anzuzeigen.

6. Kopieren Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**, und speichern Sie sie an einem geeigneten Speicherort. Sie verwenden diese Werte in diesem Tutorial, um Zugriff auf die Containerregistrierung zu gewähren.

   ![Kopieren des Anmeldeservers, des Benutzernamens und des Kennworts für die Containerregistrierung](./media/iot-edge-create-container-registry/registry-access-key.png)