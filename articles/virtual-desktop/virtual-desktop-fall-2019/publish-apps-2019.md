---
title: Veröffentlichen von integrierten Apps in Windows Virtual Desktop – Azure
description: Hier erfahren Sie, wie Sie integrierte Apps in Windows Virtual Desktop veröffentlichen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 7a453ef44a1ede86290f4130a6147eaaad09fa97
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85214144"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Veröffentlichen von integrierten Apps in Windows Virtual Desktop

>[!IMPORTANT]
>Dieser Inhalt gilt für das Release vom Herbst 2019, das keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, die im Update vom Frühjahr 2020 eingeführt wurden, finden Sie weitere Informationen in [diesem Artikel](../publish-apps.md).

In diesem Artikel erfahren Sie, wie Sie Apps in Ihrer Windows Virtual Desktop-Umgebung veröffentlichen.

## <a name="publish-built-in-apps"></a>Veröffentlichen von integrierten Apps

So veröffentlichen Sie eine integrierte App:

1. Stellen Sie eine Verbindung mit einem der virtuellen Computer in Ihrem Hostpool her.
2. Rufen Sie den **PackageFamilyName** der zu veröffentlichenden App ab. Befolgen Sie dazu die Anweisungen in [diesem Artikel](/powershell/module/appx/get-appxpackage?view=win10-ps/).
3. Führen Sie schließlich das folgende Cmdlet aus; ersetzen Sie dabei `<PackageFamilyName>` durch den **PackageFamilyName** aus dem vorherigen Schritt:

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Windows Virtual Desktop unterstützt nur das Veröffentlichen von Apps mit Installationsspeicherorten, die mit `C:\Program Files\Windows Apps`beginnen.

## <a name="update-app-icons"></a>Aktualisieren von App-Symbolen

Nach dem Veröffentlichen einer App weist diese das Windows-Standardsymbol für Apps und nicht ihr normales App-Symbolbild auf. Wenn Sie das Symbol in das normale Symbol ändern möchten, legen Sie das Bild für das gewünschte Symbol auf einer Netzwerkfreigabe ab. Unterstützte Bildformate sind PNG, BMP, GIF, JPG, JPEG und ICO.

## <a name="publish-microsoft-edge"></a>Veröffentlichen von Microsoft Edge

Der Vorgang zum Veröffentlichen von Microsoft Edge unterscheidet sich etwas von dem Vorgang beim Veröffentlichen anderer Apps. Führen Sie dieses Cmdlet aus, um Microsoft Edge mit der standardmäßigen Startseite zu veröffentlichen:

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge"
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich unter [Anpassen von Feeds für Windows Virtual Desktop-Benutzer](customize-feed-virtual-desktop-users-2019.md), wie Sie Feeds konfigurieren, um festzulegen, wie Apps für Benutzer angezeigt werden.
- Machen Sie sich unter [Einrichten des MSIX-Features zum Anfügen von Apps](../app-attach.md) mit dem MSIX-Feature zum Anfügen von Apps vertraut.

