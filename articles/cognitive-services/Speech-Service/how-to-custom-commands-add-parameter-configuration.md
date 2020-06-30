---
title: 'Anleitung: Konfigurieren von Parametern als Entität für externe Entitäten'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird erläutert, wie ein Zeichenfolgenparameter so konfiguriert wird, dass er sich auf einen Katalog bezieht, der über einen Webendpunkt verfügbar gemacht wird.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: sausin
ms.openlocfilehash: 5417ff3b53526adbaeba2b9df58694ad495475d3
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307238"
---
# <a name="add-configurations-to-commands-parameters"></a>Hinzufügen von Konfigurationen zu Befehlsparametern

In diesem Artikel erfahren Sie mehr über die erweiterte Parameterkonfiguration, einschließlich:

 - Wie Parameterwerte zu einem Satz gehören können, der außerhalb einer Anwendung für benutzerdefinierte Befehle definiert wurde.
 - Wie Validierungsklauseln über den Wert der Parameter hinzugefügt werden können.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:

> [!div class="checklist"]
> * [Vorgehensweise: Erstellen einer Anwendung mit einfachen Befehlen](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Vorgehensweise: Hinzufügen von Parametern zu Befehlen](./how-to-custom-commands-add-parameters-to-commands.md)


## <a name="configure-parameter-as-external-catalog-entity"></a>Konfigurieren von Parametern als externe Katalogentität

In diesem Abschnitt konfigurieren Sie Zeichenfolgenparameter, um auf externe Kataloge zu verweisen, die über einen Webendpunkt gehostet werden. Auf diese Weise können Sie den externen Katalog unabhängig aktualisieren, ohne Änderungen an der Anwendung für benutzerdefinierte Befehle vornehmen zu müssen. Dieser Ansatz ist in Situationen hilfreich, in denen die Anzahl der Katalogeinträge groß sein kann.

Verwenden Sie den Parameter **SubjectDevice** aus dem Befehl **TurnOnOff** wieder. Die aktuelle Konfiguration für diesen Parameter lautet **Vordefinierte Eingaben aus dem internen Katalog akzeptieren**. Dies bezieht sich auf die statische Liste der Geräte, wie sie in der Parameterkonfiguration definiert ist. Wir möchten diesen Inhalt in eine externe Datenquelle auslagern, die unabhängig aktualisiert werden kann.

Beginnen Sie dazu mit dem Hinzufügen eines neuen Webendpunkts. Wechseln Sie zum Abschnitt **Webendpunkte** im linken Bereich, und fügen Sie einen neuen Webendpunkt mit der folgenden Konfiguration hinzu.

| Einstellung | Vorgeschlagener Wert |
|----|----|
| Name | `getDevices` |
| URL | `https://aka.ms/speech/cc-sampledevices` |
| Methode | GET |


Wenn der vorgeschlagene Wert für die URL bei Ihnen nicht funktioniert, müssen Sie einen einfachen Webendpunkt konfigurieren und hosten, der einen JSCON-Code zurückgibt, der aus einer Liste der Geräte besteht, die gesteuert werden können. Der Webendpunkt sollte einen wie folgt formatierten JSON-Code zurückgeben:
    
```json
{
    "fan" : [],
    "refrigerator" : [
        "fridge"
    ],
    "lights" : [
        "bulb",
        "bulbs",
        "light"
        "light bulb"
    ],
    "tv" : [
        "telly",
        "television"
        ]
}

```


Als nächstes wechseln Sie zur Seite der **SubjectDevice**-Parametereinstellungen und ändern die Eigenschaften wie folgt.

| Einstellung | Vorgeschlagener Wert |
| ----| ---- |
| Konfiguration | Akzeptieren vordefinierter Eingaben aus einem externen Katalog |                               
| Katalogendpunkt | getDevices |
| Methode | GET |

Wählen Sie anschließend **Speichern** aus.

> [!IMPORTANT]
> Es wird keine Option zum Konfigurieren eines Parameters für die Annahme von Eingaben aus einem externen Katalog angezeigt, es sei denn, Sie haben den Webendpunkt im Abschnitt **Webendpunkt** im linken Bereich festgelegt.

### <a name="try-it-out"></a>Ausprobieren

Wählen Sie **Trainieren** aus, und warten Sie auf den Abschluss des Trainings. Sobald das Training abgeschlossen ist, wählen Sie **Testen** aus, um ein paar Interaktionen auszuprobieren.

* Eingabe: Einschalten
* Ausgabe: Welches Gerät soll gesteuert werden?
* Eingabe: Beleuchtung
* Ausgabe: OK, Beleuchtung wird eingeschaltet

> [!NOTE]
> Beachten Sie, wie Sie jetzt alle Geräte steuern können, die auf dem Webendpunkt gehostet werden. Sie müssen die Anwendung noch trainieren, um die neuen Änderungen zu testen und die Anwendung erneut zu veröffentlichen.

## <a name="add-validation-to-parameters"></a>Hinzufügen von Validierung zu Parametern

**Validierungen** sind auf bestimmte Parametertypen anwendbare Konstrukte, die es Ihnen ermöglichen, Einschränkungen für den Wert des Parameters zu konfigurieren und zur Korrektur aufzufordern, wenn Werte nicht den Einschränkungen unterliegen. Eine vollständige Liste der Parametertypen, die das Validierungskonstrukt erweitern, finden Sie unter [Referenzen](./custom-commands-references.md).

Testen Sie Validierungen mit dem Befehl **SetTemperature**. Fügen Sie mit den folgenden Schritten eine Validierung für den Parameter **Temperature** hinzu.

1. Wählen Sie den Befehl **SetTemperature** im linken Bereich aus.
1. Wählen Sie im mittleren Bereich **Temperature** aus.
1. Wählen Sie **Validierung hinzufügen** im rechten Bereich aus.
1. Konfigurieren Sie im Fenster **Neue Validierung** die Validierung wie folgt, und wählen Sie **Erstellen** aus.


    | Parameterkonfiguration | Vorgeschlagener Wert | BESCHREIBUNG |
    | ---- | ---- | ---- |
    | Mindestwert | `60` | Bei Zahlenparameter der Minimalwert, den der Parameter annehmen kann |
    | Höchstwert | `80` | Bei Zahlenparameter der Maximalwert, den der Parameter annehmen kann |
    | Fehlerantwort |  Einfacher Editor > Erste Variation > `Sorry, I can only set temperature between 60 and 80 degrees` | Aufforderung zur Eingabe eines neuen Werts, wenn die Validierung fehlschlägt |

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen einer Bereichsvalidierung](media/custom-commands/add-validations-temperature.png)

### <a name="try-it-out"></a>Ausprobieren

1. Wählen Sie das **Trainieren**-Symbol aus, das oben im rechten Bereich angezeigt wird.

1. Sobald das Training abgeschlossen ist, wählen Sie **Testen** aus, um ein paar Interaktionen auszuprobieren:

    - Eingabe: Stelle die Temperatur auf 22 Grad ein
    - Ausgabe: OK, stelle die Temperatur auf 22 Grad ein
    - Eingabe: Stelle die Temperatur auf 7 Grad ein
    - Ausgabe: Es kann nur eine Temperatur zwischen 15 und 27 Grad eingestellt werden.
    - Eingabe: Stelle stattdessen 22 Grad ein
    - Ausgabe: OK, stelle die Temperatur auf 22 Grad ein

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Hinzufügen von Interaktionsregeln](./how-to-custom-commands-add-interaction-rules.md)
