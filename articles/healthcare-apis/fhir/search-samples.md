---
title: Suchbeispiele für Azure API for FHIR
description: Suchen mit verschiedenen Suchparametern, Modifizierern und anderen FHIR-Suchtools
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 04/20/2021
ms.author: ginle
ms.openlocfilehash: edbbfe81b4926689e0a431a28ac91e9f07e8e944
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108322547"
---
# <a name="fhir-search-examples"></a>FHIR-Suchbeispiele

Im Folgenden finden Sie einige Beispiele für die Verwendung von FHIR-Suchvorgängen, einschließlich Suchparametern und -modifizierern, Ketten- und Umgekehrt-Kettensuche, zusammengesetzte Suche, Anzeigen des nächsten Eintragssatzes für Suchergebnisse und Suchen mit einer POST-Anforderung. Weitere Informationen zur Suche finden Sie unter [Übersicht über die FHIR-Suche.](overview-of-search.md)
   
## <a name="search-result-parameters"></a>Suchergebnisparameter

### <a name="_include"></a>_include

> [!NOTE]
> **_include** und **_revinclude** sind auf 100 Elemente beschränkt.

`_include` sucht ressourcenübergreifend nach den Ressourcen, die den angegebenen Parameter der Ressource enthalten. Beispielsweise können Sie ressourcenübergreifend suchen, `MedicationRequest` um nur diejenigen zu finden, die Informationen zu den Patienten für einen bestimmten Patienten enthalten, was dem Parameter entspricht: `reference` `patient`

```rest
 GET [your-fhir-server]/MedicationRequest?_include=MedicationRequest:patient

```

### <a name="_revinclude"></a>_revinclude

`_revinclude` ist eine zusätzliche Suche über , die `_include` über die Ressourcen hinweg sucht, die auf die Suchergebnisse von `_include` verweisen. Beispielsweise können Sie `MedicationRequest` Ressourcen durchsuchen. Suchen Sie für jede zurückgegebene Ressource nach `DetectedIssue` Ressourcen, die die klinischen Probleme mit dem `patient` anzeigen:

```rest
GET [your-fhir-server]/MedicationRequest?_revinclude=DetectedIssue:patient

```
### <a name="_elements"></a>_elements

`_elements` schränkt das Suchergebnis auf eine Teilmenge von Feldern ein, um die Antwortgröße zu reduzieren, indem unnötige Daten weggelassen werden. Der -Parameter akzeptiert eine durch Kommas getrennte Liste von Basiselementen:

```rest
GET [your-fhir-server]/Patient?_elements=identifier,active

```

In dieser Anforderung erhalten Sie ein Bündel von Patienten, aber jede Ressource enthält nur die Bezeichner und den aktiven Status des Patienten. Ressourcen in dieser zurückgegebenen Antwort enthalten den `meta.tag` Wert `SUBSETTED` , um anzugeben, dass es sich um einen unvollständigen Satz von Ergebnissen handelt.

## <a name="search-modifiers"></a>Suchmodifizierer

### <a name="not"></a>:not

`:not` ermöglicht es Ihnen, Ressourcen zu finden, bei denen ein Attribut nicht true ist. Sie können z. B. nach Patienten suchen, bei denen das Geschlecht nicht "female" ist:

```rest
GET [your-fhir-server]/Patient?gender:not=female

```

Als Rückgabewert erhalten Sie alle Patienteneinträge, bei denen das Geschlecht nicht "female" ist, einschließlich leerer Werte (Einträge, die ohne Geschlecht angegeben sind). Dies ist anders als bei der Suche nach Patienten, bei denen das Geschlecht ein Geschlecht ist, da dies die Einträge ohne ein bestimmtes Geschlecht nicht enthalten würde.

### <a name="missing"></a>:missing

