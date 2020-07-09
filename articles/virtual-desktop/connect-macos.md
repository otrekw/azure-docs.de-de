---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop unter macOS: Azure'
description: Vorgehensweise beim Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des macOS-Clients.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f2b4696886c898eec5895c4ccb59347cf2a49f03
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85208993"
---
# <a name="connect-with-the-macos-client"></a>Herstellen einer Verbindung mit dem macOS-Client

> Gilt für: macOS 10.12 oder höher

>[!IMPORTANT]
>Dieser Artikel gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/connect-macos-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit unserem herunterladbaren Client können Sie über Ihre macOS-Geräte auf Windows Virtual Desktop-Ressourcen zugreifen. In dieser Anleitung erfahren Sie, wie Sie den-Client einrichten.

## <a name="install-the-client"></a>Installieren des Clients

Beginnen Sie, indem Sie den Client [herunterladen](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12)  und auf Ihrem macOS-Gerät installieren.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, den Ihr Administrator Ihnen mitgeteilt hat, um die Liste der verwalteten Ressourcen abzurufen, die Ihnen auf Ihrem macOS-Gerät zur Verfügung stehen.

Abonnieren Sie einen Feed wie folgt:

1. Wählen Sie auf der Hauptseite **Arbeitsbereich hinzufügen** aus, um eine Verbindung mit dem Dienst herzustellen und Ihre Ressourcen abzurufen.
2. Geben Sie die Feed-URL ein. Dies kann eine URL oder E-Mail-Adresse sein:
   - Wenn Sie eine URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat. Normalerweise lautet die URL <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>.
   - Geben Sie Ihre E-Mail-Adresse ein, um E-Mail zu verwenden. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn der Administrator den Server auf diese Weise konfiguriert hat.
3. Wählen Sie **Hinzufügen**.
4. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.

Nachdem Sie sich angemeldet haben, sollte eine Liste der verfügbaren Ressourcen angezeigt werden.

Sobald Sie einen Feed abonniert haben, wird der Inhalt des Feeds in regelmäßigen Abständen automatisch aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum macOS-Client finden Sie in der Dokumentation [Erste Schritte mit dem macOS-Client](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/).
