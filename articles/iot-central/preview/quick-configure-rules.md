---
title: 'Schnellstart: Konfigurieren von Regeln und Aktionen in Azure IoT Central'
description: In dieser Schnellstartanleitung wird gezeigt, wie Sie als Ersteller telemetriebasierte Regeln und Aktionen in Ihrer Azure IoT Central-Anwendung konfigurieren.
author: dominicbetts
ms.author: dobett
ms.date: 10/24/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: f81171d0f545bbf7f903d54bee761817e9451d9d
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74706720"
---
# <a name="quickstart-configure-rules-and-actions-for-your-device-in-azure-iot-central-preview-features"></a>Schnellstart: Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central (Previewfunktionen)

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In dieser Schnellstartanleitung erstellen Sie eine Regel, die eine E-Mail sendet, wenn die Temperatur in einem Umgebungssensor 90 &deg;F übersteigt.

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie zunächst mithilfe der beiden vorherigen Schnellstartanleitungen [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md) und [Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung](./quick-create-pnp-device.md) die Gerätevorlage **Environment Sensor** (Umgebungssensor), um sie hier verwenden zu können.

## <a name="create-a-telemetry-based-rule"></a>Erstellen einer telemetriebasierten Regel

1. Wählen Sie im linken Bereich die Option **Regeln** aus, um Ihrer Anwendung eine neue telemetriebasierte Regel hinzuzufügen.

1. Wählen Sie **+ Neu** aus, um eine neue Regel zu erstellen.

1. Geben Sie als Regelname **Environmental temperature** (Umgebungstemperatur) ein.

1. Wählen Sie im Abschnitt **Zielgeräte** die Gerätevorlage **Environment Sensor** (Umgebungssensor) aus. Diese Option filtert die Geräte, auf die die Regel angewendet wird, nach Gerätevorlagentyp. Mithilfe von **+ Filter** können Sie weitere Filterkriterien hinzufügen.

1. Im Abschnitt **Bedingungen** definieren Sie, wodurch Ihre Regel ausgelöst wird. Verwenden Sie die folgenden Informationen, um eine Bedingung basierend auf Temperaturtelemetrie zu definieren:

    | Feld                                        | Wert                             |
    | -------------------------------------------- | ------------------------------    |
    | Messung                                  | Temperatur                       |
    | Operator                                     | größer als                   |
    | Wert                                        | 90                                |

    Wählen Sie **+ Bedingung** aus, um weitere Bedingungen hinzuzufügen.

    ![Erstellen einer Regelbedingung](./media/quick-configure-rules/condition.png)

1. Wählen Sie **+ E-Mail** aus, um eine E-Mail-Aktion hinzuzufügen, die bei Auslösung der Regel ausgeführt werden soll.

1. Nutzen Sie beim Festlegen der Aktion die Informationen in der folgenden Tabelle:

    | Einstellung   | Wert                                             |
    | --------- | ------------------------------------------------- |
    | `Display name` | Operator-E-Mail-Aktion                          |
    | To        | Ihre E-Mail-Adresse                                |
    | Notizen     | Die Umgebungstemperatur hat den Schwellenwert überschritten. |

    > [!NOTE]
    > Um eine E-Mail-Benachrichtigung zu erhalten, muss es sich bei der E-Mail-Adresse um eine [Benutzer-ID in der Anwendung](howto-administer.md) handeln, und dieser Benutzer muss sich mindestens einmal bei der Anwendung angemeldet haben.

    ![Erstellen einer Regelaktion](./media/quick-configure-rules/action.png)

1. Wählen Sie **Speichern** aus. Ihre Regel ist auf der Seite **Regeln** aufgeführt.

## <a name="test-the-rule"></a>Testen der Regel

Die Regel wird kurz nach dem Speichern aktiv. Wenn die in der Regel festgelegten Bedingungen erfüllt werden, sendet Ihre Anwendung eine Nachricht an die in der Aktion angegebene E-Mail-Adresse.

> [!NOTE]
> Deaktivieren Sie die Regel nach Abschluss der Tests, um keine weiteren Warnungen in Ihrem Posteingang zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

* Erstellen einer telemetriebasierten Regel
* Hinzufügen einer Aktion

Weitere Informationen zur Überwachung von Geräten, die mit Ihrer Anwendung verbunden sind, finden Sie in der folgenden Schnellstartanleitung:

> [!div class="nextstepaction"]
> [Überwachen Ihrer Geräte mithilfe von Azure IoT Central](quick-monitor-devices.md)
