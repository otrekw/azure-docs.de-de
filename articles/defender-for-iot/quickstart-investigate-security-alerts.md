---
title: 'Schnellstart: Untersuchen von Sicherheitswarnungen'
description: Enthält eine Beschreibung der Grundlagen und der Drilldown- und Untersuchungsvorgänge bei Sicherheitswarnungen in Defender für IoT auf Ihren IoT-Geräten.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/30/2020
ms.author: mlottner
ms.openlocfilehash: 172ae82288c2cb948839b69955b9491715eb4690
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90944137"
---
# <a name="quickstart-investigate-security-alerts"></a>Schnellstart: Untersuchen von Sicherheitswarnungen

Eine geplante Untersuchung und Behebung der von Defender für IoT ausgegebenen Warnungen ist die beste Möglichkeit, um für Ihre gesamte IoT-Lösung die Konformität und den Schutz sicherzustellen.

In dieser Schnellstartanleitung erkunden wir die Informationen in den einzelnen IoT-Sicherheitswarnungen. Darüber hinaus wird beschrieben, wie Sie einen Drilldown ausführen und die Details der einzelnen Warnungen und der zugehörigen Geräte verwenden, um richtig zu reagieren und die Warnungen zu beheben. 

Los geht’s! 


## <a name="investigate-new-security-alerts"></a>Untersuchen von neuen Sicherheitswarnungen

In der Liste mit den IoT Hub-Sicherheitswarnungen werden alle aggregierten Sicherheitswarnungen für Ihren IoT-Hub angezeigt. 

1. Öffnen Sie im Azure-Portal den **IoT-Hub**, den Sie auf neue Warnungen untersuchen möchten.
1. Wählen Sie im Menü **Sicherheit** die Option **Warnungen** aus. Alle Sicherheitswarnungen für den IoT-Hub werden angezeigt, und die Warnungen mit dem Flag **Neu** sind Ihre Warnungen der letzten 24 Stunden.
:::image type="content" source="media/quickstart/investigate-new-security-alerts.png" alt-text="Untersuchen von neuen IoT-Sicherheitswarnungen anhand des Flags für neue Warnungen":::
1. Wählen Sie in der Liste eine beliebige Warnung aus, und öffnen Sie sie. Die entsprechenden Warnungsdetails werden angezeigt, und Sie können einen Drilldown zu den gewünschten spezifischen Informationen ausführen. 

## <a name="security-alert-details"></a>Details der Sicherheitswarnung

Beim Öffnen der einzelnen aggregierten Warnungen werden die ausführliche Warnungsbeschreibung, die Schritte zur Behebung und die Geräte-ID für die einzelnen Geräte angezeigt, für die eine Warnung ausgelöst wurde. Darüber hinaus werden der Schweregrad der Warnung und die direkte Zugriffsmöglichkeit für die Untersuchung mit Log Analytics angezeigt. 

1. Wählen Sie in der Liste unter **IoT Hub** > **Sicherheit** > **Warnungen** eine beliebige Sicherheitswarnung aus. 
1. Überprüfen Sie für die Warnungen aller Geräte jeweils die **Beschreibung**, den **Schweregrad**, die **Quelle der Erkennung** und die **Gerätedetails** aller Geräte, von denen die Warnung im Aggregationszeitraum ausgegeben wurde.
:::image type="content" source="media/quickstart/drill-down-iot-alert-details.png" alt-text="Untersuchen von neuen IoT-Sicherheitswarnungen anhand des Flags für neue Warnungen"::: 
1. Verwenden Sie nach dem Überprüfen der spezifischen Warnungsinformationen den **Schritt für die manuelle Behebung**, um das Problem, das zur Warnung geführt hat, zu entschärfen bzw. zu beheben. 
:::image type="content" source="media/quickstart/iot-alert-manual-remediation-steps.png" alt-text="Untersuchen von neuen IoT-Sicherheitswarnungen anhand des Flags für neue Warnungen":::

1. Falls eine weitere Untersuchung erforderlich ist, können Sie den Link verwenden, um die **Warnungen in Log Analytics zu untersuchen**. 
:::image type="content" source="media/quickstart/investigate-iot-alert-log-analytics.png" alt-text="Untersuchen von neuen IoT-Sicherheitswarnungen anhand des Flags für neue Warnungen":::

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel fort, um weitere Informationen zu Defender-Warnungstypen und möglichen Anpassungen zu erhalten.

> [!div class="nextstepaction"]
> [Azure Security Center für IoT – Sicherheitswarnungen](concept-security-alerts.md)
