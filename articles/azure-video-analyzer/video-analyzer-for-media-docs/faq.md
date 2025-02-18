---
title: 'Häufig gestellte Fragen zum Azure Video Analyzer for Media (früher Video Indexer): Azure'
titleSuffix: Azure Video Analyzer for Media
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Azure Video Analyzer for Media (früher Video Indexer).
services: azure-video-analyzer
author: Juliako
manager: femila
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.date: 05/25/2021
ms.author: juliako
ms.openlocfilehash: 00551d5587bc5bf8afad1b2bd481ebc637bc6dce
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112119625"
---
# <a name="video-analyzer-for-media-frequently-asked-questions"></a>Video Analyzer for Media: Häufig gestellte Fragen

In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zum Azure Video Analyzer for Media (früher Video Indexer).

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="what-is-video-analyzer-for-media"></a>Was ist Video Analyzer for Media?

Video Analyzer for Media ist ein KI-Dienst, der Teil von Microsoft Azure Media Services ist. Video Analyzer for Media bietet eine Orchestrierung von mehreren Machine Learning-Modellen, mit denen Sie problemlos detaillierte Informationen aus einer Videodatei extrahieren können. Video Analyzer for Media nutzt mehrere Kanäle einer Videodatei, um erweiterte und genaue Einblicke zu bieten: Audio, Sprache und visuelle Elemente. Die Erkenntnisse von Video Analyzer for Media können in vielerlei Hinsicht hilfreich sein, etwa für ein leichteres Auffinden und Nutzen von Inhalten, das Aufdecken neuer Vermarktungsmöglichkeiten oder Erstellen neuer Benutzererlebnisse. Video Analyzer for Media bietet eine webbasierte Oberfläche für Tests, Konfiguration und die Anpassung von Modellen in Ihrem Konto. Entwickler können Video Analyzer for Media mithilfe einer REST-basierten API in ein Produktionssystem integrieren. 

### <a name="what-can-i-do-with-video-analyzer-for-media"></a>Welche Möglichkeiten bietet mir Video Analyzer for Media?

Video Analyzer for Media kann u. a. folgende Vorgänge an Mediendateien ausführen:

* Identifizieren und Extrahieren von Sprache und Identifizieren von Sprechern
* Identifizieren und Extrahieren von Text auf dem Bildschirm in Videos
* Erkennen von Objekten in Videodateien
* Identifizieren von Marken (z.B. Microsoft) in Audiospuren und Text auf dem Bildschirm in Videos
* Erfassen und Erkennen von Gesichtern aus einer Prominentendatenbank und einer benutzerdefinierten Datenbank mit Gesichtern
* Extrahieren von Themen, die in Audio- und Videoinhalten behandelt, aber nicht unbedingt erwähnt werden
* Erstellen von Untertiteln aus Audiospuren

Weitere Informationen und weitere Video Analyzer for Media-Funktionen finden Sie in der [Übersicht](video-indexer-overview.md).

### <a name="how-do-i-get-started-with-video-analyzer-for-media"></a>Wie sehen meine ersten Schritte mit Video Analyzer for Media aus?

