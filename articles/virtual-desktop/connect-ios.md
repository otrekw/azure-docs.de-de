---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop unter iOS: Azure'
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des iOS-Clients.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: helohr
ms.openlocfilehash: 472b8800a09cb12bc3e703ef53d0aab2b578e0d3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75460217"
---
# <a name="connect-with-the-ios-client"></a>Herstellen einer Verbindung mit dem iOS-Client

> Gilt für: iOS 13.0 oder höher. Kompatibel mit iPhone, iPad und iPod Touch.

Mit unserem herunterladbaren Client können Sie über Ihr iOS-Gerät auf virtuelle Windows Virtual Desktop-Ressourcen zugreifen. In dieser Anleitung erfahren Sie, wie Sie den iOS-Client einrichten.

## <a name="install-the-ios-client"></a>Installieren des iOS-Clients

Beginnen Sie mit dem [Herunterladen](https://aka.ms/rdios) des Clients, und installieren Sie ihn auf Ihrem iOS-Gerät.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, der vom Administrator bereitgestellt wird, um die Liste der verwalteten Ressourcen zu erhalten, auf die Sie über Ihr iOS-Gerät zugreifen können.

Abonnieren Sie einen Feed wie folgt:

1. Tippen Sie im Verbindungscenter auf **+** , und tippen Sie dann auf **Arbeitsbereich hinzufügen**.
2. Geben Sie die Feed-URL in das Feld **Feed-URL** ein. Die Feed-URL kann eine URL oder eine E-Mail-Adresse sein.
   - Wenn Sie eine URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat. Normalerweise lautet die URL <https://rdweb.wvd.microsoft.com>.
   - Geben Sie Ihre E-Mail-Adresse ein, um E-Mail zu verwenden. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn der Administrator den Server auf diese Weise konfiguriert hat.
3. Tippen Sie auf **Weiter**.
4. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.
   - Geben Sie als **Benutzername** den Benutzernamen mit der Berechtigung für den Zugriff auf Ressourcen an.
   - Geben Sie als **Kennwort** das diesem Benutzernamen zugeordnete Kennwort an.
   - Sie werden möglicherweise auch aufgefordert, zusätzliche Faktoren anzugeben, wenn Ihr Administrator die Authentifizierung entsprechend konfiguriert hat.
5. Tippen Sie auf **Speichern**.

Danach sollte das Verbindungscenter die Remoteressourcen anzeigen.

Sobald Sie einen Feed abonniert haben, wird der Inhalt des Feeds in regelmäßigen Abständen automatisch aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden des iOS-Betaclients finden Sie in der Dokumentation [Erste Schritte mit dem iOS-Client](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios).
