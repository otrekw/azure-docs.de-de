---
title: Basisimageaktualisierungen – Aufgaben
description: Erfahren Sie mehr über Basisimages für Anwendungscontainerimages und wie eine Basisimageaktualisierung eine Azure Container Registry-Aufgabe auslösen kann.
ms.topic: article
ms.date: 01/22/2019
ms.openlocfilehash: 017c8f8a3a15896bd6e14a54136ba713e9f9c499
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77617730"
---
# <a name="about-base-image-updates-for-acr-tasks"></a>Informationen zu Basisimageaktualisierungen für ACR Tasks

Dieser Artikel enthält Hintergrundinformationen zu Aktualisierungen des Basisimages einer Anwendung und wie diese Aktualisierungen eine Azure Container Registry-Aufgabe auslösen können.

## <a name="what-are-base-images"></a>Was sind Basisimages?

Die meisten Containerimages werden mithilfe von Dockerfiles definiert. Dockerfiles geben als Basis ein häufig als *Basisimage* bezeichnetes übergeordnetes Image an, auf dem das Image basiert. Basisimages enthalten in der Regel das Betriebssystem (also beispielsweise [Alpine Linux][base-alpine] oder [Windows Nano Server][base-windows]), auf das die restlichen Ebenen des Containers angewendet werden. Sie können auch Anwendungsframeworks wie [Node.js][base-node] oder [.NET Core][base-dotnet] enthalten. Diese Basisimages basieren üblicherweise selbst auf öffentlichen Upstreamimages. Ein Basisimage kann mehreren Anwendungsimages zugrunde liegen.

Ein Basisimage wird häufig von der für das Image zuständigen Person aktualisiert, um neue Features oder Verbesserungen für das enthaltene Betriebssystem oder Framework zu integrieren. Ein weiterer häufiger Grund für die Aktualisierung eines Basisimages sind Sicherheitspatches. Wenn diese Upstreamupdates auftreten, müssen Sie auch die Basisimages aktualisieren, damit sie die kritische Fehlerbehebung enthalten. Daraufhin müssen auch alle Anwendungsimages neu erstellt werden, um die Upstreamkorrekturen aus Ihrem Basisimage zu integrieren.

In einigen Fällen, z. B. bei einem privaten Entwicklungsteam, kann ein Basisimage mehr als BS oder Framework angeben. Beispielsweise könnte ein Basisimage ein gemeinsam genutztes Dienstkomponentenimage sein, das nachverfolgt werden muss. Mitglieder eines Teams müssen dieses Basisimage möglicherweise zu Testzwecken nachverfolgen oder das Image bei der Entwicklung von Anwendungsimages regelmäßig aktualisieren.

## <a name="track-base-image-updates"></a>Nachverfolgen von Basisimageaktualisierungen

ACR Tasks ermöglicht die automatische Erstellung von Images nach der Aktualisierung eines Containerbasisimages.

ACR Tasks ermittelt dynamisch Basisimageabhängigkeiten, wenn ein Containerimage erstellt wird. So ist zu erkennen, wenn das Basisimage eines Anwendungsimages aktualisiert wird. Mit einer einzelnen vorkonfigurierten Buildaufgabe kann ACR Tasks automatisch alle Anwendungsimages neu erstellen, die auf das Basisimage verweisen. Die automatische Erkennung und Neuerstellung von ACR Tasks spart Zeit, die ansonsten für die manuelle Nachverfolgung und Aktualisierung der einzelnen Anwendungsimages aufgewendet werden müsste, die auf Ihr aktualisiertes Basisimage verweisen.

## <a name="base-image-locations"></a>Basisimage-Speicherorte

Bei Imagebuilds aus einem Dockerfile erkennt ein ACR-Task Abhängigkeiten von Basisimages an den folgenden Speicherorten:

* Dieselbe Azure-Containerregistrierung, in der der Task ausgeführt wird
* Eine andere private Azure-Containerregistrierung in der gleichen oder einer anderen Region 
* Ein öffentliches Repository in Docker Hub 
* Ein öffentliches Repository in Microsoft Container Registry

Wenn sich das in der Anweisung `FROM` angegebene Basisimage in einem dieser Speicherorte befindet, fügt die ACR-Aufgabe einen Hook hinzu, um sicherzustellen, dass das Image nach jeder Aktualisierung seiner Basis neu erstellt wird.

## <a name="additional-considerations"></a>Weitere Überlegungen

* **Basisimages für Anwendungsimages**: Derzeit verfolgt eine ACR-Aufgabe nur Basisimageaktualisierungen für Anwendungsimages (*Runtime*) nach. Basisimageaktualisierungen für Zwischenimages (*Buildzeit*), die in mehrstufigen Dockerfiles verwendet werden, werden nicht nachverfolgt.  

* **Standardmäßig aktiviert**: Wenn Sie eine ACR-Aufgabe mit dem Befehl [az acr task create][az-acr-task-create] erstellen, wird sie standardmäßig für die Auslösung von einer Aktualisierung des Basisimages *aktiviert*. Das heißt, die `base-image-trigger-enabled`-Eigenschaft wird auf TRUE festgelegt. Wenn Sie dieses Verhalten in einer Aufgabe deaktivieren möchten, ändern Sie den Wert der Eigenschaft in FALSE. Führen Sie beispielsweise den folgenden Befehl [az acr task update][az-acr-task-update] aus:

  ```azurecli
  az acr task update --myregistry --name mytask --base-image-trigger-enabled False
  ```

* **Zum Nachverfolgen von Abhängigkeiten auslösen**: Damit eine ACR-Aufgabe die Abhängigkeiten eines Containerimages (einschließlich des Basisimages) bestimmen und nachverfolgen kann, müssen Sie die Aufgabe zunächst **mindestens einmal** zum Erstellen des Images auslösen. Lösen Sie z. B. die Aufgabe mit dem Befehl [az acr task run][az-acr-task-run] manuell aus.

* **Stabiles Tag für Basisimage**: Damit eine Aufgabe bei der Aktualisierung des Basisimages ausgelöst wird, muss das Basisimage ein *stabiles* Tag enthalten, z. B. `node:9-alpine`. Dieses Tagging ist typisch für ein Basisimage, das mit Betriebssystem- und Frameworkpatches auf eine aktuelle stabile Version aktualisiert wird. Wenn das Basisimage mit einem neuen Versionstag aktualisiert wird, wird keine Aufgabe ausgelöst. Weitere Informationen zur Imagemarkierung finden Sie in der [Anleitung zu bewährten Methoden](container-registry-image-tag-version.md). 

* **Andere Aufgabentrigger**: In einer durch Basisimageupdates ausgelösten Aufgabe können Sie Trigger auch auf Basis eines [Quellcodecommits](container-registry-tutorial-build-task.md) oder [Zeitplans](container-registry-tasks-scheduled.md) aktivieren. Ein Basisimageupdate kann auch eine [mehrstufige Aufgabe](container-registry-tasks-multi-step.md) auslösen.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Tutorials finden Sie Szenarios zum Automatisieren von Anwendungsimagebuilds, nachdem ein Basisimage aktualisiert wurde:

* [Automatisieren von Buildvorgängen für Containerimages nach der Aktualisierung eines Basisimages in derselben Registrierung](container-registry-tutorial-base-image-update.md)

* [Automatisieren von Buildvorgängen für Containerimages nach der Aktualisierung eines Basisimages in einer anderen Registrierung](container-registry-tutorial-base-image-update.md)


<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
