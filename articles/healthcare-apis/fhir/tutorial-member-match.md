---
title: 'Tutorial: $member-Match-Vorgang – Azure API for FHIR'
description: In diesem Tutorial wird der $member-Match-Vorgang vorgestellt, der als Teil von Da Health Record Exchange (HRex) definiert ist.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 06/01/2021
ms.openlocfilehash: a6d24d0ebf91e2ae6992f6b101b8be0d4a0d4201
ms.sourcegitcommit: 832e92d3b81435c0aeb3d4edbe8f2c1f0aa8a46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111562728"
---
# <a name="member-match-operation"></a>$member-Match-Vorgang

[$member-match](http://hl7.org/fhir/us/davinci-hrex/2020Sep/OperationDefinition-member-match.html) ist ein Vorgang, der als Teil des Da Health Record Exchange (HRex) definiert ist. In diesem Leitfaden erfahren Sie, was $member-match ist und wie sie verwendet wird.

## <a name="overview-of-member-match"></a>Übersicht über $member Übereinstimmung

Der $member-Match-Vorgang wurde erstellt, um den Datenaustausch zwischen Zahlenden zu unterstützen, indem es einem neuen Zahlenden ermöglicht wurde, einen eindeutigen Bezeichner für einen Patienten vom vorherigen Zahlenden des Patienten abzurufen. Der $member-Match-Vorgang erfordert, dass drei Informationen im Text der Anforderung übergeben werden:

* Demografische Daten von Patienten

* Die alten Abdeckungsinformationen

* Die neuen Abdeckungsinformationen (basierend auf unserer Implementierung nicht erforderlich)

Nachdem die Daten übergeben wurden, überprüft der Azure API for FHIR, ob er einen Patienten finden kann, der genau den übergebenen demografischen Daten mit den übergebenen alten Abdeckungsinformationen entspricht. Wenn ein Ergebnis gefunden wird, handelt es sich bei der Antwort um ein Paket mit den ursprünglichen Patientendaten sowie einem neuen Bezeichner, der vom alten Zahlenden hinzugefügt wurde, und den alten Abdeckungsinformationen.

> [!NOTE]
> Die Spezifikation beschreibt das Übergeben und Zurückgeben der neuen Abdeckungsinformationen. Wir haben uns entschieden, diese Daten auszulassen, um die Ergebnisse kleiner zu halten.

## <a name="example-of-member-match"></a>Beispiel für $member-Match

Verwenden Sie den folgenden Aufruf, um $member-match zu verwenden:

`POST {{fhirurl}}/Patient/$member-match`

Sie müssen eine Parameterressource in den Körper einschließen, die den Patienten, die alte Abdeckung und die neue Abdeckung enthält. Eine JSON-Darstellung finden Sie unter [$member-match-Beispielanforderung.](http://hl7.org/fhir/us/davinci-hrex/2020Sep/Parameters-member-match-in.json.html)

Wenn eine einzelne Übereinstimmung gefunden wird, erhalten Sie eine 200-Antwort mit einem weiteren hinzugefügten Bezeichner:

:::image type="content" source="media/cms-tutorials/two-hundred-response.png" alt-text="200 Hundert Antwortcode.":::

Wenn die $member-Übereinstimmung keine eindeutige Übereinstimmung finden kann, erhalten Sie eine 422-Antwort mit einem Fehlercode.

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden haben Sie die $member-Match-Operation kennengelernt. Als Nächstes erfahren Sie mehr über das Testen der Da Payer Data Exchange IG in Touchstone, für die der $member-Match-Vorgang erforderlich ist.

>[!div class="nextstepaction"]
>[DaVinci PDex](davinci-pdex-tutorial.md)