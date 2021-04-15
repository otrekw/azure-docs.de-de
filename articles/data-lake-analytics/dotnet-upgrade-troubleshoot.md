---
title: Behandeln von Fehlern bei U-SQL-Aufträgen in Azure Data Lake Analytics infolge des .NET Framework 4.7.2-Upgrades
description: Behandeln von Fehlern bei U-SQL-Aufträgen infolge des Upgrades auf .NET Framework 4.7.2.
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: troubleshooting
ms.date: 10/11/2019
ms.openlocfilehash: ab03ea8a88187289f5dce55f8a396a9d51346a3f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92217676"
---
# <a name="azure-data-lake-analytics-is-upgrading-to-the-net-framework-v472"></a>Azure Data Lake Analytics wird auf .NET Framework 4.7.2 aktualisiert

Die Azure Data Lake Analytics-Standardruntime wird von .NET Framework 4.5.2 auf .NET Framework 4.7.2 aktualisiert. Durch diese Änderung besteht ein geringes Risiko, dass Breaking Changes auftreten, wenn im U-SQL-Code benutzerdefinierte Assemblys und in diesen benutzerdefinierten Assemblys .NET-Bibliotheken verwendet werden.

Dieses Upgrade von .NET Framework 4.5.2 auf Version 4.7.2 bedeutet, dass als .NET Framework in einer U-SQL-Runtime (die Standardruntime) immer .NET Framework Version 4.7.2 bereitgestellt wird. Es gibt keine Möglichkeit, mehrere .NET Framework-Versionen parallel zu verwenden.

Nach dem Upgrade auf .NET Framework 4.7.2 wird der verwaltete Code des Systems als Version 4.7.2 ausgeführt. Vom Benutzer bereitgestellte Bibliotheken, z. B. die benutzerdefinierten U-SQL-Assemblys, werden in dem abwärtskompatiblen Modus ausgeführt, der für die Version geeignet ist, für die die Assembly generiert wurde.

- Wenn die Assembly-DLLs für Version 4.5.2 generiert werden, werden sie im bereitgestellten Framework als Bibliotheken der Version 4.5.2 mit Semantik der Version 4.5.2 (mit wenigen Ausnahmen) behandelt.
- Sie können nun benutzerdefinierte U-SQL-Assemblys verwenden, die Funktionen der Version 4.7.2 nutzen, wenn Sie .NET Framework 4.7.2 als Ziel verwenden.

Aufgrund dieses Upgrades auf .NET Framework 4.7.2 besteht die Möglichkeit, dass Breaking Changes für U-SQL-Aufträge eingeführt werden, die benutzerdefinierte .NET-Assemblys verwenden. Es wird empfohlen, dass Sie anhand des nachfolgenden Verfahrens Probleme mit der Abwärtskompatibilität überprüfen.

## <a name="how-to-check-for-backwards-compatibility-issues"></a>Überprüfen von Problemen mit der Abwärtskompatibilität

Überprüfen Sie das Potenzial von Problemen, die die Abwärtskompatibilität beeinträchtigen, indem Sie die .NET-Kompatibilitätsprüfungen für .NET-Code in den benutzerdefinierten U-SQL-Assemblys ausführen.

> [!Note]
> Das Tool erkennt keine tatsächlichen Breaking Changes. Es ermittelt lediglich .NET-APIs, die (bei bestimmten Eingaben) Probleme verursachen können. Wenn eine Meldung zu einem Problem angezeigt wird, ist der Code möglicherweise fehlerfrei, aber Sie sollten ihn genauer überprüfen.

