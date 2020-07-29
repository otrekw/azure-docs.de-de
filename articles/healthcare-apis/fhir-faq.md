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
ms.openlocfilehash: af891935fd474e6f1f83ff1c2ce56ef71cd065c6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536724"
---
# <a name="frequently-asked-questions-about-the-azure-api-for-fhir"></a>Häufig gestellte Fragen zu Azure API for FHIR

## <a name="azure-api-for-fhir"></a>Azure-API für FHIR

### <a name="what-is-fhir"></a>Was ist FHIR?
Fast Healthcare Interoperability Resources (FHIR, ausgesprochen wie das englische Wort „Fire“) ist ein Interoperabilitätsstandard, der den Austausch von Patientendaten zwischen verschiedenen Gesundheitssystemen ermöglicht. Dieser Standard wurde von der HL7-Organisation entwickelt und wird von Gesundheitsorganisationen auf der ganzen Welt übernommen. Die aktuelle Version von FHIR ist R4 (Release 4). Azure API for FHIR unterstützt R4 und auch die vorherige Version STU3 (Standard for Trial Use 3). Weitere Informationen zu FHIR finden Sie auf [HL7.org](http://hl7.org/fhir/summary.html).

### <a name="is-the-data-behind-the-fhir-apis-stored-in-azure"></a>Werden die Daten hinter den FHIR-APIs in Azure gespeichert?

Ja. Die Daten werden in verwalteten Datenbanken in Azure gespeichert. Die Azure API for FHIR bietet keinen direkten Zugriff auf den zugrunde liegenden Datenspeicher.

### <a name="what-identity-provider-do-you-support"></a>Welcher Identitätsanbieter wird unterstützt?

Derzeit wird Microsoft Azure Active Directory als Identitätsanbieter unterstützt.

### <a name="what-fhir-version-do-you-support"></a>Welche FHIR-Version wird unterstützt?

Die Versionen 4.0.0 und 3.0.1 werden sowohl in Azure API for FHIR (PaaS) als auch auf dem FHIR-Server für Azure (Open Source) unterstützt.

Ausführliche Informationen finden Sie unter [Unterstützte Features](fhir-features-supported.md). Informationen zu den Änderungen zwischen Versionen finden Sie im [Versionsverlauf für HL7 FHIR](https://hl7.org/fhir/R4/history.html).

### <a name="whats-the-difference-between-the-open-source-microsoft-fhir-server-for-azure-and-the-azure-api-for-fhir"></a>Worin besteht der Unterschied zwischen dem Microsoft Open-Source-FHIR-Server für Azure und Azure API for FHIR?

Azure API for FHIR ist eine gehostete und verwaltete Version des Microsoft Open-Source-FHIR-Servers für Azure. Im verwalteten Dienst stellt Microsoft die gesamte Wartung und alle Updates zur Verfügung. 

Beim Ausführen des FHIR-Servers für Azure haben Sie direkten Zugriff auf die zugrunde liegenden Dienste. Sie sind aber auch für die Wartung und Aktualisierung des Servers und alle erforderlichen Compliance-Aufgaben zuständig, wenn Sie PHI-Daten speichern.

Aus Entwicklersicht wird jedes Feature zuerst auf dem Microsoft Open-Source-FHIR-Server für Azure bereitgestellt. Nachdem es in der Open-Source-Lösung überprüft wurde, wird es in der PaaS-Lösung von Azure API for FHIR veröffentlicht. Die Zeit zwischen der Veröffentlichung in der Open-Source- und der PaaS-Lösung hängt von der Komplexität des Features und anderen Roadmapprioritäten ab. 

### <a name="what-is-smart-on-fhir"></a>Was ist SMART on FHIR?

Bei SMART (Substitutable Medical Applications and Reusable Technology) on FHIR handelt es sich um eine Reihe offener Spezifikationen für die Integration von Partneranwendungen in FHIR-Server und andere IT-Systeme im Gesundheitswesen, wie elektronische Patientenakten und den Austausch von Gesundheitsinformationen. Indem Sie eine SMART on FHIR-Anwendung erstellen, können Sie sicherstellen, dass auf Ihre Anwendung zugegriffen und diese von einer Vielzahl unterschiedlicher Systeme genutzt werden kann.
Authentifizierung und Azure API for FHIR. Weitere Informationen zu SMART finden Sie unter [SMART Health IT](https://smarthealthit.org/).


## <a name="iot-connector-preview"></a>IoT Connector (Vorschau)

### <a name="what-is-iomt"></a>Was ist IoMT?
IoMT steht für „Internet of Medical Things“ (Internet der medizinischen Dinge) und ist eine Kategorie von IoT-Geräten, die Gesundheits- und Befindlichkeitsdaten erfassen und über ein Netzwerk mit anderen IT-Systemen im Gesundheitswesen austauschen. Einige Beispiele für IoMT-Geräte sind Fitness- und klinische Wearables, Überwachungssensoren, Aktivitätstracker, Point-of-Care-Kioske oder sogar eine digitale Pille.

### <a name="how-many-iot-connectors-do-i-need"></a>Wie viele IoT Connector-Instanzen benötige ich?
Eine einzige IoT Connector-Instanz kann verwendet werden, um Daten von einer großen Anzahl verschiedener Gerätetypen zu erfassen. Vielleicht entscheiden Sie sich aus folgenden Gründen dennoch für die Verwendung weiterer Connectors:
- **Skalierung**: Für die öffentliche Vorschau ist die Kapazität der IoT Connector-Ressourcen festgelegt und soll einen Durchsatz von etwa 200 Nachrichten pro Sekunde ermöglichen. Wenn ein höherer Durchsatz erforderlich ist, können Sie weitere IoT Connectors-Instanzen hinzufügen.
- **Gerätetyp**: Aus Gründen der Geräteverwaltung können Sie für jeden Typ von IoMT-Geräten eine separate IoT Connector-Instanzen einrichten.

### <a name="is-there-a-limit-on-number-of-iot-connectors-during-public-preview"></a>Gibt es eine Beschränkung für die Anzahl von IoT Connector-Instanzen während der öffentlichen Vorschau?
Ja, Sie können nur zwei IoT Connector-Instanzen pro Abonnement erstellen, während sich die Funktion in der öffentlichen Vorschau befindet. Diese Einschränkung ist vorhanden, um unerwartete Kosten zu vermeiden, da die Funktion während der Vorschau kostenlos verfügbar ist. Auf Anfrage kann diese Grenze auf bis zu fünf IoT Connector-Instanzen angehoben werden.

### <a name="what-azure-regions-iot-connector-feature-is-available-during-public-preview"></a>In welchen Azure-Regionen ist die IoT Connector-Funktion während der öffentlichen Vorschau verfügbar?
IoT Connector ist in allen Azure-Regionen verfügbar, in denen Azure API for FHIR verfügbar ist.

### <a name="can-i-configure-scaling-capacity-for-iot-connector"></a>Kann ich die Skalierungskapazität für IoT Connector konfigurieren?
Da IoT Connector während der öffentlichen Vorschau kostenlos ist, ist die Skalierungskapazität festgelegt und begrenzt. Die in der öffentlichen Vorschau verfügbare IoT Connector-Konfiguration ermöglicht voraussichtlich einen Durchsatz von etwa 200 Nachrichten pro Sekunde. Die Konfiguration der Ressourcenkapazität wird in irgendeiner Form in der allgemeinen Verfügbarkeit (General Availability, GA) vorhanden sein.

### <a name="what-fhir-version-does-iot-connector-support"></a>Welche FHIR-Version unterstützt IoT Connector?
IoT Connector unterstützt derzeit nur die Version R4 von FHIR. Daher ist diese Funktion nur auf den R4-Instanzen von Azure API for FHIR sichtbar, und Microsoft plant derzeit keine Unterstützung von Version STU3.

### <a name="why-cant-i-install-iot-connector-when-private-link-is-enabled-on-azure-api-for-fhir"></a>Warum kann ich IoT Connector nicht installieren, wenn Private Link für Azure API for FHIR aktiviert ist?
IoT Connector unterstützt derzeit keine Private Link-Funktionalität. Wenn Sie also Private Link für Azure API for FHIR aktiviert haben, können Sie IoT Connector nicht installieren und umgekehrt. Diese Einschränkung wird voraussichtlich entfernt, wenn IoT Connector für die allgemeine Verfügbarkeit (GA) vorhanden ist.

### <a name="whats-the-difference-between-the-open-source-iomt-fhir-connector-for-azure-and-iot-connector-feature-of-azure-api-for-fhir-service"></a>Worin besteht der Unterschied zwischen dem Open-Source-IoMT-FHIR-Connector für Azure und der IoT Connector-Funktion des Azure API for FHIR-Diensts?
IoT Connector ist eine gehostete und verwaltete Version des Open-Source-IoMR-FHIR-Connectors für Azure. Im verwalteten Dienst stellt Microsoft die gesamte Wartung und alle Updates zur Verfügung.

Beim Ausführen des IoMT-FHIR-Connectors für Azure haben Sie direkten Zugriff auf die zugrunde liegenden Ressourcen. Sie sind aber auch für die Wartung und Aktualisierung des Servers und alle erforderlichen Compliance-Aufgaben zuständig, wenn Sie PHI-Daten speichern.

Aus Entwicklersicht wird jedes Feature zuerst auf dem Open-Source-IoMT-FHIR-Connector für Azure bereitgestellt. Nachdem es in der Open-Source-Lösung überprüft wurde, wird es in der PaaS-IoT-Connector-Funktion des Azure API for FHIR-Diensts veröffentlicht. Die Zeit zwischen der Veröffentlichung in der Open-Source- und der PaaS-Lösung hängt von der Komplexität des Features und anderen Roadmapprioritäten ab.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einige der häufig gestellten Fragen zu Azure API for FHIR gelesen. Weitere Informationen zu den unterstützten Features in FHIR-Server für Azure:
 
>[!div class="nextstepaction"]
>[Unterstützte FHIR-Features](fhir-features-supported.md)