---
title: 'Linux-Unterstützung für Windows Virtual Desktop: Azure'
description: Eine kurze Übersicht über die Linux-Unterstützung für Windows Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: bd3cc6c5220e2e84cbbd30b29b8034f53c813f1e
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88008694"
---
# <a name="linux-support"></a>Linux-Unterstützung

Mit den folgenden unterstützten Clients, die von unseren Linux Thin Client-Partnern bereitgestellt werden, können Sie von Ihren Linux-Geräten aus auf Windows Virtual Desktop-Ressourcen zugreifen. Wir arbeiten mit einer Reihe von Partnern zusammen, um unterstützte Windows Virtual Desktop-Clients auf mehr Linux-basierten Betriebssystemen und Geräten zu ermöglichen. Wenn Sie Windows Virtual Desktop-Unterstützung auf einer Linux-Plattform wünschen, die hier nicht aufgeführt ist, teilen Sie uns dies auf unserer [UserVoice-Seite](https://remotedesktop.uservoice.com/forums/923035-remote-desktop-support-on-linux) mit.

## <a name="connect-with-your-linux-device"></a>Herstellen einer Verbindung mit Ihrem Linux-Gerät

Die folgenden Partner haben Windows Virtual Desktop-Clients für Linux-Geräte genehmigt.

|Partner|Partnerdokumentation|Partnersupport|
|:------|:--------------------|:--------------|
|![IGEL-Logo](./media/partners/igel.png)|[IGEL-Clientdokumentation](https://www.igel.com/igel-solution-family/windows-virtual-desktop/)|[IGEL-Support](https://www.igel.com/support/)|

## <a name="what-is-the-linux-sdk"></a>Was ist das Linux SDK?

Linux Thin Client-Partner können die Linux SDK-APIs von Windows Virtual Desktop verwenden, um Ressourcenfeeds abzurufen, eine Verbindung mit Desktop- oder Remoteanwendungssitzungen herzustellen und viele der Umleitungen zu verwenden, die von unseren Erstanbieterclients unterstützt werden. Das SDK ist mit den meisten Betriebssystemen kompatibel, die auf Ubuntu 18.04 oder höher basieren.

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

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Dokumentation zu den folgenden Clients an:

- [Windows Desktop-Client](connect-windows-7-10.md)
- [Webclient](connect-web.md)
- [Android-Client](connect-android.md)
- [macOS-Client](connect-macos.md)
- [iOS-Client](connect-ios.md)
