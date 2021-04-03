---
title: Referenzleitfaden zu Azure Active Directory-Vorgängen
description: In diesem Referenzleitfaden werden die Prüfungen und Aktionen beschrieben, die Sie zum Schutz und zur Pflege von Identitäts- und Zugriffsverwaltung, Authentifizierung, Governance und Vorgängen durchführen sollten.
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: c815256e60e5ca6582220d247d587a4488c4a55a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "74534770"
---
# <a name="azure-active-directory-operations-reference-guide"></a>Referenzleitfaden zu Azure Active Directory-Vorgängen

In diesem Referenzleitfaden zu Vorgängen werden die Überprüfungen und Aktionen beschrieben, die Sie zum Schutz und zur Pflege der folgenden Bereiche durchführen sollten:

- **[Identitäts- und Zugriffsverwaltung](active-directory-ops-guide-iam.md)**: Verwalten des Lebenszyklus von Identitäten und deren Berechtigungen.
- **[Authentifizierungsverwaltung](active-directory-ops-guide-auth.md)**: Verwalten von Anmeldeinformationen, Definieren der Authentifizierungsumgebung, Delegatzuweisung, Messen der Nutzung und Definieren von Zugriffsrichtlinien basierend auf dem Sicherheitsstatus Ihres Unternehmens.
- **[Governance](active-directory-ops-guide-govern.md)**: Bewerten und Bestätigen des Zugriffs für privilegierte und nicht privilegierte Identitäten sowie zum Überwachen und Steuern von Änderungen an der Umgebung.
- **[Vorgänge](active-directory-ops-guide-ops.md)**: Optimieren der Azure Active Directory (Azure AD)-Vorgänge.

Einige der hier aufgeführten Empfehlungen gelten u. U. nicht für alle Kundenumgebungen. Zum Beispiel werden die bewährten AD FS-Methoden wahrscheinlich nicht angewendet, wenn Ihre Organisation die Kennworthashsynchronisierung verwendet.

> [!NOTE]
> Diese Empfehlungen sind auf dem Stand des Veröffentlichungsdatums, können sich aber im Laufe der Zeit ändern. Organisationen sollten ihre Identitätsmethoden fortlaufend überprüfen, da die Produkte und Dienste von Microsoft mit der Zeit weiterentwickelt werden. Empfehlungen können sich ändern, wenn Organisationen eine andere Azure AD Premium-Lizenz abonnieren. Beispielsweise gibt es für Azure AD Premium P2 weitere Governanceempfehlungen.

## <a name="stakeholders"></a>Projektbeteiligte

Jeder Abschnitt in diesem Referenzhandbuch empfiehlt die Zuweisung von Projektbeteiligten, um Schlüsselaufgaben erfolgreich zu planen und zu implementieren. In der folgenden Tabelle finden Sie die Liste aller Projektbeteiligten in diesem Handbuch:

| Projektbeteiligter | BESCHREIBUNG |
| :- | :- |
| Team für IAM-Vorgänge (Identity & Access Management, Identitäts- und Zugriffsverwaltung) | Dieses Team kümmert sich um die Verwaltung der täglichen Vorgänge des Identitäts- und Zugriffsverwaltungssystems. |
| Produktivitätsteam | Dieses Team besitzt und verwaltet Produktivitätsanwendungen wie E-Mail, Dateifreigabe und Zusammenarbeit, Instant Messaging und Konferenzen. |
| Anwendungsbesitzer | Dieses Team besitzt die spezielle Anwendung von einem Unternehmen und verfügt in der Regel in einer Organisation über eine technische Sicht. |
| Team für die Informationssicherheitsarchitektur | Dieses Team plant und entwirft die Praktiken zur Informationssicherheit in einer Organisation. |
| Team für Informationssicherheitvorgänge | Dieses Team führt die vom Team für die Informationssicherheitsarchitektur implementierten Informationssicherheitspraktiken aus und überwacht diese. |

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit den [Überprüfungen und Aktionen für die Identitäts- und Zugriffsverwaltung](active-directory-ops-guide-iam.md).
