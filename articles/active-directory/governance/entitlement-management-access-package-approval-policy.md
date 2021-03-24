---
title: Ändern der Genehmigungseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Hier erfahren Sie, wie Sie die Einstellungen zu Genehmigungs- und Anfordererinformationen für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung ändern.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 09/16/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48b5260e883d85899953240f6ee4f83127681c9e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97591298"
---
# <a name="change-approval-and-requestor-information-preview-settings-for-an-access-package-in-azure-ad-entitlement-management"></a>Ändern Sie die Einstellungen zu Genehmigungs- und Anfordererinformationen (Vorschau) für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung.

Als Zugriffspaket-Manager können Sie die Einstellungen zu Genehmigungs- und Anfordererinformationen für ein Zugriffspaket jederzeit ändern, indem Sie eine vorhandene Richtlinie bearbeiten oder eine neue Richtlinie hinzufügen.

In diesem Artikel wird beschrieben, wie die Einstellungen zu Genehmigungs- und Anfordererinformationen für ein vorhandenes Zugriffspaket geändert werden.

## <a name="approval"></a>Genehmigung

Im Abschnitt „Anforderung“ geben Sie an, ob eine Genehmigung erforderlich ist, wenn Benutzer dieses Zugriffspaket anfordern. Die Genehmigungseinstellungen funktionieren wie folgt:

- Bei einstufiger Genehmigung muss nur eine der ausgewählten genehmigenden Personen bzw. genehmigenden Fallbackpersonen eine Anforderung genehmigen. 
- Bei zweistufiger Genehmigung muss nur eine der ausgewählten genehmigenden Personen pro Stufe eine Anforderung genehmigen.
- Die genehmigende Person kann ein Vorgesetzter, ein interner Sponsor oder ein externer Sponsor sein, je nachdem, bei wem die Steuerung des Zugriffs gemäß der Richtlinie liegt.
- Weder für die einstufige noch für die zweistufige Genehmigung ist die Genehmigung durch jede ausgewählte genehmigende Person erforderlich.
- Die Genehmigungsentscheidung basiert darauf, welche genehmigende Person die Anforderung zuerst überprüft.

Eine Demonstration, wie einer Anforderungsrichtlinie genehmigende Personen hinzugefügt werden, finden Sie im folgenden Video:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4cZfg]

Eine Demonstration, wie einer Anforderungsrichtlinie mehrstufige Genehmigung hinzugefügt wird, finden Sie im folgenden Video:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4d1Jw]


## <a name="change-approval-settings-of-an-existing-access-package"></a>Ändern der Genehmigungseinstellungen eines vorhandenen Zugriffspakets

Führen Sie diese Schritte aus, um die Genehmigungseinstellungen für Anforderungen des Zugriffspakets festzulegen:

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Wählen Sie eine Richtlinie aus, um eine neue Richtlinie zu bearbeiten oder dem Zugriffspaket hinzuzufügen.
    1. Klicken Sie auf **Richtlinien** und dann auf **Richtlinie hinzufügen**, wenn Sie eine neue Richtlinie erstellen möchten.
    1. Klicken Sie auf die Richtlinie, die Sie bearbeiten möchten, und dann auf **Bearbeiten**.

1. Wechseln Sie zur Registerkarte **Anforderung**.

1. Um die Genehmigung für Anforderungen der ausgewählten Benutzer anzufordern, setzen Sie **Genehmigung anfordern** auf **Ja**. Stellen Sie den Umschalter auf **Nein**, um Anforderungen automatisch genehmigen zu lassen.

1. Um von Benutzern eine Begründung für die Anforderung des Zugriffspakets anzufordern, legen Sie die Umschaltfläche **Begründung des Anforderers erforderlich** auf **Ja** fest.
    
1. Bestimmen Sie jetzt, ob für Anforderungen eine einstufige oder eine zweistufige Genehmigung erforderlich ist. Stellen Sie den Umschalter **Wie viele Phasen** für einstufige Genehmigung auf **1** oder für zweistufige Genehmigung auf **2**.

    ![Zugriffspaket – Anforderungen – Genehmigungseinstellungen](./media/entitlement-management-access-package-approval-policy/approval.png)


