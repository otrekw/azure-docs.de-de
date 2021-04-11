---
title: Barrierefreiheit für Azure Media Player
description: Hier erfahren Sie mehr über die Barrierefreiheitseinstellungen von Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/05/2021
ms.openlocfilehash: 26e25f5391524020ef7968ba627997ea16a80921
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106449901"
---
# <a name="accessibility"></a>Barrierefreiheit #

Azure Media Player kann mit Sprachausgabefunktionen wie der Windows-Sprachausgabe oder Apple OSX/iOS VoiceOver verwendet werden. Für die Schaltflächen der Benutzeroberfläche sind alternative Tags verfügbar, und die Sprachausgabe kann diese alternativen Tags lesen, wenn der Benutzer zu ihnen navigiert. Weitere Konfigurationen können auf der Betriebssystemebene festgelegt werden.

## <a name="captions-and-subtitles"></a>Untertitel ##

Azure Media Player unterstützt Untertitel derzeit nur für On-Demand-Ereignisse im WebVTT-Format und für Liveereignisse mit CEA 708. Das TTML-Format wird aktuell nicht unterstützt. Mit Untertiteln kann die Zielgruppe für den Player erweitert werden, z. B. auf Personen mit Hörschädigung oder Benutzer, die in einer anderen Sprache mitlesen möchten. Darüber hinaus wird mit Untertiteln die Bindung an das Video erhöht, das Verständnis verbessert und das Anzeigen des Videos in geräuschempfindlichen Umgebungen, z. B. am Arbeitsplatz, ermöglicht.  

## <a name="high-contrast-mode"></a>Modus für hohen Kontrast ##

Die Standardbenutzeroberfläche von Azure Media Player ist mit den meisten Geräte-/Browseranzeigemodi für hohen Kontrast kompatibel. Konfigurationen können auf der Betriebssystemebene festgelegt werden.

## <a name="mobility-options"></a>Mobilitätsoptionen ##

### <a name="tabbing-focus"></a>Wechseln des Fokus mit TAB ###

Das von allgemeinen HTML-Standards bereitgestellte Wechseln des Fokus mit TAB ist in Azure Media Player verfügbar. Um das Wechseln des Fokus mit TAB zu aktivieren, muss `tabindex=0` (oder ein anderer Wert, wenn Sie mit den Auswirkungen der Aktivierreihenfolge in HTML vertraut sind) wie folgt dem HTML-Element `<video>` hinzugefügt werden: `<video ... tabindex=0>...</video>`. Auf manchen Plattformen ist der Fokus für die Steuerelemente möglicherweise nur vorhanden, wenn die Steuerelemente sichtbar sind und die Plattform diese Funktionen unterstützt.

Wenn das Wechseln des Fokus mit TAB aktiviert wurde, kann der Endbenutzer effektiv ohne Maus im Videoplayer navigieren und ihn steuern. Jedes Kontextmenü oder steuerbare Element ist durch Drücken von TAB erreichbar und kann durch Drücken der EINGABE- oder LEERTASTE ausgewählt werden. Wenn der Endbenutzer in einem Kontextmenü die EINGABE- oder LEERTASTE drückt, wird das Menü erweitert, damit der Benutzer durch Drücken von TAB weiter durch das Menü navigieren und ein Menüelement auswählen kann. Sobald der Kontext des auszuwählenden Elements feststeht, kann die Auswahl durch erneutes Drücken der EINGABE- oder LEERTASTE abgeschlossen werden.

### <a name="hotkeys"></a>Abkürzungstasten ###

Azure Media Player unterstützt die Steuerung per Abkürzungstaste auf der Tastatur. In einem Webbrowser kann das zugrunde liegende Videoelement nur gesteuert werden, wenn der Player fokussiert ist. Sobald der Player fokussiert ist, können die Funktionen des Players per Abkürzungstaste gesteuert werden.  In der folgenden Tabelle werden die verschiedenen Abkürzungstasten und das jeweilige Verhalten beschrieben:

| Abkürzungstaste              | Verhalten                                                                |
|----------------------|-------------------------------------------------------------------------|
| F/f                  | Dient zum Aktivieren/Deaktivieren des Vollbildmodus des Players.                                  |
| M/m                  | Dient zum Aktivieren/Deaktivieren der Stummschaltung des Players.                                          |
| NACH-OBEN-/NACH-UNTEN-TASTE    | Dient zum Erhöhen/Verringern der Lautstärke des Players.                                    |
| NACH-LINKS-/NACH-RECHTS-TASTE | Dient zum Vor-/Zurückspulen des Videos.                                  |
| 0, 1, 2, 3, 4, 5, 6, 7, 8, 9  | Dient zum Anpassen der Position innerhalb des Videos abhängig von der gedrückten Taste (zwischen 0 Prozent und 90 Prozent). |
| Klickaktion         | Dient zum Wiedergeben/Anhalten des Videos.                                                   |

## <a name="next-steps"></a>Nächste Schritte

<!---Some context for the following links goes here--->
- [Schnellstart für Azure Media Player](azure-media-player-quickstart.md)