---
title: Verwalten von in den Richtlinien für den bedingten Zugriff ausgeschlossenen Benutzern – Azure AD
description: Erfahren Sie, wie Sie mit Azure Active Directory-Zugriffsüberprüfungen (Azure AD-Zugriffsüberprüfungen) Benutzer verwalten, die aus Richtlinien für den bedingten Zugriff ausgeschlossen wurden.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/24/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91bb5a342eea079b6e9abcf109ad472151d3c13d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82144500"
---
# <a name="use-azure-ad-access-reviews-to-manage-users-excluded-from-conditional-access-policies"></a>Verwenden von Azure AD-Zugriffsüberprüfungen zum Verwalten von Benutzern, die aus Richtlinien für den bedingten Zugriff ausgeschlossen sind

Im Idealfall unterliegen alle Benutzer den Zugriffsrichtlinien, um den Zugriff auf die Ressourcen Ihrer Organisation zu schützen. Es kann aber auch Geschäftsszenarien geben, in denen Ausnahmen erforderlich sind. In diesem Artikel werden einige Beispiele für Situationen beschrieben, in denen Ausschlüsse erforderlich sein können. Sie als IT-Administrator können diese Aufgabe verwalten, das Kontrollieren von Richtlinienausnahmen vermeiden und gegenüber Prüfern nachweisen, dass diese Ausnahmen regelmäßig mithilfe von Azure Active Directory-Zugriffsüberprüfungen (Azure AD) überprüft werden.

>[!NOTE]
> Eine gültige Azure AD Premium P2-, kostenpflichtige Enterprise Mobility + Security E5- oder Testlizenz ist erforderlich, um Azure AD-Zugriffsüberprüfungen verwenden zu können. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](../fundamentals/active-directory-whatis.md).

## <a name="why-would-you-exclude-users-from-policies"></a>Welche Gründe gibt es, Benutzer von Richtlinien auszuschließen?

Nehmen wir beispielsweise an, dass Sie als Administrator den [bedingten Zugriff von Azure AD](../conditional-access/overview.md) verwenden möchten, um Multi-Factor Authentication (MFA) anzufordern und die Authentifizierungsanforderungen auf bestimmte Netzwerke oder Geräte zu beschränken. Bei der Planung der Bereitstellung stellen Sie fest, dass nicht alle Benutzer diese Anforderungen erfüllen können. Beispielsweise kann es Benutzer geben, die in Außenstellen arbeiten und nicht zu Ihrem internen Netzwerk gehören. Sie müssen möglicherweise auch Benutzer berücksichtigen, die über nicht unterstützte Geräte eine Verbindung herstellen, während sie auf den Austausch dieser Geräte warten. Kurz gesagt, für die Geschäftstätigkeit ist es erforderlich, dass sich diese Benutzer anmelden und ihre Arbeit erledigen können. Daher schließen Sie sie aus den Richtlinien für bedingten Zugriff aus.

Ein weiteres Beispiel: Unter Umständen nutzen Sie im Rahmen des bedingten Zugriffs [benannte Orte](../conditional-access/location-condition.md), um eine Reihe von Ländern und Regionen zu konfigurieren, aus denen Benutzer nicht auf Ihren Mandanten zugreifen sollen.

![Benannte Standorte beim bedingten Zugriff](./media/conditional-access-exclusion/named-locations.png)

Unglücklicherweise kann es sein, dass sich aus gutem Grund doch noch einige Benutzer aus diesen blockierten Ländern bzw. Regionen anmelden müssen. Beispiel: Benutzer sind auf Dienstreise und benötigen Zugriff auf die Unternehmensressourcen. In diesem Fall könnten Sie in der Richtlinie für bedingten Zugriff, mit der diese Länder/Regionen blockiert werden, eine Cloudsicherheitsgruppe für aus der Richtlinie ausgeschlossene Benutzer verwenden. Benutzer, die unterwegs Zugriff benötigen, können sich der Gruppe durch das [Einrichten von Azure Active Directory für die Self-Service-Gruppenverwaltung](../users-groups-roles/groups-self-service-management.md) selbst hinzufügen.

