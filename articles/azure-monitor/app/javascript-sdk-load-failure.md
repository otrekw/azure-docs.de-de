---
title: 'Beheben von Fehlern beim Laden des SDK für JavaScript-Webanwendungen: Azure Application Insights'
description: Beheben von Fehlern beim Laden des SDK für JavaScript-Webanwendungen
ms.topic: conceptual
author: MSNev
ms.author: newylie
ms.date: 06/05/2020
ms.custom: devx-track-js
ms.openlocfilehash: 6295a56abbf3466c68b968c935936dbc10e22fb5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101711415"
---
# <a name="troubleshooting-sdk-load-failure-for-javascript-web-apps"></a>Beheben von Fehlern beim Laden des SDK für JavaScript-Web-Apps

<!-- 
Editor Note: This link name above "SDK Load Failure" has a direct references by https://go.microsoft.com/fwlink/?linkid=2128109 which is embedded in the snippet and from the JavaScript Page. If you change this text you MUST also update these references.
-->

Die Ausnahme beim Fehlschlagen des Ladens des SDK wird vom JavaScript-Ausschnitt (v3 oder höher) erstellt und gemeldet, wenn festgestellt wird, dass das SDK-Skript nicht heruntergeladen oder initialisiert werden konnte. Vereinfacht ausgedrückt, konnte der Client (Browser) Ihres Endbenutzers das Application Insights SDK nicht herunterladen oder nicht über die identifizierte Hostingseite initialisieren, weshalb keine Telemetrie oder Ereignisse gemeldet werden.

![Übersicht über Browserfehler im Azure-Portal](./media/javascript-sdk-load-failure/overview.png)

> [!NOTE]
> Diese Ausnahme wird von allen gängigen Browsern unterstützt, die die fetch()-API oder XMLHttpRequest unterstützen. Dies schließt Internet Explorer 8 und niedriger aus, sodass Sie diese Browser diese Art von Ausnahmen nicht melden können (es sei denn, Ihre Umgebung enthält ein „fetch“-Polyfill).

![Details zur Ausnahme im Browser](./media/javascript-sdk-load-failure/exception.png)

Die Stapelangaben enthalten die grundlegenden Informationen, einschließlich der vom Endbenutzer verwendeten URLs.

| Name                      | BESCHREIBUNG                                                                                                  |
|---------------------------|--------------------------------------------------------------------------------------------------------------|
| &lt;CDN&nbsp;Endpoint&gt; | Die URL, die zum (fehlgeschlagenen) Herunterladen des SDK verwendet wurde.                                                      |
| &lt;Help&nbsp;Link&gt;    | Eine URL, die auf Dokumentation zur Problembehandlung verweist (diese Seite).                                              |
| &lt;Host&nbsp;URL&gt;     | Die vollständige URL der Seite, die der Endbenutzer verwendet hat.                                                    |
| &lt;Endpoint&nbsp;URL&gt; | Die URL, die zum Melden der Ausnahme verwendet wurde. Dieser Wert kann hilfreich sein, um festzustellen, ob auf die Hostingseite über das öffentliche Internet oder eine private Cloud zugegriffen wurde.

Es folgen die häufigsten Gründe für diese Ausnahme:

- Zeitweiliger Verlust der Netzwerkverbindung
- Ausfall des Application Insights CDN
- Keine Initialisierung des SDK nach Laden des Skripts
- Blockierung des JavaScript CDN von Application Insights

