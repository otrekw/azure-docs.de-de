---
title: Grundlagen des Schlüssel-Wert-Speichers von Azure App Configuration
description: Erfahren Sie, wie Konfigurationsdaten in Azure App Configuration gespeichert werden.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/19/2020
ms.openlocfilehash: 0b83a35d912c97ae25bc2d69d076e8eae8ca490f
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77523603"
---
# <a name="keys-and-values"></a>Schlüssel und Werte

Azure App Configuration speichert Konfigurationsdaten als Schlüssel-Wert-Paare. Schlüssel-Wert-Paare sind eine einfache und flexible Darstellung von Anwendungseinstellungen, die von Entwicklern verwendet werden.

## <a name="keys"></a>Schlüssel

Schlüssel dienen als Bezeichner für Schlüssel-Wert-Paare und werden zum Speichern und Abrufen von entsprechenden Werten verwendet. Es ist eine häufige Vorgehensweise, Schlüssel mit einem Trennzeichen (z. B. `/` oder `:`) in einem hierarchischen Namespace zu organisieren. Verwenden Sie hierbei eine Konvention, die für Ihre Anwendung am besten geeignet ist. Bei App Configuration werden Schlüssel als Ganzes behandelt. Sie werden nicht analysiert, um zu ermitteln, wie ihre Namen strukturiert sind, und es werden auch keine Regeln dafür erzwungen.

Hier sind zwei Beispiele für in eine Hierarchie strukturierte Schlüsselnamen:

* Basierend auf Komponentendiensten

        AppName:Service1:ApiEndpoint
        AppName:Service2:ApiEndpoint

* Basierend auf Bereitstellungsregionen

        AppName:Region1:DbEndpoint
        AppName:Region2:DbEndpoint

Bei Verwendung von Konfigurationsdaten in Anwendungsframeworks sind möglicherweise bestimmte Benennungsschemas für Schlüsselwerte vorgegeben. Beispielsweise definiert das Spring Cloud-Framework von Java `Environment`-Ressourcen, die Einstellungen für eine Spring-Anwendung bereitstellen.  Diese werden durch Variablen parametrisiert, die *Anwendungsname* und *Profil* enthalten. Schlüssel für Spring Cloud-bezogene Konfigurationsdaten beginnen normalerweise mit diesen beiden Elementen, die durch ein Trennzeichen getrennt sind.

Für in App Configuration gespeicherte Schlüssel wird die Groß-/Kleinschreibung beachtet, und es handelt sich um Unicode-basierte Zeichenfolgen. *app1* und *App1* sind in einem App Configuration-Speicher zwei unterschiedliche Schlüssel. Beachten Sie dies, wenn Sie in einer Anwendung Konfigurationseinstellungen verwenden, da für Konfigurationsschlüssel in einigen Frameworks die Groß-/Kleinschreibung nicht beachtet wird. Schlüssel sollten nicht durch Groß-/Kleinschreibung unterschieden werden.

