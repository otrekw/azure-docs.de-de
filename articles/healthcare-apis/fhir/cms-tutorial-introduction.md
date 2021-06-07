---
title: 'Tutorial: Einführung in Centers forSidere and Cmsid Services (CMS) – Azure API for FHIR'
description: In dieser Übersicht wird eine Reihe von Tutorials vorgestellt, die sich auf die Interoperabilitäts- und Patientenzugriffsregel des Center for Bestehtre und DerId Services (CMS) beziehen.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 05/27/2021
ms.openlocfilehash: 837fdb74cbbeb976641b9ee86970d87b80134bc9
ms.sourcegitcommit: ff1aa951f5d81381811246ac2380bcddc7e0c2b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111572104"
---
# <a name="centers-for-medicare-and-medicaid-services-cms-interoperability-and-patient-access-rule-introduction"></a>Einführung in die Interoperabilitäts- und Patientenzugriffsregel von Centers forSidere and Lapid Services (CMS)

In dieser Reihe von Tutorials wird eine grobe Zusammenfassung der Interoperabilitäts- und Patientenzugriffsregel des Center for Centrere und Desid Services (CMS) sowie die in dieser Regel beschriebenen technischen Anforderungen behandelt. Wir durchlaufen die verschiedenen Implementierungshandbücher, auf die für diese Regel verwiesen wird. Außerdem erfahren Sie, wie Sie die Azure API for FHIR zur Unterstützung dieser Implementierungshandbücher konfigurieren.


## <a name="rule-overview"></a>Regelübersicht

