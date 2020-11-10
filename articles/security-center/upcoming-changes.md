---
title: Wichtige bevorstehende Änderungen an Azure Security Center
description: Bevorstehende Änderungen an Azure Security Center, die Sie beachten sollten und für die Sie ggf. planen müssen
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2020
ms.author: memildin
ms.openlocfilehash: 549a95b0b2ffc2b2d2bf5670a961e0454683e33a
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2020
ms.locfileid: "93026716"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Wichtige bevorstehende Änderungen an Azure Security Center

> [!IMPORTANT]
> Die Informationen auf dieser Seite beziehen sich auf Vorabversionen von Produkten oder Features, die vor einer möglichen Veröffentlichung für den Handel wesentlich verändert werden können. Microsoft gewährt in Bezug auf die hier angegebenen Informationen keine Zusicherungen oder Garantien, weder ausdrücklich noch konkludent.

Auf dieser Seite erfahren Sie, welche Änderungen für Security Center geplant sind. Es werden geplante Änderungen am Produkt beschrieben, die sich auf Dinge wie Ihre Sicherheitsbewertung oder Workflows auswirken könnten.

Die neuesten Versionshinweise finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Geplante Änderungen

### <a name="recommendations-related-to-azure-security-benchmark-to-be-added-preview"></a>Empfehlungen im Zusammenhang mit dem Hinzufügen eines Vergleichstests für die Azure-Sicherheit (Vorschau)

| Aspekt | Details |
|---------|---------|
|Ankündigungsdatum | 26. Oktober 2020  |
|Datum dieser Änderung  |  November 2020 |
|Auswirkung     | Möglicherweise sind weitere Empfehlungen zu prüfen.<br>Keine unmittelbaren Auswirkungen auf Sicherheitsbewertung – Empfehlungen der Vorschau wirken sich nicht auf Ihre Sicherheitsbewertung aus.<br>Keine unmittelbare Auswirkung auf den Integritätsstatus Ihrer Ressourcen – Empfehlungen der Vorschau versetzen keine Ressourcen in den Zustand „Fehlerhaft“.|
|  |  |

Beim Azure-Sicherheitsvergleichstest handelt es sich um einen von Microsoft erstellten Satz Azure-spezifischer Richtlinien zu bewährten Methoden für Sicherheit und Compliance, die auf allgemeinen Complianceframeworks basieren. [Weitere Informationen zum Azure-Sicherheitsvergleichstest](../security/benchmarks/introduction.md)

Die folgenden 18 neuen Empfehlungen werden zu Security Center hinzugefügt, um die Abdeckung des Vergleichstests zu erhöhen.

Empfehlungen der Vorschau versetzen keine Ressourcen in einen fehlerhaften Zustand, und sie werden nicht in die Berechnungen Ihrer Sicherheitsbewertung einbezogen. Setzen Sie sie trotzdem um, wann immer möglich, damit sie nach Ablauf des Vorschauzeitraums zu Ihrer Bewertung beitragen. Informationen zum Umgang mit diesen Empfehlungen finden Sie unter [Umsetzen von Empfehlungen in Azure Security Center](security-center-remediate-recommendations.md).

- Azure Backup sollte für virtuelle Computer aktiviert sein
- Erzwingen einer SSL-Verbindung muss für MySQL-Datenbankserver aktiviert sein
- Erzwingen einer SSL-Verbindung muss für PostgreSQL-Datenbankserver aktiviert sein
- Georedundante Sicherung muss für Azure Database for MariaDB aktiviert sein
- Georedundante Sicherung muss für Azure Database for MySQL aktiviert sein
- Georedundante Sicherung muss für Azure Database for PostgreSQL aktiviert sein
- Java sollte für Ihre API-App auf die aktuelle Version aktualisiert werden
- Java sollte für Ihre Funktions-App auf die aktuelle Version aktualisiert werden
- Java sollte für Ihre Web-App auf die aktuelle Version aktualisiert werden
- PHP sollte für Ihre API-App auf die aktuelle Version aktualisiert werden
- PHP sollte für Ihre Web-App auf die aktuelle Version aktualisiert werden
- Privater Endpunkt muss für MariaDB-Server aktiviert sein
- Privater Endpunkt muss für MySQL-Server aktiviert sein
- Privater Endpunkt muss für PostgreSQL-Server aktiviert sein
- Python sollte für Ihre API-App auf die aktuelle Version aktualisiert werden
- Python sollte für Ihre Funktions-App auf die aktuelle Version aktualisiert werden
- Python sollte für Ihre Web-App auf die aktuelle Version aktualisiert werden
- Web-Apps sollten ein SSL-Zertifikat für alle eingehenden Anforderungen anfordern

Verwandte Links:

- [Weitere Informationen zum Vergleichstest für die Azure-Sicherheit](../security/benchmarks/introduction.md)
- [Weitere Informationen zu API-Apps in Azure](../app-service/app-service-web-tutorial-rest-api.md)
- [Weitere Informationen zu Funktions-Apps in Azure](../azure-functions/functions-overview.md)
- [Weitere Informationen zu Web-Apps in Azure](../app-service/overview.md)
- [Weitere Informationen zu Azure Database for MariaDB](../mariadb/overview.md)
- [Weitere Informationen zu Azure Database for MySQL](../mysql/overview.md)
- [Weitere Informationen zu Azure Database for PostgreSQL](../postgresql/overview.md)

## <a name="next-steps"></a>Nächste Schritte

Alle aktuellen Änderungen am Produkt finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).