`:missing` gibt alle Ressourcen zurück, die keinen Wert für das angegebene Element haben, wenn der Wert ist, und gibt alle Ressourcen zurück, die das angegebene Element enthalten, wenn der Wert `true` `false` ist. Für einfache Datentypelemente wird für alle Ressourcen übereinstimmen, in denen das Element mit Erweiterungen vorhanden `:missing=true` ist, aber über einen leeren Wert verfügt. Wenn Sie z. B. alle Ressourcen suchen möchten, bei denen Informationen zum Geburtsdatum `Patient` fehlen, können Sie folgendes tun:

```rest
GET [your-fhir-server]/Patient?birthDate:missing=true

```

### <a name="exact"></a>:exact
`:exact` wird für Parameter verwendet und gibt Ergebnisse zurück, die genau mit dem Parameter übereinstimmen, z. B. in der `string` Casing- und Zeichenkonkettung.

```rest
GET [your-fhir-server]/Patient?name:exact=Jon

```

Diese Anforderung gibt `Patient` Ressourcen zurück, deren Name exakt mit identisch `Jon` ist. Wenn die Ressource Patienten mit Namen wie oder enthält, würde die Suche die Ressource ignorieren und überspringen, da sie nicht genau mit dem `Jonathan` `joN` angegebenen Wert übereinstimmen würde.

### <a name="contains"></a>:contains
`:contains` wird für Parameter verwendet und sucht nach Ressourcen mit partiellen Übereinstimmungen des angegebenen Werts an einer beliebigen Stelle in der Zeichenfolge innerhalb des `string` durchsuchten Felds. `contains` berücksichtigt die Groß-/Kleinschreibung nicht und ermöglicht die Zeichenkonkettung. Beispiel:

```rest
GET [your-fhir-server]/Patient?address:contains=Meadow

```

Diese Anforderung gibt alle Ressourcen mit `Patient` Feldern zurück, die Werte mit der `address` Zeichenfolge "Zeichenfolge" enthalten. Dies bedeutet, dass Adressen, die Werte wie "1000" oder "59 St" enthalten, als Suchergebnisse zurückgegeben werden können.

## <a name="chained-search"></a>Verkettete Suche 

Um eine Reihe von Suchvorgängen durchzuführen, die mehrere Verweisparameter abdecken, können Sie die Reihe von Verweisparametern "verketten", indem Sie sie mithilfe eines Zeitraums nach dem anderen an die Serveranforderung `.` anfügen. Wenn Sie beispielsweise alle Ressourcen mit einem Verweis auf eine Ressource anzeigen `DiagnosticReport` `subject` `Patient` möchten, die einen bestimmten `name` enthält:  

```rest
 GET [your-fhir-server]/DiagnosticReport?subject:Patient.name=Sarah

```

Diese Anforderung gibt alle Ressourcen mit dem Patiententhema namens "Sarah" zurück. Der `.` Zeitraum, nach dem das Feld `Patient` die verkettete Suche für den Verweisparameter des `subject` Parameters ausführt.

Eine weitere häufige Verwendung der verketteten Suche besteht darin, alle Vorkommen für einen bestimmten Patienten zu finden. `Patient`s verfügen häufig über ein oder mehrere `Encounter` s mit einem Betreff. So suchen Sie nach allen `Encounter` Ressourcen für eine mit dem `Patient` bereitgestellten `id` :

```rest
GET [your-fhir-server]/Encounter?subject=Patient/78a14cbe-8968-49fd-a231-d43e6619399f

```

Mithilfe der verketteten Suche können Sie alle Ressourcen finden, die `Encounter` mit einer bestimmten `Patient` Information übereinstimmen, z. B. `birthdate` :

```rest
GET [your-fhir-server]/Encounter?subject:Patient.birthDate=1987-02-20

```

Dies würde nicht nur das Durchsuchen von `Encounter` Ressourcen für einen einzelnen Patienten ermöglichen, sondern auch für alle Patienten mit dem angegebenen Wert für das Geburtsdatum. 

