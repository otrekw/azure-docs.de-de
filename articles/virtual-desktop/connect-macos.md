---
title: Herstellen einer Verbindung von macOs zu Azure Virtual Desktop – Azure
description: Informationen zum Herstellen einer Verbindung mit Azure Virtual Desktop mithilfe des macOS-Clients.
author: Heidilohr
ms.topic: how-to
ms.date: 04/08/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: a77df2eabdb1ff5417ec0c3b7b8c6733f993a09b
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111756003"
---
# <a name="connect-to-azure-virtual-desktop-with-the-macos-client"></a>Herstellen einer Verbindung mit Azure Virtual Desktop mithilfe des macOS-Clients

> Gilt für: macOS 10.12 oder höher

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/connect-macos-2019.md).

Mit unserem herunterladbaren Client können Sie von Ihren macOS-Geräten auf Azure Virtual Desktop-Ressourcen zugreifen. In dieser Anleitung erfahren Sie, wie Sie den-Client einrichten.

## <a name="install-the-client"></a>Installieren des Clients

Beginnen Sie, indem Sie den Client [herunterladen](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) und auf Ihrem macOS-Gerät installieren.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, den Ihr Administrator Ihnen mitgeteilt hat, um die Liste der verwalteten Ressourcen abzurufen, die Ihnen auf Ihrem macOS-Gerät zur Verfügung stehen.

Abonnieren Sie einen Feed wie folgt:

1. Wählen Sie auf der Hauptseite **Arbeitsbereich hinzufügen** aus, um eine Verbindung mit dem Dienst herzustellen und Ihre Ressourcen abzurufen.
2. Geben Sie die Feed-URL ein. Dies kann eine URL oder E-Mail-Adresse sein:
   - Verwende im Fall einer URL die vom Administrator erhaltene. Normalerweise lautet die URL <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>.
   - Um E-Mail zu verwenden, gib deine E-Mail-Adresse ein. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn der Administrator den Server auf diese Weise konfiguriert hat.
   - Verwenden Sie <https://rdweb.wvd.azure.us/api/arm/feeddiscovery>, um eine Verbindung über das US Gov-Portal herzustellen.
3. Wählen Sie **Hinzufügen**.
4. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.

Nachdem du dich angemeldet hast, sollte eine Liste der verfügbaren Ressourcen angezeigt werden.

Nachdem du einen Feed abonniert hast, wird der Inhalt dieses Feeds automatisch regelmäßig aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum macOS-Client finden Sie in der Dokumentation [Erste Schritte mit dem macOS-Client](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/).
