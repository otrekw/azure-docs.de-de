---
title: Bericht „Riskante Anmeldungen“ im Portal | Microsoft-Dokumentation
description: Enthält Informationen zum Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal.
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/04/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: b77486064139895799ac5a48327377154f75da6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273836"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal

Azure Active Directory (Azure AD) erkennt verdächtige Aktionen im Zusammenhang mit Ihren Benutzerkonten. Für jede erkannte Aktion wird ein Datensatz mit der Bezeichnung **Risikoerkennung** erstellt. Weitere Details finden Sie unter [Azure Active Directory-Risikoerkennungen](concept-risk-events.md). 

Sie können über das [Azure-Portal](https://portal.azure.com) auf die Sicherheitsberichte zugreifen. Wählen Sie dazu das Blatt **Azure Active Directory** aus, und navigieren Sie dann zum Abschnitt **Sicherheit**. 

Zwei verschiedene Sicherheitsberichte werden basierend auf den Risikoerkennungen erstellt:

- **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist.

- **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. 

![Riskante Anmeldungen](./media/concept-risky-sign-ins/10.png)

Weitere Informationen zum Konfigurieren der Richtlinien, die diese Risikoerkennungen auslösen, finden Sie unter [Gewusst wie: Konfigurieren der Richtlinie zum Benutzerrisiko](../identity-protection/howto-user-risk-policy.md).  

## <a name="who-can-access-the-risky-sign-ins-report"></a>Wer hat Zugriff auf den Bericht über riskante Anmeldungen?

Die Berichte über riskante Anmeldungen stehen für Benutzer mit den folgenden Rollen zur Verfügung:

- Sicherheitsadministrator
- Globaler Administrator
- Sicherheitsleseberechtigter

Anweisungen zum Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Welche Azure AD-Lizenz benötigen Sie für den Zugriff auf einen Sicherheitsbericht?  

In allen Editionen von Azure AD stehen Sicherheitsberichte zu riskanten Anmeldungen zur Verfügung. Die Granularitätsebene von Berichten kann für die einzelnen Editionen aber variieren: 

- In der **Edition Azure Active Directory Free** erhalten Sie eine Liste mit riskanten Anmeldungen. 

- Darüber hinaus können Sie mit der Edition **Azure Active Directory Premium 1** einige zugrunde liegende Risikoerkennungen untersuchen, die für die einzelnen Berichte erkannt wurden. 

- In der Edition **Azure Active Directory Premium 2** erhalten Sie die ausführlichsten Informationen zu allen zugrunde liegenden Risikoerkennungen, und Sie können Sicherheitsrichtlinien konfigurieren, mit denen automatisch auf konfigurierte Risikostufen reagiert wird.

## <a name="risky-sign-ins-report-for-azure-ad-free-edition"></a>Bericht „Riskante Anmeldungen“ für die Edition Azure AD Free

In der Edition Azure AD Free wird Ihnen eine Liste mit riskanten Anmeldungen zur Verfügung gestellt, die für Ihre Benutzer erkannt wurden. Jeder Datensatz enthält die folgenden Attribute:

- **Benutzer**: Der Name des Benutzers, der während des Anmeldevorgangs verwendet wurde.
- **IP**: Die IP-Adresse des Geräts, die für die Verbindung mit Azure Active Directory verwendet wurde.
- **Speicherort**: Der für das Herstellen der Verbindung mit Azure Active Directory verwendete Speicherort. Dies ist eine auf Ablaufverfolgungen, Registrierungsdaten, Reverse-Lookups und anderen Informationen basierende „Beste Leistung“-Näherung.
- **Zeitpunkt der Anmeldung**: Die Uhrzeit, zu der die Anmeldung erfolgte
- **Status**: Der Status der Anmeldung

![Riskante Anmeldungen](./media/concept-risky-sign-ins/01.png)

Basierend auf Ihrer Untersuchung der riskanten Anmeldung können Sie Azure AD anhand der folgenden Aktionen Feedback bereitstellen:

- Beheben
- Als falsch positiv markieren
- Ignorieren
- Erneut aktivieren

![Riskante Anmeldungen](./media/concept-risky-sign-ins/21.png)

In diesem Bericht stehen Ihnen auch folgende Optionen zur Verfügung:

- Durchsuchen von Ressourcen
- Herunterladen der Berichtsdaten

![Riskante Anmeldungen](./media/concept-risky-sign-ins/93.png)

## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Bericht „Riskante Anmeldungen“ für Azure AD Premium

Der Bericht „Riskante Anmeldungen“ in Azure AD Premium enthält Folgendes:

- Aggregierte Informationen zu den erkannten [Risikoerkennungstypen](concept-risk-events.md). Bei der **Azure AD Premium P1-Edition** werden Erkennungen, die nicht durch Ihre Lizenz abgedeckt sind, als Risikoerkennung **Anmeldung mit erhöhtem Risiko erkannt** angezeigt. Die **Azure AD Premium P2-Edition** bietet die ausführlichsten Informationen zu allen zugrunde liegenden erkannten Ereignissen.

- Option zum Herunterladen des Berichts

![Riskante Anmeldungen](./media/concept-risky-sign-ins/456.png)

Wenn Sie eine Risikoerkennung auswählen, erhalten Sie eine ausführliche Berichtsansicht für die Risikoerkennung und haben die folgenden Optionen:

- Option zum Konfigurieren einer [Richtlinie zum Beheben des Benutzerrisikos](../identity-protection/howto-user-risk-policy.md)  

- Überprüfen der Erkennungszeitachse für die Risikoerkennung  

- Überprüfen einer Liste mit den Benutzern, für die diese Risikoerkennung erkannt wurde

- Manuelles Schließen der Risikoerkennungen 

![Riskante Anmeldungen](./media/concept-risky-sign-ins/457.png)

> [!IMPORTANT]
> In einigen Fällen finden Sie möglicherweise eine Risikoerkennung ohne entsprechenden Anmeldungseintrag im [Bericht zu Anmeldeaktivitäten](concept-sign-ins.md). Dies liegt daran, dass Identity Protection das Risiko sowohl für **interaktive** als auch **nicht interaktive** Anmeldungen beurteilt, während der Bericht zu Anmeldeaktivitäten nur die interaktiven Anmeldungen anzeigt.

Wenn Sie einen Benutzer auswählen, erhalten Sie eine ausführliche Berichtsansicht für diesen Benutzer mit folgenden Optionen:

- Öffnen der Ansicht „Alle Anmeldungen“

- Das Kennwort des Benutzers zurücksetzen

- Verwerfen aller Ereignisse

- Untersuchen der gemeldeten Risikoerkennungen für den Benutzer 

![Riskante Anmeldungen](./media/concept-risky-sign-ins/324.png)

Wählen Sie in der Liste eine Risikoerkennung aus, um sie zu untersuchen.  
Das Blatt **Details** wird für die Risikoerkennung geöffnet. Auf dem Blatt **Details** können Sie eine Risikoerkennung manuell schließen oder eine manuell geschlossene Risikoerkennung wieder aktivieren. 

![Riskante Anmeldungen](./media/concept-risky-sign-ins/325.png)

## <a name="next-steps"></a>Nächste Schritte

- [Gewusst wie: Konfigurieren der Richtlinie zum Benutzerrisiko](../identity-protection/howto-user-risk-policy.md)
- [Gewusst wie: Konfigurieren der Richtlinie zum Beheben des Benutzerrisikos](../identity-protection/howto-user-risk-policy.md)
- [Risikoerkennungstypen](concept-risk-events.md)
