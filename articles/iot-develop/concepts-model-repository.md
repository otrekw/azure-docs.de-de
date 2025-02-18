---
title: Grundlegendes zu den Konzepten des Gerätemodellrepositorys | Microsoft-Dokumentation
description: Hier lernen Sie als Lösungsentwickler oder IT-Experte die grundlegenden Konzepte des Gerätemodellrepositorys kennen.
author: rido-min
ms.author: rmpablos
ms.date: 11/17/2020
ms.topic: conceptual
ms.service: iot-develop
services: iot-develop
ms.openlocfilehash: 5a9a2126d8732a2923428efb7e58cc6ec45e9fa5
ms.sourcegitcommit: 8669087bcbda39e3377296c54014ce7b58909746
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2021
ms.locfileid: "114406423"
---
# <a name="device-models-repository"></a>Gerätemodellrepository

Das Gerätemodellrepository (Device Models Repository, DMR) ermöglicht Geräteentwicklern das Verwalten und Freigeben von IoT Plug & Play-Gerätemodellen. Bei den Gerätemodellen handelt es sich um JSON LD-Dokumente, die mit der [Digital Twins-Modellierungssprache (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) definiert werden.

Das DMR definiert ein Muster zum Speichern von DTDL-Schnittstellen in einer Ordnerstruktur auf der Grundlage des Gerätezwilling-Modellbezeichners (Device Twin Model Identifier, DTMI). Sie können eine Schnittstelle im DMR auffinden, indem Sie den DTMI in einen relativen Pfad konvertieren. Beispielsweise übersetzt sich der DTMI `dtmi:com:example:Thermostat;1` in `/dtmi/com/example/thermostat-1.json`.

## <a name="public-device-models-repository"></a>Öffentliches Gerätemodellrepository

Microsoft hostet einen öffentlichen DMR mit folgenden Merkmalen:

- Kuratierte Modelle. Microsoft überprüft und genehmigt alle verfügbaren Schnittstellen mithilfe eines GitHub-Pull Request (PR)-Validierungsworkflows.
- Unveränderlichkeit.  Schnittstellen können nach der Veröffentlichung nicht aktualisiert werden.
- Hyperskalierung. Microsoft stellt die erforderliche Infrastruktur zum Erstellen eines sicheren, skalierbaren Endpunkts bereit, in dem Sie Gerätemodelle veröffentlichen und nutzen können.

## <a name="custom-device-models-repository"></a>Benutzerdefiniertes Gerätemodellrepository

Verwenden Sie in jedem Speichermedium dasselbe DMR-Muster, z. B. ein lokales Dateisystem oder benutzerdefinierte HTTP-Webserver, um ein benutzerdefiniertes DMR zu erstellen. Sie können Gerätemodelle aus dem benutzerdefinierten DMR genauso wie aus dem öffentlichen DMR abrufen, indem Sie die für den Zugriff auf das DMR verwendete Basis-URL ändern.

> [!NOTE]
> Microsoft stellt Tools zum Überprüfen von Gerätemodellen im öffentlichen DMR bereit. Sie können diese Tools in benutzerdefinierten Repositorys wiederverwenden.

## <a name="public-models"></a>Öffentliche Modelle

Die im Modellrepository gespeicherten öffentlichen Gerätemodelle stehen für alle Benutzer zur Verfügung und können in deren Anwendungen integriert werden. Öffentliche Gerätemodelle ermöglichen ein offenes Ökosystem für Gerätehersteller und Lösungsentwickler, über das sie ihre IoT Plug   Play-Gerätemodelle teilen und wiederverwenden können.

Anweisungen zum Veröffentlichen eines Modells im Modellrepository finden Sie im Abschnitt [Veröffentlichen eines Modells](#publish-a-model).

Benutzer können nach öffentlichen Schnittstellen im öffentlichen [GitHub-Repository](https://github.com/Azure/iot-plugandplay-models) suchen, diese durchsuchen und sie anzeigen.

Alle Schnittstellen in den `dtmi`-Ordnern stehen auch über den öffentlichen Endpunkt [https://devicemodels.azure.com](https://devicemodels.azure.com) zur Verfügung.

### <a name="resolve-models"></a>Auflösen von Modellen

Für einen programmgesteuerten Zugriff auf diese Schnittstellen können Sie den `ModelsRepositoryClient` im NuGet-Paket [Azure.IoT.ModelsRepository](https://www.nuget.org/packages/Azure.IoT.ModelsRepository) verwenden. Dieser Client ist standardmäßig so konfiguriert, dass er das unter [devicemodels.azure.com](https://devicemodels.azure.com/) verfügbare öffentliche DMR abfragt. Er kann für ein beliebiges benutzerdefiniertes Repository konfiguriert werden.

Der Client akzeptiert einen `DTMI` als Eingabe und gibt ein Wörterbuch mit allen erforderlichen Schnittstellen zurück:

```cs
using Azure.IoT.ModelsRepository;

var client = new ModelsRepositoryClient();
IDictionary<string, string> models = client.GetModels("dtmi:com:example:TemperatureController;1");
models.Keys.ToList().ForEach(k => Console.WriteLine(k));
```

Die erwartete Ausgabe sollte den `DTMI` der drei in der Abhängigkeitskette gefundenen Schnittstellen anzeigen:

```txt
dtmi:com:example:TemperatureController;1
dtmi:com:example:Thermostat;1
dtmi:azure:DeviceManagement:DeviceInformation;1
```

Der `ModelsRepositoryClient` kann so konfiguriert werden, dass ein benutzerdefiniertes Modell-Repository über HTTP(s) abgefragt wird. Die Abhängigkeitsauflösung kann mithilfe der verfügbaren `ModelDependencyResolution` angegeben werden:

- Deaktiviert. Gibt nur die angegebene Schnittstelle zurück, ohne jegliche Abhängigkeit.
- Aktiviert. Gibt alle Schnittstellen in der Abhängigkeitskette zurück.
- TryFromExpanded. Verwendet die `.expanded.json`-Datei, um die vorab berechneten Abhängigkeiten abzurufen. 

> [!Tip] 
> Benutzerdefinierte Repositorys machen die `.expanded.json`-Datei möglicherweise nicht verfügbar. Wenn diese nicht verfügbar ist, greift der Client auf eine lokale Verarbeitung der einzelnen Abhängigkeiten zurück.

Das nächste Codebeispiel zeigt, wie der `ModelsRepositoryClient` mit einer benutzerdefinierten Repository-Basis-URL initialisiert wird, in diesem Fall mit den `raw`-URLs aus der GitHub-API ohne Verwendung der `expanded`-Variante, da diese am `raw`-Endpunkt nicht verfügbar ist. Der `AzureEventSourceListener` wird initialisiert, um die vom Client ausgeführte HTTP-Anforderung zu überprüfen:

```cs
using AzureEventSourceListener listener = AzureEventSourceListener.CreateConsoleLogger();

var client = new ModelsRepositoryClient(
    new Uri("https://raw.githubusercontent.com/Azure/iot-plugandplay-models/main"),
    new ModelsRepositoryClientOptions(dependencyResolution: ModelDependencyResolution.Enabled));

IDictionary<string, string> models = client.GetModels("dtmi:com:example:TemperatureController;1");

models.Keys.ToList().ForEach(k => Console.WriteLine(k));
```

Im Quellcode des Azure SDK-Repository auf GitHub finden Sie weitere Beispiele: [Azure.Iot.ModelsRepository/samples](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/modelsrepository/Azure.IoT.ModelsRepository/samples).

## <a name="publish-a-model"></a>Veröffentlichen eines Modells

> [!Important]
> Sie müssen über ein GitHub-Konto verfügen, um Modelle an den öffentlichen DMR übermitteln zu können.

1. Forken Sie das öffentliche GitHub-Repository: [https://github.com/Azure/iot-plugandplay-models](https://github.com/Azure/iot-plugandplay-models).
1. Klonen Sie das geforkte Repository. Erstellen Sie optional einen neuen Branch, um Ihre Änderungen vom `main`-Branch isoliert zu halten.
1. Fügen Sie die neuen Schnittstellen dem Ordner `dtmi` hinzu, und befolgen Sie dabei die Ordner/Dateiname-Konvention. Weitere Informationen finden Sie unter [Importieren eines Modells in den Ordner „`dtmi/`“](#import-a-model-to-the-dtmi-folder).
1. Überprüfen Sie die Modelle lokal mit dem Tool `dmr-client`. Weitere Informationen finden Sie unter [Überprüfen von Modellen](#validate-models).
1. Committen Sie die Änderungen lokal, und pushen Sie sie auf Ihren Fork.
1. Erstellen Sie aus Ihrem Fork einen Pull Request mit dem `main`-Branch als Ziel. Weitere Informationen finden Sie in der Dokumentation zum [Erstellen eines Issues oder Pull Requests](https://docs.github.com/free-pro-team@latest/desktop/contributing-and-collaborating-using-github-desktop/creating-an-issue-or-pull-request).
1. Überprüfen Sie die [Pull Request-Anforderungen](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md).

Der Pull Request löst eine Reihe von GitHub-Aktionen aus, mit denen die gesendeten Schnittstellen überprüft werden und sichergestellt wird, dass Ihr Pull Request alle Anforderungen erfüllt.

Microsoft wird auf einen Pull Request mit allen Prüfungen innerhalb von drei Geschäftstagen reagieren.

### <a name="dmr-client-tools"></a>`dmr-client`-Tools

Die Tools, mit denen die Modelle während der PR-Überprüfungen überprüft werden, können auch zum lokalen Hinzufügen und Überprüfen der DTDL-Schnittstellen verwendet werden.

> [!NOTE]
> Für dieses Tool ist das [.NET SDK](https://dotnet.microsoft.com/download), Version 3.1 oder höher, erforderlich.

### <a name="install-dmr-client"></a>Installieren von `dmr-client`

```bash
curl -L https://aka.ms/install-dmr-client-linux | bash
```

```powershell
iwr https://aka.ms/install-dmr-client-windows -UseBasicParsing | iex
```

### <a name="import-a-model-to-the-dtmi-folder"></a>Importieren eines Modells in den Ordner `dtmi/`

Wenn Sie Ihr Modell bereits in JSON-Dateien gespeichert haben, können Sie dem Ordner `dtmi/` mithilfe des Befehls `dmr-client import` die richtigen Dateinamen hinzufügen:

```bash
# from the local repo root folder
dmr-client import --model-file "MyThermostat.json"
```

> [!TIP]
> Mit dem Argument `--local-repo` können Sie den Stammordner des lokalen Repositorys hinzufügen.

### <a name="validate-models"></a>Überprüfen von Modellen

Sie können Ihre Modelle mit dem Befehl `dmr-client validate` überprüfen:

```bash
dmr-client validate --model-file ./my/model/file.json
```

> [!NOTE]
> Für diese Überprüfung wird die neueste DTDL-Parser-Version verwendet, um sicherzustellen, dass alle Schnittstellen mit der DTDL-Sprachspezifikation kompatibel sind.

Wenn Sie externe Abhängigkeiten überprüfen möchten, müssen diese im lokalen Repository vorhanden sein. Geben Sie zum Überprüfen von Modellen mit der Option `--repo` einen Ordner des Typs `local` oder `remote` an, um Abhängigkeiten aufzulösen:

```bash
# from the repo root folder
dmr-client validate --model-file ./my/model/file.json --repo .
```

### <a name="strict-validation"></a>Strenge Überprüfung

Das DMR enthält zusätzliche [Anforderungen](https://github.com/Azure/iot-plugandplay-models/blob/main/pr-reqs.md). Verwenden Sie das Flag `stict`, um Ihr Modell mit ihnen zu überprüfen:

```bash
dmr-client validate --model-file ./my/model/file.json --repo . --strict true
```

Überprüfen Sie die Konsolenausgabe auf eventuelle Fehlermeldungen.

### <a name="export-models"></a>Exportieren von Modellen

Modelle können mithilfe eines JSON-Arrays aus einem angegebenen Repository (lokal oder remote) in eine einzelne Datei exportiert werden:

```bash
dmr-client export --dtmi "dtmi:com:example:TemperatureController;1" -o TemperatureController.expanded.json
```

## <a name="next-steps"></a>Nächste Schritte

Als nächster Schritt wird empfohlen, sich die [IoT Plug & Play-Architektur](concepts-architecture.md) anzusehen.
