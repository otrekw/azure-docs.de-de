---
author: naiteeks
ms.topic: include
ms.service: azure-video-analyzer
ms.date: 04/20/2021
ms.author: juliako
ms.openlocfilehash: 1cce5d0f49604ce310e40de037b9a77e04615bc9
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110371812"
---
Der Bereitstellungsprozess dauert ungefähr **20 Minuten**. Nach Abschluss haben Sie bestimmte Azure-Ressourcen im Azure-Abonnement bereitgestellt. Hierzu zählen unter anderem:

1. **Video Analyzer-Konto:** Dieser [Clouddienst](../../overview.md) wird zum Registrieren des Video Analyzer-Edgemoduls und zur Wiedergabe aufgezeichneter Videos und Videoanalysen verwendet.
1. **Speicherkonto:** Zum Speichern aufgezeichneter Videos und Videoanalysen.
1. **Verwaltete Identität**: Dies ist die benutzerseitig zugewiesene [verwaltete Identität](../../../../active-directory/managed-identities-azure-resources/overview.md), die zum Verwalten des Zugriffs auf das obige Speicherkonto verwendet wird.
1. **Virtueller Computer:** Ein virtueller Computer, der als Ihr simuliertes Edgegerät dient.
1. **IoT Hub**: Dient als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen Ihrer IoT-Anwendung, IoT Edge-Modulen und den verwalteten Geräten.

Zusätzlich zu den oben erwähnten Ressourcen werden auf der Dateifreigabe „deployment-output“ in Ihrem Speicherkonto auch die folgenden Elemente für die Verwendung in Schnellstartanleitungen und Tutorials erstellt:

- **_appsettings.json_**: Diese Datei enthält die **Geräte-Verbindungszeichenfolge** und andere Eigenschaften, die zum Ausführen der Beispielanwendung in Visual Studio Code benötigt werden.
- **_env.txt_**: Diese Datei enthält die Umgebungsvariablen, die Sie zum Generieren von Bereitstellungsmanifesten mit Visual Studio Code benötigen.
- **_deployment.json_**: Dies ist das Bereitstellungsmanifest, das von der Vorlage zum Bereitstellen von Edgemodulen auf dem simulierten Edgegerät verwendet wird.

<!-- TODO: provide a link to the readme.md in github.com/azure-video-analyzer/setup/readme.md where we can list out all resources like virtual network etc. -->

> [!TIP]
> Falls für erstellte Azure-Ressourcen Probleme auftreten, helfen Ihnen die Informationen zum Beheben von häufigen Problemen im [Leitfaden für die Problembehandlung](../../troubleshoot.md) weiter.
