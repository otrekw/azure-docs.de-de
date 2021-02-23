---
title: 'Übersicht über Custom Speech: Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Bei Custom Speech handelt es sich um eine Reihe von Onlinetools, mit denen Sie die Genauigkeit der Spracherkennung von Microsoft für Ihre Anwendungen, Tools und Produkte bewerten und verbessern können.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: trbye
ms.custom: contperf-fy21q2; references_regions
ms.openlocfilehash: f2dca513b4413f1761ea1b21bf71f90bb27d3053
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100388650"
---
# <a name="what-is-custom-speech"></a>Was ist Custom Speech?

Bei [Custom Speech](https://aka.ms/customspeech) handelt es sich um eine Reihe von UI-basierten Tools, mit denen Sie die Genauigkeit der Spracherkennung von Microsoft für Ihre Anwendungen und Produkte bewerten und verbessern können. Für den Einstieg benötigen Sie lediglich eine Handvoll Audiotestdateien. Nutzen Sie die in diesem Artikel angegebenen Links, um mit der Erstellung einer benutzerdefinierten Spracherkennung zu beginnen.

## <a name="whats-in-custom-speech"></a>Was ist in Custom Speech enthalten?

Bevor Sie Custom Speech verwenden können, benötigen Sie ein Azure-Konto und ein Abonnement für Speech-Dienste. Nachdem Sie ein Konto eingerichtet haben, können Sie Daten vorbereiten, Modelle trainieren und testen, die Erkennungsqualität überprüfen, die Genauigkeit bewerten und das benutzerdefinierte Spracherkennungsmodell schließlich bereitstellen und verwenden.

Die folgende Abbildung zeigt die Teile, aus denen sich der [Custom Speech-Bereich in Speech Studio](https://aka.ms/customspeech) zusammensetzt. Verwenden Sie die unten angegebenen Links, um mehr über die einzelnen Schritte zu erfahren.

![Abbildung der Komponenten, aus denen sich der Custom Speech-Bereich in Speech Studio zusammensetzt](./media/custom-speech/custom-speech-overview.png)

1. [Abonnieren und Erstellen eines Projekts:](#set-up-your-azure-account) Erstellen Sie ein Azure-Konto, und abonnieren Sie die Speech-Dienste. Durch dieses einheitliche Abonnement erhalten Sie Zugriff auf die Spracherkennung, Sprachsynthese, Sprachübersetzung und auf [Speech Studio](https://speech.microsoft.com/customspeech). Erstellen Sie dann mit Ihrem Abonnement für Speech-Dienste Ihr erstes Custom Speech-Projekt.

1. [Hochladen von Testdaten:](./how-to-custom-speech-test-and-train.md) Laden Sie Testdaten (Audiodateien) hoch, um das Spracherkennungsangebot von Microsoft für Ihre Anwendungen, Tools und Produkte zu bewerten.

1. [Überprüfen der Erkennungsqualität:](how-to-custom-speech-inspect-data.md) Verwenden Sie [Speech Studio](https://speech.microsoft.com/customspeech) zur Wiedergabe hochgeladener Audiodateien, und überprüfen Sie die Qualität der Spracherkennung Ihrer Testdaten. Informationen zu quantitativen Messungen finden Sie unter [Überprüfen von Daten](how-to-custom-speech-inspect-data.md).

1. [Bewerten und Verbessern der Genauigkeit:](how-to-custom-speech-evaluate-data.md) Bewerten und verbessern Sie die Genauigkeit des Spracherkennungsmodells. In [Speech Studio](https://speech.microsoft.com/customspeech) finden Sie eine *Wort-Fehler-Rate*, mit der Sie bestimmen können, ob weiteres Training erforderlich ist. Wenn Sie mit der Genauigkeit zufrieden sind, können Sie die Speech-Dienst-APIs direkt verwenden. Wenn Sie die Genauigkeit um durchschnittlich 5 – 20 % verbessern möchten, verwenden Sie die Registerkarte **Training** im Portal, um zusätzliche Trainingsdaten hochzuladen, z. B. Humantranskripte und zugehörigen Text.

1. [Trainieren und Bereitstellen eines Modells:](how-to-custom-speech-train-model.md) Verbessern Sie die Genauigkeit Ihres Spracherkennungsmodells, indem Sie schriftliche Transkripte (10 – 1.000 Stunden) und zugehörigen Text (< 200 MB) zusammen mit Ihren Audiotestdaten bereitstellen. Mithilfe dieser Daten kann das Spracherkennungsmodell trainiert werden. Testen Sie das Modell nach dem Training erneut. Wenn Sie mit dem Ergebnis zufrieden sind, können Sie das Modell an einem benutzerdefinierten Endpunkt bereitstellen.

## <a name="set-up-your-azure-account"></a>Einrichten Ihres Azure-Kontos

Sie benötigen ein Azure-Konto und ein Abonnement für die Speech-Dienste, um [Speech Studio](https://speech.microsoft.com/customspeech) zum Erstellen eines benutzerdefinierten Modells verwenden zu können. Falls nicht, können Sie [den Speech-Dienst kostenlos testen](overview.md#try-the-speech-service-for-free).

> [!NOTE]
> Wenn Sie ein benutzerdefiniertes Modell mit **Audiodaten** trainieren möchten, wählen Sie eine der folgenden Regionen aus, in der dedizierte Hardware für das Training verfügbar ist. Dadurch wird die zum Trainieren benötigte Zeit reduziert.

* Australien (Osten)
* Kanada, Mitte
* Indien, Mitte
* East US
* USA (Ost) 2
* USA Nord Mitte
* Nordeuropa
* USA Süd Mitte
* Asien, Südosten
* UK, Süden
* US Gov Arizona
* US Government, Virginia
* Europa, Westen
* USA, Westen 2

> [!NOTE]
> Stellen Sie sicher, dass Sie ein Standard-Abonnement (S0) erstellen. Free-Abonnements (F0) werden nicht unterstützt.

Nachdem Sie ein Azure-Konto und ein Abonnement für Speech-Dienste erstellt haben, müssen Sie sich bei [Speech Studio](https://speech.microsoft.com/customspeech) anmelden und Ihr Abonnement verknüpfen.

1. Melden Sie sich in [Speech Studio](https://aka.ms/custom-speech) an.
1. Wählen Sie das Abonnement aus, das Sie bearbeiten möchten, und erstellen Sie ein Speech-Projekt.
1. Wenn Sie Ihr Abonnement ändern möchten, klicken Sie im oberen Menü auf die Zahnradschaltfläche.

## <a name="how-to-create-a-project"></a>So erstellen Sie ein Projekt

Inhalte wie Daten, Modelle, Tests und Endpunkte sind in [Speech Studio](https://speech.microsoft.com/customspeech) in *Projekten* organisiert. Jedes Projekt ist für eine Domäne und das Land/die Sprache spezifisch. Sie können beispielsweise ein Projekt für Callcenter erstellen, die Englisch in den USA verwenden.

Wählen Sie zum Erstellen Ihres ersten Projekts die **Spracherkennung/Custom Speech** aus, und klicken Sie dann auf **Neues Projekt**. Folgen Sie den Anweisungen des Assistenten, um Ihr Projekt zu erstellen. Nachdem Sie ein Projekt erstellt haben, sollten vier Registerkarten angezeigt werden: **Daten**, **Test**, **Training** und **Bereitstellung**. Verwenden Sie die Links unter [Nächste Schritte](#next-steps), um mehr über die Verwendung der einzelnen Registerkarten zu erfahren.

> [!IMPORTANT]
> [Speech Studio](https://aka.ms/custom-speech), ehemals als „Custom Speech-Portal“ bezeichnet, wurde vor Kurzem aktualisiert. Wenn Sie vorherige Daten, Modelle, Tests und veröffentlichte Endpunkte im CRIS.ai-Portal oder mit APIs erstellt haben, müssen Sie im neuen Portal ein neues Projekt erstellen, um eine Verbindung mit diesen alten Entitäten herzustellen.

## <a name="model-lifecycle"></a>Lebenszyklus von Modellen

Custom Speech verwendet sowohl *Basismodelle* als auch *benutzerdefinierte Modelle*. Jede Sprache verfügt über ein oder mehrere Basismodelle. Wenn ein neues Sprachmodell für den regulären Speech-Dienst freigegeben wird, wird es im Allgemeinen auch als neues Basismodell in den Custom Speech-Dienst importiert. Sie werden alle 3 bis 6 Monate aktualisiert. Ältere Modelle werden in der Regel mit der Zeit weniger nützlich, da das neueste Modell in der Regel eine höhere Genauigkeit aufweist.

Im Gegensatz dazu werden benutzerdefinierte Modelle erstellt, indem ein gewähltes Basismodell an ein bestimmtes Kundenszenario angepasst wird. Sie können ein bestimmtes benutzerdefiniertes Modell für einen längeren Zeitraum weiterhin verwenden, sobald Sie eines für Ihre Anforderungen gefunden haben. Es wird jedoch empfohlen, regelmäßig ein Update auf das neueste Basismodell durchzuführen und es mit zusätzlichen Daten erneut zu trainieren. 

Weitere Schlüsselbegriffe im Zusammenhang mit dem Modelllebenszyklus sind:

* **Anpassung:** Die Übernahme eines Basismodells und dessen Anpassung mithilfe von Text- und/oder Audiodaten an Ihre Domäne/Ihr Szenario.
* **Decodierung:** Die Verwendung eines Modells und Durchführung der Spracherkennung (Decodierung von Audio- in Textdaten).
* **Endpunkt**: Eine benutzerspezifische Bereitstellung eines Basismodells oder eines benutzerdefinierten Modells, das *nur* für einen bestimmten Benutzer zugänglich ist.

### <a name="expiration-timeline"></a>Zeitskala für den Ablauf

Wenn neue Modelle und neue Funktionalitäten verfügbar werden und ältere, weniger genaue Modelle eingestellt werden, finden Sie weitere Informationen in den folgenden Zeitskalen für den Ablauf von Modell und Endpunkt:

**Basismodelle** 

* Anpassung: Verfügbar für ein Jahr. Nachdem das Modell importiert wurde, steht es für ein Jahr zur Verfügung, um benutzerdefinierte Modelle zu erstellen. Nach einem Jahr müssen neue benutzerdefinierte Modelle aus einer neueren Basismodellversion erstellt werden.  
* Decodierung: Für zwei Jahre nach dem Import verfügbar. Das bedeutet, dass Sie einen Endpunkt erstellen und mithilfe dieses Modells die Batch-Transkription für zwei Jahre verwenden können. 
* Endpunkte: Auf derselben Zeitskala wie die Decodierung verfügbar.

**Benutzerdefinierte Modelle**

* Decodierung: Verfügbar für zwei Jahre, nachdem das Modell erstellt wurde. Dies bedeutet, dass Sie das benutzerdefinierte Modell nach seiner Erstellung zwei Jahre lang (Batch/Echtzeit/Test) verwenden können. Nach zwei Jahren sollten Sie *Ihr Modell erneut trainieren*, da das Basismodell in in der Regel für die Anpassung veraltet sein wird.  
* Endpunkte: Auf derselben Zeitskala wie die Decodierung verfügbar.

Wenn entweder ein Basismodell oder ein benutzerdefiniertes Modell abläuft, wird immer ein Fallback auf die *neueste Basismodellversion* durchgeführt. Auf diese Weise wird Ihre Implementierung niemals unterbrochen, aber sie ist möglicherweise für *Ihre bestimmten Daten* weniger genau, wenn benutzerdefinierte Modelle das Ablaufdatum erreichen. Sie können das Ablaufdatum für ein Modell an den folgenden Stellen im Custom Speech-Bereich von Speech Studio sehen:

* Zusammenfassung zum Modelltraining
* Modelltrainingsdetails
* Zusammenfassung der Bereitstellungen
* Bereitstellungsdetails

Sie können die Ablaufdaten auch über die Custom Speech-APIs [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) und [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) unter der Eigenschaft `deprecationDates` in der JSON-Antwort überprüfen.

Beachten Sie, dass Sie das Modell über einen Custom Speech-Endpunkt ohne Downtime aktualisieren können, indem Sie das vom Endpunkt verwendete Modell im Abschnitt „Bereitstellung“ in Speech Studio oder über die Custom Speech-API ändern.

## <a name="next-steps"></a>Nächste Schritte

* [Vorbereiten und Testen Ihrer Daten](./how-to-custom-speech-test-and-train.md)
* [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
* [Bewerten und Verbessern der Modellgenauigkeit](how-to-custom-speech-evaluate-data.md)
* [Trainieren und Bereitstellen eines Modells](how-to-custom-speech-train-model.md)
