---
title: Erste Schritte mit dem Azure Enterprise Portal
description: In diesem Artikel wird erläutert, wie Azure EA-Kunden (Azure Enterprise Agreement) das Azure Enterprise Portal verwenden.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/04/2020
ms.topic: conceptual
ms.service: cost-management-billing
manager: boalcsva
ms.openlocfilehash: 516b3704daee885541cec49b0ee374e050dd4b22
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77023343"
---
# <a name="get-started-with-the-azure-enterprise-portal"></a>Erste Schritte mit dem Azure Enterprise Portal

Dieser Artikel unterstützt direkte und indirekte Azure EA-Kunden (Azure Enterprise Agreement) beim Einstieg in die Verwendung des [Azure Enterprise Portals](https://ea.azure.com). Sie erhalten grundlegende Informationen zu folgenden Themen:

- Struktur des Azure Enterprise Portals
- Im Azure Enterprise Portal verwendete Rollen
- Erstellung von Abonnements
- Kostenanalyse im Azure Enterprise Portal und im Azure-Portal

In diesem Video können Sie sich eine vollständige Onboardingsitzung für das Azure Enterprise Portal ansehen:

> [!VIDEO https://www.youtube.com/embed/OiZ1GdBpo-I]

## <a name="azure-enterprise-portal-hierarchy"></a>Hierarchie des Azure Enterprise Portals

Die Hierarchie des Azure Enterprise Portal besteht aus folgenden Komponenten:

- **Azure Enterprise Portal**: Ein Onlineverwaltungsportal, das Sie bei der Verwaltung der Kosten Ihrer Azure EA-Dienste unterstützt. Ihre Möglichkeiten:

  - Erstellen Sie eine Azure EA-Hierarchie mit Abteilungen, Konten und Abonnements.
  - Gleichen Sie die Kosten Ihrer genutzten Dienste ab, laden Sie Nutzungsberichte herunter, und sehen Sie sich Preislisten an.
  - Erstellen Sie API-Schlüssel für Ihre Registrierung.

- **Abteilungen** helfen Ihnen beim Einteilen von Kosten in logische Gruppierungen. Mit Abteilungen können Sie ein Budget oder ein Kontingent auf Abteilungsebene festlegen.

- **Konten** sind Organisationseinheiten im Azure Enterprise Portal. Anhand von Konten können Sie Abonnements verwalten und auf Berichte zugreifen.

- **Abonnements** sind die kleinste Einheit im Azure Enterprise Portal. Dabei handelt es sich um Container für Azure-Dienste, die vom Dienstadministrator verwaltet werden.

Das folgende Diagramm veranschaulicht einfache Azure EA-Hierarchien.

![Diagramm einfacher Azure EA-Hierarchien](./media/ea-portal-get-started/ea-hierarchies.png)

## <a name="enterprise-user-roles"></a>Unternehmensbenutzerrollen

Die folgenden administrativen Benutzerrollen sind Teil Ihrer Unternehmensregistrierung:

- Unternehmensadministrator
- Abteilungsadministrator
- Kontobesitzer
- Dienstadministrator
- Benachrichtigungskontakt

Rollen werden in zwei unterschiedlichen Portalen verwendet, um Aufgaben abzuschließen. Im [Azure Enterprise Portal](https://ea.azure.com) verwalten Sie Abrechnung und Kosten, und im [Azure-Portal](https://portal.azure.com) werden Azure-Dienste verwaltet.

Benutzerrollen sind einem Benutzerkonto zugeordnet. Zum Überprüfen der Authentizität des Benutzers muss jeder Benutzer über ein gültiges Geschäfts-, Schul- oder Unikonto oder über ein Microsoft-Konto verfügen. Stellen Sie sicher, dass jedes Konto einer E-Mail-Adresse zugeordnet ist, die aktiv überwacht wird. Kontobenachrichtigungen werden an diese E-Mail-Adresse gesendet.

Beim Einrichten von Benutzern können Sie der Rolle „Unternehmensadministrator“ mehrere Konten zuweisen. Allerdings kann nur ein Konto die Rolle „Kontobesitzer“ aufweisen. Außerdem können Sie sowohl die Rolle „Unternehmensadministrator“ als auch die Rolle „Kontobesitzer“ einem einzigen Konto zuweisen.

### <a name="enterprise-administrator"></a>Unternehmensadministrator

Benutzer mit dieser Rolle verfügen über die höchste Zugriffsebene. Sie können folgende Aktionen ausführen:

- Verwalten von Konten und Kontobesitzern
- Verwalten anderer Unternehmensadministratoren
- Verwalten von Abteilungsadministratoren
- Verwalten von Benachrichtigungskontakten
- Anzeigen der Nutzung für alle Konten
- Anzeigen nicht in Rechnung gestellter Gebühren für alle Konten

In einer Unternehmensregistrierung kann es mehrere Unternehmensadministratoren geben. Sie können Unternehmensadministratoren Lesezugriff gewähren. Sie erben alle die Abteilungsadministratorrolle.

### <a name="department-administrator"></a>Abteilungsadministrator

Benutzer mit dieser Rolle können folgende Aktionen ausführen:

- Erstellen und Verwalten von Abteilungen
- Erstellen neuer Kontobesitzer
- Anzeigen von Nutzungsdetails für die von ihnen verwalteten Abteilungen
- Anzeigen von Kosten, wenn die erforderlichen Berechtigungen gewährt wurden

In einer Unternehmensregistrierung kann es mehrere Abteilungsadministratoren geben.

Sie können Abteilungsadministratoren Lesezugriff erteilen, wenn Sie einen Abteilungsadministrator bearbeiten oder erstellen. Legen Sie die Option „Schreibgeschützt“ auf **Ja** fest.

### <a name="account-owner"></a>Kontobesitzer

Benutzer mit dieser Rolle können folgende Aktionen ausführen:

- Erstellen und Verwalten von Abonnements
- Verwalten von Dienstadministratoren
- Anzeigen der Nutzung für Abonnements

Für jedes Konto ist ein eindeutiges Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto erforderlich. Weitere Informationen zu den administrativen Rollen im Azure Enterprise Portal finden Sie unter [Informationen zu Azure Enterprise Agreement-Administratorrollen in Azure](understand-ea-roles.md).

### <a name="service-administrator"></a>Dienstadministrator

Die Rolle des Dienstadministrators verfügt über Berechtigungen zum Verwalten von Diensten im Azure-Portal und kann der Rolle „Co-Administrator“ Benutzer zuweisen.

### <a name="notification-contact"></a>Benachrichtigungskontakt

Der Benachrichtigungskontakt empfängt Nutzungsbenachrichtigungen zur Registrierung.

## <a name="activate-your-enrollment"></a>Aktivieren Ihrer Registrierung

Um den Dienst zu aktivieren, öffnet der erste Unternehmensadministrator das [Azure Enterprise Portal](https://ea.azure.com) und meldet sich mit der E-Mail-Adresse aus der Einladungs-E-Mail an.

Sollten Sie als Unternehmensadministrator eingerichtet sein, benötigen Sie keine Aktivierungs-E-Mail. Stattdessen können Sie zum [Azure Enterprise Portal](https://ea.azure.com) wechseln und sich mit der E-Mail-Adresse Ihres Geschäfts-, Schul-, Uni- oder Microsoft-Kontos und Ihrem Kennwort anmelden.

Wenn Sie über mehrere Registrierungen verfügen, wählen Sie eine aus, die Sie aktivieren möchten. Standardmäßig werden nur aktive Registrierungen angezeigt. Um den Registrierungsverlauf anzuzeigen, deaktivieren Sie die Option **Aktiv** in der oberen rechten Ecke des Azure Enterprise Portals.

Unter **Registrierung** wird der Status **Aktiv** angezeigt.

![Beispiel für eine aktive Registrierung](./media/ea-portal-get-started/ea-enrollment-status.png)

Nur vorhandene Azure-Unternehmensadministratoren können andere Unternehmensadministratoren erstellen.

### <a name="create-another-enterprise-administrator"></a>Erstellen eines weiteren Unternehmensadministrators

So fügen Sie einen weiteren Unternehmensadministrator hinzu

1. Melden Sie sich beim [Azure Enterprise Portal](https://ea.azure.com) an.
1. Wechseln Sie zu **Verwalten** > **Registrierungsdetail**.
1. Wählen Sie rechts oben **+ Administrator hinzufügen** aus.

Stellen Sie sicher, dass Sie über die E-Mail-Adresse und die bevorzugte Authentifizierungsmethode des Benutzers verfügen, wie etwa ein Geschäfts-, Schul-, Uni- oder Microsoft-Konto.

Wenn Sie kein Unternehmensadministrator sind, bitten Sie einen Unternehmensadministrator, Sie einer Registrierung hinzuzufügen. Nachdem Sie einer Registrierung hinzugefügt wurden, erhalten Sie eine Aktivierungs-E-Mail.

Wenn Ihr Unternehmensadministrator Ihnen nicht helfen kann, erstellen Sie eine [Supportanfrage für das Azure Enterprise Portal](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c). Geben Sie die folgenden Informationen ein:

- Registrierungsnummer
- Hinzuzufügende E-Mail-Adresse und Authentifizierungstyp (Geschäfts-, Schul-, Uni- oder Microsoft-Konto)
- E-Mail-Genehmigung von einem vorhandenen Unternehmensadministrator
  - Wenn der vorhandene Unternehmensadministrator nicht verfügbar ist, bitten Sie Ihren Partner oder Softwareberater, die Kontaktinformationen über das VLSC-Tool (Volume Licensing Service Center) zu ändern.

Weitere Informationen zu den Unternehmensadministratorrollen finden Sie unter [Informationen zu Azure Enterprise Agreement-Administratorrollen in Azure](understand-ea-roles.md).

## <a name="create-an-azure-enterprise-department"></a>Erstellen einer Azure Enterprise-Abteilung

Unternehmensadministratoren und Abteilungsadministratoren verwenden Abteilungen zum Organisieren und Erstellen von Berichten zu Azure-Diensten für Unternehmen und zur Nutzung nach Abteilung und Kostenstelle. Der Unternehmensadministrator kann folgende Aktionen ausführen:

- Hinzufügen oder Entfernen von Abteilungen
- Zuordnen eines Kontos zu einer Abteilung
- Erstellen von Abteilungsadministratoren
- Zulassen, dass Abteilungsadministratoren Preise und Kosten anzeigen

Ein Abteilungsadministrator kann seinen Abteilungen neue Konten hinzufügen. Er kann auch Konten aus den Abteilungen entfernen, nicht jedoch aus der Registrierung.

So fügen Sie eine Abteilung hinzu:

1. Melden Sie sich beim Azure Enterprise Portal an.
1. Wählen Sie im linken Bereich **Verwalten** aus.
1. Wählen Sie die Registerkarte **Abteilung** aus, und klicken Sie dann auf **+ Abteilung hinzufügen**.
1. Geben Sie die Informationen ein.
   Der Abteilungsname ist das einzige Pflichtfeld. Er muss mindestens drei Zeichen lang sein.
1. Wählen Sie zum Abschluss **Hinzufügen** aus.

## <a name="add-a-department-administrator"></a>Hinzufügen eines Abteilungsadministrators

Nachdem eine Abteilung erstellt wurde, kann der Unternehmensadministrator Abteilungsadministratoren hinzufügen und diese jeweils einer Abteilung zuordnen. Abteilungsadministratoren können für ihre Abteilungen die folgenden Aktionen ausführen:

- Erstellen anderer Abteilungsadministratoren
- Anzeigen und Bearbeiten von Abteilungseigenschaften wie dem Namen oder der Kostenstelle
- Hinzufügen von Konten
- Entfernen von Konten
- Herunterladen von Nutzungsdetails
- Anzeigen der monatlichen Nutzung und Gebühren <sup>1</sup>

> <sup>1</sup> Diese Berechtigungen müssen von einem Unternehmensadministrator erteilt werden. Wenn Sie die Berechtigung zum Anzeigen der monatlichen Nutzung und Gebühren für die Abteilung erhalten haben, diese Informationen aber nicht angezeigt werden, wenden Sie sich an Ihren Partner.

### <a name="to-add-a-department-administrator"></a>So fügen Sie einen Abteilungsadministrator hinzu

Als Unternehmensadministrator:

1. Melden Sie sich beim Azure Enterprise Portal an.
1. Wählen Sie im linken Bereich **Verwalten** aus.
1. Wählen Sie die Registerkarte **Abteilung** und anschließend die Abteilung aus.
1. Klicken Sie auf **+ Administrator hinzufügen**, und fügen Sie die erforderlichen Informationen hinzu.
1. Legen Sie für Lesezugriff die Option **Schreibgeschützt** auf **Ja** fest, und klicken Sie anschließend auf **Hinzufügen**.

![Beispiel für das Dialogfeld „Abteilungsadministrator hinzufügen“](./media/ea-portal-get-started/ea-create-add-department-admin.png)

### <a name="to-set-read-only-access"></a>So legen Sie Lesezugriff fest

Sie können Abteilungsadministratoren Lesezugriff gewähren.

- Wenn Sie einen neuen Abteilungsadministrator erstellen, legen Sie die Option „Schreibgeschützt“ auf **Ja** fest.

- So bearbeiten Sie einen vorhandenen Abteilungsadministrator:
   1. Wählen Sie eine Abteilung aus, und klicken Sie dann auf das Stiftsymbol neben dem **Abteilungsadministrator**, den Sie bearbeiten möchten.
   1. Legen Sie die Option „Schreibgeschützt“ auf **Ja** fest, und klicken Sie dann auf **Speichern**.

Unternehmensadministratoren erhalten automatisch Abteilungsadministratorberechtigungen.

## <a name="add-an-account"></a>Konto hinzufügen

Die Struktur von Konten und Abonnements wirkt sich darauf aus, wie diese verwaltet und auf Rechnungen und Berichten angezeigt werden. Zu den Beispielen für typische Organisationsstrukturen zählt die Strukturierung nach Geschäftsbereichen, funktionsbezogenen Teams und geografischen Standorten.

So fügen Sie ein Konto hinzu:

1. Wählen Sie im Azure Enterprise Portal im Navigationsbereich auf der linken Seite die Option **Verwalten** aus.
1. Klicken Sie auf die Registerkarte **Konto**. Klicken Sie auf der Seite **Konto** auf **+ Konto hinzufügen**.
1. Wählen Sie eine Abteilung aus, oder lassen Sie sie nicht zugewiesen, und wählen Sie dann den gewünschten Authentifizierungstyp aus.
1. Geben Sie einen Anzeigenamen ein, damit Sie das Konto in Berichten identifizieren können.
1. Geben Sie die **E-Mail-Adresse des Kontobesitzers** ein, die dem neuen Konto zugeordnet werden soll.
1. Bestätigen Sie die E-Mail-Adresse, und klicken Sie auf **Hinzufügen**.

![Beispiel für die Liste der Konten und die Option „Konto hinzufügen“](./media/ea-portal-get-started/create-ea-add-an-account.png)

Um ein weiteres Konto hinzuzufügen, klicken Sie auf **Weiteres Konto hinzufügen**, oder klicken Sie in der rechten unteren Ecke der linken Symbolleiste auf **Hinzufügen**.

So bestätigen Sie den Kontobesitz:

1. Melden Sie sich beim Azure Enterprise Portal an.
1. Sehen Sie sich den Status an.

   Der Status sollte sich von **Ausstehend** in **Start-/Enddatum** ändern. Das Start-/Enddatum ist das Datum, an dem der Benutzer sich zuerst angemeldet hat, und das Enddatum der Vereinbarung.
1. Wenn die **Warnmeldung** angezeigt wird, muss der Kontobesitzer auf **Weiter** klicken, um das Konto bei der erstmaligen Anmeldung beim Azure Enterprise Portal zu aktivieren.

## <a name="change-account-owner"></a>Ändern des Kontobesitzers

Unternehmensadministratoren können das Azure Enterprise Portal verwenden, um den Abonnementkontobesitz in einer Registrierung zu übertragen. Durch die Aktion werden alle Abonnements von einem Quellbenutzerkonto in ein Zielbenutzerkonto verschoben.

Beachten Sie beim Übertragen von Konten die folgenden wichtigen Informationen:

- Diese Übertragungen können folgendermaßen durchgeführt werden:
  - Von einem Geschäfts-, Schul- oder Unikonto zu einem anderen Geschäfts-, Schul- oder Unikonto.
  - Von einem Microsoft-Konto zu einem Geschäfts-, Schul- oder Unikonto.
  - Von einem Microsoft-Konto zu einem anderen Microsoft-Konto.
  
    Das Zielkonto muss ein gültiges Azure Commerce-Konto sein, damit es ein gültiges Ziel für Übertragungen darstellt. Bei neuen Konten werden Sie bei der Anmeldung beim Azure Enterprise Portal aufgefordert, ein Azure Commerce-Konto zu erstellen. Bei vorhandenen Konten müssen Sie zuerst ein neues Azure-Abonnement erstellen, bevor das Konto berechtigt ist.

- Übertragungen von einem Geschäfts-, Schul- oder Unikonto zu einem Microsoft-Konto werden nicht unterstützt.

- Wenn Sie eine Abonnementübertragung abschließen, aktualisiert Microsoft den Kontobesitzer.

Beachten Sie diese Richtlinien für die rollenbasierte Zugriffssteuerung (RBAC):

- Wenn Sie Abonnementübertragungen zwischen zwei Organisations-IDs im gleichen Mandanten durchführen, werden RBAC-Richtlinien und vorhandene Dienstadministrator- und Co-Administrator-Rollen beibehalten.
- Andere Abonnementübertragungen führen zum Verlust Ihrer RBAC-Richtlinien und Rollenzuweisungen.
- Richtlinien und Administratorrollen werden nicht über verschiedene Verzeichnisse hinweg übertragen. Dienstadministratoren werden auf den Besitzer des Zielkontos aktualisiert.

Führen Sie vor dem Ändern des Kontobesitzers die folgenden Aktionen durch:

1. Zeigen Sie im Azure Enterprise Portal die Registerkarte **Konto** an, und identifizieren Sie das Quellkonto. Das Quellkonto muss aktiv sein.
1. Identifizieren Sie das Zielkonto, und stellen Sie sicher, dass es aktiv ist.

So übertragen Sie den Kontobesitz für alle Abonnements:

1. Melden Sie sich beim Azure Enterprise Portal an.
1. Wählen Sie im Navigationsbereich links die Option **Verwalten** aus.
1. Klicken Sie auf die Registerkarte **Konto**, und zeigen Sie auf ein Konto.
1. Klicken Sie auf der rechten Seite auf das Symbol zum Ändern des Kontobesitzers. Das Symbol stellt eine Person dar.
1. Wählen Sie ein geeignetes Konto aus, und klicken Sie dann auf **Weiter**.
1. Bestätigen Sie die Übertragung, und klicken Sie auf **Senden**.

![Bild des Symbols zum Ändern des Kontobesitzers](./media/ea-portal-get-started/create-ea-create-sub-transfer-account-ownership-of-sub.png)

So übertragen Sie den Kontobesitz für ein einzelnes Abonnement:

1. Melden Sie sich beim Azure Enterprise Portal an.
1. Wählen Sie im Navigationsbereich links die Option **Verwalten** aus.
1. Klicken Sie auf die Registerkarte **Konto**, und zeigen Sie auf ein Konto.
1. Klicken Sie rechts auf das Symbol zum Übertragen von Abonnements. Das Symbol stellt eine Seite dar.
1. Wählen Sie ein geeignetes Abonnement aus, und klicken Sie dann auf **Weiter**.
1. Bestätigen Sie die Übertragung, und klicken Sie auf **Senden**.

![Bild des Symbols zum Übertragen von Abonnements](./media/ea-portal-get-started/ea-transfer-subscriptions.png)

In diesem Video können Sie sich die Benutzerverwaltung im Azure Enterprise Portal ansehen:

> [!VIDEO https://www.youtube.com/embed/621jVkvmwm8]

## <a name="create-a-subscription"></a>Erstellen eines Abonnements

Kontobesitzer können Abonnements anzeigen und verwalten. Sie können Abonnements verwenden, um Teams in Ihrer Organisation Zugriff auf Entwicklungsumgebungen und Projekte zu ermöglichen. Beispiele dafür sind Test, Produktion, Entwicklung und Staging.

Wenn Sie verschiedene Abonnements für jede Anwendungsumgebung erstellen, trägt dies zur Sicherheit der Umgebungen bei.

- Es ist auch möglich, dass Sie jedem Abonnement ein anderes Dienstadministratorkonto zuweisen. 
- Sie können Abonnements beliebig viele Dienste zuordnen.
- Der Kontobesitzer erstellt Abonnements und weist jedem Abonnement in seinem Konto ein Dienstadministratorkonto zu.

### <a name="add-a-subscription"></a>Hinzufügen eines Abonnements

Verwenden Sie die folgenden Informationen, um ein Abonnement hinzuzufügen.

Wenn Sie Ihrem Konto zum ersten Mal ein Abonnement hinzufügen, werden Sie aufgefordert, den Microsoft Online-Abonnementvertrag (Microsoft Online Subscription Agreement, MOSA) und eine Gebührenübersicht zu akzeptieren. Auch wenn sie bei Enterprise Agreement-Kunden keine Anwendung finden, sind MOSA und Gebührenübersicht zum Erstellen des Abonnements erforderlich. Ihre Microsoft Azure Enterprise Agreement-Registrierungszusatzvereinbarung hat Vorrang vor diesen Punkten, und Ihre Vertragsbeziehung ändert sich dadurch nicht. Wenn Sie dazu aufgefordert werden, aktivieren Sie das Kontrollkästchen und bestätigen damit, dass Sie die Bedingungen akzeptieren.

_Microsoft Azure Enterprise_ ist der Standardname, der beim Erstellen eines Abonnements verwendet wird. Sie können den Namen ändern, um dieses Abonnement von den anderen Abonnements in Ihrer Registrierung zu unterscheiden und sicherzustellen, dass es in Berichten auf Unternehmensebene identifizierbar ist.

So fügen Sie ein Abonnement hinzu:

1. Melden Sie sich im Azure Enterprise Portal beim Konto an.
1. Klicken Sie auf die Registerkarte **Administrator** und anschließend oben auf der Seite auf **Abonnement**.
1. Vergewissern Sie sich, dass Sie als Kontobesitzer des Kontos angemeldet sind.
1. Wählen Sie **+ Abonnement hinzufügen** und dann **Kaufen** aus.

   Wenn Sie ein Abonnement zum ersten Mal zu einem Konto hinzufügen, müssen Sie Ihre Kontaktinformationen angeben. Wenn Sie weitere Abonnements hinzufügen, werden Ihre Kontaktinformationen für Sie hinzugefügt.

1. Wählen Sie **Abonnements** und dann das von Ihnen erstellte Abonnement aus.
1. Wählen Sie **Abonnementdetails bearbeiten** aus.
1. Bearbeiten Sie den **Abonnementnamen** und den **Dienstadministrator**, und aktivieren Sie das Kontrollkästchen.
  
   Der Abonnementname wird in Berichten angezeigt. Dies ist der Name des Projekts, das dem Abonnement im Entwicklungsportal zugeordnet ist.

Es kann bis zu 24 Stunden dauern, bis neue Abonnements in der Liste der Abonnements angezeigt werden. Nachdem Sie ein Abonnement erstellt haben, können Sie folgende Aktionen ausführen:

- [Bearbeiten von Abonnementdetails](https://account.azure.com/Subscriptions)
- [Verwalten von Abonnementdiensten](https://portal.azure.com/#home)

## <a name="transfer-an-enterprise-subscription-to-a-pay-as-you-go-subscription"></a>Übertragen eines Enterprise-Abonnements auf ein Abonnement mit nutzungsbasierter Bezahlung

Wenn Sie ein Enterprise-Abonnement auf ein einzelnes Abonnement mit nutzungsbasierter Bezahlung übertragen möchten, müssen Sie im Azure Enterprise Portal eine neue Supportanfrage erstellen. Klicken Sie zum Erstellen einer Supportanfrage im Bereich **Hilfe und Support** auf **+ Neue Supportanfrage**.

## <a name="associate-an-existing-account-with-your-pay-as-you-go-subscription"></a>Zuordnen eines vorhandenen Kontos zu Ihrem Abonnement mit nutzungsbasierter Bezahlung

Falls Sie im Azure-Portal bereits über ein Microsoft Azure-Konto verfügen, geben Sie das zugeordnete Geschäfts-, Schul-, Uni- oder Microsoft-Konto ein, um es Ihrer Enterprise Agreement-Registrierung zuzuordnen.

### <a name="associate-an-existing-account"></a>Zuordnen eines vorhandenen Kontos

1. Wählen Sie im Azure Enterprise Portal **Verwalten** aus.
1. Klicken Sie auf die Registerkarte **Konto**.
1. Klicken Sie auf **+ Konto hinzufügen**.
1. Geben Sie das Geschäfts-, Schul-, Uni- oder Microsoft-Konto ein, das dem vorhandenen Azure-Konto zugeordnet ist.
1. Bestätigen Sie das dem vorhandenen Azure-Konto zugeordnete Konto.
1. Geben Sie einen Namen ein, den Sie zum Identifizieren dieses Kontos in Berichten verwenden möchten.
1. Wählen Sie **Hinzufügen**.
1. Um ein weiteres Konto hinzuzufügen, können Sie erneut die Option **+ Konto hinzufügen** auswählen oder zur Homepage zurückkehren, indem Sie die Schaltfläche **Verwaltung** auswählen.
1. Wenn Sie die Seite **Konto** öffnen, wird das neu hinzugefügte Konto mit dem Status **Ausstehend** angezeigt.

### <a name="confirm-account-ownership"></a>Bestätigen des Kontobesitzes

1. Melden Sie sich bei dem E-Mail-Konto an, das dem angegebenen Geschäfts-, Schul-, Uni- oder Microsoft-Konto zugeordnet ist.
1. Öffnen Sie die E-Mail-Benachrichtigung mit dem Titel _Invitation to Activate your Account on the Microsoft Azure Service from Microsoft Volume Licensing_ (Einladung zur Aktivierung Ihres Kontos im Microsoft Azure-Dienst über die Microsoft-Volumenlizenzierung).
1. Klicken Sie in der Einladung auf den Link **Beim Microsoft Azure Enterprise Portal anmelden**.
1. Wählen Sie **Anmelden**.
1. Geben Sie Ihr Geschäfts-, Schul-, Uni- oder Microsoft-Konto sowie das zugehörige Kennwort ein, um sich anzumelden und den Kontobesitz zu bestätigen.

### <a name="azure-marketplace"></a>Azure Marketplace

Die meisten Abonnements können von einer Umgebung mit nutzungsbasierter Bezahlung in Azure Enterprise Agreement konvertiert werden. Bei Azure Marketplace-Diensten ist dies jedoch nicht der Fall. Um eine einzige Ansicht aller Abonnements und Gebühren zu erhalten, empfiehlt es sich, die Azure Marketplace-Dienste dem Azure Enterprise Portal hinzuzufügen.

1. Melden Sie sich beim Azure Enterprise Portal an.
1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
1. Klicken Sie auf die Registerkarte **Registrierung**.
1. Sehen Sie sich den Abschnitt **Registrierungsdetail** an.
1. Klicken Sie zur Aktivierung rechts neben dem Feld „Azure Marketplace“ auf das Stiftsymbol. Wählen Sie **Speichern** aus.

Der Kontobesitzer kann nun Azure Marketplace-Dienste erwerben, die zuvor vom Abonnement mit nutzungsbasierter Bezahlung verwaltet wurden.

Nachdem die neuen Azure Marketplace-Abonnements unter Ihrer Azure EA-Registrierung aktiviert wurden, kündigen Sie die Azure Marketplace-Abonnements, die in der Umgebung mit nutzungsbasierter Bezahlung erstellt wurden. Dieser Schritt ist wichtig, um einen Fehlerzustand Ihrer Azure Marketplace-Abonnements zu vermeiden, wenn Ihr Zahlungsmittel für die nutzungsbasierte Bezahlung abläuft.

### <a name="msdn"></a>MSDN

MSDN-Abonnements werden automatisch in MSDN Dev/Test konvertiert, und das Azure EA-Angebot verliert sämtliches ggf. vorhandenes Guthaben.

### <a name="azure-in-open"></a>Azure in Open

Wenn Sie ein Azure in Open-Abonnement einem Enterprise Agreement zuordnen, verfällt das nicht in Anspruch genommene Azure in Open-Guthaben. Daher wird empfohlen, das gesamte Guthaben für ein Azure in Open-Abonnement aufzubrauchen, bevor Sie das Konto Ihrem Enterprise Agreement hinzufügen.  

### <a name="accounts-with-support-subscriptions"></a>Konten mit Supportabonnements

Wenn Ihr Enterprise Agreement kein Supportabonnement umfasst und Sie dem Azure Enterprise Portal ein vorhandenes Konto mit einem Supportabonnement hinzufügen, wird Ihr MOSA-Supportabonnement nicht automatisch übertragen. Sie müssen während der Toleranzperiode (bis zum Ende des Folgemonats) ein Supportabonnement in Azure EA neu erwerben.

## <a name="view-usage-summary-and-download-reports"></a>Anzeigen der Verwendungszusammenfassung und Herunterladen von Berichten

Unternehmensadministratoren können eine Zusammenfassung ihrer Nutzungsdaten, den genutzten Mindestverbrauch und die Gebühren für die zusätzliche Nutzung im Azure Enterprise Portal anzeigen. Die Gebühren werden auf der Übersichtsebene für alle Konten und Abonnements angezeigt.

Laden Sie den Bericht mit den Nutzungsdetails herunter, um die ausführliche Nutzung für bestimmte Konten anzuzeigen:

1. Melden Sie sich beim Azure Enterprise Portal an.
1. Wählen Sie **Berichte** aus.
1. Wählen Sie die Registerkarte **Nutzung herunterladen** aus.
1. Klicken Sie in der Liste der Berichte für den monatlichen Bericht, den Sie abrufen möchten, auf **Herunterladen**.

   > [!NOTE]
   > Der Bericht mit den Nutzungsdetails enthält keine anwendbaren Steuern.
   >
   > Zwischen der Nutzung und der entsprechenden Angabe im Bericht gibt es unter Umständen eine Verzögerung von bis zu acht Stunden.

So zeigen Sie die zusammengefassten Nutzungsberichte und -diagramme an:

1. Melden Sie sich beim Azure Enterprise Portal an.

1. Wählen Sie einen Zeitraum aus.

   Um den Datumsbereich für die **Nutzungszusammenfassung** zu ändern, können Sie in der rechten oberen Ecke der Seite von **M** (monatlich) zu **C** (benutzerdefiniert) wechseln und dann das benutzerdefinierte Start- und Enddatum eingeben.

   ![Erstellen und Anzeigen der Nutzungszusammenfassung und Herunterladen von Berichten in der benutzerdefinierten Ansicht](./media/ea-portal-get-started/create-ea-view-usage-summary-and-download-reports-custom-view.png)
1. Wählen Sie einen Zeitraum oder einen Monat im Diagramm aus, um weitere Details anzuzeigen.

   - Das Diagramm zeigt die Nutzung im Vergleich zum Vormonat mit einer Aufschlüsselung der tatsächlichen Nutzung, der übermäßig berechneten Dienste, der getrennt berechneten Gebühren und der Azure Marketplace-Gebühren an.
   - Für den ausgewählten Monat können Sie die Felder unterhalb des Diagramms verwenden, um nach Abteilungen, Konten und Abonnements zu filtern.
   - Sie können zwischen **Gebühr nach Diensten** und **Gebühr nach Hierarchie** wechseln.
   - Zeigen Sie Details zu den Kategorien **Azure-Dienst**, **Getrennt abgerechnete Gebühren** und **Azure Marketplace** an, indem Sie die entsprechenden Abschnitte erweitern.

In diesem Video können Sie sich die Nutzungsanzeige ansehen:

> [!VIDEO https://www.youtube.com/embed/Cv2IZ9QCn9E]

### <a name="download-csv-reports"></a>Herunterladen von CSV-Berichten

Unternehmensadministratoren verwenden die Seite „Download des monatlichen Berichts“, um die folgenden Berichte als CSV-Dateien herunterzuladen:

- Saldo und Gebühren
- Nutzungsdetails
- Azure Marketplace-Gebühren
- Preisblatt

So laden Sie Berichte herunter:

1. Wählen Sie im Azure Enterprise Portal **Berichte** aus.
2. Klicken Sie oben auf der Seite auf **Nutzung herunterladen**.
3. Wählen Sie neben dem Bericht des Monats **Herunterladen** aus.

   > [!NOTE]
   > Zwischen dem Nutzungsdatum und dem Zeitpunkt, zu dem die Nutzung in den Berichten angezeigt wird, gibt es unter Umständen eine Wartezeit von bis zu fünf Tagen.
   >
   > Wenn Sie die CSV-Dateien mit Safari in Excel herunterladen, treten möglicherweise Formatierungsfehler auf. Öffnen Sie die Datei in einem Text-Editor, um solche Fehler zu vermeiden.

![Beispiel für die Seite „Nutzung herunterladen“](./media/ea-portal-get-started/create-ea-download-csv-reports.png)

Das folgende Video zeigt, wie Sie Nutzungsinformationen herunterladen:

> [!VIDEO https://www.youtube.com/embed/eY797htT1qg]

### <a name="advanced-report-download"></a>Herunterladen des erweiterten Berichts

Über den Download erweiterter Berichte können Sie Berichte abrufen, die bestimmte Datumsbereiche oder Konten abdecken. Die Ausgabedatei liegt im CSV-Format vor, um große Datensatzgruppen zu unterstützen.

1. Wählen Sie im Azure Enterprise Portal **Download erweiterter Berichte** aus.
1. Wählen Sie einen geeigneten Datumsbereich und die entsprechenden Konten aus.
1. Wählen Sie **Nutzungsdaten anfordern** aus.
1. Wählen Sie die Schaltfläche **Aktualisieren** aus, bis der Status des Berichts auf **Herunterladen** aktualisiert wird.
1. Laden Sie den Bericht herunter.

### <a name="download-usage-reports-and-billing-information-for-a-prior-enrollment"></a>Herunterladen von Nutzungsberichten und Abrechnungsinformationen für eine vorherige Registrierung

Sie können Nutzungsberichte und Abrechnungsinformationen für eine vorherige Registrierung herunterladen, nachdem eine Registrierungsübertragung durchgeführt wurde. Historische Berichte sind sowohl im Azure Enterprise Portal als auch in der Kostenverwaltung verfügbar.

Das Azure Enterprise Portal filtert inaktive Registrierungen aus der Ansicht heraus. Sie müssen das Kontrollkästchen **Aktiv** deaktivieren, um inaktive übertragene Registrierungen anzuzeigen.  

![Durch Deaktivieren des Kontrollkästchens kann der Benutzer inaktive Registrierungen anzeigen.](./media/ea-portal-get-started/unchecked-active-box.png)

## <a name="azure-ea-term-glossary"></a>Glossar für Azure EA-Begriffe

- **Konto:** Eine Organisationseinheit im Azure Enterprise Portal. Sie wird zur Abonnementverwaltung und Berichterstellung verwendet.
- **Kontobesitzer:** Die Person, die Abonnements und Dienstadministratoren in Azure verwaltet. Kontobesitzer können Nutzungsdaten für dieses Konto und die zugehörigen Abonnements anzeigen.
- **Zusatzabonnement:** Ein Jahres- oder Anschlussabonnement unter der Registrierungszusatzvereinbarung.
- **Prepaidtarif:** Prepaidtarif in Form eines jährlichen Geldbetrags für Azure-Dienste zu einem rabattierten Satz für die Nutzung im Rahmen dieser Vorauszahlung.
- **Abteilungsadministrator:** Die Person, die Abteilungen verwalten, neue Konten und Kontobesitzer erstellen, Nutzungsdetails für die von ihr verwalteten Abteilungen einsehen sowie bei entsprechender Berechtigung Kosten anzeigen kann.
- **Registrierungsnummer:** Ein eindeutiger, von Microsoft bereitgestellter Bezeichner zur Identifizierung der spezifischen Registrierung, die einem Enterprise Agreement zugeordnet ist.
- **Unternehmensadministrator:** Die Person, die Abteilungen, Abteilungsbesitzer, Konten und Kontobesitzer in Azure verwaltet. Diese Personen können sowohl Unternehmensadministratoren verwalten als auch Nutzungsdaten, in Rechnung gestellte Mengen sowie nicht in Rechnung gestellte Gebühren für alle Konten und Abonnements anzeigen, die der Unternehmensregistrierung zugeordnet sind.
- **Enterprise Agreement:** Ein Microsoft-Lizenzvertrag für Kunden mit zentralisiertem Einkauf, die ihre gesamte Organisation auf der Grundlage von Microsoft-Technologie standardisieren und eine auf Microsoft-Software basierende IT-Infrastruktur nutzen möchten.
- **Enterprise Agreement-Registrierung:** Eine Registrierung im Enterprise Agreement-Programm für die Volumenbereitstellung von Microsoft-Produkten zu vergünstigten Preisen.
- **Microsoft-Konto**: Ein webbasierter Dienst, der beteiligten Websites die Authentifizierung eines Benutzers mit einem einzelnen Satz von Anmeldeinformationen ermöglicht.
- **Microsoft Azure Enterprise-Registrierungszusatzvereinbarung (Registrierungszusatzvereinbarung)** : Eine von einem Unternehmen unterzeichnete Zusatzvereinbarung, durch die das Unternehmen im Rahmen seiner Registrierung Zugang zu Azure erhält.
- **Azure Enterprise Portal**: Das Portal, das von unseren Unternehmenskunden für die Verwaltung ihrer Azure-Konten und der zugehörigen Abonnements verwendet wird.
- **Verbrauchte Ressourcenmenge:** Die Menge eines einzelnen Azure-Diensts, die innerhalb eines Monats genutzt wurde.
- **Dienstadministrator:** Die Person, die auf Abonnements und Entwicklungsprojekte im Azure Enterprise Portal zugreifen und diese verwalten kann.
- **Abonnement**: Stellt ein Azure Enterprise Portal-Abonnement dar und ist ein Container mit Azure-Diensten, die von demselben Dienstadministrator verwaltet werden.
- **Geschäfts-, Schul- oder Unikonto:** Konto für Organisationen, die Active Directory mit Cloudverbund eingerichtet haben und bei denen sich alle Konten in einem einzigen Mandanten befinden.

### <a name="enrollment-statuses"></a>Registrierungsstatus

- **Pending**: Der Registrierungsadministrator muss sich beim Azure Enterprise Portal anmelden. Nach der Anmeldung wechselt die Registrierung den Status „Aktiv“.
- **Aktiv:** Die Registrierung ist aktiv, und Konten und Abonnements können im Azure Enterprise Portal erstellt werden. Die Registrierung bleibt bis zum Enddatum des Enterprise Agreements aktiv.
- **Indefinite Extended Term** (Unbestimmte Laufzeitverlängerung): Eine unbestimmte Laufzeitverlängerung tritt ein, wenn das Enddatum des Enterprise Agreements überschritten wurde. Mit diesem Status können Azure EA-Kunden, die sich für die Laufzeitverlängerung entschieden haben, Azure-Dienste am Ende ihres Enterprise Agreements unbefristet weiterverwenden.

   Bevor die Azure EA-Registrierung das Enddatum des Enterprise Agreements erreicht, muss der Registrierungsadministrator sich für eine der folgenden Optionen entscheiden:

  - Verlängern der Registrierung durch Hinzufügen eines zusätzlichen Mindestverbrauchs
  - Übertragen auf eine neue Registrierung
  - Migrieren zum Microsoft Online Subscription-Programm (MOSP)
  - Bestätigen der Deaktivierung aller Dienste, die der Registrierung zugeordnet sind
- **Abgelaufen:** Der Azure EA-Kunde wurde aus der Laufzeitverlängerung ausgeschlossen, und die Azure EA-Registrierung hat das Enddatum des Enterprise Agreements erreicht. Die Registrierung läuft ab, und alle zugeordneten Dienste werden deaktiviert.
- **Übertragen:** Status für Registrierungen, bei denen alle zugeordneten Konten und Dienste in eine neue Registrierung übertragen wurden.
  >[!NOTE]
  > Registrierungen werden nicht automatisch übertragen, wenn während der Verlängerung eine neue Registrierungsnummer generiert wird. Sie müssen die vorherige Registrierungsnummer in Ihren Verlängerungsdokumenten angeben, um eine automatische Übertragung zu ermöglichen.

## <a name="get-started-on-azure-ea---faq"></a>Häufig gestellte Fragen zu den ersten Schritten mit Azure EA

Dieser Abschnitt enthält ausführliche Informationen zu typischen Fragen, die Kunden während des Onboardingprozesses stellen.  

### <a name="can-i-associate-my-existing-azure-account-to-azure-ea-enrollment"></a>Kann ich der Azure EA-Registrierung mein vorhandenes Azure-Konto zuordnen?

Ja. Alle Azure-Abonnements, für die Sie als Kontobesitzer fungieren, werden in Ihr Enterprise Agreement konvertiert. Dazu gehören Abonnements, die ein monatliches Guthaben verwenden, z. B. Visual Studio, AzurePass, MPN, BizSpark usw. Beim Konvertieren solcher Abonnements verlieren Sie das monatliche Guthaben.

### <a name="i-accidentally-associated-my-existing-azure-account-with-azure-ea-enrollment-as-a-result-i-lost-my-monthly-credit-can-i-get-my-monthly-credit-back"></a>Ich habe der Azure EA-Registrierung versehentlich mein vorhandenes Azure-Konto zugeordnet. Dadurch ist mein monatliches Guthaben verloren gegangen. Kann ich mein monatliches Guthaben wieder zurückerlangen?

Wenn Sie sich als Azure EA-Kontobesitzer mit den Anmeldeinformationen Ihres Visual Studio-Abonnements angemeldet haben, können Sie den individuellen Azure-Vorteil Ihres Visual Studio-Abonnements wiederherstellen, indem Sie eine der folgenden Aktionen ausführen:

- Löschen Sie den Kontobesitzer aus dem Azure Enterprise Portal, nachdem Sie die alle zugehörigen Azure-Abonnements entfernt oder verschoben haben. Registrieren Sie sich anschließend erneut für individuelle Visual Studio-Azure-Vorteile.
- Löschen Sie den Visual Studio-Abonnenten aus der Verwaltungswebsite in VLSC, und weisen Sie das Abonnement diesmal einem Konto mit anderen Anmeldeinformationen zu. Registrieren Sie sich anschließend erneut für individuelle Visual Studio-Azure-Vorteile.

### <a name="what-type-of-subscription-should-i-create"></a>Welche Art von Abonnement soll ich erstellen?

Im Azure Enterprise Portal stehen für Unternehmenskunden zwei Arten von Abonnements zur Verfügung:

- Microsoft Azure Enterprise. Ideal für:
  - Jegliche Verwendung in der Produktion
  - Beste Preise basierend auf Infrastrukturausgaben
  
  [Wenden Sie sich an den Azure-Vertrieb](https://azure.microsoft.com/pricing/enterprise-agreement/), um weitere Informationen zu erhalten.

- Enterprise Dev/Test. Ideal für:
  - Sämtliche Team-Dev/Test-Workloads
  - Einzelne Dev/Test-Workloads mit mittlerem bis hohem Umfang
  - Zugriff auf spezielle MSDN-Images und Vorzugstarife für Dienste
  
  Weitere Informationen finden Sie im [Enterprise Dev/Test-Angebot](https://azure.microsoft.com/offers/ms-azr-0148p/).

### <a name="is-it-possible-to-transfer-subscription-ownership-to-another-account"></a>Kann der Abonnementbesitz auf ein anderes Konto übertragen werden?

Ja, Sie können den Abonnementbesitz auf ein anderes Konto übertragen. Wenn das Konto A beispielsweise über drei Abonnements verfügt, kann der Unternehmensadministrator ein Abonnement auf das Konto B, ein Abonnement auf das Konto C und ein Abonnement auf das Konto D oder aber alle Abonnements auf das Konto E übertragen.

So übertragen Sie Abonnements

1. Wählen Sie im Azure Enterprise Portal **Verwalten** > **Konto** aus.
1. Zeigen Sie ganz rechts auf **Konto**, um die Optionen **Besitz übertragen** (Personensymbol) und **Abonnement übertragen** (Listensymbol) anzuzeigen. Diese Optionen sind nur für aktive Konten sichtbar.

### <a name="my-subscription-name-is-the-same-as-the-offer-name-should-i-change-the-subscription-name-to-something-meaningful-to-my-organization"></a>Der Name meines Abonnements ist mit dem Angebotsnamen identisch. Soll ich den Abonnementnamen auf einen aussagekräftigen Namen für meine Organisation festlegen?

Wenn Sie ein Abonnement erstellen, wird standardmäßig der Name des gewählten Angebotstyps verwendet. Es empfiehlt sich, den Abonnementnamen in einen Namen zu ändern, der die Nachverfolgung des Abonnements erleichtert.

So ändern Sie den Namen

1. Melden Sie sich bei [https://account.windowsazure.com](https://account.windowsazure.com) an.
1. Wählen Sie die Abonnementliste aus.
1. Wählen Sie das Abonnement aus, das Sie bearbeiten möchten.
1. Klicken Sie auf das Symbol **Abonnement verwalten**.
1. Bearbeiten Sie die Abonnementdetails.

### <a name="how-can-i-track-costs-incurred-by-a-cost-center"></a>Wie kann ich Kosten nach Kostenstelle nachverfolgen?

Wenn Sie Kosten nach Kostenstelle nachverfolgen möchten, müssen Sie die Kostenstelle auf einer der folgenden Ebenen definieren:

- Department
- Konto
- Subscription

Abhängig von Ihren Anforderungen können Sie die gleiche Kostenstelle verwenden, um die Nutzung und Kosten nachzuverfolgen, die einer bestimmten Kostenstelle zugeordnet sind.

Wenn Sie also beispielsweise die Kosten für ein spezielles Projekt nachverfolgen möchten, an dem mehrere Abteilungen beteiligt sind, können Sie die Kostenstelle auf der Abonnementebene definieren, um die Nutzung und die Kosten nachzuverfolgen.

Eine Kostenstelle kann nicht auf der Dienstebene definiert werden. Falls Sie die Nutzung auf Dienstebene nachverfolgen möchten, können Sie das auf der Dienstebene zur Verfügung stehende _Tag_-Feature verwenden.

### <a name="how-do-i-track-usage-and-spend-by-different-departments-in-my-organization"></a>Wie kann ich die Nutzung und die Ausgaben verschiedener Abteilungen in meiner Organisation nachverfolgen?

Sie können unter Ihrer Azure EA-Registrierung beliebig viele Abteilungen erstellen. Zur ordnungsgemäßen Nachverfolgung der Nutzung müssen Sie sicherstellen, dass Abonnements nicht abteilungsübergreifend verwendet werden.

Nachdem Sie Abteilungen und Abonnements erstellt haben, werden im Nutzungsbericht Daten angezeigt. Diese Informationen können Ihnen dabei helfen, die Nutzung zu verfolgen und Kosten und Ausgaben auf Abteilungsebene zu verwalten.

Sie können auch über die Berichterstellungs-API auf Nutzungsdaten zugreifen. Ausführliche Informationen und Beispielcode finden Sie in der [Dokumentation zur Berichterstellungs-API](https://ea.azure.com/helpdocs/reportingAPI).

### <a name="can-i-set-a-spending-quota-and-get-alerts-as-i-approach-my-limit"></a>Kann ich ein Ausgabenkontingent festlegen und Warnungen erhalten, wenn ich mich dem Grenzwert nähere?

Ein Ausgabenkontingent kann auf der Abteilungsebene festgelegt werden. Daraufhin werden Sie automatisch vom System benachrichtigt, wenn Ihre Ausgabenlimits 50 Prozent, 75 Prozent, 90 Prozent und 100 Prozent des von Ihnen definierten Kontingents erreichen.

Wählen Sie zum Definieren des Ausgabenkontingents eine Abteilung aus, und klicken Sie dann auf das Bearbeitungssymbol. Nachdem Sie die Details des Ausgabenlimits bearbeitet haben, klicken Sie auf **Speichern**.

### <a name="i-used-resource-groups-to-implement-rbac-and-track-usage-how-can-i-view-the-associated-usage-details"></a>Ich habe Ressourcengruppen verwendet, um die RBAC zu implementieren und die Nutzung zu verfolgen. Wie kann ich die zugehörigen Nutzungsdetails anzeigen?

Wenn Sie _Ressourcengruppen_ und _Tags_ verwenden, werden diese Informationen auf Dienstebene nachverfolgt, und Sie können in der CSV-Downloaddatei mit ausführlichen Nutzungsdaten darauf zugreifen. Weitere Informationen finden Sie im Azure Enterprise Portal unter [Nutzungsbericht herunterladen](https://ea.azure.com/report/downloadusage).

Sie können auch über API auf die Nutzung zugreifen. Ausführliche Informationen und Beispielcode finden Sie in der [Berichterstellungs-API](https://ea.azure.com/helpdocs/reportingAPI) in der Dokumentation zum Azure Enterprise Portal.

> [!NOTE]
> Sie können Tags nur auf Ressourcen anwenden, die Azure Resource Manager-Vorgänge unterstützen. Wenn Sie virtuelle Computer, virtuelle Netzwerke oder Speicher über das klassische Bereitstellungsmodell erstellt haben (etwa über das klassische Azure-Portal), kann auf diese Ressourcen kein Tag angewendet werden. Diese Ressourcen müssen per Resource Manager neu bereitgestellt werden, um die Verwendung von Tags zu unterstützen. Alle anderen Ressourcen unterstützen die Markierung durch Tags.

### <a name="can-i-perform-analyses-using-power-bi"></a>Kann ich Analysen mit Power BI durchführen?

Ja. Mit dem Microsoft Azure Enterprise-Inhaltspaket für Power BI können Sie folgende Aktionen ausführen:

- Schnelles Importieren und Analysieren des Azure-Verbrauchs für Ihre Unternehmensregistrierung
- Herausfinden, welche Abteilung, welches Konto oder welches Abonnement den größten Verbrauch generiert hat
- Erfahren, welcher Dienst in Ihrer Organisation am meisten verwendet wurde
- Verfolgen von Ausgaben und Nutzungstrends

So verwenden Sie Power BI

1. Rufen Sie die Power BI-Website auf.
1. Melden Sie sich mit einem gültigen Geschäfts-, Schul- oder Unikonto an.

   Das Geschäfts-, Schul- oder Unikonto kann das gleiche Konto sein, das auch für den Zugriff auf die Registrierung über das Azure Enterprise Portal verwendet wird. Dies ist jedoch nicht zwingend erforderlich.
1. Wählen Sie im Dashboard der Dienste die Kachel „Microsoft Azure Enterprise“ aus, und klicken Sie auf **Verbinden**.
1. Geben Sie im Bildschirm **Verbindung mit Azure Enterprise herstellen** Folgendes ein:
    - URL der Azure-Umgebung: [https://ea.azure.com](https://ea.azure.com)
    - Anzahl von Monaten: zwischen 1 und 36
    - Registrierungsnummer: Ihre Registrierungsnummer
1. Wählen Sie **Weiter** aus.
1. Geben Sie den API-Schlüssel im Feld **Kontoschlüssel** ein.

   Den API-Schlüssel finden Sie im Azure Enterprise Portal. Sehen Sie auf der Registerkarte **Nutzung herunterladen** nach, und wählen Sie dann **API-Zugriffsschlüssel** aus. Kopieren Sie den Schlüssel, und fügen Sie ihn in Power BI im Feld **Kontoschlüssel** ein.

Abhängig von der Größe des Datasets kann es zwischen fünf und 30 Minuten dauern, bis die Daten in Power BI geladen sind.

Die Power BI-Berichterstellung steht für direkte Azure EA-Kunden sowie für Partner und indirekte Kunden zur Verfügung, die Zugriff auf Abrechnungsinformationen haben.

## <a name="next-steps"></a>Nächste Schritte

- Azure Enterprise Portal-Administratoren finden unter [Azure Enterprise Portal-Verwaltung](ea-portal-administration.md) Informationen zu allgemeinen Verwaltungsaufgaben.
- Hilfe zu Problemen im Azure Enterprise Portal finden Sie unter [Beheben von Zugriffsproblemen beim Azure Enterprise Portal](ea-portal-troubleshoot.md).
- Einen Onboardingleitfaden zu Azure EA finden Sie unter [Onboardingleitfaden zu Azure EA (PDF)](https://ea.azure.com/api/v3Help/v2AzureEAOnboardingGuide).
