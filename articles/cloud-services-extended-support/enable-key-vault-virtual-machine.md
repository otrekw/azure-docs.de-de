---
title: Anwenden der Key Vault-VM-Erweiterung in Azure Cloud Services (erweiterter Support)
description: Aktivieren der Key Vault-VM-Erweiterung für Cloud Services (erweiterter Support)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: shisriva
ms.author: shisriva
ms.reviewer: gachandw
ms.date: 05/12/2021
ms.custom: ''
ms.openlocfilehash: 27a25cbff54acda871530daca455045e5519c936
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110104120"
---
# <a name="apply-the-key-vault-vm-extension-to-azure-cloud-services-extended-support"></a>Anwenden der Key Vault-VM-Erweiterung auf Azure Cloud Services (erweiterter Support)

## <a name="what-is-the-key-vault-vm-extension"></a>Was ist die Key Vault-VM-Erweiterung?
Die Key Vault-VM-Erweiterung ermöglicht die automatische Aktualisierung von Zertifikaten, die in einer Azure Key Vault-Instanz gespeichert sind. Hierbei überwacht die Erweiterung eine Liste mit beobachteten Zertifikaten, die in Schlüsseltresoren gespeichert sind, und ruft bei Erkennung einer Änderung die entsprechenden Zertifikate ab und installiert sie. Weitere Informationen finden Sie unter [Key Vault-VM-Erweiterung für Windows](../virtual-machines/extensions/key-vault-windows.md).

## <a name="whats-new-in-the-key-vault-vm-extension"></a>Welche Neuerungen gibt es in der Key Vault VM-Erweiterung?
Die Key Vault-VM-Erweiterung wird jetzt auf der Azure Cloud Services-Plattform (erweiterter Support) unterstützt, um die End-to-End-Verwaltung von Zertifikaten zu ermöglichen. Die Erweiterung kann nun Zertifikate aus einer konfigurierten Key Vault-Instanz in einem vordefinierten Abrufintervall pullen und für die Verwendung durch den Dienst installieren. 

## <a name="how-can-i-leverage-the-key-vault-vm-extension"></a>Wie kann ich die Key Vault-VM-Erweiterung nutzen?
Im folgenden Tutorial erfahren Sie, wie Sie die Key Vault-VM-Erweiterung in PaaSV1-Diensten installieren, indem Sie zunächst ein Bootstrapzertifikat in Ihrem Tresor erstellen, um ein Token aus AAD abzurufen, das bei der Authentifizierung der Erweiterung beim Tresor hilft. Nachdem der Authentifizierungsprozess eingerichtet und die Erweiterung installiert wurde, werden alle aktuellen Zertifikate in regelmäßigen Abrufintervallen automatisch gepullt. 


## <a name="prerequisites"></a>Voraussetzungen 
Um die Azure Key Vault-VM-Erweiterung verwenden zu können, benötigen Sie einen Azure Active Directory-Mandanten. Weitere Informationen zum Einrichten eines neuen Active Directory-Mandanten finden Sie unter [Einrichten Ihres AAD-Mandanten](../active-directory/develop/quickstart-create-new-tenant.md).

## <a name="enable-the-azure-key-vault-vm-extension"></a>Aktivieren der Azure Key Vault-VM-Erweiterung

1. [Generieren Sie ein Zertifikat](../key-vault/certificates/create-certificate-signing-request.md) in Ihrem Tresor, und laden Sie die CER-Datei für dieses Zertifikat herunter.

2. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **App-Registrierungen**.
    
    :::image type="content" source="media/app-registration-0.jpg" alt-text="Zeigt die Auswahl der App-Registrierung im Portal.":::
    

3. Wählen Sie auf der Seite „App-Registrierungen“ oben links auf der Seite die Option **Neue Registrierung** aus.
    
    :::image type="content" source="media/app-registration-1.png" alt-text="Zeigt die App-Registrierung im Azure-Portal.":::

4. Auf der nächsten Seite können Sie das Formular ausfüllen und die App-Erstellung abschließen.

5. Laden Sie die CER-Datei des Zertifikats in das Azure Active Directory-App-Portal hoch.

    - Optional können Sie auch die [Key Vault Event Grid-Benachrichtigungsfunktion](https://azure.microsoft.com/updates/azure-key-vault-event-grid-integration-is-now-available/) nutzen, um das Zertifikat hochzuladen.  

6. Erteilen Sie der Azure Active Directory-App Berechtigungen zum Auflisten/Abrufen von Geheimnissen in Key Vault:
    - Wenn Sie rollenbasierte Zugriffssteuerung (Vorschau) verwenden, suchen Sie den Namen der AAD-App, die Sie erstellt haben, und weisen Sie sie der Rolle „Key Vault-Geheimnisbenutzer (Vorschau)“ zu.
    - Wenn Sie Tresorzugriffsrichtlinien verwenden, weisen Sie der AAD-App, die Sie erstellt haben, die Berechtigungen **Secret-Get** zu. Weitere Informationen finden Sie unter [Zuweisen von Zugriffsrichtlinien](../key-vault/general/assign-access-policy-portal.md).

7. Installieren Sie die erste Version der im ersten Schritt erstellten Zertifikate und die Key Vault-VM-Erweiterung mithilfe der ARM-Vorlage, wie unten gezeigt:

    ```json
        {
       "osProfile":{
          "secrets":[
             {
                "sourceVault":{
                   "id":"[parameters('sourceVaultValue')]"
                },
                "vaultCertificates":[
                   {
                      "certificateUrl":"[parameters('bootstrpCertificateUrlValue')]"
                   }
                ]
             }
          ]
       }{
          "name":"KVVMExtensionForPaaS",
          "properties":{
             "type":"KeyVaultForPaaS",
             "autoUpgradeMinorVersion":true,
             "typeHandlerVersion":"1.0",
             "publisher":"Microsoft.Azure.KeyVault",
             "settings":{
                "secretsManagementSettings":{
                   "pollingIntervalInS":"3600",
                   "certificateStoreName":"My",
                   "certificateStoreLocation":"LocalMachine",
                   "linkOnRenewal":false,
                   "requireInitialSync":false, 
                   "observedCertificates":"[parameters('keyVaultObservedCertificates']"
                },
                "authenticationSettings":{
                   "clientId":"Your AAD app ID",
                   "clientCertificateSubjectName":"Your boot strap certificate subject name [Do not include the 'CN=' in the subject name]"
                }
             }
          }
       }
    ```
    Möglicherweise müssen Sie den Zertifikatspeicher für das Bootstrapzertifikat in „ServiceDefinition.csdef“ wie folgt angeben:
    
    ```xml
        <Certificates>
                 <Certificate name="bootstrapcert" storeLocation="LocalMachine" storeName="My" />
        </Certificates> 
    ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Optimierung Ihrer Bereitstellung durch [Aktivieren von Überwachung in Cloud Services (erweiterter Support)](enable-alerts.md).