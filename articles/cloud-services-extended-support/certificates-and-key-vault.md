---
title: Speichern und Verwenden von Zertifikaten in Azure Cloud Services (erweiterter Support)
description: Prozesse zum Speichern und Verwenden von Zertifikaten in Azure Cloud Services (erweiterter Support)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 9e69b4e9279f9147c2ee13d42a42aec0c5a15d96
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743988"
---
# <a name="use-certificates-with-azure-cloud-services-extended-support"></a>Verwenden von Zertifikaten mit Azure Cloud Services (erweiterter Support)

Key Vault wird zum Speichern von Zertifikaten verwendet, die Cloud Services (erweiterter Support) zugeordnet sind. Key Vault-Instanzen können über das [Azure-Portal](https://docs.microsoft.com/azure/key-vault/general/quick-create-portal) und mit [PowerShell](https://docs.microsoft.com/azure/key-vault/general/quick-create-powershell) erstellt werden. Fügen Sie Key Vault die Zertifikate hinzu, und verweisen Sie dann in der Dienstkonfigurationsdatei auf die Zertifikatfingerabdrücke. Außerdem müssen Sie Key Vault für entsprechende Berechtigungen aktivieren, damit Cloud Services (erweiterter Support) als Geheimnisse gespeicherte Zertifikate aus Key Vault abrufen kann.  

## <a name="upload-a-certificate-to-key-vault"></a>Hochladen eines Zertifikats in Key Vault 

1.  Melden Sie sich beim Azure-Portal an, und wechseln Sie zu Ihrer Key Vault-Instanz. Wenn Sie keine Key Vault-Instanz eingerichtet haben, können Sie sich dafür entscheiden, im selben Fenster eine zu erstellen.

2. Wählen Sie **Zugriffsrichtlinien** aus.

    :::image type="content" source="media/certs-and-key-vault-1.png" alt-text="Die Abbildung zeigt die Auswahl von Zugriffsrichtlinien im Key Vault-Blatt.":::

3. Stellen Sie sicher, dass die Zugriffsrichtlinien die folgenden Eigenschaften haben:
    - **Aktivieren des Zugriffs auf Azure Virtual Machines für Bereitstellung**
    - **Aktivieren des Zugriffs auf Azure Resource Manager für Vorlagenbereitstellung** 

    :::image type="content" source="media/certs-and-key-vault-2.png" alt-text="Die Abbildung zeigt das Zugriffsrichtlinienfenster im Azure-Portal.":::
 
4.  Wählen Sie **Zertifikate** aus. 

    :::image type="content" source="media/certs-and-key-vault-3.png" alt-text="Die Abbildung zeigt die Auswahl der Zertifikateoption im Richtlinienfenster des Key Vault-Blatts im Azure-Portal.":::

5. Wählen Sie die Option **Generieren/importieren** aus.

    :::image type="content" source="media/certs-and-key-vault-4.png" alt-text="Die Abbildung zeigt die Auswahl der Option „Generieren/importieren“.":::

4.  Geben Sie die erforderlichen Informationen an, um das Hochladen des Zertifikats abzuschließen. 

    :::image type="content" source="media/certs-and-key-vault-5.png" alt-text="Die Abbildung zeigt das Importfenster im Azure-Portal.":::

5.  Fügen Sie Ihrer Rolle in der Dienstkonfigurationsdatei (CSCFG) die Zertifikatdetails hinzu. Stellen Sie sicher, dass der Fingerabdruck des Zertifikats im Azure-Portal dem Fingerabdruck in der Dienstkonfigurationsdatei (CSCFG) entspricht. 
    
    ```json
    <Certificate name="<your cert name>" thumbprint="<thumbprint in key vault" thumbprintAlgorithm="sha1" /> 
    ```

## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.