Verwenden Sie die folgenden Schritte, um genehmigende Personen hinzuzufügen, nachdem Sie die Anzahl der erforderlichen Stufen festgelegt haben: 

### <a name="single-stage-approval"></a>Einstufige Genehmigung

1. Fügen Sie die **erste genehmigende Person** hinzu:
    
    Wenn Sie Richtlinie Ihnen die Steuerung des Zugriffs für Benutzer in Ihrem Verzeichnis gestattet, können Sie **Vorgesetzter als genehmigende Person** auswählen. Fügen Sie alternativ einen bestimmten Benutzer hinzu, indem Sie nach dem Auswählen von „Bestimmte genehmigende Personen auswählen“ im Dropdownmenü´auf **Genehmigende Personen hinzufügen** klicken.
    
    ![Zugriffspaket – Anforderungen – Für Benutzer im Verzeichnis – Erste genehmigende Person](./media/entitlement-management-access-package-approval-policy/approval-single-stage-first-approver-manager.png)

    Wenn diese Richtlinie so eingestellt ist, dass sie den Zugriff für Benutzer steuert, die sich nicht in Ihrem Verzeichnis befinden, können Sie **Externer Sponsor** oder **Interner Sponsor** auswählen. Oder fügen Sie einen bestimmten Benutzer oder eine Gruppe hinzu, indem Sie unter „Bestimmte genehmigende Personen auswählen“ auf **Genehmigende Personen hinzufügen** klicken.
    
    ![Zugriffspaket – Anforderungen – Für Benutzer außerhalb des Verzeichnisses – Erste genehmigende Person](./media/entitlement-management-access-package-approval-policy/out-directory-first-approver.png)
    
1. Wenn Sie **Vorgesetzter** als erste genehmigende Person ausgewählt haben, klicken Sie auf **Fallback hinzufügen**, um einen oder mehrere Benutzer oder Gruppen in Ihrem Verzeichnis auszuwählen, die als genehmigende Fallbackperson gelten sollen. Genehmigende Fallbackpersonen empfangen die Anforderung, wenn die Berechtigungsverwaltung den Vorgesetzten des Benutzers, der Zugriff anfordert, nicht finden kann.

    Der Vorgesetzte wird von der Berechtigungsverwaltung anhand des Attributs **Vorgesetzter** gefunden. Das Attribut befindet sich im Profil des Benutzers in Azure AD. Weitere Informationen finden Sie unter [Hinzufügen oder Aktualisieren von Benutzerprofilinformationen mit Azure Active Directory](../fundamentals/active-directory-users-profile-azure-portal.md).

1. Wenn Sie **Bestimmte genehmigende Personen auswählen** ausgewählt haben, klicken Sie auf **Genehmigende Person hinzufügen**, um einen oder mehrere Benutzer oder Gruppen in Ihrem Verzeichnis auszuwählen, die genehmigende Personen sein sollen.

1. Geben Sie im Feld unter **Zulässige Anzahl von Tagen für Entscheidungsfindung** die Anzahl der Tage an, die eine genehmigende Person Zeit hat, um eine Anforderung für dieses Zugriffspaket zu überprüfen.

    Wenn eine Anforderung nicht innerhalb dieses Zeitraums genehmigt wird, wird sie automatisch abgelehnt. Der Benutzer muss eine weitere Anforderung für das Zugriffspaket senden.

1. Um von genehmigenden Personen eine Rechtfertigung ihrer Entscheidung anzufordern, legen Sie „Begründung der genehmigenden Person erforderlich“ auf **Ja** fest.

    Die Begründung ist für andere genehmigende Personen und den Anfordernden sichtbar.

### <a name="2-stage-approval"></a>Zweistufige Genehmigung

Wenn Sie eine zweistufige Genehmigung ausgewählt haben, müssen Sie eine zweite genehmigende Person hinzufügen.

