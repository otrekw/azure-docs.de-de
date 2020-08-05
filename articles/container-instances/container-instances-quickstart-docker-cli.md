---
title: 'Schnellstart: Bereitstellen von Docker-Containern in einer Containerinstanz – Docker CLI'
description: In diesem Schnellstart verwenden Sie die Docker CLI, um schnell eine containerbasierte Web-App bereitzustellen, die in einer isolierten Azure-Containerinstanz ausgeführt wird.
ms.topic: quickstart
ms.date: 07/16/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 0e4569904ef6aee304518ce012889d10dc2ecbce
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408083"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-docker-cli"></a>Schnellstart: Schnellstart: Bereitstellen einer Containerinstanz in Azure mithilfe der Docker CLI

Führen Sie mithilfe von Azure Container Instances serverlose Docker-Container schnell und einfach in Azure aus. Stellen Sie in einer Containerinstanz bei Bedarf bereit, wenn Sie cloudnative Apps entwickeln und nahtlos von der lokalen Entwicklung zur Cloudbereitstellung wechseln möchten.

In dieser Schnellstartanleitung verwenden Sie native Docker CLI-Befehle, um einen Docker-Container bereitzustellen, und machen seine Anwendung in Azure Container Instances verfügbar. Diese Funktion wird durch die [Integration zwischen Docker und Azure](https://docs.docker.com/engine/context/aci-integration/) (Betaversion) ermöglicht. Einige Sekunden nach dem Ausführen eines `docker run`-Befehls können Sie zu der im Container ausgeführten Anwendung wechseln:

:::image type="content" source="media/container-instances-quickstart-docker-cli/view-application-running-in-an-azure-container-instance.png" alt-text="Mit Azure Container Instances bereitgestellte App im Browser":::

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-account] erstellen, bevor Sie beginnen.

Für diese Schnellstartanleitung müssen Sie Docker Desktop Edge, Version 2.3.2.0 oder höher, installieren, verfügbar für [Windows](https://desktop.docker.com/win/edge/Docker%20Desktop%20Installer.exe) oder [macOS](https://desktop.docker.com/mac/edge/Docker.dmg). Oder installieren Sie die [Docker ACI-Integrations-CLI für Linux](https://docs.docker.com/engine/context/aci-integration/#install-the-docker-aci-integration-cli-on-linux) (Betaversion). 

> [!IMPORTANT]
> Diese Funktion befindet sich derzeit in der Vorschauphase und erfordert Betafunktionen (Vorschau) in Docker. Weitere Informationen über [Stabile und Edge-Versionen von Docker Desktop](https://docs.docker.com/desktop/#stable-and-edge-versions). Nicht alle Funktionen von Azure Container Instances werden unterstützt. Geben Sie Feedback zur Integration von Docker und Azure ab, indem Sie im GitHub-Repository [aci-integration-beta](https://github.com/docker/aci-integration-beta) ein Problem erstellen.

## <a name="create-azure-context"></a>Erstellen eines Azure-Kontexts

Um mithilfe von Docker-Befehlen Container in Azure Container Instances auszuführen, melden Sie sich zuerst bei Azure an:

```bash
docker login azure
```

Wenn Sie dazu aufgefordert werden, geben Sie Ihre Azure-Anmeldeinformationen ein, oder wählen Sie sie aus.


Führen Sie `docker context create aci` aus, um einen ACI-Kontext zu erstellen. Dieser Kontext ordnet Docker Ihrem Azure-Abonnement zu, damit Sie Containerinstanzen erstellen können. Um beispielsweise einen Kontext namens *myacicontext* zu erstellen:

```
docker context create aci myacicontext
```

Wählen Sie bei Aufforderung Ihre Azure-Abonnement-ID aus, wählen Sie dann eine vorhandene Ressourcengruppe aus, oder **erstellen Sie eine neue Ressourcengruppe**. Wenn Sie eine neue Ressourcengruppe ausgewählt haben, wird diese mit einem systemseitig generierten Namen erstellt. Azure Container Instances muss wie alle Azure-Ressourcen in einer Ressourcengruppe bereitgestellt werden. Mit Ressourcengruppen können verwandte Azure-Ressourcen organisiert und verwaltet werden.


Führen Sie `docker context ls` aus, um zu bestätigen, dass Sie Ihren Docker-Kontexten den ACI-Kontext hinzugefügt haben:

```
docker context ls
```

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

:::image type="content" source="media/container-instances-quickstart-docker-cli/view-application-running-in-an-azure-container-instance.png" alt-text="Mit Azure Container Instances bereitgestellte App im Browser":::

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

Fahren Sie mit dem Azure Container Instances-Tutorial fort, wenn Sie ein Containerimage erstellen und über eine private Azure-Containerregistrierung bereitstellen möchten.

> [!div class="nextstepaction"]
> [Azure Container Instances-Tutorial](./container-instances-tutorial-prepare-app.md)

<!-- LINKS - External -->

[azure-account]: https://azure.microsoft.com/free/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

