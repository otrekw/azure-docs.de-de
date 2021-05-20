---
title: Konfigurieren von ADDS LDAP über TLS für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt, wie ADDS LDAP über TLS für Azure NetApp Files konfiguriert wird, einschließlich der Zertifikatverwaltung der Stammzertifizierungsstelle.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/05/2021
ms.author: b-juche
ms.openlocfilehash: e9a1b9148948319ebf05e2bcc9f5f306b2a21ec0
ms.sourcegitcommit: 89c4843ec85d1baea248e81724781d55bed86417
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108795971"
---
# <a name="configure-adds-ldap-over-tls-for-azure-netapp-files"></a>Konfigurieren von ADDS LDAP über TLS für Azure NetApp Files

Sie können LDAP über TLS verwenden, um die Kommunikation zwischen einem Azure NetApp Files und dem Active Directory LDAP-Server zu schützen.  Sie können LDAP über TLS für NFS-, SMB- und Dualprotokollvolumes von Azure NetApp Files aktivieren.  

## <a name="considerations"></a>Überlegungen

* LDAP über TLS darf nicht aktiviert werden, wenn Sie Azure Active Directory Domain Services (AADDS) verwenden. AADDS verwendet LDAPS (Port 636), um LDAP-Datenverkehr anstelle von LDAP über TLS (Port 389) zu sichern.  

## <a name="register-the-ldap-over-tls-feature"></a>Registrieren des LDAP über TLS-Feature 

Die Option LDAP über TLS-Feature befindet sich derzeit in der Vorschauphase. Wenn Sie diese Funktion zum ersten Mal verwenden, müssen Sie sie vor der Verwendung zunächst registrieren.

1.  Registrieren Sie die Funktion:

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapOverTls
    ```

2. Überprüfen Sie den Status der Funktionsregistrierung: 

    > [!NOTE]
    > Der **RegistrationState** kann bis zu 60 Minuten lang den Wert `Registering` aufweisen, bevor er sich in `Registered` ändert. Warten Sie, bis der Status `Registered` lautet, bevor Sie fortfahren.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapOverTls
    ```
Sie können auch die [Azure CLI-Befehle](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) `az feature register` und `az feature show` verwenden, um das Feature zu registrieren und den Registrierungsstatus anzuzeigen. 

## <a name="generate-and-export-root-ca-certificate"></a>Generieren und Exportieren des Zertifikats der Stammzertifizierungsstelle 

Wenn Sie kein Zertifikat der Stammzertifizierungsstelle haben, müssen Sie ein Zertifikat generieren und für die Verwendung mit LDAP über die TLS-Authentifizierung exportieren. 

1. Führen Sie die unter [Installieren der Zertifizierungsstelle](/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority) angegebenen Schritte zum Installieren und Konfigurieren der AD DS-Zertifizierungsstelle aus. 

2. Führen Sie die unter [Anzeigen von Zertifikaten mit dem MMC-Snap-In](/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) angegebenen Schritte aus, um das MMC-Snap-In und das Tool „Zertifikat-Manager“ zu verwenden.  
    Verwenden Sie das Snap-In „Zertifikat-Manager“, um das Stamm- oder Ausstellerzertifikat für das lokale Gerät zu suchen. Sie sollten die Befehle des Snap-Ins „Zertifikatverwaltung“ über eine der folgenden Einstellungen ausführen:  
    * Windows-basierter Client, der der Domäne hinzugefügt wurde und auf dem das Stammzertifikat installiert ist 
    * Anderer Computer in der Domäne mit dem Stammzertifikat  

3. Exportieren Sie das Zertifikat der Stammzertifizierungsstelle.  
    Zertifikate der Stammzertifizierungsstelle können aus den Verzeichnissen „Persönlich“ oder „Vertrauenswürdige Stammzertifizierungsstellen“ exportiert werden, wie in den folgenden Beispielen gezeigt:   
    ![Screenshot mit persönlichen Zertifikaten](../media/azure-netapp-files/personal-certificates.png)   
    ![Screenshot mit vertrauenswürdigen Stammzertifizierungsstellen](../media/azure-netapp-files/trusted-root-certification-authorities.png)    

    Achten Sie darauf, dass das Zertifikat im Base64-codierten X.509-Format (.CER) exportiert wird: 

    ![Zertifikatexport-Assistent](../media/azure-netapp-files/certificate-export-wizard.png)

## <a name="enable-ldap-over-tls-and-upload-root-ca-certificate"></a>Aktivieren von LDAP über TLS und Hochladen des Zertifikats der Stammzertifizierungsstelle 

1. Wechseln Sie zu dem NetApp-Konto, das für das Volume verwendet wird, und klicken Sie auf **Active Directory-Verbindungen**. Klicken Sie dann auf **Beitreten**, um eine neue AD-Verbindung zu erstellen, oder auf **Bearbeiten**, um eine vorhandene AD-Verbindung zu bearbeiten.  

2. Aktivieren Sie im angezeigten Fenster **Active Directory beitreten** oder **Active Directory bearbeiten** das Kontrollkästchen **LDAP über TLS**, um LDAP über TLS für das Volume zu aktivieren. Klicken Sie dann auf **Zertifikat der Serverstammzertifizierungsstelle** und laden Sie das [generierte Zertifikat der Stammzertifizierungsstelle](#generate-and-export-root-ca-certificate) hoch, das für LDAP über TLS verwendet werden soll.  

    ![Screenshot: LDAP über TLS-Option](../media/azure-netapp-files/ldap-over-tls-option.png)

    Stellen Sie sicher, dass der Name der Zertifizierungsstelle durch DNS aufgelöst werden kann. Dieser Name ist das Feld „Ausgestellt von“ oder „Aussteller“ im Zertifikat:  

    ![Screenshot: Zertifikatinformationen](../media/azure-netapp-files/certificate-information.png)

Wenn Sie ein ungültiges Zertifikat hochgeladen haben und über vorhandene AD-Konfigurationen, SMB-Volumes oder Kerberos-Volumes verfügen, tritt ein Fehler ähnlich dem folgenden auf:

`Error updating Active Directory settings The LDAP client configuration "ldapUserMappingConfig" for Vservers is an invalid configuration.`

Um die Fehlerbedingung zu beheben, laden Sie ein gültiges Zertifikat der Stammzertifizierungsstelle in Ihr NetApp-Konto hoch, wie vom Windows Active Directory LDAP-Server für die LDAP-Authentifizierung erforderlich.

## <a name="next-steps"></a>Nächste Schritte  

* [Erstellen eines NFS-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Erstellen eines SMB-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes-smb.md) 
* [Erstellen eines Volumes mit dualem Protokoll für Azure NetApp Files](create-volumes-dual-protocol.md)

