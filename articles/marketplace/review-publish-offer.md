---
title: Überprüfen und Veröffentlichen eines Angebots im kommerziellen Microsoft-Marketplace
description: Verwenden Sie das Partner Center, um Ihr Angebot für die Vorschau zu übermitteln, eine Vorschau Ihres Angebots anzuzeigen und es dann im kommerziellen Microsoft-Marketplace zu veröffentlichen.
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 07/05/2020
ms.openlocfilehash: 34e56e5d92526cbf46408c670127e87781e342cd
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119734"
---
# <a name="review-and-publish-an-offer-to-commercial-marketplace"></a>Überprüfen und Veröffentlichen eines Angebots im kommerziellen Marketplace

In diesem Artikel erfahren Sie, wie Sie mithilfe des Partner Centers Ihr Angebot für die Vorschau übermitteln, eine Vorschau Ihres Angebots anzeigen und es dann im kommerziellen Microsoft-Marketplace veröffentlichen. Außerdem wird erläutert, wie Sie den Veröffentlichungsstatus überprüfen, während das Angebot die Veröffentlichungsschritte durchläuft. Sie müssen bereits ein Angebot erstellt haben, das Sie veröffentlichen möchten.

## <a name="go-to-your-offer-in-commercial-marketplace"></a>Wechseln zu Ihrem Angebot im kommerziellen Marketplace

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
1. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.
1. Auf der Registerkarte **Übersicht** unter **Angebote** wird in der Spalte **Status** für jedes Angebot einer der folgenden Statuswerte angezeigt.
 
    | Status | BESCHREIBUNG |
    | ------------ | ------------- |
    | Entwurf | Angebot wurde erstellt, wird aber nicht veröffentlicht. |
    | Veröffentlichung wird durchgeführt | Das Angebot durchläuft den Veröffentlichungsvorgang. |
    | Eingreifen erforderlich | Wir haben während der Zertifizierung oder einer anderen Veröffentlichungsphase ein kritisches Problem festgestellt. |
    | Vorschau | Wir haben das Angebot zertifiziert. Nun muss es abschließend durch den Herausgeber verifiziert werden. Wählen Sie **Live schalten** aus, um das Angebot live zu veröffentlichen. |
    | Live | Das Angebot ist im Marketplace live geschaltet und kann von Kunden angezeigt und erworben werden. |
    | Ausstehender Verkaufsstopp | Der Herausgeber hat „Verkaufsstopp“ für ein Angebot oder einen Plan ausgewählt, aber die Aktion ist noch nicht abgeschlossen. |
    | Im Marketplace nicht verfügbar | Ein zuvor im Marketplace veröffentlichtes Angebot wurde entfernt. |
    |||

4. Wählen Sie in der Spalte **Angebotsalias** das Angebot aus, das Sie in der Vorschau anzeigen und veröffentlichen möchten.

## <a name="submit-your-offer-to-preview"></a>Übermitteln des Angebots für die Vorschau

1. Um Ihr Angebot für die Vorschau zu übermitteln, wählen Sie oben rechts im Portal **Überprüfen und veröffentlichen** aus. Die Seite **Überprüfen und veröffentlichen** wird angezeigt.
1. Stellen Sie sicher, dass auf jeder Seite in der Spalte **Status** der Eintrag **Abgeschlossen** angezeigt wird. Die drei möglichen Statuswerte lauten wie folgt:
   - **Nicht gestartet**: Die Seite wurde nicht bearbeitet und muss abgeschlossen werden.
   - **Unvollständig**: Auf der Seite fehlen erforderliche Informationen, oder sie enthält Fehler, die korrigiert werden müssen. Sie müssen zur Seite zurückkehren und diese aktualisieren.
   - **Abgeschlossen**: Die Seite ist vollständig. Alle erforderlichen Daten wurden angegeben, und es liegen keine Fehler vor.
1. Wenn eine der Seiten einen anderen Status als **Abgeschlossen** aufweist, wählen Sie in der Spalte **Seite** den Namen der Seite aus, beheben das Problem, speichern die Seite und wählen dann erneut **Überprüfen und veröffentlichen** aus, um zu dieser Seite zurückzukehren.
1. Nachdem alle Seiten abgeschlossen sind, stellen Sie dem Zertifizierungsteam unter **Hinweise für Zertifizierung** Testanweisungen zur Verfügung, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird. Stellen Sie alle ergänzenden Anmerkungen bereit, die das Verständnis Ihrer App unterstützen.
1. Um das Angebot zur Veröffentlichung einzureichen, wählen Sie **Veröffentlichen** aus. Die Seite **Angebotsübersicht** wird mit dem Veröffentlichungsstatus angezeigt.

