---
title: Testen und Veröffentlichen eines SaaS-Angebots im kommerziellen Microsoft-Marketplace
description: Verwenden Sie das Partner Center, um Ihr SaaS-Angebot für die Vorschau zu übermitteln, eine Vorschau Ihres Angebots anzuzeigen und es dann zu testen und im kommerziellen Microsoft-Marketplace zu veröffentlichen.
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 03/10/2021
ms.openlocfilehash: ea647591194dfd6ba0bf818f4ce4d3a2005f0bc4
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102630780"
---
# <a name="how-to-test-and-publish-a-saas-offer-to-the-commercial-marketplace"></a>Testen und Veröffentlichen eines SaaS-Angebots im kommerziellen Marketplace

In diesem Artikel erfahren Sie, wie Sie mithilfe des Partner Centers Ihr SaaS-Angebot für die Veröffentlichung übermitteln, eine Vorschau Ihres Angebots anzeigen und es testen und dann im kommerziellen Marketplace live veröffentlichen. Sie müssen bereits ein Angebot erstellt haben, das Sie veröffentlichen möchten.

## <a name="submit-your-offer-for-publishing"></a>Übermitteln des Angebots für die Veröffentlichung

1. Melden Sie sich im [Partner Center](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) beim Dashboard „Kommerzieller Marketplace“ an.
1. Wählen Sie auf der Seite **Übersicht** das Angebot aus, das Sie veröffentlichen möchten.
1. Wählen Sie in der oberen rechten Ecke des Portals **Überprüfen und veröffentlichen** aus.
2. Stellen Sie sicher, dass auf jeder Seite in der Spalte **Status** der Eintrag **Abgeschlossen** angezeigt wird. Die drei möglichen Statuswerte lauten wie folgt:

   - **Nicht gestartet**: Die Seite ist unvollständig.
   - **Unvollständig**: Auf der Seite fehlen erforderliche Informationen, oder sie enthält Fehler, die korrigiert werden müssen. Sie müssen zur Seite zurückkehren und diese aktualisieren.
   - **Abgeschlossen**: Die Seite ist vollständig. Alle erforderlichen Daten wurden angegeben, und es liegen keine Fehler vor.

1. Wenn eine der Seiten einen anderen Status als **Abgeschlossen** aufweist, wählen Sie den Namen der Seite aus, beheben das Problem, speichern die Seite und wählen dann erneut **Überprüfen und veröffentlichen** aus, um zu dieser Seite zurückzukehren.
1. Nachdem alle Seiten abgeschlossen sind, stellen Sie dem Zertifizierungsteam unter **Hinweise für Zertifizierung** Testanweisungen zur Verfügung, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird. Stellen Sie alle ergänzenden Anmerkungen bereit, die das Verständnis Ihrer App unterstützen.
1. Wählen Sie **Veröffentlichen** aus, um den Veröffentlichungsprozess für Ihr Angebot zu starten. Die Seite **Angebotsübersicht** wird mit dem **Veröffentlichungsstatus** des Angebots angezeigt.