Sie können in Schlüsselnamen beliebige Unicodezeichen verwenden, mit Ausnahme von `*`, `,` und `\`.  Wenn Sie eines dieser reservierten Zeichen einfügen müssen, versehen Sie es wie folgt mit Escapezeichen: `\{Reserved Character}`. 

Für ein Schlüssel-Wert-Paar gilt ein kombiniertes Größenlimit von 10 KB. Dieses Limit umfasst alle Zeichen des Schlüssels, seinen Wert sowie alle zugeordneten optionalen Attribute. Innerhalb dieses Limits können Sie für Schlüssel viele hierarchische Ebenen verwenden.

### <a name="design-key-namespaces"></a>Entwerfen von Schlüsselnamespaces

Es gibt zwei allgemeine Vorgehensweisen beim Benennen von Schlüsseln, die für Konfigurationsdaten verwendet werden: flach und hierarchisch. Aus Sicht der Anwendungsnutzung ähneln sich diese Vorgehensweisen, aber die hierarchische Benennung bietet eine Reihe von Vorteilen:

* Einfachere Lesbarkeit. Trennzeichen in einem hierarchischen Schlüsselnamen fungieren in einem Satz als Leerstellen. Sie bieten außerdem natürliche Unterbrechungen zwischen Wörtern.
* Einfachere Verwaltung. Eine Schlüsselnamenhierarchie umfasst logische Gruppen mit Konfigurationsdaten.
* Einfachere Nutzung. Es ist einfacher, eine Abfrage zu schreiben, bei der sich für Schlüssel in einer hierarchischen Struktur Musterübereinstimmungen ergeben und nur ein Teil der Konfigurationsdaten abgerufen wird. Außerdem verfügen viele neuere Programmierframeworks über native Unterstützung für hierarchische Konfigurationsdaten, damit Ihre Anwendung spezifische Konfigurationssätze nutzen kann.

Sie können Schlüssel in App Configuration auf viele verschiedene Arten hierarchisch organisieren. Stellen Sie sich diese Schlüssel als [URIs](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) vor. Jeder hierarchische Schlüssel ist ein *Ressourcenpfad* mit einer oder mehreren Komponenten, die durch Trennzeichen verbunden sind. Wählen Sie aus, welches Zeichen Sie basierend auf den Anforderungen Ihrer Anwendung, Ihrer Programmiersprache oder Ihrem Framework verwenden möchten. Verwenden Sie für unterschiedliche Schlüssel verschiedene Trennzeichen in App Configuration.

### <a name="label-keys"></a>Bezeichnen von Schlüsseln

Schlüsselwerte in App Configuration können optional über das Attribut „label“ (Bezeichnung) verfügen. Bezeichnungen werden genutzt, um zwischen Schlüsselwerten mit demselben Schlüssel zu unterscheiden. Ein Schlüssel *app1* mit den Bezeichnungen *A* und *B* steht in einem App Configuration-Speicher für zwei separate Schlüssel. Standardmäßig weist ein Schlüsselwert keine Bezeichnung auf. Um explizit auf einen Schlüsselwert ohne eine Bezeichnung zu verweisen, verwenden Sie `\0` (URL-codiert als `%00`).

Eine Bezeichnung ist praktisch zum Erstellen von Varianten eines Schlüssels. Mit Bezeichnungen werden häufig mehrere Umgebungen für den gleichen Schlüssel angegeben:

    Key = AppName:DbEndpoint & Label = Test
    Key = AppName:DbEndpoint & Label = Staging
    Key = AppName:DbEndpoint & Label = Production

### <a name="version-key-values"></a>Versionsverwaltung von Schlüsselwerten

App Configuration versieht Schlüsselwerte nicht automatisch mit Versionsangaben. Verwenden Sie Bezeichnungen als Möglichkeit zum Erstellen von mehreren Versionen eines Schlüsselwerts. Beispielsweise können Sie eine Zahl einer Anwendungsversion oder eine Git-Commit-ID in Bezeichnungen eingeben, um Schlüsselwerte zu identifizieren, die einem bestimmten Softwarebuild zugeordnet sind.

Sie können in Bezeichnungen beliebige Unicodezeichen verwenden, mit Ausnahme von `*`, `,` und `\`. Diese Zeichen sind reserviert. Um ein reserviertes Zeichen einzufügen, müssen Sie es wie folgt mit Escapezeichen versehen: `\{Reserved Character}`.

### <a name="query-key-values"></a>Abfragen von Schlüsselwerten

Jeder Schlüsselwert wird anhand seines Schlüssels und einer Bezeichnung, die auch `null` lauten kann, eindeutig identifiziert. Sie fragen Schlüsselwerte aus einem App Configuration-Speicher ab, indem Sie ein Muster angeben. Der App Configuration-Speicher gibt alle Schlüsselwerte zurück, die mit dem Muster und den entsprechenden Werten und Attributen übereinstimmen. Verwenden Sie die folgenden Schlüsselmuster in REST-API-Aufrufen für App Configuration:

| Key | |
|---|---|
| `key` wird weggelassen oder lautet `key=*` | Übereinstimmung mit allen Schlüsseln |
| `key=abc` | Exakte Übereinstimmung mit dem Schlüsselnamen **abc** |
| `key=abc*` | Übereinstimmung mit Schlüsselnamen, die mit **abc** beginnen |
| `key=abc,xyz` | Übereinstimmung mit Schlüsselnamen mit **abc** oder **xyz**. Auf fünf CSVs beschränkt. |

Sie können auch die folgenden Bezeichnungsmuster verwenden:

| Bezeichnung | |
|---|---|
| `label` wird weggelassen oder lautet `label=*` | Übereinstimmung mit einer beliebigen Bezeichnung, einschließlich `null` |
| `label=%00` | Übereinstimmung mit der Bezeichnung `null` |
| `label=1.0.0` | Exakte Übereinstimmung mit der Bezeichnung **1.0.0** |
| `label=1.0.*` | Übereinstimmung mit Bezeichnungen, die mit **1.0.** beginnen |
| `label=%00,1.0.0` | Übereinstimmung mit den Bezeichnungen `null` oder **1.0.0**, auf fünf CSVs beschränkt |

## <a name="values"></a>Werte

Bei Werten, die Schlüsseln zugewiesen sind, handelt es sich ebenfalls um Unicode-Zeichenfolgen. Sie können alle Unicode-Zeichen für Werte verwenden. Jedem Wert ist ein optionaler, benutzerdefinierter Inhaltstyp zugeordnet. Verwenden Sie dieses Attribut, um Informationen zu einem Wert zu speichern, die Ihrer Anwendung die richtige Verarbeitung ermöglichen.

In einem App Configuration-Speicher gespeicherte Konfigurationsdaten werden im ruhenden Zustand und während der Übertragung verschlüsselt. Die Schlüssel werden im Ruhezustand nicht verschlüsselt. App Configuration ist keine Ersatzlösung für Azure Key Vault. Speichern Sie darin keine Anwendungsgeheimnisse.

## <a name="next-steps"></a>Nächste Schritte

* [Point-in-Time-Momentaufnahme](./concept-point-time-snapshot.md)  
* [Funktionsverwaltung](./concept-feature-management.md)  
