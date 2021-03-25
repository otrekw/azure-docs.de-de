---
title: Verbinden eines Azure Sphere-Geräts in Azure IoT Central | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Azure Sphere-Gerät (DevKit) mit einer Azure IoT Central-Anwendung verbinden.
services: iot-central
ms.service: iot-central
ms.topic: how-to
ms.author: sandeepu
author: sandeeppujar
ms.date: 04/30/2020
ms.custom: device-developer
ms.openlocfilehash: 770f6e56a669ab2d9b425a7a2879eeef5d37377b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92123422"
---
# <a name="connect-an-azure-sphere-device-to-your-azure-iot-central-application"></a>Verbinden eines Azure Sphere-Geräts mit Ihrer Azure IoT Central-Anwendung

*Dieser Artikel gilt für Geräteentwickler.*

In diesem Artikel wird gezeigt, wie Sie ein Azure Sphere-Gerät (DevKit) mit einer Azure IoT Central-Anwendung verbinden.

Azure Sphere ist eine abgesicherte, Anwendungsplattform auf hoher Ebene mit integrierten Kommunikations- und Sicherheitsfunktionen für mit dem Internet verbundene Geräte. Sie umfasst einen geschützten, verbundenen Crossover-Mikrocontroller (MPU), ein benutzerdefiniertes allgemeines Linux-basiertes Betriebssystem (Operating System, OS) sowie einen cloudbasierten Sicherheitsdienst, der für kontinuierliche und erneuerbare Sicherheit sorgt. Weitere Informationen finden Sie unter [Was ist Azure Sphere?](/azure-sphere/product-overview/what-is-azure-sphere).

[Azure Sphere Development Kits](https://azure.microsoft.com/services/azure-sphere/get-started/) bieten alles, was Sie zum Starten von Prototypen und Entwickeln von Azure Sphere-Anwendungen benötigen. Azure IoT Central mit Azure Sphere ermöglicht einen End-to-End-Stapel für eine IoT-Lösung. Azure Sphere stellt die Geräteunterstützung und IoT Central als eine verwaltete IoT-Anwendungsplattform ohne Code bereit.

In diesem Anleitungsartikel werden die folgenden Aufgaben beschrieben:

- Erstellen Sie mithilfe der Azure Sphere DevKit-Gerätevorlage aus der Bibliothek ein Azure Sphere-Gerät in IoT Central.
- Bereiten Sie das Azure Sphere DevKit-Gerät für Azure IoT vor.
- Verbinden Sie Azure Sphere DevKit mit Azure IoT Central.
- Zeigen Sie die Telemetriedaten vom Gerät in IoT Central an.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in diesem Artikel benötigen Sie folgende Ressourcen:

- Eine Azure IoT Central-Anwendung.
- Visual Studio 2019, Version 16.4 oder höher.
- Ein [Azure Sphere MT3620 Development Kit von Seeed Studios](/azure-sphere/hardware/mt3620-reference-board-design).

> [!NOTE]
> Wenn Sie kein physisches Gerät haben, fahren Sie nach dem ersten Schritt mit dem letzten Abschnitt fort, um ein simuliertes Gerät zu testen.

## <a name="create-the-device-in-iot-central"></a>Erstellen des Geräts in IoT Central

So erstellen Sie ein Azure Sphere-Gerät in IoT Central:

1. Wählen Sie in Ihrer Azure IoT Central-Anwendung die Registerkarte **Gerätevorlagen** und dann **+ Neu** aus. Wählen Sie im Abschnitt **Eine ausgewählte Gerätevorlage verwenden** die Option **Azure Sphere-Beispielgerät** aus.

    :::image type="content" source="media/howto-connect-sphere/sphere-create-template.png" alt-text="Gerätevorlage für Azure Sphere DevKit":::

1. Bearbeiten Sie in der Gerätevorlage die Ansicht **Übersicht**, um **Temperatur**  und **Button Press** (Schaltfläche drücken) anzuzeigen.

1. Wählen Sie den Ansichtstyp **Geräte- und Clouddaten werden bearbeitet** aus, um eine weitere Ansicht hinzuzufügen, in der die Lese-/Schreibeigenschaft **Status-LED** angezeigt wird. Ziehen Sie die Eigenschaft **Status-LED** auf das leere, gepunktete Rechteck rechts im Formular. Wählen Sie **Speichern** aus.

## <a name="prepare-the-device"></a>Vorbereiten des Geräts

Bevor Sie das Azure Sphere DevKit-Gerät mit IoT Central verbinden können, müssen Sie [die Geräte-und Entwicklungsumgebung](https://github.com/Azure/azure-sphere-samples/tree/master/Samples/AzureIoT) einrichten.

## <a name="connect-the-device"></a>Verbinden des Geräts

Um das Beispiel zum Verbinden mit IoT Central aktivieren zu können, müssen Sie [eine Azure IoT Central-Anwendung konfigurieren und dann das Anwendungsmanifest des Beispiels ändern](https://aka.ms/iotcentral-sphere-git-readme).

## <a name="view-the-telemetry-from-the-device"></a>Anzeigen der Telemetriedaten vom Gerät

Wenn das Gerät mit IoT Central verbunden ist, können Sie die Telemetriedaten im Dashboard anzeigen.

:::image type="content" source="media/howto-connect-sphere/sphere-view.png" alt-text="Dashboard für Azure Sphere DevKit":::

## <a name="create-a-simulated-device"></a>Erstellen Sie ein simuliertes Gerät.

Wenn Sie kein physisches Azure Sphere DevKit-Gerät haben, können Sie ein simuliertes Gerät zum Testen der Azure IoT Central-Anwendung erstellen.

So erstellen Sie ein simuliertes Gerät:

- Wählen Sie **Geräte > Azure IoT Sphere** aus.
- Wählen Sie **+ Neu** aus.
- Geben Sie eine eindeutige **Geräte-ID** und einen benutzerfreundlichen **Gerätenamen** ein.
- Aktivieren Sie die Einstellung **Simuliert**.
- Klicken Sie auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie ein Geräteentwickler sind, werden einige der folgenden nächsten Schritte empfohlen:

- Informieren Sie sich zu [Gerätekonnektivität in Azure IoT Central](./concepts-get-connected.md).
- Informieren Sie sich über [Überwachen der Gerätekonnektivität per Azure CLI](./howto-monitor-devices-azure-cli.md).