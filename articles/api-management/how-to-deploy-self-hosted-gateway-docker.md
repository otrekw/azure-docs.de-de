---
title: Bereitstellen eines selbstgehosteten Gateways für Docker | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine selbstgehostete Gatewaykomponente von Azure API Management für Docker bereitstellen.
services: api-management
documentationcenter: ''
author: vladvino
manager: gwallace
editor: ''
ms.service: api-management
ms.topic: article
ms.date: 04/26/2020
ms.author: apimpm
ms.openlocfilehash: b9e990988770e8aca015ae8b1159bb4f5e50df57
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "82204827"
---
# <a name="deploy-an-azure-api-management-self-hosted-gateway-to-docker"></a>Bereitstellen eines selbstgehosteten Azure API Management-Gateways für Docker

In diesem Artikel werden die Schritte für die Bereitstellung einer selbstgehosteten Gatewaykomponente von Azure API Management für eine Docker-Umgebung beschrieben.

> [!NOTE]
> Das Hosten eines selbstgehosteten Gateways in Docker eignet sich am besten für Anwendungsfälle, die sich auf die Auswertung und Entwicklung beziehen. Kubernetes wird für den Einsatz in der Produktionsumgebung empfohlen. Informationen zur Bereitstellung eines selbstgehosteten Gateways für Kubernetes finden Sie in [diesem](how-to-deploy-self-hosted-gateway-kubernetes.md) Dokument.

## <a name="prerequisites"></a>Voraussetzungen

- Absolvieren Sie den folgende Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md)
- Erstellen Sie eine Docker-Umgebung. [Docker für Desktop](https://www.docker.com/products/docker-desktop) ist eine gute Option für Entwicklungs- und Evaluierungszwecke. Weitere Informationen zu allen Docker-Editionen, ihren Features sowie eine umfassende Dokumentation zu Docker selbst finden Sie unter [Docker-Dokumentation](https://docs.docker.com).
- [Bereitstellen einer Gatewayressource in Ihrer API Management-Instanz](api-management-howto-provision-self-hosted-gateway.md)

> [!NOTE]
> Das selbstgehostete Gateway ist als x86-64 Linux-basierter Docker-Container gepackt.

## <a name="deploy-the-self-hosted-gateway-to-docker"></a>Bereitstellen des selbstgehosteten Gateways für Docker

1. Wählen Sie **Gateways** unter **Bereitstellung und Infrastruktur** aus.
2. Wählen Sie die Gatewayressource aus, die Sie bereitstellen möchten.
3. Wählen Sie **Bereitstellung** aus.
4. Beachten Sie, dass im Textfeld **Token** automatisch für Sie ein Zugriffstoken mit den Standardwerten **Ablauf** und **Geheimer Schlüssel** generiert wurde. Wählen Sie bei Bedarf die gewünschten Werte in einem oder beiden Steuerelementen aus, um ein neues Token zu generieren.
4. Stellen Sie sicher, dass **Docker** unter **Bereitstellungsskripts** ausgewählt ist.
5. Wählen Sie den Link zur Datei **env.conf** neben **Umgebung** aus, um die Datei herunterzuladen.
6. Wählen Sie das Symbol **Kopieren** rechts neben dem Textfeld **Ausführen** aus, um den Docker-Befehl in die Zwischenablage zu kopieren.
7. Fügen Sie den Befehl in das Terminalfenster (oder Befehlsfenster) ein. Passen Sie die Portzuordnungen und den Containernamen nach Bedarf an. Beachten Sie, dass der Befehl annimmt, dass die heruntergeladene Umgebungsdatei im aktuellen Verzeichnis vorhanden ist.
```
    docker run -d -p 80:8080 -p 443:8081 --name <gateway-name> --env-file env.conf mcr.microsoft.com/azure-api-management/gateway:<tag>
```
8. Führen Sie den Befehl aus. Der Befehl weist Ihre Docker-Umgebung an, den Container auszuführen. Dabei wird das aus Microsoft Container Registry heruntergeladene [Containerimage](https://aka.ms/apim/sputnik/dhub) verwendet, und die HTTP-Ports (8080) und HTTPS-Ports (8081) des Containers werden den Ports 80 und 443 auf dem Host zugeordnet.
9. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob der Gatewaycontainer ausgeführt wird:
```console
docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                         NAMES
895ef0ecf13b        mcr.microsoft.com/azure-api-management/gateway:latest   "/bin/sh -c 'dotnet …"   5 seconds ago       Up 3 seconds        0.0.0.0:80->8080/tcp, 0.0.0.0:443->8081/tcp   my-gateway
```
10. Kehren Sie zum Azure-Portal zurück, klicken Sie auf **Übersicht** und bestätigen Sie, dass der selbstgehostete Gatewaycontainer, den Sie gerade bereitgestellt haben, einen fehlerfreien Status meldet.

![Gatewaystatus](media/how-to-deploy-self-hosted-gateway-docker/status.png)

> [!TIP]
> Verwenden Sie den Befehl <code>console docker container logs <gateway-name></code>, um eine Momentaufnahme des Protokolls des selbstgehosteten Gateways anzuzeigen.
>
> Verwenden Sie den Befehl <code>docker container logs --help</code>, um alle Optionen für die Protokollanzeige anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum selbstgehosteten Gateway finden Sie unter [Azure API Management: Übersicht über das selbstgehostete Gateway](self-hosted-gateway-overview.md).
* [Konfigurieren eines benutzerdefinierten Domänennamens für das selbstgehostete Gateway](api-management-howto-configure-custom-domain-gateway.md)
