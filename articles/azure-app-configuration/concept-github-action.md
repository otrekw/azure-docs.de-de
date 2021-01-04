---
title: Synchronisieren Ihres GitHub-Repositorys auf App Configuration
description: Verwenden von GitHub Actions, um Ihre App Configuration-Instanz automatisch zu aktualisieren, wenn Sie Ihr GitHub-Repository aktualisieren.
author: AlexandraKemperMS
ms.author: alkemper
ms.date: 05/28/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 588efd692119c9e2831e16c1ce26c2759898a1e5
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97607363"
---
# <a name="sync-your-github-repository-to-app-configuration"></a>Synchronisieren Ihres GitHub-Repositorys auf App Configuration

Teams, die Ihre vorhandenen Verfahren der Quellcodeverwaltung beibehalten möchten, können mithilfe von GitHub Actions ihr GitHub-Repository automatisch mit ihrem App Configuration-Speicher synchronisieren. So können Sie auf die gewohnte Weise Änderungen an den Konfigurationsdateien vornehmen und beispielweise die folgenden Vorteile von App Configuration nutzen: <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Zentrale Konfiguration außerhalb des Codes <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Aktualisieren der Konfiguration ohne erneute Bereitstellung der gesamten App <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Integration in Dienste wie Azure App Service und Azure Functions 

