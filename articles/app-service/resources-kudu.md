---
title: Übersicht über den Kudu-Dienst
description: Hier erhalten Sie Informationen über die Engine, auf der Continuous Deployment in App Service und die entsprechenden Funktionen beruhen.
ms.date: 03/17/2021
ms.topic: reference
ms.openlocfilehash: ad1456f1ca123127f3d84aa8195c99fc34872aee
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104608591"
---
# <a name="kudu-service-overview"></a>Übersicht über den Kudu-Dienst

Kudu ist die Engine hinter einer Reihe von Features in [Azure App Service](overview.md) zur Bereitstellung mithilfe der Quellcodeverwaltung und anderen Bereitstellungsmethoden wie die Synchronisierung mit Dropbox und OneDrive. 

## <a name="access-kudu-for-your-app"></a>Zugriff auf Kudu für Ihre App
Jedes Mal, wenn Sie eine App erstellen, wird von App Service eine Begleit-App erstellt, die durch HTTPS geschützt ist. Diese Kudu-App ist unter folgender URL verfügbar:

- App außerhalb der Dienstebene „Isoliert“: `https://<app-name>.scm.azurewebsites.net`
- App in der Dienstebene „Isoliert“ (App Service-Umgebung): `https://<app-name>.scm.<ase-name>.p.azurewebsites.net`

Weitere Informationen finden Sie unter [Zugreifen auf den kudu-Dienst](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).

## <a name="kudu-features"></a>Kudu-Features

Kudu stellt hilfreiche Informationen zu Ihrer App Service-App bereit, z. B.:

- App-Einstellungen
- Verbindungszeichenfolgen
- Umgebungsvariablen
- Servervariablen
- HTTP-Header

Der Dienst bietet außerdem weitere Funktionen wie die folgenden:

- Ausführen von Befehlen in der [Kudu-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console).
- Herunterladen von IIS-Speicherabbildern zur Diagnose und Docker-Protokollen.
- Verwalten von IIS-Prozessen und Websiteerweiterungen.
- Hinzufügen von Bereitstellungswebhooks für Windows-Apps.
- Benutzeroberfläche für die ZIP-Bereitstellung mit `/ZipDeploy`.
- Generieren [benutzerdefinierter Bereitstellungsskripts](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
- Zugriff über die [REST-API](https://github.com/projectkudu/kudu/wiki/REST-API).

## <a name="more-resources"></a>Weitere Ressourcen

Kudu ist ein [Open-Source-Projekt](https://github.com/projectkudu/kudu) und ist im [Kudu-Wiki](https://github.com/projectkudu/kudu/wiki) dokumentiert.

