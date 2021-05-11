---
title: App-Seite für eine Anwendungsproxy-App wird nicht ordnungsgemäß angezeigt
description: Hilfestellung beim Beheben von Problemen mit der Anzeige der Seite für eine in Azure Active Directory integrierte Anwendungsproxyanwendung
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
ms.openlocfilehash: 96a6ddfe6bd6bd8cd742c5a005cb67126c9d7f5e
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108186281"
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Anwendungsseite für eine Anwendungsproxyanwendung nicht richtig angezeigt

Die Informationen in diesem Artikel helfen Ihnen beim Beheben von Problemen mit Azure Active Directory-Anwendungsproxyanwendungen, die auftreten, wenn Sie zur Seite navigieren und die Anzeige dort Fehler aufweist.

## <a name="overview"></a>Übersicht
Wenn Sie eine Anwendungsproxyanwendung veröffentlichen, können Sie beim Zugriff auf die Anwendung nur Seiten in Ihrem Stammordner öffnen. Wenn die Seite nicht ordnungsgemäß angezeigt wird, fehlen der internen Stamm-URL für die Anwendung möglicherweise einige Seitenressourcen. Um dies zu beheben, stellen Sie sicher, dass Sie *alle* Ressourcen für die Seite als Teil Ihrer Anwendung veröffentlicht haben.

Sie können überprüfen, ob fehlende Ressourcen Ursache des Problems sind, indem Sie eine Netzwerküberwachungssoftware öffnen (z.B. Fiddler oder F12-Tools in Internet Explorer/Microsoft Edge), die Seite laden und nach Fehlern des Typs 404 suchen. Diese Fehler geben an, dass die Seiten derzeit nicht gefunden werden können und dass sie veröffentlicht werden müssen.

Ein Beispiel für diesen Fall ist eine Ausgabenanwendung, die mit der internen URL `http://myapps/expenses` veröffentlicht wurde, jedoch das Stylesheet `http://myapps/style.css` verwendet. In diesem Fall wird das Stylesheet nicht in der Anwendung veröffentlicht, sodass das Laden der Ausgabenanwendung einen 404-Fehler beim Laden von „style.css“ auslöst. In diesem Beispiel wird das Problem durch eine Veröffentlichung der Anwendung mit der internen URL `http://myapp/` behoben.

## <a name="problems-with-publishing-as-one-application"></a>Probleme mit der Veröffentlichung in einer einzelnen Anwendung

Wenn eine Veröffentlichung aller Ressourcen innerhalb derselben Anwendung nicht möglich ist, müssen Sie mehrere Anwendungen veröffentlichen und Links zwischen diesen aktivieren.

Zu diesem Zweck empfiehlt sich die Lösung [Benutzerdefinierte Domänen](application-proxy-configure-custom-domain.md). Diese Lösung erfordert jedoch, dass Sie das Zertifikat für Ihre Domäne besitzen und Ihre Anwendungen vollqualifizierte Domänennamen (FQDNs) verwenden. Weitere Optionen finden Sie in der [Dokumentation zur Problembehandlung bei fehlerhaften Links](application-proxy-page-links-broken-problem.md).

## <a name="next-steps"></a>Nächste Schritte
[Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](../app-proxy/application-proxy-add-on-premises-application.md)
