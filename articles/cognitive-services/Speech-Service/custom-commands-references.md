---
title: Konzepte und Definitionen für benutzerdefinierte Befehle – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie mehr über Konzepte und Definitionen für Anwendungen für benutzerdefinierte Befehle.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 5a5e3590e0e184633760f080dfd3402ed75bd48e
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307208"
---
# <a name="custom-commands-concepts-and-definitions"></a>Konzepte und Definitionen für benutzerdefinierte Befehle

Dieser Artikel dient als Referenz für Konzepte und Definitionen für Anwendungen für benutzerdefinierte Befehle.

## <a name="commands-configuration"></a>Befehlskonfiguration
Befehle sind die Grundbausteine einer Anwendung für benutzerdefinierte Befehle. Ein Befehl besteht aus einer Reihe von Konfigurationen, die zur Durchführung einer bestimmten, von einem Benutzer definierten Aufgabe erforderlich sind.

### <a name="example-sentences"></a>Beispielsätze
Beispieläußerungen sind die festgelegten Beispiele, die der Benutzer sagen kann, um einen bestimmten Befehl auszulösen. Beachten Sie, dass Sie nur eine Auswahl von Äußerungen und keine vollständige Liste bereitstellen müssen. 

### <a name="parameters"></a>Parameter
Parameter sind Informationen, die von den Befehlen zur Durchführung einer Aufgabe erforderlich sind. In komplexen Szenarien können Parameter auch zur Definition von Bedingungen verwendet werden, die benutzerdefinierte Aktionen auslösen.

### <a name="completion-rules"></a>Vervollständigungsregeln
Eine Reihe von Regeln, die ausgeführt werden, sobald der Befehl für die Ausführung bereit ist, d. h. wenn alle Bedingungen der Regeln erfüllt sind.

### <a name="interaction-rules"></a>Interaktionsregeln
Zusätzliche Regeln für den Umgang mit spezifischeren oder komplexeren Situationen. Sie können zusätzliche Validierungen hinzufügen oder erweiterte Features konfigurieren, z. B. Bestätigungen oder nur einen Schritt umfassende Korrekturen. Sie können auch eigene benutzerdefinierte Interaktionsregeln erstellen.

## <a name="parameters-configuration"></a>Parameterkonfiguration

Parameter sind Informationen, die von den Befehlen zur Durchführung einer Aufgabe erforderlich sind. In komplexen Szenarien können Parameter auch zur Definition von Bedingungen verwendet werden, die benutzerdefinierte Aktionen auslösen.

### <a name="name"></a>Name
Ein Parameter wird durch die Name-Eigenschaft identifiziert. Sie sollten einem Parameter immer einen beschreibenden Namen zuweisen. Ein Parameter kann auf verschiedene Abschnitte bezogen werden, z. B. beim Erstellen von Bedingungen, Sprachantworten oder anderen Aktionen.
 
### <a name="isglobal"></a>IsGlobal
Kontrollkästchen, das angibt, ob der Umfang dieses Parameters von allen Befehlen in der Anwendung gemeinsam genutzt wird. Wenn ein Parameter global ist, kann sein Wert potenziell aus jedem Befehlsumfang bereitgestellt werden, und sobald ein Wert zugewiesen ist, kann jeder der Befehle auf ihn verweisen. 

### <a name="required"></a>Erforderlich
Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter zur Befehlserfüllung/-vervollständigung erforderlich ist. Sie müssen Antworten konfigurieren, um den Benutzer aufzufordern, einen Wert bereitzustellen, wenn ein Parameter als erforderlich markiert ist.

### <a name="type"></a>type
Benutzerdefinierte Befehle unterstützen folgende Parametertypen:


* Datetime
* Gebiet
* Number
* String

Alle diese Parametertypen unterstützen die Konfiguration von Standardwerten, die Sie über das Portal konfigurieren können.

### <a name="configuration"></a>Konfiguration
Konfiguration ist eine Parametereigenschaft, die nur für den folgenden Typ definiert ist: Eine Zeichenfolge. Nachfolgend sind die unterstützten Werte aufgeführt
* Keine
* Vollständige Eingabe akzeptieren: Wenn diese Option aktiviert ist, akzeptiert der Parameter jede Eingabeäußerung. Dies ist nützlich, wenn der Benutzer einen Parameter mit der vollständigen Äußerung benötigt. z. B. Postanschriften.
* Vordefinierte Eingabewerte aus externem Katalog akzeptieren: Dient zur Konfiguration von Parametern, die eine Vielzahl von Werten annehmen können. z. B. Verkaufskatalog. In diesem Fall wird der Katalog auf einem externen Webendpunkt gehostet und kann unabhängig konfiguriert werden.
* Vordefinierte Eingabewerte aus internem Katalog akzeptieren: Dient zur Konfiguration von Parametern, die ein paar Werte annehmen können. In diesem Fall müssen Werte in Speech Studio konfiguriert werden.


