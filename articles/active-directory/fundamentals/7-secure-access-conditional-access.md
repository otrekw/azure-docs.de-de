---
title: Verwalten des externen Zugriffs über den bedingten Zugriff mit Azure Active Directory
description: Es wird beschrieben, wie Sie Richtlinien für bedingten Zugriff von Azure Active Directory verwenden, um den externen Zugriff auf Ressourcen zu schützen.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27c34135a59521eca361c59a1c82854469626616
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97743714"
---
# <a name="manage-external-access-with-conditional-access-policies"></a>Verwalten des externen Zugriffs mit Richtlinien für bedingten Zugriff 

Das Verfahren mit [bedingtem Zugriff](../conditional-access/overview.md) wird von Azure AD genutzt, um Signale zu vereinen, Richtlinien durchzusetzen und zu ermitteln, ob für einen Benutzer der Zugriff auf Ressourcen zulässig sein soll. Ausführliche Informationen zum Erstellen und Verwenden von Richtlinien für bedingten Zugriff finden Sie unter [Planen einer Bereitstellung für bedingten Zugriff](../conditional-access/plan-conditional-access.md). 

![Diagramm: Signale und Entscheidungen für bedingten Zugriff](media/secure-external-access//7-conditional-access-signals.png)



In diesem Artikel wird das Anwenden von Richtlinien für bedingten Zugriff auf externe Benutzer beschrieben. Hierbei wird vorausgesetzt, dass Sie über Zugriff auf die Funktion für die [Berechtigungsverwaltung](../governance/entitlement-management-overview.md) verfügen. Richtlinien für bedingten Zugriff können parallel zur Berechtigungsverwaltung eingesetzt werden.

Weiter oben in dieser Dokumentation haben Sie einen [Sicherheitsplan erstellt](3-secure-access-plan.md), mit dem Folgendes festgelegt wurde:

* Anwendungen und Ressourcen verfügen über die gleichen Sicherheitsanforderungen und können zu Zugriffszwecken gruppiert werden.

* Anmeldeanforderungen für externe Benutzer.

Sie verwenden diesen Plan zum Erstellen Ihrer Richtlinien für bedingten Zugriff, um den externen Zugriff zu steuern. 

> [!IMPORTANT]
> Erstellen Sie einige Testkonten für externe Benutzer, damit Sie die von Ihnen erstellten Richtlinien testen können, bevor Sie diese auf alle externen Benutzer anwenden.

## <a name="conditional-access-policies-for-external-access"></a>Richtlinien für bedingten Zugriff zum Steuern des externen Zugriffs

Hier sind die bewährten Methoden aufgeführt, die für das Steuern des externen Zugriffs mit Richtlinien für bedingten Zugriff gelten.

* Erstellen Sie eine Azure AD-Sicherheitsgruppe oder Microsoft 365-Gruppe für jede Partnerorganisation, mit der Sie zusammenarbeiten, falls die Verwendung von verbundenen Organisationen im Rahmen der Berechtigungsverwaltung für Sie nicht möglich ist. Weisen Sie der Gruppe alle Benutzer dieses Partners zu. Anschließend können Sie diese Gruppen in Richtlinien für bedingten Zugriff verwenden.

* Erstellen Sie möglichst wenige Richtlinien für bedingten Zugriff. Falls für mehrere Anwendungen die gleichen Zugriffsanforderungen gelten, sollten Sie sie alle derselben Richtlinie hinzufügen.  
‎ 
   > [!NOTE]
   > Richtlinien für bedingten Zugriff können auf maximal 250 Anwendungen angewendet werden. Erstellen Sie doppelte Richtlinien, falls mehr als 250 Apps über die gleichen Zugriffsanforderungen verfügen. Richtlinie A gilt für die Apps 1 bis 250, Richtlinie B für die Apps 251 bis 500 usw.

* Benennen Sie die für den externen Zugriff bestimmten Richtlinien auf eindeutige Weise, indem Sie eine entsprechende Namenskonvention verwenden. Eine Namenskonvention ist ‎*ExternalAccess_actiontaken_AppGroup*. Ein Beispiel hierfür lautet „ExternalAccess_Block_FinanceApps“.

## <a name="block-all-external-users-from-resources"></a>Blockieren aller externen Benutzer für Ressourcen

Sie können für externe Benutzer den Zugriff auf bestimmte Gruppen von Ressourcen blockieren, indem Sie Richtlinien für bedingten Zugriff nutzen. Erstellen Sie eine Richtlinie, nachdem Sie die Gruppe mit den Ressourcen ermittelt haben, für die Sie den Zugriff blockieren möchten.

Erstellen Sie wie folgt eine Richtlinie, mit der der Zugriff auf bestimmte Anwendungen für externe Benutzer blockiert wird:

1. Greifen Sie auf das **Azure-Portal** zu, und wählen Sie dann **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff** aus.

2. Wählen Sie die Option **Neue Richtlinie** aus, und geben Sie einen **Namen** ein, z. B. „ExternalAccess_Block_FinanceApps“.

3. Wählen Sie die Option **Benutzer und Gruppen** aus. Wählen Sie auf der Registerkarte „Einschließen“ die Option **Benutzer und Gruppen auswählen** und dann **Alle Gast- und externen Benutzer** aus. 

4. Wählen Sie **Ausschließen** aus, und geben Sie Ihre Administratorgruppe(n) sowie alle Konten für den Notfallzugriff ein.

5. Wählen Sie **Cloud-Apps oder -aktionen** > **Apps auswählen** und dann alle Apps aus, für die Sie den externen Zugriff blockieren möchten. Wählen Sie anschließend **Auswählen** aus.

6. Wählen Sie **Bedingungen** > **Standorte** und dann unter „Konfigurieren“ die Option **Ja** und **Alle Standorte** aus.

7. Wählen Sie unter „Zugriffssteuerung“ die Option **Gewähren** aus, und schalten Sie auf **Blockieren** um. Wählen Sie anschließend **Auswählen** aus.

8. Vergewissern Sie sich, dass die Einstellung „Richtlinie aktivieren“ auf **Nur Bericht** festgelegt ist, und wählen Sie dann **Erstellen** aus.

## <a name="block-external-access-to-all-except-specific-external-users"></a>Blockieren des externen Zugriffs (mit Ausnahme bestimmter externer Benutzer)

Es kann vorkommen, dass Sie den Zugriff für externe Benutzer bis auf eine bestimmte Gruppe blockieren möchten. Beispiel: Für alle externen Benutzer, die nicht für die Finanzabteilung arbeiten, soll der Zugriff auf Finanzanwendungen blockiert werden. Gehen Sie dazu wie folgt vor:

1. Erstellen Sie eine Sicherheitsgruppe für die externen Benutzer, die Zugriff auf die Gruppe für die Finanzabteilung haben sollen.

2. Führen Sie die Schritte 1 bis 3 des obigen Abschnitts „Blockieren aller externen Benutzer für Ressourcen“ aus.

3. Fügen Sie in Schritt 4 die Sicherheitsgruppe hinzu, für die die Blockierung der Finanzanwendungen nicht gelten soll.

4. Führen Sie die restlichen Schritte aus.

## <a name="implement-conditional-access"></a>Implementieren des bedingten Zugriffs

Für viele häufig genutzte Richtlinien für bedingten Zugriff sind Dokumentationsartikel vorhanden. Sehen Sie sich die Informationen in den folgenden Artikeln an, die Sie auch für externe Benutzer verwenden können.

* [Bedingter Zugriff: Anfordern der MFA für alle Benutzer](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)

* [Bedingter Zugriff anhand des Benutzerrisikos](../conditional-access/howto-conditional-access-policy-risk-user.md)

* [Gewusst wie: Vorschreiben der Verwendung der MFA für den Zugriff von nicht vertrauenswürdigen Netzwerken mit bedingtem Zugriff](../conditional-access/untrusted-networks.md) 

* [Nutzungsbedingungen für Azure Active Directory](../conditional-access/terms-of-use.md)

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie Informationen zum Schützen des externen Zugriffs auf Ressourcen. Wir empfehlen Ihnen, die Aktionen in der angegebenen Reihenfolge durchzuführen.

1. [Ermitteln des gewünschten Sicherheitsstatus für externen Zugriff](1-secure-access-posture.md)

2. [Ermitteln des aktuellen Status](2-secure-access-current-state.md)

3. [Erstellen eines Governanceplans](3-secure-access-plan.md)

4. [Verwenden von Gruppen für die Sicherheit](4-secure-access-groups.md)

5. [Durchführen der Umstellung auf Azure AD B2B](5-secure-access-b2b.md)

6. [Schützen des Zugriffs mit der Berechtigungsverwaltung](6-secure-access-entitlement-managment.md)

7. [Schützen des Zugriffs mit Richtlinien für bedingten Zugriff](7-secure-access-conditional-access.md) (dieser Artikel)

8. [Schützen des Zugriffs mit Vertraulichkeitsbezeichnungen](8-secure-access-sensitivity-labels.md)

9. [Schützen des Zugriffs auf Microsoft Teams, OneDrive und SharePoint](9-secure-access-teams-sharepoint.md)
