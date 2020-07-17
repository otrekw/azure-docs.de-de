---
title: Azure Web Application Firewall und Azure Policy
description: Azure Web Application Firewall (WAF) in Kombination mit Azure Policy kann dabei helfen, organisatorische Standards durchzusetzen und die Einhaltung von Richtlinien für WAF-Ressourcen zu bewerten.
author: tremansdoerfer
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: rimansdo
ms.openlocfilehash: 12ad18edbb434bdfaec2ae817ea079a843661ef6
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111342"
---
# <a name="azure-web-application-firewall-and-azure-policy"></a>Azure Web Application Firewall und Azure Policy

Azure-Web Application Firewall (WAF) in Kombination mit Azure Policy kann zur Durchsetzung von Organisationsstandards und zur Bewertung von Compliance für WAF-Ressourcen beitragen. Azure Policy ist ein Governancetool, das eine aggregierte Ansicht zur Bewertung des Gesamtzustands der Umgebung mit der Möglichkeit bietet, einen Drilldown zur Granularität pro Ressource und Richtlinie durchzuführen. Außerdem trägt Azure Policy durch Massenwartung für vorhandene Ressourcen und automatische Wartung dazu bei, dass Ihre Ressourcen Complianceanforderungen erfüllen.

## <a name="azure-policies-for-web-application-firewall"></a>Azure Policy für Web Application Firewall

Es gibt mehrere integrierte Azure-Richtlinien zum Verwalten von WAF-Ressourcen. Aufschlüsselung der Richtlinien und ihrer Funktionalität:

1. **Web Application Firewall (WAF) muss für Azure Front Door Service aktiviert sein**: Azure Front Door Services werden ausgewertet, wenn für die Ressourcenerstellung eine WAF vorhanden ist. Die Richtlinie besitzt drei Auswirkungen: Audit (Überwachen), Deny (Verweigern) und Disable (Deaktivieren). „Audit“ (Überwachen) verfolgt nach, wenn ein Azure Front Door Service über keine WAF verfügt und zeigt Benutzern an, welcher Azure Front Door Service nicht konform ist. „Deny“ (Verweigern) verhindert, dass ein Azure Front Door Service erstellt wird, wenn keine WAF angefügt ist. „Disable“ (Deaktivieren) deaktiviert diese Richtlinie.

2. **Web Application Firewall (WAF) muss für Application Gateway aktiviert sein**: Application Gateways werden ausgewertet, wenn für die Ressourcenerstellung eine WAF vorhanden ist. Die Richtlinie besitzt drei Auswirkungen: Audit (Überwachen), Deny (Verweigern) und Disable (Deaktivieren). „Audit“ (Überwachen) verfolgt nach, wenn ein Application Gateway über keine WAF verfügt und zeigt Benutzern an, welches Application Gateway nicht konform ist. „Deny“ (Verweigern) verhindert, dass ein Application Gateway erstellt wird, wenn keine WAF angefügt ist. „Disable“ (Deaktivieren) deaktiviert diese Richtlinie.

3. **Web Application Firewall (WAF) muss den angegebenen Modus für Azure Front Door Service verwenden**: Legt fest, dass die Verwendung des Modus „Detection“ (Erkennung) oder „Prevention“ (Prävention) für alle Web Application Firewall-Richtlinien für Azure Front Door Service aktiv ist. Die Richtlinie besitzt drei Auswirkungen: Audit (Überwachen), Deny (Verweigern) und Disable (Deaktivieren). „Audit“ (Überwachen) verfolgt nach, wenn eine WAF nicht in den angegebenen Modus passt. „Deny“ (Verweigern) verhindert, dass eine WAF erstellt wird, wenn sie sich nicht im richtigen Modus befindet. „Disable“ (Deaktivieren) deaktiviert diese Richtlinie.

4. **Web Application Firewall (WAF) muss den angegebenen Modus für Application Gateway verwenden**: Legt fest, dass die Verwendung des Modus „Detection“ (Erkennung) oder „Prevention“ (Prävention) für alle Web Application Firewall-Richtlinien für Application Gateway aktiv ist. Die Richtlinie besitzt drei Auswirkungen: Audit (Überwachen), Deny (Verweigern) und Disable (Deaktivieren). „Audit“ (Überwachen) verfolgt nach, wenn eine WAF nicht in den angegebenen Modus passt. „Deny“ (Verweigern) verhindert, dass eine WAF erstellt wird, wenn sie sich nicht im richtigen Modus befindet. „Disable“ (Deaktivieren) deaktiviert diese Richtlinie.


## <a name="launch-an-azure-policy"></a>Starten von Azure Policy


1.  Geben Sie auf der Azure-Homepage „Policy“ (Richtlinie) in die Suchleiste ein, und klicken Sie auf das Symbol „Azure Policy“.

2.  Wählen Sie im Azure Policy-Dienst unter **Erstellung** die Option **Zuweisungen** aus.

![Azure Web Application Firewall](../media/waf-azure-policy/policy-home.png)

3.  Wählen Sie oben auf der Seite „Zuweisungen“ das Symbol **Richtlinie zuweisen** aus.

![Azure Web Application Firewall](../media/waf-azure-policy/assign-policy.png)

4.  Aktualisieren Sie auf der Registerkarte „Grundlagen“ der Seite „Richtlinie zuweisen“ die folgenden Felder:
    1.  **Bereich:** Wählen Sie aus, welche Azure-Abonnements und Ressourcengruppen von Azure Policy betroffen sein sollten.
    2.  **Ausschlüsse**: Wählen Sie alle Ressourcen im Bereich aus, die aus der Richtlinie ausgeschlossen werden sollen. 
    3.  **Richtliniendefinition**: Wählen Sie die Azure Policy aus, die auf den Bereich mit Ausschlüssen angewendet werden soll. Geben Sie in der Suchleiste "Web Application Firewall" ein, um die relevante Web Application Firewall Azure Policy auszuwählen.

![Azure Web Application Firewall](../media/waf-azure-policy/policy-listing.png)


5.  Wählen Sie die Registerkarte **Parameter** aus, und aktualisieren Sie die Richtlinienparameter. Um die Funktion des Parameters genauer zu verdeutlichen, zeigen Sie auf das Infosymbol neben dem Parameternamen, um weitere Informationen zu erhalten.

6.  Wählen Sie **Überprüfen und erstellen** aus, um Ihre Azure-Richtlinie fertigzustellen. Die Azure-Richtlinie benötigt ungefähr 15 Minuten, bis sie für neue Ressourcen aktiv wird.
