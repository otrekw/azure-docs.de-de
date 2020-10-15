---
title: Häufig gestellte Fragen zu Azure Security Center (FAQ) – Allgemeine Fragen
description: Diese häufig gestellten allgemeinen Fragen beantworten Fragen zu Azure Security Center, einem Produkt, das Ihnen hilft, Bedrohungen zu verhindern, zu erkennen und darauf zu reagieren.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 5695f9fa090419d803f4f3603b45b771321e5ce9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91301447"
---
# <a name="faq---general-questions-about-azure-security-center"></a>Häufig gestellte Fragen – allgemeine Fragen zu Azure Security Center

## <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
Azure Security Center unterstützt Sie bei der Vermeidung, Erkennung und Behandlung von Bedrohungen. Mit dieser Cloudlösung gewinnen Sie mehr Transparenz und bessere Kontrolle über die Sicherheit Ihrer Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Sektrum an Sicherheitslösungen verwendet werden.

Security Center verwendet den Log Analytics-Agent zum Erfassen und Speichern von Daten. Detaillierte Informationen finden Sie unter [Datensammlung in Azure Security Center](security-center-enable-data-collection.md).


## <a name="how-do-i-get-azure-security-center"></a>Wie erhalte ich Azure Security Center?
Azure Security Center wird mit Ihrem Microsoft Azure-Abonnement aktiviert und über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)aufgerufen. Um darauf zuzugreifen, [melden Sie sich beim Portal an](https://portal.azure.com), wählen Sie **Durchsuchen** aus, und scrollen Sie zu **Security Center**.


## <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Welche Azure-Ressourcen werden von Azure Security Center überwacht?
Azure Security Center überwacht die folgenden Azure-Ressourcen:

* Virtuelle Computer (Virtual Machines, VMs) (auch [Clouddienste](../cloud-services/cloud-services-choose-me.md))
* VM-Skalierungsgruppen
* Partnerlösungen, die in Ihr Azure-Abonnement integriert sind, z.B. die Firewall einer Webanwendung auf virtuellen Computern und in einer App Service-Umgebung
* [Die vielen Azure-PaaS-Dienste in der Produktübersicht](features-paas.md)


## <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Wie kann ich den aktuellen Sicherheitsstatus meiner Azure-Ressourcen anzeigen?
Auf der Seite **Security Center – Übersicht** wird der Gesamtsicherheitsstatus Ihrer Umgebung aufgegliedert nach Compute, Netzwerk, Speicher und Daten und Anwendungen angezeigt. Jeder Ressourcentyp verfügt über einen Indikator, der ermittelte Sicherheitsrisiken anzeigt. Wenn Sie auf die einzelnen Kacheln klicken, wird eine Liste mit von Security Center erkannten Sicherheitsproblemen sowie ein Bestand an Ressourcen in Ihrem Abonnement angezeigt.



## <a name="what-is-a-security-policy"></a>Was ist eine Sicherheitsrichtlinie?
In einer Sicherheitsrichtlinie werden die Sicherheitsmechanismen definiert, die für Ressourcen in dem angegebenen Abonnement zu empfehlen sind. In Azure Security Center definieren Sie Richtlinien für Ihre Azure-Abonnements entsprechend den Sicherheitserfordernissen Ihres Unternehmens sowie dem Typ der Anwendungen oder der Vertraulichkeit der Daten in jedem Abonnement.

Die Sicherheitsempfehlungen und -überwachung werden entsprechend den in Azure Security Center aktivierten Sicherheitsrichtlinien umgesetzt. Weitere Informationen zu Sicherheitsrichtlinien finden Sie unter [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md).


## <a name="who-can-modify-a-security-policy"></a>Wie kann ich eine Sicherheitsrichtlinie ändern?
Zum Ändern einer Sicherheitsrichtlinie müssen Sie ein **Sicherheitsadministrator** oder **Besitzer** des Abonnements sein.

Weitere Informationen dazu, wie eine Sicherheitsrichtlinie konfiguriert wird, finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md).


## <a name="what-is-a-security-recommendation"></a>Was ist eine Sicherheitsempfehlung?
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden mögliche Sicherheitsrisiken festgestellt, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess des Konfigurierens des erforderlichen Sicherheitsmechanismus geführt. Beispiele:

* Bereitstellung von Antischadsoftware, um bösartige Software zu erkennen und zu entfernen
* [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) und Regeln, um Datenverkehr zu virtuellen Computern zu steuern
* Bereitstellung einer Web Application Firewall als Schutz vor Angriffen, die auf Ihre Webanwendungen abzielen
* Bereitstellen von fehlenden Systemupdates
* Behandeln von Betriebssystemkonfigurationen, die nicht den empfohlenen Basisregeln entsprechen

Hier sind nur Empfehlungen aufgeführt, die in den Sicherheitsrichtlinien aktiviert sind.


## <a name="what-triggers-a-security-alert"></a>Wodurch wird eine Sicherheitswarnung ausgelöst?
Azure Security Center erfasst, analysiert und kombiniert automatisch Protokolldaten von Ihren Azure-Ressourcen, vom Netzwerk und von Partnerlösungen wie Antischadsoftware und Firewalls. Wurden Bedrohungen erkannt, wird eine Sicherheitswarnung erstellt. Beispiele für eine Erkennung sind:

* Gefährdete virtuelle Computer, die mit bekannten bösartigen IP-Adressen kommunizieren
* Erweiterte Schadsoftware, die mithilfe der Windows-Fehlerberichterstattung erkannt wurde
* Brute-Force-Angriffe gegen virtuelle Computer
* Sicherheitswarnungen von integrierten Partnersicherheitslösungen wie Antischadsoftware oder Web Application Firewalls


## <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Wo liegt bei der Bedrohungserkennung und Ausgabe von Warnungen der Unterschied zwischen Microsoft Security Response Center und Azure Security Center?
Das Microsoft Security Response Center (MSRC) führt eine selektive Sicherheitsüberwachung im Azure-Netzwerk und in der Infrastruktur durch und empfängt Threat Intelligence-Daten und Missbrauchsmeldungen von Drittanbietern. Wenn MSRC erkennt, dass eine Partei unrechtmäßig oder unbefugt auf Kundendaten zugreift oder dass der Kunde Azure nicht entsprechend den Bestimmungen für den Umgang mit vertraulichen Informationen (Acceptable Use) nutzt, wird der Kunde durch einen Manager für Sicherheitsincidents benachrichtigt. Die Benachrichtigung erfolgt in der Regel durch das Senden einer E-Mail an die im Azure Security Center angegebenen Sicherheitskontakte oder an den Besitzer des Azure-Abonnements, falls kein Sicherheitskontakt angegeben wurde.

Das Security Center ist ein Azure-Dienst, der die Azure-Umgebung des Kunden ständig überwacht und Analysefunktionen anwendet, um eine Vielzahl von potenziell schädlichen Aktivitäten automatisch zu erkennen. Diese Erkennungen werden als Sicherheitshinweise im Security Center-Dashboard angegeben.