---
title: Häufig gestellte Fragen zur Migration zu Partner Center – kommerzieller Microsoft-Marketplace
description: Antworten auf häufig gestellte Fragen zur Migration von Angeboten aus dem Cloud-Partnerportal zu Partner Center
author: anbene
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mingshen
ms.openlocfilehash: c4fdde94ea703d194e2de27a8df429f62ea374f4
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83727478"
---
# <a name="common-questions-about-migrating-from-the-cloud-partner-portal-to-partner-center"></a>Häufig gestellte Fragen zur Migration aus dem Cloud-Partnerportal zu Partner Center

In diesem Artikel werden häufig gestellte Fragen zur Migration von Angeboten aus dem Cloud-Partnerportal zu Partner Center behandelt.

## <a name="what-does-offer-migration-mean"></a>Was bedeutet Migration von Angeboten?

Wir verschieben Ihre Angebotsdaten aus dem Cloud-Partnerportal in Partner Center. Dabei werden Änderungen an der Benutzeroberfläche für die Veröffentlichung und Verwaltung von Angeboten vorgenommen.

| Bereich  | Änderungen  |
|-------|----------|
| **Veröffentlichungs- und Angebotsverwaltungsoberfläche** | In Partner Center steht Ihnen eine verbesserte Benutzeroberfläche mit einer intuitiven Oberfläche zur Verfügung. Ausführlichere Informationen finden Sie unter [Welche Unterschiede bestehen zwischen Partner Center und dem Cloud-Partnerportal?](#what-are-the-differences-between-partner-center-and-the-cloud-partner-portal) |
| **Verfügbarkeit Ihrer Angebote im Marketplace** | Keine Änderungen. Wenn Ihr Angebot im Marketplace live geschaltet ist, wird es während und nach Abschluss der Migration weiterhin live geschaltet sein. |
| **Neue Einkäufe und Bereitstellungen** | Keine Änderungen. Ihre Kunden können Ihre Angebote weiterhin ohne Unterbrechungen erwerben und bereitstellen. |
| **Auszahlungen** | Alle Käufe und Bereitstellungen, die während oder nach der Migration erfolgen, werden Ihnen weiterhin wie gewohnt ausgezahlt. |
|**API-Integrationen in vorhandene [Cloud-Partnerportal-APIs](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview)** | Vorhandene Cloud-Partnerportal-APIs werden nach der Migration weiterhin unterstützt, und Ihre vorhandenen Integrationen funktionieren auch weiterhin. Ausführlichere Informationen finden Sie unter [Werden die Cloud-Partnerportal-REST-APIs nach der Migration unterstützt?](#will-the-cloud-partner-portal-rest-apis-be-supported-post-migration) |
| | |

## <a name="can-i-still-access-the-cloud-partner-portal-and-manage-my-offers-during-migration"></a>Kann ich weiterhin auf das Cloud-Partnerportal zugreifen und meine Angebote während der Migration verwalten?

Alle Angebote wurden zu Partner Center migriert. Der Migrationszeitraum für jedes Angebot sollte weniger als 24 Stunden betragen. Nachdem das Angebot migriert wurde, sollten Sie eine E-Mail-Benachrichtigung erhalten haben.

Sobald Ihre Angebote migriert wurden, sind sie im Cloud-Partnerportal **für einen begrenzten Zeitraum** schreibgeschützt. Ihr Status lautet „In Partner Center verschoben“ und enthält einen Link zu Ihrem Angebot in Partner Center, wie in den folgenden Screenshots dargestellt. Ab diesem Zeitpunkt verwalten Sie Aktualisierungen all Ihrer Angebote oder erstellen neue Angebote ausschließlich über Partner Center.

:::image type="content" source="media/migration-faq/all-offers-2.png" alt-text="Zeigt die Meldung, die für zu Partner Center migrierte Angebote angezeigt wird":::

:::image type="content" source="media/migration-faq/offer-has-moved.png" alt-text="Screenshot der Seite des Cloud-Partnerportals für ein migriertes Angebot.":::

## <a name="how-will-i-create-new-offers"></a>Wie erstelle ich neue Angebote?

Im Cloud-Partnerportal werden Sie angewiesen, neue Angebote in Partner Center zu erstellen.

:::image type="content" source="media/migration-faq/create-new-offer-1.png" alt-text="Screenshot des Menüs zum Erstellen eines neuen Angebots im Cloud-Partnerportal":::

Nachdem Sie ein neues Angebot ausgewählt haben, wird eine Meldung wie die folgende angezeigt.

:::image type="content" source="media/migration-faq/create-new-offer-2.png" alt-text="Zeigt die Meldung, die beim Erstellen eines neuen Angebots im Cloud-Partnerportal angezeigt wird":::

## <a name="do-i-need-to-create-a-new-account-to-manage-offers-in-partner-center"></a>Muss ich ein neues Konto erstellen, um Angebote in Partner Center verwalten zu können?

Nein. Ihr zugrunde liegendes Konto wird beibehalten, und Sie können es bereits in Partner Center verwalten. Dies bedeutet Folgendes: Wenn Sie ein bestehender Partner sind, können Sie die Anmeldeinformationen Ihres vorhandenen Cloud-Partnerportal-Kontos verwenden, um sich nach der Migration bei Partner Center anzumelden. Nur Angebote und die zugehörige Verwaltungsumgebung werden aus dem Cloud-Partnerportal in Partner Center verschoben. Wir bitten Sie, keine neuen Konten zu erstellen, weil Ihre Angebote nicht dem neuen Konto zugeordnet werden.

## <a name="i-see-a-message-in-the-cloud-partner-portal-to-activate-my-account-what-does-this-mean"></a>Im Cloud-Partnerportal wird eine Meldung angezeigt, die besagt, dass ich mein Konto aktivieren soll. Was bedeutet das?

Wir benötigen einige weitere Details von Ihnen, damit wir Ihr Konto ordnungsgemäß zu Partner Center migrieren und Ihnen nach Abschluss der Angebotsmigration die Verwaltung Ihrer Angebote in Partner Center ermöglichen können. Ausführlichere Informationen zur Kontoaktivierung finden Sie unter [Kontomigration vom Cloud-Partnerportal zu Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/account-migration-from-cpp-to-pc).

Die zum Durchführen der Kontoaktivierung erforderlichen Schritte variieren je nach Kontorolle.

| Kontorolle | Aktivierungsschritte |
|--------------|----------------|
|Besitzer | Navigieren Sie im Cloud-Partnerportal zur Seite [Herausgeberprofil](https://cloudpartner.azure.com/#profile), und klicken Sie dann zur Aktivierung auf den Link im Banner. Sie werden zu Partner Center umgeleitet, um die Kontoaktivierung abzuschließen. |
| Mitwirkender | Nur ein Benutzer im Konto mit der Rolle „Besitzer“ kann das Konto aktivieren. Wenden Sie sich an Ihre Kontobesitzer, damit diese die Kontoaktivierung abschließen. Ihre Kontobesitzer sollten in der Bannermeldung aufgeführt sein. |
| | |

## <a name="im-having-trouble-logging-in-to-my-account-and-opening-a-support-ticket"></a>Ich kann mich nicht bei meinem Konto anmelden und habe Probleme beim Öffnen eines Supporttickets

Wenn Sie sich nicht bei Ihrem Konto anmelden können, haben Sie die Möglichkeit, ein [Supportticket](https://partner.microsoft.com/support/v2/?stage=1) zu öffnen.

## <a name="where-can-i-find-documentation-on-the-new-partner-center-publishing-experience"></a>Wo finde ich die Dokumentation zur neuen Partner Center-Veröffentlichungsoberfläche?

Navigieren Sie zur [Dokumentation zum kommerziellen Marketplace](https://docs.microsoft.com/azure/marketplace/). Erweitern Sie dann **Kommerzieller Marketplace in Partner Center**  > **Erstellen eines neuen Angebots**, um die Hilfethemen zum Erstellen der einzelnen Angebotstypen anzuzeigen.

:::image type="content" source="media/migration-faq/marketplace-help-topics.png" alt-text="Screenshot der Hilfethemen für Partner Center":::

### <a name="what-are-the-differences-between-partner-center-and-the-cloud-partner-portal"></a>Welche Unterschiede bestehen zwischen Partner Center und dem Cloud-Partnerportal?

Möglicherweise fallen Ihnen die folgenden Unterschiede zwischen dem Cloud-Partnerportal und Partner Center auf.

### <a name="modular-publishing-capabilities"></a>Modulare Veröffentlichungsfunktionen

Partner Center bietet eine modulare Veröffentlichungsoption, mit der Sie die zu veröffentlichenden Änderungen auswählen können, statt immer alle Aktualisierungen auf einmal zu veröffentlichen. Im folgenden Screenshot ist z. B. zu sehen, dass nur die an **Eigenschaften** und **Angebotsliste** vorgenommenen Änderungen für die Veröffentlichung ausgewählt sind.

:::image type="content" source="media/migration-faq/review-and-publish-migration.png" alt-text="Screenshot der Seite „Überprüfen und veröffentlichen“":::

Die Aktualisierungen, die Sie nicht veröffentlichen, werden als Entwürfe gespeichert. Verwenden Sie weiterhin Ihre Angebotsvorschau, um Ihr Angebot zu überprüfen, bevor Sie es für die Öffentlichkeit live schalten.

### <a name="rich-text-format"></a>Rich-Text-Format

Optimieren Sie Ihre Angebots- und Planbeschreibung mithilfe des Rich-Text-Editors auf der Seite „Angebotsliste“ und „Planlisting“.

:::image type="content" source="media/migration-faq/rich-text-editor-migration.png" alt-text="Screenshot des Rich-Text-Editors":::

### <a name="enhanced-preview-options"></a>Erweiterte Vorschauoptionen

Partner Center enthält eine [Vergleichsfunktion](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#compare-changes-to-marketplace-offers) mit verbesserten Filteroptionen. Damit können Sie die Vorschauversion mit der Liveversion des Angebots vergleichen.

:::image type="content" source="media/migration-faq/enhanced-preview.png" alt-text="Screenshot der Vergleichsfunktion":::

### <a name="branding-and-navigation-changes"></a>Branding- und Navigationsänderungen

Es wurden einige Änderungen am Branding vorgenommen. So werden beispielsweise „SKUs“ in Partner Center als „Pläne“ bezeichnet.

:::image type="content" source="media/migration-faq/plan-overview-migration.png" alt-text="Screenshot der Planübersicht.":::

Außerdem werden die Informationen, die Sie im Cloud-Partnerportal auf der Seite **Marketplace** oder **Details der digitalen Ladenzeile** (Beratungsdienst, Power BI-App) angegeben haben, in Partner Center auf der Seite **Angebotsliste** erfasst.

:::image type="content" source="media/migration-faq/offer-listing-migration.png" alt-text="Screenshot der Seite „Angebotsliste“.":::

Die Informationen, die Sie für SKUs im Cloud-Partnerportal auf einer einzigen Seite angegeben haben, werden jetzt in Partner Center möglicherweise auf mehreren Seiten erfasst:

* Seite „Plansetup“
* Seite „Planlisting“
* Seite für die Planverfügbarkeit
* Seite „Technische Konfiguration“ für den Plan, wie in diesem Screenshot dargestellt.

:::image type="content" source="media/migration-faq/tech-config-migration.png" alt-text="Screenshot der Seite „Technische Konfiguration“ für den Plan.":::

Ihre Angebots-ID wird jetzt auf der linken Navigationsleiste des Angebots angezeigt.

:::image type="content" source="media/migration-faq/offer-id-offer-overview.png" alt-text="Screenshot des linken Navigationsmenüs mit der Angebots-ID.":::

### <a name="stop-selling-an-offer"></a>Beenden des Verkaufs eines Angebots

Sie können direkt im Partner Center-Portal die [Beendigung des Verkaufs eines Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan) im Marketplace anfordern. Die entsprechende Option ist auf der Seite **Angebotsübersicht** für Ihr Angebot verfügbar.

:::image type="content" source="media/migration-faq/stop-selling-migration.png" alt-text="Screenshot der Seite „Angebotsübersicht“ mit der Option „Verkauf beenden“.":::

## <a name="what-pages-in-partner-center-correspond-to-pages-i-used-in-the-cloud-partner-portal"></a>Welche Seiten in Partner Center entsprechen den Seiten, die ich im Cloud-Partnerportal verwendet habe?

In der folgenden Tabelle finden Sie die entsprechenden Links der beiden Portale.

| Seite | Cloud-Partnerportal-Link | Partner Center-Link |
|------|---------------------------|---------------------|
| **Seite „Alle Angebote“** | https://cloudpartner.azure.com/#alloffers | https://partner.microsoft.com/dashboard/commercial-marketplace/overview |
| **Seite „Alle Herausgeber“** | https://cloudpartner.azure.com/#publishers | https://partner.microsoft.com/dashboard/account/v3/publishers/list |
| **Herausgeberprofil** | https://cloudpartner.azure.com/#profile | https://partner.microsoft.com/dashboard/account/management |
| **Seite „Benutzer“** | https://cloudpartner.azure.com/#users | https://partner.microsoft.com/dashboard/account/usermanagement |
| **Seite „Verlauf“** | https://cloudpartner.azure.com/#history | Die Funktion „Verlauf“ wird in Partner Center noch nicht unterstützt. |
| **Insights-Dashboard** | https://cloudpartner.azure.com/#insights | https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary |
| **Auszahlungsbericht** | https://cloudpartner.azure.com/#insights/payout | https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments |
| | |

## <a name="will-the-cloud-partner-portal-rest-apis-be-supported-post-migration"></a>Werden die Cloud-Partnerportal-REST-APIs nach der Migration unterstützt?

Die Cloud-Partnerportal-APIs sind in Partner Center integriert und werden auch nach der Migration Ihrer Angebote zu Partner Center weiterhin funktionieren. Die Integration führt zu kleineren Änderungen. Beachten Sie die Änderungen in der folgenden Tabelle, um sicherzustellen, dass Ihr Code nach der Migration zu Partner Center weiterhin funktioniert.

| **API** | **Beschreibung der Änderung** | **Auswirkung** |
| ------- | ---------------------- | ---------- |
| POST Publish, GoLive, Cancel | Bei migrierten Angeboten weist der Antwortheader ein anderes Format auf. Er funktioniert jedoch weiterhin auf dieselbe Weise und gibt einen relativen Pfad zum Abrufen des Vorgangsstatus an. | Beim Senden einer der entsprechenden POST-Anforderungen für ein Angebot weist der Adressheader je nach Migrationsstatus des Angebots eines der beiden folgenden Formate auf:<ul><li>Nicht migrierte Angebote<br>`/api/operations/{PublisherId}${offerId}$2$preview?api-version=2017-10-31`</li><li>Migrierte Angebote<br>`/api/publishers/{PublisherId}/offers/{offereId}/operations/408a4835-0000-1000-0000-000000000000?api-version=2017-10-31`</li> |
| GET-Vorgang | Bei Angeboten, die zuvor das Feld „Benachrichtigungs-E-Mail“ in der Antwort unterstützten, wird dieses Feld entfernt und für migrierte Angebote nicht mehr zurückgegeben. | Bei migrierten Angeboten werden keine Benachrichtigungen mehr an die Liste der in den Anforderungen angegebenen E-Mail-Adressen gesendet. Stattdessen wird der API-Dienst an den Benachrichtigungs-E-Mail-Prozess in Partner Center angepasst, um E-Mails zu senden. Insbesondere werden Benachrichtigungen an die E-Mail-Adresse gesendet, die in Partner Center in Ihren Kontoeinstellungen im Abschnitt der Kontaktinformationen des Verkäufers festgelegt wurde, um Sie über den Fortschritt des Vorgangs zu benachrichtigen.<br><br>Überprüfen Sie die in Partner Center in den [Kontoeinstellungen](https://partner.microsoft.com/dashboard/account/management) im Abschnitt der Kontaktinformationen des Verkäufers festgelegte E-Mail-Adresse, um sicherzustellen, dass die richtige E-Mail-Adresse für Benachrichtigungen angegeben ist.  |
| | | |
