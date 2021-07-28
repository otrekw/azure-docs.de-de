---
title: Bereitstellen eines ML-Modells mit einem verwalteten Onlineendpunkt
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Ihr Machine Learning-Modell als Webdienst bereitstellen, der automatisch von Azure verwaltet wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: seramasu
ms.reviewer: laobri
author: rsethur
ms.date: 05/13/2021
ms.topic: how-to
ms.custom: how-to
ms.openlocfilehash: 85b587dcaed162a0372f03240f9c4cb33d7507c1
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111969067"
---
# <a name="deploy-and-score-a-machine-learning-model-with-a-managed-online-endpoint-preview"></a>Bereitstellen und Bewerten eines Machine Learning-Modells mit einem verwalteten Onlineendpunkt (Vorschau)

Verwaltete Onlineendpunkte (Vorschau) bieten Ihnen die Möglichkeit zur Bereitstellung Ihres Modells, ohne dass Sie die zugrunde liegende Infrastruktur erstellen und verwalten müssen. In diesem Artikel beginnen Sie mit der Bereitstellung eines Modells auf Ihrem lokalen Computer, um eventuelle Fehler zu beheben, und anschließend werden Sie es in Azure bereitstellen und testen. Außerdem erfahren Sie, wie Sie die Protokolle anzeigen und die Vereinbarung zum Servicelevel (SLA) überwachen. Sie beginnen mit einem Modell und erhalten am Ende einen skalierbaren HTTPS/REST-Endpunkt, der für die Online-/Echtzeitbewertung verwendet werden kann.

