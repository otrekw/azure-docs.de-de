---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop (klassisch) unter macOS: Azure'
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop (klassisch) mithilfe des macOS-Clients.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a0d2bd6a07cfc9e5ebe205322550c9e898022846
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88008439"
---
# <a name="connect-to-windows-virtual-desktop-classic-with-the-macos-client"></a>Herstellen einer Verbindung mit Windows Virtual Desktop (klassisch) mithilfe des macOS-Clients

> Gilt für: macOS 10.12 oder höher

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop (klassisch). Der Dienst unterstützt keine Windows Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../connect-macos.md) weiter.

Mit unserem herunterladbaren Client können Sie über Ihre macOS-Geräte auf Windows Virtual Desktop-Ressourcen zugreifen. In dieser Anleitung erfahren Sie, wie Sie den-Client einrichten.

## <a name="install-the-client"></a>Installieren des Clients

Beginnen Sie, indem Sie den Client [herunterladen](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) und auf Ihrem macOS-Gerät installieren.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, den Ihr Administrator Ihnen mitgeteilt hat, um die Liste der verwalteten Ressourcen abzurufen, die Ihnen auf Ihrem macOS-Gerät zur Verfügung stehen.

Abonnieren Sie einen Feed wie folgt:

1. Wählen Sie auf der Hauptseite **Arbeitsbereich hinzufügen** aus, um eine Verbindung mit dem Dienst herzustellen und Ihre Ressourcen abzurufen.
2. Geben Sie die Feed-URL ein. Dies kann eine URL oder E-Mail-Adresse sein:
   - Wenn Sie eine URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat. Normalerweise lautet die URL <https://rdweb.wvd.microsoft.com>.
   - Um E-Mail zu verwenden, gib deine E-Mail-Adresse ein. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn der Administrator den Server auf diese Weise konfiguriert hat.
3. Wählen Sie **Hinzufügen**.
4. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.

Nachdem du dich angemeldet hast, sollte eine Liste der verfügbaren Ressourcen angezeigt werden.

Nachdem du einen Feed abonniert hast, wird der Inhalt dieses Feeds automatisch regelmäßig aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum macOS-Client finden Sie in der Dokumentation [Erste Schritte mit dem macOS-Client](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/).