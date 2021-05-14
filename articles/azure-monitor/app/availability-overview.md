---
title: Übersicht über die Verfügbarkeit von Application Insights
description: Richten Sie wiederkehrende Webtests ein, um die Verfügbarkeit und Reaktionsfähigkeit Ihrer App oder Website zu überwachen.
ms.topic: conceptual
ms.date: 04/15/2021
ms.openlocfilehash: 3c2d248df98fcb9f75f5de9c05b9f600955cbac7
ms.sourcegitcommit: 950e98d5b3e9984b884673e59e0d2c9aaeabb5bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2021
ms.locfileid: "107600793"
---
# <a name="availability-tests-overview"></a>Übersicht über Verfügbarkeitstests

Nachdem Sie die Web-App/Website bereitgestellt haben, können Sie regelmäßige Tests einrichten, um die Verfügbarkeit und Reaktionszeit zu überwachen. [Application Insights](./app-insights-overview.md) sendet regelmäßig Webanforderungen von verschiedenen Punkten auf der ganzen Welt an Ihre Anwendung. Außerdem kann dieser Dienst Sie warnen, wenn Ihre Anwendung nicht oder zu langsam reagieren sollte.

Sie können für jeden HTTP- oder HTTPS-Endpunkt, der über das öffentliche Internet zugänglich ist, Verfügbarkeitstests einrichten. Sie müssen keinerlei Änderungen an der Website vornehmen, die Sie testen. Sie müssen nicht einmal der Eigentümer der Website sein. Sie können die Verfügbarkeit einer REST-API testen, von der Ihr Dienst abhängig ist.

## <a name="types-of-availability-tests"></a>Arten von Verfügbarkeitstests

Es gibt drei Arten von Verfügbarkeitstests:

* [URL-Pingtest](monitor-web-app-availability.md): Diese Kategorie enthält zwei einfache Tests, die Sie über das Portal erstellen können.
* [Multi-step web test (Mehrstufiger Webtest):](availability-multistep.md) Eine Aufzeichnung einer Sequenz von Webanforderungen, die wiedergegeben werden kann, um komplexere Szenarios zu testen. Mehrstufige Webtests werden in Visual Studio Enterprise erstellt und zur Ausführung im Portal hochgeladen.
* [Custom Track Availability Tests (Benutzerdefinierte Tests zum Nachverfolgen der Verfügbarkeit):](/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) Wenn Sie eine benutzerdefinierte Anwendung zum Ausführen von Verfügbarkeitstests erstellen möchten, können Sie die `TrackAvailability()`-Methode verwenden, um die Ergebnisse an Application Insights zu senden.

> [!IMPORTANT]
> Sowohl der [URL-Pingtest](monitor-web-app-availability.md) als auch der [mehrstufige Webtest](availability-multistep.md) basieren auf der DNS-Infrastruktur des öffentlichen Internet, um die Domänennamen der getesteten Endpunkte aufzulösen. Wenn Sie privates DNS verwenden, müssen Sie daher sicherstellen, dass jeder Domänenname des Tests auch durch die öffentlichen Domänennamenserver aufgelöst werden kann. Wenn dies nicht möglich ist, können Sie stattdessen [benutzerdefinierte Tests zum Nachverfolgen der Verfügbarkeit](/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) verwenden.

**Sie können bis zu 100 Verfügbarkeitstests pro Application Insights-Ressource erstellen.**

## <a name="troubleshooting"></a>Problembehandlung

[Artikel zur Problembehandlung](troubleshoot-availability.md) (Dedicated)

## <a name="next-step"></a>Nächster Schritt

* [Availability Alerts (Verfügbarkeitswarnungen)](availability-alerts.md)
* [Multi-step web tests (Mehrstufige Webtests)](availability-multistep.md)
* [URL-Tests](monitor-web-app-availability.md)
* [Erstellen und Ausführen von benutzerdefinierten Verfügbarkeitstests mit Azure Functions.](availability-azure-functions.md)