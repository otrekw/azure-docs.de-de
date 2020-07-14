---
title: Häufig gestellte Fragen zu FHIR-Diensten in Azure – Azure API for FHIR
description: Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure API for FHIR, wie z. B. Angaben zum Speicherort von Daten hinter FHIR-APIs und zur Versionsunterstützung.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: cdec5ade4fc72fba6fcfba131b69ca9eb373874c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84870980"
---
# <a name="frequently-asked-questions-about-the-azure-api-for-fhir"></a>Häufig gestellte Fragen zu Azure API for FHIR

## <a name="what-is-fhir"></a>Was ist FHIR?
Fast Healthcare Interoperability Resources (FHIR, ausgesprochen wie das englische Wort „Fire“) ist ein Interoperabilitätsstandard, der den Austausch von Patientendaten zwischen verschiedenen Gesundheitssystemen ermöglicht. Dieser Standard wurde von der HL7-Organisation entwickelt und wird von Gesundheitsorganisationen auf der ganzen Welt übernommen. Die aktuelle Version von FHIR ist R4 (Release 4). Azure API for FHIR unterstützt R4 und auch die vorherige Version STU3 (Standard for Trial Use 3). Weitere Informationen zu FHIR finden Sie auf [HL7.org](http://hl7.org/fhir/summary.html).

## <a name="is-the-data-behind-the-fhir-apis-stored-in-azure"></a>Werden die Daten hinter den FHIR-APIs in Azure gespeichert?

Ja. Die Daten werden in verwalteten Datenbanken in Azure gespeichert. Die Azure API for FHIR bietet keinen direkten Zugriff auf den zugrunde liegenden Datenspeicher.

## <a name="what-identity-provider-do-you-support"></a>Welcher Identitätsanbieter wird unterstützt?

Derzeit wird Microsoft Azure Active Directory als Identitätsanbieter unterstützt.

## <a name="what-fhir-version-do-you-support"></a>Welche FHIR-Version wird unterstützt?

Die Versionen 4.0.0 und 3.0.1 werden sowohl in Azure API for FHIR (PaaS) als auch auf dem FHIR-Server für Azure (Open Source) unterstützt.

Ausführliche Informationen finden Sie unter [Unterstützte Features](fhir-features-supported.md). Informationen zu den Änderungen zwischen Versionen finden Sie im [Versionsverlauf für HL7 FHIR](https://hl7.org/fhir/R4/history.html).

## <a name="whats-the-difference-between-the-open-source-microsoft-fhir-server-for-azure-and-the-azure-api-for-fhir"></a>Worin besteht der Unterschied zwischen dem Microsoft Open-Source-FHIR-Server für Azure und Azure API for FHIR?

Azure API for FHIR ist eine gehostete und verwaltete Version des Microsoft Open-Source-FHIR-Servers für Azure. Im verwalteten Dienst stellt Microsoft die gesamte Wartung und alle Updates zur Verfügung. 

Beim Ausführen des FHIR-Servers für Azure haben Sie direkten Zugriff auf die zugrunde liegenden Dienste. Sie sind aber auch für die Wartung und Aktualisierung des Servers und alle erforderlichen Compliance-Aufgaben zuständig, wenn Sie PHI-Daten speichern.

Aus Entwicklersicht wird jedes Feature zuerst auf dem Microsoft Open-Source-FHIR-Server für Azure bereitgestellt. Nachdem es in der Open-Source-Lösung überprüft wurde, wird es in der PaaS-Lösung von Azure API for FHIR veröffentlicht. Die Zeit zwischen der Veröffentlichung in der Open-Source- und der PaaS-Lösung hängt von der Komplexität des Features und anderen Roadmapprioritäten ab. 

## <a name="what-is-smart-on-fhir"></a>Was ist SMART on FHIR?

Bei SMART (Substitutable Medical Applications and Reusable Technology) on FHIR handelt es sich um eine Reihe offener Spezifikationen für die Integration von Partneranwendungen in FHIR-Server und andere IT-Systeme im Gesundheitswesen, wie elektronische Patientenakten und den Austausch von Gesundheitsinformationen. Indem Sie eine SMART on FHIR-Anwendung erstellen, können Sie sicherstellen, dass auf Ihre Anwendung zugegriffen und diese von einer Vielzahl unterschiedlicher Systeme genutzt werden kann.
Authentifizierung und Azure API for FHIR. Weitere Informationen zu SMART finden Sie unter [SMART Health IT](https://smarthealthit.org/).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einige der häufig gestellten Fragen zu Azure API for FHIR gelesen. Weitere Informationen zu den unterstützten Features in FHIR-Server für Azure:
 
>[!div class="nextstepaction"]
>[Unterstützte FHIR-Features](fhir-features-supported.md)