1. Hinzufügen der **zweiten genehmigenden Person**: 
    
    Wenn sich die Benutzer in Ihrem Verzeichnis befinden, fügen Sie einen bestimmten Benutzer als zweite genehmigende Person hinzu, indem Sie unter „Bestimmte genehmigende Personen auswählen“ auf **Genehmigende Personen hinzufügen** klicken.

    ![Zugriffspaket – Anforderungen – Für Benutzer im Verzeichnis – Zweite genehmigende Person](./media/entitlement-management-access-package-approval-policy/in-directory-second-approver.png)

    Wenn sich die Benutzer nicht in Ihrem Verzeichnis befinden, wählen Sie **Interner Sponsor** oder **Externer Sponsor** als zweite genehmigende Person aus. Fügen Sie nach dem Auswählen der genehmigenden Person die genehmigenden Fallbackpersonen hinzu.

    ![Zugriffspaket – Anforderungen – Für Benutzer außerhalb des Verzeichnisses – Zweite genehmigende Person](./media/entitlement-management-access-package-approval-policy/out-directory-second-approver.png) 

1. Geben Sie in dem Feld unter **Zulässige Anzahl von Tagen für Entscheidungsfindung** die Anzahl der Tage an, die der zweiten genehmigenden Person für die Genehmigung der Anforderung zur Verfügung stehen. 

1. Legen Sie den Umschalter „Begründung der genehmigenden Person erforderlich“ auf **Ja** oder **Nein** fest.

### <a name="alternate-approvers"></a>Alternative genehmigende Personen

Sie können neben den ersten und zweiten genehmigenden Personen alternative genehmigende Personen angeben, die Anforderungen genehmigen können. Das Festlegen alternativer genehmigender Personen trägt dazu bei, dass die Anforderungen vor ihrem Ablauf (Timeout) genehmigt oder abgelehnt werden. Beim Auflisten genehmigender Personen kann bei zweistufiger Genehmigung zwischen erster und zweiter genehmigender Person unterschieden werden. 

Durch das Angeben von alternativen genehmigenden Personen kann eine ausstehende Anforderung gemäß dem von Ihnen bei der Richtlinieneinrichtung angegebenen Weiterleitungsplan an die alternativen genehmigenden Personen weitergeleitet werden, wenn die ersten oder zweiten genehmigenden Personen die Anforderung nicht genehmigen oder ablehnen können. Die alternativen genehmigenden Personen erhalten eine E-Mail mit der Aufforderung, die ausstehende Anforderung zu genehmigen oder abzulehnen.

Nachdem die Anforderung an die alternativen genehmigenden Personen weitergeleitet wurde, können die ersten oder zweiten genehmigenden Personen den Antrag dennoch weiterhin genehmigen oder ablehnen. Alternative genehmigende Personen nutzen dieselbe Website „Mein Zugriff“, um die ausstehende Anforderung zu genehmigen oder abzulehnen.

Es können Personen oder Gruppen von Personen aufgelistet werden, die als genehmigende Personen und alternative genehmigende Personen fungieren sollen. Bitte achten Sie darauf, dass Sie verschiedene Gruppen von Personen auflisten, die als erste, zweite und alternative genehmigende Personen fungieren sollen.
Wenn Sie beispielsweise Alice und Bob als erste genehmigende Personen aufgelistet haben, führen Sie Carol und Dave als alternative genehmigende Personen auf. Führen Sie die folgenden Schritte aus, um einem Zugriffspaket alternative genehmigende Personen hinzuzufügen:

1. Klicken Sie unter der ersten genehmigenden Person, der zweiten genehmigenden Person oder unter beiden auf **Erweiterte Anforderungseinstellungen anzeigen**.

    :::image type="content" source="media/entitlement-management-access-package-approval-policy/alternate-approvers-click-advanced-request.png" alt-text="Zugriffspakete – Richtlinie – Erweiterte Anforderungseinstellungen anzeigen":::

1. Legen Sie die Umschaltfläche **Wenn keine Aktion ausgeführt wird, an alternative genehmigende Personen weiterleiten?** auf **Ja** fest.

