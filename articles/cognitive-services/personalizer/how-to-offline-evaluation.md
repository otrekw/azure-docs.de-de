---
title: 'Durchführen einer Offlineauswertung: Personalisierung'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird erläutert, wie Sie mit der Offlineauswertung die Effektivität Ihrer App messen und Ihre Lernschleife analysieren können.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: how-to
ms.date: 02/20/2020
ms.author: diberry
ms.openlocfilehash: 2fa4e47f600737c9bff5dbff4dd743946b4e82aa
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84342152"
---
# <a name="analyze-your-learning-loop-with-an-offline-evaluation"></a>Analysieren Ihrer Lernschleife mit einer Offlineauswertung

Erfahren Sie, wie Sie eine Offlineauswertung ausführen und die Ergebnisse verstehen können.

Mit Offlineauswertungen können Sie messen, wie effektiv die Personalisierung verglichen mit dem Standardverhalten Ihrer Anwendung ist, erfahren, welche Features am meisten zur Personalisierung beitragen, und automatisch neue Werte für das Machine Learning ermitteln.

Weitere Informationen finden Sie unter [Offlineauswertung](concepts-offline-evaluation.md).

## <a name="prerequisites"></a>Voraussetzungen

* Eine konfigurierte Personalisierungsschleife
* Die Personalisierungsschleife muss eine repräsentative Menge an Daten aufweisen. Für sinnvolle Auswertungsergebnisse werden mindestens 50.000 Ereignisse in den Protokollen empfohlen. Optional haben Sie möglicherweise auch zuvor _Lernrichtliniendateien_ exportiert, die Sie in der gleichen Auswertung vergleichen und testen können.

## <a name="run-an-offline-evaluation"></a>Durchführen einer Offlinebewertung

1. Suchen Sie Ihre Personalisierungsressource im [Azure-Portal](https://azure.microsoft.com/free/).
1. Navigieren Sie im Azure-Portal zum Abschnitt **Bewertungen**, und wählen Sie **Bewertung erstellen** aus.
    ![Navigieren Sie im Azure-Portal zum Abschnitt **Bewertungen**, und wählen Sie **Bewertung erstellen** aus.](./media/offline-evaluation/create-new-offline-evaluation.png)
1. Konfigurieren Sie die folgenden Werte:

    * Einen Auswertungsnamen.
    * Start- und Enddatum: Hierbei handelt es sich um Datumsangaben, die den Bereich der zur Auswertung zu verwendenden Daten angeben. Diese Daten müssen gemäß dem Wert [Datenaufbewahrung](how-to-settings.md) in den Protokollen vorliegen.
    * „Ermittlung zur Optimierung“ ist auf **Ja** festgelegt.

    > [!div class="mx-imgBorder"]
    > ![Auswählen von Offlineauswertungseinstellungen](./media/offline-evaluation/create-an-evaluation-form.png)

1. Wählen Sie **OK** aus, um die Auswertung zu starten.

## <a name="review-the-evaluation-results"></a>Überprüfen der Auswertungsergebnisse

Auswertungen können je nach der Menge zu verarbeitender Daten, der Anzahl der zu vergleichenden Lernrichtlinien und abhängig davon, ob eine Optimierung angefordert wurde, sehr lange dauern.

Nach Abschluss können Sie die Auswertung aus der Auswertungsliste auswählen und dann die Option zum **Vergleichen der Bewertung Ihrer Anwendung mit anderen potenziellen Lerneinstellungen** aktivieren. Aktivieren Sie dieses Feature, wenn Sie sehen möchten, wie Ihre aktuelle Lernrichtlinie im Vergleich zu einer neuen Richtlinie abschneidet.

1. Überprüfen Sie die Leistung der [Lernrichtlinien](concepts-offline-evaluation.md#discovering-the-optimized-learning-policy).

    > [!div class="mx-imgBorder"]
    > [![Überprüfen der Auswertungsergebnisse](./media/offline-evaluation/evaluation-results.png)](./media/offline-evaluation/evaluation-results.png#lightbox)

1. Wählen Sie **Übernehmen** aus, um die Richtlinie anzuwenden, die das Modell am besten für Ihre Daten optimiert.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Funktionsweise von Offlineauswertungen](concepts-offline-evaluation.md).