[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Für die Verwendung von Azure Machine Learning ist ein Azure-Abonnement erforderlich. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Sie müssen die Azure CLI und die ML-Erweiterung installieren und konfigurieren. Weitere Informationen finden Sie unter [Installieren, Einrichten und Verwenden der 2.0 CLI (Vorschau)](how-to-configure-cli.md). 

* Sie müssen über eine Azure-Ressourcengruppe verfügen, für die Ihnen (oder dem von Ihnen genutzten Dienstprinzipal) die Zugriffsberechtigung `Contributor` (Mitwirkender) zugeordnet ist. Sie verfügen über eine Ressourcengruppe dieser Art, wenn Sie Ihre ML-Erweiterung gemäß dem obigen Artikel konfiguriert haben. 

* Sie müssen über einen Azure Machine Learning-Arbeitsbereich verfügen. Sie verfügen über einen Arbeitsbereich dieser Art, wenn Sie Ihre ML-Erweiterung gemäß dem obigen Artikel konfiguriert haben.

* Falls Sie die Standardwerte für die Azure CLI noch nicht festgelegt haben, sollten Sie Ihre Standardeinstellungen speichern. Führen Sie Folgendes aus, um zu vermeiden, dass Sie die Werte immer wieder neu übergeben müssen:

   ```azurecli
   az account set --subscription <subscription id>
   az configure --defaults workspace=<azureml workspace name> group=<resource group>

* [Optional] To deploy locally, you must have [Docker engine](https://docs.docker.com/engine/install/) running locally. This step is **highly recommended**. It will help you debug issues.

## Prepare your system

To follow along with the article, clone the samples repository, and navigate to the right directory by running the following commands:

```azurecli
git clone https://github.com/Azure/azureml-examples
cd azureml-examples
cd cli
```

Legen Sie ihren Endpunktnamen fest (benennen Sie unten `YOUR_ENDPOINT_NAME` in einen eindeutigen Namen um). Der folgende Befehl gilt für Unix-Umgebungen:

```azurecli
export ENDPOINT_NAME=YOUR_ENDPOINT_NAME
```

Bei einem Windows-Betriebssystem verwenden Sie stattdessen den Befehl `set ENDPOINT_NAME=YOUR_ENDPOINT_NAME`.

> [!NOTE]
> Endpunktnamen müssen auf Azure-Regionsebene eindeutig sein. Beispielsweise kann es nur einen Endpunkt namens `my-endpoint` in `westus2` geben. 

## <a name="define-the-endpoint-configuration"></a>Definieren der Endpunktkonfiguration

Für die Bereitstellung eines Modells auf einem Onlineendpunkt sind folgende Eingaben erforderlich:

- Modelldateien (oder Name und Version eines Modells, das bereits in Ihrem Arbeitsbereich registriert ist). In diesem Beispiel verwenden wir ein `scikit-learn`-Modell, das Regressionen durchführt.
- Code, der für die Bewertung des Modells erforderlich ist. In diesem Fall verfügen wir über eine `score.py`-Datei.
- Eine Umgebung, in der Ihr Modell ausgeführt wird (wie Sie sehen werden, kann es sich bei der Umgebung um ein Docker-Image mit Conda-Abhängigkeiten oder um eine Dockerfile handeln).
- Einstellungen zum Angeben des Instanztyps und der Skalierungskapazität.

Der folgende Codeausschnitt zeigt die `endpoints/online/managed/simple-flow/1-create-endpoint-with-blue.yml`-Datei, die alle oben genannten Informationen erfasst: 

:::code language="yaml" source="~/azureml-examples-main/cli/endpoints/online/managed/simple-flow/1-create-endpoint-with-blue.yml":::

> [!Note]
> Ein vollständig angegebener YAML-Beispielcode für verwaltete Onlineendpunkte steht als [Referenz](https://azuremlschemas.azureedge.net/latest/managedOnlineEndpoint.template.yaml) zur Verfügung.

Die Referenz für das YAML-Endpunktformat ist unten angegeben. Informationen zum Angeben dieser Attribute finden Sie im YAML-Beispiel aus diesem Artikel oder im vollständig angegebenen YAML-Beispiel, das im vorherigen Hinweis erwähnt wurde. Weitere Informationen zu Grenzwerten im Zusammenhang mit verwalteten Endpunkten finden Sie unter [Verwalten und Erhöhen von Kontingenten für Ressourcen mit Azure Machine Learning](how-to-manage-quotas.md).

| Schlüssel | BESCHREIBUNG |
| --- | --- |
| $schema    | [Optional] Das YAML-Schema. Sie können das Schema aus dem obigen Beispiel in einem Browser betrachten, um alle verfügbaren Optionen in der YAML-Datei anzuzeigen.|
| name       | Name des Endpunkts. Muss auf Azure-Regionsebene eindeutig sein.|
| traffic | Prozentsatz des Datenverkehrs vom Endpunkt, der zu den einzelnen Bereitstellungen umgeleitet wird. Datenverkehrswerte müssen addiert 100 ergeben. |
| auth_mode | Verwenden Sie `key` für die schlüsselbasierte Authentifizierung und `aml_token` für die tokenbasierte Azure Machine Learning-Authentifizierung. `key` läuft nicht ab, dafür aber `aml_token`. Rufen Sie das letzte Token mit dem Befehl `az ml endpoint get-credentials` ab. |
| deployments | Enthält eine Liste der Bereitstellungen, die am Endpunkt erstellt werden sollen. In diesem Fall gibt es nur eine Bereitstellung namens `blue`. Weitere Informationen zu mehreren Bereitstellungen finden Sie unter [Sicherer Rollout für Onlineendpunkte (Vorschau)](how-to-safely-rollout-managed-endpoints.md).|

Attribute von `deployments`:

| Schlüssel | BESCHREIBUNG |
| --- | --- |
| name  | Name der Bereitstellung |
| model | In diesem Beispiel geben wir die Modelleigenschaften inline an: `name`, `version` und `local_path`. Die Modelldateien werden automatisch hochgeladen und registriert. Ein Nachteil der Inlinespezifikation ist, dass Sie die Version manuell erhöhen müssen, wenn Sie die Modelldateien aktualisieren möchten. Informationen zu verwandten bewährten Methoden finden Sie im folgenden Abschnitt unter **Tipp**. |
| code_configuration.code.local_path | Das Verzeichnis, das den gesamten Python-Quellcode für die Bewertung des Modells enthält. Geschachtelte Verzeichnisse/Pakete werden unterstützt. |
| code_configuration.scoring_script | Die Python-Datei im obigen Bewertungsverzeichnis. Dieser Python-Code muss über eine `init()`- und eine `run()`-Funktion verfügen. Die Funktion `init()` wird aufgerufen, nachdem das Modell erstellt oder aktualisiert wurde (Sie können es verwenden, um das Modell im Arbeitsspeicher zwischenzuspeichern usw.). Die Funktion `run()` wird bei jedem Aufruf des Endpunkts aufgerufen, um die tatsächliche Bewertung/Vorhersage auszuführen. |
| Environment | Enthält die Details der Umgebung zum Hosten des Modells und des Codes. In diesem Beispiel verfügen wir über Inlinedefinitionen, die `name`, `version` und `path` einbeziehen. In diesem Beispiel wird `environment.docker.image` als Image verwendet, und die `conda_file`-Abhängigkeiten werden zusätzlich installiert. Weitere Informationen finden Sie unter **Tipp** im folgenden Abschnitt. |
| instance_type | Die VM-SKU zum Hosten Ihrer Bereitstellungsinstanzen. Weitere Informationen finden Sie unter [Unterstützte VM-SKUs für verwaltete Onlineendpunkte](reference-managed-online-endpoints-vm-sku-list.md). |
| scale_settings.scale_type | Zurzeit muss dieser Wert auf `manual` festgelegt werden. Um nach dem Erstellen des Endpunkts und der Bereitstellung hoch- oder herunterskalieren zu können, aktualisieren Sie `instance_count` im YAML-Code, und führen Sie den Befehl `az ml endpoint update -n $ENDPOINT_NAME --file <yaml filepath>` aus.|
| scale_settings.instance_count | Die Anzahl der Instanzen in der Bereitstellung. Richten Sie den Wert nach der zu erwartenden Workload. Für die Hochverfügbarkeit empfiehlt Microsoft, den Wert mindestens auf `3` festzulegen. |

Weitere Informationen zum YAML-Schema finden Sie im Dokument [YAML-Referenz für verwalteten Onlineendpunkt](reference-online-endpoint-yaml.md).

> [!Note]
> So verwenden Sie Azure Kubernetes Service (AKS) als Computeziel anstelle von verwalteten Endpunkten:
> 1. Erstellen Sie Ihren AKS-Cluster, und fügen Sie ihn [mithilfe von Azure ML Studio](how-to-create-attach-compute-studio.md#whats-a-compute-target) als Computeziel an Ihren Azure Machine Learning-Arbeitsbereich an.
> 2. Verwenden Sie diesen [YAML-Endpunktcode](https://github.com/Azure/azureml-examples/blob/main/cli/endpoints/online/aks/simple-flow/1-create-aks-endpoint-with-blue.yml), um AKS anstelle des obigen verwalteten YAML-Endpunktcodes als Ziel zu verwenden. Sie müssen den YAML-Code bearbeiten, um den Wert von `target` in den Namen Ihres registrierten Computeziels zu ändern.
> Die Befehle dieses Artikels sind, mit Ausnahme der optionalen SLA-Überwachung und der Log Analytics-Integration, zwischen verwalteten und AKS-Endpunkten austauschbar.

### <a name="registering-your-model-and-environment-separately"></a>Separates Registrieren von Modell und Umgebung

 In diesem Beispiel geben wir die Modell- und Umgebungseigenschaften inline an: `name`, `version` und `local_path`, von wo aus Dateien hochgeladen werden sollen. Im Hintergrund lädt die CLI die Dateien hoch und registriert das Modell und die Umgebung automatisch. Als bewährte Methode für die Produktion sollten Sie das Modell und die Umgebung separat registrieren und den registrierten Namen sowie die Version im YAML-Code angeben. Das Format lautet `model: azureml:my-model:1` oder `environment: azureml:my-env:1`.

 Um die Registrierung durchzuführen, können Sie die YAML-Definitionen von `model` und `environment` in separate YAML-Dateien extrahieren und die Befehle `az ml model create` und `az ml environment create` verwenden. Führen Sie `az ml model create -h` und `az ml environment create -h` aus, um mehr über diese Befehle zu erfahren.

### <a name="using-different-cpu--gpu-instance-types"></a>Verwenden verschiedener CPU- und GPU-Instanztypen

Der oben genannte YAML-Code verwendet einen universellen Typ (`Standard_F2s_v2`) und ein Docker-Image ohne GPU (im YAML-Code finden Sie das Attribut `image`). Für GPU-Compute sollten Sie eine GPU-Computetyp-SKU und ein GPU-Docker-Image auswählen.

Die unterstützten universellen und GPU-Instanztypen finden Sie unter [Unterstützte VM-SKUs für verwaltete Onlineendpunkte](reference-managed-online-endpoints-vm-sku-list.md). Eine Liste der CPU- und GPU-Basisimages für Azure ML finden Sie unter [Azure Machine Learning-Basisimages](https://github.com/Azure/AzureML-Containers).

### <a name="using-more-than-one-model"></a>Verwenden mehrerer Modelle

Derzeit können Sie nur ein Modell pro Bereitstellung in YAML angeben. Wenn Sie über mehrere Modelle verfügen, können Sie diese Einschränkung umgehen: Wenn Sie das Modell registrieren, kopieren Sie alle Modelle (als Dateien oder Unterverzeichnisse) in einen Ordner, den Sie für die Registrierung verwenden. In Ihrem Bewertungsskript können Sie die Umgebungsvariable `AZUREML_MODEL_DIR` verwenden, um den Pfad zum Stammordner des Modells abzurufen. Die zugrunde liegende Verzeichnisstruktur wird beibehalten.

## <a name="understand-the-scoring-script"></a>Grundlegendes zum Bewertungsskript

> [!Tip]
> Das Format des Bewertungsskripts für verwaltete Onlineendpunkte entspricht dem Format, das in der vorherigen Version der CLI und im Python SDK verwendet wurde.

Wie im obigen YAML-Code erwähnt, muss `code_configuration.scoring_script` über die Funktionen `init()` und `run()` verfügen. In diesem Beispiel wird diese [score.py-Datei](https://github.com/Azure/azureml-examples/blob/main/cli/endpoints/online/model-1/onlinescoring/score.py) verwendet. Die `init()`-Funktion wird aufgerufen, wenn der Container initialisiert/gestartet wird. Diese Initialisierung erfolgt in der Regel kurz nach dem Erstellen oder Aktualisieren der Bereitstellung. Schreiben Sie hier eine Logik, um globale Initialisierungsvorgänge wie das Zwischenspeichern des Modells im Arbeitsspeicher durchzuführen (wie in diesem Beispiel). Die Funktion `run()` wird für jeden Aufruf des Endpunkts aufgerufen und sollte die tatsächliche Bewertung/Vorhersage ausführen. Im Beispiel extrahieren wir die Daten aus der JSON-Eingabe, rufen die `predict()`-Methode des `scikit-learn`-Modells auf und geben das Ergebnis zurück.

## <a name="deploy-and-debug-locally-using-local-endpoints"></a>Lokales Bereitstellen und Debuggen mit lokalen Endpunkten

Um Zeit beim Debuggen zu sparen, **wird dringend empfohlen**, den Endpunkt lokal zu testen.

> [!Note]
> * Für die lokale Bereitstellung muss die [Docker-Engine](https://docs.docker.com/engine/install/) installiert sein.
> * Ihre Docker-Engine muss ausgeführt werden. Typischerweise wird die Engine beim Start gestartet. Wenn dies nicht der Falls ist, können Sie die [Problembehandlung hier durchführen](https://docs.docker.com/config/daemon/#start-the-daemon-manually).

> [!Important]
> Das Ziel einer lokalen Endpunktbereitstellung besteht darin, Ihren Code und Ihre Konfiguration vor der Bereitstellung in Azure zu überprüfen und zu debuggen. Die lokale Bereitstellung weist die folgenden Einschränkungen auf:
> - Lokale Endpunkte unterstützen **keine** Datenverkehrsregeln, Authentifizierung, Skalierungseinstellungen oder Testeinstellungen. 
> - Lokale Endpunkte unterstützen nur eine Bereitstellung pro Endpunkt. Das heißt, dass Sie in einer lokalen Bereitstellung keinen Verweis auf ein Modell oder eine Umgebung verwenden können, das bzw. die in Ihrem Azure Machine Learning-Arbeitsbereich registriert ist. 

### <a name="deploy-the-model-locally"></a>Lokales Bereitstellen des Modells

Führen Sie den folgenden Befehl aus, um das Modell lokal bereitzustellen:

```azurecli
az ml endpoint create --local -n $ENDPOINT_NAME -f endpoints/online/managed/simple-flow/1-create-endpoint-with-blue.yml
```

Das `--local`-Flag weist die CLI an, den Endpunkt in der Docker-Umgebung bereitzustellen.

>[!NOTE]
>Bei einem Windows-Betriebssystem verwenden Sie `%ENDPOINT_NAME%` hier und in nachfolgenden Befehlen anstelle von `$ENDPOINT_NAME`.

### <a name="check-if-the-local-deployment-succeeded"></a>Überprüfen Sie, ob die lokale Bereitstellung erfolgreich war.

Überprüfen Sie anhand der Protokolle, ob das Modell ohne Fehler bereitgestellt wurde:

```azurecli
az ml endpoint get-logs --local -n $ENDPOINT_NAME --deployment blue
```

### <a name="invoke-the-local-endpoint-to-score-data-with-your-model"></a>Aufrufen des lokalen Endpunkts zum Bewerten von Daten mit Ihrem Modell

Rufen Sie den Endpunkt auf, um das Modell mithilfe des praktischen Befehls `invoke` und der Übergabe von in einer JSON-Datei gespeicherten Abfrageparametern zu bewerten:

```azurecli
az ml endpoint invoke --local -n $ENDPOINT_NAME --request-file endpoints/online/model-1/sample-request.json
```

Wenn Sie einen REST-Client (z. B. curl) verwenden möchten, benötigen Sie die Bewertungs-URI. Sie können sie über den Befehl `az ml endpoint show --local -n $ENDPOINT_NAME` abrufen. In den zurückgegebenen Daten finden Sie ein Attribut namens `scoring_uri`. 

### <a name="review-the-logs-for-output-from-the-invoke-operation"></a>Überprüfen der Protokolle hinsichtlich der Ausgaben des Aufrufvorgangs

Im Beispiel `score.py` protokolliert die `run()`-Methode einige Ausgaben in der Konsole. Sie können diese Ausgabe mit dem Befehl `get-logs` erneut anzeigen:

```azurecli
az ml endpoint get-logs --local -n $ENDPOINT_NAME --deployment blue
```

##  <a name="deploy-your-managed-online-endpoint-to-azure"></a>Bereitstellen Ihres verwalteten Onlineendpunkts in Azure 

### <a name="deploy-to-azure"></a>In Azure bereitstellen

Führen Sie den folgenden Befehl aus, um die YAML-Konfiguration in der Cloud bereitzustellen:

::: code language="azurecli" source="~/azureml-examples-main/cli/deploy-managed-online-endpoint.sh" ID="deploy" :::

Diese Bereitstellung dauert etwa bis zu 15 Minuten – je nachdem, ob die zugrunde liegende Umgebung bzw. das zugrunde liegende Image zum ersten Mal erstellt wird. Nachfolgende Bereitstellungen mit der gleichen Umgebung sind schneller.

> [!Tip]
> Wenn Sie ihre CLI-Konsole nicht blockieren möchten, können Sie dem Befehl das Flag `--no-wait` hinzufügen. Dadurch wird jedoch die interaktive Anzeige des Bereitstellungsstatus nicht mehr angezeigt.

> [!Tip]
> Verwenden Sie [Problembehandlung für die Bereitstellung verwalteter Onlineendpunkte](how-to-troubleshoot-managed-online-endpoints.md), um Fehler zu debuggen.

### <a name="check-the-status-of-the-deployment"></a>Überprüfen Sie den Status der Bereitstellung.

Der Befehl `show` enthält `provisioning_status` sowohl für den Endpunkt als auch für die Bereitstellung:

::: code language="azurecli" source="~/azureml-examples-main/cli/deploy-managed-online-endpoint.sh" ID="get_status" :::

Sie können alle Endpunkte im Arbeitsbereich in einem Tabellenformat mit dem Befehl `list` auflisten:

```azurecli
az ml endpoint list --output table
```

### <a name="check-if-the-cloud-deployment-succeeded"></a>Überprüfen Sie, ob die Cloudbereitstellung erfolgreich war.

Überprüfen Sie anhand der Protokolle, ob das Modell ohne Fehler bereitgestellt wurde:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-managed-online-endpoint.sh" ID="get_logs" :::

Standardmäßig werden Protokolle per Pullvorgang vom Rückschlussserver abgerufen. Wenn Sie die Protokolle aus dem Speicherinitialisierer (der die Ressourcen wie Modell und Code in den Container einbindet) anzeigen möchten, fügen Sie das Flag `--container storage-initializer` hinzu.

### <a name="invoke-the-endpoint-to-score-data-with-your-model"></a>Aufrufen des Endpunkts zum Bewerten von Daten mit Ihrem Modell

Sie können entweder den `invoke`-Befehl oder einen REST-Client Ihrer Wahl verwenden, um den Endpunkt aufzurufen und einige Daten zu bewerten: 

::: code language="azurecli" source="~/azureml-examples-main/cli/deploy-managed-online-endpoint.sh" ID="test_endpoint" :::

Sie können den oben gezeigten Befehl `get-logs` erneut verwenden, um die Aufrufprotokolle anzuzeigen.

Sie benötigen den `scoring_uri` und den Authentifizierungsschlüssel bzw. das Authentifizierungstoken, um einen REST-Client zu verwenden. Der `scoring_uri` ist in der Ausgabe des Befehls `show` verfügbar:
 
::: code language="azurecli" source="~/azureml-examples-main/cli/deploy-managed-online-endpoint.sh" ID="get_scoring_uri" :::

Beachten Sie, dass wir die `--query` verwenden, um nur nach erforderlichen Attributen zu filtern. Weitere Informationen zu `--query` finden Sie unter [Abfragen der Azure CLI-Befehlsausgabe](/cli/azure/query-azure-cli).

Rufen Sie die erforderlichen Anmeldeinformationen mit dem Befehl `get-credentials` ab:

```azurecli
az ml endpoint get-credentials -n $ENDPOINT_NAME
```

### <a name="optional-update-the-deployment"></a>[Optional] Aktualisieren der Bereitstellung

Wenn Sie den Code, das Modell, die Umgebung oder Ihre Skalierungseinstellungen aktualisieren möchten, aktualisieren Sie die YAML-Datei, und führen Sie den Befehl `az ml endpoint update` aus. 

>[!IMPORTANT]
> Sie können nur **einen** Aspekt (Datenverkehr, Skalierungseinstellungen, Code, Modell oder Umgebung) in einem einzelnen `update`-Befehl ändern. 

Informationen zur Funktionsweise von `update`:

1. Öffnen Sie die Datei `online/model-1/onlinescoring/score.py`.
1. Ändern Sie die letzte Zeile der `init()`-Funktion: Fügen Sie nach `logging.info("Init complete")` einfach `logging.info("Updated successfully")` hinzu. 
1. Speichern Sie die Datei.
1. Führen Sie den folgenden Befehl aus:
```azurecli
az ml endpoint update -n $ENDPOINT_NAME -f endpoints/online/managed/simple-flow/1-create-endpoint-with-blue.yml
```

> [!IMPORTANT]
> Die Aktualisierung per YAML-Code ist deklarativ. Dies bedeutet, dass Änderungen am YAML-Code in den zugrunde liegenden Azure Resource Manager-Ressourcen (Endpunkte und Bereitstellungen) widergespiegelt werden. Bei diesem Ansatz kann [GitOps](https://www.atlassian.com/git/tutorials/gitops) genutzt werden: *ALLE* Änderungen an Endpunkten bzw. Bereitstellungen erfolgen über den YAML-Code (auch `instance_count`). Dies ist mit der folgenden Nebenwirkung verbunden: Wenn Sie eine Bereitstellung aus dem YAML-Code entfernen und `az ml endpoint update` mit der Datei ausführen, wird die Bereitstellung gelöscht. Sie können Updates ohne Verwendung des YAML-Codes mithilfe des `--set `-Flags durchführen, wie im folgenden Tipp beschrieben.

5. Da Sie die Funktion `init()` geändert haben, die ausgeführt wird, wenn der Endpunkt erstellt oder aktualisiert wird, befindet sich die Meldung `Updated successfully` in den Protokollen. Rufen Sie die Protokolle ab, indem Sie Folgendes ausführen:
```azurecli
az ml endpoint get-logs -n $ENDPOINT_NAME --deployment blue
```

Verwenden Sie Folgendes in dem seltenen Fall, dass Sie Ihre Bereitstellung aufgrund eines unlösbaren Problems löschen und neu erstellen möchten:

```azurecli
az ml endpoint delete -n $ENDPOINT_NAME --deployment blue
```

Der Befehl `update` funktioniert auch mit lokalen Endpunkten. Verwenden Sie denselben `az ml endpoint update`-Befehl mit dem Flag `--local`.

> [!Tip]
> Mit dem Befehl `az ml endpoint update` können Sie den in der [Azure CLI verfügbaren `--set`-Parameter](/cli/azure/use-cli-effectively#generic-update-arguments) verwenden, um Attribute in Ihrer YAML-Datei außer Kraft zu setzen **oder** bestimmte Attribute ohne Übergabe der YAML-Datei festzulegen. Die Verwendung von `--set` für einzelne Attribute ist besonders in Dev/Testszenarien nützlich. Sie können das Flag `--set deployments[0].scale_settings.instance_count=2` verwenden, um z. B. den `instance_count` der ersten Bereitstellung hochzuskalieren. Da die YAML-Datei jedoch nicht aktualisiert wird, unterstützt dieses Verfahren [GitOps](https://www.atlassian.com/git/tutorials/gitops) nicht.

### <a name="optional-monitor-sla-using-azure-monitor"></a>[Optional] Überwachen der SLA mit Azure Monitor

Sie können Metriken anzeigen und Warnungen basierend auf Ihrer SLA festlegen, indem Sie die Anweisungen unter [Überwachen verwalteter Onlineendpunkte](how-to-monitor-online-endpoints.md) befolgen.

### <a name="optional-integrate-with-log-analytics"></a>[Optional] Integrieren mit Log Analytics

Der Befehl `get-logs` stellt nur die letzten paar hundert Protokollzeilen aus einer automatisch ausgewählten Instanz zur Verfügung. Log Analytics bietet jedoch eine Möglichkeit, Protokolle dauerhaft zu speichern und zu analysieren. Führen Sie zunächst die Schritte unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../azure-monitor/logs/quick-create-workspace.md#create-a-workspace) aus, um einen Log Analytics-Arbeitsbereich zu erstellen.

Gehen Sie dann im Azure-Portal wie folgt vor:

1. Wechseln Sie zur Ressourcengruppe.
1. Wählen Sie Ihren Endpunkt aus.
1. Wählen Sie die Seite **ARM-Ressource** aus.
1. Wählen Sie **Diagnoseeinstellungen** aus.
1. Wählen Sie **Einstellungen hinzufügen** aus: Aktivieren Sie das Senden von Konsolenprotokollen an den Log Analytics-Arbeitsbereich.

Beachten Sie, dass es bis zu einer Stunde dauern kann, bis die Protokolle verbunden sind. Senden Sie nach diesem Zeitraum einige Bewertungsanforderungen, und überprüfen Sie dann die Protokolle mithilfe der folgenden Schritte:

1. Öffnen des Log Analytics-Arbeitsbereichs 
1. Wählen Sie im linken Navigationsbereich **Protokolle** aus.
1. Schließen Sie das Popupfenster **Abfragen**, das automatisch geöffnet wird.
1. Doppelklicken Sie auf **AmlOnlineEndpointConsoleLog**.
1. Wählen Sie *Ausführen* aus.

## <a name="delete-the-endpoint-and-deployment"></a>Löschen des Endpunkts und der Bereitstellung

Wenn Sie die Bereitstellung nicht verwenden, sollten Sie sie mit dem folgenden Befehl löschen (der Endpunkt und alle zugrunde liegenden Bereitstellungen werden gelöscht):

::: code language="azurecli" source="~/azureml-examples-main/cli/deploy-managed-online-endpoint.sh" ID="delete_endpoint" :::

## <a name="next-steps"></a>Nächste Schritte

- [Sicherer Rollout für Onlineendpunkte (Vorschau)](how-to-safely-rollout-managed-endpoints.md)
- [Problembehandlung für die Bereitstellung verwalteter Onlineendpunkte](how-to-troubleshoot-managed-online-endpoints.md)