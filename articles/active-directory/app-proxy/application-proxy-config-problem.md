---
title: Probleme beim Erstellen einer Azure Active Directory-Anwendungsproxyanwendung
description: Beheben von Problemen beim Erstellen von Anwendungsproxyanwendungen im Azure Active Directory-Verwaltungsportal
services: active-directory
author: kenwith
manager: mtillman
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: troubleshooting
ms.date: 04/27/2021
ms.author: kenwith
ms.reviewer: asteen
ms.openlocfilehash: a3f60e63d4fb5257d7d822a8e1a9326adf9eb62d
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108186562"
---
# <a name="problem-creating-an-application-proxy-application"></a>Problem beim Erstellen einer Anwendungsproxyanwendung 

Nachfolgend sind einige der häufigen Probleme aufgeführt, die beim Erstellen einer neuen Anwendungsproxyanwendung auftreten können.

## <a name="recommended-documents"></a>Empfohlene Dokumente 

Informationen zum Erstellen einer Anwendungsproxyanwendung über das Verwaltungsportal finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-add-on-premises-application.md).

Wenn Sie die Schritte in dieser Dokumentation befolgen und beim Erstellen der Anwendung einen Fehler erhalten, finden Sie weitere Informationen und Vorschläge zur Behebung der Anwendung in den Fehlerdetails. Die meisten Fehlermeldungen enthalten eine empfohlene Problemlösung. 

## <a name="specific-things-to-check"></a>Überprüfen bestimmter Punkte

Stellen Sie Folgendes sicher, um häufige Fehler zu vermeiden:

-   Sie sind ein Administrator mit der Berechtigung zum Erstellen einer Anwendungsproxyanwendung.

-   Die interne URL ist eindeutig.

-   Die externe URL ist eindeutig.

-   Die URLs beginnen mit „http“ oder „https“ und enden mit einem „/“.

-   Die URL muss ein Domänenname, keine IP-Adresse sein.

Die Fehlermeldung sollte beim Erstellen der Anwendung in der oberen rechten Ecke angezeigt werden. Sie können auch das Benachrichtigungssymbol auswählen, um die Fehlermeldungen anzuzeigen.

   ![Benachrichtigungsaufforderung](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Nächste Schritte
[Aktivieren des Anwendungsproxys über das Azure-Portal](application-proxy-add-on-premises-application.md)
