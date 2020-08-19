---
title: Azure EA-Portalverwaltung
description: In diesem Artikel werden die häufigsten Aufgaben erläutert, die Administratoren im Azure EA-Portal durchführen.
author: bandersmsft
ms.author: banders
ms.date: 08/14/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: boalcsva
ms.openlocfilehash: 9714b3efe7052b837a83769fe24794a373adbc84
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2020
ms.locfileid: "88245348"
---
# <a name="azure-ea-portal-administration"></a>Azure EA-Portalverwaltung

In diesem Artikel werden die häufigsten Aufgaben erläutert, die Administratoren im Azure EA-Portal durchführen (https://ea.azure.com). Das Azure EA-Portal ist ein Onlineverwaltungsportal, das Kunden beim Verwalten der Kosten ihrer Azure EA-Dienste unterstützt. Einführende Informationen zum Azure EA-Portal finden Sie im Artikel [Erste Schritte mit dem Azure EA-Portal](ea-portal-get-started.md).

## <a name="associate-an-account-to-a-department"></a>Zuordnen eines Kontos zu einer Abteilung

Unternehmensadministratoren können Abteilungen unter der Registrierung vorhandene Konten zuordnen.

### <a name="to-associate-an-account-to-a-department"></a>So ordnen Sie ein Konto einer Abteilung zu

1. Melden Sie sich beim Azure EA-Portal als Unternehmensadministrator an.
1. Wählen Sie im linken Navigationsbereich **Verwalten** aus.
1. Wählen Sie **Abteilung** aus.
1. Zeigen Sie auf die Zeile mit dem Konto, und wählen Sie rechts das Stiftsymbol aus.
1. Wählen Sie im Dropdownmenü die Abteilung aus.
1. Wählen Sie **Speichern** aus.

## <a name="department-spending-quotas"></a>Ausgabenkontingente für Abteilungen

EA-Kunden können für jede Abteilung unter einer Registrierung Ausgabenkontingente festlegen oder ändern. Die Höhe des Ausgabenkontingents wird für die aktuelle Laufzeit der Vorauszahlung festgelegt. Am Ende der aktuellen Laufzeit der Vorauszahlung wird das vorhandene Ausgabenkontingent automatisch auf die nächste Laufzeit der Vorauszahlung verlängert, sofern die Werte nicht aktualisiert werden.

Der Abteilungsadministrator kann das Ausgabenkontingent zwar anzeigen, aber nur der Unternehmensadministrator kann den Kontingentbetrag aktualisieren. Unternehmensadministrator und Abteilungsadministrator erhalten Benachrichtigungen, sobald 50 Prozent, 75 Prozent, 90 Prozent und 100 Prozent des Kontingents erreicht werden.

### <a name="enterprise-administrator-to-set-the-quota"></a>Als Unternehmensadministrator können Sie das Kontingent wie folgt festlegen:

 1. Öffnen Sie das Azure EA-Portal.
 1. Wählen Sie im linken Navigationsbereich **Verwalten** aus.
 1. Wählen Sie die Registerkarte **Abteilung** aus.
 1. Wählen Sie die Abteilung aus.
 1. Wählen Sie im Abschnitt mit den Abteilungsdetails das Stiftsymbol oder das Symbol **+ Abteilung hinzufügen** aus, um zusammen mit einer neuen Abteilung ein Ausgabenkontingent hinzuzufügen.
 1. Geben Sie in den Abteilungsdetails ein Ausgabenkontingent in der Währung der Registrierung in das entsprechende Feld ein. (Der angegebene Betrag muss größer Null sein.)
    - Bei dieser Gelegenheit können Sie auch den Abteilungsnamen und die Kostenstelle bearbeiten.
 1. Wählen Sie **Speichern** aus.

Das Ausgabenkontingent für die Abteilung wird nun auf der Registerkarte „Abteilung“ in der Abteilungsliste angezeigt. Am Ende der aktuellen Vorauszahlung behält das Azure EA-Portal die Ausgabenkontingente für die nächste Laufzeit der Vorauszahlung bei.

Die Höhe des Abteilungskontingents ist unabhängig von der aktuellen Azure-Vorauszahlung, und die Höhe des Kontingents sowie die Warnungen gelten nur für die Erstanbieternutzung. Das Ausgabenkontingent für die Abteilung dient nur zur Information und erzwingt keine Ausgabenlimits.

### <a name="department-administrator-to-view-the-quota"></a>Als Abteilungsadministrator können Sie das Kontingent wie folgt anzeigen:

1. Öffnen Sie das Azure EA-Portal.
1. Wählen Sie im linken Navigationsbereich **Verwalten** aus.
1. Wählen Sie die Registerkarte **Abteilung** aus, und sehen Sie sich die Abteilungsliste mit den Ausgabenkontingenten an.

Wenn Sie ein indirekter Kunde sind, müssen die Kostenfeatures von Ihrem Channelpartner aktiviert werden.

## <a name="enterprise-user-roles"></a>Unternehmensbenutzerrollen

Das Azure EA-Portal unterstützt Sie bei der Verwaltung Ihrer Azure EA-Kosten und der Nutzung. Im Azure EA-Portal gibt es drei zentrale Rollen:

- EA-Administrator
- Abteilungsadministrator
- Kontobesitzer

Jede Rolle verfügt über eine andere Zugriffs- und Autorisierungsebene.

Weitere Informationen zu Benutzerrollen finden Sie unter [Unternehmensbenutzerrollen](https://docs.microsoft.com/azure/billing/billing-ea-portal-get-started#enterprise-user-roles).

## <a name="add-an-azure-ea-account"></a>Hinzufügen eines Azure EA-Kontos

Das Azure EA-Konto ist eine Organisationseinheit im Azure EA Portal. Sie wird zur Abonnementverwaltung und auch für die Berichterstellung verwendet. Für den Zugriff auf Azure-Dienste und ihre Verwendung müssen Sie ein Konto erstellen oder erstellen lassen.

Weitere Informationen zu Azure-Konten finden Sie unter [Hinzufügen eines Kontos](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-get-started#add-an-account).

## <a name="enterprise-devtest-offer"></a>Enterprise Dev/Test-Angebot

Als Azure-Unternehmensadministrator können Sie Kontobesitzern in Ihrer Organisation die Erstellung von Abonnements auf Grundlage des EA Dev/Test-Angebots ermöglichen. Wählen Sie hierfür im Azure EA Portal das Feld „Dev/Test“ für den Kontobesitzer aus.

Informieren Sie anschließend den Kontobesitzer, dass er nun die EA Dev/Test-Abonnements einrichten kann, die er für seine Teams mit den Dev/Test-Abonnenten benötigt.

Dank dieses Angebots können aktive Visual Studio-Abonnenten Entwicklungs- und Testworkloads in Azure zu speziellen Dev/Test-Tarifen ausführen. Es besteht Zugriff auf den vollständigen Katalog mit Dev/Test-Images, einschließlich Windows 8.1 und Windows 10.

### <a name="to-set-up-the-enterprise-devtest-offer"></a>So richten Sie das Enterprise Dev/Test-Angebot ein:

1. Melden Sie sich als Unternehmensadministrator an.
1. Wählen Sie im linken Navigationsbereich **Verwalten** aus.
1. Klicken Sie auf die Registerkarte **Konto**.
1. Wählen Sie die Zeile für das Konto aus, unter dem Sie den Dev/Test-Zugriff aktivieren möchten.
1. Wählen Sie das Stiftsymbol rechts neben der Zeile aus.
1. Aktivieren Sie das Kontrollkästchen „Dev/Test“.
1. Wählen Sie **Speichern** aus.

Wenn ein Benutzer über das Azure EA-Portal als Kontobesitzer hinzugefügt wird, werden alle dem Kontobesitzer zugeordneten Azure-Abonnements, die entweder auf dem Dev/Test-Angebot mit nutzungsbasierter Zahlung oder auf den Angeboten mit monatlichem Guthaben für Visual Studio-Abonnenten basieren, in das EA Dev/Test-Angebot konvertiert. Abonnements, die auf anderen Angebotstypen (beispielsweise nutzungsbasierte Zahlung) basieren und dem Kontobesitzer zugeordnet sind, werden in Microsoft Azure Enterprise-Angebote konvertiert.

Das Dev/Test-Angebot gilt derzeit nicht für Azure Government-Kunden.

## <a name="delete-subscription"></a>Löschen eines Abonnements

Löschen Sie ein Abonnement, für das Sie als Kontobesitzer fungieren, wie folgt:

1. Melden Sie sich mit den Anmeldeinformationen Ihres Kontos beim Azure-Portal an.
1. Wählen Sie im Menü „Hub“ die Option **Abonnements** aus.
1. Wählen Sie links oben auf der Seite auf der Registerkarte „Abonnements“ das Abonnement aus, das Sie kündigen möchten, und wählen Sie **Cancel Sub** (Abo kündigen) aus, um die Registerkarte für die Kündigung zu öffnen.
1. Geben Sie den Abonnementnamen ein, wählen Sie einen Kündigungsgrund aus, und wählen Sie die Schaltfläche **Cancel Sub** (Abo kündigen) aus.

Abonnements können nur von Kontoadministratoren gekündigt werden.

Weitere Informationen finden Sie unter [Was geschieht, nachdem ich mein Abonnement gekündigt habe?](cancel-azure-subscription.md#what-happens-after-i-cancel-my-subscription).

## <a name="delete-an-account"></a>Löschen eines Kontos

Eine Kontoentfernung kann nur für aktive Konten ohne aktive Abonnements durchgeführt werden.

1. Wählen Sie im Enterprise Portal im linken Navigationsbereich die Option **Verwalten** aus.
1. Klicken Sie auf die Registerkarte **Konto**.
1. Wählen Sie in der Kontotabelle das Konto aus, das Sie löschen möchten.
1. Wählen Sie rechts von der Zeile „Konto“ das Symbol „X“ aus.
1. Wenn unter dem Konto keine aktiven Abonnements mehr vorhanden sind, können Sie unter der Kontozeile die Schaltfläche **Ja** auswählen, um die Kontoentfernung zu bestätigen.

## <a name="update-notification-settings"></a>Aktualisierung der Benachrichtigungseinstellungen

Unternehmensadministratoren werden automatisch für den Erhalt von Nutzungsbenachrichtigungen für ihre Registrierung registriert. Jeder Unternehmensadministrator kann das Intervall der einzelnen Benachrichtigungen ändern oder diese vollständig deaktivieren.

Benachrichtigungskontakte werden im Azure EA-Portal im Abschnitt **Benachrichtigungskontakt** angezeigt. Durch die Verwaltung Ihrer Benachrichtigungskontakte stellen Sie sicher, dass die richtigen Personen in Ihrer Organisation Azure EA-Benachrichtigungen erhalten.

So zeigen Sie die aktuellen Benachrichtigungseinstellungen an:

1. Navigieren Sie im Azure EA-Portal zu **Verwalten** > **Benachrichtigungskontakt**.
2. E-Mail-Adresse: Die E-Mail-Adresse, die dem Microsoft-Konto des Unternehmensadministrators oder dem Geschäfts-, Schul- oder Unikonto zugeordnet ist, das die Benachrichtigungen erhält
3. Benachrichtigungshäufigkeit zu nicht in Rechnung gestellten Salden: Die Häufigkeit, mit der Benachrichtigungen an die einzelnen Unternehmensadministratoren gesendet werden

So fügen Sie einen Kontakt hinzu:

1. Wählen Sie **+ Kontakt hinzufügen** aus.
2. Geben Sie die E-Mail-Adresse ein, und bestätigen Sie sie.
3. Wählen Sie **Speichern** aus.

Der neue Benachrichtigungskontakt wird im Abschnitt **Benachrichtigungskontakt** angezeigt. Wählen Sie zum Ändern der Benachrichtigungshäufigkeit den Benachrichtigungskontakt und das Stiftsymbol rechts neben der ausgewählten Zeile aus. Legen Sie die Häufigkeit auf **Täglich**, **Wöchentlich**, **Monatlich** oder **Keine** fest.

Sie können das _bevorstehende Enddatum des Abdeckungszeitraums_ unterdrücken und deaktivieren und die _Bereitstellung von Lebenszyklusbenachrichtigungen zu bevorstehenden Daten aufheben_. Durch das Deaktivieren von Lebenszyklusbenachrichtigungen werden Benachrichtigungen über den Abdeckungszeitraum und das Enddatum der Vereinbarung unterdrückt.

## <a name="azure-sponsorship-offer"></a>Azure Sponsorship-Angebot

Das Azure Sponsorship-Angebot ist ein eingeschränktes gesponsertes Microsoft Azure-Konto. Es ist nur per E-Mail-Einladung für bestimmte Kunden verfügbar, die von Microsoft ausgewählt wurden. Wenn Sie zur Nutzung des Microsoft Azure Sponsorship-Angebots berechtigt sind, erhalten Sie eine E-Mail-Einladung für Ihre Konto-ID.

Sollten Sie weitere Informationen benötigen, erstellen Sie eine [Supportanfrage für die Sponsorship-Aktivierung](https://aka.ms/azrsponsorship).

## <a name="conversion-to-work-or-school-account-authentication"></a>Umstellung auf die Authentifizierung per Geschäfts-, Schul- oder Unikonto

Azure-Unternehmensbenutzer können von der Authentifizierung per Microsoft-Konto (MSA oder Live-ID) zur Authentifizierung per Geschäfts-, Schul- oder Unikonto (unter Verwendung von Active Directory in Azure) wechseln.

Vorbereitung:

1. Fügen Sie dem Azure EA-Portal das Geschäfts-, Schul- oder Unikonto in den benötigten Rollen hinzu.
1. Sollten Fehler auftreten, ist das Konto in Active Directory möglicherweise nicht gültig.  Azure verwendet den Benutzerprinzipalnamen (User Principal Name, UPN), und dieser ist nicht immer mit der E-Mail-Adresse identisch.
1. Authentifizieren Sie sich beim Azure EA-Portal mit dem Geschäfts-, Schul- oder Unikonto.

### <a name="to-convert-subscriptions-from-microsoft-accounts-to-work-or-school-accounts"></a>So stellen Sie Abonnements von Microsoft-Konten auf Geschäfts-, Schul- oder Unikonten um:

1. Melden Sie sich mit dem Microsoft-Konto, zu dem die Abonnements gehören, beim Verwaltungsportal an.
1. Verwenden Sie die Übertragung des Kontobesitzes, um zu dem neuen Konto zu wechseln.
1. Für das Microsoft-Konto sollten nun keine aktiven Abonnements mehr vorhanden sein, sodass es gelöscht werden kann.
1. Aus Abrechnungsgründen werden gelöschte Konten im Portal weiterhin angezeigt, sie haben allerdings den Status „Inaktiv“.  Wenn Sie sie aus der Ansicht herausfiltern möchten, können Sie durch Aktivieren eines Kontrollkästchens festlegen, dass nur aktive Konten angezeigt werden sollen.

## <a name="account-subscription-ownership-faq"></a>Häufige gestellte Fragen zum Kontoabonnementbesitz

In diesem Dokument werden häufig gestellte Fragen im Zusammenhang mit dem Kontoabonnementbesitz beantwortet.

### <a name="how-many-azure-account-owners-can-you-have-per-subscription"></a>Wie viele Azure-Kontobesitzer sind pro Abonnement zulässig?

Pro Abonnement ist immer nur ein einzelner Kontobesitzer zulässig.  Weitere Rollen können mithilfe des rollenbasierten Zugriffs oder der Zugriffssteuerung (IAM) im [Azure-Portal](https://portal.azure.com) auf der Registerkarte „Abonnement“ (links oben auf der Seite) hinzugefügt werden.

### <a name="can-an-azure-account-owner-be-listed-under-more-than-one-department"></a>Kann ein Azure-Kontobesitzer unter mehreren Abteilungen aufgeführt werden?

Nein. Ein Kontobesitzer kann immer nur einer einzelnen Abteilung zugeordnet sein. Durch diese Richtlinie wird eine korrekte Überwachung und Zuordnung von Kosten und Ausgaben im Zusammenhang mit der entsprechenden Abteilung unter der EA-Registrierung im Azure EA Portal sichergestellt.

### <a name="can-an-azure-account-owner-be-listed-as-a-security-group"></a>Kann ein Azure-Kontobesitzer als Sicherheitsgruppe aufgeführt werden?

Nein. Bei einem Abonnementbesitzer muss es sich um eine eindeutige Authentifizierung vom Typ Microsoft-Konto (MSA) oder Azure Active Directory (AAD) handeln. Zur Regelung der Nachfolge in Ihrer Organisation können Sie ggf. generische Konten erstellen und den Abonnementzugriff mithilfe von AAD verwalten.

### <a name="can-an-individual-user-own-multiple-subscriptions"></a>Kann ein einzelner Benutzer mehrere Abonnements besitzen?

Ein Azure-Kontobesitzer kann eine unbegrenzte Anzahl von Abonnements erstellen und verwalten.

### <a name="how-can-i-accessview-all-my-organizations-subscriptions"></a>Wie kann ich auf alle Abonnements meiner Organisation zugreifen bzw. diese anzeigen?

Hierzu muss aktuell eine Richtlinie verwendet werden. Ihr Konto muss also für jedes erstellte Abonnement mithilfe des rollenbasierten Zugriffs einer Abonnementrolle hinzugefügt werden.

### <a name="where-do-i-go-to-create-a-subscription"></a>Wo kann ich ein Abonnement erstellen?

Wenn Sie ein Abonnement für ein EA-Angebot (Enterprise Azure) erstellen möchten, muss Ihr Konto zunächst im Azure EA Portal durch den Administrator Ihrer EA-Registrierung der Rolle „Kontobesitzer“ hinzugefügt werden. Anschließend müssen Sie sich beim Azure EA Portal anmelden, um Ihre Berechtigung zum Erstellen von Abonnements für den EA-Angebotstyp zu erhalten. Das erste EA-Abonnement sollte über den Link „+ Abonnement hinzufügen“ auf der Registerkarte „Abonnement“ des EA Portals erstellt werden.  Sobald Ihr Konto allerdings über die nötige Berechtigung verfügt, ist es unter Umständen einfacher, Abonnements unter „portal.azure.com“ auf der Registerkarte „Abonnement“ (links oben auf der Seite) zu erstellen, da Sie Ihr Abonnement dort in einem einzelnen Schritt erstellen und umbenennen können.

### <a name="who-can-create-a-subscription"></a>Wer kann ein Abonnement erstellen?

Abonnements für den Enterprise Azure-Angebotstyp können nur von Benutzern erstellt werden, die im [EA-Portal](https://ea.azure.com) über die Rolle „Kontobesitzer“ verfügen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie mit [Reservierungen virtueller Computer](ea-portal-vm-reservations.md) Geld sparen können.
- Hilfe zu Problemen im Azure EA-Portal finden Sie unter [Beheben von Zugriffsproblemen beim Azure EA-Portal](ea-portal-troubleshoot.md).
