---
title: Konfigurieren von Regeln und Aktionen in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Gewusst-wie-Artikel wird gezeigt, wie Sie als Ersteller telemetriebasierte Regeln und Aktionen in Ihrer Azure IoT Central-Anwendung konfigurieren.
author: vavilla
ms.author: vavilla
ms.date: 11/27/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 509f9557a8128df12353ad02a7c7db02b7b42631
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158455"
---
# <a name="configure-rules"></a>Konfigurieren von Regeln



*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

Regeln in IoT Central dienen als anpassbares Antworttool, das bei aktiv überwachten Ereignisse von verbundenen Geräten ausgelöst wird. In den folgenden Abschnitten wird beschrieben, wie Regeln ausgewertet werden.

## <a name="select-target-devices"></a>Auswählen von Zielgeräten

Wählen Sie über den Abschnitt „Zielgeräte“ aus, auf welche Art von Geräten die jeweilige Regel angewendet werden soll. Mithilfe von Filtern können Sie weiter verfeinern, welche Geräte einbezogen werden sollen. Die Filter verwenden Eigenschaften in der Gerätevorlage zum Filtern der Gruppe von Geräten. Filter selbst lösen keine Aktion aus. Im nachstehenden Screenshot sind die Zielgeräte vom Gerätevorlagentyp **Refrigerator** (Kühlschrank). Der Filter gibt an, dass die Regel nur **Refrigerators** (Kühlschränke) enthalten soll, deren Eigenschaft **Manufactured State** (Staat der Fertigung) gleich **Washington** ist.

![Bedingungen](media/howto-configure-rules/filters.png)

## <a name="use-multiple-conditions"></a>Verwenden von mehreren Bedingungen

Bedingungen sind die Grundlage für das Auslösen von Regeln. Wenn Sie einer Regel mehrere Bedingungen hinzufügen, werden sie derzeit durch AND logisch verbunden. Mit anderen Worten: Alle Bedingungen müssen erfüllt sein, damit die Regel als „true“ (wahr) ausgewertet wird.  

Im nachstehenden Screenshot wird mit den Bedingungen überprüft, wann die Temperatur höher als 70&deg;F und die Luftfeuchtigkeit niedriger als 10 ist. Wenn diese beiden Anweisungen wahr sind, wird die Regel als „true“ ausgewertet und löst eine Aktion aus.

![Bedingungen](media/howto-configure-rules/conditions.png)

## <a name="use-aggregate-windowing"></a>Verwenden von aggregiertem Windowing

Regeln werten aggregierte Zeitfenster als rollierende Fenster aus. Im nachstehenden Screenshot beträgt das Zeitfenster fünf Minuten. Die Regel wertet alle fünf Minuten die Daten der letzten fünf Minuten aus. Die Daten werden nur einmal in dem für sie zutreffenden Fenster ausgewertet.

![Rollierende Fenster](media/howto-configure-rules/tumbling-window.png)

## <a name="use-rules-with-iot-edge-modules"></a>Verwenden von Regeln bei IoT Edge-Modulen

Für Regeln, die auf IoT Edge-Module angewendet werden, gilt eine Einschränkung. Regeln für Telemetriedaten aus verschiedenen Modulen werden nicht als gültige Regeln ausgewertet. Verwenden Sie die nachstehende Regel als Beispiel. Die erste Bedingung der Regel bezieht sich auf Temperaturtelemetriedaten von Modul A. Die zweite Bedingung der Regel bezieht sich auf Luftfeuchtigkeitstelemetriedaten von Modul B. Da die beiden Bedingungen von unterschiedlichen Modulen stammen, handelt es sich hierbei um eine ungültige Gruppe von Bedingungen. Die Regel ist ungültig und wird beim Versuch, sie zu speichern, einen Fehler auslösen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie eine Regel in Ihrer Azure IoT Central-Anwendung konfigurieren, wird als Nächstes Folgendes empfohlen:

> [!div class="nextstepaction"]
> [Analysieren Ihrer Daten während der Ausführung](howto-create-analytics.md)
