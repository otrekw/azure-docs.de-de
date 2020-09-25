---
title: Planen Ihrer Azure Communication Services-Lösung für Telefonie und SMS
titleSuffix: An Azure Communication Services concept document
description: Es wird beschrieben, wie Sie die Verwendung von Telefonnummern und der Telefoniefunktionen effektiv planen.
author: stkozak
manager: rampras
services: azure-communication-services
ms.author: stkozak
ms.date: 06/23/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 39f88ab8b735438f60d8e20513ea5cbda43d41ee
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944242"
---
# <a name="plan-your-telephony-and-sms-solution"></a>Planen Ihrer Telefonie- und SMS-Lösung

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

In diesem Dokument werden die verschiedenen Telefoniepläne und Nummerntypen von Azure Communication Services beschrieben. Es werden die einzelnen Entscheidungsabläufe beschrieben, um Ihnen die Auswahl eines Sprachdienstanbieters und der Telefonnummerntypen, Pläne und Funktionen zu erleichtern, die von Communication Services bereitgestellt werden.

## <a name="about-phone-numbers-in-azure-communications-services"></a>Informationen zu Telefonnummern in Azure Communication Services

Azure Communication Services ermöglicht Ihnen die Verwendung von Telefonnummern zum Tätigen/Senden und Empfangen von Telefonanrufen und SMS-Nachrichten. Diese Telefonnummern können verwendet werden, um die Anrufer-ID für ausgehende Anrufe zu konfigurieren, die von Ihrem Dienst getätigt werden.
  
Falls Sie nicht über eine vorhandene Telefonnummer zum Importieren in Ihre Communication Services-Lösung verfügen, besteht die einfachste Vorgehensweise darin, über Azure Communication Services eine neue Telefonnummer zu beziehen. Dies dauert nur wenige Minuten.

Falls Sie eine vorhandene Telefonnummer in Ihrer Lösung weiter nutzen möchten (z. B. 1-800-COMPANY), können Sie sie vom vorhandenen Anbieter zu Communication Services portieren.

Das folgende Diagramm soll Ihnen als Hilfe beim Navigieren durch die verfügbaren Optionen dienen:

:::image type="content" source="../media/plan-solution/decision-tree-basic.png" alt-text="Diagramm: Treffen einer Entscheidung zu Ihren Telefonnummern":::

Wir sehen uns nun die Typen und Funktionen der Telefonnummern an, die in Communication Services verfügbar sind. 

## <a name="microsoft-direct-offer-of-phone-numbers-and-capabilities"></a>Direktes Microsoft-Angebot zu Telefonnummern und Funktionen

Bei Azure Communication Services verfügen Entwickler über besondere Flexibilität. Für die meisten Telefonnummern können die Pläne beliebig zusammengestellt werden. Einige Entwickler benötigen nur einen Plan für eingehende Anrufe, und andere entscheiden sich ggf. für Pläne für eingehende Anrufe und ausgehende SMS. Sie können diese Pläne auswählen, wenn Sie Ihre Telefonnummern in Communication Services leasen bzw. portieren.

Die Verfügbarkeit der Pläne richtet sich nach dem Land und dem Telefonnummerntyp, den Sie nutzen. Im Diagramm unten ist der Entscheidungsfluss dargestellt:    Die Verfügbarkeit der Pläne richtet sich nach dem genutzten Land und Telefonnummerntyp.

<!-- Tami/team have rejected this multiple times despite updates, says it needs to be higher res - need to work with her to get approval for this image. Commenting out to move our staging forward. :::image type="content" source="../../media/example-decision-flow.png" alt-text="Example for the decision flow"::: -->

Bevor Sie einen Telefonnummerntyp auswählen können, sehen wir uns den Plan für internationale Telefonnummern an.

### <a name="optional-reading-international-public-telecommunication-numbering-plan-e164"></a>Optionale Lektüre: International Public Telecommunication Numbering Plan (E.164)

> [!NOTE]
> Wir empfehlen Ihnen die Lektüre dieser Informationen auch für den Fall, dass Sie bereits mit dem E.164-Plan für Telefonnummern vertraut sind, um sich noch besser mit den Nummerntypen und Funktionen des direkten Azure Communication Services-Angebots vertraut zu machen.

Der International Public Telecommunication Numbering Plan ist in der Empfehlung E.164 der International Telecommunication Union (ITU) definiert. Es handelt sich um eine konforme Nummer, wenn sie maximal 15 Ziffern lang ist.

Die Telefonnummer besteht aus den folgenden Elementen:

