---
title: 'Tutorial: DaVinci PDex – Azure API for FHIR'
description: Dieses Tutorial führt Sie durch die Einrichtung der Azure API for FHIR zum Bestehen von Tests für den Da Payer Data Exchange-Implementierungsleitfaden.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 06/02/2021
ms.openlocfilehash: 0e6ee8ba2288ae61eb1fe0d5e6b76ebc63a64897
ms.sourcegitcommit: 832e92d3b81435c0aeb3d4edbe8f2c1f0aa8a46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111562819"
---
# <a name="davinci-pdex"></a>DaVinci PDex

In diesem Tutorial wird das Einrichten der Azure API for FHIR zum Bestehen der [Touchstone-Tests](https://touchstone.aegis.net/touchstone/) für das [Da Payer Data Exchange Implementation Guide](http://hl7.org/fhir/us/davinci-pdex/toc.html) (PDex IG) durchlaufen.

> [!NOTE]
> Für alle diese Tests führen wir sie für die JSON-Tests aus. Die Azure API for FHIR unterstützt sowohl JSON als auch XML, verfügt jedoch nicht über separate Endpunkte für den Zugriff auf JSON oder XML. Aus diesem Grund schlagen alle XML-Tests fehl. Wenn Sie die Funktionsanweisung in XML anzeigen möchten, übergeben Sie einfach den \_ Formatparameter \` GET {fhirurl}/metadata? \_ format=xml\`

## <a name="touchstone-capability-statement"></a>Touchstonefunktions-Anweisung

Der erste Satz von Tests, auf den wir uns konzentrieren werden, besteht darin, die Azure API for FHIR mit der PDex IG-Funktions-Anweisung zu testen. Diese Tests verfügen über drei Validierungsprozesse:

* Der erste Test überprüft lediglich die grundlegende Funktions-Anweisung anhand der IG-Anforderungen und wird ohne Updates bestanden.

* Der zweite Test überprüft, ob alle Profile für US Core hinzugefügt wurden. Dieser Test wird ohne Updates bestanden, enthält jedoch eine Reihe von Warnungen. Damit diese Warnungen entfernt werden, müssen Sie [die US Core-Profile laden.](validation-against-profiles.md) Wir haben eine [HTTP-Beispieldatei](https://github.com/microsoft/fhir-server/blob/main/docs/rest/PayerDataExchange/USCore.http) erstellt, die die Erstellung aller Profile durchläuft. Sie können die [Profile](http://hl7.org/fhir/us/core/STU3.1.1/profiles.html#profiles) auch direkt von der HL7-Website abrufen, die über die aktuellsten Versionen verfügt.

* Der dritte Test überprüft, ob der Vorgang patient-everything unterstützt wird. Im Moment schlägt dieser Test fehl. Der Vorgang ist Mitte Juni 2021 im Azure API for FHIR verfügbar und jetzt auf dem Open-Source-FHIR-Server auf Cosmos DB verfügbar. Es fehlt jedoch in der Capability-Anweisung, sodass dieser Test fehlschlägt, bis wir eine Korrektur für Fehler [1989](https://github.com/microsoft/fhir-server/issues/1989)veröffentlichen. 

 
:::image type="content" source="media/cms-tutorials/davinci-pdex-test-script-failed.png" alt-text="Fehler bei der DaVinci PDex-Ausführung.":::

## <a name="touchstone-member-match-test"></a>Touchstone $member-Match-Test

Der [zweite Test](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/PayerExchange/01-Member-Match&activeOnly=false&contentEntry=TEST_SCRIPTS) im Abschnitt "Payer Data Exchange" testet das Vorhandensein des [$member-Match-Vorgangs](http://hl7.org/fhir/us/davinci-hrex/2020Sep/OperationDefinition-member-match.html). Weitere Informationen zum $member-Match-Vorgang finden Sie in unserer [Übersicht über $member-Match-Vorgänge.](tutorial-member-match.md)

In diesem Test müssen Sie einige Beispieldaten laden, damit der Test erfolgreich ist. Wir haben [hier](https://github.com/microsoft/fhir-server/blob/main/docs/rest/PayerDataExchange/membermatch.http) eine Restdatei mit dem Patienten und der Abdeckung verknüpft, die Sie für den Test benötigen. Nachdem diese Daten geladen wurden, können Sie diesen Test erfolgreich bestehen. Wenn die Daten nicht geladen werden, erhalten Sie eine 422-Antwort, da keine genaue Übereinstimmung gefunden wurde.

:::image type="content" source="media/cms-tutorials/davinci-pdex-test-script-passed.png" alt-text="DaVinci PDex-Testskript erfolgreich.":::

## <a name="touchstone-patient-by-reference"></a>Touchstone-Patient als Referenz

Die nächsten Tests, die wir überprüfen, sind die [Patienten anhand von Referenztests.](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/PayerExchange/02-PatientByReference&activeOnly=false&contentEntry=TEST_SCRIPTS) Dieser Satz von Tests überprüft anhand verschiedener Suchkriterien, ob Sie einen Patienten finden können. Die beste Möglichkeit, den Patienten anhand einer Referenz zu testen, ist das Testen mit Ihren eigenen Daten, aber wir haben eine [Beispielressourcendatei](https://github.com/microsoft/fhir-server/blob/main/docs/rest/PayerDataExchange/PDex_Sample_Data.http) hochgeladen, die Sie auch laden können.

:::image type="content" source="media/cms-tutorials/davinci-pdex-test-execution-passed.png" alt-text="DaVinci PDex-Ausführung übergeben.":::

## <a name="touchstone-patienteverything-test"></a>Touchstone patient/$everything test (Touchstone-Patient/$everything-Test)

Der letzte Test, den wir durchlaufen, ist das Testen von patient-everything. Für diesen Test müssen Sie einen Patienten laden, und dann verwenden Sie die ID dieses Patienten, um zu testen, ob Sie den $everything Vorgang verwenden können, um alle Daten im Zusammenhang mit dem Patienten zu pullen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir schritt für Schritt erfahren, wie Sie die Exchange-Tests für die Zahlung in Touchstone bestehen. Als Nächstes erfahren Sie mehr über alle Azure API for FHIR Features.

>[!div class="nextstepaction"]
>[Unterstützte Features](fhir-features-supported.md)  