---
title: Herstellen einer Windows Virtual Desktop-Webclientverbindung – Azure
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des Webclients.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: e95498498fd15d7a6e73630e218f9bdd1b892f09
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85204794"
---
# <a name="connect-with-the-web-client"></a>Herstellen einer Verbindung mit dem Webclient

>[!IMPORTANT]
>Dieser Inhalt gilt für das Release vom Herbst 2019, das keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, die im Update vom Frühjahr 2020 eingeführt wurden, finden Sie weitere Informationen in [diesem Artikel](../connect-web.md).

Mit dem Webclient können Sie über einen Webbrowser auf Ihre Ressourcen von Windows Virtual Desktop zugreifen, ohne den langwierigen Installationsvorgang durchführen zu müssen.

>[!NOTE]
>Der Webclient verfügt derzeit nicht über Unterstützung für Mobilgerät-Betriebssysteme.

## <a name="supported-operating-systems-and-browsers"></a>Unterstützte Betriebssysteme und Browser

Es sollten alle HTML5-fähigen Browser funktionieren, aber hier sind die Betriebssysteme und Browser angegeben, die offiziell unterstützt werden.

| Browser           | Unterstütztes Betriebssystem                     | Notizen               |
|-------------------|----------------------------------|---------------------|
| Microsoft Edge    | Windows                          |                     |
| Internet Explorer | Windows                          |                     |
| Apple Safari      | macOS                            |                     |
| Mozilla Firefox   | Windows, macOS, Linux            | Version 55 oder höher |
| Google Chrome     | Windows, macOS, Linux, Chrome OS |                     |

## <a name="access-remote-resources-feed"></a>Zugreifen auf den Feed für Remoteressourcen

Navigieren Sie in einem Browser unter <https://rdweb.wvd.microsoft.com/webclient> zum Windows Virtual Desktop-Webclient, und melden Sie sich mit Ihrem Benutzerkonto an.

>[!NOTE]
>Wenn Sie das Windows Virtual Desktop-Release vom Frühjahr 2020 mit Azure Resource Manager-Integration verwenden, stellen Sie stattdessen bei <https://rdweb.wvd.microsoft.com/arm/webclient> eine Verbindung mit Ihren Ressourcen her.

>[!NOTE]
>Wenn Sie sich bereits mit einem anderen Azure Active Directory-Konto angemeldet haben als dem, das Sie für Windows Virtual Desktop verwenden möchten, sollten Sie sich entweder abmelden oder ein privates Browserfenster verwenden.

Nach dem Anmelden sollte eine Liste mit Ressourcen angezeigt werden. Sie können Ressourcen starten, indem Sie sie auf der Registerkarte **Alle Ressourcen** wie eine normale App auswählen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Webclients finden Sie unter [Erste Schritte mit dem Webclient](/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client).
