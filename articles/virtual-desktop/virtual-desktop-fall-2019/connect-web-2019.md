---
title: Herstellen einer Azure Virtual Desktop-Webclientverbindung (klassisch) – Azure
description: Informationen zum Herstellen einer Verbindung mit Azure Virtual Desktop (klassisch) mithilfe des Webclients.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: e604e5de1a5abd46961c0ec447f7d3fb29aa3b49
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111749955"
---
# <a name="connect-to-azure-virtual-desktop-classic-with-the-web-client"></a>Herstellen einer Verbindung mit Azure Virtual Desktop (klassisch) mithilfe des Webclients

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../connect-web.md) weiter.

Mit dem Webclient können Sie über einen Webbrowser auf Ihre Ressourcen von Azure Virtual Desktop zugreifen, ohne den langwierigen Installationsvorgang durchführen zu müssen.

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

Navigieren Sie in einem Browser unter <https://rdweb.wvd.microsoft.com/webclient> zum Azure Virtual Desktop-Webclient, und melden Sie sich mit Ihrem Benutzerkonto an.

>[!NOTE]
>Wenn Sie Azure Virtual Desktop mit Azure Resource Manager-Integration verwenden, stellen Sie stattdessen bei <https://rdweb.wvd.microsoft.com/arm/webclient> eine Verbindung mit Ihren Ressourcen her.

>[!NOTE]
>Wenn Sie sich bereits mit einem anderen Azure Active Directory-Konto angemeldet haben als dem, das Sie für Azure Virtual Desktop verwenden möchten, sollten Sie sich entweder abmelden oder ein privates Browserfenster verwenden.

Nach dem Anmelden sollte eine Liste mit Ressourcen angezeigt werden. Sie können Ressourcen starten, indem Sie sie auf der Registerkarte **Alle Ressourcen** wie eine normale App auswählen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Webclients finden Sie unter [Erste Schritte mit dem Webclient](/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client).
