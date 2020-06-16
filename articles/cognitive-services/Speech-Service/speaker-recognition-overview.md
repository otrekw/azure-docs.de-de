---
title: Azure-Sprechererkennungsdienst
titleSuffix: Azure Cognitive Services
description: Die Sprechererkennung von Azure Cognitive Services bietet Algorithmen, mit denen Sprecher anhand ihrer individuellen Stimmmerkmale überprüft und identifiziert werden. Die Sprechererkennung wird verwendet, um herauszufinden, wer spricht.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/27/2020
ms.author: trbye
ms.openlocfilehash: 2d4ce6f274efbd4d8afe2ac48856b0fc312f0a09
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261770"
---
# <a name="what-is-the-azure-speaker-recognition-service"></a>Was ist der Azure-Sprechererkennungsdienst?

Die Sprechererkennungsdienst bietet Algorithmen, mit denen Sprecher anhand ihrer individuellen Stimmmerkmale überprüft und identifiziert werden. Die Sprechererkennung wird verwendet, um herauszufinden, wer spricht. Wenn Sie Audiotrainingsdaten für einen einzelnen Sprechers bereitstellen, wird basierend auf den eindeutigen Stimmmerkmalen des Sprechers ein Registrierungsprofil erstellt. Sie können anschließend Sprachbeispiele mit diesem Profil abgleichen, um zu überprüfen, ob es sich um denselben Sprecher handelt (Sprecherüberprüfung), oder Sprachbeispiele mit einer *Gruppe* von registrierten Sprecherprofilen abgleichen, um herauszufinden, ob es eine Übereinstimmung mit einem Profil gibt (Sprecheridentifikation). Im Gegensatz dazu gruppiert die [Sprecherdiarisierung](batch-transcription.md#speaker-separation-diarization) Audiosegmente nach Sprecher in einen Batchvorgang.

## <a name="speaker-verification"></a>Sprecherüberprüfung

Die Sprecherüberprüfung optimiert den Prozess für die Überprüfung einer registrierten Sprecheridentität mithilfe von Passphrasen oder einer Spracheingabe in Freiform. Sie kann verwendet werden, um Kunden sicher und reibungslos für eine Vielzahl von Lösungen zu verifizieren – angefangen bei der Identitätsüberprüfung von Kunden in Callcentern bis hin zu kontaktlosem Zugang zu Gebäuden.

### <a name="how-does-speaker-verification-work"></a>Wie funktioniert die Sprecherüberprüfung?

![Wie funktioniert die Sprecherüberprüfung?](media/speaker-recognition/speaker-rec.png)

Die Sprecherüberprüfung kann entweder textabhängig oder textunabhängig sein. Bei der **textabhängigen** Überprüfung müssen die Sprecher sowohl bei der Registrierung als auch bei der Überprüfung dieselbe Passphrase verwenden. Bei der **textunabhängigen** Überprüfung können Sprecher sowohl bei der Registrierung als auch bei der Überprüfung etwas beliebiges sagen.

Bei der **textabhängigen** Überprüfung wird die Stimme des Sprechers registriert, wenn er eine Passphrase aus vorgegebenen Phrasen spricht. Einerseits werden Stimmmerkmale aus der Audioaufzeichnung extrahiert, um eine eindeutige Stimmsignatur zu bilden, und andererseits wird die Passphrase erkannt. Die Stimmsignatur und die Passphrase werden verwendet, um den Sprecher zu überprüfen. 

Bei der **textunabhängigen** Überprüfung gibt es keine Vorgaben für den zu sprechenden Text bei der Registrierung oder für das Audiobeispiel, das überprüft werden soll, da nur Stimmmerkmale extrahiert werden, um die Ähnlichkeit zu bewerten. 

Die APIs sind nicht dafür vorgesehen zu bestimmen, ob das Audio von einer echten Person oder einer Imitation oder Aufzeichnung eines registrierten Sprechers stammt. 

## <a name="speaker-identification"></a>Sprecheridentifikation

Die Sprecheridentifikation wird verwendet, um die Identität eines unbekannten Sprechers innerhalb einer Gruppe registrierter Sprecher zu ermitteln. Sie ermöglicht es Ihnen, Spracheingaben einzelnen Sprechern zuzuordnen und einen Mehrwert aus Szenarios mit mehreren Sprechern zu ziehen:

* Supportlösungen für die Produktivität bei Remotemeetings 
* Erstellen von Gerätepersonalisierung mit mehreren Benutzern

### <a name="how-does-speaker-identification-work"></a>Wie funktioniert die Sprecheridentifikation?

Die Registrierung für die Sprecheridentifikation ist **textunabhängig**. Es spielt also keine Rolle, was der Sprecher in der Audioaufnahme sagt. Ähnlich wie bei der Sprecherüberprüfung wird in der Registrierungsphase die Stimme des Sprechers aufgezeichnet. Stimmmerkmale werden extrahiert, um eine eindeutige Stimmsignatur zu bilden. In der Identifikationsphase wird das Spracheingabebeispiel mit einer bestimmten Liste registrierter Stimmen verglichen (bis zu 50 in jeder Anforderung).

## <a name="data-security-and-privacy"></a>Datensicherheit und -schutz

Sprecheranmeldungsdaten werden in einem gesicherten System gespeichert, einschließlich der Sprachaudioinformationen für die Registrierung und der Sprachsignaturmerkmale. Die Sprachaudiodaten für die Registrierung wird nur beim Upgrade des Algorithmus verwendet, und die Merkmale müssen noch einmal extrahiert werden. Der Dienst speichert weder die Sprachaufzeichnung noch die extrahierten Stimmmerkmale, die während der Erkennungsphase an den Dienst gesendet werden. 

Sie steuern, wie lange Daten aufbewahrt werden sollen. Sie können über API-Aufrufe Registrierungsdaten für einzelne Sprecher erstellen, aktualisieren und entfernen. Beim Löschen des Abonnements werden auch alle mit dem Abonnement verbundenen Sprecherregistrierungsdaten gelöscht. 

Wie bei allen Cognitive Services-Ressourcen müssen Entwickler, die den Sprechererkennungsdienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Sie müssen sicherstellen, dass Sie von den Benutzern die entsprechende Erlaubnis für die Sprechererkennung erhalten haben. Weitere Informationen finden Sie im Microsoft Trust Center auf der  [Seite zu Cognitive Services](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/) . 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> * Weitere Informationen finden Sie im [Videotutorial](https://azure.microsoft.com/resources/videos/speaker-recognition-text-independent-verification-developer-tutorial/) zur textunabhängigen Sprecherüberprüfung.