Ein weiteres Beispiel: Sie haben eine Richtlinie für bedingten Zugriff, mit der die [Legacyauthentifizierung für den Großteil Ihrer Benutzer blockiert wird](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/07/azure-ad-conditional-access-support-for-blocking-legacy-auth-is-in-public-preview/). Wenn Sie jedoch einige Benutzer haben, die über Office 2010 oder IMAP/SMTP/POP-basierte Clients mit Legacyauthentifizierungsmethoden auf Ihre Ressourcen zugreifen müssen, können Sie diese Benutzer aus der Richtlinie ausschließen, mit der die Legacyauthentifizierung blockiert wird.

>[!NOTE]
>Microsoft empfiehlt dringend, dass Sie die Nutzung von Legacyprotokollen in Ihrem Mandanten blockieren, um Ihren Sicherheitsstatus zu verbessern.

## <a name="why-are-exclusions-challenging"></a>Warum stellen Ausschlüsse eine Herausforderung dar?

In Azure AD können Sie den Bereich für eine Richtlinie für bedingten Zugriff auf eine Gruppe von Benutzern festlegen. Sie können Ausschlüsse auch durch Auswahl von Azure AD-Rollen, einzelnen Benutzern oder Gästen konfigurieren. Bei der Konfiguration von Ausschlüssen sollten Sie beachten, dass der Zweck der Richtlinie bei den ausgeschlossenen Benutzern nicht durchgesetzt werden kann. Wenn Sie Ausschlüsse mithilfe einer Liste von Benutzern oder von lokalen älteren Sicherheitsgruppen konfigurieren, haben Sie nur begrenzten Einblick in die Ausschlüsse. Infolgedessen:

- Benutzer wissen möglicherweise nicht, dass sie ausgeschlossen wurden.

- Benutzer können der Sicherheitsgruppe beitreten, um die Richtlinie zu umgehen.

- Ausgeschlossene Benutzer waren möglicherweise zuvor für den Ausschluss qualifiziert, sind es aber nicht mehr.

Wenn Sie zum ersten Mal einen Ausschluss konfigurieren, gibt es häufig eine Shortlist von Benutzern, die die Richtlinie umgehen. Im Laufe der Zeit werden immer mehr Benutzer ausgeschlossen, und die Liste wird umfangreicher. An einem bestimmten Punkt müssen Sie die Liste überprüfen und bestätigen, dass die einzelnen Benutzer weiterhin für den Ausschluss qualifiziert sind. Aus technischer Sicht ist die Verwaltung der Liste wahrscheinlich relativ einfach. Aber wer trifft die geschäftlichen Entscheidungen, und wie stellen Sie sicher, dass alles überwachbar ist? Wenn Sie den Ausschluss jedoch mithilfe einer Azure AD-Gruppe konfigurieren, können Sie Zugriffsüberprüfungen als ausgleichendes Steuerelement nutzen, um die Sichtbarkeit zu erhöhen und die Anzahl von ausgeschlossenen Benutzern zu reduzieren.

## <a name="how-to-create-an-exclusion-group-in-a-conditional-access-policy"></a>Erstellen einer Ausschlussgruppe in einer Richtlinie für bedingten Zugriff

Führen Sie diese Schritte aus, um eine neue Azure AD-Gruppe und eine Richtlinie für bedingten Zugriff zu erstellen, die für diese Gruppe nicht gilt.

### <a name="create-an-exclusion-group"></a>Erstellen einer Ausschlussgruppe

1. Melden Sie sich beim Azure-Portal an.

2. Klicken Sie im linken Navigationsbereich auf **Azure Active Directory** und dann auf **Gruppen**.

3. Klicken Sie im oberen Menü auf **Neue Gruppe**, um den Gruppenbereich zu öffnen.

4. Wählen Sie in der Liste **Gruppentyp** die Option **Sicherheit**. Geben Sie einen Namen und eine Beschreibung an.

5. Stellen Sie sicher, dass Sie den Typ **Mitgliedschaft** auf **Zugewiesen** festlegen.

