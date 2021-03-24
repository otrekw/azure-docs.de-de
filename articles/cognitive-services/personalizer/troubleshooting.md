---
title: 'Problembehandlung: Personalisierung'
description: Dieser Artikel enthält Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: troubleshooting
ms.date: 02/26/2020
ms.openlocfilehash: b5f7ed1f2ded8f6ec0320d417b59bab016d75028
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91777274"
---
# <a name="personalizer-troubleshooting"></a>Troubleshooting für Personalizer

Dieser Artikel enthält Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.

## <a name="configuration-issues"></a>Konfigurationsprobleme

### <a name="i-changed-a-configuration-setting-and-now-my-loop-isnt-performing-at-the-same-learning-level-what-happened"></a>Ich habe eine Konfigurationseinstellung geändert, und jetzt erreicht meine Schleife nicht dieselbe Lernebene. Was ist passiert?

Einige Konfigurationseinstellungen [setzen Ihr Modell zurück](how-to-settings.md#settings-that-include-resetting-the-model). Konfigurationsänderungen sollten sorgfältig geplant werden.

### <a name="when-configuring-personalizer-with-the-api-i-received-an-error-what-happened"></a>Beim Konfigurieren der Personalisierung über die API habe ich einen Fehler erhalten. Was ist passiert?

Wenn Sie eine einzelne API-Anforderung verwenden, um Ihren Dienst zu konfigurieren und Ihr Lernverhalten zu ändern, erhalten Sie eine Fehlermeldung. Sie müssen zwei separate API-Aufrufe durchführen. Mit dem ersten Aufruf konfigurieren Sie Ihren Dienst, und mit dem zweiten Aufruf ändern Sie das Lernverhalten.

## <a name="transaction-errors"></a>Transaktionsfehler

### <a name="i-get-an-http-429-too-many-requests-response-from-the-service-what-can-i-do"></a>Ich erhalte die Antwort „HTTP 429 (Zu viele Anforderungen) vom Dienst. Was kann ich tun?

Wenn Sie beim Erstellen der Personalisierungsinstanz einen kostenlosen Tarif ausgewählt haben, besteht ein Kontingentlimit für die Anzahl von zulässigen Ranganforderungen. Überprüfen Sie die API-Aufrufquote für die Rang-API (im Azure-Portal für Ihre Personalisierungsressource im Bereich „Metriken“), und passen Sie den Tarif an (im Bereich „Tarif“), wenn Sie damit rechnen, dass das Aufrufvolumen über den Schwellenwert für den ausgewählten Tarif hinaus anwächst.

### <a name="im-getting-a-5xx-error-on-rank-or-reward-apis-what-should-i-do"></a>Ich erhalte einen 5xx-Fehler bei Rang- oder Belohnungs-APIs. Wie sollte ich vorgehen?

Diese Probleme sollten vorübergehend sein. Falls sie weiterhin auftreten, wenden Sie sich an den Support, indem Sie im Azure-Portal für Ihre Personalisierungsressource im Abschnitt **Support und Problembehandlung** die Option **Neue Supportanfrage** auswählen.

## <a name="learning-loop"></a>Lernschleife

### <a name="the-learning-loop-doesnt-attain-a-100-match-to-the-system-without-personalizer-how-do-i-fix-this"></a>Die Lernschleife erreicht ohne Personalisierung keine 100 %ige Übereinstimmung mit dem System. Wie kann ich dies korrigieren?

Die Gründe, warum Sie Ihr Ziel mit der Lernschleife nicht erreichen:
* Nicht ausreichend Features mit dem Rang-API-Aufruf gesendet
* Fehler in den gesendeten Features – z. B. das Senden von nicht aggregierten Featuredaten wie Zeitstempel an die Rang-API
* Fehler bei der Schleifenverarbeitung – z. B. kein Senden von Relevanzdaten an die Relevanz-API für Ereignisse

Um das Problem zu beheben, müssen Sie die Verarbeitung ändern, indem Sie entweder die an die Schleife gesendeten Features ändern oder sicherstellen, dass die Relevanz eine korrekte Bewertung der Qualität der Antwort des Rangs ist.

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>Die Lernschleife funktioniert anscheinend nicht. Wie kann ich dies korrigieren?

Die Lernschleife benötigt einige Tausend Belohnungsaufrufe, bevor Priorisierungsaufrufe effektiv priorisiert werden.

Wenn Sie sich nicht sicher sind, wie Ihre Lernschleife sich derzeit verhält, führen Sie eine [Offlineauswertung](concepts-offline-evaluation.md) aus, und wenden Sie die korrigierte Lernrichtlinie an.

### <a name="i-keep-getting-rank-results-with-all-the-same-probabilities-for-all-items-how-do-i-know-personalizer-is-learning"></a>Ich erhalte immer wieder Rangergebnisse mit der gleichen Wahrscheinlichkeit für alle Elemente. Woher weiß ich, ob die Personalisierung lernt?

Die Personalisierung gibt die gleichen Wahrscheinlichkeiten in einem Rang-API-Ergebnis zurück, wenn die API gerade gestartet wurde und mit einem _leeren_ Modell arbeitet. Dies ist auch der Fall, wenn Sie die Personalisierungsschleife zurücksetzen und das Modell sich noch im Zeitraum **Aktualisierungshäufigkeit des Modells** befindet.

Wenn ein neuer Aktualisierungszeitraum beginnt, wird das aktualisierte Modell verwendet, und Sie werden feststellen, dass sich die Wahrscheinlichkeiten ändern.

### <a name="the-learning-loop-was-learning-but-seems-to-not-learn-anymore-and-the-quality-of-the-rank-results-isnt-that-good-what-should-i-do"></a>In der Lernschleife fand ein Lernprozess statt, dies scheint aber nicht mehr der Fall zu sein, und die Qualität der Rangergebnisse ist nicht besonders gut. Wie sollte ich vorgehen?

* Stellen Sie sicher, dass Sie im Azure-Portal eine vollständige Auswertung für diese Personalisierungsressource abgeschlossen und angewendet haben (Lernschleife).
* Stellen Sie sicher, dass alle Belohnungen über die Belohnungs-API gesendet und verarbeitet wurden.

### <a name="how-do-i-know-that-the-learning-loop-is-getting-updated-regularly-and-is-used-to-score-my-data"></a>Woher weiß ich, ob die Lernschleife regelmäßig aktualisiert und zum Bewerten meiner Daten verwendet wird?

Sie finden den Zeitpunkt der letzten Aktualisierung des Modells auf der Seite **Modell- und Lerneinstellungen** im Azure-Portal. Wenn dort ein älterer Zeitstempel angezeigt wird, ist es wahrscheinlich, dass die Rang- und Belohnungsaufrufe nicht gesendet werden. Wenn der Dienst keine eingehenden Daten feststellt, aktualisiert er den Lernvorgang nicht. Wenn Sie der Ansicht sind, dass die Lernschleife nicht häufig genug aktualisiert wird, können Sie die **Aktualisierungshäufigkeit des Modells** bearbeiten.

## <a name="offline-evaluations"></a>Offlineauswertungen

### <a name="an-offline-evaluations-feature-importance-returns-a-long-list-with-hundreds-or-thousands-of-items-what-happened"></a>Die Featurerelevanz einer Offlineauswertung gibt eine lange Liste mit Hunderttausenden von Elementen zurück. Was ist passiert?

Dies liegt in der Regel an Zeitstempeln, Benutzer-IDs oder anderen differenzierten Merkmalen, die gesendet wurden.

### <a name="i-created-an-offline-evaluation-and-it-succeeded-almost-instantly-why-is-that-i-dont-see-any-results"></a>Ich habe eine Offlineauswertung erstellt, und sie wurde fast sofort erfolgreich ausgeführt. Warum? Es werden keine Ergebnisse angezeigt.

Die Offlineauswertung verwendet die trainierten Modelldaten aus den Ereignissen in diesem Zeitraum. Wenn Sie zwischen dem Start- und Endzeitpunkt der Auswertung keine Daten gesendet haben, wird die Auswertung ohne Ergebnisse abgeschlossen. Starten Sie eine neue Offlineauswertung, indem Sie einen Zeitbereich mit Ereignissen auswählen, von denen Sie wissen, dass sie an die Personalisierung gesendet wurden.

## <a name="learning-policy"></a>Lernrichtlinie

### <a name="how-do-i-import-a-learning-policy"></a>Wie importiere ich eine Lernrichtlinie?

Informieren Sie sich über [Konzepte von Lernrichtlinien](concept-active-learning.md#understand-learning-policy-settings) und die [Anwendung](how-to-manage-model.md) einer neuen Lernrichtlinie. Wenn Sie keine Lernrichtlinie auswählen möchten, können Sie eine [Offlineauswertung](how-to-offline-evaluation.md) verwenden, um basierend auf Ihren aktuellen Ereignissen eine Lernrichtlinie vorzuschlagen.


## <a name="security"></a>Sicherheit

### <a name="the-api-key-for-my-loop-has-been-compromised-what-can-i-do"></a>Der API-Schlüssel für meine Schleife wurde kompromittiert. Was kann ich tun?

Sie können einen Schlüssel erneut generieren, nachdem Sie die Schlüsselverwendung durch die Clients getauscht haben. Wenn Sie über zwei Schlüssel verfügen, können Sie den Schlüssel verzögert weitergeben, ohne dass Ausfallzeiten auftreten. Wir empfehlen einen regelmäßigen Tausch als Sicherheitsmaßnahme.


## <a name="next-steps"></a>Nächste Schritte

[Configure the model update frequency (Ändern der Häufigkeit der Modellaktualisierung)](how-to-settings.md#model-update-frequency)