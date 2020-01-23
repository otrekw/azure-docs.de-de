---
title: Verwalten einer Lernrichtlinie – Personalisierung
description: Dieser Artikel enthält Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: 707ba9fe7b73ef74e21321533ba02b11f2bad850
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75840716"
---
# <a name="manage-learning-policy"></a>Verwalten einer Lernrichtlinie

Die Lernrichtlinieneinstellungen legen die _Hyperparameter_ des Modelltrainings fest. Die Lernrichtlinie wird in einer `.json`-Datei definiert.

## <a name="import-a-new-learning-policy"></a>Importieren einer neuen Lernrichtlinie

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und wählen Sie Ihre Personalisierungsressource aus.
1. Wählen Sie im Abschnitt **Ressourcenmanagement** die Option **Modell- und Lerneinstellungen** aus.
1. Wählen Sie für **Lerneinstellungen importieren**  die Datei aus, die Sie mit dem oben angegebenen JSON-Format erstellt haben, und wählen Sie dann die Schaltfläche **Hochladen** aus.

    Warten Sie auf die Benachrichtigung, dass die Lernrichtlinie erfolgreich hochgeladen wurde.

## <a name="export-a-learning-policy"></a>Exportieren einer Lernrichtlinie

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und wählen Sie Ihre Personalisierungsressource aus.
1. Wählen Sie im Abschnitt **Ressourcenmanagement** die Option **Modell- und Lerneinstellungen** aus.
1. Wählen Sie für **Lerneinstellungen importieren** die Schaltfläche **Lerneinstellungen exportieren** aus. Damit wird die `json`-Datei auf dem lokalen Computer gespeichert.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Konzepte](concept-active-learning.md#learning-settings) der Lernrichtlinien.

[Erfahren Sie mehr zur regionalen Verfügbarkeit.](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)