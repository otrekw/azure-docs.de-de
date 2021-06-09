---
title: 'Tutorial: Da- Oder-Formulary -Azure API for FHIR'
description: Dieses Tutorial führt Sie durch die Einrichtung der Azure API for FHIR, um die Touchstone-Tests mit dem Implementierungsleitfaden für DaVinci-Formulary zu bestehen.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: modillon
ms.date: 06/07/2021
ms.openlocfilehash: fc66de13c02d6d04e8a156937de70775b9053da3
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111751089"
---
# <a name="da-vinci-drug-formulary"></a>Da- Und-Ich-Formulary

In diesem Tutorial erfahren Sie, wie Sie die Azure API for FHIR einrichten, um die [Touchstone-Tests](https://touchstone.aegis.net/touchstone/) für den [Da Payer Data Exchange US-Formulary Implementation Guide zu bestehen.](http://hl7.org/fhir/us/Davinci-drug-formulary/)

## <a name="touchstone-capability-statement"></a>Touchstone capability statement (Touchstonefunktions-Anweisung)

Der erste Test, auf den wir uns konzentrieren, ist das Testen der Azure API for FHIR mit der [Da- Und-Mal-Formulary-Funktionsanweisung](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/Formulary/00-Capability&activeOnly=false&contentEntry=TEST_SCRIPTS). Wenn Sie diesen Test ohne Updates ausführen, kann der Test aufgrund fehlender Suchparameter und fehlender Profile nicht ausgeführt werden.

### <a name="define-search-parameters"></a>Definieren von Suchparametern

Im Rahmen der Da Formulary IG für Formulary [](how-to-do-custom-search.md) Müssen Sie drei neue Suchparameter für die Ressource FormularyDrug definieren. Alle drei werden in der Capability-Anweisung getestet.

* [Einsentier](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugTier.json.html)
* [Veralten](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugPlan.json.html)
* [Name des Vornamens](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugName.json.html)

Die restlichen Suchparameter, die für die DaUch-Ig für das Formulary-Formular benötigt werden, werden durch die Basisspezifikation definiert und sind bereits in der Azure API for FHIR ohne weitere Updates verfügbar.

### <a name="store-profiles"></a>Speichern von Profilen

Außerhalb der Definition von Suchparametern müssen Sie nur noch die erforderlichen Profile laden, um diesen [Test zu bestehen.](validation-against-profiles.md) Es gibt zwei Profile, die als Teil der DA-Ig für das Da- Und-Dabei-Formular verwendet werden.

* [Formulary-Abhängige](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/StructureDefinition-usdf-FormularyDrug.html)
* [Formulary Coverage-Plan](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/StructureDefinition-usdf-CoveragePlan.html)

### <a name="sample-rest-file"></a>Beispiel-REST-Datei

Um die Erstellung dieser Suchparameter und Profile zu unterstützen, verfügen wir auf der Open-Source-Website über die HTTP-Beispieldatei [Da Header Formulary,](https://github.com/microsoft/fhir-server/blob/main/docs/rest/DaVinciFormulary/DaVinciFormulary.http) die alle oben beschriebenen Schritte in einer einzelnen Datei enthält. Nachdem Sie alle erforderlichen Profile und Suchparameter hochgeladen haben, können Sie den Funktionsauszugstest in Touchstone ausführen. Sie sollten eine erfolgreiche Ausführung erhalten:

:::image type="content" source="media/cms-tutorials/davinci-test-script-execution.png" alt-text="Da Script-Testskriptausführung.":::

## <a name="touchstone-query-test"></a>Touchstone-Abfragetest

Der zweite Test ist die [Abfragefunktionen](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/Formulary/01-Query&activeOnly=false&contentEntry=TEST_SCRIPTS). Mit diesem Test wird überprüft, ob Sie mithilfe verschiedener Parameter nach bestimmten Ressourcen für Abdeckungsplan und -ressourcen suchen können. Der beste Weg wäre, anhand von Ressourcen zu testen, die bereits in Ihrer Datenbank vorhanden sind, aber wir verfügen auch über die DA VINCIFORMULARY_SAMPLE_RESOURCES-HTTP-Datei mit Beispielressourcen, die aus den Beispielen in der IG entnommen wurden und mit denen Sie die Ressourcen erstellen und testen können. [](https://github.com/microsoft/fhir-server/blob/main/docs/rest/DaVinciFormulary/DaVinciFormulary_Sample_Resources.http)

:::image type="content" source="media/cms-tutorials/davinci-test-execution-results.png" alt-text="Ergebnisse der Da- Und-Testausführung.":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir erfahren, wie Sie das Da Payer Payer Data Exchange US-Formulary in Touchstone übergeben. Als Nächstes erfahren Sie, wie Sie das Da Guides PDex-Implementierungshandbuch in Touchstone testen.

>[!div class="nextstepaction"]
>[Da Und PDex](davinci-pdex-tutorial.md)