Darüber hinaus kann die verkettete Suche in einer Anforderung mehrmals ausgeführt werden, indem das Symbol verwendet wird, mit dem Sie in einer Anforderung nach `&` mehreren Bedingungen suchen können. In solchen Fällen sucht die verkettete Suche "unabhängig" nach jedem Parameter, anstatt nach Bedingungen zu suchen, die nur alle Bedingungen gleichzeitig erfüllen. Es handelt sich um einen OR-Vorgang, nicht um einen AND-Vorgang. Wenn Sie beispielsweise alle Patienten abrufen möchten, die einen Patienten mit einem bestimmten Namen oder aus einem bestimmten Zustand hatten:

```rest
GET [your-fhir-server]/Patient?general-practitioner.name=Sarah&general-practitioner.address-state=WA

```

Dadurch werden alle `Patient` Ressourcen zurückgegeben, die "Sarah" als `generalPractitioner` aufweisen, und alle `Patient` `generalPractitioner` Ressourcen, die über die Adresse mit der Zustands-WA verfügen. Anders ausgedrückt: Sie können Sowohl Sarah aus dem Bundesstaat NY als auch Bill aus der Bundesstaat-WA als zurückgegebene Ergebnisse haben. Die verkettete Suche erfordert nicht, dass alle Bedingungen erfüllt sind, und wird einzeln anhand des Parameters ausgewertet.

Informationen zu Szenarien, in denen die Suche ein AND-Vorgang sein muss, der alle Bedingungen als Gruppe abdeckt, finden Sie im beispiel für die **zusammengesetzte Suche** weiter unten.

## <a name="reverse-chain-search"></a>Umgekehrte Kettensuche

Mit der Kettensuche können Sie basierend auf den Eigenschaften der Ressourcen, auf die sie verweisen, nach Ressourcen suchen. Mithilfe der umgekehrten Kettensuche können Sie dies umgekehrt tun. Sie können anhand der Eigenschaften von Ressourcen, die auf sie verweisen, mithilfe des Parameters nach Ressourcen `_has` suchen. Beispielsweise verfügt die `Observation` Ressource über einen Suchparameter, `patient` der auf eine Patient-Ressource verweist. So suchen Sie alle Patient-Ressourcen, auf die von mit `Observation` einem bestimmten verwiesen `code` wird:

```rest
GET [base]/Patient?_has:Observation:patient:code=527

```

Diese Anforderung gibt Patient-Ressourcen zurück, auf die mit `Observation` dem Code verwiesen `527` wird. 

Darüber hinaus kann die umgekehrte Kettensuche eine rekursive Struktur haben. Wenn Sie beispielsweise nach allen Patienten suchen möchten, bei denen die Beobachtung über ein Überwachungsereignis eines bestimmten Benutzers `Observation` `janedoe` verfügt, können Sie folgende Aufgaben unternehmen:

```rest
GET [base]/Patient?_has:Observation:patient:_has:AuditEvent:entity:user=janedoe

``` 

> [!NOTE]
> In der Azure API for FHIR und dem von Cosmos unterstützten Open-Source-FHIR-Server ist die verkettete Suche und die umgekehrte verkettete Suche eine MVP-Implementierung. Um eine verkettete Suche auf Cosmos DB zu ermöglichen, durchsucht die Implementierung den Suchausdruck und gibt Unterabfragen aus, um die übereinstimmenden Ressourcen zu beheben. Dies erfolgt für jede Ebene des Ausdrucks. Wenn eine Abfrage mehr als 100 Ergebnisse zurückgibt, wird ein Fehler ausgelöst. Standardmäßig befindet sich die verkettete Suche hinter einem Featureflag. Verwenden Sie den Header x-ms-enable-chained-search: true, um die verkettete Suche für Cosmos DB zu verwenden.

## <a name="composite-search"></a>Zusammengesetzte Suche

