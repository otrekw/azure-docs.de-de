---
title: 'Tutorial: DaVinci-Formulary –Azure API for FHIR'
description: Dieses Tutorial führt Sie durch die Einrichtung der Azure API for FHIR, um die Touchstone-Tests mit dem Implementierungsleitfaden für DaVinci-Formulary zu bestehen.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: modillon
ms.date: 06/01/2021
ms.openlocfilehash: 22a4f42c0b63c42a50824301bcd9c056e7883a38
ms.sourcegitcommit: 832e92d3b81435c0aeb3d4edbe8f2c1f0aa8a46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111562763"
---
# <a name="davinci-drug-formulary"></a>DaVinci-Formulary

In diesem Tutorial erfahren Sie, wie Sie die Azure API for FHIR einrichten, um die [Touchstonetests](https://touchstone.aegis.net/touchstone/) für den [DaVinci Payer Data Exchange US-Formulary Implementation Guide zu bestehen.](http://hl7.org/fhir/us/Davinci-drug-formulary/)

## <a name="touchstone-capability-statement"></a>Touchstone capability statement (Touchstonefunktions-Anweisung)

Der erste Test, auf den wir uns konzentrieren, ist das Testen der Azure API for FHIR mit der [DaVinci-Anweisung zur Formulary-Funktion für Daseinsbeige.](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/Formulary/00-Capability&activeOnly=false&contentEntry=TEST_SCRIPTS) Wenn Sie diesen Test ohne Updates ausführen, kann der Test aufgrund fehlender Suchparameter und fehlender Profile nicht ausgeführt werden.

### <a name="define-search-parameters"></a>Definieren von Suchparametern

Im Rahmen der DaVinci-Formulary IG müssen Sie [](how-to-do-custom-search.md) drei neue Suchparameter für die Ressource FormularyDrug definieren. Alle drei werden in der Capability-Anweisung getestet.

-   [Einsentier](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugTier.json.html)

-   [Veralten](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugPlan.json.html)

-   [Name des Vornamens](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/SearchParameter-DrugName.json.html)

Die restlichen Suchparameter, die für die DaVinci-Ig für Daseinsuchformulare erforderlich sind, werden durch die Basisspezifikation definiert und sind bereits in der Azure API for FHIR ohne weitere Updates verfügbar.

### <a name="store-profiles"></a>Speichern von Profilen

Außerhalb der Definition von Suchparametern müssen Sie nur noch die erforderlichen Profile laden, um diesen [Test zu bestehen.](validation-against-profiles.md) Es gibt zwei Profile, die als Teil der DaVinci-Ig für Daseinsbeteiligungsformulare verwendet werden.

-   [Formulary-Abhängige](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/StructureDefinition-usdf-FormularyDrug.html)

-   [Formulary Coverage-Plan](http://hl7.org/fhir/us/davinci-drug-formulary/STU1.0.1/StructureDefinition-usdf-CoveragePlan.html)

### <a name="sample-rest-file"></a>Beispiel-REST-Datei

Zur Unterstützung bei der Erstellung dieser Suchparameter und Profile verfügen wir über die HTTP-Beispieldatei [DaVinci Formulary](https://github.com/microsoft/fhir-server/blob/main/docs/rest/DaVinciFormulary/DaVinciFormulary.http) auf der Open-Source-Website, die alle oben beschriebenen Schritte in einer einzelnen Datei enthält. Nachdem Sie alle erforderlichen Profile und Suchparameter hochgeladen haben, können Sie den Funktionsauszugstest in Touchstone ausführen. Sie sollten eine erfolgreiche Ausführung erhalten:

:::image type="content" source="media/cms-tutorials/davinci-test-script-execution.png" alt-text="DaVinci-Testskriptausführung.":::

## <a name="touchstone-query-test"></a>Touchstone-Abfragetest

Der zweite Test ist die [Abfragefunktionen](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/DaVinci/FHIR4-0-1-Test/PDEX/Formulary/01-Query&activeOnly=false&contentEntry=TEST_SCRIPTS). Mit diesem Test wird überprüft, ob Sie mithilfe verschiedener Parameter nach bestimmten Ressourcen für Abdeckungsplan und -ressourcen suchen können. Der beste Weg wäre, anhand von Ressourcen zu testen, die bereits in Ihrer Datenbank vorhanden sind, aber wir verfügen auch über die [DaVinciFormulary_Sample_Resources-HTTP-Datei](https://github.com/microsoft/fhir-server/blob/main/docs/rest/DaVinciFormulary/DaVinciFormulary_Sample_Resources.http) mit Beispielressourcen, die aus den Beispielen in der IG entnommen wurden und mit denen Sie die Ressourcen erstellen und testen können.

:::image type="content" source="media/cms-tutorials/davinci-test-execution-results.png" alt-text="DaVinci-Testausführungsergebnisse.":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir erfahren, wie Sie das DaVinci Payer Data Exchange US-Formulary in Touchstone übergeben. Als Nächstes erfahren Sie, wie Sie das Da Guides PDex-Implementierungshandbuch in Touchstone testen.

>[!div class="nextstepaction"]
>[DaVinci PDex](davinci-pdex-tutorial.md)