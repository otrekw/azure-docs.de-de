---
title: Verwenden der Azure IoT-Erweiterung für Azure CLI zum Interagieren mit IoT Plug & Play-Vorschaugeräten | Microsoft-Dokumentation
description: Installieren Sie die Azure IoT-Erweiterung für Azure CLI, und verwenden Sie sie zum Interagieren mit den IoT Plug &Play-Geräten, die mit Ihrem IoT-Hub verbunden sind.
author: dominicbetts
ms.author: dobett
ms.date: 07/20/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: dadb1f044547acd6e5f0d274143123e89d7dae46
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475480"
---
# <a name="install-and-use-the-azure-iot-extension-for-the-azure-cli"></a>Installieren und Verwenden der Azure IoT-Erweiterung für die Azure CLI

[Die Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) (Befehlszeilenschnittstelle) ist ein plattformübergreifendes Open-Source-Befehlszeilentool zum Verwalten von Azure-Ressourcen wie z.B. IoT Hub. Die Azure CLI ist unter Windows, Linux und macOS verfügbar. Mit der Azure CLI können Sie Azure IoT Hub-Ressourcen, Device Provisioning-Dienstinstanzen und verknüpfte Hubs verwalten, ohne Erweiterungen installieren zu müssen.

Die Azure IoT-Erweiterung für die Azure CLI ist ein Befehlszeilentool zum Interagieren mit und Testen von IoT Plug & Play-Vorschaugeräten. Sie können die Erweiterung für folgende Aufgaben verwenden:

- Herstellen einer Verbindung mit einem Gerät.
- Anzeigen der vom Gerät gesendeten Telemetriedaten.
- Arbeiten mit Geräteeigenschaften.
- Aufrufen von Gerätebefehlen.

In diesem Artikel lernen Sie Folgendes:

- Installieren und Konfigurieren der Azure IoT-Erweiterung für die Azure CLI.
- Verwenden der Erweiterung zum Interagieren mit Ihren Geräten und Testen Ihrer Geräte.

## <a name="install-azure-iot-extension-for-the-azure-cli"></a>Installieren der Azure IoT-Erweiterung für die Azure CLI

### <a name="step-1---install-the-azure-cli"></a>Schritt 1: Installieren der Azure CLI

Folgen Sie den [Installationsanleitungen](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) zum Einrichten der Azure CLI in Ihrer Umgebung. Zur Erzielung der optimalen Leistung sollten Sie die Azure CLI-Version 2.9.1 oder höher verwenden. Verwenden Sie `az -–version`, um dies zu überprüfen.

### <a name="step-2---install-iot-extension"></a>Schritt 2: Installieren der IoT-Erweiterung

