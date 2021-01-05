---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2020
ms.author: trbye
ms.openlocfilehash: dc569050b78a5797808f2e2e000019ba516ba22e
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97739234"
---
**Interactive**
- Für Befehls- und Steuerungsszenarien vorgesehen.
- Hat den Segmentierungstimeoutwert X.
- Am Ende einer erkannten Äußerung hält der Dienst die Verarbeitung von Audiodaten aus dieser Anforderungs-ID an und beendet den Turn. Die Verbindung wird nicht geschlossen.
- Die maximale Dauer für die Erkennung beträgt 20 s.
- Der typische Aufruf in Carbon lautet `RecognizeOnceAsync`.

**Unterhaltung**
- Für Erkennung mit längerer Dauer vorgesehen.
- Hat den Segmentierungstimeoutwert Y. (Y != X)
- Verarbeitet mehrere komplette Äußerungen, ohne dass der Turn beendet wird.
- Bei zu viel Stille wird der Turn beendet.
- Carbon fährt mit einer neuen Anforderungs-ID fort und wiederholt ggf. die Audiowiedergabe.
- Nach 10 Minuten Spracherkennung wird die Trennung der Dienstverbindung erzwungen.
- Carbon stellt die Verbindung wieder her und wiederholt die Wiedergabe nicht bestätigter Audioinhalte.
- Wird in Carbon mit `StartContinuousRecognition` aufgerufen.

**Diktieren**
- Ermöglicht Benutzern die Angabe von Interpunktionszeichen während des Sprechens.
- Wird in Carbon aufgerufen, indem `EnableDictation` für das `SpeechConfig`-Objekt angegeben wird, unabhängig vom API-Aufruf, der die Erkennung startet.
- Der 1.<sup> </sup>Partycluster gibt `speech.fragment`-Meldungen für Zwischenergebnisse zurück, der 3.<sup> </sup>Partycluster gibt `speech.hypothesis`-Meldungen zurück.