1. Klicken Sie auf **Alternative genehmigende Personen hinzufügen**, und wählen Sie die alternativen genehmigende Personen aus der Liste aus.

    ![Zugriffspaket – Richtlinie – Alternative genehmigende Personen hinzufügen](./media/entitlement-management-access-package-approval-policy/alternate-approvers-add.png)

    Wenn Sie als „Erste genehmigende Person“ den Vorgesetzten auswählen, können Sie im Feld „Alternative genehmigende Person“ zusätzlich die Option **Mitglied der zweiten Führungsebene als alternative genehmigende Person** auswählen. Wenn Sie diese Option auswählen, müssen Sie eine stellvertretende genehmigende Person hinzufügen, an die die Anforderung weitergeleitet wird, wenn das System das Mitglied der zweiten Führungsebene nicht finden kann.

1. Geben Sie im Feld **Nach wie viel Tagen an alternative genehmigende Personen weiterleiten?** die Anzahl von Tagen ein, die Sie den genehmigenden Personen zum Genehmigen oder Ablehnen einer Anforderung einräumen. Wenn nach Ablauf der Anforderungsdauer keine der genehmigenden Personen die Anforderung genehmigt oder abgelehnt hat, läuft die Anforderung ab (Timeout), und der Benutzer muss eine weitere Anforderung für das Zugangspaket senden. 

    Anforderungen können erst einen Tag nach Erreichen der Hälfte der Anforderungsdauer an alternative genehmigende Personen weitergeleitet werden, und für die Entscheidung der Hauptgenehmiger muss ein Timeout von mindestens vier Tagen verwendet werden. Bei einem Anforderungstimeout von bis zu drei Tagen reicht die Zeit nicht aus, um die Anforderung an alternative genehmigende Personen weiterzuleiten. In diesem Beispiel beträgt die Dauer der Anforderung 14 Tage. Die Hälfte der Anforderungsdauer wird also an Tag 7 erreicht. Die Anforderung kann deshalb erst an Tag 8 weitergeleitet werden. Darüber hinaus können Anforderungen nicht am letzten Tag der Antragsdauer weitergeleitet werden. Im Beispiel kann die Anforderung also spätestens an Tag 13 weitergeleitet werden.

## <a name="enable-requests"></a>Ermöglichen von Anforderungen

1. Wenn das Zugriffspaket Benutzern in der Anforderungsrichtlinie sofort zum Anfordern zur Verfügung gestellt werden soll, legen Sie den Umschalter auf **Ja** fest.

    Sie können sie in der Zukunft immer aktivieren, nachdem Sie das Erstellen des Zugriffspakets abgeschlossen haben.

    Wenn Sie **Keine (nur direkte Administratorzuweisungen)** ausgewählt haben und Sie die Aktivierung auf **Nein** festlegen, können Administratoren dieses Zugriffspaket nicht direkt zuweisen.

    ![Zugriffspaket – Richtlinie – Einstellung „Richtlinie aktivieren“](./media/entitlement-management-access-package-approval-policy/enable-requests.png)

1. Klicken Sie auf **Weiter**.

## <a name="collect-additional-requestor-information-for-approval-preview"></a>Sammeln zusätzlicher Anfordererinformationen für die Genehmigung (Vorschau)

Damit sichergestellt ist, dass Benutzer Zugriff auf die richtigen Zugriffspakete erhalten, können Sie anfordern, dass Anforderer zum Zeitpunkt der Anforderung eine Antwort in ein benutzerdefiniertes Textfeld eingeben oder Multiple-Choice-Fragen beantworten müssen. Es besteht eine Beschränkung auf 20 Fragen pro Richtlinie und 25 Antworten auf Multiple-Choice-Fragen. Die Fragen werden dann den genehmigenden Personen angezeigt, um ihnen die Entscheidung zu erleichtern.

1. Wechseln Sie zur Registerkarte **Anfordererinformationen**, und klicken Sie auf die Unterregisterkarte **Fragen**.
 
