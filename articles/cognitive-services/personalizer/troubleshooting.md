---
title: 'Problembehandlung: Personalisierung'
description: Dieser Artikel enthält Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: diberry
ms.openlocfilehash: 432b33243bdb38cf359d4fea1a336500eb244464
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650519"
---
# <a name="personalizer-troubleshooting"></a>Troubleshooting für Personalizer

Dieser Artikel enthält Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.

## <a name="transaction-errors"></a>Transaktionsfehler

<details>
<summary><b>Ich erhalte eine HTTP 429-Antwort (Zu viele Anforderungen) vom Dienst. Was kann ich tun?</b></summary>

**Antwort:** Wenn Sie beim Erstellen der Personalisierungsinstanz einen kostenlosen Tarif ausgewählt haben, besteht ein Kontingentlimit für die Anzahl von zulässigen Ranganforderungen. Überprüfen Sie die API-Aufrufquote für die Rang-API (im Azure-Portal für Ihre Personalisierungsressource im Bereich „Metriken“), und passen Sie den Tarif an (im Bereich „Tarif“), wenn Sie damit rechnen, dass das Aufrufvolumen über den Schwellenwert für den ausgewählten Tarif hinaus anwächst.

</details>

<details>
<summary><b>Ich erhalte einen 5xx-Fehler bei Rang- oder Belohnungs-APIs. Wie soll ich vorgehen?</b></summary>

**Antwort:** Diese Probleme sollten vorübergehend sein. Falls sie weiterhin auftreten, wenden Sie sich an den Support, indem Sie im Azure-Portal für Ihre Personalisierungsressource im Abschnitt **Support und Problembehandlung** die Option **Neue Supportanfrage** auswählen.

</details>

## <a name="learning-loop"></a>Lernschleife

<details>
<summary>
<b>Die Lernschleife erreicht ohne Personalisierung keine 100 %ige Übereinstimmung mit dem System. Wie kann ich dies korrigieren?</b></summary>

**Antwort:** Die Gründe, warum Sie Ihr Ziel mit der Lernschleife nicht erreichen:
* Nicht ausreichend Features mit dem Rang-API-Aufruf gesendet
* Fehler in den gesendeten Features – z. B. das Senden von nicht aggregierten Featuredaten wie Zeitstempel an die Rang-API
* Fehler bei der Schleifenverarbeitung – z. B. kein Senden von Relevanzdaten an die Relevanz-API für Ereignisse

Um das Problem zu beheben, müssen Sie die Verarbeitung ändern, indem Sie entweder die an die Schleife gesendeten Features ändern oder sicherstellen, dass die Relevanz eine korrekte Bewertung der Qualität der Antwort des Rangs ist.

</details>

<details>
<summary>
<b>Die Lernschleife funktioniert anscheinend nicht. Wie kann ich dies korrigieren?</b></summary>

**Antwort:** Die Lernschleife benötigt einige Tausend Belohnungsaufrufe, bevor Priorisierungsaufrufe effektiv priorisiert werden.

Wenn Sie sich nicht sicher sind, wie Ihre Lernschleife sich derzeit verhält, führen Sie eine [Offlineauswertung](concepts-offline-evaluation.md) aus, und wenden Sie die korrigierte Lernrichtlinie an.

</details>

<details>
<summary><b>Ich erhalte immer wieder Rangergebnisse mit der gleichen Wahrscheinlichkeit für alle Elemente. Woher weiß ich, ob die Personalisierung lernt?</b></summary>

**Antwort:** Die Personalisierung gibt die gleichen Wahrscheinlichkeiten in einem Rang-API-Ergebnis zurück, wenn die API gerade gestartet wurde und mit einem _leeren_ Modell arbeitet. Dies ist auch der Fall, wenn Sie die Personalisierungsschleife zurücksetzen und das Modell sich noch im Zeitraum **Aktualisierungshäufigkeit des Modells** befindet.

Wenn ein neuer Aktualisierungszeitraum beginnt, wird das aktualisierte Modell verwenden, und Sie werden feststellen, dass sich die Wahrscheinlichkeiten ändern.

