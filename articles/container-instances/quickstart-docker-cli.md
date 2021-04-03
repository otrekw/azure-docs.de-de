---
title: 'Schnellstart: Bereitstellen von Docker-Containern in einer Containerinstanz – Docker CLI'
description: In diesem Schnellstart verwenden Sie die Docker CLI, um schnell eine containerbasierte Web-App bereitzustellen, die in einer isolierten Azure-Containerinstanz ausgeführt wird.
ms.topic: quickstart
ms.date: 09/14/2020
ms.custom: devx-track-js
ms.openlocfilehash: 9904fb8c2142816196a1939a16445318bdb245d0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91262313"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-docker-cli"></a>Schnellstart: Schnellstart: Bereitstellen einer Containerinstanz in Azure mithilfe der Docker CLI

Führen Sie mithilfe von Azure Container Instances serverlose Docker-Container schnell und einfach in Azure aus. Stellen Sie in einer Containerinstanz bei Bedarf bereit, wenn Sie cloudnative Apps entwickeln und nahtlos von der lokalen Entwicklung zur Cloudbereitstellung wechseln möchten.

In dieser Schnellstartanleitung verwenden Sie native Docker CLI-Befehle, um einen Docker-Container bereitzustellen, und machen seine Anwendung in Azure Container Instances verfügbar. Diese Funktion wird durch die [Integration zwischen Docker und Azure](https://docs.docker.com/engine/context/aci-integration/) ermöglicht. Einige Sekunden nach dem Ausführen eines `docker run`-Befehls können Sie zu der im Container ausgeführten Anwendung wechseln:

:::image type="content" source="media/quickstart-docker-cli/view-application-running-in-an-azure-container-instance.png" alt-text="Mit Azure Container Instances bereitgestellte App im Browser":::

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-account] erstellen, bevor Sie beginnen.

Für diese Schnellstartanleitung benötigen Sie Docker Desktop Edge, Version 2.3.0.5 oder höher, verfügbar für [Windows](https://desktop.docker.com/win/edge/Docker%20Desktop%20Installer.exe) oder [macOS](https://desktop.docker.com/mac/edge/Docker.dmg). Oder installieren Sie die [Docker ACI-Integrations-CLI für Linux](https://docs.docker.com/engine/context/aci-integration/#install-the-docker-aci-integration-cli-on-linux). 

> [!IMPORTANT]
> Nicht alle Funktionen von Azure Container Instances werden unterstützt. Geben Sie Feedback zur Integration von Docker und Azure ab, indem Sie im GitHub-Repository [aci-integration-beta](https://github.com/docker/aci-integration-beta) ein Problem erstellen.

[!INCLUDE [container-instances-create-docker-context](../../includes/container-instances-create-docker-context.md)]

## <a name="create-a-container"></a>Erstellen eines Containers

Nachdem Sie einen Docker-Kontext erstellt haben, können Sie einen Container in Azure erstellen. In diesem Schnellstart verwenden Sie das öffentliche Image `mcr.microsoft.com/azuredocs/aci-helloworld`. Dieses Image verpackt eine kleine in Node.js geschriebene Web-App, die eine statische HTML-Seite bedient.

Zuerst wechseln Sie den ACI-Kontext. Alle nachfolgenden Docker-Befehle werden in diesem Kontext ausgeführt.

```
docker context use myacicontext
```

Führen Sie den folgenden `docker run`-Befehl aus, um die Azure-Containerinstanz mit Port 80 zu erstellen, der über das Internet verfügbar ist:

```
docker run -p 80:80 mcr.microsoft.com/azuredocs/aci-helloworld
```

Beispielausgabe für eine erfolgreiche Bereitstellung:

```
[+] Running 2/2
 ⠿ hungry-kirch            Created                                                                               5.1s
 ⠿ single--container--aci  Done                                                                                 11.3s
hungry-kirch
```

Führen Sie `docker ps` aus, um Details zum ausgeführten Container zu erhalten, einschließlich der öffentlichen IP-Adresse:

```
docker ps
```


Die Beispielausgabe zeigt eine öffentliche IP-Adresse, in diesem Fall *52.230.225.232*:

```
CONTAINER ID        IMAGE                                        COMMAND             STATUS              PORTS
hungry-kirch        mcr.microsoft.com/azuredocs/aci-helloworld                       Running             52.230.225.232:80->80/tcp
```

 Wechseln Sie jetzt in Ihrem Webbrowser zu der IP-Adresse. Der Vorgang war erfolgreich, wenn in etwa die folgende Webseite angezeigt wird. Sie haben eine in einem Docker-Container ausgeführte Anwendung in Azure bereitgestellt.

:::image type="content" source="media/quickstart-docker-cli/view-application-running-in-an-azure-container-instance.png" alt-text="Mit Azure Container Instances bereitgestellte App im Browser":::

## <a name="pull-the-container-logs"></a>Herunterladen der Containerprotokolle

Wenn Sie ein Problem mit einem Container oder der darin ausgeführten Anwendung beheben (oder nur seine Ausgabe anzeigen) müssen, sehen Sie sich zuerst die Protokolle der Containerinstanz an.

Führen Sie z. B. den Befehl `docker logs` aus, um die Protokolle des *hungry-kirch*-Containers im ACI-Kontext anzuzeigen:

```azurecli-interactive
docker logs hungry-kirch
```

Die Ausgabe enthält die Protokolle für den Container und sollte die HTTP GET-Anforderungen anzeigen, die beim Betrachten der Anwendung in Ihrem Browser generiert wurden.

```output
listening on port 80
::ffff:10.240.255.55 - - [07/Jul/2020:17:43:53 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [07/Jul/2020:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [07/Jul/2020:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
```


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Container nicht mehr benötigen, führen Sie `docker rm` aus, um ihn zu entfernen. Dieser Befehl beendet und löscht die Azure-Containerinstanz.

```
docker rm hungry-kirch
```


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure-Containerinstanz aus einem öffentlichen Image erstellt, indem Sie die Integration zwischen Docker und Azure verwendet haben. Weitere Informationen zu Integrationsszenarien finden Sie in der [Docker-Dokumentation](https://docs.docker.com/engine/context/aci-integration/). 

Sie können auch die [Docker-Erweiterung](https://aka.ms/VSCodeDocker) für Visual Studio Code verwenden, um eine integrierte Erfahrung zum Entwickeln, Ausführen und Verwalten von Containern, Images und Kontexten zu erhalten.

Informationen zum Verwenden von Azure Tools zum Erstellen und Verwalten von Containerinstanzen finden Sie in anderen Schnellstartanleitungen, die die [Azure CLI](container-instances-quickstart.md), [Azure PowerShell](container-instances-quickstart-powershell.md), das [Azure-Portal](container-instances-quickstart-portal.md) oder [Azure Resource Manager-Vorlagen](container-instances-quickstart-template.md) verwenden.

Wenn Sie Docker Compose verwenden möchten, um eine Anwendung mit mehreren Containern lokal zu definieren und auszuführen, und dann zu Azure Container Instances wechseln, fahren Sie mit dem Tutorial fort.

> [!div class="nextstepaction"]
> [Docker Compose-Tutorial](./tutorial-docker-compose.md)

<!-- LINKS - External -->

[azure-account]: https://azure.microsoft.com/free/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

