---
title: 'Tutorial: DaVinci-Formular für Rauschmittel – Azure API for FHIR'
description: In diesem Tutorial wird das Einrichten der Azure API for FHIR zum Bestehen der Touchstone-Tests für den DaVinci-Leitfaden zur Formularimplementierung für Suchtmittel durchlaufen.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: modillon
ms.date: 06/01/2021
ms.openlocfilehash: feeccd8f194397e9c99f19920d09b8bb2056ff08
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111592959"
---
# <a name="davinci-drug-formulary"></a>DaVinci-Formular für Mittel

In diesem Tutorial wird das Einrichten des Azure API for FHIR zum Bestehen der [Touchstone-Tests](https://touchstone.aegis.net/touchstone/) für den [DaVinci Payer Data Exchange US-Leitfaden zur Formularimplementierung von Us-Dollar](http://hl7.org/fhir/us/Davinci-drug-formulary/)durchlaufen.

## <a name="touchstone-capability-statement"></a>Touchstonefunktions-Anweisung

Der erste Test, auf den wir uns konzentrieren werden, besteht darin, die Azure API for FHIR mit der [DaVinci-Anweisung für die Formularfunktion von DaVinci zu](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/Formulary/00-Capability&activeOnly=false&contentEntry=TEST_SCRIPTS)testen. Wenn Sie diesen Test ohne Updates ausführen, schlägt der Test aufgrund fehlender Suchparameter und fehlender Profile fehl.

### <a name="define-search-parameters"></a>Definieren von Suchparametern

Im Rahmen der Da Wiesn-Formular-IG müssen Sie drei [neue Suchparameter](how-to-do-custom-search.md) für die FormularyDrug-Ressource definieren. Alle drei werden in der capability-Anweisung getestet.

* [DrugTier](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugTier.json.html)
* [Pharmaplan](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugPlan.json.html)
* [Pharmaname](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugName.json.html)

Die restlichen Suchparameter, die für die Da Wiesn Formulary IG erforderlich sind, werden durch die Basisspezifikation definiert und sind bereits im Azure API for FHIR ohne weitere Updates verfügbar.

### <a name="store-profiles"></a>Speichern von Profilen

Abgesehen von der Definition von Suchparametern müssen Sie nur die [erforderlichen Profile](validation-against-profiles.md)laden, um diesen Test zu bestehen. Es gibt zwei Profile, die als Teil der Da Wiesn-Formular-IG verwendet werden.

* [Formulary Pharma](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/StructureDefinition-usdf-FormularyDrug.html)
* [Formularabdeckungsplan](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/StructureDefinition-usdf-CoveragePlan.html)

### <a name="sample-rest-file"></a>Beispiel-REST-Datei

Um die Erstellung dieser Suchparameter und Profile zu unterstützen, verfügen wir auf der Open-Source-Website über die HTTP-Beispieldatei [Da Formulary,](https://github.com/microsoft/fhir-server/blob/main/docs/rest/DaVinciFormulary/DaVinciFormulary.http) die alle oben beschriebenen Schritte in einer einzelnen Datei enthält. Nachdem Sie alle erforderlichen Profile und Suchparameter hochgeladen haben, können Sie den Funktionshinweistest in Touchstone ausführen. Sie sollten eine erfolgreiche Ausführung erhalten:

:::image type="content" source="media/cms-tutorials/davinci-test-script-execution.png" alt-text="DaVinci-Testskriptausführung.":::

## <a name="touchstone-query-test"></a>Touchstone–Abfragetest

Der zweite Test ist die [Abfragefunktion](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/Formulary/01-Query&activeOnly=false&contentEntry=TEST_SCRIPTS). Dieser Test überprüft, ob Sie mithilfe verschiedener Parameter nach bestimmten Ressourcen und suchen `CoveragePlan` `Drug` können. Der beste Pfad wäre, mit Ressourcen zu testen, die Sie bereits in Ihrer Datenbank haben, aber wir haben auch die [DaVinciFormulary_Sample_Resources](https://github.com/microsoft/fhir-server/blob/main/docs/rest/DaVinciFormulary/DaVinciFormulary_Sample_Resources.http) HTTP-Datei mit Beispielressourcen verfügbar, die aus den Beispielen in der IG abgerufen wurden, die Sie zum Erstellen der Ressourcen und zum Testen verwenden können.

:::image type="content" source="media/cms-tutorials/davinci-test-execution-results.png" alt-text="DaVinci- Testausführungsergebnisse.":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir schritt für Schritt erfahren, wie Sie das Formular da Payer Data Exchange US Formulay in Touchstone übergeben. Als Nächstes erfahren Sie, wie Sie den DaEment PDex-Implementierungsleitfaden in Touchstone testen.

>[!div class="nextstepaction"]
>[DaDex PDex](davinci-pdex-tutorial.md)