-   Präfix „+“
-   Präfix für die internationale Vorwahl oder Landes-/Regionsvorwahl (ein, zwei oder drei Ziffern) 
-   *(Optional)* Nationale Vorwahl oder Nummerierungsplan, meist als Ortsvorwahl bezeichnet. Die Länge hängt hierbei vom jeweiligen Land ab. In den USA hat diese Vorwahl eine Länge von drei Ziffern. In Australien und Neuseeland wird nur eine Ziffer verwendet. In Deutschland, Japan, Mexiko und einigen anderen Ländern haben die Ortsvorwahlen unterschiedliche Längen. In Deutschland kann die Ortsvorwahl zwischen zwei und fünf Ziffern lang sein, während in Japan ein bis fünf Ziffern verwendet werden.
-   Eine Teilnehmerrufnummer

> [!NOTE]
> Die obige Klassifizierung entspricht nicht vollständig dem E.164-Standard der ITU und stellt lediglich eine vereinfachte Beschreibung dar. Die Teilnehmerrufnummer ist innerhalb des Standards beispielsweise noch weiter unterteilt. Falls Sie sich eingehender mit dem Plan für internationale Telefonnummern vertraut machen möchten, ist es ratsam, mit dem [E.164-Standard der ITU](https://www.itu.int/rec/T-REC-E.164) zu beginnen.  

Hier sind einige Beispiele angegeben, die Ihnen das Verständnis des Telefonnummernplans erleichtern:

Regionale Telefonnummer in den USA:

:::image type="content" source="../media/plan-solution/regional-us.png" alt-text="Beispiel für eine regionale Telefonnummer in den USA":::

Eine regionale Telefonnummer in Kanada:

:::image type="content" source="../media/plan-solution/regional-canada.png" alt-text="Beispiel für eine regionale Telefonnummer in Kanada":::

Eine gebührenfreie Nummer in der Region Nordamerika:

:::image type="content" source="../media/plan-solution/tollfree-us.png" alt-text="Beispiel für eine gebührenfreie Nummer in Nordamerika":::

Eine Mobiltelefonnummer im Vereinigten Königreich:

:::image type="content" source="../media/plan-solution/mobile-uk.png" alt-text="Beispiel für eine Mobiltelefonnummer im Vereinigten Königreich":::

Als Nächstes sehen wir uns die spezifischen Telefonnummerntypen an, die in Azure Communication Services verfügbar sind.

## <a name="phone-number-types-in-azure-communication-services"></a>Telefonnummerntypen in Azure Communication Services

Microsoft bietet regionale und gebührenfreie Pläne für Kurzwahl- und Mobiltelefonnummern in Ländern an, in denen dies möglich ist.

In der folgenden Tabelle sind diese Telefonnummerntypen zusammengefasst: 

| Telefonnummerntyp | Beispiel                              | Länderverfügbarkeit    | Gängiger Anwendungsfall                                                                                                     |
| ----------------- | ------------------------------------ | ----------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Länderspezifisch          | +1 (geografische Ortsvorwahl) XXX XX XX  | USA, Kanada, Puerto Rico | Zuweisen von Telefonnummern für Benutzer in Ihren Anwendungen oder Zuweisen zu IVR-Systemen (Interactive Voice Response, Interaktive Sprachantwort) oder Bots |
| Gebührenfrei         | +1 (gebührenfreie *Ortsvorwahl*) XXX XX XX | USA, Kanada, Puerto Rico | Zuweisen zu IVR-Systemen (Interactive Voice Response, Interaktive Sprachantwort) oder Bots, SMS-Anwendungen                                        |

## <a name="plans"></a>Pläne 

Nun sehen wir uns die Funktionen an, die Sie für Ihre Telefonnummern aktivieren können. Diese Funktionen variieren aufgrund der rechtlichen Anforderungen je nach Land. Azure Communication Services verfügt über die folgenden Funktionen:

- **Unidirektionale ausgehende SMS**: Nützlich für die Benachrichtigung und Szenarien mit zweistufiger Authentifizierung.
- **Bidirektionale ein- und ausgehende SMS**: Vordefiniertes Paket, bei dem die SMS als Teil eines Plans gesendet und empfangen werden kann.
- **Anrufe über das Telefonfestnetz**: Sie können eingehende Anrufe auswählen und die Anrufer-ID verwenden, um ausgehende Anrufe zu tätigen.

## <a name="next-steps"></a>Nächste Schritte

### <a name="quickstarts"></a>Schnellstarts

- [Beschaffen einer Telefonnummer](../../quickstarts/telephony-sms/get-phone-number.md)
- [Tätigen eines Anrufs](../../quickstarts/voice-video-calling/calling-client-samples.md)
- [Senden einer SMS](../../quickstarts/telephony-sms/send.md)

### <a name="conceptual-documentation"></a>Dokumentation

- [Sprach- und Videokonzepte](../voice-video-calling/about-call-types.md)
- [Anruf- und SMS-Abläufe](../call-flows.md)
- [Preise](../pricing.md)
