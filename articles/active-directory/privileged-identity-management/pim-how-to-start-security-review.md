---
title: Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM – Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) erstellen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 3/16/2021
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 310122177d4bd1603f5f498aa2a51620eeda4a20
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104592795"
---
# <a name="create-an-access-review-of-azure-ad-roles-in-privileged-identity-management"></a>Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen in Privileged Identity Management

Daher sollten Sie den Zugriff in regelmäßigen Abständen überprüfen, um das mit veralteten Rollenzuweisungen verbundene Risiko zu verringern. Mit Azure AD Privileged Identity Management (PIM) können Sie Zugriffsüberprüfungen für privilegierte Azure AD-Rollen erstellen. Sie können auch wiederholte Zugriffsüberprüfungen konfigurieren, die automatisch ausgeführt werden.

In diesem Artikel wird beschrieben, wie Sie eine oder mehrere Zugriffsüberprüfungen für privilegierte Azure AD-Rollen erstellen.

## <a name="prerequisites"></a>Voraussetzungen

[Administrator für privilegierte Rollen](../roles/permissions-reference.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Öffnen von Zugriffsüberprüfungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Benutzer an, der Mitglied der Rolle „Administrator für privilegierte Rollen“ ist.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

1. Wählen Sie unter „Verwalten“ die Option **Zugriffsüberprüfungen** und anschließend **Neu** aus.

    ![Azure AD-Rollen: Liste der Zugriffsüberprüfungen mit dem Status aller Überprüfungen](./media/pim-how-to-start-security-review/access-reviews.png)

Klicken Sie auf **Neu**, um eine neue Zugriffsüberprüfung zu erstellen.

1. Benennen Sie die Zugriffsüberprüfung. Wahlweise können Sie jeder Überprüfung eine Beschreibung hinzufügen. Den Prüfern werden Name und Beschreibung angezeigt.

    ![Erstellen einer Zugriffsüberprüfung – Name und Beschreibung der Überprüfung](./media/pim-how-to-start-security-review/name-description.png)

1. Legen Sie das **Startdatum** fest. Standardmäßig findet eine Zugriffsüberprüfung einmalig statt. Sie beginnt an dem Tag, an dem sie erstellt wird, und endet nach einem Monat. Sie können Start- und Enddatum so ändern, dass der Start der Zugriffsüberprüfung in der Zukunft liegt und sie so lange dauert, wie Sie wünschen.

    ![Startdatum, Häufigkeit, Dauer, Ende, Anzahl und Enddatum](./media/pim-how-to-start-security-review/start-end-dates.png)

1. Wenn die Zugriffsüberprüfung wiederholt ausgeführt werden soll, ändern Sie die Einstellung **Häufigkeit** von **Einmal** in **Wöchentlich**, **Monatlich**,  **Vierteljährlich**, **Jährlich** oder **Halbjährlich**. Verwenden Sie den Schieberegler oder das Textfeld **Dauer**, um festzulegen, wie viele Tage Prüfer Eingaben für jede Überprüfung der Serie vornehmen können. Für eine monatliche Überprüfung kann beispielsweise eine maximale Dauer von 27 Tagen angegeben werden, um Überschneidungen zu vermeiden.

1. Geben Sie mithilfe der Einstellung **Ende** an, wie die wiederkehrende Zugriffsüberprüfungsreihe beendet werden soll. Die Reihe kann auf drei Arten enden: Die Serie wird unendlich ausgeführt, um Überprüfungen ohne zeitliche Beschränkung zu starten, die Serie wird bis zu einem bestimmten Datum ausgeführt, oder sie wird nach einer bestimmten Anzahl von Vorkommen beendet. Sie (oder ein anderer Benutzeradministrator oder globaler Administrator) können die Serie nach der Erstellung beenden, indem Sie unter **Einstellungen** das Datum ändern, sodass die Serie an diesem Datum endet.

1. Wählen Sie im Abschnitt **Benutzer** eine oder mehrere Rollen aus, deren Mitgliedschaft überprüft werden soll.

    ![Benutzerbereich zum Überprüfen der Rollenmitgliedschaft](./media/pim-how-to-start-security-review/users.png)

    > [!NOTE]
    > - Die hier ausgewählten Rollen enthalten [permanente und berechtigte Rollen](../privileged-identity-management/pim-how-to-add-role-to-user.md).
    > - Bei der Auswahl mehrerer Rollen werden mehrere Zugriffsüberprüfungen erstellt. Bei der Auswahl von fünf Rollen werden z. B. fünf separate Zugriffsüberprüfungen erstellt.
    > - Für Rollen, denen Gruppen zugewiesen sind, wird der Zugriff auf jede Gruppe, die mit der zu überprüfenden Rolle verknüpft ist, im Rahmen der Zugriffsüberprüfung überprüft.
    Wenn Sie eine Zugriffsüberprüfung für **Azure AD-Rollen** erstellen, sieht die Liste der zu überprüfenden Mitgliedschaften in etwa wie im folgenden Beispiel aus.

    ![Bereich „Mitgliedschaft überprüfen“ mit aufgeführten Azure AD-Rollen, die Sie auswählen können](./media/pim-how-to-start-security-review/review-membership.png)

    Wenn Sie eine Zugriffsüberprüfung für **Azure-Ressourcenrollen** erstellen, sieht die Liste der zu überprüfenden Mitgliedschaften in etwa wie in der folgenden Abbildung aus.

    ![Bereich „Mitgliedschaft überprüfen“ mit aufgeführten Azure-Ressourcenrollen, die Sie auswählen können](./media/pim-how-to-start-security-review/review-membership-azure-resource-roles.png)

1. Wählen Sie im Abschnitt **Prüfer** mindestens eine Person für die Überprüfung aller Benutzer aus. Alternativ können Sie auswählen, dass die Mitglieder ihren eigenen Zugriff überprüfen.

    ![Liste der Prüfer mit ausgewählten Benutzer oder Mitgliedern (selbst)](./media/pim-how-to-start-security-review/reviewers.png)

    - **Ausgewählte Benutzer**: Verwenden Sie diese Option, wenn Sie nicht wissen, wer Zugriff benötigt. Mit dieser Option können Sie die Überprüfungsdurchführung einem Ressourcenbesitzer oder Gruppen-Manager zuweisen.
    - **Mitglieder (selbst)** : Mit dieser Option können Benutzer ihre Rollenzuweisungen selbst überprüfen. Gruppen, die der Rolle zugewiesen sind, unterliegen nicht der Überprüfung, wenn diese Option ausgewählt ist.
    - **Manager**: Verwenden Sie diese Option, wenn der Vorgesetzte des Benutzers seine Rollenzuweisung überprüfen soll. Wenn Sie „Manager“ auswählen, haben Sie auch die Möglichkeit, einen Fallbackprüfer anzugeben. Fallbackprüfer werden aufgefordert, eine Überprüfung für einen Benutzer durchzuführen, wenn für diesen kein Vorgesetzter (Manager) im Verzeichnis angegeben ist. Gruppen, die der Rolle zugewiesen sind, werden vom Fallbackprüfer überprüft, sofern ein solcher ausgewählt ist. 

### <a name="upon-completion-settings"></a>Einstellungen nach Abschluss

1. Erweitern Sie den Abschnitt **Einstellungen nach Abschluss**, um anzugeben, was nach Abschluss der Überprüfung geschehen soll.

    ![Automatisch anzuwendende „Einstellungen nach Abschluss“, und falls die Überprüfung nicht reagieren sollte](./media/pim-how-to-start-security-review/upon-completion-settings.png)

1. Soll abgelehnten Benutzern automatisch der Zugriff entzogen werden, legen Sie **Ergebnisse automatisch auf Ressource anwenden** auf **Aktivieren** fest. Falls Sie die Ergebnisse nach Abschluss der Überprüfung manuell anwenden möchten, legen Sie die Einstellung auf **Deaktivieren** fest.

1. Geben Sie mithilfe der Liste **Falls der Prüfer nicht reagiert** an, was bei Benutzern geschehen soll, die vom Prüfer nicht innerhalb des vorgesehenen Zeitraums überprüft werden. Diese Einstellung hat keine Auswirkungen auf Benutzer, die von den Prüfern manuell überprüft wurden. Lautet die Entscheidung des Prüfers letztlich „Verweigern“, wird dem Benutzer der Zugriff entzogen.

    - **Keine Änderung**: Der Zugriff des Benutzers bleibt unverändert.
    - **Zugriff entfernen**: Dem Benutzer wird der Zugriff entzogen.
    - **Zugriff genehmigen**: Der Zugriff des Benutzers wird genehmigt.
    - **Empfehlungen annehmen**: Die Systemempfehlungen hinsichtlich der Ablehnung oder Gewährung des weiteren Benutzerzugriffs werden verwendet.

### <a name="advanced-settings"></a>Erweiterte Einstellungen

1. Erweitern Sie den Abschnitt **Erweiterte Einstellungen**, um weitere Einstellungen anzugeben.

    ![„Erweiterte Einstellungen“ für „Empfehlungen anzeigen“, „Bei Genehmigung Grund anfordern“, E-Mail-Benachrichtigungen und Erinnerungen](./media/pim-how-to-start-security-review/advanced-settings.png)

1. Legen Sie **Empfehlungen anzeigen** auf **Aktivieren** fest, damit den Prüfern die Systemempfehlungen auf der Grundlage der Zugriffsinformationen des Benutzers angezeigt werden.

1. Legen Sie **Bei Genehmigung Grund anfordern** auf **Aktivieren** fest, damit der Prüfer einen Grund für die Genehmigung angeben muss.

1. Legen Sie **E-Mail-Benachrichtigungen** auf **Aktivieren** fest, damit Azure AD beim Start einer Zugriffsüberprüfung E-Mail-Benachrichtigungen an die Prüfer und beim Abschluss einer Überprüfung Benachrichtigungen an Administratoren sendet.

1. Legen Sie **Erinnerungen** auf **Aktivieren** fest, damit Azure AD Erinnerungen zu laufenden Zugriffsüberprüfungen an Prüfer sendet, die ihre Überprüfung noch nicht abgeschlossen haben.
1. Der Inhalt der an Prüfer gesendeten E-Mail wird automatisch basierend auf den Überprüfungsdetails generiert, z. B. Name der Überprüfung, Name der Ressource, Fälligkeitsdatum usw. Wenn Sie eine Möglichkeit benötigen, zusätzliche Informationen wie etwa weitere Anweisungen oder Kontaktinformationen mitzuteilen, können Sie diese Informationen in die **E-Mail mit zusätzlichen Inhalten für Prüfer** einfügen. Diese E-Mail wird in die Einladung sowie in Erinnerungs-E-Mails an die zugewiesenen Prüfer einbezogen. Diese Informationen werden in der folgenden Abbildung im hervorgehobenen Abschnitt angezeigt.

    ![Inhalt der an Prüfer gesendeten E-Mail mit Hervorhebungen](./media/pim-how-to-start-security-review/email-info.png)

## <a name="start-the-access-review"></a>Starten der Zugriffsüberprüfung

Wählen Sie nach dem Festlegen der Einstellungen für eine Zugriffsüberprüfung die Option **Starten** aus. Die Zugriffsüberprüfung wird in der Liste mit einer Angabe des Status angezeigt.

![Liste der Zugriffsüberprüfungen mit dem Status gestarteter Überprüfungen](./media/pim-how-to-start-security-review/access-reviews-list.png)

Standardmäßig sendet Azure AD kurz nach dem Start der Überprüfung eine E-Mail an die Prüfer. Wenn Sie nicht möchten, dass Azure AD die E-Mail sendet, stellen Sie sicher, dass die Prüfer darüber in Kenntnis gesetzt werden, dass sie eine ausstehende Zugriffsüberprüfung abschließen müssen. Sie können ihnen die Anweisungen zum [Durchführen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-perform-security-review.md) zeigen.

## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung

Sie können den Fortschritt der Überprüfungen durch die Prüfer auf der Seite **Übersicht** der Zugriffsüberprüfung nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](pim-how-to-complete-review.md) ist.