6. Wählen Sie die Benutzer aus, die Teil dieser Ausschlussgruppe sein sollen, und klicken Sie anschließend auf **Erstellen**.

![Neuer Gruppenbereich in Azure Active Directory](./media/conditional-access-exclusion/new-group.png)

### <a name="create-a-conditional-access-policy-that-excludes-the-group"></a>Erstellen einer Richtlinie für bedingten Zugriff, die die Gruppe ausschließt

Sie können jetzt eine Richtlinie für bedingten Zugriff erstellen, die diese Ausschlussgruppe verwendet.

1. Klicken Sie im linken Navigationsbereich auf **Azure Active Directory** und dann auf **Bedingter Zugriff**, um das Blatt **Richtlinien** zu öffnen.

2. Klicken Sie auf **Neue Richtlinie**, um den Bereich **Neu** zu öffnen.

3. Geben Sie einen Namen an.

4. Klicken Sie unter „Zuweisungen“ auf **Benutzer und Gruppen**.

5. Wählen Sie auf der Registerkarte **Einschließen** die Option **Alle Benutzer**.

6. Aktivieren Sie auf der Registerkarte **Ausschließen** das Kontrollkästchen **Benutzer und Gruppen**, und klicken Sie dann auf **Ausgeschlossene Benutzer auswählen**.

7. Wählen Sie die Ausschlussgruppe aus, die Sie erstellt haben.

   > [!NOTE] 
   > Die empfohlene bewährte Methode besteht darin, beim Testen mindestens ein Administratorkonto aus der Richtlinie auszuschließen, um sicherzustellen, dass Sie nicht aus Ihrem Mandanten ausgesperrt werden.

8. Fahren Sie mit dem Einrichten der Richtlinie für bedingten Zugriff basierend auf den Anforderungen Ihrer Organisation fort.

![Auswählen des Bereichs ausgeschlossener Benutzer im bedingten Zugriff](./media/conditional-access-exclusion/select-excluded-users.png)
  
Im Folgenden werden zwei Beispiele beschrieben, in denen Sie Zugriffsüberprüfungen nutzen können, um Ausschlüsse in Richtlinien für bedingten Zugriff zu verwalten.

## <a name="example-1-access-review-for-users-accessing-from-blocked-countriesregions"></a>Beispiel 1: Zugriffsüberprüfung für Benutzer, die aus blockierten Ländern/Regionen zugreifen

Angenommen, Sie verfügen über eine Richtlinie für bedingten Zugriff, die den Zugriff aus bestimmten Ländern/Regionen blockiert. Sie enthält eine Gruppe, die von der Richtlinie ausgeschlossen ist. Hier ist eine empfohlene Zugriffsüberprüfung angegeben, mit der die Mitglieder der Gruppe überprüft werden.

> [!NOTE] 
> Für die Erstellung von Zugriffsüberprüfungen ist die Rolle „Globaler Administrator“ oder „Benutzeradministrator“ erforderlich.

1. Die Überprüfung erfolgt wöchentlich.

2. Sie erfolgt auf unbegrenzte Zeit, um sicherzustellen, dass Sie diese Ausschlussgruppe immer auf dem aktuellen Stand halten.

3. Alle Mitglieder dieser Gruppe werden in die Überprüfung einbezogen.

4. Jeder Benutzer muss selbst nachweisen, dass er weiterhin Zugriff aus diesen blockierten Ländern/Regionen benötigt und deshalb ein Mitglied der Gruppe bleiben muss.

5. Wenn der Benutzer nicht auf die Überprüfungsanforderung reagiert, wird er automatisch aus der Gruppe entfernt und kann nicht mehr auf den Mandanten zugreifen, wenn er in diesen Ländern/Regionen unterwegs ist.

6. Aktivieren Sie E-Mail-Benachrichtigungen, damit die Benutzer über Beginn und Abschluss der Zugriffsüberprüfung informiert werden.

    ![Erstellen eines Zugriffsüberprüfungsbereichs für Beispiel 1](./media/conditional-access-exclusion/create-access-review-1.png)

## <a name="example-2-access-review-for-users-accessing-with-legacy-authentication"></a>Beispiel 2: Zugriffsüberprüfung für Benutzer, die per Legacyauthentifizierung zugreifen

