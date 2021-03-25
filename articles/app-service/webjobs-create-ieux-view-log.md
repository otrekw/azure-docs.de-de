---
title: Anzeigen des Protokollverlaufs von WebJobs
description: Hier erfahren Sie, wie Sie den Protokollverlauf für erfolgreiche und nicht erfolgreiche Aufträge anzeigen.
author: ggailey777
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 4b4e421458438aecc7c08e397f1fc919a8cc2225
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743434"
---
# <a name="view-webjob-history-in-the-azure-portal"></a>Anzeigen des WebJob-Verlaufs im Azure-Portal

Hier erfahren Sie, wie Sie den Protokollverlauf für erfolgreiche und nicht erfolgreiche Aufträge anzeigen.

1. Wählen Sie den WebJob aus, für den der Verlauf angezeigt werden soll, und klicken Sie dann auf die Schaltfläche **Protokolle**.

    ![Schaltfläche „Protokolle“](./media/web-sites-create-web-jobs/wjbladelogslink.png)

1. Wählen Sie auf der Seite **Details zum WebJob** einen Zeitpunkt aus, um die Details für eine Ausführung anzuzeigen.

    ![Details zum WebJob](./media/web-sites-create-web-jobs/webjobdetails.png)

1. Wählen Sie auf der Seite **Details zur WebJob-Ausführung** die Option **Ausgabe umschalten**, um den Text der Protokollinhalte anzuzeigen.

    ![Ausführungsdetails zu WebJobs](./media/web-sites-create-web-jobs/webjobrundetails.png)

    Um den Ausgabetext in einem separaten Browserfenster anzuzeigen, klicken Sie auf **Herunterladen**. Um den Text selbst herunterzuladen, klicken Sie mit der rechten Maustaste auf **Herunterladen**, und speichern Sie die Dateiinhalte mithilfe der Browseroptionen.

1. Wählen Sie am oberen Rand der Seite den Breadcrumb-Link **WebJobs** aus, um eine Liste von WebJobs aufzurufen.

    ![WebJob-Breadcrumb](./media/web-sites-create-web-jobs/breadcrumb.png)

    ![Liste der Webaufträge im Verlaufsdashboard](./media/web-sites-create-web-jobs/webjobslist.png)

## <a name="next-steps"></a>Nächste Schritte

* Verwenden des [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) zum Vereinfachen vieler Programmieraufgaben

* [Entwickeln und Bereitstellen von WebJobs mit Visual Studio](webjobs-dotnet-deploy-vs.md)