### <a name="validation"></a>Überprüfen
Validierungen sind Konstrukte für bestimmte Parametertypen, mit denen Sie Einschränkungen für den Wert des Parameters konfigurieren können. Derzeit unterstützen benutzerdefinierte Befehle Validierungen für die folgenden Parametertypen:
* Datetime
* Number

## <a name="rules-configuration"></a>Konfiguration von Regeln
Eine Regel in benutzerdefinierten Befehlen wird durch einen Satz von **Bedingungen** definiert, die einen Satz von **Aktionen** ausführen, wenn sie erfüllt wurden. Eine Regel gestattet auch das Konfigurieren ihres **Status nach der Ausführung** und der **Erwartungen** für die nächste Runde.

### <a name="types"></a>Typen
Die folgenden Regelkategorien werden von benutzerdefinierten Befehlen unterstützt:
* Vervollständigungsregeln

    Liste der Regeln, die bei der Befehlserfüllung auszuführen sind. Alle in diesem Abschnitt konfigurierten Regeln, für die die Bedingungen erfüllt sind, werden ausgeführt.
    
* Interaktionsregeln

    Interaktionsregeln können verwendet werden, um zusätzliche benutzerdefinierte Prüfungen, Bestätigungen, einstufige Korrekturen zu konfigurieren oder um jede andere benutzerdefinierte Dialoglogik zu erfüllen. Diese Regeln werden bei jeder Rundenverarbeitung ausgewertet und können zum Auslösen von Vervollständigungsregeln verwendet werden.

Die verschiedenen als Teil einer Regel konfigurierten Aktionen werden in der Reihenfolge ausgeführt, in der sie im Erstellungsportal angezeigt werden.
    
### <a name="conditions"></a>Bedingungen
Eine Reihe von Bedingungen, die erfüllt sein müssen, damit eine Regel ausgeführt werden kann. Regelbedingungen können folgende Typen aufweisen:
* Parameterwert ist gleich: Der Wert des konfigurierten Parameters ist gleich einem bestimmten Wert.
* Kein Parameterwert: Konfigurierte Parameter dürfen keinen Wert aufweisen.
* Erforderliche Parameter: Der konfigurierte Parameter hat einen Wert.
* Alle erforderlichen Parameter: Alle Parameter, die als erforderlich markiert wurden, haben einen Wert.
* Aktualisierte Parameter: Ein oder mehrere Parameterwerte wurden als Ergebnis der Verarbeitung der aktuellen Eingabe (Äußerung/Aktivität) aktualisiert.
* Bestätigung war erfolgreich: Die Eingabeäußerung/-aktivität war eine erfolgreiche Bestätigung (Ja).
* Bestätigung wurde verweigert: Die Eingabeäußerung/-aktivität war eine erfolgreiche Bestätigung (Nein).
* Vorheriger Befehl muss aktualisiert werden: Diese Bedingung wird in Fällen verwendet, in denen Sie eine negierte Bestätigung zusammen mit einer Aktualisierung abfangen möchten. Dies wird im Hintergrund für den Fall konfiguriert, dass das Dialogmodul eine negative Bestätigung erkennt, bei der die Absicht dieselbe ist wie bei der vorherigen Runde, und der Benutzer mit einer Aktualisierung reagiert hat.

### <a name="actions"></a>Aktionen
* Sprachantwort senden: Senden Sie eine Sprachantwort an den Client zurück.
* Parameterwert aktualisieren: Aktualisiert den Wert eines Befehlsparameters auf einen bestimmten Wert.
* Parameterwert löschen: Löscht den Befehlsparameterwert.
* Webendpunkt aufrufen: Ruft einen Webendpunkt auf.
* Aktivität an Client senden: Senden Sie eine benutzerdefinierte Aktivität an den Client.

### <a name="expectations"></a>Erwartungen
Erwartungen werden verwendet, um Hinweise für die Verarbeitung der nächsten Benutzereingabe zu konfigurieren. Folgende Typen werden unterstützt:
* Bestätigung vom Benutzer erwarten: Gibt an, dass die Anwendung für die nächste Benutzereingabe eine Bestätigung (Ja/Nein) erwartet.
* Parametereingabe(n) vom Benutzer erwarten: Geben Sie einen oder mehrere Befehlsparameter an, die die Anwendung von der Benutzereingabe erwartet.

### <a name="post-execution-state"></a>Status nach der Ausführung
Dialogzustand nach der Verarbeitung der aktuellen Eingabe (Äußerung/Aktivität). Folgende Typen sind verfügbar:
* Befehl abgeschlossen: Wenn der Befehl abgeschlossen ist, werden keine weiteren Regeln des Befehls verarbeitet.
* Vervollständigungsregeln ausführen: Führen Sie alle gültigen Vervollständigungsregeln aus.
* Auf Benutzereingabe warten: Warten Sie auf die nächste Benutzereingabe.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen finden Sie in den Beispielen auf GitHub](https://aka.ms/speech/cc-samples)
