---
title: Neuerungen in der Formularerkennung
titleSuffix: Azure Cognitive Services
description: Informieren Sie sich über die neuesten Änderungen an der Formularerkennungs-API.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: pafarley
ms.openlocfilehash: 16bdb0d8ed421763895e61a6e1173fc7c5a48d4d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91297945"
---
# <a name="whats-new-in-form-recognizer"></a>Neuerungen in der Formularerkennung

Der Formularerkennungsdienst wird fortlaufend aktualisiert. In diesem Artikel finden Sie aktuelle Informationen zu Featureverbesserungen, Fixes und Dokumentationsaktualisierungen.

## <a name="august-2020"></a>August 2020

### <a name="new-features"></a>Neue Funktionen

**Die öffentliche Vorschau der Formularerkennung v2.1 ist jetzt verfügbar.** V2.1-preview.1 wurde veröffentlicht, einschließlich der folgenden Features: 


- **REST-API-Referenz ist verfügbar**: [v2.1-preview.1-Referenz](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync) anzeigen 
- **Zusätzlich zu Englisch werden neue Sprachen unterstützt.** Die folgenden [Sprachen](language-support.md) werden jetzt unterstützt: für `Layout` und `Train Custom Model`: Englisch (`en`), Chinesisch (vereinfacht) (`zh-Hans`), Niederländisch (`nl`), Französisch (`fr`), Deutsch (`de`), Italienisch (`it`), Portugiesisch (`pt`) und Spanisch (`es`).
- **Kontrollkästchen-/Auswahlmarkierungserkennung**: Die Formularerkennung unterstützt das Erkennen und Extrahieren von Auswahlmarkierungen, z. B. von Kontrollkästchen und Optionsfeldern. Auswahlmarkierungen werden in `Layout` extrahiert, und Sie können jetzt auch `Train Custom Model` - _mit Bezeichnungen trainieren_, um Schlüssel-Wert-Paare für Auswahlmarkierungen zu extrahieren. 
- **Model Compose** ermöglicht das Zusammensetzen und Aufrufen mehrerer Modelle mit einer einzigen Modell-ID. Wenn ein Dokument zur Analyse mit einer zusammengesetzten Modell-ID übermittelt wird, wird zunächst ein Klassifizierungsschritt durchgeführt, um es an das richtige benutzerdefinierte Modell weiterzuleiten. Model Compose ist für das `Train Custom Model` - _Trainieren mit Bezeichnungen_ verfügbar.
- **Modellname**: Fügen Sie den benutzerdefinierten Modellen einen Anzeigenamen zur einfacheren Verwaltung und Nachverfolgung hinzu.
- **[Neues vordefiniertes Modell für Visitenkarten](concept-business-cards.md)** zum Extrahieren allgemeiner Felder auf Visitenkarten in englischer Sprache.
- **[Neue Gebietsschemas für vordefinierte Belege](concept-receipts.md)** : Neben en-US ist jetzt zusätzliche Unterstützung für en-AU, en-CA, en-GB und en-IN verfügbar.
- **Qualitätsverbesserungen** für `Layout`, `Train Custom Model` - _Trainieren ohne Bezeichnungen_ und _Trainieren mit Bezeichnungen_.


**v2.0** enthält das folgende Update:

- Die [Clientbibliotheken](quickstarts/client-library.md) für .NET, Python, Java und JavaScript sind nun allgemein verfügbar. 


