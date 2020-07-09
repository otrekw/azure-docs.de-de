---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/15/2020
ms.author: cephalin
ms.openlocfilehash: 0fccb1ed092190f88d197c9bf2cd17678a4d1731
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83649055"
---
Die Plattformkomponenten von App Service werden aktiv geschützt und gehärtet. Zu diesen Komponenten zählen unter anderem virtuelle Azure-Computer, Speicher, Netzwerkverbindungen und Webframeworks sowie Verwaltungs- und Integrationsfeatures. Die Konformität von App Service wird kontinuierlich und streng geprüft, um Folgendes zu gewährleisten:

- Ihre App-Ressourcen sind zu ihrem [Schutz](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) von den Azure-Ressourcen anderer Kunden isoliert.
- [VM-Instanzen und Laufzeitsoftware werden regelmäßig aktualisiert](../articles/app-service/overview-patch-os-runtime.md), um auf neu entdeckte Sicherheitsrisiken zu reagieren. 
- Die Weitergabe von Geheimnissen (etwa Verbindungszeichenfolgen) zwischen Ihrer App und anderen Azure-Ressourcen (etwa [SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)) erfolgt innerhalb von Azure und überschreitet keine Netzwerkgrenzen. Gespeicherte Geheimnisse werden immer verschlüsselt.
- Die gesamte Kommunikation über die App Service-Verbindungsfeatures (beispielsweise [Hybridverbindung](../articles/app-service/app-service-hybrid-connections.md)) wird verschlüsselt. 
- Verbindungen mit Remoteverwaltungstools wie Azure PowerShell, Azure CLI, Azure SDKs und REST-APIs werden jeweils verschlüsselt.
- Das Bedrohungsmanagement schützt die Infrastruktur und die Plattform rund um die Uhr vor Schadsoftware sowie vor DDoS-Angriffen (Distributed Denial of Service), MITM-Angriffen (Man in the Middle) und anderen Bedrohungen.

Weitere Informationen zur Infrastruktur und Plattform-Sicherheit in Azure finden Sie unter [Azure Trust Center](https://azure.microsoft.com/overview/trusted-cloud/).