## <a name="validation-and-publishing-steps"></a>Schritte für die Validierung und Veröffentlichung

Nachdem Sie **Veröffentlichen** ausgewählt haben, werden nacheinander der Validierungs- und Veröffentlichungsprozess ausgeführt. In dieser Tabelle ist der häufigste Veröffentlichungsprozess dargestellt:

| Phase | Ablauf | 
| ------------ | ------------- | ------------- |
| Automatisierte Validierung | Wir verarbeiten eine Reihe automatisierter Validierungen. | 
| Zertifizierung | Wir führen manuelle Validierungen durch. | 
| Erstellung einer Vorschau | Die Listungsseite für Ihre Angebotsvorschau steht allen zur Verfügung, die über den Vorschaulink verfügen. Wenn Ihr Angebot über Microsoft vertrieben wird (also transaktionsfähig ist), kann nur die auf der Seite **Vorschauzielgruppe** Ihres Angebots angegebene Zielgruppe das Angebot erwerben und zu Testzwecken darauf zugreifen. | 
| Einholen der Genehmigung des Herausgebers | Wir senden Ihnen eine E-Mail mit der Aufforderung, Ihr Angebot in der Vorschau zu überprüfen und zu genehmigen. | 
| Veröffentlichen | Wir stellen anhand einiger Schritte sicher, dass das Vorschauangebot im kommerziellen Marketplace live veröffentlicht wird. | 
|||

## <a name="automated-validation-phase"></a>Phase „Automatisierte Validierung“

Der erste Schritt im Veröffentlichungsprozess sieht eine Reihe automatisierter Validierungen vor. Jeder Validierungsschritt entspricht einer Funktion, die Sie bei der Erstellung des Angebots ausgewählt haben. Jede Validierungsprüfung muss abgeschlossen sein, bevor das Angebot zum nächsten Schritt im Veröffentlichungsprozess übergehen kann.

- **Einrichten des Flows zum Kauf eines Angebots (<10 Min.)**

   In diesem Schritt stellen wir sicher, dass Ihr Angebot vom Kunden im Azure-Portal gekauft werden kann. Dieser Schritt gilt nur für Angebote, die über Microsoft verkauft werden.
- **Datenvalidierung mit Testversion (ca. 5 Min.)**

   In diesem Schritt validieren wir die Daten, die Sie auf der Seite „Technische Konfiguration“ des Angebots angegeben haben. Die Funktionalität der Testversion wird überprüft und genehmigt. Dieser Schritt gilt nur für Angebote mit aktivierter Testversion.

-   **Bereitstellung der Testversion (ca. 30 Min.)**

    Nach der Validierung der Daten und Funktionalität Ihrer Testversion im vorherigen Schritt werden wir in diesem Schritt Instanzen Ihrer Testversion so bereitstellen und replizieren, dass sie für Kunden einsatzbereit sind. Dieser Schritt gilt nur für Angebote mit aktivierter Testversion.

-   **Überprüfung der Leadverwaltung und Registrierung (<15 Min.)**

    In diesem Schritt bestätigen wir, dass Ihr Leadverwaltungssystem Kundenleads basierend auf Ihren Angaben auf der Seite **Angebotseinrichtung** empfangen kann. Dieser Schritt gilt nur für Angebote mit aktivierter Leadverwaltung.

## <a name="certification-phase"></a>Phase „Zertifizierung“

