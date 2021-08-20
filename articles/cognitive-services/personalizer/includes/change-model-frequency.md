---
title: Datei einfügen
description: Datei einfügen
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 08/25/2020
ms.openlocfilehash: 6473eeee350299ba98460f3f4f022e0514f90e69
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2021
ms.locfileid: "112254987"
---
### <a name="change-the-model-update-frequency"></a>Ändern der Häufigkeit der Modellaktualisierung

Ändern Sie im Azure-Portal auf der Seite **Konfiguration** die Einstellung **Häufigkeit der Modellaktualisierung** der Ressource für die Personalisierung in „30 Sekunden“. Mit dieser kurzen Dauer wird der Dienst schnell trainiert, und Sie können sehen, wie sich die oberste Aktion für jede Iteration ändert.

![Ändern der Häufigkeit der Modellaktualisierung](../media/settings/configure-model-update-frequency-settings.png)

Bei der Erstinstanziierung einer Personalisierungsschleife ist kein Modell vorhanden, da noch keine Relevanz-API-Aufrufe zum Trainieren ausgeführt wurden. Rangfolgeaufrufe geben gleiche Wahrscheinlichkeiten für jedes Element zurück. Ihre Anwendung sollte dem Inhalt dennoch immer anhand der Ausgabe von RewardActionId einen Rang zuweisen.