[Zeitweiliger Verlust der Netzwerkverbindung](#intermittent-network-connectivity-failure) ist der häufigste Grund für diese Ausnahme, vor allem in mobilen Roamingszenarien, in denen die Benutzer die Netzwerkverbindung zeitweilig verlieren.

In den folgenden Abschnitten wird beschrieben, wie Sie die einzelnen potenziellen Grundursachen dieses Fehlers beseitigen können.

> [!NOTE]
> Mehrere der Problembehandlungsschritte setzen voraus, dass Ihre Anwendung direkte Kontrolle über das Tag &lt;script /&gt; im Ausschnitt und dessen Konfiguration hat, die als Teil der hostenden HTML-Seite zurückgegeben werden. Wenn dies nicht der Fall ist, gelten die genannten Schritte für Ihr Szenario nicht.

## <a name="intermittent-network-connectivity-failure"></a>Zeitweiliger Verlust der Netzwerkverbindung

**Wenn es beim Benutzer zu zeitweiligen Ausfällen der Netzwerkverbindung kommt, gibt es nur wenige Lösungsmöglichkeiten. Diese Probleme beheben sich in der Regel innerhalb kurzer Zeit von selbst.** Wenn der Benutzer beispielsweise Ihre Website neu lädt (die Seite aktualisiert), werden die Dateien (schließlich) heruntergeladen und lokal zwischengespeichert, bis eine aktualisierte Version veröffentlicht wird.

> [!NOTE]
> Wenn diese Ausnahme hartnäckig ist und bei vielen Ihrer Benutzer auftritt, was anhand einer raschen und kontinuierlichen Meldung dieser Ausnahme erkannt wird, und gleichzeitig ein Rückgang der normalen Clienttelemetrie zu verzeichnen ist, dürfte es _unwahrscheinlich_ sein, dass zeitweilige Probleme mit der Netzwerkverbindung die wahre Ursache des Problems sind. Sie sollten daher die Diagnose mit den anderen bekannten möglichen Problemen fortsetzen.

In dieser Situation ist es unwahrscheinlich, dass das Hosten des SDK in Ihrem eigenen CDN das Auftreten dieser Ausnahme bewirkt oder verringert, da Ihr eigenes CDN vom selben Problem betroffen ist.

Dasselbe gilt auch für die Verwendung des SDK mit der auf NPM-Paketen basierenden Lösung. Aus Endbenutzersicht kann in diesem Fall jedoch Ihre gesamte Anwendung (statt _nur_ das Telemetrie SDK, das sie nicht sehen) nicht geladen/initialisiert werden, sodass die Endbenutzer höchstwahrscheinlich Ihre Website aktualisieren werden, bis sie vollständig geladen ist.

Sie können auch versuchen, das Application Insights SDK mithilfe von [NPM-Paketen](#use-npm-packages-to-embed-the-application-insight-sdk) einzubetten.

Um zeitweilige Ausfälle der Netzwerkverbindung zu minimieren, haben wir Cache-Control-Header für sämtliche CDN-Dateien implementiert. Sobald der Browser des Endbenutzers die aktuelle Version des SDK heruntergeladen hat, muss er es nicht erneut herunterladen, denn der Browser verwendet die zuvor abgerufene Kopie wieder (weitere Informationen finden Sie unter [Funktionsweise der Zwischenspeicherung](../../cdn/cdn-how-caching-works.md)). Wenn die Zwischenspeicherprüfung fehlschlägt oder es eine neue Version gibt, muss der Browser Ihres Endbenutzers die aktualisierte Version herunterladen. So kann es sein, dass Sie einen Hintergrundpegel von _Stördatenverkehr_ im Szenario der Ausfallprüfung oder eine vorübergehende Spitze erleben, wenn eine neue Version vorliegt und allgemein verfügbar gemacht (im CDN bereitgestellt) wird.
 
## <a name="application-insights-cdn-outage"></a>Ausfall des Application Insights CDN

Ob ein Application Insights CDN-Ausfall vorliegt, können Sie folgendermaßen überprüfen: Versuchen Sie direkt im Browser (z. B. https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js oder https://js.monitor.azure.com/scripts/b/ai.2.min.js) ) an einem anderen Standort als Ihre Endbenutzer, z. B. auf Ihrem eigenen Entwicklungscomputer, auf den CDN-Endpunkt zuzugreifen (vorausgesetzt, Ihre Organisation hat diese Domäne nicht gesperrt).

Wenn Sie bestätigen, dass ein Ausfall vorliegt, können Sie [ein neues Supportticket eröffnen](https://azure.microsoft.com/support/create-ticket/) oder versuchen, die zum Herunterladen des SDK verwendete URL zu ändern.

### <a name="change-the-cdn-endpoint"></a>Ändern des CDN-Endpunkts
  
Da der Ausschnitt und seine Konfiguration von Ihrer Anwendung als Teil jeder generierten Seite zurückgegeben werden, können Sie die Konfiguration von `src` im Ausschnitt ändern, um eine andere URL für das SDK zu verwenden. Mit diesem Ansatz könnten Sie das Problem der CDN-Blockierung umgehen, da die neue URL nicht blockiert werden sollte.

Aktuelle CDN-Endpunkte für das JavaScript SDK für Application Insights
- `https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js`
- `https://js.monitor.azure.com/scripts/b/ai.2.min.js`

> [!NOTE]
> Der Endpunkt `https://js.monitor.azure.com/` ist ein Alias, der es uns ermöglicht, innerhalb von etwa 5 Minuten zwischen CDN-Anbietern zu wechseln, ohne dass Sie die Konfiguration ändern müssen. Dadurch können wir festgestellte Probleme im Zusammenhang mit dem CDN schneller beheben, wenn ein CDN-Anbieter regionale oder globale Probleme hat, ohne dass alle Beteiligten ihre Einstellungen anpassen müssen.

## <a name="sdk-failed-to-initialize-after-loading-the-script"></a>Keine Initialisierung des SDK nach Laden des Skripts

Wenn das SDK nicht initialisiert werden kann, wurde &lt;script /&gt; zwar erfolgreich aus dem CDN heruntergeladen, schlägt aber während der Initialisierung fehl. Ursache dafür können fehlende oder ungültige Abhängigkeiten oder eine bestimmte Form einer JavaScript-Ausnahme sein.

Zunächst ist zu prüfen, ob das SDK erfolgreich heruntergeladen wurde. Wenn das Skript NICHT heruntergeladen wurde, ist dieses Szenario __nicht__ die Ursache für die Ausnahme aufgrund des fehlgeschlagenen Ladens des SDK.

Schnellprüfung: Überprüfen Sie mit einem Browser, der Entwicklertools (F12) unterstützt, auf der Registerkarte „Netzwerk“, dass das in der Konfiguration des Ausschnitts ```src``` definierte Skript mit dem Antwortcode 200 (erfolgreich) oder 304 (nicht geändert) heruntergeladen wurde. Sie könnten auch ein Tool wie Fiddler einsetzen, um den Netzwerkdatenverkehr zu überprüfen.

In den folgenden Abschnitten finden Sie verschiedene Möglichkeiten zur Berichterstellung. Es wird empfohlen, entweder ein Supportticket zu eröffnen oder auf GitHub ein Problem zu melden.

 Grundlegende Berichterstellungsregeln:

- Wenn das Problem nur eine kleine Anzahl von Benutzern und eine bestimmte oder Teilmenge von Browserversionen betrifft (prüfen Sie die Details der gemeldeten Ausnahme), handelt es sich wahrscheinlich um ein Problem, das nur den Endbenutzer oder nur die Umgebung betrifft, sodass Ihre Anwendung wahrscheinlich zusätzliche Implementierungen von `polyfill` bereitstellen muss. [Melden Sie hierfür ein Problem auf GitHub](https://github.com/Microsoft/ApplicationInsights-JS/issues).
- Wenn dieses Problem Ihre gesamte Anwendung betrifft und alle Ihre Benutzer davon betroffen sind, handelt es sich wahrscheinlich um ein versionsbezogenes Problem. Daher sollten Sie [ein neues Supportticket eröffnen](https://azure.microsoft.com/support/create-ticket/).

### <a name="javascript-exceptions"></a>JavaScript-Ausnahmen

Lassen Sie zunächst mit einem Browser, der Entwicklertools (F12) unterstützt, die Seite auf JavaScript-Ausnahmen prüfen, indem Sie die Seite laden und überprüfen, ob Ausnahmen aufgetreten sind.

Wenn im SDK-Skript Ausnahmen gemeldet werden (z. B. ai.2.min.js), kann dies ein Hinweis darauf sein, dass die an das SDK übergebene Konfiguration eine unerwartete oder fehlende erforderliche Konfiguration aufweist oder eine fehlerhafte Version im CDN bereitgestellt wurde.

Um auf fehlerhafte Konfiguration zu prüfen, ändern Sie die in den Ausschnitt übergebene Konfiguration (falls noch nicht geschehen) so, dass sie nur Ihren Instrumentierungsschlüssel als Zeichenfolgenwert enthält.

> src: "https://js.monitor.azure.com/scripts/b/ai.2.min.js ",<br />
> cfg:{<br />
> instrumentationKey: "INSTRUMENTATION_KEY"<br />
> }});<br />

Wenn bei dieser Minimalkonfiguration weiterhin eine JavaScript-Ausnahme im SDK-Skript auftritt, [eröffnen Sie ein neues Supportticket](https://azure.microsoft.com/support/create-ticket/). Dies erfordert ein Zurücksetzen des fehlerhaften Builds, da es sich wahrscheinlich um ein Problem mit einer neu bereitgestellten Version handelt.

Wenn die Ausnahme verschwindet, ist das Problem wahrscheinlich auf eine Diskrepanz zwischen Typen oder einen unerwarteten Wert zurückzuführen. Beginnen Sie, Ihre Konfigurationsoptionen eine nach der anderen wieder hinzuzufügen, und testen Sie solange, bis die Ausnahme erneut auftritt. Prüfen Sie dann die Dokumentation auf das Element, welches das Problem verursacht. Wenn die Dokumentation unklar ist oder Sie Unterstützung benötigen, [melden Sie ein Problem auf GitHub](https://github.com/Microsoft/ApplicationInsights-JS/issues).

Wenn Ihre Konfiguration bereits bereitgestellt wurde und funktioniert hat, aber gerade erst begonnen hat, diese Ausnahme zu melden, könnte es sich um Probleme mit einer neu bereitgestellten Version handeln. Prüfen Sie, ob nur ein kleiner Teil Ihrer Benutzer/Browser davon betroffen ist, und [melden Sie entweder ein Problem auf GitHub](https://github.com/Microsoft/ApplicationInsights-JS/issues), oder [eröffnen Sie ein neues Supportticket](https://azure.microsoft.com/support/create-ticket/).

### <a name="enable-console-debugging"></a>Aktivieren des Debuggens in der Konsole

Wenn keine Ausnahmen ausgelöst werden, ist der nächste Schritt, das Aktivieren des Debuggens in der Konsole durch Hinzufügen der Einstellung `loggingLevelConsole` zur Konfiguration. Dadurch werden alle Initialisierungsfehler und Warnungen an die Browserkonsole übermittelt (normalerweise über die Entwicklertools [F12] verfügbar). Alle gemeldeten Fehler sollten selbsterklärend sein. Wenn Sie weitere Hilfe benötigen, [melden Sie ein Problem auf GitHub](https://github.com/Microsoft/ApplicationInsights-JS/issues).

> cfg:{<br />
> instrumentationKey: "INSTRUMENTATION_KEY",<br />
> loggingLevelConsole: 2<br />
> }});<br />

> [!NOTE]
> Während der Initialisierung führt das SDK einige grundlegende Prüfungen auf bekannte Hauptabhängigkeiten durch. Wenn diese von der aktuellen Runtime nicht bereitgestellt werden, werden die Ausfälle als Warnmeldungen in der Konsole gemeldet, aber nur, wenn `loggingLevelConsole` größer als 0 ist.

Wenn die Initialisierung weiter fehlschlägt, versuchen Sie, die Konfigurationseinstellung ```enableDebug``` zu aktivieren. Dies führt dazu, dass bei allen internen Fehlern eine Ausnahme ausgelöst wird (wodurch Telemetrie verloren geht). Da es sich hierbei um eine reine Entwicklereinstellung handelt, wird es wahrscheinlich unübersichtlich werden, weil im Rahmen einiger interner Prüfungen Ausnahmen ausgelöst werden. Daher müssen Sie jede Ausnahme überprüfen, um festzustellen, welches Problem zum Fehlschlagen des SDK führt. Verwenden Sie nicht die minierte Version des Skripts (beachten Sie, dass die Erweiterung unten „.js“ und nicht „.min.js“ lautet), da sonst die Ausnahmen unlesbar sind.

> [!WARNING]
> Dies ist eine reine Entwicklereinstellung, die NIEMALS in einer echten Produktionsumgebung aktiviert werden darf, da Sie sonst Telemetrie verlieren.

> src: "https://js.monitor.azure.com/scripts/b/ai.2.min.js ",<br />
> cfg:{<br />
> instrumentationKey: "INSTRUMENTATION_KEY",<br />
> enableDebug: true<br />
> }});<br />

Wenn dies weiterhin keine Erkenntnisse liefert, melden Sie[ein Problem auf GitHub](https://github.com/Microsoft/ApplicationInsights-JS/issues) mit den Details und einer Beispielwebsite, sofern vorhanden. Geben Sie die Browserversion, das Betriebssystem und Details zum JS-Framework an, um das Problem zu ermitteln.

## <a name="the-application-insights-javascript-cdn-has-been-blocked"></a>Blockierung des JavaScript CDN von Application Insights

Eine Blockierung des CDN ist möglich, wenn ein CDN-Endpunkt des JavaScript SDK von Application Insights als unsicher gemeldet und/oder identifiziert wurde. In diesem Fall wird der Endpunkt in einer öffentlichen Sperrliste eingetragen, und die Nutzer dieser Listen beginnen, jeglichen Zugriff zu unterbinden.

Zur Behebung des Problems muss der Besitzer des CDN-Endpunkts mit der für die Sperrliste zuständigen Stelle zusammenarbeiten, die den Endpunkt als unsicher markiert hat, damit er von der entsprechenden Liste entfernt werden kann.

Prüfen Sie, ob der CDN-Endpunkt als unsicher identifiziert wurde.
- [Google Transparenzbericht](https://transparencyreport.google.com/safe-browsing/search)
- [VirusTotal](https://www.virustotal.com/gui/home/url)
- [Sucuri SiteCheck](https://sitecheck.sucuri.net/)

Je nach Häufigkeit, mit der die Anwendung, Firewall oder Umgebung ihre lokalen Kopien dieser Listen aktualisiert, kann es beträchtliche Zeit in Anspruch nehmen und/oder ein manuelles Eingreifen von Endbenutzern oder IT-Abteilungen des Unternehmens erfordern, um eine Aktualisierung zu erzwingen oder den CDN-Endpunkten ausdrücklich zu gestatten, das Problem zu lösen.

Wenn der CDN-Endpunkt als unsicher identifiziert wird, [eröffnen Sie ein Supportticket](https://azure.microsoft.com/support/create-ticket/), um sicherzustellen, dass das Problem so schnell wie möglich behoben wird.

Um dieses Problem *möglicherweise* schneller zu umgehen, können Sie [den CDN-Endpunkt des SDK](#change-the-cdn-endpoint) ändern.

### <a name="application-insights-javascript-cdn-is-blocked-by-end-user---blocked-by-browser-installed-blocker-personal-firewall"></a>CDN des JavaScript SDK für Application Insights ist blockiert (durch Endbenutzer: durch Browser blockiert; installierter Blocker; persönliche Firewall)

Prüfen Sie, ob Ihre Endbenutzer folgende Aufgaben ausgeführt haben:

- Ein Browser-Plug-In installiert (typischerweise eine Form von Werbe-, Schadsoftware- oder Popupblocker).
- Die CDN-Endpunkte von Application Insights in Ihrem Browser oder Proxy als blockiert (oder nicht zulässig) konfiguriert.
- Eine Firewallregel konfiguriert, die bewirkt, dass die CDN-Domäne für das SDK blockiert wird (oder der DNS-Eintrag nicht aufgelöst wird).

Wenn sie eine dieser Optionen konfiguriert haben, müssen Sie mit ihnen zusammenarbeiten (oder Dokumentation bereitstellen), um die CDN-Endpunkte zuzulassen.

Möglicherweise verwendet das von ihnen installierte Plug-In die öffentliche Sperrliste. Wenn das nicht der Fall ist, handelt es sich höchstwahrscheinlich um eine andere manuell konfigurierte Lösung oder um die Verwendung einer privaten Sperrliste für Domänen.

#### <a name="add-exceptions-for-cdn-endpoints"></a>Hinzufügen von Ausnahmen für CDN-Endpunkte

Arbeiten Sie mit Ihren Endbenutzern zusammen, oder stellen Sie Dokumentation zur Verfügung, die sie darüber informiert, dass sie das Herunterladen von Skripts von CDN-Endpunkten von Application Insights erlauben sollen, indem sie diese in die Ausnahmeliste der Plug-Ins oder Firewallregeln ihres Browsers aufnehmen (was von der Umgebung des Endbenutzers abhängt).

Hier ist ein Beispiel dafür, wie Sie [Chrome so konfigurieren können, dass der Zugriff auf Websites zugelassen bzw. blockiert wird.](https://support.google.com/chrome/a/answer/7532419?hl=en)

### <a name="application-insights-cdn-is-blocked-by-corporate-firewall"></a>Das Application Insights CDN wird blockiert (von der Unternehmensfirewall)

Wenn sich Ihre Benutzer in einem Unternehmensnetzwerk befinden, liegt es höchstwahrscheinlich an ihrer Firewalllösung und daran, dass ihre IT-Abteilung eine Art Internetfiltersystem implementiert hat. In diesem Fall müssen Sie mit den Verantwortlichen zusammenarbeiten, um die für Ihre Benutzer notwendigen Regeln zuzulassen.

#### <a name="add-exceptions-for-cdn-endpoints-for-corporations"></a>Hinzufügen von Ausnahmen für CDN-Endpunkte für Unternehmen

  Dies ist vergleichbar mit dem [Hinzufügen von Ausnahmen für Endbenutzer](#add-exceptions-for-cdn-endpoints). Sie müssen jedoch mit der IT-Abteilung des Unternehmens zusammenarbeiten, damit diese die herunterzuladenden CDN-Endpunkte von Application Insights so konfiguriert, dass sie in alle Dienste für Zulassungs- und Sperrlisten für Domänen aufgenommen (oder daraus entfernt) werden.

  > [!WARNING]
  > Wenn Ihr Unternehmensbenutzer eine [private Cloud](https://azure.microsoft.com/overview/what-is-a-private-cloud/) nutzt und er keine Form der Ausnahme aktivieren kann, um seinen internen Benutzern öffentlichen Zugriff auf die CDN-Endpunkte zu gewähren, müssen Sie die [NPM-Pakete für Application Insights](https://www.npmjs.com/package/applicationinsights) verwenden oder das Application Insights SDK in Ihrem eigenen CDN hosten.  

### <a name="additional-troubleshooting-for-blocked-cdn"></a>Zusätzliche Problembehandlung für blockiertes CDN

> [!NOTE]
> Wenn Ihre Benutzer eine [private Cloud](https://azure.microsoft.com/overview/what-is-a-private-cloud/) nutzen und keinen Zugriff auf das öffentliche Internet haben, müssen Sie das SDK in Ihrem eigenen CDN hosten oder NPM-Pakete verwenden.

#### <a name="host-the-sdk-on-your-own-cdn"></a>Hosten des SDK in Ihrem eigenen CDN

 Anstatt dass Ihre Benutzer das Application Insights SDK aus dem öffentlichen CDN herunterladen, können Sie das Application Insights SDK über Ihren eigenen CDN-Endpunkt hosten. Es wird empfohlen, eine bestimmte Version (ai.2.#.#.min.js) zu verwenden, damit Sie die von Ihnen verwendete Version leichter erkennen können. Aktualisieren Sie sie außerdem regelmäßig auf die aktuelle Version (ai.2.min.js), damit Sie alle Fehlerkorrekturen und neuen Features, die verfügbar werden, nutzen können.

#### <a name="use-npm-packages-to-embed-the-application-insight-sdk"></a>Einbetten des Application Insight SDK mithilfe von NPM-Paketen

Anstatt den Ausschnitt und die öffentlichen CDN-Endpunkte zu verwenden, können Sie die [NPM-Pakete](https://www.npmjs.com/package/applicationinsights) einsetzen, um das SDK in Ihre eigenen JavaScript-Dateien einzubinden. Das SDK wird dadurch lediglich ein weiteres Paket innerhalb Ihrer eigenen Skripts.

> [!NOTE]
> Es wird empfohlen, bei Verwendung von NPM-Paketen auch eine Form von [JavaScript-Bundler](https://www.bing.com/search?q=javascript+bundler) einzusetzen, um die Codeaufteilung und Minifizierung zu unterstützen.

Wie beim Ausschnitt ist es auch hier möglich, dass Ihre eigenen Skripts (mit oder ohne Verwendung der NPM-Pakete des SDK) von den gleichen hier aufgeführten Blockierungsproblemen betroffen sein können. Abhängig von Ihrer Anwendung, Ihren Benutzern und Ihrem Framework sollten Sie also in Erwägung ziehen, etwas Ähnliches wie die Logik im Ausschnitt zu implementieren, um diese Probleme zu erkennen und zu melden.


## <a name="next-steps"></a>Nächste Schritte 
- [Zusätzliche Hilfe beim Melden eines Problem auf GitHub](https://github.com/Microsoft/ApplicationInsights-JS/issues)
- [Überwachen der Nutzung von Webseiten](javascript.md)