Das CMS hat die [Interoperabilitäts- und Patientenzugriffsregel](https://www.cms.gov/Regulations-and-Guidance/Guidance/Interoperability/index) am 1. Mai 2020 veröffentlicht. Diese Regel erfordert einen kostenlosen und sicheren Datenfluss zwischen allen Beteiligten in der Patientenversorgung (Patienten, Anbieter und Zahlende), damit Patienten bei Bedarf auf ihre Gesundheitsinformationen zugreifen können. Die Interoperabilität hat das Gesundheitswesen seit Mehreren Jahren beeinträchtigt, was zu isolierten Daten führt, die negative Gesundheitsergebnisse mit höheren und unvorhersehbaren Kosten für die Pflege verursachen. CMS verwendet ihre Autorität, um Aussteller von "Advantage" (MA), "Sideid", "Children es Health Insurance Program" (CHIP) und "Qualified Health Plan" (QHP) in den Federally Exchanges (FFEs) zu regeln, um diese Regel zu erzwingen. 

Im August 2020 hat CMS ausführlich erläutert, wie Organisationen das Mandate erfüllen können. Um sicherzustellen, dass Daten sicher und standardisiert ausgetauscht werden können, identifiziert CMS FHIR Version 4 (R4) als grundlegenden Standard, der für den Datenaustausch erforderlich ist. 

Es gibt drei Hauptteile der Interoperabilität und des Patientenzugriffs:

* **Patientenzugriffs-API (erforderlich am 1. Juli 2021)** – Cms-regulierte Zahlende (wie oben definiert) müssen eine sichere, standardbasierte API implementieren und verwalten, die es Patienten ermöglicht, einfach auf ihre Ansprüche zuzugreifen und Informationen, einschließlich Kosten, sowie eine definierte Teilmenge ihrer klinischen Informationen über Anwendungen von Drittanbietern ihrer Wahl zu finden.  

* **Anbieterverzeichnis-API (erforderlich am 1. Juli 2021):** Von CMS regulierte Zahlende sind für diesen Teil der Regel erforderlich, um Anbieterverzeichnisinformationen über eine standardbasierte API öffentlich verfügbar zu machen. Durch die Bereitstellung dieser Informationen können Anwendungsentwickler von Drittanbietern Dienste erstellen, die Patienten dabei unterstützen, Anbieter für bestimmte Pflegeanforderungen zu finden, und Ärzte andere Anbieter für die Koordination der Pflege finden.  

* **Payer-to-Payer Data Exchange (Erforderlich: 1. Januar 2022):** Cms-regulierte Zahler müssen bestimmte patientenklinische Daten auf Anforderung des Patienten mit anderen Zahlenden austauschen. Es ist zwar nicht erforderlich, irgendeine Art von Standard zu befolgen, es wird jedoch empfohlen, FHIR zum Austauschen dieser Daten anzuwenden. 

## <a name="key-fhir-concepts"></a>Wichtige FHIR-Konzepte

Wie bereits erwähnt, ist FHIR R4 erforderlich, um dieses Erfordernis zu erfüllen. Darüber hinaus wurden mehrere Implementierungsleitfäden entwickelt, die Anleitungen für die Regel bereitstellen. [Implementierungshandbücher](https://www.hl7.org/fhir/implementationguide.html) bieten zusätzlich zur FHIR-Basisspezifikation zusätzlichen Kontext. Dies umfasst das Definieren zusätzlicher Suchparameter, Profile, Erweiterungen, Vorgänge, Wertsätze und Codesysteme.

Die Azure API for FHIR verfügt über die folgenden Funktionen, mit denen Sie Ihre Datenbank für die verschiedenen Implementierungshandbücher konfigurieren können:

* [Unterstützung für RESTful-Interaktionen](fhir-features-supported.md)
* [Speichern und Überprüfen von Profilen](validation-against-profiles.md)
* [Definieren und Indizieren von benutzerdefinierten Suchparametern](how-to-do-custom-search.md)
* [Konvertieren von Daten](convert-data.md)

## <a name="patient-access-api-implementation-guides"></a>Implementierungshandbücher für die Api für den Patientenzugriff

Die Api für den Patientenzugriff beschreibt die Einhaltung von vier FHIR-Implementierungshandbüchern.

### <a name="carin-ig-for-blue-button"></a>[CARIN IG for Blue Button®](http://hl7.org/fhir/us/carin-bb/STU1/index.html):

Die Zahlenden müssen die Ansprüche von Patienten stellen und auf Daten stoßen, die gemäß dem Implementierungshandbuch der CARIN IG for Blue Button (C4BB IG) verfügbar sind. Die C4BB IG stellt eine Reihe von Ressourcen bereit, die Zahlende über eine FHIR-API für Kunden anzeigen können, und enthält die details, die für Anspruchsdaten in der Interoperabilitäts- und Patientenzugriffs-API erforderlich sind. In diesem Implementierungsleitfaden wird die ExplanationOfBenefit-Ressource (EOB) als Hauptressource verwendet, die andere Ressourcen abruft, während auf sie verwiesen wird.

### <a name="hl7-fhir-da-vinci-pdex-ig"></a>[HL7 FHIR DaDex PDex IG](http://hl7.org/fhir/us/davinci-pdex/STU1/index.html)

Das Implementierungshandbuch für den Datenaustausch für Zahlende (Payer Data Exchange Implementation Guide, PDex IG) konzentriert sich darauf, sicherzustellen, dass die Zahler alle relevanten patientenklinische Daten bereitstellen, um die Anforderungen für die Patientenzugriffs-API zu erfüllen. Dies verwendet die US Core-Profile für R4-Ressourcen und umfasst (mindestens) Treffen, Anbieter, Organisationen, Standorte, Datumsangaben des Diensts, Diagnosen, Prozeduren und Beobachtungen. Obwohl diese Daten im FHIR-Format verfügbar sein können, können sie auch von anderen Systemen im Format von Anspruchsdaten, HL7 V2-Nachrichten und C-CDA-Dokumenten stammen.

### <a name="hl7-us-core-ig"></a>[HL7 US Core IG](https://www.hl7.org/fhir/us/core/toc.html)

Der HL7 US Core Implementation Guide (US Core IG) ist der Backbone für die oben beschriebene PDex IG. Während die PDex IG einige Ressourcen noch weiter als die US Core IG einschränkt, befolgen viele Ressourcen einfach die Standards in der US Core IG.

### <a name="hl7-fhir-da-vinci---pdex-us-drug-formulary-ig"></a>[HL7 FHIR DaDex – PDex US- Pharmaformulary IG](http://hl7.org/fhir/us/Davinci-drug-formulary/index.html)

Part D Advantage-Pläne müssen Formularinformationen über die Patienten-API zur Verfügung stellen. Dazu wird der PDex US-Leitfaden zur Formularimplementierungen (USDF IG) verwendet. Die USDF IG definiert eine FHIR-Schnittstelle für die Medizinischen Formularinformationen eines Gesundheitsunternehmens. Dabei handelt es sich um eine Liste von Marken- und generischen Händlern, für die ein Gesundheitsversorgungsanbieter zustimmt. Der Wichtigste Anwendungsfall besteht darin, dass Patienten verstehen können, ob ein ihnen vorgeschriebenes alternatives Mittel zur Verfügung steht, und um die Kosten für Dasinshen zu vergleichen.

## <a name="provider-directory-api-implementation-guide"></a>Implementierungshandbuch für die Anbieterverzeichnis-API

Die Anbieterverzeichnis-API beschreibt die Einhaltung eines Implementierungsleitfadens.

### <a name="hl7-da-vinci-pdex-plan-network-ig"></a>[HL7 DaDex PDex Plan Network IG](http://build.fhir.org/ig/HL7/davinci-pdex-plan-net/)

Dieser Implementierungsleitfaden definiert eine FHIR-Schnittstelle für die Versicherungspläne eines Gesundheitsdienstleisters, die zugehörigen Netzwerke und die Organisationen und Anbieter, die an diesen Netzwerken teilnehmen.

## <a name="touchstone"></a>Prüfstein

[Touchstone](https://touchstone.aegis.net/touchstone/) ist eine hervorragende Ressource, um die Einhaltung der verschiedenen Implementierungshandbücher zu testen. In den nächsten Tutorials konzentrieren wir uns darauf, sicherzustellen, dass die Azure API for FHIR so konfiguriert ist, dass verschiedene Touchstone-Tests erfolgreich bestanden werden. Die Touchstone-Website enthält viele hervorragende Dokumentationen, die Ihnen beim Einstieg helfen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über ein grundlegendes Verständnis der Interoperabilitäts- und Patientenzugriffsregel, implementierungsleitfäden und des verfügbaren Testtools (Touchstone) verfügen, werden sie durch die Einrichtung der Azure API for FHIR für die CARIN IG for Blue Button geführt. 

>[!div class="nextstepaction"]
>[CARIN-Implementierungshandbuch für Blue Button](carin-implementation-guide-blue-button-tutorial.md)  