Mit der kostenlosen Testversion von Video Analyzer for Media stehen Ihnen 600 Minuten in der webbasierten Oberfläche und 2.400 Minuten über die API zur Verfügung. [Melden Sie sich bei der webbasierten Oberfläche von Video Analyzer for Media an](https://www.videoindexer.ai/), und testen Sie den Dienst selbst. Dazu können Sie eine beliebige Webidentität verwenden und müssen kein Azure-Abonnement einrichten. Folgen Sie [diesem einfachen Einführungslab](https://github.com/Azure-Samples/media-services-video-indexer/blob/master/IntroToVideoIndexer.md), um eine bessere Vorstellung von der Verwendung von Video Analyzer for Media zu erhalten.

Wenn Sie Video- und Audiodateien in großem Umfang indizieren möchten, können Sie Video Analyzer for Media mit einem kostenpflichtigen Microsoft Azure-Abonnement verknüpfen. Weitere Preisinformationen finden Sie auf der Seite mit der [Preisübersicht](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/).

Weitere Informationen zu den ersten Schritten finden Sie unter [Erste Schritte](video-indexer-get-started.md).

### <a name="do-i-need-coding-skills-to-use-video-analyzer-for-media"></a>Benötige ich Programmierkenntnisse, um Video Analyzer for Media verwenden zu können?

Über die webbasierte Oberfläche von Video Analyzer for Media können Sie Ihr Konto **ohne Codierung** auswerten, konfigurieren und verwalten.  Wenn Sie komplexere Anwendungen entwickeln möchten, können Sie Video Analyzer for Media mithilfe der [Video Analyzer for Media-API](https://api-portal.videoindexer.ai/) in Ihre eigene Anwendungen, Websites oder [benutzerdefinierten Workflows mit serverlosen Technologien wie Azure Logic Apps](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/) sowie Azure Functions integrieren.

### <a name="do-i-need-machine-learning-skills-to-use-video-analyzer-for-media"></a>Benötige ich Kenntnisse im maschinellen Lernen, um Video Analyzer for Media zu verwenden?

Nein, Video Analyzer for Media ermöglicht die Integration mehrerer Machine Learning-Modelle in eine Pipeline. Bei der Indizierung einer Video- oder Audiodatei über Video Analyzer for Media wird ein vollständiger Satz von Erkenntnissen abgerufen und in eine freigegebene Zeitachse extrahiert. Dazu sind aufseiten des Kunden keine Kenntnisse im maschinellen Lernen oder von Algorithmen erforderlich.

### <a name="what-media-formats-does-video-analyzer-for-media-support"></a>Welche Medienformate werden von Video Analyzer for Media unterstützt?

Video Analyzer for Media unterstützt die gängigsten Medienformate. Einzelheiten hierzu finden Sie in der Liste der [Standardformate von Azure Media Encoder](../../media-services/latest/encode-media-encoder-standard-formats-reference.md).

### <a name="how-do-i-upload-a-media-file-into-video-analyzer-for-media-and-what-are-the-limitations"></a>Wie lade ich eine Mediendatei in Video Analyzer for Media hoch, und welche Einschränkungen bestehen?

Im webbasierten Video Analyzer for Media-Portal können Sie über das Dialogfeld zum Hochladen von Dateien eine Mediendatei hochladen. Alternativ dazu können Sie auch auf eine URL zeigen, welche die Quelldatei direkt hostet (siehe [Beispiel](https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4)). URLs, die Medieninhalte mithilfe eines iFrames oder Einbindungscodes hosten, funktionieren nicht (siehe [Beispiel](https://www.videoindexer.ai/accounts/7e1282e8-083c-46ab-8c20-84cae3dc289d/videos/5cfa29e152/?t=4.11)). 

Weitere Informationen finden Sie in dieser [ausführlichen Anleitung](./upload-index-videos.md).

#### <a name="limitations"></a>Einschränkungen

* Der Name des Videos darf nicht länger als 80 Zeichen sein.
* Wenn Sie ein Video mit einem Bytearray hochladen, ist die Videogröße auf 2 GB beschränkt (30 GB bei Verwendung einer URL). 

Eine umfassende Liste finden Sie unter [Überlegungen und Einschränkungen zum Hochladen](upload-index-videos.md#uploading-considerations-and-limitations).

### <a name="how-long-does-it-take-video-analyzer-for-media-to-extract-insights-from-media"></a>Wie lange dauert das Extrahieren von Informationen aus Mediendateien mit Video Analyzer for Media?

Die Zeit, die Video Analyzer for Media für das Indizieren einer Video- oder Audiodatei benötigt – entweder mithilfe der Video Analyzer for Media-API oder über die webbasierte Video Analyzer for Media-Oberfläche – hängt von mehreren Parametern ab. Dazu zählen die Länge und Qualität der Datei, die Anzahl der in der Datei identifizierten Informationen, die Anzahl der [reservierten Einheiten](../../media-services/previous/media-services-scale-media-processing-overview.md) und die Aktivierung oder Deaktivierung des [Streamingendpunkts](../../media-services/previous/media-services-streaming-endpoints-overview.md). Es wird ein Testlauf mit mehreren Dateien Ihrer eigenen Inhalte empfohlen, um die durchschnittliche Dauer einschätzen zu können.

### <a name="can-i-create-customized-workflows-to-automate-processes-with-video-analyzer-for-media"></a>Kann ich mit Video Analyzer for Media angepasste Workflows zur Automatisierung von Prozessen erstellen?

Ja, Video Analyzer for Media lässt sich in serverlose Technologien wie Logic Apps, Flow und [Azure Functions](https://azure.microsoft.com/services/functions/) integrieren. Weitere Informationen zu den [Logic Apps](https://azure.microsoft.com/services/logic-apps/)- und [Flow](https://flow.microsoft.com/en-us/)-Connectors für Video Analyzer for Media finden Sie [hier](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/). Einige Automatisierungsprojekte von Partnern finden Sie im Repository für [Video Analyzer for Media-Beispiele](https://github.com/Azure-Samples/media-services-video-indexer).

### <a name="in-which-azure-regions-is-video-analyzer-for-media-available"></a>In welchen Azure-Regionen ist Video Analyzer for Media verfügbar?

Eine Übersicht über die Azure-Regionen, in denen Video Analyzer for Media verfügbar ist, finden Sie auf der Seite [Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

### <a name="can-i-customize-video-analyzer-for-media-models-for-my-specific-use-case"></a>Kann ich Video Analyzer for Media-Modelle meinem speziellen Anwendungsfall anpassen? 

Ja. In Video Analyzer for Media können Sie einige der verfügbaren Modelle anpassen, sodass sie Ihre Anforderungen besser erfüllen. 

Unser Modell für Personen beispielsweise unterstützt von vorneherein 1.000.000 Gesichter von Prominenten, Sie können es aber auch trainieren, sodass weitere Gesichter erkannt werden, die sich nicht in dieser Datenbank befinden. 

Details dazu finden Sie in den Artikeln zum Anpassen der Modelle für [Personen](customize-person-model-overview.md), [Marken](customize-brands-model-overview.md) und [Sprache](customize-language-model-overview.md). 

###  <a name="can-i-edit-the-videos-in-my-library"></a>Kann ich die Videos in meiner Bibliothek bearbeiten?

Ja. Klicken Sie in der Bibliotheksanzeige auf die Schaltfläche **Video bearbeiten** oder in der Playeranzeige auf die Schaltfläche **In Editor öffnen**, um zur Registerkarte **Projekte** zu gelangen. Sie können ein neues Projekt erstellen und weitere Videos aus Ihrer Bibliothek hinzufügen, um sie gemeinsam zu bearbeiten. Wenn Sie damit fertig sind, können Sie das Video rendern und herunterladen. 

Wenn Sie Erkenntnisse zu Ihrem neuen Video erhalten möchten, indizieren Sie es mit Video Analyzer for Media. Es wird dann mit den Erkenntnissen in Ihrer Bibliothek angezeigt.

### <a name="can-i-index-multiple-audio-streams-or-channels"></a>Kann ich mehrere Audiodatenströme oder Kanäle indizieren?

Wenn mehrere Audiodatenströme vorhanden sind, greift Video Analyzer for Media den ersten angetroffenen Datenstrom auf und verarbeitet ausschließlich ihn. In jedem Audiodatenstrom, den Video Analyzer for Media verarbeitet, greift er die verschiedenen Kanäle auf (sofern vorhanden) und verarbeitet sie gemeinsam als Monosignal. Zur Bearbeitung von Datenströmen/Kanälen können Sie vor der Indizierung ffmpeg-Befehle auf die Datei anwenden.

### <a name="can-a-storage-account-connected-to-the-media-services-account-be-behind-a-firewall"></a>Kann sich ein Speicherkonto, das mit dem Media Services-Konto verbunden ist, hinter einer Firewall befinden?

Ihr kostenpflichtiges Video Analyzer for Media-Konto verwendet das angegebene Media Services-Konto, das mit einem Speicherkonto verbunden ist. Um ein verbundenes Speicherkonto zu verwenden, das sich hinter einer Firewall befindet, müssen Sie sich derzeit an den Kundensupport von Video Analyzer for Media wenden und die Anweisungen der Berater befolgen. 

Navigieren Sie zu [Supportanfrage](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) (support request), um eine neue Supportanfrage im Azure-Portal zu öffnen.

### <a name="what-is-the-sla-for-video-analyzer-for-media"></a>Welche SLA ist für Video Analyzer for Media gültig?

Die SLA von Azure Media Services gilt für Video Analyzer for Media. Sie finden sie auf der [SLA](https://azure.microsoft.com/support/legal/sla/media-services/v1_2/)-Seite. Die SLA gilt nur für kostenpflichtige Video Analyzer for Media-Konten, nicht für die kostenlose Testversion.

## <a name="privacy-questions"></a>Fragen zum Datenschutz

### <a name="are-video-and-audio-files-indexed-by-video-analyzer-for-media-stored"></a>Werden die von Video Analyzer for Media indizierten Video- und Audiodateien gespeichert?

Ja, sofern Sie die Dateien nicht mithilfe der Video Analyzer for Media-Website oder -API aus Video Analyzer for Media löschen, werden Ihre Video- und Audiodateien gespeichert. Bei der kostenlosen Testversion werden die Video- und Audiodateien, die Sie indizieren, in der Azure-Region „USA, Osten“ gespeichert. Andernfalls werden Ihre Video- und Audiodateien im Speicherkonto für Ihr Azure-Abonnement gespeichert.

### <a name="can-i-delete-my-files-that-are-stored-in-video-analyzer-for-media-portal"></a>Kann ich meine im Video Analyzer für Media-Portal gespeicherten Dateien löschen?

Ja, Sie können Ihre Video- und Audiodateien sowie Metadaten und Erkenntnisse, die von Video Analyzer for Media daraus extrahiert wurden, jederzeit löschen. Nach dem Löschen einer Datei aus Video Analyzer for Media sind die Datei sowie die zugehörigen Metadaten und Erkenntnisse endgültig aus Video Analyzer for Media entfernt. Wenn Sie jedoch eine eigene Sicherungslösung im Azure-Speicher implementiert haben, verbleibt die Datei in Ihrem Azure-Speicher.

### <a name="can-i-control-user-access-to-my-video-analyzer-for-media-account"></a>Kann ich den Benutzerzugriff auf mein Video Analyzer für Media-Konto steuern?

Ja, nur Kontoadministratoren können Personen für ihre jeweiligen Konten ein- und ausladen sowie Bearbeitungsrechte und schreibgeschützten Zugriff zuweisen.

### <a name="who-has-access-to-my-video-and-audio-files-that-have-been-indexed-andor-stored-by-video-analyzer-for-media-and-the-metadata-and-insights-that-were-extracted"></a>Wer hat Zugriff auf meine Video- und Audiodateien, die von Video Analyzer for Media indiziert und/oder gespeichert wurden, sowie auf die extrahierten Metadaten und Erkenntnisse?

Auf Ihre Video- oder Audioinhalte mit der Datenschutzeinstellung „Öffentlich“ kann jeder zugreifen, der über den Link zu Ihren Video- oder Audioinhalten und den zugehörigen Erkenntnissen verfügt. Auf Video- oder Audioinhalte mit der Datenschutzeinstellung „Privat“ können nur Benutzer zugreifen, die dazu eingeladen wurden. Die Datenschutzeinstellung Ihrer Inhalte gilt auch für die von Video Analyzer for Media extrahierten Metadaten und Erkenntnisse. Die Datenschutzeinstellung wird beim Hochladen Ihrer Video- oder Audiodatei festgelegt. Sie können die Datenschutzeinstellung auch nach der Indizierung ändern.

### <a name="what-access-does-microsoft-have-to-my-video-or-audio-files-that-have-been-indexed-andor-stored-by-video-analyzer-for-media-and-the-metadata-and-insights-that-were-extracted"></a>Welchen Zugriff hat Microsoft auf meine Video- und Audiodateien, die von Video Analyzer for Media indiziert und/oder gespeichert wurden, sowie auf die extrahierten Metadaten und Erkenntnisse?

Den [Azure Online Services-Nutzungsbedingungen](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) (OST) entsprechend sind Sie Eigentümer Ihrer Inhalte, und Microsoft greift nur gemäß den OST und der Microsoft-Datenschutzerklärung auf Ihre Inhalte sowie auf die von Video Analyzer for Media daraus extrahierten Metadaten und Erkenntnisse zu.

### <a name="are-the-custom-models-that-i-build-in-my-video-analyzer-for-media-account-available-to-other-accounts"></a>Stehen die benutzerdefinierten Modelle, die ich in meinem Video Analyzer for Media-Konto erstelle, für andere Konten zur Verfügung?

 Nein, die benutzerdefinierten Modelle, die Sie in Ihrem Konto erstellen, stehen keinem anderen Konto zur Verfügung. Video Analyzer for Media ermöglicht Ihnen derzeit die Erstellung benutzerdefinierter [Marken-](customize-brands-model-overview.md), [Sprach-](customize-language-model-overview.md) und [Personen](customize-person-model-overview.md)modelle in Ihrem Konto. Diese Modelle stehen nur in dem Konto zur Verfügung, in dem Sie sie erstellt haben.
    
### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>Bleiben die von Video Indexer indizierten Inhalte in der Azure-Region, in der ich Video Indexer verwende?

Ja, der Inhalt und die gewonnenen Informationen befinden sich weiterhin in der entsprechenden Azure-Region (mit Ausnahme von „Singapur“ und „Brasilien, Süden“). Eine Ausnahme stellt die manuelle Konfiguration, die mehrere Azure-Regionen verwendet, im Azure-Abonnement dar.

Kundendaten in einer Region werden aus BCDR-Gründen in die [gekoppelte Region](../../best-practices-availability-paired-regions.md#azure-regional-pairs) repliziert.

### <a name="what-is-the-privacy-policy-for-video-analyzer-for-media"></a>Welche Datenschutzrichtlinie gilt für Video Analyzer for Media?

Video Analyzer for Media wird von den [Datenschutzbestimmungen von Microsoft](https://privacy.microsoft.com/privacystatement) abgedeckt. Die Datenschutzerklärung erläutert die von Microsoft verarbeiteten personenbezogenen Daten sowie die Art und die Zwecke der Verarbeitung dieser Daten durch Microsoft. Weitere Informationen zum Datenschutz finden Sie im [Microsoft Trust Center](https://www.microsoft.com/trustcenter).

### <a name="what-certifications-does-video-analyzer-for-media-have"></a>Über welche Zertifizierungen verfügt Video Analyzer for Media?

Video Analyzer for Media verfügt derzeit über die SOC-Zertifizierung. Die Zertifizierung von Video Analyzer for Media finden Sie im [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings?product=Azure).

### <a name="what-is-the-difference-between-private-and-public-videos"></a>Was ist der Unterschied zwischen privaten und öffentlichen Videos? 

Beim Hochladen von Videos in Video Analyzer for Media können Sie zwischen zwei Datenschutzeinstellungen wählen: privat oder öffentlich. Öffentliche Videos sind für alle zugänglich, einschließlich anonymer und nicht identifizierter Benutzer. Private Videos sind ausschließlich auf Mitglieder des Kontos beschränkt. 

### <a name="i-tried-to-upload-a-video-as-public-and-it-was-flagged-for-inappropriate-or-offensive-content-what-does-that-mean"></a>Ich habe versucht, ein Video mit der Option „öffentlich“ hochzuladen, aber es wurde als unpassend oder anstößig gekennzeichnet. Was bedeutet das? 

Beim Hochladen eines Videos in Video Analyzer for Media führen Algorithmen und Modelle eine automatische Inhaltsanalyse durch, um sicherzustellen, dass keine unpassenden Inhalte öffentlich präsentiert werden. Wenn ein Video als möglicherweise anstößig eingestuft wird, kann es nicht als „öffentlich“ festgelegt werden. Die Mitglieder des Kontos können jedoch weiterhin als privates Video darauf zugreifen (anzeigen, Erkenntnisse und extrahierte Artefakte herunterladen und andere Vorgänge ausführen, die Kontomitgliedern zur Verfügung stehen).   

Um ein Video für den öffentlichen Zugriff festzulegen, haben Sie folgende Möglichkeiten: 

* Erstellen Sie eine eigene Schnittstellenschicht (z. B. eine App oder eine Website), und verwenden Sie diese für die Interaktion mit dem Video Analyzer for Media-Dienst. Auf diese Weise bleibt das Video in unserem Portal privat, und Ihre Benutzer können über Ihre Schnittstelle damit interagieren. In Ihrer Schnittstelle können Sie beispielsweise weiterhin Erkenntnisse abrufen oder die Anzeige des Videos zulassen. 
* Fordern Sie die Überprüfung des Inhalts durch eine Person an. Unter der Voraussetzung, dass der Inhalt tatsächlich nicht anstößig ist, führt dies zur Aufhebung der Einschränkung. 

    Diese Option können Sie ausprobieren, wenn die Video Analyzer for Media-Website direkt von Ihren Benutzern als Schnittstellenschicht verwendet wird und das Video öffentlich (durch nicht authentifizierte Benutzer) einsehbar ist. 

## <a name="api-questions"></a>Fragen zu APIs

### <a name="what-apis-does-video-analyzer-for-media-offer"></a>Welche APIs bietet Video Analyzer for Media?

Die Video Analyzer for Media-API ermöglicht Indizierung, das Extrahieren von Metadaten, Anlagenverwaltung, Übersetzung, Einbettung, Anpassung von Modellen und vieles mehr. Ausführlichere Informationen zur Verwendung der Video Analyzer for Media-API finden Sie im [Video Analyzer for Media-Entwicklerportal](https://api-portal.videoindexer.ai/).

### <a name="what-client-sdks-does-video-analyzer-for-media-offer"></a>Welche Client-SDKs bietet Video Analyzer for Media?

Derzeit werden keine Client-SDKs angeboten. Das Video Analyzer for Media-Team arbeitet an den SDKs, und ihre baldige Bereitstellung ist geplant.

### <a name="how-do-i-get-started-with-video-analyzer-for-medias-api"></a>Wie sehen meine ersten Schritte mit der Video Analyzer for Media-API aus?

Befolgen Sie das [Tutorial: Erste Schritte mit der Video Analyzer for Media-API](video-indexer-use-apis.md).

### <a name="what-is-the-difference-between-the-video-analyzer-for-media-api-and-the-azure-media-service-v3-api"></a>Was ist der Unterschied zwischen der Video Analyzer for Media-API und der Azure Media Service v3-API?

Derzeit bestehen einige Überlappungen bei den von der Video Analyzer for Media-API und der Azure Media Services v3-API gebotenen Features. Weitere Informationen zum Vergleichen der beiden Dienste finden Sie [hier](compare-video-indexer-with-media-services-presets.md).

### <a name="what-is-an-api-access-token-and-why-do-i-need-it"></a>Was ist ein API-Zugriffstoken und warum benötige ich es?

Die Video Analyzer for Media-API enthält eine Autorisierungs-API und eine Operations-API. Die Autorisierungs-API enthält Aufrufe, über die Sie Zugriffstoken erhalten. Jeder Aufruf der Operations-API sollte einem Zugriffstoken zugeordnet sein und mit dem Autorisierungsbereich des Aufrufs übereinstimmen.

Zugriffstoken sind bei der Verwendung der Video Analyzer for Media-APIs aus Sicherheitsgründen erforderlich. Dadurch wird sichergestellt, dass alle Aufrufe von Ihnen oder zugriffsberechtigten Personen auf Ihr Konto stammen. 

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>Was ist der Unterschied zwischen Zugriffstoken für Konten, Benutzer und Videos?

* Kontoebene: Mit Zugriffstoken auf Kontoebene können Sie Vorgänge auf der Kontoebene oder der Videoebene durchführen. Beispiele dafür sind das Hochladen von Videos, Auflisten aller Videos, Abrufen von Videoinformationen usw.
* Benutzerebene: Mit Zugriffstoken auf Benutzerebene können Sie Vorgänge auf der Benutzerebene durchführen. Ein Beispiel hierfür ist das Abrufen von zugeordneten Konten.
* Videoebene: Mit Zugriffstoken auf Videoebene können Sie Vorgänge für ein bestimmtes Video durchführen. Beispiele hierfür sind das Abrufen von Videoinformationen, Herunterladen von Untertiteln, Abrufen von Widgets usw.

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>Wie oft muss das Zugriffstoken erneuert werden? Nach welcher Zeit laufen Zugriffstoken ab?

Zugriffstoken laufen nach einer Stunde ab. Daher müssen Sie einmal pro Stunde ein neues Zugriffstoken generieren. 

### <a name="what-are-the-login-options-to-video-analyzer-for-media-developer-portal"></a>Welche Anmeldeoptionen bestehen für das Entwicklerportal für Video Analyzer for Media?

Informationen zu [Anmeldeinformationen](release-notes.md#october-2020) finden Sie in den Versionshinweisen.

Nachdem Sie Ihr E-Mail-Konto mithilfe eines Identitätsanbieters registriert haben, können Sie das betreffende E-Mail-Konto nicht mehr mit einem anderen Identitätsanbieter verwenden.

## <a name="billing-questions"></a>Fragen zur Abrechnung

### <a name="how-much-does-video-analyzer-for-media-cost"></a>Was kostet Video Analyzer for Media?

Für Video Analyzer for Media gilt ein einfaches nutzungsbasiertes Bezahlungsmodell, das auf der Dauer des eingegebenen Inhalts beruht, den Sie indizieren. Zusätzliche Gebühren können für Codierung, Streaming, Speicher, Netzwerknutzung und reservierte Einheiten für Medien anfallen. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/).

### <a name="when-am-i-billed-for-using-video-analyzer-for-media"></a>Wann wird mir die Verwendung von Video Analyzer for Media in Rechnung gestellt?

Beim Senden eines Videos, das indiziert werden soll, definiert der Benutzer die gewünschte Indizierung als Videoanalyse, als Audioanalyse oder beides. Dadurch wird bestimmt, welche SKUs in Rechnung gestellt werden. Wenn während der Verarbeitung ein schwerwiegender Fehler auftritt, wird als Antwort darauf ein Fehlercode zurückgegeben. In solch einem Fall erfolgt keine Abrechnung.  Ein schwerwiegender Fehler kann durch einen Fehler in unserem Code oder durch einen kritischen Fehler in einer internen Abhängigkeit des Diensts verursacht werden. Fehler wie falsche Identifizierung oder Erkenntnisextraktion gelten nicht als schwerwiegend, und es wird eine Antwort zurückgegeben. In jedem Fall, in dem eine gültige Antwort (ohne Fehlercode) zurückgegeben wird, erfolgt eine Abrechnung.
 
### <a name="does-video-analyzer-for-media-offer-a-free-trial"></a>Gibt es eine kostenlose Testversion von Video Analyzer for Media?

Ja, eine kostenlose Testversion von Video Analyzer for Media mit vollständigem Funktionsumfang sowie voller API-Funktionalität ist erhältlich. Das Kontingent für Benutzer der webbasierten Oberfläche beträgt 600 Minuten, das für API-Benutzer beträgt 2.400 Minuten. 

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht](video-indexer-overview.md)
* [Stack Overflow](https://stackoverflow.com/search?q=video-indexer)
