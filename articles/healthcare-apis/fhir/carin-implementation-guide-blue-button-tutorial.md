---
title: 'Tutorial: CARIN-Implementierungshandbuch für blue button&#174; – Azure API for FHIR'
description: In diesem Tutorial werden die Schritte zum Einrichten der Azure API for FHIR zum Bestehen der Touchstonetests für das CARIN Implementation Guide for Blue Button (C4BB IG) beschrieben.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 05/27/2021
ms.openlocfilehash: 1fade881fbe6d5c401712a4f97c8a249006d8190
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111590376"
---
# <a name="carin-implementation-guide-for-blue-button174"></a>CARIN Implementation Guide for Blue Button&#174;

In diesem Tutorial erfahren Sie, wie Sie die Azure API for FHIR einrichten, um die [Touchstonetests](https://touchstone.aegis.net/touchstone/) für das [CARIN Implementation Guide for Blue Button](https://build.fhir.org/ig/HL7/carin-bb/index.html) (C4BB IG) zu bestehen.

## <a name="touchstone-capability-statement"></a>Touchstone capability statement (Touchstonefunktions-Anweisung)

Der erste Test, auf den wir uns konzentrieren, ist das Testen der Azure API for FHIR mit der [C4BB IG-Funktions-Anweisung](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/CARIN/CARIN-4-BlueButton/00-Capability&activeOnly=false&contentEntry=TEST_SCRIPTS). Wenn Sie diesen Test für die Azure API for FHIR ohne Updates ausführen, kann der Test aufgrund fehlender Suchparameter und fehlender Profile nicht ausgeführt werden. 

### <a name="define-search-parameters"></a>Definieren von Suchparametern

Im Rahmen der C4BB IG müssen Sie drei neue Suchparameter [für](how-to-do-custom-search.md) die Ressource `ExplanationOfBenefit` definieren. Zwei davon werden in der Capability-Anweisung (Typ und Dienstdatum) getestet, und eine wird für Suchvorgänge `_include` (Veröffentlichung) benötigt.  

* [type](https://build.fhir.org/ig/HL7/carin-bb/SearchParameter-explanationofbenefit-type.json)
* [Dienstdatum](https://build.fhir.org/ig/HL7/carin-bb/SearchParameter-explanationofbenefit-service-date.json)
* [Versicherer](https://build.fhir.org/ig/HL7/carin-bb/SearchParameter-explanationofbenefit-insurer.json)

> [!NOTE]
> Im unformatiert JSON-Code für diese Suchparameter ist der Name auf `ExplanationOfBenefit_<SearchParameter Name>` festgelegt. Der Touchstone-Test erwartet, dass der Name für diese den Typ **,** **das** Dienstdatum und den Namen **hat.**  
 
Die restlichen Suchparameter, die für die C4BB IG erforderlich sind, werden durch die Basisspezifikation definiert und sind bereits im Azure API for FHIR ohne zusätzliche Updates verfügbar.
 
### <a name="store-profiles"></a>Speichern von Profilen

Neben der Definition von Suchparametern müssen Sie zum Bestehen dieses Tests auch die erforderlichen [Profile laden.](validation-against-profiles.md) Innerhalb der C4BB IG sind acht Profile definiert. 

* [C4BB-Abdeckung](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-Coverage.html) 
* [C4BB ExplanationOfBenefit InpatientEs](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-ExplanationOfBenefit-Inpatient-Institutional.html) 
* [C4BB ExplanationOfBenefit Bzw.](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-ExplanationOfBenefit-Outpatient-Institutional.html) 
* [C4BB ExplanationOfBenefit-](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-ExplanationOfBenefit-Pharmacy.html) 
* [C4BB ExplanationOfBenefit Professional Non -Polizisch](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-ExplanationOfBenefit-Professional-NonClinician.html) 
* [C4BB-Organisation](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-Organization.html) 
* [C4BB-Patient](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-Patient.html) 
* [C4BB-2016](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-Practitioner.html) 

### <a name="sample-rest-file"></a>Beispiel-REST-Datei

Um die Erstellung dieser Suchparameter und Profile [](https://github.com/microsoft/fhir-server/blob/main/docs/rest/C4BB/C4BB.http) zu unterstützen, verfügen wir über eine HTTP-Beispieldatei, die alle oben beschriebenen Schritte in einer einzelnen Datei enthält. Nachdem Sie alle erforderlichen Profile und Suchparameter hochgeladen haben, können Sie den Funktionsauszugstest in Touchstone ausführen.

:::image type="content" source="media/cms-tutorials/capability-test-script-execution-results.png" alt-text="Ergebnisse der Ausführung des Funktionstestskripts.":::

## <a name="touchstone-read-test"></a>Touchstonelesetest

Nach dem Testen der Capabilities-Anweisung testen wir die Lesefunktionen der Azure API for FHIR C4BB IG. [](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/CARIN/CARIN-4-BlueButton/01-Read&activeOnly=false&contentEntry=TEST_SCRIPTS) Dieser Test testet die Konformität mit den acht Profilen, die Sie im ersten Test geladen haben. Sie müssen Ressourcen geladen haben, die den Profilen entsprechen. Der beste Weg wäre, anhand von Ressourcen zu testen, die bereits in Ihrer Datenbank vorhanden sind, aber wir verfügen auch über eine [HTTP-Datei](https://github.com/microsoft/fhir-server/blob/main/docs/rest/C4BB/C4BB_Sample_Resources.http) mit Beispielressourcen, die aus den Beispielen in der IG entnommen wurden und mit denen Sie die Ressourcen erstellen und testen können.

:::image type="content" source="media/cms-tutorials/test-execution-results-touchstone.png" alt-text="Touchstone liest Testausführungsergebnisse.":::

## <a name="touchstone-eob-query-test"></a>Touchstone – EOB-Abfragetest

Der nächste Test, den wir überprüfen, ist der [EOB-Abfragetest](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/CARIN/CARIN-4-BlueButton/02-EOBQuery&activeOnly=false&contentEntry=TEST_SCRIPTS). Wenn Sie den Lesetest bereits abgeschlossen haben, haben Sie alle Daten geladen, die Sie benötigen. Mit diesem Test wird überprüft, ob Sie mithilfe verschiedener Parameter nach bestimmten Ressourcen und `Patient` `ExplanationOfBenefit` Ressourcen suchen können.

:::image type="content" source="media/cms-tutorials/test-execution-touchstone-eob-query-test.png" alt-text="Touchstone-EOB-Abfrageausführungsergebnisse.":::

## <a name="touchstone-error-handling-test"></a>Touchstone– Fehlerbehandlungstest

Der letzte Test, den wir durchlaufen, ist das Testen [der Fehlerbehandlung.](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/CARIN/CARIN-4-BlueButton/99-ErrorHandling&activeOnly=false&contentEntry=TEST_SCRIPTS) Der einzige Schritt, den Sie tun müssen, ist das Löschen einer ExplanationOfBenefit-Ressource aus Ihrer Datenbank und die Verwendung der ID der gelöschten `ExplanationOfBenefit` Ressource im Test.

:::image type="content" source="media/cms-tutorials/test-execution-touchstone-error-handling.png" alt-text="Touchstone-EOB-Fehlerbehandlungsergebnisse.":::


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir erfahren, wie Sie die CARIN IG for Blue Button-Tests in Touchstone bestehen. Als Nächstes können Sie überprüfen, wie Sie die Da- Oder-Formulary-Tests testen.

>[!div class="nextstepaction"]
>[DaVinci-Formulary](davinci-drug-formulary-tutorial.md)       
 