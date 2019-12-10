---
title: Veröffentlichen von integrierten Apps in Windows Virtual Desktop – Azure
description: Erfahren Sie, wie Sie moderne Apps in Windows Virtual Desktop veröffentlichen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 11/25/2019
ms.author: helohr
ms.openlocfilehash: 26ebcc7f2196267719a5f49aeeb8de00d48b9c3e
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483736"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop"></a>Veröffentlichen von integrierten Apps in Windows Virtual Desktop

In diesem Artikel erfahren Sie, wie Sie Apps in Ihrer Windows Virtual Desktop-Umgebung veröffentlichen.

## <a name="publish-built-in-apps"></a>Veröffentlichen von integrierten Apps

So veröffentlichen Sie eine integrierte App:

1. Stellen Sie eine Verbindung mit einem der virtuellen Computer in Ihrem Hostpool her.
2. Rufen Sie den **PackageFamilyName** der zu veröffentlichenden App ab. Befolgen Sie dazu die Anweisungen in [diesem Artikel](https://docs.microsoft.com/powershell/module/appx/get-appxpackage?view=win10-ps).
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



