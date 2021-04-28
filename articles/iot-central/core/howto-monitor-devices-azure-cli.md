---
title: Überwachen der Gerätekonnektivität mit dem Azure IoT Central-Explorer
description: Überwachen Sie Gerätemeldungen, und beobachten Sie Änderungen bei Gerätezwillingen über die Befehlszeilenschnittstelle des IoT Central-Explorers.
author: viv-liu
ms.author: viviali
ms.date: 03/27/2020
ms.topic: how-to
ms.service: iot-central
ms.custom: devx-track-azurecli, device-developer
services: iot-central
manager: corywink
ms.openlocfilehash: 30a823b7e78145224929b427e9e37522a7e29f09
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107871289"
---
# <a name="monitor-device-connectivity-using-azure-cli"></a>Überwachen der Gerätekonnektivität per Azure CLI

*Dieses Thema gilt für Geräte- und Lösungsentwickler.*

Verwenden Sie die IoT-Erweiterung der Azure CLI, um Meldungen anzuzeigen, die Ihre Geräte an IoT Central senden und Änderungen im Gerätezwilling zu beobachten. Mit diesem Tool können Sie die Gerätekonnektivität debuggen und beobachten und Probleme von Gerätemeldungen diagnostizieren, die die Cloud oder Geräte nicht erreichen, die auf Änderungen beim Zwilling nicht reagieren.

[Weitere Einzelheiten erhalten Sie in der Referenz zu Azure CLI-Erweiterungen.](/cli/azure/iot/central)

## <a name="prerequisites"></a>Voraussetzungen

+ Die Azure CLI (Version 2.7.0 oder höher) muss installiert sein. Überprüfen Sie die Version der Azure CLI, indem Sie `az --version`ausführen. Informieren Sie sich in der [Azure CLI-Dokumentation](/cli/azure/install-azure-cli) über Installation und Aktualisierung.
+ Ein Geschäfts-, Schul- oder Unikonto in Azure, hinzugefügt als Benutzer in einer IoT Central-Anwendung.

## <a name="install-the-iot-central-extension"></a>Installieren der IoT Central-Erweiterung

Führen Sie den folgenden Befehl an der Befehlszeile zum Installieren aus:

```azurecli
az extension add --name azure-iot
```

Überprüfen Sie die Version der Erweiterung, indem Sie Folgendes ausführen:

```azurecli
az --version
```

Die Version der Erweiterung „azure-iot“, 0.9.9 oder höher, sollte angezeigt werden. Führen Sie andernfalls Folgendes aus:

```azurecli
az extension update --name azure-iot
```

## <a name="using-the-extension"></a>Verwenden der Erweiterung

Die folgenden Abschnitte beschreiben allgemeine Befehle und Optionen, die Sie beim Ausführen von `az iot central` verwenden können. Wenn Sie die vollständige Sammlung von Befehlen und Optionen anzeigen möchten, übergeben Sie `--help` an `az iot central` oder einen seiner Unterbefehle.

### <a name="login"></a>Anmeldename

Melden Sie zuerst bei der Azure CLI an. 

```azurecli
az login
```

### <a name="get-the-application-id-of-your-iot-central-app"></a>Abrufen der Anwendungs-ID Ihrer IoT Central-App
Kopieren Sie in **Verwaltung/Anwendungseinstellungen** die **Anwendungs-ID**. Sie verwenden diesen Wert in späteren Schritten.

### <a name="monitor-messages"></a>Überwachen von Meldungen
Überwachen Sie die Nachrichten, die von ihren Geräten an die IoT Central-App gesendet werden. Zu der Ausgabe zählen auch alle Header und Anmerkungen.

```azurecli
az iot central diagnostics monitor-events --app-id <app-id> --properties all
```

### <a name="view-device-properties"></a>Anzeigen von Geräteeigenschaften
Zeigen Sie die aktuellen Lesen- und Lesen/Schreiben-Geräteeigenschaften für ein bestimmtes Gerät an.

```azurecli
az iot central device twin show --app-id <app-id> --device-id <device-id>
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie ein Geräteentwickler sind, sollten Sie sich als Nächstes über [Gerätekonnektivität in Azure IoT Central](./concepts-get-connected.md) informieren.