Vor der Veröffentlichung müssen Angebote, die für den kommerziellen Marketplace übermittelt werden, zertifiziert werden. Übermittelte Angebote werden strengen Tests unterzogen, die teils automatisch und teils manuell erfolgen. Weitere Informationen finden Sie in den [Zertifizierungsrichtlinien für den kommerziellen Marketplace](https://aka.ms/commercial-marketplace-certification-policies).

### <a name="types-of-validation-that-take-place-during-certification"></a>Typen der während der Zertifizierung erfolgenden Validierung
Für jedes übermittelte Angebot gibt es im Zertifizierungsprozess drei Validierungsstufen.
-   Eignung des Herausgebers
-   Validierung von Inhalten
-   Technische Validierung

#### <a name="publisher-business-eligibility"></a>Eignung des Herausgebers
Bei jeder Angebotsart wird eine Reihe von Standardeignungskriterien geprüft, die der Herausgeber erfüllen muss. Zu den Eignungskriterien können der MPN-Status des Herausgebers, die erworbenen Kompetenzen, die Kompetenzebenen usw. gehören.

#### <a name="content-validation"></a>Validierung von Inhalten

Die bei der Erstellung Ihres Angebots eingegebenen Informationen werden auf Qualität und Relevanz geprüft. Bei diesen Prüfungen werden Ihre Eingaben für die Listungsdetails für Marketplace, Preise, Verfügbarkeit, zugehörige Pläne usw. geprüft. Damit Ihr Angebot die Listungskriterien von Azure Marketplace und Microsoft AppSource erfüllt, wird es auf folgende Voraussetzungen geprüft:
-   Titel, der das Angebot genau beschreibt
-   Verständlich formulierte Beschreibungen mit umfassender Übersicht und einem Wertversprechen
-   Qualitativ hochwertige Screenshots und Videos
-   Erläuterung, wie das Angebot Microsoft-Plattformen und -Tools nutzt

Erfahren Sie im Artikel zu den [allgemeinen Richtlinien für die Listung](https://aka.ms/commercial-marketplace-certification-policies#100-general) mehr zu den Kriterien für die Validierung von Inhalten.

#### <a name="technical-validation"></a>Technische Validierung
Bei der technischen Validierung wird das Angebot (Paket oder Binärdatei) den folgenden Prüfungen unterzogen.
-   Prüfung auf Schadsoftware
-   Überwachung von Netzwerkaufrufen
-   Paketanalyse
-   Umfassende Überprüfung der Funktionalität des Angebots

Das Angebot wird auf verschiedenen Plattformen und mit verschiedenen Versionen getestet, um eine stabile Ausführung sicherzustellen.

### <a name="certification-failure-report"></a>Zertifizierungsfehlerbericht

Wenn Ihr Angebot eine der Listungs-, technischen oder Richtlinienprüfungen nicht besteht oder Sie nicht berechtigt sind, ein solches Angebot zu übermitteln, erhalten Sie per E-Mail einen Bericht mit Zertifizierungsfehlern.

Dieser Bericht enthält Beschreibungen aller Richtlinien, gegen die verstoßen wurde, sowie Prüfhinweise. Überprüfen Sie diesen E-Mail-Bericht, beheben Sie mögliche Probleme, aktualisieren Sie Ihr Angebot bei Bedarf, und reichen Sie es über das [Portal des kommerziellen Marketplace](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) erneut im Partner Center ein. Sie können das Angebot bis zum Bestehen der Zertifizierung beliebig oft erneut einreichen.

## <a name="preview-creation-phase"></a>Phase „Erstellung einer Vorschau“

Während der Phase „Erstellung einer Vorschau“ erstellen wir eine Version Ihres Angebots, die nur für die Zielgruppe zugänglich ist, die Sie ggf. auf der Seite **Vorschauzielgruppe** Ihres Angebots angegeben haben. Personen, die nicht der Vorschauzielgruppe angehören, können erst auf die Vorschauversion Ihres Angebots zugreifen, nachdem Sie das Angebot live veröffentlicht haben.

> [!NOTE]
> Die Vorschauzielgruppe sollte nicht dazu dienen, um Personen außerhalb Ihres Unternehmens Einblick in ein Angebot zu geben. Verwenden Sie stattdessen die Option „Privates Angebot“. An diesem Punkt wurde ihr Angebot noch nicht vollständig getestet und überprüft und ist nicht für die externe Verteilung bereit. 

## <a name="publisher-signoff-phase"></a>Phase „Einholen der Genehmigung des Herausgebers“

Wenn das Angebot für Ihre Prüfung und Genehmigung bereit ist, senden wir Ihnen eine E-Mail mit der Bitte, Ihre Angebotsvorschau zu prüfen und zu genehmigen. Sie können auch die Seite **Angebotsübersicht** in Ihrem Browser aktualisieren, um festzustellen, ob Ihr Angebot die Phase „Einholen der Genehmigung des Herausgebers“ erreicht hat. Wenn dies der Fall ist, sind die Schaltfläche **Live schalten** und Vorschaulinks verfügbar.

Im folgenden Screenshot ist die Seite **Angebotsübersicht** für ein SaaS-Angebot zu sehen. Die auf dieser Seite angezeigten Validierungsschritte variieren je nach Angebotstyp und der Auswahl, die Sie beim Erstellen des Angebots getroffen haben.

![Veranschaulicht die Seite „Angebotsübersicht“ für ein Angebot im Partner Center. Die Schaltfläche „Live schalten“ und Vorschaulinks sind dargestellt.](./partner-center-portal/media/publish-status-publisher-signoff.png)

**So können Sie Ihr Angebot in der Vorschau anzeigen und genehmigen**
1. Wählen Sie auf der Seite **Angebotsübersicht** den Link unter der Schaltfläche **Live schalten** aus, um Ihr Angebot in der Vorschau anzuzeigen.
   > [!NOTE]
   > Abhängig von den Optionen, die Sie beim Erstellen Ihres Angebots ausgewählt haben, ist ein Link für die AppSource-Vorschau, die Azure Marketplace-Vorschau oder beide verfügbar. Wenn Sie sich für den Verkauf Ihres Angebots über Microsoft entschieden haben, können alle Mitglieder der Vorschauzielgruppe den Kauf und die Bereitstellung Ihres Angebots testen, um sicherzustellen, dass es Ihre Anforderungen in dieser Phase erfüllt.

1. Wenn Sie nach der Vorschauphase Änderungen am Angebot vornehmen möchten, können Sie es bearbeiten und erneut übermitteln, um eine neue Vorschau zu veröffentlichen. Weitere Informationen finden Sie unter [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./partner-center-portal/update-existing-offer.md).

1. Nachdem Sie Ihre Vorschau genehmigt haben, wählen Sie **Live schalten** aus, um Ihr Angebot live im kommerziellen Marketplace zu veröffentlichen.
   > [!TIP]
   > Wenn Ihr Angebot bereits live geschaltet und im Marketplace für die Öffentlichkeit verfügbar ist, werden alle von Ihnen vorgenommenen Aktualisierungen erst dann live geschaltet, wenn Sie **Live schalten** auswählen.

## <a name="publish-phase"></a>Phase „Veröffentlichung“

Nachdem Sie sich entschieden haben, Ihr Angebot live zu schalten und im kommerziellen Marketplace verfügbar zu machen, stellen wir mit einer Reihe abschließender Validierungsprüfungen sicher, dass das Liveangebot auf dieselbe Weise wie die Vorschauversion des Angebots konfiguriert ist.

-   **Einrichten des Flows zum Kauf eines Angebots (> 10 Min.)**

    In diesem Schritt stellen wir sicher, dass Ihr Angebot vom Kunden im Azure-Portal gekauft werden kann. Dieser Schritt gilt nur für Angebote, die über Microsoft verkauft werden.
-   **Datenvalidierung mit Testversion (ca. 5 Min.)**

    In diesem Schritt validieren wir die Daten, die Sie auf der Seite „Technische Konfiguration“ des Angebots angegeben haben. Die Funktionalität der Testversion wird überprüft und genehmigt. Dieser Schritt gilt nur für Angebote mit aktivierter Testversion.

-   **Bereitstellung der Testversion (ca. 30 Min.)**

      In diesem Schritt werden wir Instanzen Ihrer Testversion so einsetzen und replizieren, dass sie für den Einsatz durch Kunden bereit sind. Dieser Schritt gilt nur für Angebote mit aktivierter Testversion.
-   **Überprüfung der Leadverwaltung und Registrierung (>15 Min.)**

    In diesem Schritt stellen wir sicher, dass Ihr Leadverwaltungssystem Kundenleads basierend auf den Informationen empfangen kann, die auf der Seite **Angebotseinrichtung** des Angebots angegeben wurden. Dieser Schritt gilt nur für Angebote mit aktivierter Leadverwaltung.

-   **Endgültige Veröffentlichung (>30 Min.)**

    In diesem Schritt stellen wir sicher, dass Ihr Angebot im Marketplace öffentlich zur Verfügung steht.

Nachdem die Validierungsprüfungen abgeschlossen wurden, wird Ihr Angebot im Marketplace live geschaltet.

## <a name="next-step"></a>Nächster Schritt

[Zugreifen auf Analysen für den kommerziellen Marketplace in Partner Center](./partner-center-portal/analytics.md)