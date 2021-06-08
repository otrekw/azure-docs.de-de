---
title: 'Tutorial: DaVinci PDex – Azure API for FHIR'
description: Dieses Tutorial führt Sie durch die Einrichtung der Azure API for FHIR, um Tests für den Da Payer Data Exchange Implementation Guide zu bestehen.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 06/02/2021
ms.openlocfilehash: 3f9aa795a08aa027fd0cc9758e9479fa0ec81e09
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111592977"
---
# <a name="davinci-pdex"></a>DaVinci PDex

In diesem Tutorial erfahren Sie, wie Sie die Azure API for FHIR einrichten, um die [Touchstone-Tests](https://touchstone.aegis.net/touchstone/) für das [Da Payer Data Exchange Implementation Guide](http://hl7.org/fhir/us/davinci-pdex/toc.html) (PDex IG) zu bestehen.

> [!NOTE]
> Für alle diese Tests führen wir sie für die JSON-Tests aus. Der Azure API for FHIR unterstützt sowohl JSON als auch XML, hat jedoch keine separaten Endpunkte für den Zugriff auf JSON oder XML. Aus diesem Grund können alle XML-Tests nicht durchgeführt werden. Wenn Sie die Capability-Anweisung in XML anzeigen möchten, übergeben Sie einfach den \_ Formatparameter \` GET {fhirurl}/metadata? \_ format=xml\`

## <a name="touchstone-capability-statement"></a>Touchstone capability statement (Touchstonefunktions-Anweisung)

Die erste Gruppe von Tests, auf die wir uns konzentrieren, ist das Testen der Azure API for FHIR mit der PDex IG-Funktions-Anweisung. Dies umfasst drei Tests:

* Der erste Test überprüft die grundlegende Funktions-Anweisung anhand der IG-Anforderungen und wird ohne Updates übergeben.

* Der zweite Test überprüft, ob alle Profile für US Core hinzugefügt wurden. Dieser Test wird ohne Updates bestanden, enthält jedoch eine Reihe von Warnungen. Um diese Warnungen zu entfernen, müssen Sie [die US Core-Profile laden.](validation-against-profiles.md) Wir haben eine [HTTP-Beispieldatei erstellt,](https://github.com/microsoft/fhir-server/blob/main/docs/rest/PayerDataExchange/USCore.http) die die Erstellung aller Profile durchfingt. Sie können die Profile auch [direkt von](http://hl7.org/fhir/us/core/STU3.1.1/profiles.html#profiles) der HL7-Website mit den neuesten Versionen erhalten.

* Der dritte Test überprüft, ob der [$patient -Alles-Vorgang](patient-everything.md) unterstützt wird. Derzeit ist dieser Test nicht bestanden. Der Vorgang wird Mitte Juni 2021 im Azure API for FHIR verfügbar sein und ist jetzt auf dem Open Source-FHIR-Server auf Cosmos DB. Es fehlt jedoch in der Capability-Anweisung, sodass dieser Test fehlschlägt, bis wir eine Fehlerbehebung für Fehler [1989 veröffentlichen.](https://github.com/microsoft/fhir-server/issues/1989) 

 
:::image type="content" source="media/cms-tutorials/davinci-pdex-test-script-failed.png" alt-text="Fehler bei der DaVinci-PDex-Ausführung.":::

## <a name="touchstone-member-match-test"></a>Touchstone $member –Übereinstimmungstest

Der [zweite Test](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/PayerExchange/01-Member-Match&activeOnly=false&contentEntry=TEST_SCRIPTS) im Abschnitt "Payer Data Exchange" testet das Vorhandensein des $member [Übereinstimmungsvorgang.](http://hl7.org/fhir/us/davinci-hrex/2020Sep/OperationDefinition-member-match.html) Weitere Informationen zum Vorgang "$member-match" finden Sie in der [Übersicht $member-Match-Vorgangs.](tutorial-member-match.md)

In diesem Test müssen Sie einige Beispieldaten laden, damit der Test bestanden wird. Wir haben hier eine Restdatei [mit](https://github.com/microsoft/fhir-server/blob/main/docs/rest/PayerDataExchange/membermatch.http) dem Patienten und der Abdeckung verknüpft, die Sie für den Test benötigen. Nachdem diese Daten geladen wurden, können Sie diesen Test erfolgreich bestehen. Wenn die Daten nicht geladen werden, erhalten Sie eine 422-Antwort, weil sie keine genaue Übereinstimmung finden.

:::image type="content" source="media/cms-tutorials/davinci-pdex-test-script-passed.png" alt-text="DaVinci PDex-Testskript erfolgreich.":::

## <a name="touchstone-patient-by-reference"></a>Touchstone-Patient nach Referenz

Die nächsten Tests, die wir überprüfen, sind der [Patient nach Referenztests.](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/PayerExchange/02-PatientByReference&activeOnly=false&contentEntry=TEST_SCRIPTS) Mit diesem Satz von Tests wird überprüft, ob Sie einen Patienten anhand verschiedener Suchkriterien finden können. Die beste Möglichkeit, den Patienten nach Verweis zu testen, ist das [](https://github.com/microsoft/fhir-server/blob/main/docs/rest/PayerDataExchange/PDex_Sample_Data.http) Testen mit Ihren eigenen Daten, aber wir haben eine Beispielressourcendatei hochgeladen, die Sie auch laden können, um sie zu verwenden.

:::image type="content" source="media/cms-tutorials/davinci-pdex-test-execution-passed.png" alt-text="DaVinci PDex-Ausführung erfolgreich.":::

## <a name="touchstone-patienteverything-test"></a>Touchstone-Patient/$everything Test

Der letzte Test, den wir durchlaufen, ist das Testen von patient-everything. Für diesen Test müssen Sie einen Patienten laden. Anschließend verwenden Sie die ID dieses Patienten, um zu testen, ob Sie den $everything-Vorgang verwenden können, um alle Daten im Zusammenhang mit dem Patienten zu pullen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir erfahren, wie Sie die Payer Exchange-Tests in Touchstone bestehen. Als Nächstes erfahren Sie mehr über alle Azure API for FHIR Features.

>[!div class="nextstepaction"]
>[Unterstützte Features](fhir-features-supported.md)  