1. Führen Sie die Überprüfung zur Abwärtskompatibilität für die .NET-DLLs auf eine der folgenden Weisen aus:
   1. Verwenden der Visual Studio-Erweiterung [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)
   1. Herunterladen und Verwenden des eigenständigen Tools ([GitHub dotnetapiport](https://github.com/microsoft/dotnet-apiport)). Anweisungen zum Ausführen des eigenständigen Tools finden Sie unter [GitHub dotnetapiport Breaking Changes](https://github.com/microsoft/dotnet-apiport/blob/dev/docs/HowTo/BreakingChanges.md).
   1. Bei 4.7.2. `read isRetargeting == True` werden mögliche Probleme im Zusammenhang mit der Kompatibilität mit 4.7.2 ermittelt.
2. Wenn in dem Tool angegeben wird, dass der Code unter Umständen durch eine der möglichen Abwärtsinkompatibilitäten beeinträchtigt ist (einige häufige Beispiele für Inkompatibilitäten sind nachfolgend aufgeführt), können Sie zusätzlich folgende Überprüfungen durchführen:
   1. Analysieren des Codes und Ermitteln, ob der Code Werte an die betroffenen APIs übergibt
   1. Ausführen einer Runtimeüberprüfung. Die Runtimebereitstellung wird in Azure Data Lake Analytics nicht parallel ausgeführt. Sie können vor dem Upgrade eine Runtimeüberprüfung ausführen. Verwenden Sie dazu die lokale Visual Studio-Ausführung mit einem lokalen .NET Framework 4.7.2 für ein repräsentatives Dataset.
3. Wenn tatsächlich eine Abwärtsinkompatibilität vorliegt, führen Sie die erforderlichen Schritte aus, um das Problem zu beheben (z. B. Beheben der Daten oder der Codelogik).

In den meisten Fällen sollte keine Abwärtsinkompatibilität auftreten.

## <a name="timeline"></a>Zeitachse

Informationen zum Überprüfen der Bereitstellung der neuen Runtime finden Sie [hier](runtime-troubleshoot.md). Außerdem können Sie vorherige erfolgreich ausgeführte Aufträge überprüfen.

### <a name="what-if-i-cant-get-my-code-reviewed-in-time"></a>Wie gehe ich vor, wenn ich den Code nicht rechtzeitig überprüfen kann?

Sie können den Auftrag mit der alten Runtimeversion (die als Ziel für 4.5.2 erstellt wurde) übermitteln. Aufgrund der fehlenden parallelen .NET Framework-Funktionalität wird der Auftrag jedoch weiterhin nur im 4.5.2-Kompatibilitätsmodus ausgeführt. Aufgrund dieses Verhaltens treten möglicherweise weiterhin einige Probleme mit der Abwärtskompatibilität auf.

### <a name="what-are-the-most-common-backwards-compatibility-issues-you-may-encounter"></a>Gängigste Probleme mit der Abwärtskompatibilität

Nachfolgend sind die gängigsten Abwärtsinkompatibilitäten, die bei der Überprüfung wahrscheinlich ermittelt werden (wir haben diese Liste durch Ausführen der Überprüfung für unsere internen ADLA-Aufträge erstellt), die betroffenen Bibliotheken (Beachten Sie, dass die Bibliotheken nur indirekt aufgerufen werden können. Daher ist es wichtig, Aktion 1 auszuführen, um zu überprüfen, ob Ihre Aufträge betroffen sind.) und mögliche Maßnahmen zur Behebung aufgeführt. Hinweis: In fast allen Fällen erwiesen sich die Warnungen für unsere Aufträge aufgrund der engen Grenzen der meisten Breaking Changes als falsch positiv.

- Die IAsyncResult.CompletedSynchronously-Eigenschaft muss korrekt sein, damit die resultierende Aufgabe abgeschlossen wird
  - Beim Aufruf von TaskFactory.FromAsync muss die Implementierung der IAsyncResult.CompletedSynchronously-Eigenschaft korrekt sein, damit die resultierende Aufgabe abgeschlossen wird. Das heißt, die Eigenschaft muss für den Fall, und ausschließlich für den Fall, dass die Implementierung synchron abgeschlossen wurde, „true“ zurückgeben. Zuvor wurde die Eigenschaft nicht überprüft.
  - Betroffene Bibliotheken: mscorlib, System.Threading.Tasks
  - Vorgeschlagene Aktion: Stellen Sie sicher, dass TaskFactory.FromAsync korrekt TRUE zurückgibt.

- DataObject.GetData ruft Daten jetzt als UTF-8 ab
  - Bei Anwendungen, die .NET Framework 4 als Ziel verwenden oder die in .NET Framework 4.5.1 oder früheren Versionen ausgeführt werden, ruft DataObject.GetData Daten im HTML-Format als ASCII-Zeichenfolge ab. Demzufolge werden ASCII fremde Zeichen (Zeichen mit ASCII-Codes größer als 0x7F) durch zwei zufällige Zeichen dargestellt (#N##N#). Bei Anwendungen, die .NET Framework 4.5 oder höher als Ziel verwenden und in .NET Framework 4.5.2 ausgeführt werden, ruft `DataObject.GetData` HTML-formatierte Daten im UTF-8-Format ab, in dem Zeichen größer als 0x7F korrekt dargestellt werden.
  - Betroffene Bibliotheken: Glo
  - Vorgeschlagene Aktion: Stellen Sie sicher, dass die abgerufenen Daten das gewünschte Format aufweisen.

- XmlWriter löst bei ungültigen Ersatzzeichenpaaren Ausnahmen aus
  - Bei Apps, die auf .NET Framework 4.5.2 oder niedrigere Versionen abzielen, löst das Schreiben eines ungültigen Ersatzzeichenpaars mithilfe der Fallbackbehandlung nicht immer eine Ausnahme aus. Bei Apps mit der Zielplattform .NET Framework 4.6 löst der Versuch, ein ungültiges Ersatzzeichenpaar zu schreiben, eine `ArgumentException` aus.
  - Betroffene Bibliotheken: System.Xml, System.Xml.ReaderWriter
  - Vorgeschlagene Aktion: Stellen Sie sicher, dass Sie kein ungültiges Ersatzzeichenpaar schreiben, das eine Argumentausnahme verursacht.

- HtmlTextWriter rendert das Element `<br/>` nicht ordnungsgemäß
  - Ab .NET Framework 4.6 fügt der Aufruf von `HtmlTextWriter.RenderBeginTag()` und `HtmlTextWriter.RenderEndTag()` mit einem `<BR />`-Element ordnungsgemäß nur ein (anstatt zwei) `<BR />` ein.
  - Betroffene Bibliotheken: System.Web
  - Vorgeschlagene Aktion: Stellen Sie sicher, dass Sie die erwartete Anzahl der `<BR />`-Elemente eingeben, um ein zufälliges Verhalten im Produktionsauftrag zu vermeiden.

- Das Aufrufen von CreateDefaultAuthorizationContext mit einem NULL-Argument wurde geändert
  - Die Implementierung des AuthorizationContext-Elements, das bei einem Aufruf von `CreateDefaultAuthorizationContext(IList<IAuthorizationPolicy>)` mit einem authorizationPolicies-Argument mit dem Wert NULL zurückgegeben wird, wurde in .NET Framework 4.6 geändert.
  - Betroffene Bibliotheken: System.IdentityModel
  - Vorgeschlagene Aktion: Stellen Sie sicher, dass das neue erwartete Verhalten verarbeitet wird, wenn eine NULL-Autorisierungsrichtlinie vorliegt.
  
- RSACng lädt nun fehlerfrei RSA-Schlüssel, die nicht der Standardschlüsselgröße entsprechen
  - In .NET Framework-Versionen, die älter sind als Version 4.6.2, können Kunden, die keine Standardschlüsselgrößen für RSA-Zertifikate verwenden, auf diese Schlüssel nicht über die Erweiterungsmethoden `GetRSAPublicKey()` und `GetRSAPrivateKey()` zugreifen. Eine `CryptographicException` mit der Meldung „The requested key size is not supported“ (Angeforderte Schlüsselgröße wird nicht unterstützt) wird ausgelöst. Bei .NET Framework 4.6.2 wurde dieses Problem behoben. Ebenso werden `RSA.ImportParameters()` und `RSACng.ImportParameters()` jetzt mit nicht standardmäßigen Schlüsselgrößen ausgeführt, ohne dass `CryptographicException` ausgelöst wird.
  - Betroffene Bibliotheken: mscorlib, System.Core
  - Vorgeschlagene Aktion: Stellen Sie sicher, dass die RSA-Schlüssel wie erwartet funktionieren.

- Die Überprüfung von Pfaden auf Doppelpunkte ist genauer
  - In .NET Framework 4.6.2 wurde eine Reihe von Änderungen vorgenommen, um zuvor nicht unterstützte Pfade zu unterstützen (hinsichtlich Länge und Format). Prüfungen der korrekten Trennzeichensyntax (Doppelpunkt) wurden korrigiert. Dies hatte den Nebeneffekt, dass einige URI-Pfade in einigen ausgewählten Pfad-APIs blockiert wurden, in denen sie bisher toleriert wurden.
  - Betroffene Bibliotheken: mscorlib, System.Runtime.Extensions
  - Vorgeschlagene Aktion:

- Aufrufe von ClaimsIdentity-Konstruktoren
  - Ab .NET Framework 4.6.2 legen `T:System.Security.Claims.ClaimsIdentity`-Konstruktoren mit einem `T:System.Security.Principal.IIdentity`-Parameter die Eigenschaft `P:System.Security.Claims.ClaimsIdentify.Actor` anders fest. Wenn es sich bei dem `T:System.Security.Principal.IIdentity`-Argument um ein `T:System.Security.Claims.ClaimsIdentity`-Objekt handelt und die `P:System.Security.Claims.ClaimsIdentify.Actor`-Eigenschaft des `T:System.Security.Claims.ClaimsIdentity`-Objekts nicht `null` ist, wird die `P:System.Security.Claims.ClaimsIdentify.Actor`-Eigenschaft mithilfe der `M:System.Security.Claims.ClaimsIdentity.Clone`-Methode angefügt. In .NET Framework 4.6.1 und früheren Versionen wird die `P:System.Security.Claims.ClaimsIdentify.Actor`-Eigenschaft als vorhandener Verweis angefügt. Aufgrund dieser Änderung entspricht die `P:System.Security.Claims.ClaimsIdentify.Actor`-Eigenschaft des neuen `T:System.Security.Claims.ClaimsIdentity`-Objekts ab .NET Framework 4.6.2 nicht der `P:System.Security.Claims.ClaimsIdentify.Actor`-Eigenschaft des `T:System.Security.Principal.IIdentity`-Arguments des Konstruktors. In .NET Framework 4.6.1 und früheren Versionen sind die Eigenschaften gleich.
  - Betroffene Bibliotheken: mscorlib
  - Vorgeschlagene Aktion: Stellen Sie sicher, dass ClaimsIdentity in der neuen Laufzeit wie erwartet funktioniert.

- Die Serialisierung von Steuerzeichen in DataContractJsonSerializer ist jetzt konform mit ECMAScript V6 und V8
  - In .NET Framework 4.6.2 und früheren Versionen wurden einige Sondersteuerzeichen, z. B. „\b“, „\f“ und „\t“, mit DataContractJsonSerializer nicht so serialisiert, dass dies mit den ECMAScript V6- und V8-Standards kompatibel war. Ab .NET Framework 4.7 ist die Serialisierung dieser Steuerzeichen mit ECMAScript V6 und V8 kompatibel.
  - Betroffene Bibliotheken: System.Runtime.Serialization.Json
  - Vorgeschlagene Aktion: Achten Sie darauf, dass das Verhalten mit DataContractJsonSerializer identisch ist.
