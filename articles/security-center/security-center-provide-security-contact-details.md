---
title: Bereitstellen von Sicherheitskontaktinformationen in Azure Security Center | Microsoft Docs
description: In diesem Dokument wird erläutert, wie Sie Sicherheitskontaktinformationen in Azure Security Center bereitstellen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2019
ms.author: memildin
ms.openlocfilehash: 1a66ea200082f60a3a763c6a4e2bdea62ec473d8
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2020
ms.locfileid: "75920997"
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Bereitstellen von Sicherheitskontaktinformationen in Azure Security Center
In Azure Security Center wird die Bereitstellung von Sicherheitskontaktinformationen für Ihr Azure-Abonnement empfohlen (sofern noch nicht geschehen). Microsoft kontaktiert Sie anhand dieser Informationen, wenn Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Kundendaten zugegriffen haben. MSRC führt eine selektive Sicherheitsüberwachung im Azure-Netzwerk und in der Infrastruktur durch und empfängt Threat Intelligence-Daten und Missbrauchsmeldungen von Drittanbietern.

Eine E-Mail-Benachrichtigung wird beim ersten Auftreten einer Warnung am Tag und nur für Warnungen mit hohem Schweregrad gesendet. E-Mail-Optionen können nur für Abonnementsrichtlinien konfiguriert werden. Ressourcengruppen in einem Abonnement erben diese Einstellungen. Warnungen sind nur im Standard-Tarif von Azure Security Center verfügbar.

Warnungsbenachrichtigungen per E-Mail werden gesendet:
- Nur für Warnungen mit hohem Schweregrad
- An einen einzelnen E-Mail-Empfänger pro Warnungstyp pro Tag  
- Nicht mehr als 3-E-Mail-Nachrichten werden an einem Tag an einen einzelnen Empfänger gesendet
- Jede E-Mail-Nachricht enthält eine einzelne Warnung, nicht eine Sammlung von Warnungen
 
Wenn z.B. bereits eine E-Mail-Nachricht gesendet wurde, um Sie vor einem RDP-Angriff zu warnen, erhalten Sie nicht am selben Tag eine andere E-Mail-Nachricht über einen RDP-Angriff, auch dann nicht, wenn eine andere Warnung ausgelöst wird. 

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Es ist keine schrittweise Anleitung.

## Einrichten von E-Mail-Benachrichtigungen für Warnungen <a name="email"></a>

1. Wählen Sie im Portal **Preise und Einstellungen** aus.
1. Klicken Sie auf das Abonnement.
1. Klicken Sie auf **E-Mail-Benachrichtigungen**.

> [!NOTE]
> Wenn Sie eine Empfehlung umsetzen, wählen Sie unter **Empfehlungen** die Option **Sicherheitskontaktinformationen bereitstellen** und dann das Azure-Abonnement aus,für das Sie Kontaktinformationen angeben möchten. Dadurch wird **E-Mail-Benachrichtigungen** geöffnet.

   ![Sicherheitskontaktinformationen bereitstellen][2]

   * Geben Sie die E-Mail-Adressen der Sicherheitskontakte durch Kommas getrennt ein. Sie können beliebig viele E-Mail-Adressen eingeben.
   * Geben Sie eine internationale Telefonnummer für den Sicherheitskontakt ein.
   * Um E-Mails zu Warnungen mit hohem Schweregrad zu erhalten, aktivieren Sie die Option **E-Mails zu Warnungen an mich senden**.
   * Sie haben die Möglichkeit, E-Mail-Benachrichtigungen an Abonnementbesitzer zu senden (den klassischen Dienstadministrator und Co-Administratoren sowie die Rolle „RBAC-Besitzer“ im Abonnementbereich).
   * Wählen Sie **Speichern** aus, um die Sicherheitskontaktinformationen für Ihr Abonnement zu übernehmen.

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