**Neue Beispiele** sind auf GitHub verfügbar. 
- Unter [Knowledge Extraction Recipes - Forms Playbook](https://github.com/microsoft/knowledge-extraction-recipes-forms) werden bewährte Methoden aus echten Kundenengagements mit der Formularerkennung gesammelt und nützliche Codebeispiele, Checklisten und Beispielpipelines für die Entwicklung dieser Projekte bereitgestellt. 
- Das [Samplebezeichnungstool](https://github.com/microsoft/OCR-Form-Tools) wurde aktualisiert, um die neue Funktionalität von v2.1 zu unterstützen. Informationen zu den ersten Schritten mit dem Tool finden Sie in diesem [Schnellstart](quickstarts/label-tool.md). 
- Im Formularerkennungsbeispiel [Intelligent Kiosk](https://github.com/microsoft/Cognitive-Samples-IntelligentKiosk/blob/master/Documentation/FormRecognizer.md) wird gezeigt, wie `Analyze Receipt` und `Train Custom Model` - _Trainieren ohne Bezeichnungen_ integriert werden.



## <a name="july-2020"></a>Juli 2020

### <a name="new-features"></a>Neue Funktionen

* **v2.0-Referenz verfügbar** Mehr dazu finden Sie in der [v2.0 API-Referenz](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm) und den aktualisierten SDKs für [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/ai.formrecognizer-readme-pre?view=azure-dotnet), [Python](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python), [Java](https://docs.microsoft.com/java/api/overview/azure/ai-formrecognizer-readme-pre?view=azure-java-preview) und [JavaScript](https://docs.microsoft.com/javascript/api/overview/azure/?view=azure-node-latest).
* **Tabellenerweiterungen und Extraktionserweiterungen** umfassen Verbesserungen der Genauigkeit und der Tabellenextraktion, insbesondere eine Funktion zum Erlernen von Tabellenheadern und Strukturen beim _benutzerdefinierten Trainieren ohne Bezeichnungen_. 

* **Währungsunterstützung**: Erkennung und Extraktion von globalen Währungssymbolen.
* **Azure Gov**: Die Formularerkennung ist jetzt auch in Azure Gov verfügbar.
* **Erweiterte Sicherheitsfeatures**: 
   * **Bring Your Own Key**: Die Formularerkennung verschlüsselt Ihre Daten automatisch, wenn sie in der Cloud persistent gespeichert werden, um sie zu schützen und Sie bei der Einhaltung der Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu unterstützen. Standardmäßig verwendet Ihr Abonnement von Microsoft verwaltete Verschlüsselungsschlüssel. Sie können Ihr Abonnement jetzt auch mit eigenen Verschlüsselungsschlüsseln verwalten. [Kundenseitig verwaltete Schlüssel (Customer-Managed Keys, CMK) werden auch als Bring Your Own Key (BYOK) bezeichnet](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/form-recognizer-encryption-of-data-at-rest) und bieten eine größere Flexibilität beim Erstellen, Rotieren, Deaktivieren und Widerrufen von Zugriffssteuerungen. Außerdem können Sie die zum Schutz Ihrer Daten verwendeten Verschlüsselungsschlüssel überwachen.  
   * **Private Endpunkte**: ermöglichen Ihnen in einem virtuellen Netzwerk (VNet) den [sicheren Zugriff auf Daten über einen privaten Link. ](https://docs.microsoft.com/azure/private-link/private-link-overview)


## <a name="june-2020"></a>Juni 2020

### <a name="new-features"></a>Neue Funktionen
* **CopyModel-API wurde zu Client-SDKs hinzugefügt** Sie können jetzt die Client-SDKs verwenden, um Modelle von einem Abonnement in ein anderes zu kopieren. Allgemeine Informationen zu diesem Feature finden Sie unter [Sichern und Wiederherstellen von Modellen](./disaster-recovery.md).
* **Azure Active Directory-Integration**: Sie können jetzt Ihre Azure AD-Anmeldeinformationen verwenden, um Clientobjekte der Formularerkennung in den SDKs zu authentifizieren.
* **SDK-spezifische Änderungen** Dazu gehören sowohl geringfügige Ergänzungen als auch entscheidende Änderungen. Weitere Informationen finden Sie in den SDK-Änderungsprotokollen.
  * [Änderungsprotokoll zu C# SDK Preview 3](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/formrecognizer/Azure.AI.FormRecognizer/CHANGELOG.md)
  * [Änderungsprotokoll zu Python SDK Preview 3](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/CHANGELOG.md)
  * [Änderungsprotokoll zu Java SDK Preview 3](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/CHANGELOG.md)
  * [Änderungsprotokoll zu JavaScript SDK Preview 3](https://github.com/Azure/azure-sdk-for-js/blob/%40azure/ai-form-recognizer_1.0.0-preview.3/sdk/formrecognizer/ai-form-recognizer/CHANGELOG.md)

## <a name="april-2020"></a>April 2020

### <a name="new-features"></a>Neue Funktionen
* **SDK-Unterstützung für Version 2.0 der Formularerkennungs-API (Public Preview):** Diesen Monat wurde die Dienstunterstützung um ein Vorschau-SDK für Version 2.0 der Formularerkennung (Vorschauversion) erweitert. Verwenden Sie die folgenden Links, um die ersten Schritte mit Ihrer bevorzugten Sprache auszuführen: 
   * [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/ai.formrecognizer-readme-pre?view=azure-dotnet)
   * [Java SDK](https://docs.microsoft.com/java/api/overview/azure/ai-formrecognizer-readme-pre?view=azure-java-preview)
   * [Python SDK](https://docs.microsoft.com/python/api/overview/azure/ai-formrecognizer-readme-pre?view=azure-python-preview)
   * [JavaScript SDK](https://docs.microsoft.com/javascript/api/overview/azure/ai-form-recognizer-readme-pre?view=azure-node-preview)

  Das neue SDK unterstützt alle Features von Version 2.0 der REST-API für die Formularerkennung. Sie können beispielsweise ein Modell mit oder ohne Bezeichnungen trainieren und Text, Schlüssel-Wert-Paare und Tabellen aus Ihren Formularen extrahieren, mit dem vorgefertigten Belegdienst Daten aus Belegen extrahieren sowie mit dem Layoutdienst Text und Tabellen aus Ihren Dokumenten extrahieren. Sie können Ihr Feedback zu den SDKs über das [SDK-Feedbackformular](https://aka.ms/FR_SDK_v1_feedback) teilen.
 
* **Kopieren eines benutzerdefinierten Modells:** Mithilfe der neuen Funktion zum Kopieren benutzerdefinierter Modelle können Sie nun Modelle zwischen Regionen und Abonnements kopieren. Vor dem Aufrufen der API zum Kopieren eines benutzerdefinierten Modells müssen Sie zunächst die Autorisierung zum Kopieren in die Zielressource erhalten, indem Sie den Vorgang für die Kopierautorisierung für den Zielressourcenendpunkt aufrufen.
   * REST-API zum [Generieren einer Kopierautorisierung](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/CopyCustomFormModelAuthorization)
   * REST-API zum [Kopieren eines benutzerdefinierten Modells](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/CopyCustomFormModel) 

### <a name="security-improvements"></a>Verbesserungen der Sicherheit

* Kundenseitig verwaltete Schlüssel sind jetzt für FormRecognizer verfügbar. Weitere Informationen finden Sie unter [Verschlüsselung für ruhende Daten der Formularerkennung](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/form-recognizer-encryption-of-data-at-rest).
* Verwenden Sie verwaltete Identitäten für den Zugriff auf Azure-Ressourcen mit Azure Active Directory. Weitere Informationen finden Sie unter [Autorisieren des Zugriffs auf verwaltete Identitäten](https://docs.microsoft.com/azure/cognitive-services/authentication#authorize-access-to-managed-identities).

## <a name="march-2020"></a>März 2020 

### <a name="new-features"></a>Neue Funktionen

* **Werttypen für die Beschriftung** Sie können nun die Typen der Werte angeben, die Sie mit dem Tool „Formularerkennung“ zur Beschriftung von Beispielen beschriften können. Derzeit werden die folgenden Werttypen und Variationen unterstützt:
  * `string`
    * Standardwert, `no-whitespaces`, `alphanumeric`
  * `number`
    * Standardwert, `currency`
  * `date` 
    * Standardwert, `dmy`, `mdy`, `ymd`
  * `time`
  * `integer`

  Weitere Informationen zur Verwendung dieses Features finden Sie in der Anleitung zum [Tool zum Beschriften von Beispielen](./quickstarts/label-tool.md#specify-tag-value-types).


* **Visualisierung von Tabellen** Im Tool für die Beschriftung von Beispielen werden nun im Dokument erkannte Tabellen angezeigt. Dadurch können Sie die im Dokument erkannten und aus dem Dokument extrahierten Tabellen anzeigen, bevor Sie die Beschriftung und Analyse durchführen. Dieses Feature kann mithilfe der Ebenenoption aktiviert bzw. deaktiviert werden.

  Hier sehen Sie ein Beispiel für die Erkennung und Extraktion von Tabellen:

  > [!div class="mx-imgBorder"]
  > ![Tabellenvisualisierung mit dem Tool für die Beschriftung von Beispielen](./media/whats-new/formre-table-viz.png)

    Die extrahierten Tabellen stehen in der JSON-Ausgabe unter `"pageResults"` zur Verfügung.

  > [!IMPORTANT]
  > Das Beschriften von Tabellen wird nicht unterstützt. Nicht automatisch erkannte und extrahierte Tabellen können nur als Schlüssel-Wert-Paare beschriftet werden. Beim Beschriften von Tabellen als Schlüssel-Wert-Paare muss jede Zelle als eindeutiger Wert beschriftet werden.

### <a name="extraction-enhancements"></a>Extraktionsverbesserungen

Dieses Release bietet Verbesserungen bei der Extraktion sowie eine verbesserte Genauigkeit. Hierzu zählt insbesondere die Möglichkeit, mehrere Schlüssel-Wert-Paare in der gleichen Textzeile zu beschriften und zu extrahieren. 
 
### <a name="sample-labeling-tool-is-now-open-source"></a>Tool für die Beschriftung von Beispielen jetzt als Open-Source-Projekt verfügbar

Das Tool „Formularerkennung“ für die Beschriftung von Beispielen ist jetzt als Open-Source-Projekt verfügbar. Sie können es in Ihre Lösungen integrieren und kundenspezifische Änderungen vornehmen, um es an Ihre individuellen Anforderungen anzupassen.

Weitere Informationen zum Tool „Formularerkennung“ für die Beschriftung von Beispielen finden Sie in der Dokumentation auf [GitHub](https://github.com/microsoft/OCR-Form-Tools/blob/master/README.md).

### <a name="tls-12-enforcement"></a>Erzwingen von TLS 1.2

TLS 1.2 wird nun für alle HTTP-Anforderungen erzwungen, die an diesen Dienst gerichtet werden. Weitere Informationen finden Sie unter [Sicherheit von Azure Cognitive Services](../cognitive-services-security.md).

## <a name="january-2020"></a>Januar 2020

In diesem Release wird die Formularerkennung 2.0 (Vorschauversion) eingeführt. In den folgenden Abschnitten finden Sie weitere Informationen zu neuen Features, Verbesserungen und Änderungen. 

### <a name="new-features"></a>Neue Funktionen

* **Benutzerdefiniertes Modell**
  * **Trainieren mit Bezeichnungen** Sie können jetzt ein benutzerdefiniertes Modell mit manuell gekennzeichneten Daten trainieren. Dies führt zu Modellen mit besserer Leistung und kann Modelle hervorbringen, die mit komplexen Formularen oder Formularen arbeiten, die Werte ohne Schlüssel enthalten.
  * **Asynchrone API** Sie können asynchrone API-Aufrufe verwenden, um mit großen Datasets und Dateien zu trainieren und diese zu analysieren.
  * **Unterstützung von TIFF-Dateien** Sie können jetzt mit TIFF-Dokumenten trainieren und Daten aus ihnen extrahieren.
  * **Verbesserte Extrahierungsgenauigkeit**

* **Vordefiniertes Belegmodell**
  * **Trinkgeldbeträge** Sie können jetzt Trinkgeldbeträge und andere handschriftliche Werte extrahieren.
  * **Extraktion von Zeilenelementen** Sie können Werte von Zeilenelementen aus Belegen extrahieren.
  * **Zuverlässigkeitswerte** Sie können die Zuverlässigkeit des Modells für jeden extrahierten Wert anzeigen.
  * **Verbesserte Extrahierungsgenauigkeit**

* **Layoutextraktion** Sie können nun mit der Layout-API Textdaten und Tabellendaten aus Ihren Formularen extrahieren.

### <a name="custom-model-api-changes"></a>Änderungen an der benutzerdefinierten Modell-API

Alle APIs für das Trainieren und Verwenden benutzerdefinierter Modelle wurden umbenannt, und einige synchrone Methoden sind jetzt asynchron. Dies sind die wichtigsten Änderungen:

* Der Prozess zum Trainieren eines Modells ist jetzt asynchron. Sie initiieren das Training über den API-Aufruf **/custom/models**. Dieser Aufruf gibt eine Vorgangs-ID zurück, die Sie in **custom/models/{modelID}** übergeben können, um die Trainingsergebnisse zurückzugeben.
* Die Schlüssel-Wert-Extraktion wird jetzt vom API-Aufruf **/custom/models/{modelID}/analyze** initiiert. Dieser Aufruf gibt eine Vorgangs-ID zurück, die Sie in **custom/models/{modelID}/analyzeResults/{resultID}** übergeben können, um die Extraktionsergebnisse zurückzugeben.
* Vorgangs-IDs für das Trainieren finden sich jetzt im Header **Location** von HTTP-Antworten, nicht mehr im Header **Operation-Location**.

### <a name="receipt-api-changes"></a>Änderungen an der Beleg-API

Die APIs zum Lesen von Belegen wurden umbenannt.

* Die Extraktion von Belegdaten wird jetzt vom API-Aufruf **/prebuilt/receipt/analyze** initiiert. Dieser Aufruf gibt eine Vorgangs-ID zurück, die Sie in **/prebuilt/receipt/analyzeResults/{resultID}** übergeben können, um die Extraktionsergebnisse zurückzugeben.

### <a name="output-format-changes"></a>Änderungen am Ausgabeformat

Die JSON-Antworten für alle API-Aufrufe haben neue Formate. Es wurden einige Schlüssel und Werte hinzugefügt, entfernt oder umbenannt. Beispiele für die aktuellen JSON-Formate finden Sie in den Schnellstarts.

## <a name="next-steps"></a>Nächste Schritte

Durchlaufen Sie eine [Schnellstartanleitung für die Clientbibliothek](quickstarts/client-library.md), um damit zu beginnen, eine Formularverarbeitungs-App mit Formularerkennung in der Sprache Ihrer Wahl zu schreiben.

## <a name="see-also"></a>Weitere Informationen

* [Was ist die Formularerkennung?](./overview.md)
