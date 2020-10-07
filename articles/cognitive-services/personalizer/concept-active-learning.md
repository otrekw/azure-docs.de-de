---
title: Lernrichtlinie – Personalisierung
description: Die Lerneinstellungen legen die *Hyperparameter* des Modelltrainings fest. Zwei Modelle, die anhand der gleichen Daten mit unterschiedlichen Einstellungen trainiert werden, sind im Ergebnis verschieden.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 1d2038e3796df843736eb80f7e5645f4141c82f4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91253631"
---
# <a name="learning-policy-and-settings"></a>Lernrichtlinie und -einstellungen

Die Lerneinstellungen legen die *Hyperparameter* des Modelltrainings fest. Zwei Modelle, die anhand der gleichen Daten mit unterschiedlichen Einstellungen trainiert werden, sind im Ergebnis verschieden.

[Lernrichtlinie und -einstellungen](how-to-settings.md#configure-rewards-for-the-feedback-loop) werden für Ihre Personalisierungsressource im Azure-Portal festgelegt.

## <a name="import-and-export-learning-policies"></a>Importieren und Exportieren von Lernrichtlinien

Sie können Lernrichtliniendateien über das Azure-Portal importieren und exportieren. Auf diese Weise können Sie vorhandene Richtlinien speichern, testen, ersetzen und zur späteren Referenz und Überprüfung als Artefakte in Ihrer Quellcodeverwaltung archivieren.

Erfahren Sie, [wie](how-to-manage-model.md#import-a-new-learning-policy) Sie eine Lernrichtlinie im Azure-Portal für Ihre Personalisierungsressource importieren und exportieren können.

## <a name="understand-learning-policy-settings"></a>Grundlegendes zu Lernrichtlinieneinstellungen

Die Einstellungen in der Lernrichtlinie sollten nicht geändert werden. Ändern Sie die Einstellungen nur, wenn Sie genau wissen, wie sich die Änderungen auf die Personalisierung auswirken. Ohne dieses Wissen könnten Sie Probleme verursachen, einschließlich der Invalidierung von Personalisierungsmodellen.

Die Personalisierung verwendet [vowpalwabbit](https://github.com/VowpalWabbit) zum Trainieren und Bewerten der Ereignisse. Informationen zum Bearbeiten der Lerneinstellungen mit vowpalwabbit finden Sie in der [vowpalwabbit-Dokumentation](https://github.com/VowpalWabbit/vowpal_wabbit/wiki/Command-line-arguments). Wenn Sie über die richtigen Befehlszeilenargumente verfügen, speichern Sie den Befehl im folgenden Format in einer Datei (ersetzen Sie die Eigenschaftswerte der Argumente durch den jeweils gewünschten Befehl). Zum Importieren von Lerneinstellungen laden Sie die Datei anschließend im Azure-Portal für Ihre Personalisierungsressource im Bereich **Modell- und Lerneinstellungen** hoch.

Der folgende `.json`-Code ist ein Beispiel für eine Lernrichtlinie.

```json
{
  "name": "new learning settings",
  "arguments": " --cb_explore_adf --epsilon 0.2 --power_t 0 -l 0.001 --cb_type mtr -q ::"
}
```

## <a name="compare-learning-policies"></a>Vergleichen von Lernrichtlinien

Mithilfe von [Offlineauswertungen](concepts-offline-evaluation.md) können Sie das Abschneiden verschiedener Lernrichtlinien anhand von Vergangenheitsdaten in Personalisierungsprotokollen vergleichen.

[Laden Sie Ihre eigenen Lernrichtlinien hoch](how-to-manage-model.md), um sie mit der aktuellen Lernrichtlinie zu vergleichen.

## <a name="optimize-learning-policies"></a>Optimieren von Lernrichtlinien

Mithilfe einer [Offlineauswertung](how-to-offline-evaluation.md) kann die Personalisierung eine optimierte Lernrichtlinie erstellen. Eine optimierte Lernrichtlinie zeichnet sich bei einer Offlineauswertung durch eine höhere Relevanz aus und liefert bessere Ergebnisse, wenn sie online bei der Personalisierung verwendet wird.

Nachdem Sie eine Lernrichtlinie optimiert haben, können Sie sie direkt auf die Personalisierung anwenden und damit die aktuelle Richtlinie sofort ersetzen. Sie können die optimierte Richtlinie aber auch zur weiteren Auswertung speichern und später entscheiden, ob sie verworfen, gespeichert oder angewandt werden soll.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu [aktiven und inaktiven Ereignissen](concept-active-inactive-events.md).
