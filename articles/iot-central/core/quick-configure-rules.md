---
title: 'Schnellstart: Konfigurieren von Regeln und Aktionen in Azure IoT Central'
description: In dieser Schnellstartanleitung wird gezeigt, wie Sie telemetriebasierte Regeln und Aktionen in Ihrer IoT Central-Anwendung konfigurieren.
author: dominicbetts
ms.author: dobett
ms.date: 05/27/2021
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 9357198e140a6ba403fcb74787d31a0940554fa7
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114459135"
---
# <a name="quickstart-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Schnellstart: Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central

In dieser Schnellstartanleitung erstellen Sie eine IoT Central-Regel, die eine E-Mail sendet, wenn eine Person Ihr Telefon umdreht.

## <a name="prerequisites"></a>Voraussetzungen

Arbeiten Sie zunächst die vorherige Schnellstartanleitung ([Schnellstart: Erstellen einer Azure IoT Central-Anwendung und Verwenden Ihres Smartphones zum Senden von Telemetriedaten](./quick-deploy-iot-central.md)) durch, um die Smartphone-App **IoT Plug & Play** mit Ihrer IoT Central-Anwendung zu verbinden.

## <a name="create-a-telemetry-based-rule"></a>Erstellen einer telemetriebasierten Regel

Die Smartphone-App sendet Telemetriedaten, die Werte vom Beschleunigungsmessersensor enthalten. Liegt das Smartphone mit der Vorderseite nach oben, ist der Wert **z** größer als `9`, liegt es mit der Vorderseite nach unten, ist der Wert **z** kleiner als `-9`.

1. Wählen Sie im linken Bereich die Option **Regeln** aus, um Ihrer Anwendung eine neue telemetriebasierte Regel hinzuzufügen.

1. Wählen Sie **Regel erstellen** aus, um eine neue Regel zu erstellen.

1. Geben Sie als Regelname **Phone turned over** (Smartphone umgedreht) ein.

1. Wählen Sie im Abschnitt **Zielgeräte** unter **Gerätevorlage** die Option **IoT Plug and Play mobile** (IoT Plug & Play-Mobiltelefon) aus. Diese Option filtert die Geräte, auf die die Regel angewendet wird, nach Gerätevorlagentyp. Sie können weitere Filterkriterien hinzufügen, indem Sie **+ Filter** auswählen.

1. Im Abschnitt **Bedingungen** definieren Sie, wodurch Ihre Regel ausgelöst wird. Definieren Sie anhand der folgenden Informationen eine einzelne Bedingung, die auf Telemetriedaten der z-Achse des Beschleunigungsmesser basiert. Diese Regel verwendet die Aggregation, sodass Sie alle fünf Minuten maximal eine E-Mail für jedes Gerät erhalten:

    | Feld            | Wert            |
    |------------------|------------------|
    | Zeitaggregation | Ein, 5 Minuten    |
    | Telemetrie        | Beschleunigung/Z |
    | Betreiber         | Ist kleiner als     |
    | Aggregation      | Minimum          |
    | Wert            | –9               |

    :::image type="content" source="media/quick-configure-rules/rule-target-condition.png" alt-text="Screenshot: Regelbedingung":::

1. Wählen Sie im Abschnitt **Aktion** die Option **+ E-Mail** aus, um eine E-Mail-Aktion hinzuzufügen, die bei Auslösung der Regel ausgeführt werden soll.

1. Nutzen Sie beim Festlegen der Aktion die Informationen in der folgenden Tabelle, und wählen Sie anschließend **Fertig** aus:

    | Einstellung      | Wert                    |
    |--------------|--------------------------|
    | `Display name` | Ihr Telefon wurde bewegt.         |
    | To           | Ihre E-Mail-Adresse       |
    | Notizen        | Ihr Telefon liegt mit der Vorderseite nach unten. |

    > [!TIP]
    > Um eine E-Mail-Benachrichtigung zu erhalten, muss es sich bei der E-Mail-Adresse um eine [Benutzer-ID in der Anwendung](howto-manage-users-roles.md) handeln, und der Benutzer muss sich mindestens einmal bei der Anwendung angemeldet haben.

    :::image type="content" source="media/quick-configure-rules/rule-action.png" alt-text="Screenshot: Regel mit hinzugefügter E-Mail-Aktion":::

1. Wählen Sie **Speichern** aus. Ihre Regel ist nun auf der Seite **Regeln** aufgeführt.

## <a name="test-the-rule"></a>Testen der Regel

Die Regel wird kurz nach dem Speichern aktiv. Wenn die in der Regel definierten Bedingungen erfüllt sind, wird von IoT Central eine E-Mail an die in der Aktion angegebene Adresse gesendet.

Stellen Sie zum Auslösen der Regel sicher, dass die Smartphone-App Daten sendet, und legen Sie es dann mit der Vorderseite nach unten auf den Schreibtisch. Die App sendet nun Telemetriedaten der z-Achse des Beschleunigungsmesser, die kleiner sind als `-9`. Nach fünf Minuten sendet IoT Central Ihnen eine E-Mail, um Sie darüber zu informieren, dass Ihr Smartphone mit der Vorderseite nach unten liegt.

Deaktivieren Sie die Regel nach Abschluss der Tests, um keine weiteren Benachrichtigungs-E-Mails in Ihrem Posteingang zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

* Erstellen einer telemetriebasierten Regel
* Hinzufügen einer Aktion

Weitere Informationen zum Integrieren Ihrer IoT Central-Anwendung in andere Dienste finden Sie unter folgendem Link:

> [!div class="nextstepaction"]
> [Schnellstart: Exportieren von Daten aus einer IoT Central-Anwendung](quick-export-data.md)
