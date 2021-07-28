---
title: 'Azure Virtual Desktop-Hostpool (klassisch) zum Überwachen von Dienstupdates: Azure'
description: Hier erfahren Sie, wie Sie in Azure Virtual Desktop (klassisch) einen Überprüfungshostpool für die Überwachung von Dienstupdates erstellen, bevor Updates in der Produktion bereitgestellt werden.
author: Heidilohr
ms.topic: tutorial
ms.date: 05/27/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: bca4d55d0c5abb4e133a8eef8b51a2a5ae57b5d5
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111749829"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates-in-azure-virtual-desktop-classic"></a>Tutorial: Erstellen eines Hostpools zum Überprüfen von Dienstupdates in Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../create-validation-host-pool.md) weiter.

Hostpools sind eine Sammlung identischer virtueller Computer innerhalb von Azure Virtual Desktop-Mandantenumgebungen. Es wird empfohlen, einen Überprüfungshostpool zu erstellen, in dem Dienstupdates zuerst angewendet werden. Auf diese Weise können Sie Dienstupdates überwachen, bevor sie vom Dienst auf Ihre Standardumgebung oder Nicht-Überprüfungsumgebung angewendet werden. Ohne einen Überprüfungshostpool können Sie keine Änderungen erkennen, die Fehler einführen, was zu Ausfallzeiten für Benutzer in der Produktionsumgebung führen könnte.

Um sicherzustellen, dass Ihre Apps mit den neuesten Updates funktionieren, sollte der Überprüfungshostpool Hostpools in Ihrer Nicht-Überprüfungsumgebung so ähnlich wie möglich sein. Benutzer sollten so häufig eine Verbindung mit dem Überprüfungshostpool herstellen wie mit dem Standardhostpool. Wenn Sie automatisierte Tests mit Ihrem Hostpool durchführen, sollten Sie auch beim Überprüfungshostpool automatisierte Tests durchführen.

Sie können Probleme beim Überprüfungshostpool entweder mit der [Diagnosefunktion](diagnostics-role-service-2019.md) oder den [Artikeln zur Problembehandlung bei Azure Virtual Desktop](troubleshoot-set-up-overview-2019.md) debuggen.

>[!NOTE]
> Wir empfehlen, dass Sie den Überprüfungshostpool eingerichtet lassen, um alle zukünftigen Updates zu testen.

Zur Vorbereitung müssen Sie ggf. zunächst das [Azure Virtual Desktop-PowerShell-Modul herunterladen und importieren](/powershell/windows-virtual-desktop/overview/). Führen Sie anschließend das folgende Cmdlet aus, um sich bei Ihrem Konto anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="create-your-host-pool"></a>Erstellen Ihres Hostpools

Sie können einen Hostpool erstellen, indem Sie die Anweisungen in diesen Artikeln befolgen:
- [Tutorial: Erstellen eines Hostpools mit Azure Marketplace](create-host-pools-azure-marketplace-2019.md)
- [Erstellen eines Hostpools mit einer Azure Resource Manager-Vorlage](create-host-pools-arm-template.md)
- [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell-2019.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Definieren Ihres Hostpools als Überprüfungshostpool

Führen Sie die folgenden PowerShell-Cmdlets aus, um den neuen Hostpool als Überprüfungshostpool zu definieren. Ersetzen Sie die Werte in Anführungszeichen durch die für Ihre Sitzung relevanten Werte:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Führen Sie das folgende PowerShell-Cmdlet aus, um zu bestätigen, dass die Überprüfungseigenschaft festgelegt ist. Ersetzen Sie die Werte in Anführungszeichen durch die für Ihre Sitzung relevanten Werte.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool"
```

Die Ergebnisse des Cmdlets sollte der folgenden Ausgabe ähneln:

```
    TenantName          : contoso
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False
    CustomRdpProperty    : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Zeitplan für Updates

Dienstupdates werden monatlich bereitgestellt. Wenn schwerwiegende Probleme vorliegen, werden kritische Updates in häufigeren Abständen bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Überprüfungshostpool erstellt haben, können Sie sich darüber informieren, wie Sie Azure Service Health zum Überwachen Ihrer Azure Virtual Desktop-Bereitstellung verwenden.

> [!div class="nextstepaction"]
> [Einrichten von Dienstwarnungen](set-up-service-alerts-2019.md)
