---
title: 'Azure Attestation: Grammatik für Anspruchsregeln'
description: Details zu Azure Attestation-Richtlinienansprüchen und -Anspruchsregeln.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: a0f3e517e60037de6456bc3a549e072e58e2fa67
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89244737"
---
# <a name="claim-and-claim-rules"></a>Anspruch und Anspruchsregeln

Um die Grammatik von Anspruchsregeln zu verstehen, müssen Sie sich zunächst mit Nachweisrichtlinienansprüchen vertraut machen.

## <a name="claim"></a>Anspruch

Bei einem Anspruch handelt es sich um eine Gruppe von Eigenschaften zur Bereitstellung relevanter Informationen. Für Azure Attestation enthält ein Anspruch folgende Eigenschaften:

- **Typ**: Ein Zeichenfolgenwert, der die Art des Anspruchs darstellt.
- **value:** Ein boolescher Wert, ein ganzzahliger Wert oder ein Zeichenfolgenwert, der den Wert des Anspruchs darstellt.
- **valueType**: Der Datentyp der in der Eigenschaft „value“ gespeicherten Informationen. Unterstützte Typen sind „String“ (Zeichenfolge), „Integer“ (ganze Zahl) und „Boolean (boolescher Wert). Ohne Angabe wird standardmäßig „String“ (Zeichenfolge) verwendet.
- **issuer**: Informationen zum Aussteller des Anspruchs. Mögliche Ausstellertypen:
  - **AttestationService**: Bestimmte Ansprüche werden dem Richtlinienersteller von Azure Attestation zur Verfügung gestellt und können von diesem verwendet werden, um die entsprechende Richtlinie zu erstellen.
  - **AttestationPolicy**: Durch die (vom Administrator definierte) Richtlinie können im Rahmen der Verarbeitung Ansprüche zu eingehenden Beweisen hinzugefügt werden. In diesem Fall wird der Aussteller auf „AttestationPolicy“ festgelegt.
  - **CustomClaim**: Der Testator (Client) kann dem Nachweisbeweis zusätzliche Ansprüche hinzufügen. In diesem Fall wird der Aussteller auf „CustomClaim“ festgelegt.

Ohne Angabe wird der Standardwert „CustomClaim“ verwendet.

## <a name="claim-rule"></a>Anspruchsregel

Der eingehende Anspruchssatz wird von der Richtlinienengine zur Berechnung des Nachweisergebnisses verwendet. Bei einer Anspruchsregel handelt es sich um eine Gruppe von Bedingungen, die dazu dienen, die eingehenden Ansprüche zu überprüfen und die definierte Aktion auszuführen.

```
Conditions list => Action (Claim)
```

Die Azure Attestation-Auswertung einer Anspruchsregel umfasst folgende Schritte:

- Ist keine Bedingungsliste vorhanden, wird die Aktion mit dem angegebenen Anspruch ausgeführt. 
- Andernfalls werden die Bedingungen aus der Bedingungsliste ausgewertet.
- Ergibt die Auswertung der Bedingungsliste „false“, wird der Vorgang beendet. Andernfalls wird der Vorgang fortgesetzt.

Die Bedingungen in einer Anspruchsregel werden verwendet, um zu bestimmen, ob die Aktion ausgeführt werden muss. Bei der Bedingungsliste handelt es sich um eine Reihe von Bedingungen, die jeweils durch den Operator „&&“ getrennt sind.

Die Bedingungsliste ist wie folgt strukturiert:

```
Condition && Condition && ...
```

Die Bedingung ist wie folgt strukturiert:

```
Identifier:[ClaimPropertyCondition, ClaimPropertyCondition,…]
```

Die Bedingung setzt sich aus einzelnen Bedingungen für verschiedene Eigenschaften eines Anspruchs zusammen. Eine Bedingung kann über einen optionalen Bezeichner verfügen, mit dem auf die Ansprüche verwiesen werden kann, die die Bedingung erfüllen. Dieser Verweis kann in den anderen Bedingungen oder in der Aktion der gleichen Regel verwendet werden.

Beispiel:

```
F1:[type=="OSName" , issuer=="CustomClaim"] && 
[type=="OSName" , issuer=="AttestationService", value== F1.value ] 
=> issueproperty(type="report_validity_in_minutes", value=1440);

F1:[type=="OSName" , issuer=="CustomClaim"] && 
C2:[type=="OSName" , issuer=="AttestationService", value== F1.value ] 
=> issue(claim = C2);
```

Folgende Operatoren können zur Überprüfung von Bedingungen verwendet werden:

| Werttyp | Unterstützte Vorgänge |
|--|--|
| Integer | == (gleich), \!= (ungleich), <= (kleiner als oder gleich), < (kleiner als), >= (größer als oder gleich), > (größer als) |
| String | == (gleich), \!= (ungleich) |
| Boolean | == (gleich), \!= (ungleich) |

Auswertung der Bedingungsliste:

- Wenn der Operator „&&“ vorhanden ist, impliziert dies, dass die Auswertung der Bedingungsliste nur dann „true“ ergibt, wenn alle Bedingungen erfüllt sind.
- Eine Bedingung stellt Filterkriterien für die Ansprüche dar. Die Auswertung der Bedingung selbst ergibt „true“, wenn mindestens ein Anspruch gefunden wird, der die Bedingung erfüllt.
- Ein Anspruch erfüllt das durch die Bedingung dargestellte Filterkriterium, wenn jede seiner Eigenschaften die entsprechenden Anspruchseigenschaftsbedingungen aus der Bedingung erfüllt.  

Im Anschluss werden die Aktionen beschrieben, die in einer Richtlinie zulässig sind.

| Aktionsverb | BESCHREIBUNG | Relevante Richtlinienabschnitte |
|--|--|--|
| permit() | Der eingehende Anspruchssatz kann zur Berechnung von **issuancerules** verwendet werden. Akzeptiert keinen Anspruch als Parameter. | **authorizationrules** |
| deny() | Der eingehende Anspruchssatz darf nicht zur Berechnung von **issuancerules** verwendet werden. Akzeptiert keinen Anspruch als Parameter. | **authorizationrules** |
| add(Anspruch) | Fügt den Anspruch dem eingehenden Anspruchssatz hinzu. Jeder Anspruch, der dem eingehenden Anspruchssatz hinzugefügt wird, steht für nachfolgende Anspruchsregeln zur Verfügung. |**authorizationrules**, **issuancerules** |
| issue(Anspruch) | Fügt den Anspruch dem ein- und ausgehenden Anspruchssatz hinzu. | **issuancerules** |
| issueproperty(Anspruch) | Fügt den Anspruch dem eingehenden Anspruchssatz und dem Eigenschaftsanspruchssatz hinzu. | **issuancerules**

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Signieren einer Nachweisrichtlinie](author-sign-policy.md)
- [Einrichten von Azure Attestation mithilfe von PowerShell](quickstart-powershell.md)

