---
title: Best Practices für den Zugriff mit den geringsten Berechtigungen auf Azure AD – Microsoft Identity Platform
description: Informationen über eine Reihe von Best Practices und allgemeiner Leitfaden für das Prinzip der geringsten Berechtigungen.
services: active-directory
author: Chrispine-Chiedo
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/26/2021
ms.custom: template-concept
ms.author: cchiedo
ms.reviewer: yuhko, saumadan, marsma
ms.openlocfilehash: db2eb2dcda894b997f485795ab62d983966a7f2f
ms.sourcegitcommit: eb20dcc97827ef255cb4ab2131a39b8cebe21258
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2021
ms.locfileid: "111372756"
---
# <a name="best-practices-for-least-privileged-access-for-applications"></a>Best Practices für den Zugriff mit den geringsten Berechtigungen für Anwendungen

Das Prinzip der geringsten Berechtigungen ist ein Informationssicherheitskonzept, das vorsieht, Benutzern und Anwendungen die geringstmöglichen Zugriffsberechtigungen zu gewähren, die zum Ausführen der erforderlichen Aufgaben erforderlich sind. Wenn Sie das Prinzip der geringsten Berechtigungen verstehen, können Sie vertrauenswürdige Anwendungen für Ihre Kunden erstellen.

Die geringsten Berechtigungen zu verwenden ist mehr als nur eine gute Sicherheitsmaßnahme. Das Konzept hilft Ihnen dabei, die Integrität und Sicherheit Ihrer Daten zu gewährleisten. Es trägt auch zur Geheimhaltung Ihrer Daten bei und verringert Risiken, indem verhindert wird, dass Anwendungen mehr Zugriff auf Daten haben als unbedingt benötigt. Allgemein betrachtet ist das Prinzip der geringsten Berechtigungen ein Teil der [Zero Trust-Philosophie](https://www.microsoft.com/security/business/zero-trust) für proaktive Sicherheit in Unternehmen.

In diesem Artikel werden einige Best Practices beschrieben, mit denen Sie das Prinzip der geringsten Berechtigungen umsetzen können, um Ihre Anwendungen für Endbenutzer sicherer zu machen. Es werden folgende Aspekte des Prinzips der geringsten Berechtigungen behandelt:
- Wie das Zusammenspiel von Einwilligungen mit Zugriffsberechtigungen funktioniert
- Was es bedeutet, wenn eine App überprivilegiert oder geringstmöglich privilegiert ist
- Wie Entwickler mit dem Prinzip der geringsten Berechtigungen umgehen sollten
- Wie Organisationen mit dem Prinzip der geringsten Berechtigungen umgehen sollten

## <a name="using-consent-to-control-access-permissions-to-data"></a>Verwenden von Einwilligung zum Steuern von Zugriffsberechtigungen auf Daten

Für den Zugriff auf geschützte Daten ist die [Einwilligung](../develop/application-consent-experience.md#consent-and-permissions) des Endbenutzers erforderlich. Wenn eine Anwendung auf dem Gerät Ihres Benutzers Zugriff auf geschützte Daten anfordert, sollte sie nach der Einwilligung des Benutzers fragen, bevor sie Zugang zu den geschützten Daten gewährt. Der Endbenutzer muss die Einwilligung für die angeforderte Berechtigung erteilen (oder verweigern), bevor die Anwendung fortfahren kann. Als Anwendungsentwickler ist es am besten, die am geringsten privilegierte Zugangsberechtigung anzufordern.

:::image type="content" source="./media/least-privilege-best-practice/api-permissions.png" alt-text="Screenshot des Bereichs „API-Berechtigungen“ einer App-Registrierung im Azure-Portal":::

## <a name="overprivileged-and-least-privileged-applications"></a>Überprivilegierte und geringstmöglich privilegierte Anwendungen

Eine überprivilegierte Anwendung kann eines der folgenden Merkmale aufweisen:
- **Ungenutzte Berechtigungen:** Eine Anwendung hat ungenutzte Berechtigungen, wenn die von ihr ausgeführten API-Aufrufe nicht alle ihr gewährten Berechtigungen nutzen. Beispielsweise liest eine App in [MS Graph](/graph/overview) möglicherweise nur OneDrive-Dateien (mit der Berechtigung „*Files.Read.All*“), hat aber auch die Berechtigung „*Calendars.Read*“ erhalten, obwohl sie nicht die Kalender-API nutzt.
- **Reduzierbare Berechtigungen**: Einer App wurde eine Berechtigung erteilt, um einen API-Aufruf durchzuführen, der auch mit einer geringer berechtigten Alternative funktioniert. Beispielsweise kann eine App, die nur Benutzerprofile liest, aber über „*User.ReadWrite.All*“ verfügt, als überprivilegiert betrachtet werden. In diesem Fall sollte der App stattdessen „*User.Read.All*“ erteilt werden. Dies ist die geringstmögliche Berechtigung zum ausführen der Anforderung.

Auf eine Anwendung, die als geringstmöglich privilegiert angesehen werden kann, trifft Folgendes zu:
- **Gerade genug Berechtigungen**: Erteilen Sie nur den geringstmöglichen Satz an Berechtigungen, der von einem Endbenutzer einer Anwendung, eines Diensts oder eines Systems zum Ausführen der erforderlichen Aufgaben benötigt wird.

## <a name="approaching-least-privilege-as-an-application-developer"></a>Anwendungsentwickler und das Prinzip der geringsten Rechte

Als Entwickler sind Sie für die Sicherheit der Daten Ihrer Kunden mitverantwortlich. Bei der Entwicklung Ihrer Anwendungen müssen Sie dem Prinzip der geringsten Rechte folgen. Es wird empfohlen, folgende Prinzipien zu beachten, um zu verhindern, dass Ihre Anwendung überprivilegiert ist:
- Die Berechtigungen, die für die von Ihrer Anwendung auszuführenden API-Aufrufe erforderlich sind, vollständig verstehen.
- Die am geringsten privilegierte Berechtigung für die jeweiligen API-Aufrufe Ihrer App kennen. Dazu den [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) verwenden.
- Die entsprechenden [Berechtigungen](/graph/permissions-reference) mit den geringsten bis hin zu den meisten Privilegien finden.
- Wenn Ihre App API-Aufrufe mit überlappenden Berechtigungen ausführt, alle doppelten Berechtigungssätze entfernen.
- Nur den Berechtigungssatz mit den geringsten Berechtigungen auf Ihre Anwendung anwenden, indem Sie die Berechtigung mit den geringsten Berechtigungen in der Berechtigungsliste auswählen.

## <a name="approaching-least-privilege-as-an-organization"></a>Umgang mit dem Prinzip der geringsten Berechtigungen durch Organisationen

Organisationen ändern vorhandene Anwendungen oft ungern, da dies die Geschäftsabläufe beeinträchtigen kann. Dies stellt jedoch ein Problem dar, wenn bereits erteilte Berechtigungen überprivilegiert sind und widerrufen werden müssen. Für Organisationen lohnt es sich, Berechtigungen regelmäßig zu überprüfen. Es wird empfohlen, folgende Prinzipien zu beachten, damit Ihre Anwendungen sicher bleiben:
- Die von Ihren Anwendungen vorgenommenen API-Aufrufe evaluieren.
- Den [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) und die [Microsoft Graph](/graph/overview)-Dokumentation verwenden, um die erforderlichen und am wenigsten privilegierten Berechtigungen zu ermitteln.
- Die Berechtigungen überwachen, die Benutzern oder Anwendungen gewährt werden.
- Ihre Anwendungen aktualisieren, sodass sie den am wenigsten privilegierten Berechtigungssatz verwenden.
- Berechtigungen regelmäßig überprüfen, um sicherzustellen, dass alle autorisierten Berechtigungen weiterhin relevant sind.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Einwilligung und Berechtigungen in Azure Active Directory finden Sie unter [Grundlegendes zu Einwilligungen für Azure AD-Anwendungen](../develop/application-consent-experience.md).
- Weitere Informationen zu Berechtigungen und Einwilligung in Microsoft Identity finden Sie unter [Berechtigungen und Zustimmung auf der Microsoft Identity Platform](../develop/v2-permissions-and-consent.md).
- Weitere Informationen zu Zero Trust finden Sie unter [Zero Trust-Bereitstellungscenter](/security/zero-trust/).
