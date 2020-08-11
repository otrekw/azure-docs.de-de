---
title: Problembehandlung bei der Azure Data Factory-Benutzeroberfläche
description: Hier erfahren Sie, wie Probleme bei der Azure Data Factory (ADF)-Benutzeroberfläche behandelt werden können.
services: data-factory
author: ceespino
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 8/03/2020
ms.author: ceespino
ms.reviewer: daperlov
ms.openlocfilehash: 9a96c385c25eea1c5e217665f162de402a73e558
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87567818"
---
# <a name="troubleshoot-azure-data-factory-ux-issues"></a>Problembehandlung bei der Azure Data Factory-Benutzeroberfläche
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden die gängigen Problembehandlungsmethoden für die Azure Data Factory (ADF)-Benutzeroberfläche beschrieben.

## <a name="adf-ux-not-loading"></a>ADF-Benutzeroberfläche wird nicht geladen

> [!NOTE]
> Die Azure Data Factory-Benutzeroberfläche unterstützt offiziell Microsoft Edge und Google Chrome. Die Verwendung anderer Webbrowser kann zu unerwartetem oder nicht dokumentiertem Verhalten führen.

### <a name="third-party-cookies-blocked"></a>Drittanbietercookies blockiert

In der ADF-Benutzeroberfläche werden Browsercookies verwendet, um die Benutzersitzung dauerhaft zu speichern sowie interaktive Entwicklungs- und Überwachungsumgebungen zu aktivieren. Es ist möglich, dass Ihr Browser Drittanbietercookies blockiert, weil Sie eine Inkognitositzung verwenden oder einen Werbeblocker aktiviert haben. Das Blockieren von Drittanbietercookies kann beim Laden des Portals zu Problemen führen. So werden Sie beispielsweise zu einer leeren Seite umgeleitet https://adf.azure.com/accesstoken.html oder erhalten eine Warnmeldung mit der Information, dass Drittanbietercookies blockiert werden. Aktivieren Sie zur Behebung dieses Problems die Optionen für Drittanbietercookies in Ihrem Browser mit den folgenden Schritten:

### <a name="google-chrome"></a>Google Chrome

#### <a name="allow-all-cookies"></a>Alle Cookies zulassen

1. Navigieren Sie in Ihrem Browser zu **chrome://settings/cookies**.
1. Wählen Sie die Option für **Alle Cookies zulassen** (![Chrome-Allow-All-Cookies](media/data-factory-ux-troubleshoot-guide/chrome-allow-all-cookies.png)) aus.
1. Aktualisieren Sie die ADF-Benutzeroberfläche, und versuchen Sie es erneut.

#### <a name="only-allow-adf-ux-to-use-cookies"></a>Die Verwendung von Cookies nur für die ADF-Benutzeroberfläche zulassen
Wenn Sie nicht alle Cookies zulassen möchten, können Sie sie optional nur für die ADF-Benutzeroberfläche zulassen:
1. Navigieren Sie zu **chrome://settings/cookies**.
1. Klicken Sie auf **Hinzufügen** und unter **Sites that can always use cookies** (Websites, die immer Cookies verwenden können) auf die Option ![Add ADF UX to allowed sites](media/data-factory-ux-troubleshoot-guide/chrome-only-adf-cookies-1.png) (ADF-Benutzeroberfläche zugelassenen Websites hinzufügen).
1. Fügen Sie die Website **adf.azure.com** hinzu, aktivieren Sie die Option **Alle Cookies**, und speichern Sie. ![Alle Cookies von der Website der ADF-Benutzeroberfläche zulassen](media/data-factory-ux-troubleshoot-guide/chrome-only-adf-cookies-2.png)
1. Aktualisieren Sie die ADF-Benutzeroberfläche, und versuchen Sie es erneut.


### <a name="microsoft-edge"></a>Microsoft Edge

1. Navigieren Sie in Ihrem Browser zu **edge://settings/content/cookies**.
1. Vergewissern Sie sich, dass **Allow sites to save and read cookie data** (Für Websites das Speichern und Lesen von Cookiedaten zulassen) aktiviert und die Option **Cookies von Drittanbietern blockieren** deaktiviert ist. ![Alle Cookies in Edge zulassen](media/data-factory-ux-troubleshoot-guide/edge-allow-all-cookies.png)
1. Aktualisieren Sie die ADF-Benutzeroberfläche, und versuchen Sie es erneut.

#### <a name="only-allow-adf-ux-to-use-cookies"></a>Die Verwendung von Cookies nur für die ADF-Benutzeroberfläche zulassen

Wenn Sie nicht alle Cookies zulassen möchten, können Sie sie optional nur für die ADF-Benutzeroberfläche zulassen:

1. Navigieren Sie zu **edge://settings/content/cookies**.
1. Klicken Sie im Abschnitt **Zulassen** auf **Hinzufügen**, und fügen Sie die Website **adf.azure.com** hinzu. ![Hinzufügen der ADF-Benutzeroberfläche zu zugelassenen Websites](media/data-factory-ux-troubleshoot-guide/edge-allow-adf-cookies.png)
1. Aktualisieren Sie die ADF-Benutzeroberfläche, und versuchen Sie es erneut.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:

* [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
* [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
* [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
* [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
* [Azure-Videos](https://azure.microsoft.com/resources/videos/index/)
* [Frageseite von Microsoft Q&A (Fragen und Antworten)](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
