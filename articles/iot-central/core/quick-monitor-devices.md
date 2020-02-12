---
title: 'Schnellstart: Überwachen Ihrer Geräte in Azure IoT Central'
description: Hier erfahren Sie, wie Sie als Bediener mit der Azure IoT Central-Anwendung Ihre Geräte überwachen können.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: e5185d78b0ffc17e861f49d294af65ddcd200d07
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026163"
---
# <a name="quickstart-use-azure-iot-central-to-monitor-your-devices"></a>Schnellstart: Überwachen Ihrer Geräte mithilfe von Azure IoT Central

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

Diese Schnellstartanleitung richtet sich an Bediener und erläutert, wie Sie mithilfe Ihrer Microsoft Azure IoT Central-Anwendung Ihre Geräte überwachen und Einstellungen ändern.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, sollten Sie die drei vorherigen Schnellstartanleitungen durcharbeiten: [Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)](./quick-deploy-iot-central.md), [Schnellstart: Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung (Previewfunktionen)](./quick-create-pnp-device.md) und [Schnellstart: Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central (Previewfunktionen)](quick-configure-rules.md).

## <a name="receive-a-notification"></a>Empfangen einer Benachrichtigung

Azure IoT Central sendet Benachrichtigungen für Geräte in Form von E-Mails. Der Ersteller hat eine Regel hinzugefügt, die dafür sorgt, dass eine Benachrichtigung gesendet wird, wenn die Temperatur in einem verbundenen Gerätesensor einen Schwellenwert übersteigt. Überprüfen Sie die E-Mails, die an das Konto gesendet wurden, das der Ersteller für den Empfang von Benachrichtigungen ausgewählt hat.

Öffnen Sie die E-Mail, die Sie am Ende der Schnellstartanleitung [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](quick-configure-rules.md) erhalten haben. Wählen Sie in der E-Mail den Link zum Gerät aus:

![Benachrichtigungs-E-Mail mit Warnung](media/quick-monitor-devices/email.png)

Die Ansicht **Übersicht** für das simulierte Gerät, das Sie in den vorherigen Schnellstartanleitungen erstellt haben, wird in Ihrem Browser geöffnet:

![Gerät, das die Benachrichtigungs-E-Mail ausgelöst hat](media/quick-monitor-devices/dashboard.png)

## <a name="investigate-an-issue"></a>Untersuchen eines Problems

Als Bediener können Sie in den Ansichten **Übersicht**, **Info** und **Befehle** Informationen zum Gerät anzeigen. Der Ersteller hat die Ansicht **Gerät verwalten** erstellt, damit Sie Geräteinformationen bearbeiten und Geräteeigenschaften festlegen können.

Wählen Sie die Ansicht **Übersicht** aus, um Informationen zum Gerät anzuzeigen.

Das Diagramm auf dem Dashboard zeigt einen Plot der Gerätetemperatur. Sie entscheiden, dass die Gerätetemperatur zu hoch ist.

## <a name="remediate-an-issue"></a>Beheben eines Problems

Eine Änderung am Gerät können Sie über die Seite **Gerät verwalten** vornehmen.

Ändern Sie die Angabe für **Lüfterdrehzahl** in „500“, um das Gerät zu kühlen. Wählen Sie **Speichern** aus, um das Gerät zu aktualisieren. Wenn das Gerät die Einstellungsänderung bestätigt, ändert sich der Status der Eigenschaft in **Synchronisiert**:

![Aktualisieren der Einstellungen](media/quick-monitor-devices/change-settings.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

* Empfangen einer Benachrichtigung
* Untersuchen eines Problems
* Beheben eines Problems

Nachdem Sie nun wissen, wie Sie Ihr Gerät überwachen, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Einrichten und Verwalten einer Gerätevorlage (Previewfunktionen)](howto-set-up-template.md)