</details>

<details>
<summary><b>In der Lernschleife fand ein Lernprozess statt, dies scheint aber nicht mehr der Fall zu sein, und die Qualität der Rangergebnisse ist nicht besonders gut. Wie soll ich vorgehen?</b></summary>

**Antwort:**
* Stellen Sie sicher, dass Sie im Azure-Portal eine vollständige Auswertung für diese Personalisierungsressource abgeschlossen und angewendet haben (Lernschleife).
* Stellen Sie sicher, dass alle Belohnungen über die Belohnungs-API gesendet und verarbeitet wurden.

</details>


<details>
<summary><b>Woher weiß ich, ob die Lernschleife regelmäßig aktualisiert und zum Bewerten meiner Daten verwendet wird?</b></summary>

**Antwort:** Sie finden den Zeitpunkt der letzten Aktualisierung des Modells auf der Seite **Modell- und Lerneinstellungen** im Azure-Portal. Wenn dort ein älterer Zeitstempel angezeigt wird, ist es wahrscheinlich, dass die Rang- und Belohnungsaufrufe nicht gesendet werden. Wenn der Dienst keine eingehenden Daten feststellt, aktualisiert er den Lernvorgang nicht. Wenn Sie der Ansicht sind, dass die Lernschleife nicht häufig genug aktualisiert wird, können Sie die **Aktualisierungshäufigkeit des Modells** bearbeiten.

</details>

## <a name="offline-evaluations"></a>Offlineauswertungen

<details>
<summary><b>Die Featurerelevanz einer Offlineauswertung gibt eine lange Liste mit Hunderttausenden von Elementen zurück. Was ist passiert?</b></summary>

**Antwort:** Dies liegt in der Regel an Zeitstempeln, Benutzer-IDs oder anderen differenzierten Merkmalen, die gesendet wurden.

</details>

<details>
<summary><b>Ich habe eine Offlineauswertung erstellt, und sie wurde fast sofort erfolgreich ausgeführt. Warum? Es werden keine Ergebnisse angezeigt.</b></summary>

**Antwort:** Die Offlineauswertung verwendet die trainierten Modelldaten aus den Ereignissen in diesem Zeitraum. Wenn Sie zwischen dem Start- und Endzeitpunkt der Auswertung keine Daten gesendet haben, wird die Auswertung ohne Ergebnisse abgeschlossen. Starten Sie eine neue Offlineauswertung, indem Sie einen Zeitbereich mit Ereignissen auswählen, von denen Sie wissen, dass sie an die Personalisierung gesendet wurden.

</details>


## <a name="learning-policy"></a>Lernrichtlinie

<details>
<summary><b>Wie importiere ich eine Lernrichtlinie?</b></summary>

**Antwort:** Informieren Sie sich über [Konzepte von Lernrichtlinien](concept-active-learning.md#understand-learning-policy-settings) und die [Anwendung](how-to-manage-model.md) einer neuen Lernrichtlinie. Wenn Sie keine Lernrichtlinie auswählen möchten, können Sie eine [Offlineauswertung](how-to-offline-evaluation.md) verwenden, um basierend auf Ihren aktuellen Ereignissen eine Lernrichtlinie vorzuschlagen.

</details>

## <a name="security"></a>Sicherheit

<details>
<summary><b>Der API-Schlüssel für meine Schleife wurde kompromittiert. Was kann ich tun?</b></summary>

**Antwort:** Sie können einen Schlüssel erneut generieren, nachdem Sie die Schlüsselverwendung durch die Clients getauscht haben. Wenn Sie über zwei Schlüssel verfügen, können Sie den Schlüssel verzögert weitergeben, ohne dass Ausfallzeiten auftreten. Wir empfehlen einen regelmäßigen Tausch als Sicherheitsmaßnahme.

</details>

## <a name="next-steps"></a>Nächste Schritte

[Configure the model update frequency (Ändern der Häufigkeit der Modellaktualisierung)](how-to-settings.md#model-update-frequency)