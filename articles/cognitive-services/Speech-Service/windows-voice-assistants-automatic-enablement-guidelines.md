---
title: Datenschutzrichtlinien für Sprachassistenten unter Windows
titleSuffix: Azure Cognitive Services
description: Die Anweisungen für die standardmäßige Sprachaktivierung eines Sprachassistenten.
services: cognitive-services
author: cfogg6
manager: trrwilson
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: travisw
ms.openlocfilehash: 436367ede4f4be323b5334a201b1c9fb8f7f28e8
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2020
ms.locfileid: "82997325"
---
# <a name="privacy-guidelines-for-voice-assistants-on-windows"></a>Datenschutzrichtlinien für Sprachassistenten unter Windows

Es ist wichtig, dass Benutzern klare Informationen darüber gegeben werden, wie ihre Sprachdaten erfasst und verwendet werden, und es wichtig, den Benutzern die Kontrolle darüber zu geben, ob und wie diese Erfassung erfolgt. Diese Kernfacetten des Datenschutzes – *Offenlegung* und *Zustimmung* – sind insbesondere für in Windows integrierte Sprachassistenten wichtig, angesichts des bei ihrer Nutzung impliziten dauernden Zuhörens.

Entwickler, die Sprachassistenten unter Windows erstellen, müssen ihre Anwendungen mit klar verständlichen Benutzeroberflächenelementen ausstatten, die die Lauschfunktionen des Assistenten widerspiegeln.

> [!NOTE]
> Wird das Bereitstellen angemessener Offenlegungs- und Zustimmungsmöglichkeiten für eine Assistentenanwendung versäumt, auch nach Updates der Anwendung, steht der Assistent möglicherweise bis zur Behebung der Datenschutzprobleme nicht mehr zur Verfügung. 

## <a name="minimum-requirements-for-feature-inclusion"></a>Mindestanforderungen für die Featureintegration

Windows-Benutzer können die Verfügbarkeit ihrer Assistentenanwendungen in **`Settings > Privacy > Voice activation`** anzeigen und steuern.

 > [!div class="mx-imgBorder"]
 > [![privacy-app-listing](media/voice-assistants/windows_voice_assistant/privacy-app-listing.png "Eintrag in den Datenschutzeinstellungen der Windows-Sprachaktivierung für eine Assistentenanwendung")](media/voice-assistants/windows_voice_assistant/privacy-app-listing.png#lightbox)

Um sich für die Aufnahme in diese Liste zu qualifizieren, muss eine Anwendung diese Voraussetzung erfüllen:

1. Die Benutzer werden an hervorgehobener Stelle darüber informiert, dass die Anwendung nach einem Schlüsselwort lauscht, selbst wenn die Anwendung nicht ausgeführt wird, und wie dieses Schlüsselwort lautet
1. Es ist eine Beschreibung enthalten, wie die Sprachdaten von Benutzern verwendet werden, einschließlich eines Links oder Verweises auf relevante Datenschutzrichtlinien
1. Benutzer werden darüber informiert, dass sie ihre Datenschutzeinstellungen über die In-App-Einstellungen hinaus in **`Settings > Privacy > Voice activation`** anzeigen und ändern können; optional kann ein Protokolllink zu `ms-settings:privacy-voiceactivation` für den direkten Zugriff eingeschlossen werden

Nachdem diese Anforderungen erfüllt wurden und die Genehmigung von Microsoft erteilt wurde, wird eine Assistentenanwendung im der Liste der sprachaktivierten Anwendungen angezeigt, sobald sie durch die `Windows.ApplicationModel.ConversationalAgent`-APIs registriert wurde. Dann haben Benutzer die Möglichkeit, der Anwendung ihre Zustimmung für die Schlüsselwortaktivierung zu erteilen. Standardmäßig sind beide Einstellungen `Off` und erfordern es, dass der Benutzer die Seite „Einstellungen“ aufsucht, um sie manuell zu aktivieren.

> [!NOTE]
> Unter allen Umständen erfordern Berechtigungen zur Sprachaktivierung die Berechtigung zur Mikrofonverwendung. Wenn eine Assistentenanwendung nicht über Mikrofonzugriff verfügt, ist sie nicht für Sprachaktivierung qualifiziert und wird in den Datenschutzeinstellungen der Sprachaktivierung in deaktiviertem Zustand angezeigt.

## <a name="additional-requirements-for-inclusion-in-microphone-consent"></a>Zusätzliche Anforderungen für den Einschluss in die Einwilligung zum Mikrofongebrauch

Autoren von Assistenten können die Zustimmung zur Sprachaktivierung für ihre Benutzer einfacher und flüssiger gestalten, indem sie über die oben genannten hinaus weiteren Anforderungen genügen. Nachdem diese erfüllt wurden, ist der Standardwert für die Sprachaktivierungseinstellung einer Assistentenanwendung bei entsperrtem Gerät `On`, nachdem (und erst nachdem) der Anwendung Zugriff auf das Mikrofon erteilt wurde. Dadurch entfällt die Extrarunde über „Einstellungen“ für die Sprachaktivierung eines Assistenten.

Die zusätzlichen Anforderungen besagen, dass eine Assistentenanwendung diese Punkte erfüllen muss:

1. **Vor** der Aufforderung zum Erteilen der Zustimmung zur Mikrofonnutzung (beispielsweise mithilfe der `AppCapability.RequestAccessAsync`-API) muss dem Benutzer an hervorgehobener Stelle angezeigt werden, dass die Assistentenanwendung in der Sprache eines Benutzers nach einem Schlüsselwort lauschen möchte, auch bei nicht ausgeführter Anwendung, und um Zustimmung des Benutzers bittet
2. Es werden alle relevanten Informationen zur Datennutzung und Datenschutzrichtlinien bereitgestellt, **bevor** der Mikrofonzugriff angefordert wird oder die `Windows.ApplicationModel.ConversationalAgent`-APIs verwendet werden
3. Vermeiden Sie im Ablauf der Benutzeroberfläche, der das Verhalten der Audioerfassung offenlegt und die Berechtigung anfordert, anweisende oder anleitende Formulierungen in jeder Form (beispielsweise „Klicken Sie bei der folgenden Anforderung auf ‚Ja‘“)

Sobald diesen Anforderungen Genüge getan wird, wird eine qualifizierte Assistentenanwendung in der Liste der Anwendungen, die für Sprachaktivierung qualifiziert sind, im Zustand `enabled` angezeigt, sobald der Mikrofonzugriff erteilt wird.

> [!NOTE]
> Die Sprachaktivierung bei gesperrtem Gerät ist nicht für die automatische Aktivierung bei erteilten Mikrofonzugriff qualifiziert. Der Benutzer muss die Datenschutzseite der Sprachaktivierung aufsuchen, um den Zugang für einen Assistenten auch bei gesperrtem Gerät zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zu bewährten Methoden für Sprachassistenten unter Windows](windows-voice-assistants-best-practices.md)