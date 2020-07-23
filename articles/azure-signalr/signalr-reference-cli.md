---
title: Azure CLI-Beispiele – Azure SignalR-Dienst
description: Folgen Sie echten Beispielen, um die Azure CLI für Azure SignalR Service zu lernen. Sie erfahren, wie Sie SignalR Service mit weiteren Azure-Diensten erstellen können.
author: sffamily
ms.service: signalr
ms.topic: reference
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 99ebe9c078ef4775e8f0df0545fb0af4b3d1057b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515239"
---
# <a name="azure-cli-reference"></a>Azure CLI-Referenz

Die folgende Tabelle enthält Links zu Bash-Skripts für den Azure SignalR-Dienst, die mithilfe der Azure CLI 2.0 erstellt wurden.

| Skript | Beschreibungen |
|-|-|
|**Erstellen**||
| [Erstellen eines neuen SignalR-Diensts und einer neuen Ressourcengruppe](scripts/signalr-cli-create-service.md) | Dieses Skript erstellt eine neue Azure SignalR-Dienstressource in einer neuen Ressourcengruppe mit einem beliebigen Namen.  |
|**Integrieren**||
| [Erstellen eines neuen SignalR-Diensts und einer für die Verwendung von SignalR konfigurierten Web-App](scripts/signalr-cli-create-with-app-service.md) | Dieses Skript erstellt eine neue Azure SignalR-Dienstressource in einer neuen Ressourcengruppe mit einem beliebigen Namen. Es fügt auch eine neue Web-App und einen App Service-Plan hinzu, um eine ASP.NET Core-Web-App zu hosten, die den SignalR-Dienst verwendet. Die Web-App ist mit einer App-Einstellung konfiguriert, um eine Verbindung mit der neuen SignalR-Dienstressource herzustellen. |
| [Erstellen eines neuen SignalR-Diensts und einer für die Verwendung von SignalR und GitHub OAuth konfigurierten Web-App](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Dieses Skript erstellt eine neue Azure SignalR-Dienstressource in einer neuen Ressourcengruppe mit einem beliebigen Namen. Es fügt auch eine neue Azure Web-App und einen Hostingplan hinzu, um eine ASP.NET Core-Web-App zu hosten, die den SignalR-Dienst verwendet. Die Web-App ist mit App-Einstellungen für die Verbindungszeichenfolge für die neue SignalR-Dienstressource sowie mit geheimen Clientschlüsseln konfiguriert, um die [GitHub-Authentifizierung](https://developer.github.com/v3/guides/basics-of-authentication/) zu unterstützen. Dies wird im [Tutorial zur Authentifizierung](signalr-concept-authenticate-oauth.md) veranschaulicht. Die Web-App ist auch für die Verwendung eines lokalen Git-Repositorys als Bereitstellungsquelle konfiguriert. |
| | |