![Übersichtsseite der Zugriffsüberprüfung mit den Details der Überprüfung](./media/pim-how-to-start-security-review/access-review-overview.png)

Führen Sie bei einer einmaligen Überprüfung nach Ablauf des Zeitraums für die Zugriffsüberprüfung oder nach Beenden der Zugriffsüberprüfung durch den Administrator die Schritte unter [Abschließen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-complete-review.md) aus, um die Ergebnisse anzuzeigen und anzuwenden.  

Um eine Serie von Zugriffsüberprüfungen zu verwalten, navigieren Sie zur Zugriffsüberprüfung. Dort finden Sie unter den geplanten Überprüfungen die anstehenden Überprüfungen, und Sie können das Enddatum bearbeiten oder Prüfer entsprechend hinzufügen/entfernen.

Basierend auf Ihrer Auswahl unter **Einstellungen nach Abschluss** wird nach dem Enddatum der Überprüfung oder bei manueller Beendigung der Überprüfung die automatische Anwendung ausgeführt. Der Status der Überprüfung ändert sich von **Abgeschlossen** über Zwischenzustände wie **Wird angewandt** schließlich in den Status **Angewandt**. Erwartungsgemäß sollten abgelehnte Benutzer (sofern vorhanden) innerhalb weniger Minuten aus den Rollen entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen des Zugriffs auf Azure AD-Rollen](pim-how-to-perform-security-review.md)
- [Abschließen einer Zugriffsüberprüfung für Azure AD-Rollen](pim-how-to-complete-review.md)
- [Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen](pim-resource-roles-start-access-review.md)