Ein GitHub Actions-[Workflow](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions#the-components-of-github-actions) definiert einen automatisierten Prozess in einem GitHub-Repository. Die Aktion *Azure App Configuration-Synchronisierung* löst Aktualisierungen einer App Configuration-Instanz aus, wenn Änderungen am Quellrepository vorgenommen werden. Dabei wird zum Definieren der Schritte und Parameter eine YAML-Datei (.yml) im Pfad `/.github/workflows/` des Repositorys verwendet. Sie können Konfigurationsupdates beim Pushen, Überprüfen oder Branchen von App-Konfigurationsdateien auslösen – genauso wie beim App-Code.

Die GitHub-[Dokumentation](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions) bietet ausführliche Informationen zu GitHub-Workflows und GitHub Actions. 

## <a name="enable-github-actions-in-your-repository"></a>Aktivieren von GitHub Actions in Ihrem Repository
Um mit der Verwendung dieser GitHub-Aktion zu beginnen, navigieren Sie zu Ihrem Repository, und wählen Sie die Registerkarte **Aktionen** aus. Wählen Sie **Neuer Workflow** und dann **Eigenen Workflow einrichten** aus. Suchen Sie zum Schluss im Marketplace nach „Azure App Configuration-Synchronisierung“.
> [!div class="mx-imgBorder"]
> ![Auswählen der Registerkarte „Aktionen“](media/find-github-action.png)

> [!div class="mx-imgBorder"]
> ![Auswählen der Aktion „App Configuration-Synchronisierung“](media/app-configuration-sync-action.png)

## <a name="sync-configuration-files-after-a-push"></a>Synchronisieren von Konfigurationsdateien nach einem Push
Mit dieser Aktion werden Azure App Configuration-Dateien synchronisiert, wenn eine Änderung an `appsettings.json` gepusht wird. Wenn ein Entwickler eine Änderung an `appsettings.json` pusht, aktualisiert die Aktion „App Configuration-Synchronisierung“ die App Configuration-Instanz mit den neuen Werten.

Der erste Abschnitt dieses Workflows gibt an, dass die Aktion *bei* einem *Push* mit `appsettings.json` zum *Mainbranch* ausgelöst wird. Im zweiten Abschnitt werden die Aufträge aufgelistet, die nach dem Auslösen der Aktion ausgeführt werden. Die Aktion prüft die relevanten Dateien und aktualisiert die App Configuration-Instanz mithilfe der als Geheimnis im Repository gespeicherten Verbindungszeichenfolge.  Weitere Informationen zur Verwendung von Geheimnissen in GitHub finden Sie in diesem [GitHub-Artikel](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets) zum Erstellen und Verwenden verschlüsselter Geheimnisse.

```json
on: 
  push: 
    branches: 
      - 'main' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your                        
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }} 
          separator: ':' 
```

## <a name="use-strict-sync"></a>Verwenden der strikten Synchronisierung
Die GitHub-Aktion aktiviert den Strict-Modus standardmäßig nicht, d. h., dass die Synchronisierung nur Schlüssel-Wert-Paare aus der Konfigurationsdatei zur App Configuration-Instanz hinzufügt (es werden keine Schlüssel-Wert-Paare gelöscht). Wenn Sie den Strict-Modus aktivieren, werden Schlüssel-Wert-Paare aus der App Configuration-Instanz gelöscht, die nicht in der Konfigurationsdatei enthalten sind, damit sie mit der Konfigurationsdatei übereinstimmen. Wenn Sie eine Synchronisierung mit mehreren Quellen oder mithilfe von Azure Key Vault mit App Configuration durchführen, sollten Sie verschiedene Präfixe oder Bezeichnungen mit strenger Synchronisierung verwenden, um zu vermeiden, dass die Konfigurationseinstellungen aus anderen Dateien gelöscht werden (siehe Beispiele unten). 

```json
on: 
  push: 
    branches: 
      - 'main' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          label: 'Label' 
          prefix: 'Prefix:' 
          strict: true 
```
## <a name="sync-multiple-files-in-one-action"></a>Synchronisieren mehrerer Dateien in einer Aktion 

Wenn Ihre Konfiguration auf mehrere Dateien verteilt ist, können Sie das Muster unten verwenden, um eine Synchronisierung auszulösen, wenn eine der Dateien geändert wird. In diesem Muster wird die glob-Bibliothek unter https://www.npmjs.com/package/glob verwendet. Beachten Sie Folgendes: Wenn der Name der Konfigurationsdatei ein Komma enthält, können Sie einen umgekehrten Schrägstrich als Escapezeichen für das Komma verwenden. 

```json
on:
  push:
    branches:
      - 'main'
    paths:
      - 'appsettings.json'
      - 'appsettings2.json'

jobs:
  syncconfig:
    runs-on: ubuntu-latest
    steps:
      # checkout done so that files in the repo can be read by the sync
      - uses: actions/checkout@v1
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: '{appsettings.json,appsettings2.json}'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
```

## <a name="sync-by-prefix-or-label"></a>Synchronisierung nach Präfix oder Bezeichnung
Beim Festlegen von Präfixen oder Bezeichnungen in Ihrer Synchronisierungsaktion wird nur die jeweilige Gruppe synchronisiert. Dies ist für die Verwendung der strengen Synchronisierung mit mehreren Dateien wichtig. Je nachdem, wie die Konfiguration eingerichtet ist, kann jeder Datei entweder ein Präfix oder eine Bezeichnung zugeordnet werden. Dann kann jeder Präfix oder jede Bezeichnung separat synchronisiert werden, damit nichts überschrieben wird. Präfixe werden in der Regel für unterschiedliche Anwendungen oder Dienste verwendet, während Bezeichnungen für verschiedene Umgebungen verwendet werden. 

Synchronisierung nach Präfix: 

```json
on:
  push:
    branches:
      - 'main'
    paths:
      - 'appsettings.json'

jobs:
  syncconfig:
    runs-on: ubuntu-latest
    steps:
      # checkout done so that files in the repo can be read by the sync
      - uses: actions/checkout@v1
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: 'appsettings.json'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
          prefix: 'Prefix::'
```

Synchronisierung nach Bezeichnung: 

```json
on:
  push:
    branches:
      - 'main'
    paths:
      - 'appsettings.json'

jobs:
  syncconfig:
    runs-on: ubuntu-latest
    steps:
      # checkout done so that files in the repo can be read by the sync
      - uses: actions/checkout@v1
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: 'appsettings.json'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
          label: 'Label'

```

## <a name="use-a-dynamic-label-on-sync"></a>Verwenden einer dynamischen Bezeichnung bei Synchronisierung
Die folgende Aktion fügt bei jeder Synchronisierung eine dynamische Bezeichnung ein, um sicherzustellen, dass jede Synchronisierung eindeutig identifiziert werden kann, und die Zuordnung von Codeänderungen zu Konfigurationsänderungen zu ermöglichen.

Der erste Abschnitt dieses Workflows gibt an, dass die Aktion *bei* einem *Push* mit `appsettings.json` zum *Mainbranch* ausgelöst wird. Der zweite Abschnitt führt einen Auftrag aus, der basierend auf dem Commithash eine eindeutige Bezeichnung für das Konfigurationsupdate erstellt. Der Auftrag aktualisiert dann die App Configuration-Instanz mit den neuen Werten und der eindeutigen Bezeichnung für dieses Update.

```json
on: 
  push: 
    branches: 
      - 'main' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # Creates a label based on the branch name and the first 8 characters          
      # of the commit hash 
      - id: determine_label 
        run: echo ::set-output name=LABEL::"${GITHUB_REF#refs/*/}/${GITHUB_SHA:0:8}" 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          label: ${{ steps.determine_label.outputs.LABEL }} 
```

## <a name="use-azure-key-vault-with-github-action"></a>Verwenden von Azure Key Vault mit einer GitHub-Aktion
Entwickler, die Azure Key Vault mit App Configuration verwenden, sollten zwei separate Dateien verwenden. Üblicherweise werden die Dateien „appsettings.json“ und „secretreferences.json“ verwendet. Die Datei „secretreferences.json“ enthält die URL zum Schlüsseltresorgeheimnis.

{"mySecret": "{\"uri\":\"https://myKeyVault.vault.azure.net/secrets/mySecret "}"}

Die GitHub-Aktion kann dann zum Durchführen einer strengen Synchronisierung für „appsettings.json“ gefolgt von einer nicht strengen Synchronisierung für „secretreferences.json“ konfiguriert werden. Das folgende Beispiel löst eine Synchronisierung aus, wenn eine der Dateien geändert wird:

```json
on:
  push:
    branches:
      - 'main'
    paths:
      - 'appsettings.json'
      - 'secretreferences.json'

jobs:
  syncconfig:
    runs-on: ubuntu-latest
    steps:
      # checkout done so that files in the repo can be read by the sync
      - uses: actions/checkout@v1
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: 'appsettings.json'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
          strict: true
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: 'secretreferences.json'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
          contentType: 'application/vnd.microsoft.appconfig.keyvaultref+json;charset=utf-8'

```

## <a name="use-max-depth-to-limit-github-action"></a>Verwenden der maximalen Tiefe zum Einschränken der GitHub-Aktion
Das Standardverhalten für geschachtelte JSON-Attribute besteht darin, das gesamte Objekt zu vereinfachen.  Der folgende JSON-Code definiert dieses Schlüssel-Wert-Paar:

| Schlüssel | Wert |
| --- | --- |
| Object:Inner:InnerKey | InnerValue |

```json
{ "Object": 
    { "Inner":
        {
        "InnerKey": "InnerValue"
        }
    }
}
```

Wenn das geschachtelte-Objekt der an die App Configuration-Instanz gepushte Wert sein soll, können Sie den Wert *depth* verwenden, um die Vereinfachung ab der entsprechenden Tiefe zu verhindern. 

```json
on: 
  push: 
    branches: 
      - 'main' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          depth: 2 
```

Bei einer Tiefe von 2 gibt das obige Beispiel nun das folgende Schlüssel-Wert-Paar zurück:

| Schlüssel | Wert |
| --- | --- |
| Object:Inner | {"InnerKey":"InnerValue"} |

## <a name="understand-action-inputs"></a>Grundlegendes zu Aktionseingaben
Eingabeparameter geben Daten an, die von der Aktion während der Laufzeit verwendet werden.  In der folgenden Tabelle sind die von der App Configuration-Synchronisierung akzeptierten Eingabeparameter sowie die jeweils erwarteten Werte enthalten.  Weitere Informationen zu Aktionseingaben für GitHub Actions finden Sie in der GitHub-[Dokumentation](https://docs.github.com/en/free-pro-team@latest/actions/creating-actions/metadata-syntax-for-github-actions#inputs).

> [!Note]
> Bei den Eingabe-IDs wird die Groß-/Kleinschreibung nicht berücksichtigt.


| Eingabename | Erforderlich? | Wert |
|----|----|----|
| configurationFile | Ja | Relativer Pfad zur Konfigurationsdatei im Repository.  Globmuster werden unterstützt und können mehrere Dateien enthalten. |
| format | Ja | Dateiformat der Konfigurationsdatei.  Gültige Formate sind: JSON, YAML, Eigenschaften. |
| connectionString | Ja | Verbindungszeichenfolge für die App Configuration-Instanz. Die Verbindungszeichenfolge muss als Geheimnis im GitHub-Repository gespeichert werden. Im Workflow sollte nur der Geheimnisname verwendet werden. |
| Trennzeichen | Ja | Trennzeichen bei der Vereinfachung der Konfigurationsdatei für Schlüssel-Wert-Paare.  Gültige Werte sind: . , ; : - _ __ / |
| prefix | Nein | Das Präfix, das am Anfang der Schlüssel hinzugefügt werden soll. |
| label | Nein | Bezeichnung, die beim Festlegen von Schlüssel-Wert-Paaren verwendet werden soll. Wenn keine Angabe erfolgt, wird eine NULL-Bezeichnung verwendet. |
| strict | Nein | Ein boolescher Wert, der bestimmt, ob der Strict-Modus aktiviert ist. Der Standardwert ist „FALSE“. |
| depth | Nein | Maximale Tiefe für das Vereinfachen der Konfigurationsdatei.  Die Tiefe muss eine positive Zahl sein.  Beim Standardwert wird keine maximale Tiefe angegeben. |
| tags | Nein | Gibt den Tagsatz für Schlüssel-Wert-Paare an.  Das erwartete Format ist eine Zeichenfolgenform eines JSON-Objekts in folgendem Format: { [Eigenschaftenname: Zeichenfolge]: Zeichenfolge; }. Jeder Eigenschaftennamenswert wird zu einem Tag. |

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie etwas über die GitHub-Aktion „App Configuration-Synchronisierung“ und ihre Verwendung zum Automatisieren von Updates für Ihre App Configuration-Instanz erfahren. Um zu erfahren, wie Azure App Configuration auf Änderungen an Schlüssel-Wert-Paaren reagiert, fahren Sie mit dem nächsten [Artikel](./concept-app-configuration-event.md) fort.
