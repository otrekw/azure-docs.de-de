---
title: Bereitstellen einer Azure App Service-App mit Visual Studio 2015 | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Web-Apps, API-Apps oder mobile Apps mithilfe von Visual Studio 2015 und dem Azure SDK in Azure Deutschland bereitgestellt werden.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2019
ms.author: ralfwi
ms.openlocfilehash: 03533fb96a5585ed4f62d1731ba0bc1fdbd4cde2
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88080465"
---
# <a name="deploy-an-azure-app-service-app-by-using-visual-studio-2015"></a>Bereitstellen einer Azure App Service-App mit Visual Studio 2015

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

Dieser Artikel beschreibt, wie Azure App Service-Apps (API-Apps, Web-Apps, mobile Apps) mithilfe von Visual Studio 2015 in Azure Deutschland bereitgestellt werden.

## <a name="prerequisites"></a>Voraussetzungen
* Informieren Sie sich über die [Voraussetzungen in Visual Studio](../app-service/quickstart-dotnetcore.md#prerequisites) für die Installation und Konfiguration von Visual Studio 2015 und dem Azure SDK.
* Konfigurieren Sie Visual Studio anhand [dieser Anweisungen](./germany-get-started-connect-with-vs.md) für das Herstellen einer Verbindung mit einem Azure Deutschland-Konto. 

## <a name="open-an-app-project-in-visual-studio"></a>Öffnen des App-Projekts in Visual Studio
Öffnen Sie eine vorhandene App-Projektmappe oder ein Projekt in Visual Studio, oder [erstellen Sie ein Projekt](../app-service/quickstart-dotnet-framework.md#create-and-publish-the-web-app). Führen Sie die App dann in Visual Studio aus, um sicherzustellen, dass sie lokal funktioniert.

## <a name="deploy-to-azure-germany"></a>Bereitstellen in Azure Deutschland
Nachdem Sie Visual Studio für das Herstellen der Verbindung mit Ihrem Azure Deutschland-Konto konfiguriert haben (was Sie bereits als Voraussetzung erledigt haben), können Sie zum Bereitstellen einer App Service-App genau so vorgehen, wie es in den entsprechenden [Anweisungen für die globale Azure-Umgebung](../app-service/quickstart-dotnetcore.md) beschrieben ist.

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen einer ASP.NET-Web-App für Azure App Service mit Visual Studio](../app-service/quickstart-dotnetcore.md)
* Allgemeine App Service-Informationen finden Sie unter [App Service – API-Apps-Dokumentation](../app-service/index.yml).
* Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.