In der [Infodatei zur IoT-Erweiterung](https://github.com/Azure/azure-iot-cli-extension) werden mehrere Wege zum Installieren der Erweiterung beschrieben. Die einfachste Möglichkeit ist die Ausführung von `az extension add --name azure-iot`. Nach der Installation können Sie `az extension list` verwenden, um die derzeit installierten Erweiterungen zu überprüfen, oder mit `az extension show --name azure-iot` Details zur IoT-Erweiterung anzeigen. Zum Zeitpunkt der Artikelerstellung lautet die Versionsnummer der Erweiterung `0.9.7`.

Sie können `az extension remove --name azure-iot` verwenden, um die Erweiterung zu entfernen.

## <a name="use-azure-iot-extension-for-the-azure-cli"></a>Verwenden der Azure IoT-Erweiterung für die Azure CLI

### <a name="prerequisites"></a>Voraussetzungen

Führen Sie den folgenden Befehl aus, um sich bei Ihrem Azure-Abonnement anzumelden:

```azurecli
az login
```

Um die Azure IoT-Erweiterung für die Azure CLI verwenden zu können, benötigen Sie Folgendes:

- Einen Azure IoT Hub. Es gibt viele Möglichkeiten, um einen IoT-Hub zu Ihrem Azure-Abonnement hinzuzufügen, z.B. das [Erstellen eines IoT-Hubs mit der Azure CLI](../iot-hub/iot-hub-create-using-cli.md). Sie benötigen die Verbindungszeichenfolge für den IoT-Hub, um die Befehle für die Azure IoT-Erweiterung ausführen zu können. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Ein in Ihrem IoT-Hub registriertes Gerät. Sie können den folgenden Azure CLI-Befehl zum Registrieren eines Geräts verwenden. Ersetzen Sie darin die Platzhalter `{YourIoTHubName}` und `{YourDeviceID}` durch Ihre Werte:

    ```azurecli
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

### <a name="interact-with-a-device"></a>Interagieren mit einem Gerät

Sie können die Erweiterung nutzen, um die mit einem IoT-Hub verbundenen IoT Plug & Play-Geräte anzuzeigen und damit zu interagieren. Die Erweiterung funktioniert mit dem digitalen Zwilling, der das IoT Plug & Play-Gerät darstellt.

#### <a name="list-devices"></a>Listen Sie die Geräte auf.

Auflisten aller Geräte auf einem IoT-Hub:

```azurecli
az iot hub device-identity list --hub-name {YourIoTHubName}
```

#### <a name="view-digital-twin"></a>Anzeigen des digitalen Zwillings

So zeigen Sie den digitalen Zwilling eines Geräts an:

```azurecli
az iot pnp twin show -n {iothub_name} -d {device_id}
```

#### <a name="properties"></a>Eigenschaften

Festlegen des Werts für eine Lese-/Schreibeigenschaft:

```azurecli
az iot pnp twin update --hub-name {iothub_name} --device-id {device_id} --json-patch '{"op":"add", "path":"/thermostat1/targetTemperature", "value": 54}'
```

#### <a name="commands"></a>Befehle

Aufrufen eines Befehls:

```azurecli
az iot pnp twin invoke-command --cn getMaxMinReport -n {iothub_name} -d {device_id} --component-path thermostat1
```

#### <a name="digital-twin-events"></a>Digital Twins-Ereignisse

Überwachen aller IoT Plug & Play-Digital Twins-Ereignisse, die zur Event Hub-Consumergruppe **$Default** gesendet werden, von einem bestimmten Gerät und einer bestimmten Schnittstelle aus:

```azurecli
az iot hub monitor-events -n {iothub_name} -d {device_id} -i {interface_id}
```

### <a name="manage-models-in-the-model-repository"></a>Verwalten von Modellen im Modellrepository

Sie können die Azure CLI-Befehle für Modellrepositorys verwenden, um Modelle im Repository zu verwalten.

#### <a name="create-model-repository"></a>Erstellen eines Modellrepositorys

Erstellen Sie ein neues IoT Plug & Play-Unternehmensrepository für Ihren Mandanten, wenn Sie der erste Benutzer in diesem Mandanten sind:

```azurecli
az iot pnp repo create
```

#### <a name="manage-model-repository-tenant-roles"></a>Verwalten von Mandantenrollen für das Modellrepository

Erstellen Sie für einen Benutzer oder Dienstprinzipal eine Rollenzuweisung zu einer bestimmten Ressource.

Weisen Sie beispielsweise user@consoso.com die Rolle **ModelsCreator** für den Mandanten zu:

```azurecli
az iot pnp role-assignment create --resource-id {tenant_id} --resource-type Tenant --subject-id {user@contoso.com} --subject-type User --role ModelsCreator
```

Oder weisen Sie user@consoso.com die Rolle **ModelAdministrator** für ein bestimmtes Modell zu:

```azurecli
az iot pnp role-assignment create --resource-id {model_id} --resource-type Model --subject-id {user@contoso.com} --subject-type User --role ModelAdministrator
```

#### <a name="create-a-model"></a>Erstellen eines Modells

Erstellen Sie ein neues Modell im Unternehmensrepository:

```azurecli
az iot pnp model create --model {model_json or path_to_file}
```

#### <a name="search-a-model"></a>Suchen nach einem Modell

Listen Sie Modelle auf, die einem bestimmten Schlüsselwort entsprechen:

```azurecli
az iot pnp model list -q {search_keyword}
```

#### <a name="publish-a-model"></a>Veröffentlichen eines Modells

Veröffentlichen Sie ein Gerätemodell, das sich im Unternehmensrepository befindet, im öffentlichen Repository.

Machen Sie beispielsweise das Modell mit der ID `dtmi:com:example:ClimateSensor;1` öffentlich:

```azurecli
az iot pnp model publish --dtmi "dtmi:com:example:ClimateSensor;1"
```

Zum Veröffentlichen eines Modells müssen die folgenden Anforderungen erfüllt sein:

- Der Unternehmens- bzw. Organisationsmandant muss ein Microsoft-Partner sein. 
- Der Benutzer oder Dienstprinzipal muss Mitglied der Rolle **Herausgeber** des Repositorymandanten sein.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie die Azure IoT-Erweiterung für die Azure CLI installieren und für die Interaktion mit Ihren IoT Plug & Play-Geräten verwenden. Als nächster Schritt wird empfohlen, dass Sie sich darüber informieren, wie [Azure IoT-Explorer bei Ihren Geräten](./howto-use-iot-explorer.md) verwendet wird.
