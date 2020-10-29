---
title: Grundlegendes zu den Konzepten des Gerätemodellrepositorys | Microsoft-Dokumentation
description: Hier lernen Sie als Lösungsentwickler oder IT-Experte die grundlegenden Konzepte des Gerätemodellrepositorys kennen.
author: rido-min
ms.author: rmpablos
ms.date: 09/30/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 4e15ef5256c1552fc8ab7fb9bd84f15bb3433834
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131359"
---
# <a name="device-model-repository"></a>Gerätemodellrepository

Das Gerätemodellrepository (Device Model Repository, DMR) ermöglicht Geräteentwicklern das Verwalten und Freigeben von IoT Plug & Play-Gerätemodellen. Bei den Gerätemodellen handelt es sich um JSON LD-Dokumente, die mit der [Digital Twins-Modellierungssprache (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) definiert werden.

Das DMR definiert ein Muster zum Speichern von DTDL-Schnittstellen in einer Ordnerstruktur auf der Grundlage des Gerätezwilling-Modellbezeichners (Device Twin Model Identifier, DTMI). Sie können eine Schnittstelle im DMR auffinden, indem Sie den DTMI in einen relativen Pfad konvertieren. Beispielsweise übersetzt sich der DTMI `dtmi:com:example:Thermostat;1` in `/dtmi/com/example/thermostat-1.json`.

## <a name="public-device-model-repository"></a>Öffentliches Gerätemodellrepository

Microsoft hostet einen öffentlichen DMR mit folgenden Merkmalen:

- Kuratierte Modelle. Microsoft überprüft und genehmigt alle verfügbaren Schnittstellen mithilfe eines GitHub-Pull Request (PR)-Validierungsworkflows.
- Unveränderlichkeit.  Schnittstellen können nach der Veröffentlichung nicht aktualisiert werden.
- Hyperskalierung. Microsoft stellt die erforderliche Infrastruktur zum Erstellen eines sicheren, skalierbaren Endpunkts bereit, in dem Sie Gerätemodelle veröffentlichen und nutzen können.

## <a name="custom-device-model-repository"></a>Benutzerdefiniertes Gerätemodellrepository

Sie können in jedem Speichermedium das gleiche DMR-Muster verwenden, z. B. ein lokales Dateisystem oder benutzerdefinierte HTTP-Webserver, um ein benutzerdefiniertes DMR zu erstellen. Sie können Gerätemodelle aus dem benutzerdefinierten DMR genauso wie aus dem öffentlichen DMR abrufen, indem Sie einfach die für den Zugriff auf das DMR verwendete Basis-URL ändern.

> [!NOTE]
> Microsoft stellt Tools zum Überprüfen von Gerätemodellen im öffentlichen DMR bereit. Sie können diese Tools in benutzerdefinierten Repositorys wiederverwenden.

## <a name="public-models"></a>Öffentliche Modelle

Die im Modellrepository gespeicherten öffentlichen Gerätemodelle stehen für alle Benutzer zur Verfügung und können in deren Anwendungen integriert werden. Öffentliche Gerätemodelle ermöglichen ein offenes Ökosystem für Gerätehersteller und Lösungsentwickler, über das sie ihre IoT Plug   Play-Gerätemodelle teilen und wiederverwenden können.

Anweisungen zum Veröffentlichen eines Modells im Modellrepository finden Sie im Abschnitt [Veröffentlichen eines Modells](#publish-a-model).

Benutzer können nach öffentlichen Schnittstellen im öffentlichen [GitHub-Repository](https://github.com/Azure/iot-plugandplay-models) suchen, diese durchsuchen und sie anzeigen.

Alle Schnittstellen in den `dtmi`-Ordnern stehen auch über den öffentlichen Endpunkt [https://devicemodels.azure.com](https://devicemodels.azure.com) zur Verfügung.

### <a name="resolve-models"></a>Auflösen von Modellen

Für den programmgesteuerten Zugriff auf diese Schnittstellen müssen Sie einen DTMI in einen relativen Pfad konvertieren, den Sie zum Abfragen des öffentlichen Endpunkts verwenden können. Im folgenden Codebeispiel wird dies veranschaulicht:

Zum Konvertieren eines DTMI in einen absoluten Pfad verwenden wir die Funktion `DtmiToPath` mit `IsValidDtmi`:

```cs
static string DtmiToPath(string dtmi)
{
    if (!IsValidDtmi(dtmi))
    {
        return null;
    }
    // dtmi:com:example:Thermostat;1 -> dtmi/com/example/thermostat-1.json
    return $"/{dtmi.ToLowerInvariant().Replace(":", "/").Replace(";", "-")}.json";
}

static bool IsValidDtmi(string dtmi)
{
    // Regex defined at https://github.com/Azure/digital-twin-model-identifier#validation-regular-expressions
    Regex rx = new Regex(@"^dtmi:[A-Za-z](?:[A-Za-z0-9_]*[A-Za-z0-9])?(?::[A-Za-z](?:[A-Za-z0-9_]*[A-Za-z0-9])?)*;[1-9][0-9]{0,8}$");
    return rx.IsMatch(dtmi);
}
```

Mit dem resultierenden Pfad und der Basis-URL für das Repository können wir die Schnittstelle abrufen:

```cs
const string _repositoryEndpoint = "https://devicemodels.azure.com";

string dtmiPath = DtmiToPath(dtmi.ToString());
string fullyQualifiedPath = $"{_repositoryEndpoint}{dtmiPath}";
string modelContent = await _httpClient.GetStringAsync(fullyQualifiedPath);
```

## <a name="publish-a-model"></a>Veröffentlichen eines Modells

> [!Important]
> Sie müssen über ein GitHub-Konto verfügen, um Modelle an den öffentlichen DMR übermitteln zu können.

1. Forken Sie das öffentliche GitHub-Repository: [https://github.com/Azure/iot-plugandplay-models](https://github.com/Azure/iot-plugandplay-models).
1. Klonen Sie das geforkte Repository. Erstellen Sie optional einen neuen Branch, um Ihre Änderungen vom `main`-Branch isoliert zu halten.
1. Fügen Sie die neuen Schnittstellen dem Ordner `dtmi` hinzu, und befolgen Sie dabei die Ordner/Dateiname-Konvention. Mehr dazu finden Sie im [add-model](#add-model)-Tool.
1. Überprüfen Sie die Gerätemodelle lokal anhand der Informationen im Abschnitt [Skripts zum Überprüfen von Änderungen](#validate-files).
1. Committen Sie die Änderungen lokal, und pushen Sie sie auf Ihren Fork.
1. Erstellen Sie aus Ihrem Fork einen Pull Request mit dem `main`-Branch als Ziel. Weitere Informationen finden Sie in der Dokumentation zum [Erstellen eines Issues oder Pull Requests](https://docs.github.com/free-pro-team@latest/desktop/contributing-and-collaborating-using-github-desktop/creating-an-issue-or-pull-request).
1. Überprüfen Sie die [Pull Request-Anforderungen](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md).

Der Pull Request löst eine Reihe von GitHub-Aktionen aus, mit deren Hilfe die neu gesendeten Schnittstellen validiert werden und sichergestellt wird, dass Ihr Pull Request alle Prüfungen besteht.

Microsoft wird auf einen Pull Request mit allen Prüfungen innerhalb von drei Geschäftstagen reagieren.

### <a name="add-model"></a>add-model

In den folgenden Schritten sehen Sie, wie das add-model.js-Skript Sie beim Hinzufügen einer neuen Schnittstelle unterstützt. Für die Ausführung dieses Skripts ist Node.js erforderlich:

1. Navigieren Sie an einer Eingabeaufforderung zum lokalen Git-Repository.
1. Ausführen von `npm install`
1. Ausführen von `npm run add-model <path-to-file-to-add>`

Achten Sie auf eventuelle Fehlermeldungen in der Konsolenausgabe.

### <a name="local-validation"></a>Lokale Überprüfung

Sie können die gleichen Validierungsprüfungen lokal ausführen, bevor Sie den Pull Request absenden, um die Diagnose von Problemen vorab zu unterstützen.

#### <a name="validate-files"></a>validate-files

`npm run validate-files <file1.json> <file2.json>` überprüft, ob der Dateipfad mit den erwarteten Ordern- und Dateinamen übereinstimmt.

#### <a name="validate-ids"></a>validate-ids

`npm run validate-ids <file1.json> <file2.json>` überprüft, ob alle im Dokument definierten IDs den gleichen Stamm wie die Haupt-ID verwenden.

#### <a name="validate-deps"></a>validate-deps

`npm run validate-deps <file1.json> <file2.json>` prüft, ob alle Abhängigkeiten im Ordner `dtmi` verfügbar sind.

#### <a name="validate-models"></a>validate-models

Sie können das [DTDL-Validierungsbeispiel](https://github.com/Azure-Samples/DTDL-Validator) ausführen, um Ihre Gerätemodelle lokal zu überprüfen.

## <a name="next-steps"></a>Nächste Schritte

Als nächster Schritt wird empfohlen, sich die [IoT Plug & Play-Architektur](concepts-architecture.md) anzusehen.
