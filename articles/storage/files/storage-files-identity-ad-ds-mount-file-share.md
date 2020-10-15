---
title: Einbinden einer Azure-Dateifreigabe in eine in AD DS eingebundene VM
description: Hier erfahren Sie, wie Sie eine Dateifreigabe in Ihre lokalen, in Active Directory Domain Services eingebundenen Computer einbinden.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: 9807563c768b82c823ff754aaa679ddc917bf62d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87535058"
---
# <a name="part-four-mount-a-file-share-from-a-domain-joined-vm"></a>Teil 4: Einbinden einer Dateifreigabe von einer in eine Domäne eingebundenen VM

Stellen Sie sicher, dass Sie den vorherigen Artikel [Konfigurieren von Berechtigungen auf Verzeichnis- und Dateiebene über SMB](storage-files-identity-ad-ds-configure-permissions.md) gelesen haben, bevor Sie mit diesem Artikel beginnen.

Mit dem in diesem Artikel beschriebenen Prozess wird überprüft, ob Ihre Dateifreigabe und Ihre Zugriffsberechtigungen ordnungsgemäß eingerichtet wurden und Sie von einem in die Domäne eingebundenen virtuellen Computer aus Zugriff auf eine Azure-Dateifreigabe haben. Es kann einige Zeit dauern, bis die Azure-Rollenzuweisung auf Freigabenebene wirksam wird. 

Melden Sie sich mithilfe der Anmeldeinformationen beim Client an, denen Sie die Berechtigungen erteilt haben (siehe folgende Abbildung).

![Screenshot mit dem Azure AD-Anmeldebildschirm für die Benutzerauthentifizierung](media/storage-files-aad-permissions-and-mounting/azure-active-directory-authentication-dialog.png)

## <a name="mounting-prerequisites"></a>Voraussetzungen für die Einbindung

Bevor Sie die Dateifreigabe einbinden können, müssen Sie sicherstellen, dass die folgenden Voraussetzungen erfüllt sind:

- Wenn Sie die Dateifreigabe von einem Client einbinden, der zuvor die Dateifreigabe mithilfe Ihres Speicherkontoschlüssels eingebunden hat, müssen Sie sicherstellen, dass Sie die Freigabe entkoppelt haben, die persistenten Anmeldeinformationen des Speicherkontoschlüssels entfernt haben und derzeit AD DS-Anmeldeinformationen für die Authentifizierung verwenden.
- Ihr Client muss über eine Sichtverbindung zu Ihrer AD DS-Domäne verfügen. Wenn Ihr Computer oder Ihre VM sich außerhalb des Netzwerks befindet, das von Ihrer AD DS-Domäne verwaltet wird, müssen Sie den VPN aktivieren, um AD DS für die Authentifizierung zu erreichen.

Ersetzen Sie die Platzhalterwerte durch Ihre eigenen Werte, und verwenden Sie dann den folgenden Befehl zum Einbinden der Azure-Dateifreigabe:

```PSH
# Always mount your share using.file.core.windows.net, even if you setup a private endpoint for your share.
$connectTestResult = Test-NetConnection -ComputerName <storage-account-name>.file.core.windows.net -Port 445
if ($connectTestResult.TcpTestSucceeded)
{
  net use <desired-drive letter>: \\<storage-account-name>.file.core.windows.net\<fileshare-name>
} 
else 
{
  Write-Error -Message "Unable to reach the Azure storage account via port 445. Check to make sure your organization or ISP is not blocking port 445, or use Azure P2S VPN, Azure S2S VPN, or Express Route to tunnel SMB traffic over a different port."
}

```

Wenn beim Einbinden von AD DS-Anmeldeinformationen Probleme auftreten, finden Sie Anweisungen unter [Azure Files mit AD-Anmeldeinformationen kann nicht eingebunden werden](storage-troubleshoot-windows-file-connection-problems.md#unable-to-mount-azure-files-with-ad-credentials).

Wenn die Einbindung Ihrer Dateifreigabe erfolgreich war, haben Sie die lokale AD DS-Authentifizierung für Ihre Azure-Dateifreigaben aktiviert und konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

Wenn sich die Identität, die Sie zum Darstellen des Speicherkontos in AD DS erstellt haben, in einer Domäne oder Organisationseinheit befindet, die die Kennwortrotation erzwingt, fahren Sie mit dem nächsten Artikel mit Anweisungen zum Aktualisieren Ihres Kennworts fort:

[Aktualisieren Sie das Kennwort für Ihre Speicherkontoidentität in AD DS.](storage-files-identity-ad-ds-update-password.md)
