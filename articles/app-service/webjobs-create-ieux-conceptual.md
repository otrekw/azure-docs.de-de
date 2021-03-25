---
title: WebJob, Hintergrundaufgaben, in Azure
description: Hier finden Sie Informationen zu WebJobs.
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 7714b090399b0b184e2e216ff6da7b10f2bf4386
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452270"
---
# <a name="webjobs-run-background-tasks-in-azure-app-service"></a>WebJobs: Ausführen von Hintergrundaufgaben in Azure App Service

Dieser Artikel zeigt, wie Sie WebJobs mithilfe des [Azure-Portals](https://portal.azure.com) zum Hochladen einer ausführbaren Datei oder eines Skripts bereitstellen. Informationen zum Entwickeln und Bereitstellen von WebJobs mit Visual Studio finden Sie unter [Bereitstellen von WebJobs mit Visual Studio](webjobs-dotnet-deploy-vs.md).

## <a name="overview"></a>Übersicht
WebJobs ist ein Feature von [Azure App Service](index.yml), mit dem Sie ein Programm oder Skript in derselben Instanz wie eine Web-App, API-App oder mobile App ausführen können. Für die Nutzung von WebJobs fallen keine zusätzlichen Kosten an.

> [!IMPORTANT]
> WebJobs wird für Azure App Service für Linux zurzeit noch nicht unterstützt.

Das Azure WebJobs SDK kann in Verbindung mit WebJobs zahlreiche Programmieraufgaben vereinfachen. Weitere Informationen finden Sie unter [Was ist das WebJobs-SDK?](https://github.com/Azure/azure-webjobs-sdk/wiki).

Azure Functions bietet eine weitere Möglichkeit zum Ausführen von Programmen und Skripts. Einen Vergleich zwischen WebJobs und Functions finden Sie unter [Auswählen zwischen Flow, Logic Apps, Functions und WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="webjob-types"></a>WebJob-Typen

Die folgende Tabelle beschreibt die Unterschiede zwischen *fortlaufenden* und *ausgelösten* WebJobs.


|Fortlaufend  |Ausgelöst  |
|---------|---------|
| Werden sofort nach Erstellung des WebJobs gestartet. Damit der Auftrag nicht beendet wird, wird das Programm oder Skript in der Regel in einer Endlosschleife ausgeführt. Wenn der Auftrag beendet ist, können Sie ihn neu starten. | Werden nur gestartet, wenn sie manuell oder nach einem Zeitplan ausgelöst werden. |
| Werden in allen Instanzen ausgeführt, in denen die Web-App ausgeführt wird. Sie können den WebJob wahlweise auf eine einzelne Instanz beschränken. |Werden in einer einzelnen Instanz ausgeführt, die Azure zum Lastenausgleich auswählt.|
| Unterstützen Remotedebuggen. | Bieten keine Unterstützung für Remotedebuggen.|

[!INCLUDE [webjobs-always-on-note](../../includes/webjobs-always-on-note.md)]

## <a name="add-webjob-to-source-control"></a>Hinzufügen eines WebJobs zur Quellcodeverwaltung

Wenn Sie die Quellcodeverwaltung mit Ihrer Anwendung konfiguriert haben, sollten die Webaufträge im Rahmen der Integration der Quellcodeverwaltung bereitgestellt werden. Sobald die Quellcodeverwaltung mit Ihrer Anwendung konfiguriert wurde, kann ein Webauftrag nicht über das Azure-Portal hinzugefügt werden.

## <a name="supported-file-types-for-scripts-or-programs"></a><a name="acceptablefiles"></a>Unterstützte Dateitypen für Skripts oder Programme

Die folgenden Dateitypen werden unterstützt:

* CMD, BAT, EXE (bei Verwendung von Windows-CMD)
* PS1 (bei Verwendung von PowerShell)
* SH (bei Verwendung von Bash)
* PHP (bei Verwendung von PHP)
* PY (bei Verwendung von Python)
* JS (bei Verwendung von Node.js)
* JAR (bei Verwendung von Java)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines WebJobs](./webjobs-create-ieux.md)
* Anzeigen des Protokollverlaufs von [WebJobs](./webjobs-create-ieux-view-log.md)
