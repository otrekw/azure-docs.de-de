---
title: Abrufen von Ressourcenereignissen in Azure App Service
description: Erfahren Sie, wie Sie Ressourcenereignisse durch Aktivitätsprotokolle und Event Grid in Ihre App Service-App abrufen.
ms.topic: article
ms.date: 04/24/2020
ms.author: msangapu
ms.openlocfilehash: 7075e3eacc85198e22a9aa0e53f67a22416b2678
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649074"
---
# <a name="get-resource-events-in-azure-app-service"></a>Abrufen von Ressourcenereignissen in Azure App Service

Azure App Service bietet integrierte Tools zum Überwachen des Status und der Integrität Ihrer Ressourcen. Ressourcenereignissen helfen Ihnen beim Verständnis aller Änderungen, die an Ihren zugrunde liegenden Web-App-Ressourcen vorgenommen wurden, und dabei, gegebenenfalls Maßnahmen zu ergreifen. Zu den Ereignisbeispielen zählen: Skalieren von Instanzen, Aktualisierungen von Anwendungseinstellungen, Neustarten der Web-App und vieles mehr. In diesem Artikel erfahren Sie, wie Sie [Azure-Aktivitätsprotokolle](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) anzeigen und [Event Grid](https://docs.microsoft.com/azure/event-grid/) aktivieren, um Ressourcenereignisse im Zusammenhang mit Ihrer App Service-Web-App zu überwachen.

> [!NOTE]
> Die Integration von App Sich in Event Grid befindet sich in der **Vorschauphase**. [Weitere Details finden Sie in der Ankündigung.](https://aka.ms/app-service-event-grid-announcement)
>

## <a name="view-azure-activity-logs"></a>Anzeigen von Azure-Aktivitätsprotokollen
Azure-Aktivitätsprotokolle enthalten Ressourcenereignisse, die von Vorgängen ausgegeben werden, die an Ressourcen in Ihrem Abonnement ausgeführt werden. Sowohl die Benutzeraktionen im Azure-Portal als auch Azure Resource Manager-Vorlagen tragen zu den Ereignissen bei, die vom Aktivitätsprotokoll erfasst werden. 

Azure-Aktivitätsprotokolle erfassen für App Service Details wie:
- welche Vorgänge an den Ressourcen vorgenommen wurden (z. B.: App Service-Pläne)
- Den Benutzer oder das System, von dem der Vorgang initiiert wurde
- Wann der Vorgang abgeschlossen wurde
- Den Status des Vorgangs
- Eigenschaftswerte, die bei der Untersuchung des Vorgangs helfen

### <a name="what-can-you-do-with-azure-activity-logs"></a>Welche Möglichkeiten bieten Azure-Aktivitätsprotokolle?

Azure-Aktivitätsprotokolle können über das Azure-Portal, die PowerShell, die REST-API oder die CLI abgefragt werden. Sie können die Protokolle an ein Speicherkonto, an Event Hub und an Log Analytics senden. Sie können sie auch in Power BI analysieren oder Benachrichtigungen erstellen, um bei Ressourcenereignissen auf dem aktuellen Stand zu bleiben.

[Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen.](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

## <a name="ship-activity-logs-to-event-grid"></a>Senden von Aktivitätsprotokollen an Event Grid

Obwohl Aktivitätsprotokolle benutzerbasiert sind, gibt es eine neue [Event Grid](https://docs.microsoft.com/azure/event-grid/)-Integration in App Service (Vorschau), die sowohl Benutzeraktionen als auch automatisierte Ereignisse protokolliert. In Event Grid können Sie einen Handler konfigurieren, der auf besagte Ereignisse reagiert. Verwenden Sie Event Grid z.B. zum automatischen Auslösen einer serverlosen Funktion, um jedes Mal, wenn ein neues Foto einem Blob Storage-Container hinzugefügt wird, eine Bildanalyse durchzuführen.

Alternativ dazu können Sie Event Grid mit Logic Apps verwenden, um Daten überall verarbeiten zu können, ohne Code schreiben zu müssen. Event Grid verknüpft Datenquellen und Ereignishandler. Verwenden Sie Event Grid z.B. zum automatischen Auslösen einer serverlosen Funktion, um jedes Mal, wenn ein neues Foto einem Blob Storage-Container hinzugefügt wird, eine Bildanalyse durchzuführen.

[Anzeigen der Eigenschaften und des Schemas für Azure App Service-Ereignisse.](https://docs.microsoft.com/azure/event-grid/event-schema-app-service)

## <a name="next-steps"></a><a name="nextsteps"></a> Nächste Schritte
* [Abfrageprotokolle mit Azure Monitor](../azure-monitor/log-query/log-query-overview.md)
* [How to Monitor Azure App Service (Vorgehensweise: Überwachen von Azure App Service)](web-sites-monitor.md)
* [Troubleshooting Azure App Service in Visual Studio (Problembehandlung für Azure App Service in Visual Studio)](troubleshoot-dotnet-visual-studio.md)
* [Analyze app Logs in HDInsight (Analyse von App-Protokollen in HDInsight)](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