Der Veröffentlichungsstatus Ihres Angebots ändert sich, während es den Veröffentlichungsprozess durchläuft. Ausführliche Informationen zu diesem Prozess finden Sie unter [Schritte für die Validierung und Veröffentlichung](review-publish-offer.md#validation-and-publishing-steps).

## <a name="preview-and-test-your-offer"></a>Vorschau und Test Ihres Angebots

Wenn das Angebot für Ihre Genehmigung bereit ist, senden wir Ihnen eine E-Mail mit der Bitte, Ihre Angebotsvorschau zu überprüfen und zu genehmigen. Sie können auch die Seite **Angebotsübersicht** in Ihrem Browser aktualisieren, um festzustellen, ob Ihr Angebot die Phase „Einholen der Genehmigung des Herausgebers“ erreicht hat. Wenn dies der Fall ist, sind die Schaltfläche **Live schalten** und Vorschaulinks verfügbar. Abhängig von den Optionen, die Sie beim Erstellen Ihres Angebots ausgewählt haben, ist ein Link für die Microsoft AppSource-Vorschau, die Azure Marketplace-Vorschau oder beide verfügbar. Wenn Sie sich für den Verkauf Ihres Angebots über Microsoft entschieden haben, können alle Mitglieder der Vorschauzielgruppe den Kauf und die Bereitstellung Ihres Angebots testen, um sicherzustellen, dass es Ihre Anforderungen in dieser Phase erfüllt.

Der folgende Screenshot zeigt die Seite **Angebotsübersicht** für ein SaaS-Angebot mit zwei Vorschaulinks unter der Schaltfläche **Live schalten**. Die auf dieser Seite angezeigten Validierungsschritte variieren je nach der Auswahl, die Sie beim Erstellen des Angebots getroffen haben.

![Veranschaulicht die Seite „Angebotsübersicht“ für ein Angebot im Partner Center. Die Schaltfläche „Live schalten“ und Vorschaulinks sind dargestellt. Der Link „Prüfbericht anzeigen“ wird auch unter „Automatisierte Validierung“ angezeigt.](./media/review-publish-offer/publish-status-saas.png)

Gehen Sie wie folgt vor, um Ihr Angebot in der Vorschau anzuzeigen.

1. Wählen Sie auf der Seite **Angebotsübersicht** unter der Schaltfläche **Live schalten** einen Vorschaulink aus.

1. Um den gesamten Ablauf vom Kauf bis zur Einrichtung zu überprüfen, erwerben Sie Ihr Angebot, während es sich in der Vorschau befindet. Benachrichtigen Sie zunächst Microsoft mit einem [Supportticket](https://aka.ms/marketplacesupport), um sicherzustellen, dass wir keine Gebühren erheben.

1. Wenn Ihr SaaS-Angebot die [getaktete Abrechnung mit dem Messungsdienst des kommerziellen Marketplace](./partner-center-portal/saas-metered-billing.md) unterstützt, lesen und befolgen Sie die bewährten Testmethoden, die unter [APIs für getaktete Abrechnung im Marketplace](./partner-center-portal/marketplace-metering-service-apis.md#development-and-testing-best-practices) beschrieben sind.

1. Lesen und befolgen Sie die Testanweisungen unter [SaaS-Fulfillment-APIs (Version 2) im kommerziellen Microsoft-Marketplace](./partner-center-portal/pc-saas-fulfillment-api-v2.md#development-and-testing), um sicherzustellen, dass Ihr Angebot erfolgreich in die APIs integriert wird, bevor Sie Ihr Angebot live veröffentlichen.

1. Wenn der Schritt zur Angebotsvalidierung zu Warnungen geführt hat, wird auf der Seite **Angebotsübersicht** der Link **Prüfbericht anzeigen** angezeigt. Überprüfen Sie den Bericht, und beheben Sie die Probleme, bevor Sie die Schaltfläche **Live schalten** auswählen. Andernfalls riskieren Sie eine fehlerhafte Zertifizierung, die zu einer Verzögerung bei der Aktivierung Ihres Angebots führt.

1. Wenn Sie nach der Vorschau und den Tests Änderungen am Angebot vornehmen möchten, können Sie es bearbeiten und erneut übermitteln, um eine neue Vorschau zu veröffentlichen. Weitere Informationen finden Sie unter [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./partner-center-portal/update-existing-offer.md).

## <a name="publish-your-offer-live"></a>Veröffentlichen Ihres Angebots im Livemodus

Nachdem Sie Ihre Vorschau vollständig getestet haben, wählen Sie **Live schalten** aus, um das Angebot live im kommerziellen Marketplace zu veröffentlichen.

   > [!TIP]
   > Wenn Ihr Angebot bereits im kommerziellen Marketplace live geschaltet ist, werden alle von Ihnen vorgenommenen Aktualisierungen erst dann live geschaltet, wenn Sie **Live schalten** auswählen.

Nachdem Sie sich entschieden haben, Ihr Angebot im kommerziellen Marketplace verfügbar zu machen, stellen wir mit einer Reihe abschließender Validierungsprüfungen sicher, dass das Liveangebot auf dieselbe Weise wie die Vorschauversion des Angebots konfiguriert ist. Ausführliche Informationen zu diesen Validierungsprüfungen finden Sie unter [Phase „Veröffentlichung“](review-publish-offer.md#publish-phase).

Nachdem die Validierungsprüfungen abgeschlossen wurden, wird Ihr Angebot im Marketplace live geschaltet.

## <a name="next-steps"></a>Nächste Schritte

- [Zugreifen auf Analysen für den kommerziellen Marketplace in Partner Center](./partner-center-portal/analytics.md)
