---
title: 'Windows Virtual Desktop-Hostpool in Azure Resource Manager: Azure'
description: Beschreibt, wie Sie in Windows Virtual Desktop mit einer Azure Resource Manager-Vorlage einen Hostpool erstellen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
ms.openlocfilehash: 5b07416f785ad263b4dbb9a0d249cb6022c01b13
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2020
ms.locfileid: "77367470"
---
# <a name="create-a-host-pool-with-an-azure-resource-manager-template"></a>Erstellen eines Hostpools mit einer Azure Resource Manager-Vorlage

Hostpools sind eine Sammlung identischer virtueller Computer innerhalb von Windows Virtual Desktop-Mandantenumgebungen. Jeder Hostpool kann eine App-Gruppe enthalten, mit der Benutzer genau wie auf einem physischen Desktop interagieren können.

Befolgen Sie die Anleitung in diesem Abschnitt, um mit einer von Microsoft bereitgestellten Azure Resource Manager-Vorlage einen Hostpool für einen Windows Virtual Desktop-Mandanten zu erstellen. In diesem Artikel wird beschrieben, wie Sie in Windows Virtual Desktop einen Hostpool erstellen, eine Ressourcengruppe mit VMs unter einem Azure-Abonnement erstellen und diese VMs dann in die AD-Domäne einbinden und für Windows Virtual Desktop registrieren.

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>Benötigte Komponenten zum Ausführen der Azure Resource Manager-Vorlage

Stellen Sie sicher, dass Sie Folgendes wissen, bevor Sie die Azure Resource Manager-Vorlage ausführen:

- Wo befindet sich die Quelle des Images, das Sie verwenden möchten? Stammt es aus dem Azure-Katalog, oder handelt es sich um ein benutzerdefiniertes Image?
- Wo befinden sich Ihre Anmeldeinformationen für den Domänenbeitritt?
- Wo befinden sich Anmeldeinformationen für Windows Virtual Desktop?

Beim Erstellen eines Windows Virtual Desktop-Hostpools mit der Azure Resource Manager-Vorlage können Sie einen virtuellen Computer über den Azure-Katalog, ein verwaltetes Image oder ein nicht verwaltetes Image erstellen. Weitere Informationen zur Erstellung von VM-Images finden Sie unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](../virtual-machines/windows/prepare-for-upload-vhd-image.md) bzw. unter [Erstellen eines verwalteten Images eines generalisierten virtuellen Computers in Azure](../virtual-machines/windows/capture-image-resource.md).

## <a name="run-the-azure-resource-manager-template-for-provisioning-a-new-host-pool"></a>Ausführen der Azure Resource Manager-Vorlage zum Bereitstellen eines neuen Hostpools

Greifen Sie auf [diese GitHub-URL](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool) zu, um zu beginnen.

### <a name="deploy-the-template-to-azure"></a>Bereitstellen der Vorlage in Azure

Scrollen Sie beim Bereitstellen unter einem Enterprise-Abonnement nach unten, und wählen Sie **In Azure bereitstellen**. Navigieren Sie weiter, und fügen Sie die Parameter basierend auf Ihrer Imagequelle ein.

Führen Sie diese Schritte zum Bereitstellen in Azure aus, wenn Sie die Bereitstellung unter einem Cloud Solution Provider-Abonnement durchführen:

1. Scrollen Sie nach unten, klicken Sie mit der rechten Maustaste auf **In Azure bereitstellen**, und wählen Sie dann **Link-Adresse kopieren**.
2. Öffnen Sie einen Text-Editor, z. B. Editor, und fügen Sie den Link ein.
3. Geben Sie direkt nach „https://portal.azure.com/“ und vor dem Hashtag (#) ein at-Zeichen (@) gefolgt vom Namen der Mandantendomäne ein. Hier ist ein Beispiel für das Format angegeben, das Sie verwenden sollten: https://portal.azure.com/@Contoso.onmicrosoft.com#create/.
4. Melden Sie sich am Azure-Portal als Benutzer mit den Berechtigungen „Administrator/Mitwirkender“ am Cloud Solution Provider-Abonnement an.
5. Fügen Sie den Link, den Sie in den Text-Editor kopiert haben, in die Adressleiste ein.

Eine Anleitung dazu, welche Parameter Sie für Ihr Szenario eingeben sollten, finden Sie in der [Infodatei](https://github.com/Azure/RDS-Templates/blob/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool/README.md) zu Windows Virtual Desktop. Die Infodatei wird immer mit den neuesten Änderungen aktualisiert.

## <a name="assign-users-to-the-desktop-application-group"></a>Zuweisen von Benutzern zur Desktopanwendungsgruppe

Nachdem der Vorgang für die GitHub-Azure Resource Manager-Vorlage abgeschlossen ist, sollten Sie den Benutzerzugriff zuweisen, bevor Sie mit dem Testen der vollständigen Sitzungsdesktops auf Ihren virtuellen Computern beginnen.

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](/powershell/windows-virtual-desktop/overview/), um es in Ihrer PowerShell-Sitzung verwenden zu können.

Öffnen Sie zum Zuweisen von Benutzern zur Desktopanwendungsgruppe ein PowerShell-Fenster, und führen Sie dieses Cmdlet aus, um sich an der Windows Virtual Desktop-Umgebung anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Fügen Sie die Benutzer anschließend mit diesem Cmdlet der Desktopanwendungsgruppe hinzu:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Der UPN des Benutzers muss der Identität des Benutzers in Azure Active Directory entsprechen (Beispiel: user1@contoso.com). Wenn Sie mehrere Benutzer hinzufügen möchten, müssen Sie dieses Cmdlet für jeden Benutzer separat ausführen.

Nach Abschluss dieser Schritte können sich Benutzer, die der Desktopanwendungsgruppe hinzugefügt wurden, mit unterstützten Remotedesktopclients bei Windows Virtual Desktop anmelden und eine Ressource für einen Sitzungsdesktop anzeigen.

>[!IMPORTANT]
>Zum Schutz Ihrer Windows Virtual Desktop-Umgebung in Azure empfiehlt es sich, den eingehenden Port 3389 auf Ihren virtuellen Computern nicht zu öffnen. Für Windows Virtual Desktop muss der eingehende Port 3389 nicht geöffnet sein, damit Benutzer auf die virtuellen Computer des Hostpools zugreifen können. Wenn Sie den Port 3389 zur Problembehandlung öffnen müssen, verwenden Sie am besten den [Just-In-Time-Zugriff auf virtuelle Computer](../security-center/security-center-just-in-time.md).