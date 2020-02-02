---
title: 'Linux-Unterstützung für Windows Virtual Desktop: Azure'
description: Eine kurze Übersicht über die Linux-Unterstützung für Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: helohr
ms.openlocfilehash: f7d62487605c26a68e7c71b6d95d7814da0a07c9
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76776099"
---
# <a name="linux-support"></a>Linux-Unterstützung

Sie können das Linux SDK für Windows Virtual Desktop verwenden, um einen eigenständigen Windows Virtual Desktop-Client zu erstellen. Sie können es auch verwenden, um Unterstützung für Windows Virtual Desktop für Ihre Clientanwendung zu aktivieren. In diesem kurzen Leitfaden wird erläutert, was das Linux SDK ist und wie Sie mit seiner Verwendung beginnen können.

## <a name="what-is-the-linux-sdk"></a>Was ist das Linux SDK?

Sie können die SDK-APIs verwenden, um Ressourcenfeeds abzurufen, eine Verbindung mit Desktop- oder Remoteanwendungssitzungen herzustellen und viele der Umleitungen zu verwenden, die von unseren Erstanbieterclients unterstützt werden.

### <a name="supported-linux-distributions"></a>Unterstützte Linux-Distributionen

Das SDK ist mit den meisten Betriebssystemen kompatibel, die auf Ubuntu 18.04 oder höher basieren. Wenn Sie eine andere Linux-Distribution verwenden, können wir mit Ihnen zusammenarbeiten, um herauszufinden, wie Ihre Anforderungen optimal unterstützt werden können.

### <a name="feature-support"></a>Featureunterstützung

Das SDK unterstützt mehrere Verbindungen mit Desktop- und Remoteanwendungssitzungen. Die folgenden Umleitungen werden unterstützt:

| Umleitung       | Unterstützt |
| :---------------- | :-------: |
| Tastatur          | &#10004;  |
| Maus             | &#10004;  |
| Audioeingang          | &#10004;  |
| Audioausgang         | &#10004;  |
| Zwischenablage (Text)  | &#10004;  |
| Zwischenablage (Bild) | &#10004;  |
| Zwischenablage (Datei)  | &#10004;  |
| Smartcard         | &#10004;  |
| Laufwerk/Ordner      | &#10004;  |

Das SDK unterstützt auch Anzeigekonfigurationen für mehrere Monitore, sofern die Monitore, die Sie für Ihre Sitzung auswählen, zusammenhängend sind.

Wir aktualisieren dieses Dokument, wenn wir Unterstützung für neue Features und Umleitungen hinzufügen. Wenn Sie neue Features und andere Verbesserungen vorschlagen möchten, besuchen Sie unsere [UserVoice-Seite](https://go.microsoft.com/fwlink/?linkid=2116523).

## <a name="get-started-with-the-linux-sdk"></a>Erste Schritte mit dem Linux SDK

Bevor Sie einen Linux-Client für Windows Virtual Desktop entwickeln können, müssen Sie die folgenden Schritte ausführen:

1. Erstellen und Bereitstellen einer Windows Virtual Desktop-Umgebung für die Verwendung in Tests oder in der Produktion.
2. Testen der verfügbaren Erstanbieterclients, um sich mit der Benutzeroberfläche von Windows Virtual Desktop vertraut zu machen.

## <a name="next-steps"></a>Nächste Schritte

Sie können Zugriff auf das Linux SDK in unserem [Tech Community-Forum](https://go.microsoft.com/fwlink/?linkid=2116541)anfordern.

Sehen Sie sich die Dokumentation zu den folgenden Clients an:

- [Windows Desktop-Client](connect-windows-7-and-10.md)
- [Webclient](connect-web.md)
- [Android-Client](connect-android.md)
- [macOS-Client](connect-macos.md)
- [iOS-Client](connect-ios.md)
