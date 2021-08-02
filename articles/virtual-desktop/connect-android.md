---
title: Verbinden mit Azure Virtual Desktop von Android aus – Azure
description: So verbinden Sie den Android-Client mit Azure Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 03/25/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: bb88149c3efb6eca528fe38141b3a472b2990107
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111757929"
---
# <a name="connect-to-azure-virtual-desktop-with-the-android-client"></a>Verbinden des Android-Clients mit Windows Virtual Desktop

> Gilt für: Android 4.1 und höher, Chromebooks mit ChromeOS 53 und höher.

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/connect-android-2019.md).

Mit unserem herunterladbaren Client können Sie von Ihrem Android-Gerät auf Azure Virtual Desktop-Ressourcen zugreifen. Sie können den Android-Client auch auf Chromebook-Geräten verwenden, die Google Play Store unterstützen. In dieser Anleitung erfahren Sie, wie Sie den Android-Client einrichten.

## <a name="install-the-android-client"></a>Installieren des Android-Clients

Beginnen Sie mit dem [Herunterladen](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) des Clients, und installieren Sie ihn auf Ihrem Android-Gerät.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, der vom Administrator bereitgestellt wird, um die Liste der verwalteten Ressourcen zu erhalten, auf die Sie über Ihr Android-Gerät zugreifen können.

Abonnieren Sie einen Feed wie folgt:

1. Tippen Sie im Connection Center auf **+** und dann auf **Remote Resource Feed**.
2. Geben Sie die Feed-URL in das Feld **Feed-URL** ein. Die Feed-URL kann eine URL oder eine E-Mail-Adresse sein.
   - Wenn Sie eine URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat, normalerweise <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>.
   - Geben Sie Ihre E-Mail-Adresse ein, um E-Mail zu verwenden. Wenn Ihr Administrator den Server entsprechend konfiguriert hat, sucht der Client nach einer Ihrer E-Mail-Adresse zugeordneten URL.
   - Verwenden Sie <https://rdweb.wvd.azure.us/api/arm/feeddiscovery>, um eine Verbindung über das US Gov-Portal herzustellen.
3. Tippen Sie auf **WEITER**.
4. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.
   - Geben Sie als **Benutzername** den Benutzernamen mit der Berechtigung für den Zugriff auf Ressourcen an.
   - Geben Sie als **Kennwort** das diesem Benutzernamen zugeordnete Kennwort an.
   - Sie werden möglicherweise auch aufgefordert, zusätzliche Faktoren anzugeben, wenn Ihr Administrator die Authentifizierung entsprechend konfiguriert hat.

Danach sollte das Connection Center die Remoteressourcen anzeigen.

Sobald Sie einen Feed abonniert haben, wird der Inhalt des Feeds in regelmäßigen Abständen automatisch aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Android-Clients finden Sie unter [Erste Schritte mit dem Android-Client](/windows-server/remote/remote-desktop-services/clients/remote-desktop-android/).