Angenommen, Sie verfügen über eine Richtlinie für bedingten Zugriff, die den Zugriff für Benutzer mit Legacyauthentifizierung und älteren Clientversionen blockiert und eine Gruppe enthält, die aus der Richtlinie ausgeschlossen ist. Hier ist eine empfohlene Zugriffsüberprüfung angegeben, mit der die Mitglieder der Gruppe überprüft werden.

1. Bei dieser Überprüfung muss es sich um eine wiederkehrende Überprüfung handeln.

2. Alle Mitglieder der Gruppe müssen überprüft werden.

3. Sie kann so konfiguriert werden, dass die „Besitzer“ von Geschäftseinheiten als ausgewählte Prüfer aufgeführt werden.

4. Verwenden Sie die automatische Anwendung der Ergebnisse, und entfernen Sie Benutzer, die noch nicht genehmigt wurden, damit diese weiterhin die Methoden für die Legacyauthentifizierung nutzen können.

5. Es kann vorteilhaft sein, die Empfehlungen zu aktivieren, damit für die Prüfer großer Gruppen das Treffen von Entscheidungen vereinfacht wird.

6. Aktivieren Sie E-Mail-Benachrichtigungen, damit die Benutzer über den Beginn und den Abschluss der Zugriffsüberprüfung informiert sind.

    ![Erstellen eines Zugriffsüberprüfungsbereichs für Beispiel 2](./media/conditional-access-exclusion/create-access-review-2.png)

>[!IMPORTANT] 
>Wenn Sie über viele Ausschlussgruppen verfügen und deshalb mehrere Zugriffsüberprüfungen erstellen müssen, können Sie am Microsoft Graph Beta-Endpunkt jetzt eine API nutzen, um die Erstellung und Verwaltung programmgesteuert durchzuführen. Informationen zu den ersten Schritten finden Sie unter [Azure AD access reviews API reference](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/accessreviews_root) (Azure AD-Zugriffsüberprüfungen – API-Referenz) und [Example of retrieving Azure AD access reviews via Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/td-p/236096) (Beispiel für das Abrufen von Azure AD-Zugriffsüberprüfungen mit Microsoft Graph).

## <a name="access-review-results-and-audit-logs"></a>Zugreifen auf Überprüfungsergebnisse und Überwachungsprotokolle

Nachdem Sie nun alles eingerichtet haben (Gruppe, Richtlinie für bedingten Zugriff und Zugriffsüberprüfungen), können Sie die Ergebnisse der Überprüfungen überwachen und nachverfolgen.

1. Öffnen Sie im Azure-Portal das Blatt **Zugriffsüberprüfungen**.

2. Öffnen Sie das Steuerelement und das Programm, das Sie für die Verwaltung der Ausschlussgruppe erstellt haben.

3. Klicken Sie auf **Ergebnisse**, um anzuzeigen, für wen der Verbleib auf der Liste genehmigt und wer entfernt wurde.

    ![Ergebnisse der Zugriffsüberprüfungen zeigen, wer genehmigt wurde](./media/conditional-access-exclusion/access-reviews-results.png)

4. Klicken Sie anschließend auf **Überwachungsprotokolle**, um die Aktionen anzuzeigen, die während der Überprüfung durchgeführt wurden.

    ![Überwachungsprotokolle von Zugriffsüberprüfungen, die Aktionen auflisten](./media/conditional-access-exclusion/access-reviews-audit-logs.png)

Als IT-Administrator wissen Sie, dass sich die Verwaltung von Ausschlussgruppen für Ihre Richtlinien manchmal nicht verhindern lässt. Mit Azure AD-Zugriffsüberprüfungen können Sie jedoch die Wartung dieser Gruppen, die regelmäßige Überprüfung durch den Geschäftsinhaber oder die Benutzer selbst sowie die Überprüfung dieser Änderungen deutlich vereinfachen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Zugriffsüberprüfung von Gruppen oder Anwendungen](create-access-review.md)
- [Was ist bedingter Zugriff?](../conditional-access/overview.md)
