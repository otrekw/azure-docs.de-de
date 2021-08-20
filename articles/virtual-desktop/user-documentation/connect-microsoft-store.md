---
title: Herstellen einer Verbindung mit dem Microsoft Store-Client – Azure
description: Hier erfahren Sie, wie Sie über den Microsoft Store-Client eine Verbindung mit Azure Virtual Desktop herstellen.
author: Heidilohr
ms.topic: how-to
ms.date: 10/05/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: d48ce1f2d885783901f4b5d74dfaebf1854943ab
ms.sourcegitcommit: b044915306a6275c2211f143aa2daf9299d0c574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113034246"
---
# <a name="connect-with-the-microsoft-store-client"></a>Herstellen einer Verbindung mit dem Microsoft Store-Client

>Gilt für: Windows 10

Sie können auf Geräten mit Windows 10 auf Azure Virtual Desktop-Ressourcen zugreifen.

## <a name="install-the-microsoft-store-client"></a>Installieren des Microsoft Store-Clients

Sie können den Client für den aktuellen Benutzer installieren. Hierfür sind keine Administratorrechte erforderlich. Alternativ kann Ihr Administrator den Client installieren und konfigurieren, damit alle Benutzer auf dem Gerät darauf zugreifen können.

Nach der Installation kann der Client über das Startmenü gestartet werden, indem Sie nach Remote Desktop suchen.

Laden Sie zunächst [den Client im Microsoft Store herunter, und installieren Sie ihn](https://www.microsoft.com/store/productId/9WZDNCRFJ3PS).

## <a name="subscribe-to-a-workspace"></a>Abonnieren eines Arbeitsbereichs

Abonnieren Sie den von Ihrem Administrator bereitgestellten Arbeitsbereich, um die Liste der verwalteten Ressourcen zu erhalten, auf die Sie über Ihren PC zugreifen können.

So abonnieren Sie einen Arbeitsbereich

1. Klicken Sie im Verbindungscenterbildschirm auf **+ Hinzufügen** und dann auf **Arbeitsbereiche**.
2. Geben Sie im Feld „Arbeitsbereich-URL“ die von Ihrem Administrator bereitgestellte Arbeitsbereich-URL an. Es kann sich dabei um eine URL oder eine E-Mail-Adresse handeln.
   
   - Wenn Sie eine Arbeitsbereich-URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat.
   - Wenn Sie über Azure Virtual Desktop eine Verbindung herstellen, verwenden Sie eine der folgenden URLs, je nachdem, welche Version des Diensts Sie verwenden:
       - Azure Virtual Desktop (klassisch): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`.
       - Azure Virtual Desktop: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`.
  
3. Klicken Sie auf **Abonnieren**.
4. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.
5. Nach dem Abonnieren sollten die Arbeitsbereiche im Verbindungscenter angezeigt werden.

Arbeitsbereiche können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Microsoft Store-Clients finden Sie unter [Erste Schritte mit dem Microsoft Store-Client](/windows-server/remote/remote-desktop-services/clients/windows/).