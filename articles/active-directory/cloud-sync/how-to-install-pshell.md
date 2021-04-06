---
title: Installieren des Azure AD Connect-Cloudbereitstellungs-Agents mit PowerShell
description: Erfahren Sie, wie Sie den Azure AD Connect-Cloudbereitstellungs-Agent mithilfe von PowerShell-Cmdlets installieren.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/16/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c20cfb96b5cd6e1d05e332fa7157fe6e0cde8656
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98612653"
---
# <a name="install-the-azure-ad-connect-provisioning-agent-using-powershell-cmdlets"></a>Installieren des Azure AD Connect-Cloudbereitstellungs-Agents mit PowerShell-Cmdlets 
Im folgenden Dokument wird erläutert, wie Sie den Azure AD Connect-Bereitstellungs-Agent mithilfe von PowerShell-Cmdlets installieren.
 

## <a name="prerequisite"></a>Voraussetzung: 


>[!IMPORTANT]
>In den folgenden Installationsanweisungen wird davon ausgegangen, dass alle [Voraussetzungen](how-to-prerequisites.md) erfüllt sind.
>
> Auf dem Windows-Server muss TLS 1.2 aktiviert sein, bevor Sie den Azure AD Connect-Cloudbereitstellungs-Agent mithilfe von PowerShell-Cmdlets installieren können. Zum Aktivieren von TLS 1.2 können Sie die [hier](how-to-prerequisites.md#tls-requirements) aufgeführten Schritte ausführen.

 

## <a name="install-the-azure-ad-connect-provisioning-agent-using-powershell-cmdlets"></a>Installieren des Azure AD Connect-Cloudbereitstellungs-Agents mit PowerShell-Cmdlets 


 1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu **Azure Active Directory**.
 2. Wählen Sie im linken Menü die Option **Azure AD Connect** aus.
 3. Wählen Sie **Bereitstellung verwalten (Vorschau)**  > **Alle Agents überprüfen** aus.
 4. Laden Sie den Azure AD Connect-Bereitstellungs-Agent über das Azure-Portal lokal herunter.  

   ![Herunterladen des lokalen Agents](media/how-to-install/install-9.png)</br>
 5. Für diesen Leitfaden wurde der Agent in den folgenden Ordner heruntergeladen:   „C:\ProvisioningSetup“. 
 6. Installieren des Bereitstellungs-Agents im stillen Modus

   ```
   $installerProcess = Start-Process c:\temp\AADConnectProvisioningAgent.Installer.exe /quiet -NoNewWindow -PassThru 
   $installerProcess.WaitForExit()  
   ```
 7. Importieren des PS-Moduls für den Bereitstellungs-Agent 

   ```
   Import-Module "C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\Microsoft.CloudSync.Powershell.dll" 
   ```
 8. Stellen Sie mithilfe der Anmeldeinformationen des globalen Administrators eine Verbindung mit Azure AD her, um diesen Abschnitt anzupassen und das Kennwort aus einem sicheren Speicher abzurufen. 

   ```
   $globalAdminPassword = ConvertTo-SecureString -String "Global admin password" -AsPlainText -Force 

   $globalAdminCreds = New-Object System.Management.Automation.PSCredential -ArgumentList ("GlobalAdmin@contoso.onmicrosoft.com", $globalAdminPassword) 
   ```

   Connect-AADCloudSyncAzureAD -Credential $globalAdminCreds 

 9. Fügen Sie das gMSA-Konto hinzu, und geben Sie die Anmeldeinformationen des Domänenadministrators an, um das Standard-gMSA-Konto zu erstellen: 
 
   ```
   $domainAdminPassword = ConvertTo-SecureString -String "Domain admin password" -AsPlainText -Force 

   $domainAdminCreds = New-Object System.Management.Automation.PSCredential -ArgumentList ("DomainName\DomainAdminAccountName", $domainAdminPassword) 

   Add-AADCloudSyncGMSA -Credential $domainAdminCreds 
   ```
 10. Oder verwenden Sie das obige Cmdlet, um ein vorab erstelltes gMSA-Konto anzugeben: 

 
   ```
   Add-AADCloudSyncGMSA -CustomGMSAName preCreatedGMSAName$ 
   ```
 11. Domäne hinzufügen 

   ```
   $contosoDomainAdminPassword = ConvertTo-SecureString -String "Domain admin password" -AsPlainText -Force 

   $contosoDomainAdminCreds = New-Object System.Management.Automation.PSCredential -ArgumentList ("DomainName\DomainAdminAccountName", $contosoDomainAdminPassword) 

   Add-AADCloudSyncADDomain -DomainName contoso.com -Credential $contosoDomainAdminCreds 
   ```
 12. Oder verwenden Sie das obige Cmdlet, um die bevorzugten Domänencontroller zu konfigurieren: 

   ```
   $preferredDCs = @("PreferredDC1", "PreferredDC2", "PreferredDC3") 

   Add-AADCloudSyncADDomain -DomainName contoso.com -Credential $contosoDomainAdminCreds -PreferredDomainControllers $preferredDCs 
   ```
 13. Wiederholen Sie den vorherigen Schritt, um weitere Domänen hinzuzufügen. Geben Sie dabei die Konto- und Domänennamen der jeweiligen Domänen an. 
 14. Starten Sie den Dienst neu. 
   ```
   Restart-Service -Name AADConnectProvisioningAgent  
   ```
 15.  Wechseln Sie zum Azure-Portal, um die Konfiguration für die Cloudsynchronisierung zu erstellen.

## <a name="provisioning-agent-gmsa-powershell-cmdlets"></a>gMSA-PowerShell-Cmdlets für den Bereitstellungs-Agent
Nachdem Sie den Agent installiert haben, können Sie präzisere Berechtigungen auf das gMSA anwenden.  Weitere Informationen und Schrittanleitungen zum Konfigurieren der Berechtigungen finden Sie unter [gMSA-PowerShell-Cmdlets für den Agent für die Azure AD Connect-Cloudbereitstellung](how-to-gmsa-cmdlets.md).

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [gMSA-PowerShell-Cmdlets für den Agent für die Azure AD Connect-Cloudbereitstellung](how-to-gmsa-cmdlets.md)
- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)