Um nach Ressourcen zu suchen, die mehrere Bedingungen gleichzeitig erfüllen, verwenden Sie die zusammengesetzte Suche, die eine Sequenz einzelner Parameterwerte mit einem Symbol `$` verbindet. Das zurückgegebene Ergebnis wäre die Schnittmenge der Ressourcen, die mit allen Bedingungen übereinstimmen, die von den parametern für die verbundene Suche angegeben werden. Solche Suchparameter werden als zusammengesetzte Suchparameter bezeichnet und definieren einen neuen Parameter, der die mehreren Parameter in einer geschachtelten Struktur kombiniert. Wenn Sie z. B. alle Ressourcen suchen möchten, die einen wert kleiner oder `DiagnosticReport` `Observation` gleich 9,2 enthalten:

```rest
GET [your-fhir-server]/DiagnosticReport?result.code-value-quantity=2823-3$lt9.2

``` 

Diese Anforderung gibt die Komponente an, die einen Code von `2823-3` enthält, in diesem Fall wäre dies "füllium". Nach dem `$` Symbol wird der Bereich des Werts für die Komponente mit für `lt` "kleiner als oder gleich" und `9.2` für den Wertbereich angegeben. 

## <a name="search-the-next-entry-set"></a>Durchsuchen des nächsten Eintragssatzes

Die maximale Anzahl von Einträgen, die pro einzelner Suchabfrage zurückgegeben werden können, beträgt 1000. Möglicherweise verfügen Sie jedoch über mehr als 1.000 Einträge, die mit der Suchabfrage übereinstimmen, und Sie möchten möglicherweise den nächsten Satz von Einträgen nach den ersten 1000 zurückgegebenen Einträgen anzeigen. In diesem Fall verwenden Sie den Fortsetzungstokenwert `url` in wie im folgenden `searchset` `Bundle` Ergebnis:

```json
    "resourceType": "Bundle",
    "id": "98731cb7-3a39-46f3-8a72-afe945741bd9",
    "meta": {
        "lastUpdated": "2021-04-22T09:58:16.7823171+00:00"
    },
    "type": "searchset",
    "link": [
        {
            "relation": "next",
            "url": "[your-fhir-server]/Patient?_sort=_lastUpdated&ct=WzUxMDAxNzc1NzgzODc5MjAwODBd"
        },
        {
            "relation": "self",
            "url": "[your-fhir-server]/Patient?_sort=_lastUpdated"
        }
    ],

```

Außerdem würden Sie eine GET-Anforderung für die bereitgestellte URL im Feld `relation: next` durchführen:

```rest
GET [your-fhir-server]/Patient?_sort=_lastUpdated&ct=WzUxMDAxNzc1NzgzODc5MjAwODBd

```

Dadurch wird der nächste Satz von Einträgen für Das Suchergebnis zurückgegeben. `searchset`ist der vollständige Satz von Suchergebniseinträgen, und das `url` Fortsetzungstoken ist der Link, der vom Server bereitgestellt wird, damit Sie die Einträge abrufen können, die nicht im ersten Satz angezeigt werden, da die Maximale Anzahl von Einträgen für eine Suchabfrage eingeschränkt ist.

## <a name="search-using-post"></a>Suchen mit POST

Alle oben erwähnten Suchbeispiele haben `GET` Anforderungen verwendet. Sie können Suchvorgänge auch `POST` mithilfe von Anforderungen `_search` durchführen:

```rest
POST [your-fhir-server]/Patient/_search?_id=45

```

Diese Anforderung gibt alle `Patient` Ressourcen mit dem Wert `id` 45 zurück. Wie bei GET-Anforderungen bestimmt der Server, welche der Ressourcen die Bedingungen erfüllt, und gibt eine Bündelressource in der HTTP-Antwort zurück.

Ein weiteres Beispiel für die Suche mit POST, bei der die Abfrageparameter als Formulartext übermittelt werden, ist:

```rest
POST [your-fhir-server]/Patient/_search
content-type: application/x-www-form-urlencoded

name=John

```
## <a name="next-steps"></a>Nächste Schritte

>[!div class="nextstepaction"]
>[Übersicht über die FHIR-Suche](overview-of-search.md)