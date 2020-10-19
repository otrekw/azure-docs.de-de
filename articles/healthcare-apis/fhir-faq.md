---
title: Häufig gestellte Fragen zu FHIR-Diensten in Azure – Azure API for FHIR
description: Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure API for FHIR, wie z. B. Angaben zum Speicherort von Daten hinter FHIR-APIs und zur Versionsunterstützung.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 08/03/2020
ms.author: matjazl
ms.openlocfilehash: 9c32ebef16750954f3df1a1d1b379bf42853f2b3
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92056854"
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

### <a name="where-can-i-see-what-is-releasing-into-the-azure-api-for-fhir"></a>Wo kann ich sehen, was in Azure API for FHIR veröffentlicht wird?

Informationen zu den Funktionen, die in Azure API for FHIR veröffentlicht werden, finden Sie in dem [Release](https://github.com/microsoft/fhir-server/releases) des Open-Source-FHIR-Servers. Wir haben daran gearbeitet, Elemente mit Azure-API-for-FHIR zu markieren, wenn Sie im verwalteten Dienst freigegeben werden, und sie sind in der Regel zwei Wochen nach Anzeige auf der Releaseseite in Open-Source verfügbar. Wir haben außerdem Anweisungen zum Testen des Builds [hier](https://github.com/microsoft/fhir-server/blob/master/docs/Testing-Releases.md) eingeschlossen, wenn Sie Tests in Ihrer eigenen Umgebung durchführen möchten. Wir evaluieren, wie sich zusätzliche verwaltete Dienstupdates am besten teilen lassen.

### <a name="what-is-smart-on-fhir"></a>Was ist SMART on FHIR?

Bei SMART (Substitutable Medical Applications and Reusable Technology) on FHIR handelt es sich um eine Reihe offener Spezifikationen für die Integration von Partneranwendungen in FHIR-Server und andere IT-Systeme im Gesundheitswesen, wie elektronische Patientenakten und den Austausch von Gesundheitsinformationen. Indem Sie eine SMART on FHIR-Anwendung erstellen, können Sie sicherstellen, dass auf Ihre Anwendung zugegriffen und diese von einer Vielzahl unterschiedlicher Systeme genutzt werden kann.
Authentifizierung und Azure API for FHIR. Weitere Informationen zu SMART finden Sie unter [SMART Health IT](https://smarthealthit.org/).

### <a name="can-i-create-a-custom-fhir-resource"></a>Kann ich eine benutzerdefinierte FHIR-Ressource erstellen?

Wir lassen keine benutzerdefinierten FHIR-Ressourcen zu. Wenn Sie eine benutzerdefinierte FHIR-Ressource benötigen, können Sie eine benutzerdefinierte Ressource auf Grundlage der [Basisressource](http://www.hl7.org/fhir/basic.html) mit Erweiterungen erstellen. 

### <a name="are-extensions-supported-on-azure-api-for-fhir"></a>Werden für Azure API for FHIR [Erweiterungen](https://www.hl7.org/fhir/extensibility.html) unterstützt?

Wir erlauben Ihnen, alle gültigen FHIR-JSON-Daten in den Server zu laden. Wenn Sie die Strukturdefinition, die die Erweiterung definiert, speichern möchten, können Sie diese als Strukturdefinitionsressource speichern. Derzeit ist es nicht möglich, nach Erweiterungen zu suchen.

### <a name="what-is-the-limit-on-_count"></a>Welcher Grenzwert gilt für _count?

Der aktuelle Grenzwert ist 100.

### <a name="are-there-any-limitations-on-the-group-export-functionality"></a>Gelten Einschränkungen für die Funktion „Gruppenexport“?

Für „Gruppenexport“ exportieren wir nur die eingeschlossenen Verweise aus der Gruppe, nicht alle Merkmale der [Gruppenressource](https://www.hl7.org/fhir/group.html).

### <a name="can-i-post-a-bundle-to-the-azure-api-for-fhir"></a>Kann ich ein Bündel in Azure API for FHIR bereitstellen?

Zurzeit unterstützen wir das Bereitstellen von [Batchbündeln](https://www.hl7.org/fhir/valueset-bundle-type.html), wir unterstützen aber nicht das Bereitstellen von Transaktionsbündeln in Azure API for FHIR. Sie können den Open-Source-FHIR-Server verwenden, der von SQL unterstützt wird, um Transaktionsbündel bereitzustellen.

### <a name="how-can-i-get-all-resources-for-a-single-patient-in-the-azure-api-for-fhir"></a>Wie kann ich alle Ressourcen für einen einzelnen Patienten in Azure API for FHIR abrufen?

Wir unterstützen die [Depotsuche](https://www.hl7.org/fhir/compartmentdefinition.html) in Azure API for FHIR. Auf diese Weise können Sie alle Ressourcen, die auf einen bestimmten Patienten bezogen sind, abrufen. Beachten Sie, dass ein Depot zurzeit alle Ressourcen enthält, die sich auf den Patienten beziehen, aber nicht den Patienten selbst, sodass Sie auch nach dem Patienten suchen müssen, wenn Sie die Patientenressource in Ihren Ergebnissen benötigen.

Einige Beispiele hierfür sehen Sie im Folgenden:

* GET Patient/<id>/*
* GET Patient/<id>/Observation
* GET Patient/<id>/Observation?code=8302-2

### <a name="where-can-i-see-some-examples-of-using-the-azure-api-for-fhir-within-a-workflow"></a>Wo finde ich einige Beispiele für die Verwendung von Azure API for FHIR in einem Workflow?

Auf der [GitHub-Seite zur Gesundheitsarchitektur (Health Architecture)](https://github.com/microsoft/health-architectures) finden Sie eine Sammlung von Referenzarchitekturen.

## <a name="azure-iot-connector-for-fhir-preview"></a>Azure IoT Connector for FHIR (Vorschau)

### <a name="what-is-iomt"></a>Was ist IoMT?
IoMT steht für „Internet of Medical Things“ (Internet der medizinischen Dinge) und ist eine Kategorie von IoT-Geräten, die Gesundheits- und Befindlichkeitsdaten erfassen und über ein Netzwerk mit anderen IT-Systemen im Gesundheitswesen austauschen. Einige Beispiele für IoMT-Geräte sind Fitness- und klinische Wearables, Überwachungssensoren, Aktivitätstracker, Point-of-Care-Kioske oder sogar eine digitale Pille.

### <a name="how-many-azure-iot-connector-for-fhir-preview-do-i-need"></a>Wie viele Instanzen von Azure IoT Connector for FHIR (Vorschau) benötige ich?
Ein einziger Azure IoT Connector for FHIR* kann verwendet werden, um Daten von einer großen Anzahl unterschiedlicher Gerätetypen zu erfassen. Vielleicht entscheiden Sie sich aus folgenden Gründen dennoch für die Verwendung weiterer Connectors:
- **Skalierung**: Für die öffentliche Vorschau ist die Ressourcenkapazität von Azure IoT Connector for FHIR festgelegt, und es wird erwartet, dass sie einen Durchsatz von etwa 200 Nachrichten pro Sekunde bereitstellt. Wenn ein höherer Durchsatz erforderlich ist, können Sie weitere Instanzen von Azure IoT Connector for FHIR hinzufügen.
- **Gerätetyp**: Zum Zwecke der Geräteverwaltung können Sie für jeden Typ von IoMT-Gerät einen separaten Azure IoT Connector for FHIR einrichten.

### <a name="is-there-a-limit-on-number-of-azure-iot-connector-for-fhir-preview-during-public-preview"></a>Gibt es eine Beschränkung für die Anzahl der Instanzen von Azure IoT Connector for FHIR (Vorschau) während der öffentlichen Vorschau?
Ja, Sie können nur zwei Instanzen von Azure IoT Connector for FHIR pro Abonnement erstellen, während sich die Funktion in der öffentlichen Vorschau befindet. Diese Einschränkung ist vorhanden, um unerwartete Kosten zu vermeiden, da die Funktion während der Vorschau kostenlos verfügbar ist. Auf Anfrage kann diese Einschränkung auf ein Maximum von bis zu fünf Instanzen von Azure IoT Connector for FHIR angehoben werden.

### <a name="what-azure-regions-azure-iot-connector-for-fhir-preview-feature-is-available-during-public-preview"></a>In welchen Azure-Regionen ist die Funktion „Azure IoT Connector for FHIR (Vorschau)“ während der öffentlichen Vorschau verfügbar?
Azure IoT Connector for FHIR ist in allen Azure-Regionen verfügbar, in denen Azure API for FHIR verfügbar ist.

### <a name="can-i-configure-scaling-capacity-for-azure-iot-connector-for-fhir-preview"></a>Kann ich die Skalierungskapazität für Azure IoT Connector for FHIR (Vorschau) konfigurieren?
Da Azure IoT Connector for FHIR während der öffentlichen Vorschau kostenlos ist, ist die Skalierungskapazität festgelegt und begrenzt. Die in der öffentlichen Vorschau verfügbare Azure IoT Connector für FHIR-Konfiguration ermöglicht voraussichtlich einen Durchsatz von etwa 200 Nachrichten pro Sekunde. Die Konfiguration der Ressourcenkapazität wird in irgendeiner Form in der allgemeinen Verfügbarkeit (General Availability, GA) vorhanden sein.

### <a name="what-fhir-version-does-azure-iot-connector-for-fhir-preview-support"></a>Welche FHIR-Version wird von Azure IoT Connector for FHIR (Vorschau) unterstützt?
Azure IoT Connector for FHIR unterstützt derzeit nur die Version R4 von FHIR. Daher ist diese Funktion nur auf den R4-Instanzen von Azure API for FHIR sichtbar, und Microsoft plant derzeit keine Unterstützung von Version STU3.

### <a name="why-cant-i-install-azure-iot-connector-for-fhir-preview-when-private-link-is-enabled-on-azure-api-for-fhir"></a>Warum kann ich Azure IoT Connector for FHIR (Vorschau) nicht installieren, wenn Private Link für Azure API for FHIR aktiviert ist?
Azure IoT Connector for FHIR unterstützt derzeit keine Private Link-Funktionalität. Wenn Sie also Private Link für Azure API for FHIR aktiviert haben, können Sie Azure IoT Connector for FHIR nicht installieren und umgekehrt. Diese Einschränkung wird voraussichtlich entfernt, wenn Azure IoT Connector for FHIR für die allgemeine Verfügbarkeit (GA) vorhanden ist.

### <a name="whats-the-difference-between-the-open-source-iomt-fhir-connector-for-azure-and-azure-iot-connector-for-fhir-preview-feature-of-azure-api-for-fhir-service"></a>Worin besteht der Unterschied zwischen dem Open-Source-IoMT-FHIR-Connector für Azure und der Funktion „Azure IoT Connector for FHIR (Vorschau)“ des Azure API for FHIR-Diensts?
Azure IoT Connector for FHIR ist eine gehostete und verwaltete Version des Open-Source-IoMR-FHIR-Connectors für Azure. Im verwalteten Dienst stellt Microsoft die gesamte Wartung und alle Updates zur Verfügung.

Beim Ausführen des IoMT-FHIR-Connectors für Azure haben Sie direkten Zugriff auf die zugrunde liegenden Ressourcen. Sie sind aber auch für die Wartung und Aktualisierung des Servers und alle erforderlichen Compliance-Aufgaben zuständig, wenn Sie PHI-Daten speichern.

Aus Entwicklersicht wird jedes Feature zuerst auf dem Open-Source-IoMT-FHIR-Connector für Azure bereitgestellt. Nachdem es in der Open-Source-Lösung überprüft wurde, wird es in der PaaS-Funktion „Azure IoT Connector for FHIR“ des Azure API for FHIR-Diensts veröffentlicht. Die Zeit zwischen der Veröffentlichung in der Open-Source- und der PaaS-Lösung hängt von der Komplexität des Features und anderen Roadmapprioritäten ab.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einige der häufig gestellten Fragen zu Azure API for FHIR gelesen. Weitere Informationen zu den unterstützten Features in FHIR-Server für Azure:
 
>[!div class="nextstepaction"]
>[Unterstützte FHIR-Features](fhir-features-supported.md)

*Im Azure-Portal wird Azure IoT Connector for FHIR als „IoT-Connector (Vorschau)“ bezeichnet.

FHIR ist ein eingetragenes Markenzeichen von HL7 und wird mit Erlaubnis von HL7 verwendet.