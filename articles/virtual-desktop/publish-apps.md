---
title: 'Veröffentlichen integrierter Anwendungen in Azure Virtual Desktop: Azure'
description: So veröffentlichen Sie integrierte Anwendungen in Azure Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
ms.custom: devx-track-azurepowershell
manager: femila
ms.openlocfilehash: fc704b992100430c247260b730929e6bc049b98d
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111753069"
---
# <a name="publish-built-in-apps-in-azure-virtual-desktop"></a>Veröffentlichen integrierter Anwendungen in Azure Virtual Desktop

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/publish-apps-2019.md).

In diesem Artikel erfahren Sie, wie Sie Apps in Ihrer Azure Virtual Desktop-Umgebung veröffentlichen.

## <a name="publish-built-in-apps"></a>Veröffentlichen von integrierten Apps

So veröffentlichen Sie eine integrierte App:

1. Stellen Sie eine Verbindung mit einem der virtuellen Computer in Ihrem Hostpool her.
2. Rufen Sie den **PackageFamilyName** der zu veröffentlichenden App ab. Befolgen Sie dazu die Anweisungen in [diesem Artikel](/powershell/module/appx/get-appxpackage).
3. Führen Sie schließlich das folgende Cmdlet aus; ersetzen Sie dabei `<PackageFamilyName>` durch den **PackageFamilyName** aus dem vorherigen Schritt:

   ```powershell
   New-AzWvdApplication -Name <applicationname> -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname> -FilePath "shell:appsFolder\<PackageFamilyName>!App" -CommandLineSetting <Allow|Require|DoNotAllow> -IconIndex 0 -IconPath <iconpath> -ShowInPortal:$true
   ```

>[!NOTE]
> Azure Virtual Desktop unterstützt nur das Veröffentlichen von Apps mit Installationsspeicherorten, die mit `C:\Program Files\WindowsApps`beginnen.

## <a name="update-app-icons"></a>Aktualisieren von App-Symbolen

Nach dem Veröffentlichen einer App weist diese das Windows-Standardsymbol für Apps und nicht ihr normales App-Symbolbild auf. Wenn Sie das Symbol in das normale Symbol ändern möchten, legen Sie das Bild für das gewünschte Symbol auf einer Netzwerkfreigabe ab. Unterstützte Bildformate sind PNG, BMP, GIF, JPG, JPEG und ICO.

## <a name="publish-microsoft-edge"></a>Veröffentlichen von Microsoft Edge

Der Vorgang zum Veröffentlichen von Microsoft Edge unterscheidet sich etwas von dem Vorgang beim Veröffentlichen anderer Apps. Führen Sie dieses Cmdlet aus, um Microsoft Edge mit der standardmäßigen Startseite zu veröffentlichen:

```powershell
New-AzWvdApplication -Name -ResourceGroupName -ApplicationGroupName -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge" -CommandLineSetting <Allow|Require|DoNotAllow> -iconPath "C:\Windows\SystemApps\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\microsoftedge.exe" -iconIndex 0 -ShowInPortal:$true
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich unter [Anpassen von Feeds für Azure Virtual Desktop-Benutzer](customize-feed-for-virtual-desktop-users.md), wie Sie Feeds konfigurieren, um festzulegen, wie Apps für Benutzer angezeigt werden.
- Machen Sie sich unter [Einrichten des MSIX-Features zum Anfügen von Apps](app-attach.md) mit dem MSIX-Feature zum Anfügen von Apps vertraut.