1. Geben Sie als Frage in das Feld **Frage** ein, was Sie den Anforderer fragen möchten (auch als Anzeigezeichenfolge bezeichnet).

    ![Zugriffspaket – Richtlinie – Einstellung „Anfordererinformationen aktivieren“](./media/entitlement-management-access-package-approval-policy/add-requestor-info-question.png)

1. Wenn die Benutzer, die Zugriff auf das Zugriffspaket benötigen, nicht alle die gleiche bevorzugte Sprache verwenden, können Sie die Benutzerfreundlichkeit für Benutzer verbessern, die Zugriff auf myaccess.microsoft.com anfordern. Hierzu können Sie alternative Anzeigezeichenfolgen für verschiedene Sprachen bereitstellen. Wenn z. B. der Webbrowser eines Benutzers auf Spanisch festgelegt ist und Sie die Anzeigezeichenfolgen für Spanisch konfiguriert haben, werden dem anfordernden Benutzer diese Zeichenfolgen angezeigt. Um die Lokalisierung für Anforderungen zu konfigurieren, klicken Sie auf **Lokalisierung hinzufügen**.
    1. Wenn Sie sich im Bereich **Lokalisierungen für Frage hinzufügen** befinden, wählen Sie den **Sprachcode** für die Sprache aus, in die Sie die Frage lokalisieren.
    1. Geben Sie die Frage in der von Ihnen konfigurierten Sprache das Feld **Lokalisierter Text** ein.
    1. Nachdem Sie alle benötigten Lokalisierungen hinzugefügt haben, klicken Sie auf **Speichern**.

    ![Zugriffspaket – Richtlinie – Lokalisierten Text konfigurieren](./media/entitlement-management-access-package-approval-policy/add-localization-question.png)

1. Wählen Sie das **Antwortformat** aus, in dem Anforderer antworten sollen. Antwortformate umfassen: *Kurzer Text*, *Mehrfachauswahl* und *Langer Text*.
 
    ![Zugriffspaket – Richtlinie – „Anzeigen und bearbeiten“ für Antwortformat von Multiple Choice auswählen](./media/entitlement-management-access-package-approval-policy/answer-format-view-edit.png)
 
1. Wenn Sie „Multiple Choice“ auswählen, klicken Sie auf die Schaltfläche **Anzeigen und bearbeiten**, um die Antwortoptionen zu konfigurieren.
    1. Nachdem Sie „Anzeigen und bearbeiten“ ausgewählt haben, wird der Bereich **Frage anzeigen/bearbeiten** angezeigt.
    1. Geben Sie die Antwortoptionen in die Felder **Antwortwerte** ein, die Sie dem Anforderer zur Verfügung stellen möchten, wenn dieser die Frage beantwortet.
    1. Geben Sie so viele Antworten ein, wie Sie benötigen, und klicken Sie dann auf **Speichern**.
    
    ![Zugriffspaket – Richtlinie – Multiple Choice-Optionen eingeben](./media/entitlement-management-access-package-approval-policy/answer-multiple-choice.png)
  
1. Damit der Anforderer die Frage beantworten muss, wenn dieser den Zugriff auf ein Zugriffspaket anfordert, klicken Sie auf das Kontrollkästchen unter **Erforderlich**.

1. Füllen Sie die restlichen Registerkarten (z. B. „Lebenszyklus“) nach Bedarf aus.

Nachdem Sie Anfordererinformationen in Ihrer Zugriffspaketrichtlinie konfiguriert haben, können Sie die Antworten der Anforderer auf die Fragen anzeigen. Anleitungen zum Anzeigen von Anfordererinformationen finden Sie unter [Anzeigen der Antworten des Anforderers auf Fragen (Vorschau)](entitlement-management-request-approve.md#view-requestors-answers-to-questions-preview).

## <a name="next-steps"></a>Nächste Schritte
- [Ändern der Lebenszykluseinstellungen für ein Zugriffspaket](entitlement-management-access-package-lifecycle-policy.md)
- [Anzeigen der Anforderungen für ein Zugriffspaket](entitlement-management-access-package-requests.md)
