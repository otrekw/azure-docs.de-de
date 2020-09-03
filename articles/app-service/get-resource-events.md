---
title: Abrufen von Ressourcenereignissen in Azure App Service
description: Erfahren Sie, wie Sie Ressourcenereignisse durch Aktivitätsprotokolle und Event Grid in Ihre App Service-App abrufen.
ms.topic: article
ms.date: 04/24/2020
ms.author: msangapu
ms.openlocfilehash: c5c84891187c540c0b24162cf5c8c7f96e9e731a
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88962297"
---
# <a name="get-resource-events-in-azure-app-service"></a>Abrufen von Ressourcenereignissen in Azure App Service

Azure App Service bietet integrierte Tools zum Überwachen des Status und der Integrität Ihrer Ressourcen. Ressourcenereignissen helfen Ihnen beim Verständnis aller Änderungen, die an Ihren zugrunde liegenden Web-App-Ressourcen vorgenommen wurden, und dabei, gegebenenfalls Maßnahmen zu ergreifen. Zu den Ereignisbeispielen zählen: Skalieren von Instanzen, Aktualisierungen von Anwendungseinstellungen, Neustarten der Web-App und vieles mehr. In diesem Artikel erfahren Sie, wie Sie [Azure-Aktivitätsprotokolle](../azure-monitor/platform/activity-log.md#view-the-activity-log) anzeigen und [Event Grid](../event-grid/index.yml) aktivieren, um Ressourcenereignisse im Zusammenhang mit Ihrer App Service-Web-App zu überwachen.

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

[Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen.](../azure-monitor/platform/activity-log.md#view-the-activity-log)

## <a name="ship-activity-logs-to-event-grid"></a>Senden von Aktivitätsprotokollen an Event Grid

Obwohl Aktivitätsprotokolle benutzerbasiert sind, gibt es eine neue [Event Grid](../event-grid/index.yml)-Integration in App Service (Vorschau), die sowohl Benutzeraktionen als auch automatisierte Ereignisse protokolliert. In Event Grid können Sie einen Handler konfigurieren, der auf besagte Ereignisse reagiert. Verwenden Sie Event Grid z.B. zum automatischen Auslösen einer serverlosen Funktion, um jedes Mal, wenn ein neues Foto einem Blob Storage-Container hinzugefügt wird, eine Bildanalyse durchzuführen.

Alternativ dazu können Sie Event Grid mit Logic Apps verwenden, um Daten überall verarbeiten zu können, ohne Code schreiben zu müssen. Event Grid verknüpft Datenquellen und Ereignishandler. Verwenden Sie Event Grid z.B. zum automatischen Auslösen einer serverlosen Funktion, um jedes Mal, wenn ein neues Foto einem Blob Storage-Container hinzugefügt wird, eine Bildanalyse durchzuführen.

[Anzeigen der Eigenschaften und des Schemas für Azure App Service-Ereignisse.](../event-grid/event-schema-app-service.md)

## <a name="next-steps"></a><a name="nextsteps"></a> Nächste Schritte
* [Abfrageprotokolle mit Azure Monitor](../azure-monitor/log-query/log-query-overview.md)
* [How to Monitor Azure App Service (Vorgehensweise: Überwachen von Azure App Service)](web-sites-monitor.md)
* [Troubleshooting Azure App Service in Visual Studio (Problembehandlung für Azure App Service in Visual Studio)](troubleshoot-dotnet-visual-studio.md)
* [Analyze app Logs in HDInsight (Analyse von App-Protokollen in HDInsight)](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)